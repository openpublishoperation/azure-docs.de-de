---
title: 'Tutorial: Azure Active Directory-Integration mit xMatters OnDemand | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und xMatters OnDemand konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: a235b85887e64e0a5ca35aae8f31734250a78bb5
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160233"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Tutorial: Azure Active Directory-Integration mit xMatters OnDemand

In diesem Tutorial erfahren Sie, wie Sie xMatters OnDemand in Azure Active Directory (Azure AD) integrieren.

Die Integration von xMatters OnDemand in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf xMatters OnDemand hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei xMatters OnDemand anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit xMatters OnDemand konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein xMatters OnDemand-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von xMatters OnDemand aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Hinzufügen von xMatters OnDemand aus dem Katalog
Zum Konfigurieren der Integration von xMatters OnDemand in Azure AD müssen Sie xMatters OnDemand aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um xMatters OnDemand aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld **xMatters OnDemand** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

1. Wählen Sie im Ergebnisbereich **xMatters OnDemand** aus, und klicken Sie dann auf **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand einer Testbenutzerin namens Britta Simon das einmalige Anmelden von Azure AD bei xMatters OnDemand.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer das Pendant in xMatters OnDemand zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in xMatters OnDemand muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in xMatters OnDemand den Wert von **Benutzername** in Azure AD als Wert für **Benutzername** zu, um die Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei xMatters OnDemand müssen Sie die folgenden Schritte ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines xMatters OnDemand-Testbenutzers](#creating-a-xmatters-ondemand-test-user)**, um ein Pendant von Britta Simon in xMatters OnDemand zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer xMatters OnDemand-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei xMatters OnDemand die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **xMatters OnDemand** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für xMatters OnDemand** die folgenden Schritte aus:

    ![Configure single sign-on](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von xMatters OnDemand](https://www.xmatters.com/company/contact-us/), um diese Werte zu erhalten.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei lokal unter **c:\\XMatters OnDemand.cer**.

    ![Configure single sign-on](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)

    > [!IMPORTANT]
    > Sie müssen das Zertifikat an das [xMatters OnDemand-Supportteam](https://www.xmatters.com/company/contact-us/) weiterleiten. Das Zertifikat muss vom xMatters-Supportteam hochgeladen werden, bevor Sie die Konfiguration der einmaligen Anmeldung abschließen können. 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/xmatters-ondemand-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **xMatters OnDemand-Konfiguration** auf **xMatters OnDemand konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

1. Melden Sie sich in einem anderen Webbrowserfenster bei der xMatters OnDemand-Unternehmenswebsite als Administrator an.

1. Klicken Sie in der Symbolleiste oben auf **Administrator**, und klicken Sie dann in der Navigationsleiste auf der linken Seite auf **Unternehmensdetails**.

    ![Admin](./media/xmatters-ondemand-tutorial/IC776795.png "Admin")

1. Führen Sie auf der Seite **SAML-Konfiguration** die folgenden Schritte aus:

    ![SAML-Konfiguration](./media/xmatters-ondemand-tutorial/IC776796.png "SAML-Konfiguration")

    a. Wählen Sie **SAML aktivieren**.

    b. Fügen Sie in das Textfeld **Identity Provider ID** (Identitätsanbieter-ID) den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie im Textfeld **Single Sign On URL** (URL für einmaliges Anmelden) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie im Textfeld **Single Logout URL** (URL für einmaliges Abmelden) die **Abmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    e. Klicken Sie auf der Seite „Unternehmensdetails“ oben auf **Speichern**.

    ![Unternehmensdetails](./media/xmatters-ondemand-tutorial/IC776797.png "Unternehmensdetails")

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/xmatters-ondemand-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/xmatters-ondemand-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/xmatters-ondemand-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

### <a name="creating-a-xmatters-ondemand-test-user"></a>Erstellen eines xMatters OnDemand-Testbenutzers

In diesem Abschnitt wird in xMatters OnDemand eine Benutzerin namens Britta Simon erstellt.

**Wenn Sie einen Benutzer manuell erstellen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **XMatters OnDemand** -Mandanten an.

1.  Klicken Sie auf die Registerkarte **Benutzer** und dann auf **Benutzer hinzufügen**.

    ![Benutzer](./media/xmatters-ondemand-tutorial/IC781048.png "Benutzer")

1. Führen Sie im Abschnitt **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/xmatters-ondemand-tutorial/IC781049.png "Benutzer hinzufügen")

    a. Wählen Sie **Aktiv**.

    b. Geben Sie im Textfeld **Benutzer-ID** die Benutzer-ID des Benutzers ein, z.B. Brittasimon@contoso.com.

    c. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein, z.B. Britta.

    d. Geben Sie im Textfeld **Nachname** den Nachnamen des Benutzers ein, z.B. Simon.

    e. Geben Sie im Textfeld **Website** die gültige Website eines gültigen Azure AD-Kontos ein, das Sie bereitstellen möchten.

    f. Klicken Sie auf **Speichern**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf xMatters OnDemand gewähren.

![Benutzer zuweisen][200] 

**Führen Sie zum Zuweisen von Britta Simon zu xMatters OnDemand folgende Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **xMatters OnDemand** aus.

    ![Configure single sign-on](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „xMatters OnDemand“ klicken, sollten Sie automatisch bei Ihrer xMatters OnDemand-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/xmatters-ondemand-tutorial/tutorial_general_203.png
