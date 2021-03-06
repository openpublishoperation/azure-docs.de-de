---
title: Abrufen von Konformitätsdaten in Azure Policy
description: Azure Policy-Auswertungen und -Effekte bestimmen die Konformität. Erfahren Sie, wie Sie Konformitätsinformationen abrufen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 3fa185e741f1b14bf3f2e7413945b70b1ea1baaa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970854"
---
# <a name="getting-compliance-data"></a>Abrufen von Konformitätsdaten

Sie genießen mit Azure Policy den Vorteil, Einblicke in Steuerelemente für Ressourcen in einem Abonnement oder einer [Verwaltungsgruppe](../../management-groups/overview.md) von Abonnements zu erhalten. Dieses Steuerelement kann unterschiedlich verwendet werden, z.B. zum Verhindern, dass Ressourcen am falschen Speicherort erstellt werden, zum Erzwingen häufiger und konsistenter Tagnutzung oder zur Überwachung vorhandener Ressourcen für geeignete Konfigurationen und Einstellungen. In jedem Fall werden Daten aber von Azure Policy generiert, damit Sie den Konformitätsstatus Ihrer Umgebung kennen.

Es gibt mehrere Möglichkeiten, auf Konformitätsinformationen, die von Ihrer Richtlinie sowie Initiativenzuweisungen erstellt wurden, zuzugreifen:

- Über das [Azure-Portal](#portal)
- Über [Befehlszeilen](#command-line)-Skripting

Bevor wir uns die Methoden zur Berichterstellung zur Konformität ansehen, beschäftigen wir uns damit, wann Konformitätsinformationen aktualisiert werden und mit den Ereignissen, die einen Auswertungszyklus auslösen sowie mit der Häufigkeit.

> [!WARNING]
> Wenn der Konformitätszustand als **Nicht registriert** gemeldet wird, sollten Sie überprüfen, ob der **Microsoft.PolicyInsights**-Ressourcenanbieter registriert ist und der Benutzer über die entsprechenden Berechtigungen für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verfügt. Dies ist [hier](../overview.md#rbac-permissions-in-azure-policy) beschrieben.

## <a name="evaluation-triggers"></a>Auswertungsauslöser

Die Ergebnisse eines abgeschlossenen Auswertungszyklus werden im `Microsoft.PolicyInsights`-Ressourcenanbieter über die Vorgänge `PolicyStates` und `PolicyEvents` dargestellt. Weitere Informationen zu den Optionen und Funktionen der Policy Insights-REST-APIs finden Sie unter [Policy Insights](/rest/api/policy-insights/).

Auswertungen zugewiesener Richtlinien und Initiativen geschehen im Zuge unterschiedlicher Ereignisse:

- Eine Richtlinie oder Initiative wird einem Bereich neu zugewiesen. Wenn dies geschieht, dauert es etwa 30 Minuten, bis die Zuweisung auf den definierten Bereich angewendet wird. Sobald sie angewendet wird, startet der Auswertungszyklus für Ressourcen innerhalb dieses Bereich für die neu zugewiesene Richtlinie oder Initiative. Je nach den Effekten, die von der Richtlinie oder Initiative verwendet werden, werden Ressourcen als „konform“ oder „nicht konform“ markiert. Eine große Richtlinie oder Initiative, die für einen großen Ressourcenbereich angewendet wird, kann einige Zeit in Anspruch nehmen. Es kann keine exakte Dauer angegeben werden, wie lange der Auswertungszyklus ausgeführt wird. Sobald er abgeschlossen ist, sind aktualisierte Konformitätsergebnisse im Portal und den SDKs verfügbar.
- Eine Richtlinie oder Initiative, die bereits einem Bereich zugewiesen ist, wird aktualisiert. Der Auswertungszyklus und die zeitliche Steuerung für dieses Szenario entspricht denjenigen von Neuzuweisungen zu einem Bereich.
- Eine Ressource wird in einem Bereich mit einer Zuweisung über den Ressourcen-Manager, REST, Azure CLI oder Azure PowerShell bereitgestellt. In diesem Szenario werden nach etwa 15 Minuten die Informationen über das betroffene Ereignis (Anfügen, Überwachen, Verweigern, Bereitstellen) und den Konformitätsstatus für die jeweilige Ressource im Portal und den SDKs verfügbar. Dieses Ereignis löst keine Auswertung anderer Ressourcen aus.
- Standard-Konformitätsauswertungszyklus. Zuweisungen werden alle 24 Stunden automatisch neu ausgewertet. Eine große Richtlinie oder Initiative, die für einen großen Ressourcenbereich angewendet wird, kann einige Zeit in Anspruch nehmen. Es kann keine exakte Dauer angegeben werden, wie lange der Auswertungszyklus dauert. Sobald er abgeschlossen ist, sind aktualisierte Konformitätsergebnisse im Portal und den SDKs verfügbar.

## <a name="how-compliance-works"></a>Funktionsweise der Konformität

In einer Zuweisung ist eine Ressource **nicht konform**, wenn dafür die Richtlinien- oder Initiativenregeln nicht eingehalten werden. Die folgende Tabelle gibt Aufschluss über das Zusammenspiel zwischen den verschiedenen Richtlinienauswirkungen, der Bedingungsauswertung und dem resultierenden Konformitätszustand:

| Ressourcenzustand | Wirkung | Richtlinienauswertung | Konformitätszustand |
| --- | --- | --- | --- |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Nicht konform |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Konform |
| Neu | Audit, AuditIfNotExist\* | True | Nicht konform |
| Neu | Audit, AuditIfNotExist\* | False | Konform |

\* Für die Auswirkungen „Append“, „DeployIfNotExist“ und „AuditIfNotExist“ muss die IF-Anweisung auf TRUE festgelegt sein.
Für die Auswirkungen muss die Existenzbedingung außerdem auf FALSE festgelegt sein, damit sie nicht konform sind. Bei TRUE löst die IF-Bedingung die Auswertung der Existenzbedingung für die zugehörigen Ressourcen aus.

Angenommen, Sie verfügen über die Ressourcengruppe ContosoRG mit einigen Speicherkonten (rot hervorgehoben), die in öffentlichen Netzwerken verfügbar gemacht werden.

![In öffentlichen Netzwerken verfügbar gemachte Speicherkonten](../media/getting-compliance-data/resource-group01.png)

In diesem Beispiel ist Vorsicht aufgrund von Sicherheitsrisiken geboten. Nachdem Sie nun eine Richtlinienzuweisung erstellt haben, wird sie für alle Speicherkonten in der Ressourcengruppe ContosoRG ausgewertet. Sie überprüft die drei nicht konformen Speicherkonten und ändert den Status daher jeweils in **nicht konform**.

![Überwachte nicht konforme Speicherkonten](../media/getting-compliance-data/resource-group03.png)

Zusätzlich zu **Konform** und **Nicht konform** können für Richtlinien und Ressourcen noch drei andere Zustände gelten:

- **Konflikt**: Mindestens zwei Richtlinien mit in Konflikt stehenden Regeln sind vorhanden (z.B. zwei Richtlinien, die das gleiche Tag mit unterschiedlichen Werten anfügen).
- **Nicht gestartet**: Der Auswertungszyklus wurde für die Richtlinie oder Ressource noch nicht gestartet.
- **Nicht registriert**: Der Azure Policy-Ressourcenanbieter wurde nicht registriert, oder das angemeldete Konto hat keine Berechtigung zum Lesen der Konformitätsdaten.

Policy verwendet die Felder **type** und **name** in der Definition der Richtlinienregel, um zu ermitteln, ob eine Ressource eine Übereinstimmung ergibt. Wenn ja, wird sie als anwendbar angesehen und hat entweder den Status **Konform** oder **Nicht konform**. Wenn entweder **type** oder **name** die einzige Eigenschaft in der Definition der Richtlinienregel ist, werden alle Ressourcen als anwendbar angesehen und ausgewertet.

Der Prozentsatz der Konformität wird ermittelt, indem die **konformen** Ressourcen durch _Ressourcen gesamt_ geteilt werden.
_Ressourcen gesamt_ ist als Summe der Ressourcen mit dem Zustand **Konform**, **Nicht konform** und **Konflikt** definiert. Die Gesamtzahl für die Konformität ist die Summe der einzelnen Ressourcen, die **konform** sind, geteilt durch die Summe aller einzelnen Ressourcen. Die Abbildung unten enthält 20 einzelne Ressourcen, die zutreffen, und nur eine davon ist **nicht konform**. Daher lautet der Gesamtwert für die Ressourcenkonformität 19/20 oder 95%.

![Beispiel für einfache Konformität](../media/getting-compliance-data/simple-compliance.png)

## <a name="portal"></a>Portal

Im Azure-Portal ist eine grafische Benutzeroberfläche zum Anzeigen und Verstehen des Konformitätsstatus Ihrer Umgebung dargestellt. Auf der Seite **Richtlinie** stellt die Option **Übersicht** Details für verfügbare Bereiche zur Konformität für Richtlinien und Initiativen bereit. Zusätzlich zum Konformitätsstatus und der Anzahl pro Zuweisung ist ein Diagramm enthalten, das die Konformität der letzten sieben Tage anzeigt. Die Seite **Konformität** enthält im Grunde genommen die gleichen Informationen (mit Ausnahme des Diagramms), stellt jedoch zusätzliche Optionen zum Filtern und Sortieren bereit.

![Seite zur Richtlinienkonformität](../media/getting-compliance-data/compliance-page.png)

Da eine Richtlinie oder Initiative unterschiedlichen Bereichen zugewiesen werden kann, beachten Sie in der Tabelle den Bereich für jede Zuweisung und den Typ der Definition, die diesem Bereich zugeordnet wurde. Die Anzahl der nicht konformen Ressourcen und Richtlinien für jede Zuweisung wird ebenfalls bereitgestellt. Wenn Sie auf eine Richtlinie oder Initiative in der Tabelle klicken, erhalten Sie weitere Informationen zur Konformität für eine bestimmte Zuweisung.

![Details zur Richtlinienkonformität](../media/getting-compliance-data/compliance-details.png)

Die Liste mit den Ressourcen auf der Registerkarte **Ressourcenkonformität** (standardmäßig **Nicht konform**, kann aber gefiltert werden) spiegelt den Auswertungsstatus vorhandener Ressourcen für die aktuelle Zuweisung wider. Ereignisse (Anfügen, Überwachen, Verweigern, Bereitstellen), die von der Anforderung ausgelöst werden, um eine Ressource zu erstellen, werden auf der Registerkarte **Ereignisse** angezeigt.

![Ereignisse der Richtlinienkonformität](../media/getting-compliance-data/compliance-events.png)

Klicken Sie mit der rechten Maustaste auf die Zeile des Ereignisses, über das Sie gern mehr Details erhalten möchten, und wählen Sie **Aktivitätsprotokolle anzeigen** aus. Die Seite des Aktivitätsprotokolls wird geöffnet und wird durch die Suche gefiltert. Die Details für die Zuweisung und Ereignisse werden angezeigt. Das Aktivitätsprotokoll stellt zusätzlichen Kontext sowie Informationen über diese Ereignisse bereit.

![Aktivitätsprotokoll der Richtlinienkonformität](../media/getting-compliance-data/compliance-activitylog.png)

## <a name="command-line"></a>Befehlszeile

Die gleichen Informationen, die im Portal verfügbar sind, können auch direkt über die REST-API (u.a. mit [ARMClient](https://github.com/projectkudu/ARMClient)) oder Azure PowerShell mit der REST-API abgerufen werden. Ausführliche Informationen zur REST-API finden Sie in der Referenz zu [Policy Insights](/rest/api/policy-insights/). Die Referenzseiten zur REST-API verfügen über eine grüne „Ausprobieren“-Schaltfläche, mit der Sie jeden Vorgang direkt im Browser testen können.

Um die folgenden Beispiele in Azure PowerShell zu verwenden, erstellen Sie ein Authentifizierungstoken mit folgendem Beispielcode. Ersetzen Sie dann den $restUri mit der gewünschten Zeichenfolge in den Beispielen, um ein JSON-Objekt abzurufen, das dann analysiert werden kann.

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell

$azContext = Get-AzureRmContext
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
$token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
$authHeader = @{
    'Content-Type'='application/json'
    'Authorization'='Bearer ' + $token.AccessToken
}

# Define the REST API to communicate with
# Use double quotes for $restUri as some endpoints take strings passed in single quotes
$restUri = "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04"

# Invoke the REST API
$response = Invoke-RestMethod -Uri $restUri -Method POST -Headers $authHeader

# View the response object (as JSON)
$response
```

### <a name="summarize-results"></a>Zusammenfassen der Ergebnisse

Mithilfe der REST-API kann die Zusammenfassung von der Verwaltungsgruppe, dem Abonnement, der Ressourcengruppe, der Ressource, der Initiative, der Richtlinie, der Abonnementstufenzuweisung oder der Ressourcengruppenstufenzuweisung durchgeführt werden. Hier sehen Sie ein Beispiel der Zusammenfassung auf Abonnementebene mithilfe der Option [Summarize For Subscription (Für das Abonnement zusammenfassen)](/rest/api/policy-insights/policystates/summarizeforsubscription) von Policy Insight:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

Die Ausgabe fasst das Abonnement zusammen. In der Beispielausgabe unten befindet sich die zusammengefasste Konformität unter **value.results.nonCompliantResources** und **value.results.nonCompliantPolicies**. Diese Anforderung stellt weitere Details bereit, einschließlich jeder Zuweisung, aus der die nicht konformen Zahlen und die Definitionsinformationen für jede Zuweisung bestehen. Jedes Richtlinienobjekt in der Hierarchie bietet einen **queryResultsUri**, der zum Abrufen zusätzlicher Details auf dieser Ebene verwendet werden kann.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Abfragen von Ressourcen

Mithilfe des oben genannten Beispiels konnte **value.policyAssignments.policyDefinitions.results.queryResultsUri** einen Beispiel-URI bereitstellen, mit dem alle nicht konformen Ressourcen für eine bestimmte Richtliniendefinition abgerufen werden können. Sehen Sie sich den Wert **$filter** an. Sie werden feststellen, dass „IsCompliant“ gleich (eq) FALSE ist und „PolicyAssignmentId“ für die Richtliniendefinition sowie die PolicyDefinitionId selbst angegeben ist.
Der Grund für das Einschließen der PolicyAssignmentId in den Filter ist der, dass die PolicyDefinitionId in mehreren Zuweisungen von Richtlinien oder Initiativen mit mehreren Bereichen vorhanden sein kann. Durch Angeben der PolicyAssignmentId und der PolicyDefinitionId können wir die Ergebnisse eingrenzen, die wir suchen. Zuvor haben wir **latest** (letztes) für PolicyStates verwendet (der einzig zulässige Wert für **policyStatesSummaryResource** auf dem Operator „Summarize For Subscription“), was automatisch ein Zeitfenster von **from** (von) und **to** (bis) der letzten 24 Stunden festlegt.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Die Beispielantwort unten wurde gekürzt, damit nur eine nicht konforme Ressource angezeigt wird, um die Komplexität gering zu halten. Beachten Sie, dass @odata.count tatsächlich 15 ist und mit der Anzahl nicht konformer Ressourcen aus dem Beispiel oben übereinstimmt. Die ausführliche Antwort bietet mehrere Teile von Ressourcendaten, die Richtlinie (oder Initiative) sowie die Zuweisung. Beachten Sie, dass Sie auch anzeigen können, welche Zuweisungsparameter an die Richtliniendefinition übergeben wurden.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 15,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "timestamp": "2018-05-19T04:41:09Z",
        "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Compute/virtualMachines/linux",
        "policyAssignmentId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Authorization/policyAssignments/37ce239ae4304622914f0c77",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "effectiveParameters": "",
        "isCompliant": false,
        "subscriptionId": "{subscriptionId}",
        "resourceType": "/Microsoft.Compute/virtualMachines",
        "resourceLocation": "westus2",
        "resourceGroup": "RG-Tags",
        "resourceTags": "tbd",
        "policyAssignmentName": "37ce239ae4304622914f0c77",
        "policyAssignmentOwner": "tbd",
        "policyAssignmentParameters": "{\"tagName\":{\"value\":\"costCenter\"},\"tagValue\":{\"value\":\"Contoso-Test\"}}",
        "policyAssignmentScope": "/subscriptions/{subscriptionId}/resourceGroups/RG-Tags",
        "policyDefinitionName": "1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "policyDefinitionAction": "deny",
        "policyDefinitionCategory": "tbd",
        "policySetDefinitionId": "",
        "policySetDefinitionName": "",
        "policySetDefinitionOwner": "",
        "policySetDefinitionCategory": "",
        "policySetDefinitionParameters": "",
        "managementGroupIds": "",
        "policyDefinitionReferenceId": ""
    }]
}
```

### <a name="view-events"></a>Ereignisse anzeigen

Wenn eine Ressource erstellt oder aktualisiert wird, wird ein Ergebnis der Richtlinienauswertung generiert. Diese Ergebnisse werden als _Richtlinienereignisse_ bezeichnet. Verwenden Sie den folgenden URI, um die neuesten Richtlinienereignisse anzuzeigen, die dem Abonnement zugewiesen sind.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2018-04-04
```

Ihre Ergebnisse sollten in etwa wie im folgenden Beispiel aussehen:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 16
    }]
}
```

Weitere Informationen zum Abfragen von Richtlinienereignissen finden Sie im Referenzartikel zu [Richtlinienereignissen](/rest/api/policy-insights/policyevents).

### <a name="azure-powershell"></a>Azure PowerShell

Das Azure PowerShell-Modul für Policy ist im PowerShell-Katalog unter [AzureRM.PolicyInsights](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights) verfügbar. Mit PowerShellGet können Sie das Modul mit `Install-Module -Name AzureRM.PolicyInsights` installieren (stellen Sie sicher, dass Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-azurerm-ps) installiert haben):

```azurepowershell-interactive
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

Das Modul verfügt über drei Cmdlets:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Beispiel: Abrufen der Zustandszusammenfassung für die oberste zugewiesene Richtlinie mit der höchsten Anzahl nicht konformer Ressourcen

```azurepowershell-interactive
PS> Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Beispiel: Abrufen des Zustandsdatensatzes für die zuletzt ausgewertete Ressource (Standardwert ist nach Zeitstempel in absteigender Reihenfolge)

```azurepowershell-interactive
PS> Get-AzureRmPolicyState -Top 1

Timestamp                  : 5/22/2018 3:47:34 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/networkInterfaces/linux316
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/networkInterfaces
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Beispiel: Abrufen der Details für alle nicht konformen Ressourcen virtueller Netzwerke

```azurepowershell-interactive
PS> Get-AzureRmPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

Timestamp                  : 5/22/2018 4:02:20 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Beispiel: Abrufen von Ereignissen, die auf nicht konforme Ressourcen virtueller Netzwerke bezogen sind, die nach einem bestimmten Datum aufgetreten sind

```azurepowershell-interactive
PS> Get-AzureRmPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

Timestamp                  : 5/19/2018 5:18:53 AM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : eastus
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
TenantId                   : {tenantId}
PrincipalOid               : {principalOid}
```

Das **PrincipalOid**-Feld kann verwendet werden, um einen bestimmten Benutzer mit dem Azure PowerShell-Cmdlet `Get-AzureRmADUser` abzurufen. Ersetzen Sie **{principalOid}** mit der Antwort, die Sie aus dem vorherigen Beispiel erhalten haben.

```azurepowershell-interactive
PS> (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Wenn Sie über einen [Log Analytics](../../../log-analytics/log-analytics-overview.md)-Arbeitsbereich verfügen, bei dem die Lösung `AzureActivity` mit Ihrem Abonnement verknüpft ist, können Sie auch nicht konforme Ergebnisse aus dem Auswertungszyklus mithilfe einfacher Kusto-Abfragen und über die `AzureActivity`-Tabelle anzeigen. Dadurch dass Nichtkonformitätsdetails in Log Analytics vorhanden sind, bedeutet dies, dass Warnungen konfiguriert werden können, die nach Nichtkonformität auf einer bestimmten Ressource, Ressourcengruppe oder auch nach einem Schwellenwert nicht konformer Elemente suchen, z.B. nach mehr als 10 Elementen in den letzten 24 Stunden.

![Richtlinienkonformität mithilfe von Log Analytics](../media/getting-compliance-data/compliance-loganalytics.png)

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](../samples/index.md) finden Sie Beispiele.
- Befassen Sie sich mit der [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Entdecken Sie, wie Sie [nicht konforme Ressourcen](remediate-resources.md) korrigieren können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).