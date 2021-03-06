---
title: Einrichten der Anmeldung bei Azure Active Directory-Konten mittels einer integrierten Richtlinie in Azure Active Directory B2C | Microsoft-Dokumentation
description: Einrichten der Anmeldung bei Azure Active Directory-Konten mittels einer integrierten Richtlinie in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5f51fbff11412324ad167d49202f7215cefb5ac2
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076917"
---
# <a name="set-up-sign-in-azure-active-directory-accounts-a-built-in-policy-in-azure-active-directory-b2c"></a>Einrichten der Anmeldung bei Azure Active Directory-Konten mittels einer integrierten Richtlinie in Azure Active Directory B2C

>[!NOTE]
> Dieses Feature befindet sich in der Phase der öffentlichen Vorschau. Verwenden Sie dieses Feature nicht in Produktionsumgebungen.

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer einer bestimmten Azure Active Directory-Organisation (Azure AD) mittels einer integrierten Richtlinie in Azure AD B2C aktivieren.

## <a name="create-an-azure-ad-app"></a>Erstellen einer Azure AD-App

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren.

>[!NOTE]
>In den folgenden Anweisungen wird `Contoso.com` für den Azure AD-Mandanten der Organisation und `fabrikamb2c.onmicrosoft.com` als Azure AD B2C-Mandant verwendet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten der Organisation („contoso.com“) enthält, indem Sie im oberen Menü auf den Verzeichnis- und Abonnementfilter klicken und das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Registrierung einer neuen Anwendung** aus.
5. Geben Sie einen Namen für Ihre Anwendung ein. Beispiel: `Azure AD B2C App`.
6. Wählen Sie als **Anwendungstyp** die Option `Web app / API` aus.
7. Geben Sie als **Anmelde-URL** die folgende URL in Kleinbuchstaben ein, und ersetzen Sie dabei `your-tenant` durch den Namen Ihres Azure AD B2C-Mandanten („fabrikamb2c.onmicrosoft.com“):

    ```
    https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```

    Alle URLs sollten jetzt [b2clogin.com](b2clogin.md) verwenden.

8. Klicken Sie auf **Create**. Kopieren Sie die **Anwendungs-ID** zur späteren Verwendung.
9. Wählen Sie die Anwendung und dann **Einstellungen** aus.
10. Wählen Sie **Schlüssel** aus, geben Sie die Schlüsselbeschreibung ein, wählen Sie eine Dauer aus, und klicken Sie dann auf **Speichern**. Kopieren Sie den angezeigten Wert des Schlüssels zur späteren Verwendung.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Azure AD als Identitätsanbieter in Ihrem Mandanten

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das den Azure AD B2C-Mandanten („fabrikamb2c.onmicrosoft.com“) enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen, das Ihren Azure AD B2C-Mandanten enthält.
2. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
3. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
4. Geben Sie einen **Namen** ein. Geben Sie beispielsweise „Contoso Azure AD“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **OpenID Connect (Vorschau)** aus, und klicken Sie dann auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**.
7. Geben Sie für **Metadaten-URL** die folgende URL ein, in der `your-tenant` durch den Namen Ihres Azure AD-Mandanten ersetzt wird:

    ```
    https://login.microsoftonline.com/your-tenant/.well-known/openid-configuration
    ```
8. Geben Sie für die **Client-ID** die Anwendungs-ID und für **Clientgeheimnis** den Schlüsselwert ein, die Sie beide zuvor notiert haben.
9. Geben Sie optional einen Wert für **Domänenhinweis** ein (z.B. `ContosoAD`). Dieser Wert wird zum Verweis auf diesen Identitätsanbieter mit *domain_hint* in der Anforderung verwendet. 
10. Klicken Sie auf **OK**.
11. Wählen Sie **Ansprüche dieses Identitätsanbieters zuordnen** aus, und legen Sie die folgenden Ansprüche fest:
    
    - Geben Sie für **Benutzer-ID** `oid` ein.
    - Geben Sie für **Anzeigename** `name` ein.
    - Geben Sie für **Vorname** `given_name` ein.
    - Geben Sie für **Nachname** `family_name` ein.
    - Geben Sie für **E-Mail** `unique_name` ein.

12. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Konfiguration zu speichern.
