---
title: 'Tutorial: Azure Active Directory-Integration mit LinkedIn Learning | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedIn Learning konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 02e7d9d26b389e82365f3447cceb5566244236f1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440907"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: Azure Active Directory-Integration mit LinkedIn Learning

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Learning in Azure Active Directory (Azure AD) integrieren.

Die Integration von LinkedIn Learning in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LinkedIn Learning hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Learning anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LinkedIn Learning konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LinkedIn Learning-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von LinkedIn Learning aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-linkedin-learning-from-the-gallery"></a>Hinzufügen von LinkedIn Learning aus dem Katalog
Zum Konfigurieren der Integration von LinkedIn Learning in Azure AD müssen Sie LinkedIn Learning aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um LinkedIn Learning aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![ANWENDUNGEN][3]

1. Geben Sie **LinkedIn Learning** im Suchfeld ein. Wählen Sie im Ergebnisbereich die Option **LinkedIn Learning** aus, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei LinkedIn Learning mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LinkedIn Learning als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Learning muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in LinkedIn Learning zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei LinkedIn Learning über Azure AD müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines LinkedIn Learning-Testbenutzers](#creating-a-linkedin-learning-test-user)** – um das einmalige Anmelden über Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden über Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LinkedIn Learning-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei LinkedIn Learning über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LinkedIn Learning** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial-linkedin_01.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem LinkedIn Learning-Mandanten als Administrator an.

1. Klicken Sie im **Account Center** unter **Settings** auf **Global Settings**. Wählen Sie außerdem aus der Dropdownliste die Option **Learning - Default** aus.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

1. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, und kopieren Sie die **Entitäts-ID** und **Assertion Consumer Access(ACS)-URL**.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

1. Führen Sie im Azure-Portal unter **Domäne und URLs für LinkedIn Learning** die folgenden Schritte aus, wenn Sie das einmalige Anmelden im Modus **IdP-initiiert** konfigurieren möchten.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie im Feld **Antwort-URL** den Wert für **Assertion Consumer Access (ACS) Url** ein, den Sie aus dem LinkedIn-Portal kopiert haben.

1. Wenn Sie das einmalige Anmelden im Modus **SP-initiiert** konfigurieren möchten, klicken Sie im Konfigurationsabschnitt auf die Option „Erweiterte URL-Einstellungen anzeigen“, und konfigurieren Sie die Anmelde-URL nach folgendem Muster:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
1. Die LinkedIn Learning-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, LinkedIn Learning erwartet jedoch, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste verwenden oder den entsprechenden Attributwert gemäß der Konfiguration in Ihrer Organisation angeben. 

    ![Configure single sign-on](./media/linkedinlearning-tutorial/updateusermail.png)
    
1. Klicken Sie im Abschnitt **Benutzerattribute** auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und legen Sie die Attribute fest. Der Benutzer muss vier Ansprüche mit den Namen **email**, **department**, **firstname** und **lastname** hinzufügen, und der Wert muss mit **user.mail**, **user.department**, **user.givenname** bzw. **user.surname** zugeordnet werden.

    | Attributname | Attributwert |
    | --- | --- |
    | E-Mail| user.mail |    
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/userattribute.png)
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld zum Attribut zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/tutorial_attribute_04.png)

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

1. Führen Sie für das Attribut **Name** die folgenden Schritte aus:

    a. Klicken Sie auf das Attribut, um das Fenster **Attribut bearbeiten** zu öffnen.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/url_update.png)

    b. Löschen Sie den URL-Wert aus dem **Namespace**.
    
    c. Klicken Sie auf **OK**, um die Einstellung zu speichern.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png)

1. Klicken Sie auf **Speichern**.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_general_400.png)

1. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Laden Sie die XML-Datei hoch, die Sie gerade aus dem Azure-Portal heruntergeladen haben, indem Sie auf die Option „XML-Datei hochladen“ klicken.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

1. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

### <a name="creating-a-linkedin-learning-test-user"></a>Erstellen eines LinkedIn Learning-Testbenutzers

Die LinkedIn Learning-Anwendung unterstützt die Just-In-Time-Benutzerbereitstellung. Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Aktivieren Sie im LinkedIn Learning-Portal auf der Seite mit den Administratoreinstellungen die Option **Automatically Assign licenses** (Lizenzen automatisch zuweisen), um die Just-In-Time-Bereitstellung zu aktivieren. Dadurch wird dem Benutzer auch eine Lizenz zugewiesen.

   ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf LinkedIn Learning gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu LinkedIn Learning zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

1. Wählen Sie in der Anwendungsliste **LinkedIn Learning** aus.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png)

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LinkedIn Learning“ klicken, sollte die Azure-Anmeldeseite geöffnet werden. Nach der erfolgreichen Anmeldung auf diese Seite sollten Sie Ihre LinkedIn Learning-Anwendung öffnen können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/linkedinlearning-tutorial/tutorial_general_203.png
