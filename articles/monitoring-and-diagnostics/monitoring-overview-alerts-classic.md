---
title: Überblick über klassische Warnungen in Microsoft Azure und Microsoft Azure Monitor
description: Klassische Warnungen sind veraltet. Mit Warnungen können Sie Metriken, Ereignisse oder Protokolle für Ihre Azure-Ressourcen überwachen. Lassen Sie sich benachrichtigen, wenn eine von Ihnen festgelegte Bedingung erfüllt ist.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 7046a0c6ac84ad5f156098a26dcef2b8accd50af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987644"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Was sind klassische Warnungen in Microsoft Azure?

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie ältere klassische Metrikwarnungen erstellen. Azure Monitor unterstützt nun [neuere Metrikwarnungen, die nahezu in Echtzeit stattfinden, und eine neue Oberfläche für Warnungen](monitoring-overview-alerts.md). 
>

Mit Warnungen können Sie Bedingungen für Daten konfigurieren und benachrichtigt werden, wenn die Bedingungen Übereinstimmungen mit den aktuellen Überwachungsdaten ergeben.

## <a name="old-and-new-alerting-capabilities"></a>Alte und neue Warnungsfunktionen

In der Vergangenheit verfügten Azure Monitor, Application Insights, Log Analytics und Service Health über separate Benachrichtigungsfunktionen. Im Laufe der Zeit verbesserte und kombinierte Azure sowohl die Benutzeroberfläche als auch verschiedene Benachrichtigungsmethoden. Die Konsolidierung ist noch nicht abgeschlossen. Alerts

Sie können klassische Warnungen nur auf dem Benutzerbildschirm für klassische Warnungen im Azure-Portal anzeigen. Sie erhalten diesen Bildschirm über die Schaltfläche **Klassische Warnungen anzeigen** auf dem Bildschirm „Warnungen“. 

 ![Warnungsoptionen im Azure-Portal](./media/monitoring-overview-alerts-classic/monitor-alert-screen2.png) 

Die neuere Benutzeroberfläche „Warnungen“ bietet gegenüber der Oberfläche für klassische Warnungen folgende Vorteile:
-   **Besseres Benachrichtigungssystem:** – Alle neuere Warnungen verwenden Aktionsgruppen. Hierbei handelt es sich um benannte Gruppen von Benachrichtigungen und Aktionen, die in mehreren Warnungen wiederverwendet werden können. Klassische Metrikwarnungen und ältere Log Analytics-Warnungen verwenden keine Aktionsgruppen.
-   **Einheitliche Oberfläche für die Erstellung** – Alle Funktionen zum Erstellen von Warnungen für Metriken, Protokolle und Aktivitätsprotokolle für Azure Monitor, Log Analytics und Application Insights befinden sich an einem Ort.
-   **Anzeige von ausgelösten Log Analytics-Warnungen im Azure-Portal** – Sie können nun auch ausgelöste Log Analytics-Warnungen in Ihrem Abonnement sehen. Zuvor wurden diese in einem separaten Portal angezeigt.
-   **Unterscheidung zwischen ausgelösten Warnungen und Warnungsregeln** – Warnungsregeln (der Definition der Bedingung, die eine Warnung auslöst) und ausgelöste Warnungen (einer Instanz zur Auslösung von Warnungsregeln) werden unterschieden, sodass die Betriebs- und Konfigurationsansichten getrennt sind.
-   **Verbesserter Workflow** – Die neue Oberfläche zum Erstellen von Warnungen führt den Benutzer durch die Konfiguration einer Warnungsregel, was die Auswahl der entsprechenden Warnungselemente vereinfacht.
-   **Konsolidierung von intelligenten Warnungen** und **Festlegen des Warnungsstatus** – Neuere Warnungen umfassen eine automatische Gruppierungsfunktion, die ähnliche Warnungen zusammen anzeigt, um die Überladung der Benutzeroberfläche zu vermeiden. 

Die neueren Metrikwarnungen haben folgende Vorteile gegenüber den klassischen Metrikwarnungen:
-   **Kürzere Wartezeit:** Neuere Metrikwarnungen können im Minutentakt ausgeführt werden. Ältere Metrikwarnungen werden immer nur alle fünf Minuten ausgeführt. Neuere Warnungen weisen eine immer geringere Verzögerung vom Auftreten des Problems bis zur Benachrichtigung oder Aktion auf (drei bis fünf Minuten). Ältere Warnungen dauern je nach Typ 5 bis 15 Minuten.  Protokollwarnungen verzögern sich aufgrund der Zeit, die bis zur Aufnahme der Protokolle vergeht, in der Regel um 10 bis 15 Minuten, aber neuere Verarbeitungsmethoden reduzieren diese Zeit. 
-   **Unterstützung für mehrdimensionale Metriken**: Es können für dimensionale Metriken Warnungen ausgegeben werden, sodass Sie ein interessantes Segment der Metrik überwachen können.
-   **Mehr Kontrolle über Metrikbedingungen:** Sie können umfangreichere Warnungsregeln definieren. Die neueren Warnungen unterstützen die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
-   **Kombinierte Überwachung mehrerer Metriken:** Sie können mehrere Metriken (aktuell bis zu zwei) mit einer einzelnen Regel überwachen. Eine Warnung wird ausgelöst, wenn beide Metriken ihren jeweiligen Schwellenwert für den angegebenen Zeitraum überschreiten.
-   **Besseres Benachrichtigungssystem:** Alle neuere Warnungen verwenden [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md). Hierbei handelt es sich um benannte Gruppen von Benachrichtigungen und Aktionen, die in mehreren Warnungen wiederverwendet werden können.  Klassische Metrikwarnungen und ältere Log Analytics-Warnungen verwenden keine Aktionsgruppen. 
-   **Metriken aus Protokollen** (Public Preview): Bei Log Analytics eingehende Protokolldaten können jetzt extrahiert und in Azure Monitor-Metriken konvertiert werden. Dadurch können sie genau wie andere Metriken in Warnungen genutzt werden. Informationen zur spezifischen Terminologie für klassische Warnungen finden Sie unter [Warnungen (klassisch)](monitoring-overview-alerts-classic.md). 


## <a name="classic-alerts-on-azure-monitor-data"></a>Klassische Warnungen zu Azure Monitor-Daten
Es sind zwei Arten von klassischen Warnungen verfügbar – Metrikwarnungen und Aktivitätsprotokollwarnungen.

* **Klassische Metrikwarnungen**: Diese Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert überschreitet. Die Warnung generiert eine Benachrichtigung, wenn die Warnung „aktiviert“ ist (wenn der Schwellenwert überschritten wird und die Warnungsbedingung erfüllt ist). Es wird eine weitere Benachrichtigung generiert, wenn die Warnung „gelöst“ ist (wenn der Schwellenwert erneut überschritten wird und die Warnungsbedingung nicht mehr erfüllt ist).

* **Klassische Aktivitätsprotokollwarnungen**: Eine Streamingprotokollwarnung, die ausgelöst wird, wenn ein Aktivitätsprotokollereignis generiert wird, das von Ihnen zugewiesene Filterkriterien erfüllt. Diese Warnungen haben nur einen Status: „Aktiviert“. Dies liegt daran, dass die Warnungs-Engine die Filterkriterien einfach auf ein neues Ereignis anwendet. Diese Warnungen können zum Empfangen von Benachrichtigungen für den Fall verwendet werden, dass ein neuer Service Health-Vorfall eintritt oder ein Benutzer oder eine Anwendung in Ihrem Abonnement einen Vorgang durchführt, z.B. „Virtuellen Computer löschen“.

Für Diagnoseprotokolldaten, die über Azure Monitor verfügbar sind, leiten Sie die Daten zu Log Analytics (ehemals OMS) um und verwenden dann eine Log Analytics-Abfragewarnung. Log Analytics verwendet jetzt die [neue Warnmethode](monitoring-overview-unified-alerts.md) 

Das folgende Diagramm enthält eine Zusammenfassung der Quellen von Daten in Azure Monitor und des Konzepts zum Erzeugen von Warnungen aus diesen Daten.

![Warnungen im Überblick](./media/monitoring-overview-alerts-classic/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Taxonomie von Warnungen (klassisch)
In Azure werden die folgenden Begriffe verwendet, um klassische Warnungen und ihre Funktionen zu beschreiben:
* **Warnung**: Eine Definition mit Kriterien (mindestens eine Regel oder Bedingung), die aktiviert wird, wenn die Kriterien erfüllt sind.
* **Aktiv**: Der Status, wenn die für eine klassische Warnung definierten Kriterien erfüllt sind.
* **Gelöst**: Der Status, wenn die für eine klassische Warnung definierten Kriterien nicht mehr erfüllt sind, nachdem dies zuvor der Fall war.
* **Benachrichtigung**: Die Aktion, die durchgeführt wird, wenn eine klassische Warnung aktiv wird.
* **Aktion**: Ein bestimmte Kontaktaufnahme, die an den Empfänger einer Benachrichtigung gesendet wird (z.B. E-Mail an eine Adresse oder das Posten an eine Webhook-URL). Benachrichtigungen können in der Regel mehrere Aktionen auslösen.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Wie erhalte ich eine Benachrichtigung von einer klassischen Azure Monitor-Warnung?
Bisher wurden für Azure-Warnungen unterschiedlicher Dienste die eigenen integrierten Benachrichtigungsmethoden verwendet. 

Azure Monitor hat eine wiederverwendbare Benachrichtigungsgruppierung mit der Bezeichnung *Aktionsgruppen* erstellt. Mit Aktionsgruppen wird eine Reihe von Empfängern für eine Benachrichtigung angegeben, und Jedes Mal, wenn eine Warnung aktiviert wird, die auf die Aktionsgruppe verweist, erhalten alle Empfänger eine Benachrichtigung. Aktionsgruppen ermöglichen es, eine Gruppierung von Empfängern (z. B. Ihre Liste mit Technikern in Bereitschaft) für viele Warnungsobjekte wiederzuverwenden. Für die Aktionsgruppen werden Benachrichtigungen unterstützt, indem zusätzlich zu E-Mail-Adressen, SMS-Telefonnummern und einer Reihe anderer Aktionen eine Webhook-URL bereitgestellt wird.  Weitere Informationen finden Sie unter [Aktionsgruppen](monitoring-action-groups.md). 

Ältere klassische Aktivitätsprotokollwarnungen verwenden Aktionsgruppen.

Für die älteren Metrikwarnungen werden jedoch keine Aktionsgruppen verwendet. Sie können stattdessen die folgenden Aktionen konfigurieren: 
- Senden von E-Mail-Benachrichtigungen an den Dienstadministrator, an Co-Administratoren oder an zusätzliche von Ihnen festgelegte E-Mail-Adressen
- Aufrufen eines Webhooks, um zusätzliche automatisierte Aktionen auszuführen

Webhooks ermöglichen die Automatisierung und Fehlerbehebung, z.B. mit folgenden Komponenten:
    - Azure Automation-Runbook
    - Azure Function
    - Azure-Logik-App
    - ein Drittanbieter-Dienst

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über Warnregeln und ihre Konfiguration erhalten Sie hier:

* Weitere Informationen zu [Metriken](monitoring-overview-metrics.md)
* Konfigurieren von [klassischen Metrikwarnungen über das Azure-Portal](insights-alerts-portal.md)
* Konfigurieren von [klassischen Metrikwarnungen mit PowerShell](insights-alerts-powershell.md)
* Konfigurieren von [klassischen Metrikwarnungen über die Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
* Konfigurieren von [klassischen Metrikwarnungen mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Weitere Informationen zu [Aktivitätsprotokollen](monitoring-overview-activity-logs.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über das Azure-Portal](monitoring-activity-log-alerts.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über den Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
* Erfahren Sie mehr über [Aktionsgruppen](monitoring-action-groups.md).
* Konfigurieren [neuerer Warnungen](monitor-alerts-unified-usage.md)