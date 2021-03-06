---
title: Verwenden von API-Versionsprofilen mit .NET SDK in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie mehr zur Verwendung von API-Versionsprofilen mit .NET in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 35329468ee01d5b70d654c1eb4a908db9d3fcb5d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184401"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>Verwenden von API-Versionsprofilen mit .NET in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Das .NET SDK für Azure Stack Resource Manager umfasst Tools zum Erstellen und Verwalten Ihrer Infrastruktur. Zu den Ressourcenanbietern im SDK zählen Compute, Netzwerk, Speicher, App-Dienste und [KeyVault](../../key-vault/key-vault-whatis.md). Das .NET SDK umfasst 14 NuGet-Pakete, welche die Profilinformationen einbinden und jedes Mal in Ihre Projektmappe heruntergeladen werden müssen. Sie können jedoch speziell den Ressourcenanbieter herunterladen, den Sie für das Profil „2018-03-01-hybrid“ oder „2017-03-09-profile“ verwenden werden, um den Arbeitsspeicher für Ihre Anwendung zu optimieren. Jedes Paket besteht aus einem Ressourcenanbieter, der entsprechenden API-Version und dem API-Profil, zu dem es gehört. API-Profile im .NET SDK ermöglichen die Entwicklung einer Hybrid Cloud, indem sie das Wechseln zwischen globalen Azure-Ressourcen und Ressourcen in Azure Stack leicht machen.

## <a name="net-and-api-version-profiles"></a>.NET und API-Versionsprofile

Ein API-Profil ist eine Kombination aus Ressourcenanbietern und API-Versionen. Sie können ein API-Profil verwenden, um die aktuelle, stabilste Version der einzelnen Ressourcentypen in einem Ressourcenanbieterpaket abzurufen.

-   Verwenden Sie das **aktuelle** Profil von Paketen, um die aktuellen Versionen aller Dienste zu nutzen. Dieses Profil ist Teil des NuGet-Pakets **Microsoft.Azure.Management**.

-   Zur Nutzung der mit Azure Stack kompatiblen Dienste verwenden Sie das Paket **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg** oder **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**.

    -   Es gibt zwei Pakete für jeden Ressourcenanbieter und für jedes Profil.

    -   Stellen Sie sicher, dass der **ResourceProvider**-Teil des NuGet-Pakets oben in den richtigen Anbieter geändert wird.

-   Verwenden Sie das **aktuelle** Profil des bestimmten NuGet-Pakets, um die aktuelle API-Version eines Diensts zu nutzen. Wenn Sie beispielsweise nur die **aktuelle API-Version** des Computediensts nutzen möchten, verwenden Sie das **aktuelle** Profil des **Compute**-Pakets. Das **aktuelle** Profil ist Teil des NuGet-Pakets **Microsoft.Azure.Management**.

-   Zum Verwenden von bestimmten API-Versionen für einen Ressourcentyp in einen bestimmten Ressourcenanbieter verwenden Sie die im Paket definierten API-Versionen.

Hinweis: Sie können alle Optionen in derselben Anwendung kombinieren.

## <a name="install-the-azure-net-sdk"></a>Installieren des Azure .NET SDK

1.  Installieren Sie Git. Anweisungen hierzu finden Sie unter [Erste Schritte – Installieren von Git][] (Erste Schritte: Installieren von Git).

2.  Informationen zum Installieren der richtigen NuGet-Pakete finden Sie unter [Suchen und Installieren eines Pakets][].

3.  Welche Pakete installiert werden müssen, hängt von der Profilversion ab, die Sie verwenden möchten. Die Paketnamen für die Profilversionen sind:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**

4.  Zum Installieren der richtigen NuGet-Pakete für Visual Studio Code, rufen Sie den folgenden Link zum Herunterladen der [NuGet-Paket-Manager-Anweisungen][] auf.

5.  Erstellen Sie ein Abonnement, wenn keins verfügbar ist, und speichern Sie die Abonnement-ID zur späteren Verwendung. Eine Anleitung zum Erstellen eines Abonnements finden Sie unter [Erstellen von Abonnements für Angebote in Azure Stack][].

6.  Erstellen Sie einen Dienstprinzipals ein, und speichern Sie die Client-ID und den geheimen Clientschlüssel. Eine Anleitung zum Erstellen eines Dienstprinzipals für Azure Stack finden Sie unter [Bereitstellen des Anwendungszugriffs auf Azure Stack][]. Hinweis: Die Client-ID wird beim Erstellen eines Dienstprinzipals auch als „Anwendungs-ID“ bezeichnet.

7.  Stellen Sie sicher, dass Ihr Dienstprinzipal über die Rolle „Mitwirkender“ bzw. „Besitzer“ für Ihr Abonnement verfügt. Eine Anleitung zum Zuweisen einer Rolle zum Dienstprinzipal finden Sie unter [Bereitstellen des Anwendungszugriffs auf Azure Stack][].

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung des .NET Azure SDK mit Azure Stack müssen Sie die folgenden Werte angeben und dann Werte mit Umgebungsvariablen festlegen. Befolgen Sie die Anweisungen in der Tabelle für Ihr Betriebssystem, um die Umgebungsvariablen festzulegen.

| Wert                     | Umgebungsvariablen   | BESCHREIBUNG                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Mandanten-ID                 | AZURE_TENANT_ID       | Der Wert Ihrer [*Mandanten-ID*][] für Azure Stack.                                                                          |
| Client-ID                 | AZURE_CLIENT_ID       | Die Anwendungs-ID des Dienstprinzipals, die beim Erstellen des Dienstprinzipals im vorherigen Abschnitt dieses Artikels gespeichert wurde. |
| Abonnement-ID           | AZURE_SUBSCRIPTION_ID | Mit der [*Abonnement-ID*][] greifen Sie in Azure Stack auf Angebote zu.                                                      |
| Geheimer Clientschlüssel             | AZURE_CLIENT_SECRET   | Das Geheimnis der Dienstprinzipalanwendung, die bei der Erstellung des Dienstprinzipals gespeichert wurde.                                      |
| Resource Manager-Endpunkt | ARM_ENDPOINT           | Siehe [*Azure Stack-Resource Manager-Endpunkt*][].                                                                    |

Folgen Sie den Anweisungen [hier](../azure-stack-csp-ref-operations.md), um die Mandanten-ID für Ihre Azure Stack-Instanz zu suchen. Zum Festlegen Ihrer Umgebungsvariablen führen Sie folgende Schritte aus:

### <a name="microsoft-windows"></a>Microsoft Windows

Verwenden Sie das folgende Format, um die Umgebungsvariablen in der Windows-Eingabeaufforderung zu verwenden:

```shell
Set Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="macos-linux-and-unix-based-systems"></a>MacOS-, Linux- und Unix-basierte Systeme

In Unix-basierten Systemen können Sie den folgenden Befehl verwenden:

```shell
Export Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack-Resource Manager-Endpunkt

Microsoft Azure Resource Manager ist ein Verwaltungsframework, mit dem Administratoren Azure-Ressourcen bereitstellen, verwalten und überwachen können. Azure Resource Manager kann diese Aufgaben als Gruppe – anstatt einzeln – in einem gemeinsamen Vorgang verarbeiten.

Sie können die Metadateninformationen vom Resource Manager-Endpunkt abrufen. Der Endpunkt gibt eine JSON-Datei zurück, die die erforderlichen Informationen zum Ausführen Ihres Codes enthält.

Beachten Sie die folgenden Überlegungen:

- Der **ResourceManagerUrl**-Wert im Azure Stack Development Kit (ASDK) lautet https://management.local.azurestack.external/.

- Der **ResourceManagerUrl**-Wert in integrierten Systemen lautet: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` Zum Abrufen der erforderlichen Metadaten: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

JSON-Beispieldatei:

```json
{ 
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Vorhandene API-Profile

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg**: Aktuelles, für Azure Stack erstelltes Profil. Verwenden Sie dieses Profil für Dienste für die höchste Kompatibilität mit Azure Stack, sofern Sie bei Stempel 1808 oder weiter sind.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**: Wenn Sie bei einem niedrigeren Stempel als dem 1808-Build sind, verwenden Sie dieses Profil.

3.  **Aktuell**: Profil, das aus den aktuellen Versionen aller Dienste besteht. Verwenden Sie die neuesten Versionen aller Dienste. Dieses Profil ist Teil des NuGet-Pakets **Microsoft.Azure.Management**.

Weitere Informationen zu Azure Stack und API-Profilen finden Sie unter [Zusammenfassung zu API-Profilen][].

## <a name="azure-net-sdk-api-profile-usage"></a>Verwendung des API-Profils aus dem Azure .NET SDK

Zum Instanziieren des Profilclients sollte der folgende Code verwendet werden. Dieser Parameter ist nur für Azure Stack oder andere private Clouds erforderlich. Für Azure sind diese Einstellungen standardmäßig global vorhanden.

Zum Authentifizieren des Dienstprinzipals in Azure Stack ist der folgende Code erforderlich. Erstellt anhand der Mandanten-ID und der Authentifizierungsbasis ein für Azure Stack spezifisches Token.

```csharp
public class CustomLoginCredentials : ServiceClientCredentials
{
    private string clientId;
    private string clientSecret;
    private string resourceId;
    private string tenantId;

    private const string authenticationBase = "https://login.windows.net/{0}";

    public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
    {
        clientId = servicePrincipalId;
        clientSecret = servicePrincipalSecret;
        resourceId = azureEnvironmentResourceId;
        tenantId = azureEnvironmentTenandId;
    }
```

Dies ermöglicht Ihnen die Verwendung der NuGet-Pakete des API-Profils zum erfolgreichen Bereitstellen Ihrer Anwendung in Azure Stack.

## <a name="define-azure-stack-environment-setting-functions"></a>Definieren von Einstellungsfunktionen für die Azure Stack-Umgebung

Verwenden Sie den folgenden Code, um den Dienstprinzipal in der Azure Stack-Umgebung zu authentifizieren:

```csharp
private string AuthenticationToken { get; set; }
public override void InitializeServiceClient<T>(ServiceClient<T> client)
{
    var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
    var credential = new ClientCredential(clientId, clientSecret);
    var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
    clientCredential: credential).Result;
    if (result == null)
    {
        throw new InvalidOperationException("Failed to obtain the JWT token");
    }
    AuthenticationToken = result.AccessToken;
}
```

Dadurch wird der Client zum Initialisieren des Diensts für die Authentifizierung bei Azure Stack außer Kraft gesetzt.

## <a name="samples-using-api-profiles"></a>Beispiele für die Verwendung von API-Profilen

Sie können die folgenden Beispiele aus GitHub-Repositorys als Referenz zum Erstellen von Lösungen mit .NET- und Azure Stack-API-Profilen verwenden.

-   [Testprojekt für virtuelle Computer, vNet, Ressourcengruppen und -konten][]
-   Verwalten von virtuellen Computern mit .NET

### <a name="sample-unit-test-project"></a>Beispiel für Komponententestprojekt 

1.  Klonen Sie das Repository mit dem folgenden Befehl:

    `git clone <https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject>`

2.  Erstellen Sie einen Azure-Dienstprinzipal, und weisen Sie eine Rolle zu, um auf das Abonnement zuzugreifen. Eine Anleitung zur Erstellung eines Dienstprinzipals finden Sie unter [Verwenden von Azure PowerShell zum Erstellen eines Dienstprinzipals mit einem Zertifikat][].

3.  Rufen Sie die folgenden erforderlichen Werte ab:

    1.  Mandanten-ID
    2.  Client-ID
    3.  Geheimer Clientschlüssel
    4.  Abonnement-ID
    5.  Resource Manager-Endpunkt

4.  Legen Sie die folgenden Umgebungsvariablen fest, und verwenden Sie dabei die Informationen, die Sie aus dem mithilfe der Eingabeaufforderung erstellten Dienstprinzipal abgerufen haben:

    1.  export AZURE_TENANT_ID={Ihre Mandanten-ID}
    2.  export AZURE_CLIENT_ID={Ihre Client-ID}
    3.  export AZURE_CLIENT_SECRET={Ihr geheimer Clientschlüssel}
    4.  export AZURE_SUBSCRIPTION_ID={Ihre Abonnement-ID}
    5.  export ARM_ENDPOINT={Ihre Azure Stack-Resource Manager-URL}

   Verwenden Sie unter Windows **set** anstelle von **export**.

5.  Stellen Sie sicher, dass die Standortvariable auf Ihren Azure Stack-Standort festgelegt ist. Beispielsweise LOCAL = „lokal“.

6.  Legen Sie die benutzerdefinierten Anmeldeinformationen fest, mit denen Sie sich bei Azure Stack authentifizieren können. Hinweis: Dieser Teil des Codes in diesem Beispiel im Ordner „Autorisierung“ enthalten.

   ```csharp
   public class CustomLoginCredentials : ServiceClientCredentials
   {
       private string clientId;
       private string clientSecret;
       private string resourceId;
       private string tenantId;
       private const string authenticationBase = "https://login.windows.net/{0}";
       public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
       {
           clientId = servicePrincipalId;
           clientSecret = servicePrincipalSecret;
           resourceId = azureEnvironmentResourceId;
           tenantId = azureEnvironmentTenandId;
       }
   private string AuthenticationToken { get; set; }
   ```

7.  Fügen Sie den folgenden Code hinzu, wenn Sie Azure Stack zum Außerkraftsetzen der Clients zum Initialisieren des Diensts für die Authentifizierung bei Azure Stack verwenden. Hinweis: Ein Teil des Codes ist bereits in diesem Beispiel im Ordner „Autorisierung“ enthalten.

   ```csharp
   public override void InitializeServiceClient<T>(ServiceClient<T> client)
   {
      var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
      var credential = new ClientCredential(clientId, clientSecret);
      var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
                clientCredential: credential).Result;
      if (result == null)
      {
          throw new InvalidOperationException("Failed to obtain the JWT token");
      }
      AuthenticationToken = result.AccessToken;
   }
   ```
 
8.  Suchen Sie mithilfe des NuGet-Paket-Managers nach „2018-03-01-hybrid“, und installieren Sie die Pakete, die diesem Profil zugeordnet sind, für die Ressourcenanbieter Compute, Netzwerk, Speicher, KeyVault und App Services.

2.  Legen Sie in jeder Aufgabe in der CS-Datei die erforderlichen Parameter für das Arbeiten mit Azure Stack fest. Hier sehen Sie ein Beispiel für die Aufgabe `CreateResourceGroupTest`:

   ```csharp
   var location = Environment.GetEnvironmentVariable("AZURE_LOCATION");
   var baseUriString = Environment.GetEnvironmentVariable("AZURE_BASE_URL");
   var resourceGroupName = Environment.GetEnvironmentVariable("AZURE_RESOURCEGROUP");
   var servicePrincipalId = Environment.GetEnvironmentVariable("AZURE_CLIENT_ID");
   var servicePrincipalSecret = Environment.GetEnvironmentVariable("AZURE_CLIENT_SECRET");
   var azureResourceId = Environment.GetEnvironmentVariable("AZURE_RESOURCE_ID");
   var tenantId = Environment.GetEnvironmentVariable("AZURE_TENANT_ID");
   var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
   var credentials = new CustomLoginCredentials(servicePrincipalId, servicePrincipalSecret, azureResourceId, tenantId);
   ```

1.  Klicken Sie mit der rechten Maustaste auf die einzelnen Aufgaben, und wählen Sie **Test ausführen** aus.

    1.  Die grünen Häkchen im Fenster im Seitenbereich weisen Sie auf die erfolgreiche Erstellung der einzelnen Aufgabe basierend auf den angegebenen Parametern hin. Überprüfen Sie Ihr Azure Stack-Abonnement, um sicherzustellen, dass die Ressourcen erfolgreich erstellt wurden.

    2.  Weitere Informationen zum Ausführen von Komponententests finden Sie unter [Ausführen von Komponententests mit Test-Explorer][].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu API-Profilen finden Sie unter:

- [Verwalten von API-Versionsprofilen in Azure Stack](azure-stack-version-profiles.md)
- [Von Profilen unterstützte API-Versionen von Ressourcenanbietern](azure-stack-profiles-azure-resource-manager-versions.md)

  [Erste Schritte – Installieren von Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [Suchen und Installieren eines Pakets]: /nuget/tools/package-manager-ui
  [NuGet-Paket-Manager-Anweisungen]: https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager
  [Erstellen von Abonnements für Angebote in Azure Stack]: ../azure-stack-subscribe-plan-provision-vm.md
  [Bereitstellen des Anwendungszugriffs auf Azure Stack]: ../azure-stack-create-service-principals.md
  [*Mandanten-ID*]: ../azure-stack-identity-overview.md
  [*Abonnement-ID*]: ../azure-stack-plan-offer-quota-overview.md#subscriptions
  [*Azure Stack-Resource Manager-Endpunkt*]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [Zusammenfassung zu API-Profilen]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Testprojekt für virtuelle Computer, vNet, Ressourcengruppen und -konten]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Verwenden von Azure PowerShell zum Erstellen eines Dienstprinzipals mit einem Zertifikat]: ../azure-stack-create-service-principals.md
  [Ausführen von Komponententests mit Test-Explorer]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
