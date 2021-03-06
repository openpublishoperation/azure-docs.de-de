---
title: Bereitstellen einer ASP.NET-App mit Azure SQL-Datenbank mithilfe des Azure DevOps-Projekts | Azure DevOps Services-Tutorial
description: Das DevOps-Projekt erleichtert die ersten Schritte mit Azure. Mit dem Azure DevOps-Projekt können Sie Ihre ASP.NET-App mit Azure SQL-Datenbank in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: d9c7c94e344daee5af87ce40ddf4dcb686696ded
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297332"
---
# <a name="tutorial--deploy-your-aspnet-app-and-azure-sql-database-with-the-azure-devops-project"></a>Tutorial: Bereitstellen einer ASP.NET-App mit Azure SQL-Datenbank mithilfe des Azure DevOps-Projekts

Das Azure DevOps-Projekt stellt eine vereinfachte Umgebung dar, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine der Beispielanwendungen wählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) zu Azure zu erstellen.  Mit dem DevOps-Projekt werden Azure-Ressourcen wie Azure SQL-Datenbanken automatisch erstellt, eine Releasepipeline in Azure DevOps mit einer Buildpipeline für CI erstellt und konfiguriert, eine Releasepipeline für CD eingerichtet und anschließend eine Azure Application Insights-Ressource für die Überwachung erstellt.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App mit Azure SQL-Datenbank
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnement 
> * Überprüfen der Azure DevOps Services-CI-Pipeline
> * Überprüfen der Azure DevOps Services-CD-Pipeline
> * Ausführen eines Commits für Änderungen an Azure DevOps Services und automatisches Bereitstellen in Azure
> * Herstellen einer Verbindung mit der Azure SQL Server-Datenbank 
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="create-an-azure-devops-project-for-an-aspnet-app-and-azure-sql-database"></a>Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App mit Azure SQL-Datenbank

Mit dem Azure DevOps-Projekt wird in Azure eine CI/CD-Pipeline erstellt.  Sie können eine **neue Azure DevOps Services**-Organisation erstellen oder eine **bestehende Organisation** verwenden.  Ferner werden mit dem Azure DevOps-Projekt **Azure-Ressourcen** wie Azure SQL-Datenbanken im **Azure-Abonnement** Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie in der linken Navigationsleiste das Symbol **Ressource erstellen** aus, und suchen Sie anschließend nach **DevOps-Projekt**.  Wählen Sie **Erstellen**.

    ![Starten von Continuous Delivery](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie **.NET** und dann **Weiter** aus.

1. Wählen Sie für **Anwendungsframework auswählen** die Option **ASP.NET** aus.

1. Wählen Sie **Datenbank hinzufügen** und anschließend **Weiter** aus.  

1. Das Anwendungsframework, das Sie im vorherigen Schritt ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst.  Klicken Sie auf **Weiter**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps Services und eines Azure-Abonnement

1. Erstellen Sie eine **neue Azure DevOps Services**-Organisation, oder verwenden Sie eine **bestehende Organisation**.  Wählen Sie einen **Namen** für das Azure DevOps-Projekt.  

1. Wählen Sie Ihr Azure-**Abonnement** aus.

1. Wählen Sie optional den Link **Ändern** aus, um weitere Azure-Konfigurationseinstellungen anzuzeigen und im Abschnitt **Anmeldeinformationen für Datenbankserver** den **Benutzernamen** anzugeben.  **Speichern** Sie den **Benutzernamen** für zukünftige Schritte in diesem Tutorial.
 
1. Wenn Sie den oben beschriebenen Schritt durchgeführt haben, verlassen Sie den Bereich für die Konfiguration von Azure und wählen **Fertig** aus.  Andernfalls wählen Sie einfach **Fertig** aus.

1. Dieser Prozess dauert einige Minuten.  Sobald der Prozess abgeschlossen ist, wird das **Azure DevOps-Projektdashboard** im Azure-Portal geladen.  Über **Alle Ressourcen** können Sie das **Azure DevOps-Projektdashboard** auch direkt im **Azure-Portal** laden.  Wählen Sie auf der rechten Seite des Dashboards **Durchsuchen**, um Ihre aktive Anwendung anzuzeigen.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Überprüfen der Azure DevOps Services-CI-Pipeline

Mit dem Azure DevOps-Projekt wird in Ihrer Azure DevOps Services-Organisation automatisch eine vollständige Azure-CI/CD-Pipeline erstellt.  Sie können die Pipeline untersuchen und anpassen.  Gehen Sie wie folgt vor, um sich mit der Azure DevOps Services-Buildpipeline vertraut zu machen.

1. Navigieren Sie zum **Azure DevOps-Projektdashboard**.

1. Wählen Sie **oben** im **Azure DevOps-Projektdashboard** **Pipelines erstellen** aus.  Dieser Link öffnet eine Browserregisterkarte und die Azure DevOps Services-Buildpipeline für Ihr neues Projekt.

1. Verschieben Sie den Cursor rechts neben die Buildpipeline neben das Feld **Status**. Wählen Sie die **Auslassungspunkte** aus, die angezeigt werden.  Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten durchführen können. So können Sie beispielsweise **einen neuen Build zur Warteschlange hinzufügen**, **einen Build anhalten** und **die Buildpipeline bearbeiten**.

1. Wählen Sie **Bearbeiten** aus.

1. In dieser Ansicht können Sie sich **die verschiedenen Aufgaben ansehen**, die Sie für Ihre Buildpipeline durchführen können.  Vom Build werden verschiedene Aufgaben durchgeführt. So werden beispielsweise Quellen aus dem Azure DevOps Services-Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den **Buildpipelinenamen**.

1. Ersetzen Sie den **Namen** Ihrer Buildpipeline durch einen aussagekräftigeren Namen.  Wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.  Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt.  An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps Services überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  Mit dem Azure DevOps-Projekt wurde automatisch ein CI-Trigger erstellt, und mit jedem Commit, der für das Repository ausgeführt wird, wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  Je nach Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Überprüfen der Azure DevOps Services-CD-Pipeline

Mit dem Azure DevOps-Projekt werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps Services-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert.  Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von Azure DevOps Services für Ihr Azure-Abonnement.  Bei der Automatisierung wird außerdem eine Azure DevOps Services-Releasedefinition erstellt. Zudem wird durch das Release die CD für Azure bereitgestellt.  Gehen Sie wie folgt vor, um mehr über die Azure DevOps Services-Releasedefinition zu erfahren.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  Mit dem Azure DevOps-Projekt wurde eine Azure DevOps Services-Releasepipeline zum Verwalten von Bereitstellungen für Azure erstellt.

1. Wählen Sie links im Browser die **Auslassungspunkte** neben Ihrer Releasepipeline und anschließend **Bearbeiten** aus.

1. Die Releasepipeline enthält eine **Pipeline**, die den Releaseprozess definiert.  Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie die Option **Continuous Deployment-Trigger** rechts neben dem **Symbol** **Ablegen** (das als Gewitterblitz dargestellt wird) aus.  Diese Releasepipeline verfügt über einen aktivierten CD-Trigger.  Der Trigger wird jedes Mal ausgelöst, wenn ein neues Buildartefakt verfügbar ist.  Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Das Azure DevOps-Projekt richtet ein zufälliges SQL-Passwort ein, und dieses Passwort wird für die Releasepipeline verwendet.  Wählen Sie links im Browser die Option **Variablen** aus. 

1. **Führen Sie diesen Schritt nur aus, wenn Sie das SQL Server-Kennwort geändert haben.**  Es gibt eine **Kennwortvariable**.  Wählen Sie rechts neben dem Textfeld **Wert** das **Vorhängeschloss**-Symbol aus.  **Geben Sie das neue Kennwort ein**, und klicken Sie anschließend auf **Speichern**.

1. Wählen Sie links im Browser **Aufgaben** und anschließend Ihre **Umgebung** aus.  

1. Bei den Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess durchgeführt werden. Sie sind in **Phasen** gruppiert.  Es gibt eine Phase für diese Releasepipeline.  Die Phase enthält die Aufgaben **Azure App Service-Bereitstellung** und **Azure SQL-Datenbankbereitstellung**.

1. Wählen Sie die Aufgabe **Azure SQL ausführen** aus, und sehen Sie sich die verschiedenen Eigenschaften an, die für die SQL-Bereitstellung verwendet werden.  Unter **Bereitstellungspaket** können Sie erkennen, dass bei der Aufgabe eine **SQL DACPAC-Datei** verwendet wird.

1. Wählen Sie rechts im Browser **Releases anzeigen** aus.  In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie die **Auslassungsp** neben einem Ihrer Releases und dann die Option **Öffnen** aus.  In dieser Ansicht finden Sie verschiedene Menüs, wie etwa eine **Releasezusammenfassung**, **zugeordnete Arbeitsaufgaben** und **Tests**.

1. Wählen Sie **Commits** aus.  In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. Sie können Releases vergleichen, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess.  Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Ausführen eines Commits für Änderungen an Azure DevOps Services und automatisches Bereitstellen in Azure 

 > [!NOTE]
 > Mit den folgenden Schritten wird die CI/CD-Pipeline mit einer einfachen Textänderung getestet.  Optional können Sie das SQL Server-Schema der Tabelle ändern, um den SQL-Bereitstellungsprozess zu testen.

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten.  Jede Änderung am Azure DevOps Services-Git-Repository startet einen Build in Azure DevOps Services, und eine Azure DevOps Services-CD-Pipeline führt eine Bereitstellung in Azure durch.  Gehen Sie wie folgt vor, um einen Commit für Änderungen an Ihrem Repository auszuführen, oder verwenden Sie hierfür andere Verfahren.  Mit den Codeänderungen wird der CI/CD-Prozess gestartet. Ferner werden die neuen Änderungen automatisch in Azure bereitgestellt.

1. Wählen Sie im Azure DevOps Services-Menü den Eintrag **Code** aus, und navigieren Sie zu Ihrem Repository.

1. Navigieren Sie zum Verzeichnis **SampleWebApplication\Views\Home**. Wählen Sie anschließend die **Auslassungspunkte** neben der Datei **Index.cshtml** und dann **Bearbeiten** aus.

1. Nehmen Sie eine Änderung an der Datei vor. Ändern Sie beispielsweise den Text in einem der **div-Tags**.  Wählen Sie oben rechts **Commit** aus.  Wählen Sie erneut **Commit** aus, um die Änderung mittels Push zu übertragen. 

1. Kurz danach wird **in Azure DevOps Services ein Build gestartet**. Anschließend wird zur Bereitstellung der Änderungen ein Release ausgeführt.  Sie können den **Buildstatus** mit dem DevOps-Projektdashboard oder im Browser mit Ihrer Azure DevOps Services-Organisation überwachen.

1. Nachdem das Release ausgeführt wurde, **aktualisieren Sie Ihre Anwendung** im Browser, um sicherzustellen, dass Ihre Änderungen angezeigt werden.

## <a name="connect-to-the-azure-sql-server-database"></a>Herstellen einer Verbindung mit der Azure SQL Server-Datenbank

Zum Herstellen einer Verbindung mit der Azure SQL-Datenbank benötigen Sie die entsprechenden Berechtigungen.

1. Wählen Sie im Azure DevOps-Projektdashboard die **SQL-Datenbank** aus, um zur Verwaltungsseite für die SQL-Datenbank zu navigieren.
   
1. Wählen Sie **Serverfirewall festlegen** und anschließend **+ Client-IP-Adresse hinzufügen** aus.  

1. Wählen Sie **Speichern**aus.  Danach können Sie über Ihre Client-IP-Adresse auf die **Azure SQL Server-Ressource** zugreifen.

1. Navigieren Sie zurück zum Blatt **SQL-Datenbank**. 

1. Wählen Sie rechts im Bildschirm die Option **Servername** aus, um zur Konfigurationsseite für **SQL Server** zu navigieren.

1. Wählen Sie **Kennwort zurücksetzen** aus, geben Sie für die **SQL Server-Administratoranmeldung** ein Kennwort ein, und wählen Sie anschließend **Speichern** aus.  **Speichern** Sie dieses Kennwort für zukünftige Schritte in diesem Tutorial.

1. Danach können Sie optional Clienttools wie **SQL Server Management Studio** oder **Visual Studio** verwenden, um eine Verbindung mit dem Azure SQL-Server und der -Datenbank herzustellen.  Verwenden Sie zum Herstellen der Verbindung die Eigenschaft **Servername**.

   Wenn Sie den DB-Benutzernamen bei der Erstkonfiguration des DevOps-Projekts nicht geändert haben, ist Ihr Benutzername der lokale Teil Ihrer E-Mail-Adresse.  Wenn Ihre E-Mail-Adresse beispielsweise johndoe@microsoft.com lautet, ist „johndoe“ Ihr Benutzername.

 > [!NOTE]
 > Wenn Sie Ihr Kennwort für die SQL-Anmeldung geändert haben, müssen Sie das Kennwort in der Variablen der Azure DevOps Services-Releasepipeline ändern. Eine entsprechende Anleitung finden Sie im Abschnitt **Untersuchen der Azure DevOps Services-CD-Pipeline**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

 > [!NOTE]
 > Mit den nachfolgenden Schritten werden Ressourcen dauerhaft gelöscht.  Verwenden Sie diese Funktion erst, nachdem Sie die Eingabeaufforderungen aufmerksam durchgelesen haben.

Beim Durchführen von Tests können Sie Ressourcen bereinigen, um zu verhindern, dass Gebühren entstehen.  Sie können die mit diesem Tutorial erstellte Azure SQL-Datenbank und die zugehörigen Ressourcen mithilfe der Funktion **Löschen** im Azure DevOps-Projektdashboard löschen, wenn Sie die Bereitstellung nicht mehr benötigen.  **Seien Sie vorsichtig**, denn mit der Löschfunktion werden die im Rahmen des Azure DevOps-Projekts erstellten Daten in Azure und Azure DevOps Services gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden.

1. Navigieren Sie im **Azure-Portal** zum **Azure DevOps-Projekt**.
2. Wählen Sie **oben rechts** im Dashboard die Option **Löschen** aus.  Nachdem Sie die Eingabeaufforderung gelesen haben, wählen Sie **Ja** aus, um die Ressourcen **dauerhaft zu löschen**.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasepipelines können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Projekte verwenden.  Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App mit Azure SQL-Datenbank
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnement 
> * Überprüfen der Azure DevOps Services-CI-Pipeline
> * Überprüfen der Azure DevOps Services-CD-Pipeline
> * Ausführen eines Commits für Änderungen an Azure DevOps Services und automatisches Bereitstellen in Azure
> * Herstellen einer Verbindung mit der Azure SQL Server-Datenbank 
> * Bereinigen von Ressourcen

Weitere Informationen zur Azure-Pipeline finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Anpassen von CD-Prozessen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Videos

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]