---
title: Grundlegendes zum Zugriff auf LUIS-Anwendungen
titleSuffix: Azure Cognitive Services
description: Erstellungszugriff ist für Besitzer und Projektmitarbeiter möglich. Bei einer privaten App ist der Zugriff auf den Endpunkt für Besitzer und Projektmitarbeiter möglich. Bei einer öffentlichen App ist der Zugriff auf den Endpunkt für jede Person möglich, die über ein eigenes LUIS-Konto und die öffentliche App-ID verfügt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: d0f9d0834cffd642961b2c49d5d252a665b49e73
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637409"
---
# <a name="authoring-and-endpoint-user-access"></a>Zugriff für die Erstellung und Benutzerzugriff auf Endpunkte
Erstellungszugriff ist für Besitzer und Projektmitarbeiter möglich. Bei einer privaten App ist der Zugriff auf den Endpunkt für Besitzer und Projektmitarbeiter möglich. Bei einer öffentlichen App ist der Zugriff auf den Endpunkt für jede Person möglich, die über ein eigenes LUIS-Konto und die öffentliche App-ID verfügt. 

## <a name="access-to-authoring"></a>Zugriff für die Erstellung
Der Zugriff auf die App von der [LUIS](luis-reference-regions.md#luis-website)-Website oder den [Erstellungs-APIs](https://aka.ms/luis-authoring-apis) wird vom Besitzer der App gesteuert. 

Der Besitzer und alle Projektmitarbeiter haben Zugriff für die Erstellung der App. 

|Der Zugriff für die Erstellung umfasst|Notizen|
|--|--|
|Hinzufügen oder Entfernen von Endpunktschlüsseln||
|Exportieren von Versionen||
|Exportieren von Endpunktprotokollen||
|Importieren von Versionen||
|Öffentlichmachen von Apps|Wenn eine App öffentlich ist, kann jede Person mit einem Erstellungs- oder Endpunktschlüssel die App abfragen.|
|Ändern von Modellen|
|Veröffentlichen|
|Überprüfen von Endpunktäußerungen für das [aktive Lernen](luis-how-to-review-endoint-utt.md)|
|Trainieren|

## <a name="access-to-endpoint"></a>Zugriff auf den Endpunkt
Der Zugriff zum Abfragen des Endpunkts wird von einer Einstellung auf der Seite **Anwendungsinformationen** im Abschnitt **Verwalten** gesteuert. 

![Festlegen der App als öffentlich](./media/luis-concept-security/set-application-as-public.png)

|[Privater Endpunkt](#private-app-endpoint-security)|[Öffentlicher Endpunkt](#public-app-endpoint-access)|
|:--|:--|
|Für Besitzer und Projektmitarbeiter verfügbar|Für Besitzer, Projektmitarbeiter und alle Personen verfügbar, denen die App-ID bekannt ist|

### <a name="private-app-endpoint-security"></a>Sicherheit am Endpunkt privater Apps
Der Endpunkt einer privaten App ist nur für Folgendes verfügbar:

|Schlüssel und Benutzer|Erklärung|
|--|--|--|
|Erstellungsschlüssel des Besitzers| Bis zu 1.000 Endpunkttreffer|
|Erstellungsschlüssel von Projektmitarbeitern| Bis zu 1.000 Endpunkttreffer|
|Jeder LUIS von einem Ersteller oder Projektmitarbeiter zugewiesene Schlüssel|Basierend auf dem Tarif für die Schlüsselverwendung|

#### <a name="microsoft-user-accounts"></a>Microsoft-Benutzerkonten
Ersteller und Projektmitarbeiter können einer privaten LUIS-App Schlüssel zuweisen. Bei dem Microsoft-Benutzerkonto, das den LUIS-Schlüssel im Azure-Portal erstellt, muss es sich um den App-Besitzer oder einen App-Projektmitarbeiter handeln. Sie können einer privaten App keine Schlüssel von einem anderen Azure-Konto zuweisen.

Unter [Azure Active Directory-Mandantenbenutzer](luis-how-to-collaborate.md#azure-active-directory-tenant-user) finden Sie weitere Informationen zu Active Directory-Benutzerkonten. 

### <a name="public-app-endpoint-access"></a>Zugriff auf Endpunkte öffentlicher Apps
Wenn eine App als öffentlich konfiguriert ist, können _alle_ gültigen LUIS-Erstellungsschlüssel oder LUIS-Endpunktschlüssel Ihre App abfragen, solange der Schlüssel nicht das gesamte Endpunktkontingent verbraucht hat.

Ein Benutzer, der nicht Besitzer oder Projektmitarbeiter ist, hat Zugriff auf eine öffentliche App, wenn er die App-ID hat. LUIS verfügt nicht über einen öffentlichen _Markt_ oder eine andere Möglichkeit der Suche nach einer öffentlichen App.  

Eine öffentliche App wird in allen Regionen veröffentlicht, so dass ein Benutzer mit einem regionsbasierten LUIS-Ressourcenschlüssel auf die App in jeder Region zugreifen kann, die dem Ressourcenschlüssel zugeordnet ist.

## <a name="microsoft-user-accounts"></a>Microsoft-Benutzerkonten
Ersteller und Projektmitarbeiter können LUIS auf der Seite „Veröffentlichen“ Schlüssel hinzufügen. Bei dem Microsoft-Benutzerkonto, das den LUIS-Schlüssel im Azure-Portal erstellt, muss es sich um den App-Besitzer oder einen App-Projektmitarbeiter handeln. 

Unter [Azure Active Directory-Mandantenbenutzer](luis-how-to-collaborate.md#azure-active-directory-tenant-user) finden Sie weitere Informationen zu Active Directory-Benutzerkonten. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->

## <a name="securing-the-endpoint"></a>Schützen von Endpunkten 
Sie können steuern, wer Ihre LUIS-Endpunktschlüssel anzeigen kann, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und die Authentifizierung und Autorisierung überprüft wurden, übergeben Sie den Aufruf an LUIS. Auch wenn diese Strategie Man-in-the-Middle-Angriffe nicht verhindert, verbirgt sie Ihren Endpunkt vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (z.B. [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Sicherheit und Compliance
 
[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-security-compliance.md)]

## <a name="next-steps"></a>Nächste Schritte

Unter [Best Practices](luis-concept-best-practices.md) erfahren Sie, wie Sie Absichten und Entitäten für die besten Vorhersagen verwenden.
