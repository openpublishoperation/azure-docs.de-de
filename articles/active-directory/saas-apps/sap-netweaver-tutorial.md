---
title: 'Tutorial: Azure Active Directory-Integration mit SAP NetWeaver | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP NetWeaver konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b773380a21e0c47a1e1519e592aa0ddd5e6388fa
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421949"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Tutorial: Azure Active Directory-Integration mit SAP NetWeaver

In diesem Tutorial erfahren Sie, wie Sie SAP NetWeaver in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP NetWeaver in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SAP NetWeaver hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP NetWeaver anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).


## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SAP NetWeaver konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SAP NetWeaver-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von SAP NetWeaver aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-netweaver-from-the-gallery"></a>Hinzufügen von SAP NetWeaver aus dem Katalog
Zum Konfigurieren der Integration von SAP NetWeaver in Azure AD müssen Sie SAP NetWeaver aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SAP NetWeaver aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld den Suchbegriff **SAP NetWeaver**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

1. Wählen Sie im Ergebnisbereich **SAP NetWeaver** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP NetWeaver mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP NetWeaver als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP NetWeaver muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in SAP NetWeaver zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SAP NetWeaver müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines SAP NetWeaver-Testbenutzers](#creating-an-sap-netweaver-test-user)**, um eine Entsprechung von Britta Simon in SAP NetWeaver zu erstellen, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer SAP NetWeaver-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SAP NetWeaver die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP NetWeaver** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

1. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für SAP NetWeaver** aus:

    ![Configure single sign-on](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<your company instance of SAP NetWeaver>`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<your company instance of SAP NetWeaver>`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Wenden Sie sich an das [Kundensupportteam von SAP NetWeaver](https://www.sap.com/support.html), um diese Werte zu erhalten. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/sap-netweaver-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **SAP NetWeaver-Konfiguration** auf **SAP NetWeaver konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** aus dem Abschnitt mit der **Kurzübersicht**.

    ![Configure single sign-on](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

1. Zum Konfigurieren der einmaligen Anmeldung auf der Seite von **SAP NetWeaver** müssen Sie die heruntergeladene **Metadaten-XML-Datei** und die **SAML-Entitäts-ID** an den [SAP NetWeaver-Support](https://www.sap.com/support.html) senden. 

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/sap-netweaver-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/sap-netweaver-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/sap-netweaver-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/sap-netweaver-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **Britta Simon** ein.

    b. Geben Sie im Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-an-sap-netweaver-test-user"></a>Erstellen eines SAP NetWeaver-Testbenutzers

In diesem Abschnitt erstellen Sie in SAP NetWeaver einen Benutzer namens Britta Simon. Wenden Sie sich an Ihren [SAP NetWeaver-Support](https://www.sap.com/support.html), um die Benutzer der SAP NetWeaver-Plattform hinzufügen zu lassen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf SAP NetWeaver gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon SAP NetWeaver zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste den Eintrag **SAP NetWeaver**aus.

    ![Configure single sign-on](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP NetWeaver“ klicken, sollten Sie automatisch bei Ihrer SAP NetWeaver-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/sap-netweaver-tutorial/tutorial_general_203.png

