---
title: Hinzufügen von Azure Storage mithilfe von verbundenen Diensten in Visual Studio | Microsoft Docs
description: Fügen Sie Ihrer App mithilfe des Dialogfelds "Verbundene Dienste hinzufügen" in Visual Studio Azure Storage hinzu
services: visual-studio-online
author: ghogen
manager: douge
assetId: 521ec044-ad4b-4828-8864-01decde2e758
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/26/2017
ms.author: ghogen
ms.openlocfilehash: 73b9a1725ada07835db518b57140a2e4d43e143a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42142880"
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Hinzufügen von Azure-Speicher mithilfe von verbundenen Visual Studio-Diensten
Mit Visual Studio können Sie über das Dialogfeld **Verbundene Dienste hinzufügen** eine Verbindung zwischen jedem der folgenden Elemente und Azure Storage herstellen:

- C#-Clouddienst
- Mobiler .NET-Back-End-Dienst
- ASP-NET-Website oder -Dienst
- ASP.NET Core-Dienst
- Azure WebJob-Dienst 

Mit der Funktion für verbundene Dienste werden die benötigten Verweise und der Verbindungscode zu Ihrem Projekt hinzugefügt und Ihre Konfigurationsdateien entsprechend geändert. 

Nach Abschluss zeigt das Dialogfeld **Verbundene Dienste hinzufügen** automatisch die ausführlichen Schritte an, die erforderlich sind, um mit der Arbeit mit Blob Storage, Warteschlangen und Tabellen zu beginnen.

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Herstellen einer Verbindung mit Azure Storage mithilfe des Dialogfelds "Verbundene Dienste"
1. Öffnen Sie Ihr Projekt in Visual Studio.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Knoten **Verbundene Dienste**, und wählen Sie im Kontextmenü die Option **Verbundene Dienste hinzufügen** aus.
   
    ![Verbundenen Azure-Dienst hinzufügen](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. Wählen Sie auf der Seite **Verbundene Dienste** die Option **Cloudspeicher mit Azure Storage** aus.
   
    ![Azure Storage hinzufügen](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. Wählen Sie im Dialogfeld **Azure Storage** ein vorhandenes Speicherkonto aus, und klicken Sie dann auf **Hinzufügen**.
   
    Wenn Sie ein neues Speicherkonto erstellen müssen, fahren Sie mit dem nächsten Schritt fort. Fahren Sie andernfalls mit Schritt 6 fort.
    
    ![Vorhandenes Speicherkonto zum Projekt hinzufügen](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. So erstellen Sie ein Speicherkonto: 
   
   1. Wählen Sie am unteren Rand des Dialogfelds die Option **Neues Speicherkonto erstellen** aus.

   1. Füllen Sie die erforderlichen Optionen im Dialogfeld **Speicherkonto erstellen** aus, und klicken Sie auf **Erstellen**.
      
       ![Neues Azure-Speicherkonto](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Wenn das Dialogfeld **Azure Storage** erneut geöffnet wird, wird das neue Speicherkonto in der Liste angezeigt. Wählen Sie das neue Speicherkonto in der Liste aus, und klicken Sie auf **Hinzufügen**.

1. Der verbundene Speicherdienst wird unter dem Knoten **Dienstverweise** Ihres Projekts angezeigt.
   
## <a name="how-your-project-is-modified"></a>Änderungen am Projekt
Wenn Sie die Bearbeitung des Dialogfelds abschließen, fügt Visual Studio Verweise hinzu und ändert bestimmte Konfigurationsdateien. Die genauen Änderungen hängen vom Projekttyp ab: 

- ASP.NET-Projekt: [Was ist passiert – ASP.NET-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- ASP.NET Core-Projekt: [Was ist passiert – ASP.NET 5-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Clouddienstprojekt (Web- und Workerrollen): [Was ist passiert – Clouddienstprojekte](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- WebJob-Projekt: [Was ist passiert – WebJob-Projekte](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Nächste Schritte
- [MSDN Forum: Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure Storage-Dokumentation](https://docs.microsoft.com/azure/storage/)
