---
title: Latenzen bei Azure Active Directory-Berichten | Microsoft-Dokumentation
description: Erfahren Sie etwas über den erforderliche Zeitraum, bis Ereignisse in Ihrem Azure-Portal angezeigt werden.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 12/15/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b81c66acc0a90ba9b74cf1f4fb34ef7a545837f9
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736605"
---
# <a name="azure-active-directory-reporting-latencies"></a>Latenzen bei Azure Active Directory-Berichten

Mit der [Berichterstellungsfunktion](../active-directory-preview-explainer.md) in Azure Active Directory erhalten Sie alle Informationen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen. Die Dauer bis zur Anzeige von Berichtsdaten im Azure-Portal wird auch als Latenz bezeichnet. 

Dieses Thema enthält Informationen zur Latenz für alle Berichtskategorien im Azure-Portal. 


## <a name="activity-reports"></a>Aktivitätsberichte

Es gibt zwei Bereiche von Aktivitätsberichten:

- **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
- **Überwachungsprotokolle** : Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung, zu verwalteten Anwendungen und Verzeichnisaktivitäten

Die folgende Tabelle enthält Latenzzeitinformationen für Aktivitätsberichte.

| Bericht | Wartezeit (95 %) |Wartezeit (99 %)|
| :-- | --- | --- | 
| Überwachungsprotokolle | 2 Min.  | 5 Min.  |
| Anmeldungen | 2 Min.  | 5 Min. |


## <a name="security-reports"></a>Sicherheitsberichte

Es gibt zwei Bereiche von Sicherheitsberichten:

- **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. 
- **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. 

Die folgende Tabelle enthält Latenzzeitinformationen für Sicherheitsberichte.

| Bericht | Minimum | Durchschnitt | Maximum |
| :-- | --- | --- | --- |
| Gefährdete Benutzer          | 5 Minuten   | 15 Minuten  | 2 Stunden  |
| Riskante Anmeldungen         | 5 Minuten   | 15 Minuten  | 2 Stunden  |

## <a name="risk-events"></a>Risikoereignisse

Azure Active Directory verwendet adaptive Machine Learning-Algorithmen und -Heuristiken, um verdächtige Aktivitäten im Zusammenhang mit Ihren Benutzerkonten zu erkennen. Jede erkannte verdächtige Aktion wird in einem Datensatz gespeichert, der als Risikoereignis bezeichnet wird.

Die folgende Tabelle enthält Latenzzeitinformationen für Risikoereignisse.

| Bericht | Minimum | Durchschnitt | Maximum |
| :-- | --- | --- | --- |
| Anmeldungen von anonymen IP-Adressen |5 Minuten |15 Minuten |2 Stunden |
| Anmeldungen von unbekannten Standorten |5 Minuten |15 Minuten |2 Stunden |
| Benutzer mit kompromittierten Anmeldeinformationen |2 Stunden |4 Stunden |8 Stunden |
| Unmöglicher Ortswechsel zu atypischen Orten |5 Minuten |1 Stunde |8 Stunden  |
| Anmeldungen von infizierten Geräten |2 Stunden |4 Stunden |8 Stunden  |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |2 Stunden |4 Stunden |8 Stunden  |



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Aktivitätsberichten im Azure-Portal finden Sie unter:

- [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](concept-sign-ins.md)
- [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](concept-audit-logs.md)

Weitere Informationen zu Sicherheitsberichten im Azure-Portal finden Sie unter:

- [Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal](concept-user-at-risk.md)
- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](concept-risky-sign-ins.md)

Weitere Informationen zu Risikoereignissen finden Sie unter [Azure Active Directory-Risikoereignisse](concept-risk-events.md).
