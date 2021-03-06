---
title: Verwaltbarkeit und Überwachung von Azure SQL Data Warehouse – Abfrageaktivität, Ressourcennutzung | Microsoft-Dokumentation
description: Erfahren Sie, welche Funktionen zum Verwalten von Überwachen von Azure SQL Data Warehouse zur Verfügung stehen. Verwenden Sie das Azure-Portal und dynamische Verwaltungssichten (DMVs), um Informationen zu Abfrageaktivität und Ressourcennutzung für Ihre Data Warehouse-Instanz zu erhalten.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/26/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: c783045d242725ee19dfe7e0baee13625d986312
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246493"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Überwachen von Ressourcennutzung und Abfrageaktivität in Azure SQL Data Warehouse
Azure SQL Data Warehouse bietet umfassende Überwachungsfunktionen im Azure-Portal, um Erkenntnisse zu Ihrer Data Warehouse-Workload zu gewinnen. Das Azure-Portal ist das empfohlene Tool zum Überwachen Ihrer Data Warehouse-Instanz, weil es eine konfigurierbare Aufbewahrungsdauer, Warnungen, Empfehlungen und anpassbare Diagramme und Dashboards für Metriken und Protokolle bietet. Das Portal ermöglicht außerdem eine Integration weiterer Azure-Überwachungsdienste – z.B. Operations Management Suite (OMS)/Log Analytics und Azure Monitor –, um Ihnen eine umfassende und integrierte Überwachungsoberfläche für Data Warehouse sowie für Ihre gesamte Azure-Analyseplattform zu bieten. In dieser Dokumentation wird beschrieben, welche Überwachungsfunktionen zur Verfügung stehen, um Ihre Analyseplattform mit SQL Data Warehouse zu optimieren und zu verwalten. 

## <a name="resource-utilization"></a>Ressourcennutzung 
Im Azure-Portal stehen die folgenden Metriken zur Verfügung.

| Metrikname                           | Beschreibung     | Aggregationstyp |
| --------------------------------------- | ---------------- | --------------------------------------- |
| CPU-Prozentsatz                          | CPU-Auslastung für alle Knoten der Data Warehouse-Instanz | Maximum      |
| E/A-Prozentsatz für Daten                      | E/A-Auslastung für alle Knoten der Data Warehouse-Instanz | Maximum   |
| Erfolgreiche Verbindungen                  | Anzahl von erfolgreichen Datenverbindungen | Gesamt            |
| Verbindungsfehler                      | Anzahl von Fehlern bei der Verbindungsherstellung mit Data Warehouse | Gesamt            |
| Von der Firewall blockiert                     | Anzahl von blockierten Anmeldungen bei Data Warehouse | Gesamt            |
| DWU-Grenzwert                              | Servicelevelziel für Data Warehouse | Maximum   |
| DWU in Prozent                          | Höchstwert zwischen CPU-Prozentsatz und E/A-Prozentsatz für Daten | Maximum   |
| DWU-Verbrauch                                | DWU-Limit × DWU-Prozentsatz | Maximum   |
| Prozentsatz der Cachetreffer | (Cachetreffer/Cachefehler) × 100, wobei die Cachetreffer die Summe aller Columnstore-Segmenttreffer im lokalen SSD-Cache und die Cachefehler die Columnstore-Segmentfehler im lokalen SSD-Cache repräsentieren, summiert über alle Knoten | Maximum |
| Cacheverwendung in Prozent | (Cacheverwendung/Cachekapazität) × 100, wobei die Cacheverwendung die Summe aller Bytes im lokalen SSD-Cache für alle Knoten darstellt und die Cachekapazität die Summe der Speicherkapazität im lokalen SSD-Cache für alle Knoten repräsentiert | Maximum |

## <a name="query-activity"></a>Abfrageaktivität
Für eine programmgesteuerte Benutzeroberfläche bei der Überwachung von SQL Data Warehouse über T-SQL bietet der Dienst einen Satz an dynamischen Verwaltungssichten (DMVs). Diese Sichten sind nützlich für die aktive Problembehandlung und das Identifizieren von Leistungsengpässen in Ihrer Workload.

Eine Liste der von SQL Data Warehouse bereitgestellten DMVs finden Sie in dieser [Dokumentation](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Protokollierung von Metriken und Diagnosedaten
Sie können sowohl Metriken als auch Protokolle in die [Operations Management Suite](https://azure.microsoft.com/resources/videos/operations-management-suite-oms-overview/) (OMS) exportieren, insbesondere in die [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)-Komponente, und programmgesteuert über die [Protokollsuche](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata) auf die Daten zugreifen.


> [!NOTE]
> Seit August 2018 werden Protokolle für SQL Data Warehouse implementiert.

## <a name="next-steps"></a>Nächste Schritte
Die folgenden Anleitungen beschreiben gängige Szenarios und Anwendungsfälle bei der Überwachung und Verwalten Ihrer Data Warehouse-Instanz:

- [Überwachen Ihrer Data Warehouse-Workload mit DMVs](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

