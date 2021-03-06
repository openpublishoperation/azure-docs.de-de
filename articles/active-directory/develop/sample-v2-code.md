---
title: Azure Active Directory-Codebeispiele | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Index der verfügbaren Azure Active Directory-Codebeispiele (v2-Endpunkt), geordnet nach Szenario.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e76097c0d0cbaf14f2fc2b1a407bc2d320a2091d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964397"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory-Codebeispiele (v2-Endpunkt)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Sie können Microsoft Azure Active Directory (Azure AD) für Folgendes verwenden:

- Hinzufügen von Authentifizierungs- und Autorisierungsfeatures zu Ihren Webanwendungen und Web-APIs
- Anfordern eines Zugriffstokens für den Zugriff auf eine geschützte Web-API

In diesem Artikel wird der Azure AD v2-Endpunkt kurz erläutert, und es werden Links zu Beispielen angegeben. Anhand dieser Beispiele wird die Vorgehensweise erläutert, und die Codeausschnitte können Sie in Ihren Anwendungen verwenden. Auf der Codebeispielseite finden Sie ausführliche Infothemen mit hilfreichen Angaben zu Anforderungen, Installation und Einrichtung. Außerdem enthält der Code Kommentare, die die wichtigsten Abschnitte besser verständlich machen.

> [!NOTE]
> Wenn Sie an Beispielen zu v1 interessiert sind, lesen Sie [Azure Active Directory-Codebeispiele](sample-v1-code.md).

Informationen zum grundlegenden Szenario für die einzelnen Beispieltypen finden Sie unter [App-Typen für den Azure Active Directory v2.0-Endpunkt](v2-app-types.md).

Sie können auch Beiträge zu den Beispielen auf GitHub schreiben. Wie das geht, erfahren Sie unter [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications-spa"></a>Single-Page-Webanwendungen (SPA)

Dieses Beispiel zeigt, wie Sie eine Single-Page-Webanwendung schreiben, die mit Azure AD gesichert ist. Diese Beispiele verwenden einen der Typen von „MSAL.js“: [Microsoft Authentication Library für JavaScript](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core), [Microsoft Authentication Library für Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular), [Microsoft Authentication Library für AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs).

 Plattform |  Aufrufen von Microsoft Graph
 -------- |  ---------------------
![JavaScript](media/sample-v2-code/logo_js.png) JavaScript (msal.js)  | [javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2)
![Angular JS](media/sample-v2-code/logo_angular.png) JavaScript (msal AngularJS) | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
![Angular](media/sample-v2-code/logo_angular.png) JavaScript (msal Angular) | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp)

## <a name="web-applications"></a>Webanwendungen

Die folgenden Beispiele zeigen Webanwendungen, die Benutzer anmelden. Einige Beispiele zeigen außerdem, wie die Anwendung Microsoft Graph oder Ihre eigene Web-API mit der Identität des Benutzers aufruft.

 Plattform | Nur Benutzeranmeldung | Benutzeranmeldung und Aufruf von Microsoft Graph
 -------- | ------------------- | ---------------------------------
![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET | [appmodelv2-webapp-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [dotnet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [aspnet-connect-rest-sample](https://github.com/microsoftgraph/aspnet-connect-rest-sample)
![ASP.NET](media/sample-v2-code/logo_NETcore.png)<p/>ASP.NET Core 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)
![Node.js](media/sample-v2-code/logo_nodejs.png)  |                   | [AppModelv2-WebApp-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)
![Ruby](media/sample-v2-code/logo_ruby.png) |                   | [ruby-connect-rest-sample](https://github.com/microsoftgraph/ruby-connect-rest-sample)

## <a name="desktop-and-mobile-public-client-apps"></a>Öffentliche Desktop- und mobile Client-Apps

Die folgenden Beispiele veranschaulichen öffentliche Clientanwendungen (Desktop-/mobile Anwendungen), die im Namen eines Benutzers mithilfe interaktiver Anmeldung auf die Microsoft Graph- oder Ihre eigene Web-API zugreifen. Alle diese Clientanwendungen verwenden Microsoft Authentication Libraries (MSAL).

Clientanwendung | Plattform | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET Core 2.0-Web-API
------------------ | -------- |  -------------------- | -------------------------
Desktop (WPF)      | ![.NET/C#](media/sample-v2-code/logo_NET.png) | [dotnet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobil (UWP)   | ![.NET/C# (UWP)](media/sample-v2-code/logo_windows.png) | [dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobil (Android, iOS, UWP)   | ![.NET/C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | [xamarin-native-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobil (iOS)       | ![iOS/Objective-C/Swift](media/sample-v2-code/logo_iOS.png) | [ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobil (Android)   | ![Android/Java](media/sample-v2-code/logo_Android.png) |   [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="daemon-applications"></a>Daemonanwendungen

Das folgende Beispiel zeigt eine Anwendung, die auf Microsoft Graph mit ihrer eigenen Identität (ohne Benutzer) zugreift.

Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph
------------------ | -------- | ---------- | --------------------
Web-App | ![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET  | Clientanmeldeinformationen | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2)

> Ein Beispiel für eine Desktopdaemonanwendung im Backlog.

## <a name="web-apis"></a>Web-APIs

Im folgenden Beispiel wird gezeigt, wie eine Web-API mit Azure AD v2-Endpunkt geschützt wird. Diese API wird von einer WPF-Anwendung ausgeführt (sie könnte aber von wirklich jeder Anwendung aufgerufen werden).

Plattform | Beispiel
 -------- | -------------------
![.NET/C#](media/sample-v2-code/logo_NET.png) | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)

## <a name="other-microsoft-graph-samples"></a>Weitere Microsoft Graph-Beispiele

[Beispiele](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) und Tutorials, die verschiedene Verwendungsmuster für die Microsoft Graph-API (einschließlich Authentifizierung mit Azure AD) veranschaulichen, finden Sie unter [Beispiele und Tutorials der Microsoft Graph-Community](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Weitere Informationen

[Entwicklerhandbuch zu Azure Active Directory](azure-ad-developers-guide.md)

[Azure AD-Graph-API – Konzepte und Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Bibliothek für das Azure AD Graph-API-Hilfsprogramm](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
