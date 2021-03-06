---
title: Verwalten von Warnungsinstanzen
description: Verwalten von Warnungsinstanzen in Azure
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 614e65ca791559f4649963ee82940c3f96beddb3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948450"
---
# <a name="manage-alert-instances"></a>Verwalten von Warnungsinstanzen
Mit der [einheitlichen Oberfläche für Warnungen](https://aka.ms/azure-alerts-overview) in Azure Monitor können Sie jetzt Ihre gesamten verschiedenen Warnungstypen in Azure, die mehrere Abonnements umfassen, in einem einzelnen Bereich anzeigen. Dieser Artikel führt Sie durch die Möglichkeiten, wie Sie Ihre Warnungsinstanzen anzeigen und wie Sie im Portal intensiv nach bestimmten Warnungsinstanzen zur Fehlerbehebung suchen können.

1. Es gibt drei Möglichkeiten, um auf die Seite „Warnungen“ zu gelangen

   + Wählen Sie im [Portal](https://portal.azure.com/) die Option **Überwachen** und im Abschnitt „Überwachen“ dann **Warnungen** aus.  
    ![Überwachung](./media/monitoring-alerts-managing-alerts-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
   + Sie können aus dem Kontext einer bestimmten **Ressource** heraus zu „Warnungen“ navigieren. Sobald eine Ressource geöffnet ist, navigieren Sie durch ihr Inhaltsverzeichnis zum Überwachungsabschnitt, und wählen Sie **Warnungen** aus, wobei die Zielseite für Warnungen zu dieser spezifischen Ressource vorgefiltert ist.
   
    ![Überwachung](./media/monitoring-alerts-managing-alerts-instances/alert-resource.JPG)
    
   + Sie können aus dem Kontext einer bestimmten **Ressourcengruppe** heraus zu „Warnungen“ navigieren. Sobald eine Ressourcengruppe geöffnet ist, navigieren Sie durch ihr Inhaltsverzeichnis zum Überwachungsabschnitt, und wählen Sie **Warnungen** aus, wobei die Zielseite für Warnungen zu dieser spezifischen Ressourcengruppe vorgefiltert ist.    
   
    ![Überwachung](./media/monitoring-alerts-managing-alerts-instances/alert-rg.JPG)

1.  Sie gelangen auf die Seite **Zusammenfassung von Warnungen**, die Ihnen einen Überblick über Ihre gesamten Warnungsinstanzen in Azure gibt. Sie können die zusammenfassende Übersicht ändern, indem Sie **mehrere Abonnements** (maximal fünf) auswählen oder nach **Ressourcengruppen**, bestimmten **Ressourcen** oder **Zeitbereichen** filtern. Klicken Sie entweder auf „Warnungen gesamt“ oder einen der Schweregrade, um in die Listenansicht für Ihre Warnungen zu gelangen.     
    ![Zusammenfassung von Warnungen](./media/monitoring-alerts-managing-alerts-instances/alerts-summary.jpg)
 
1.  Sie gelangen auf die Seite **Alle Warnungen**, auf der alle Warnungsinstanzen in Azure aufgelistet werden. Wenn Sie über eine Warnungsbenachrichtigung zum Portal gelangen, können Sie die verfügbaren Filter verwenden, um die Eingrenzung auf diese bestimmte Warnungsinstanz vorzunehmen. (**Hinweis**: Wenn Sie durch Klicken auf einen der Schweregrade auf die Seite gelangt sind, wird die Liste vorab nach diesem Schweregrad gefiltert). Neben den auf der vorherigen Seite verfügbaren Filtern können Sie jetzt auch auf Basis des Überwachungsdiensts (z. B. Plattform für Metriken), der Überwachungsbedingung (ausgelöst oder gelöst), des Schweregrads, des Warnungszustands (neu/bestätigt/geschlossen) oder der ID der intelligenten Gruppe filtern.

    ![Alle Warnungen](./media/monitoring-alerts-managing-alerts-instances/all-alerts.jpg)

    > [!NOTE]
    >  Wenn Sie durch Klicken auf einen der Schweregrade auf die Seite gelangt sind, wird die Liste vorab nach diesem Schweregrad gefiltert.
 
1.  Wenn Sie auf eine beliebige Warnungsinstanz klicken, wird die Seite **Warnungsdetails** geöffnet, auf der Sie die Informationen zu dieser bestimmten Warnungsinstanz eingehend untersuchen können.   
![Warnungsdetails](./media/monitoring-alerts-managing-alerts-instances/alert-details.jpg)  
