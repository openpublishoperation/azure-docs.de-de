---
title: Grundlegendes zum Azure Active Directory-App-Manifest | Microsoft-Dokumentation
description: Sie erhalten ausführliche Informationen über das Azure Active Directory-App-Manifest, das die Identitätskonfiguration einer Anwendung in einem Azure AD-Mandanten darstellt, und für die OAuth-Autorisierung, den Zustimmungsprozess und andere Zwecke genutzt wird.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: 74bcd1e795186eba652d2da986c290e1989dd1d1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041573"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory-App-Manifest

Apps, die in Azure Active Directory (Azure AD) integriert werden, müssen bei einem Azure AD-Mandanten registriert werden. Sie können die App im [Azure-Portal](https://portal.azure.com) konfigurieren, indem Sie unter **Azure Active Directory** die Option **App-Registrierungen**, die zu konfigurierende App und dann **Manifest** auswählen.

## <a name="manifest-reference"></a>Manifestreferenz

> [!NOTE]
> Wird nach der **Beschreibung** nicht die Spalte **Beispielwert** angezeigt, maximieren Sie das Browserfenster, und scrollen oder wischen Sie, bis die Spalte **Beispielwert** angezeigt wird.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Schlüssel  | Werttyp | BESCHREIBUNG  | Beispielwert |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Nullable Int32 | Gibt die akzeptierte Zugriffstokenversion für die aktuelle API-Ressource an. Mögliche Werte: 1, 2, Null. Der Standardwert ist Null, was als 2 behandelt wird. | `2` |
| `allowPublicClient` | boolean | Gibt den Fallbackanwendungstyp zurück. Azure AD leitet den Anwendungstyp standardmäßig von „replyUrlsWithType“ ab. Es gibt bestimmte Szenarien, in den Azure AD den Client-App-Typ nicht bestimmen kann (etwa beim [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3)-Fluss, in dem die HTTP-Anforderung ohne URL-Umleitung ausgeführt wird). In diesen Fällen interpretiert Azure AD den Anwendungstyp basierend auf dem Wert dieser Eigenschaft. Wird für diesen Wert TRUE verwendet, wird der Fallbackanwendungstyp als öffentlicher Client festgelegt, etwa als installierte App, die auf einem mobilen Gerät ausgeführt wird. Der Standardwert ist FALSE. Das bedeutet, dass der Fallbackanwendungstyp ein vertraulicher Client ist, etwa eine Web-App. | `false` |
| `appId` | Bezeichnerzeichenfolge | Gibt den eindeutigen Bezeichner für die App an, die einer App von Azure AD zugewiesen wird. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | Arraytyp | Gibt Auflistung der Rollen an, die von einer App deklariert werden können. Diese Rollen können Benutzern, Gruppen oder Dienstprinzipalen zugewiesen werden. Weitere Beispiele und Informationen finden Sie unter [Hinzufügen von App-Rollen in Ihrer Anwendung und Empfangen der Rollen im Token](howto-add-app-roles-in-azure-ad-apps.md). | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `groupMembershipClaims` | Zeichenfolge | Eine Bitmaske, die den in einem Benutzer- oder OAuth 2.0-Zugriffstoken ausgegebenen Anspruch `groups` konfiguriert, der von der App erwartet wird. Die Bitmaskenwerte sind:<br>0: Keine<br>1: Sicherheitsgruppen und Azure AD-Rollen<br>2: Reserviert<br>4: Reserviert<br>Durch das Festlegen der Bitmaske auf 7 werden alle Sicherheitsgruppen, Verteilergruppen und Azure AD-Verzeichnisrollen abgerufen, bei denen der angemeldete Benutzer ein Mitglied ist. | `1` |
| `optionalClaims` | Zeichenfolge | Die optionalen Ansprüche, die im Token vom Sicherheitstokendienst für diese spezifische App zurückgegeben werden. Weitere Informationen finden Sie unter [Optionale Ansprüche](active-directory-optional-claims.md). | `null` |
| `id` | Bezeichnerzeichenfolge | Ein eindeutiger Bezeichner für die App im Verzeichnis. Diese ID ist nicht der Bezeichner, der verwendet wird, um die App in einer Protokolltransaktion zu bezeichnen. Sie dient dazu, in Verzeichnisabfragen auf das Objekt zu verweisen. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | Zeichenfolgenarray | Benutzerdefinierte URIs, die eine Web-App innerhalb des zugehörigen Azure AD-Mandanten oder innerhalb einer überprüften benutzerdefinierten Domäne eindeutig identifizieren, wenn es sich um eine mehrinstanzenfähige App handelt. | <code>[<br>&nbsp;&nbsp;"https://MyRegistererdApp"<br>]</code> |
| `informationalUrls` | Zeichenfolge | Gibt die Links zu den Nutzungsbedingungen und Datenschutzbestimmungen der App an. Die Nutzungsbedingungen und Datenschutzbestimmungen werden auf der Oberfläche für die Benutzerzustimmung angezeigt. Weitere Informationen finden Sie unter [Nutzungsbedingungen und Datenschutzerklärung für registrierte Azure Active Directory-Apps](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | Arraytyp | Enthält Verweise auf von der App zugewiesene Anmeldeinformationen, auf Zeichenfolgen basierende gemeinsame geheime Schlüssel und X.509-Zertifikate. Diese Anmeldeinformationen werden beim Anfordern von Zugriffstokens verwendet (wenn die App nicht als Ressource, sondern als Client agiert). | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | Arraytyp | Wird zur Bündelung der Zustimmung verwendet, wenn Sie eine aus zwei Teilen bestehende Lösung haben (eine Client-App und eine benutzerdefinierte Web-API-App). Wenn Sie hier die App-ID der Client-App eingeben, muss der Benutzer nur einmal seine Zustimmung zur Verwendung der Client-App geben. Azure AD weiß, dass die Zustimmung für die Clientanwendung implizit die Zustimmung zur Verwendung der Web-API beinhaltet, und stellt die Dienstprinzipale für die Client- und Web-API automatisch bereit. Die Client- und Web-API-App müssen beim selben Mandanten registriert sein. | `[GUID]` |
| `logoUrl` | Zeichenfolge | Schreibgeschützter Wert, der auf die CDN-URL des Logos verweist, das im Portal hochgeladen wurde | `https://MyRegisteredAppLogo` |
| `logoutUrl` | Zeichenfolge | Die URL zum Abmelden von der App. | `https://MyRegisteredAppLogout` |
| `name` | Zeichenfolge | Der Anzeigename für die App. | `MyRegisteredApp` |
| `oauth2AllowImplicitFlow` | boolean | Dieser Wert gibt an, ob die Web-App Zugriffstoken des impliziten OAuth 2.0-Flusses anfordern kann. Die Standardeinstellung ist „false“. Dieses Flag wird für browserbasierte Apps wie JavaScript-basierte Single-Page-Webanwendungen verwendet. Wenn Sie weitere Informationen benötigen, geben Sie im Inhaltsverzeichnis `OAuth 2.0 implicit grant flow` ein, und sehen Sie sich die Themen zum impliziten Flow an. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | boolean | Dieser Wert gibt an, ob die Web-App ID-Token des impliziten OAuth 2.0-Flusses anfordern kann. Die Standardeinstellung ist „false“. Dieses Flag wird für browserbasierte Apps wie JavaScript-basierte Single-Page-Webanwendungen verwendet. | `false` |
| `oauth2Permissions` | Arraytyp | Gibt die Sammlung von OAuth 2.0-Berechtigungsbereichen an, die die Web-API-App (Ressource) für Client-Apps verfügbar macht. Diese Berechtigungsbereiche können Client-Apps im Zuge der Zustimmung gewährt werden. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"u| Gibt die Sammlung von OAuth 2.0-Berechtigungsbereichen an, die die Web-API-App (Ressource) für Client-Apps verfügbar macht. Diese Berechtigungsbereiche können Client-Apps im Zuge der Zustimmung gewährt werden. ser_impersonation"<br>&nbsp;&nbsp;}<br>]</code> |
| `oauth2RequiredPostResponse` | boolean | Gibt an, ob Azure AD im Rahmen von OAuth 2.0-Tokenanforderungen POST-Anforderungen zulässt (im Gengensatz zu GET-Anforderungen). Beim Standartwert „false“ sind nur GET-Anforderungen zulässig. | `false` |
| `parentalControlSettings` | Zeichenfolge | `countriesBlockedForMinors` gibt an, die Länder/Regionen an, in denen die App für Minderjährige blockiert wird.<br>`legalAgeGroupRule` gibt die Regel für die rechtliche Altersgruppe an, die für Benutzer der App gilt. Dieser Wert kann auf `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids` oder `BlockMinors` festgelegt werden.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>}</code> |
| `passwordCredentials` | Arraytyp | Siehe Beschreibung für die `keyCredentials`-Eigenschaft. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>]</code> |
| `preAuthorizedApplications` | Arraytyp | Listet Anwendungen und angeforderte Berechtigungen für die implizite Einwilligung auf. Ein Administrator muss seine Einwilligung zur Anwendung gegeben haben. Bei „preAuthorizedApplications“ muss der Benutzer den angeforderten Berechtigungen nicht zustimmen. Für die in „preAuthorizedApplications“ aufgelisteten Berechtigungen ist keine Benutzereinwilligung erforderlich. Weitere angeforderte Berechtigungen, die nicht in „preAuthorizedApplications“ aufgeführt sind, erfordern jedoch die Benutzereinwilligung. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `replyUrlsWithType` | Zeichenfolgenarray | Diese Eigenschaft, die mehrere Werte zulässt, enthält die Liste der registrierten „redirect_uri“-Werte, die von Azure AD bei der Rückgabe von Token als Ziele akzeptiert werden. Jeder URI-Wert muss einen zugehörigen App-Typwert enthalten. Folgende Typwerte werden unterstützt: `Web` und `InstalledClient`. | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | Arraytyp | Mit dynamischer Einwilligung steuert `requiredResourceAccess` die Einwilligungsoberfläche für Administratoren und Benutzer, die statische Einwilligung verwenden. Die Oberfläche für die Benutzerzustimmung für den Allgemeinfall wird jedoch nicht gesteuert.<br>`resourceAppId` ist der eindeutige Bezeichner für die Ressource, auf die die App zugreifen muss. Dieser Wert muss mit der „appId“ identisch sein, die für die Zielressourcen-App deklariert wurde.<br>`resourceAccess` ist ein Array, das die OAuth 2.0-Berechtigungsbereiche und App-Rollen auflistet, welche die App für die jeweilige Ressource erfordert. Enthält die `id`- und `type`-Werte der jeweiligen Ressourcen. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `samlMetadataUrl` | Zeichenfolge | Die URL zu den SAML-Metadaten für die App. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | Zeichenfolge | Gibt die URL zur Startseite der App an. | `https://MyRegisteredApp` |
| `signInAudience` | Zeichenfolge | Gibt an, welche Microsoft-Konten für die aktuelle Anwendung unterstützt werden. Folgende Werte werden unterstützt:<ul><li>**AzureADMyOrg:** Benutzer mit einem Geschäfts-, Schul- oder Unikonto von Microsoft im Azure AD-Mandanten meiner Organisation (einzelner Mandant)</li><li>**AzureADMultipleOrgs:** Benutzer mit einem Geschäfts-, Schul- oder Unikonto von Microsoft im Azure AD-Mandanten einer beliebigen Organisation (mehrinstanzenfähig)</li> <li>**AzureADandPersonalMicrosoftAccount:** Benutzer mit einem persönlichen Microsoft-Konto oder einem Geschäfts-, Schul- oder Unikonto im Azure AD-Mandanten einer beliebigen Organisation</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | Zeichenfolgenarray | Benutzerdefinierte Zeichenfolgen, die zum Kategorisieren und Identifizieren der Anwendung verwendet werden können | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Beziehungen zwischen den App- und Dienstprinzipalobjekten einer App finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](app-objects-and-service-principals.md).
* Unter [Azure AD-Entwicklerglossar](developer-glossary.md) finden Sie Definitionen für einige der wichtigsten Konzepte für Azure Active Directory-Entwickler (AD).

Bitte geben Sie uns über den folgenden Kommentarabschnitt Ihr Feedback, um uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
