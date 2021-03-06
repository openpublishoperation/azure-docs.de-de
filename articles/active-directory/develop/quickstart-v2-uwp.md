---
title: 'Schnellstart: Azure AD v2 Windows UWP | Microsoft Docs'
description: Erfahren Sie, wie eine UWP-Anwendung (Universelle Windows-Plattform, XAML-Anwendung) ein Zugriffstoken erhalten und eine API aufrufen kann, die durch einen Azure Active Directory v2.0-Endpunkt geschützt ist.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 5241df87297fb2e293e2cc828821e66d6f2837b0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970822"
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Aufrufen der Microsoft Graph-API über eine UWP-Anwendung (UWP = Universelle Windows-Plattform)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Dieser Schnellstart enthält ein Codebeispiel, das zeigt, wie eine UWP-Anwendung (Universelle Windows-Plattform) Benutzer mit persönlichen oder Geschäfts-, Schul- oder Unikonten anmelden, ein Zugriffstoken abrufen und die Microsoft Graph-API aufrufen kann.

![Funktionsweise der in diesem Schnellstart generierten Beispiel-App](media/quickstart-v2-uwp/uwp-intro.png)

> [!div renderon="docs"]
> ## <a name="register-and-download"></a>Registrieren und herunterladen
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Registrieren und Konfigurieren Ihrer Anwendung und des Codebeispiels
> #### <a name="step-1-register-your-application"></a>Schritt 1: Registrieren Ihrer Anwendung
> Wenn Sie Ihre Anwendung registrieren und die Anwendungsregistrierungsinformationen Ihrer Projektmappe hinzufügen möchten, führen Sie folgende Schritte aus:
> 1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
> 1. Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein.
> 1. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) deaktiviert ist, und klicken Sie anschließend auf **Erstellen**.
> 1. Klicken Sie auf **Plattform hinzufügen** > **Native Anwendung** und anschließend auf **Speichern**.

> [!div renderon="portal" class="sxs-lookup alert alert-info"]
> #### <a name="step-1-configure-your-application"></a>Schritt 1: Konfigurieren Ihrer Anwendung
> Damit das Codebeispiel für diesen Schnellstart funktioniert, müssen Sie eine Umleitungs-URL als **urn:ietf:wg:oauth:2.0:oob** hinzufügen.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Diese Änderung für mich vornehmen]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ist konfiguriert](media/quickstart-v2-uwp/green-check.png) Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-your-visual-studio-project"></a>Schritt 2: Herunterladen des Visual Studio-Projekts

 - [Visual Studio 2017-Projekt herunterladen](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Schritt 3: Konfigurieren des Visual Studio-Projekts

1. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner (z.B. **C:\Azure-Samples**).
1. Öffnen Sie das Projekt in Visual Studio.
1. Bearbeiten Sie **App.Xaml.cs**, und ersetzen Sie die Zeile, die mit `private static string ClientId` beginnt, durch Folgendes:

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```

## <a name="more-information"></a>Weitere Informationen

Unten finden Sie eine Übersicht über diesen Schnellstart:

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) ist die Bibliothek zum Anmelden von Benutzern und Anfordern von Token, die für den Zugriff auf eine durch Microsoft Azure Active Directory geschützte API verwendet wird. Sie können sie installieren, indem Sie die folgenden Befehle in der *Paket-Manager-Konsole* von Visual Studio ausführen:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

### <a name="msal-initialization"></a>MSAL-Initialisierung

Sie können den Verweis auf MSAL hinzufügen, indem Sie die folgende Codezeile hinzufügen:

```csharp
using Microsoft.Identity.Client;
```

Initialisieren Sie MSAL anschließend mit der folgenden Codezeile:

```csharp
public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
```

> |Hinweis: ||
> |---------|---------|
> |ClientId | Die Anwendungs-ID der in *portal.microsoft.com* registrierten Anwendung. |

### <a name="requesting-tokens"></a>Anfordern von Token

MSAL verfügt über zwei Methoden, die zum Abrufen von Token verwendet werden: `AcquireTokenAsync` und `AcquireTokenSilentAsync`.

#### <a name="get-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

 In einigen Situationen müssen Benutzer gezwungen werden, über ein Popupfenster mit dem Azure Active Directory v2-Endpunkt zu interagieren, um entweder ihre Anmeldeinformationen zu überprüfen oder ihre Zustimmung zu erteilen, beispielsweise unter den folgenden Umständen:

- Erstmaliges Anmelden von Benutzern bei der Anwendung.
- Benutzer müssen möglicherweise ihre Anmeldeinformationen erneut eingeben, da das Kennwort abgelaufen ist.
- Ihre Anwendung fordert Zugriff auf eine Ressource, der der Benutzer zustimmen muss.
- Die zweistufige Authentifizierung ist erforderlich.

```csharp
authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
```

> |Hinweis:||
> |---------|---------|
> |Bereiche | Enthält die angeforderten Bereiche (d.h. `{ "user.read" }` für Microsoft Graph oder `{ "api://<Application ID>/access_as_user" }` für benutzerdefinierte Web-APIs). |

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Sie möchten nicht, dass der Benutzer seine Anmeldeinformationen jedes Mal überprüfen muss, wenn er auf eine Ressource zugreift. In der Regel sollen der Abruf und die Erneuerung von Token ohne Benutzerinteraktion erfolgen. `AcquireTokenSilentAsync` ist die gängige Methode zum Abrufen von Token, die für den Zugriff auf geschützte Ressourcen nach dem ersten `AcquireTokenAsync`-Vorgang verwendet werden:

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, accounts.FirstOrDefault());
```

> |Hinweis: ||
> |---------|---------|
> |Bereiche | Enthält die angeforderten Bereiche (d.h. `{ "user.read" }` für Microsoft Graph oder `{ "api://<Application ID>/access_as_user" }` für benutzerdefinierte Web-APIs). |
> |accounts.FirstOrDefault() | Der erste Benutzer im Cache (MSAL unterstützt mehrere Benutzer in einer App). |

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie das Windows Desktop-Tutorial aus, um eine vollständige Schritt-für-Schritt-Anleitung zum Erstellen von Anwendungen und neuen Features zu erhalten, einschließlich einer vollständigen Erläuterung dieses Schnellstarts.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Informieren Sie sich über die Schritte zum Erstellen der in diesem Schnellstart verwendeten Anwendung.

> [!div class="nextstepaction"]
> [Tutorial: UWP – Aufrufen der Microsoft Graph-API](tutorial-v2-windows-uwp.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]