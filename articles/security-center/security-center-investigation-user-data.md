---
title: Verwalten von Benutzerdaten, die in einer Untersuchung des Azure Security Center gefunden wurden | Microsoft-Dokumentation
description: " Informationen zum Verwalten von Benutzerdaten, die mit dem Untersuchungsfeature in Azure Security Center gefunden wurden. "
services: operations-management-suite
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: operations-management-suite
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: d3b5f0ee4aa656fa66d577aa40f66fc98dff148d
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301649"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Verwalten von Benutzerdaten, die in einer Untersuchung des Azure Security Center gefunden wurden
Dieser Artikel enthält Informationen zum Verwalten von Benutzerdaten, die mit dem Untersuchungsfeature in Azure Security Center gefunden wurden. Die Untersuchungsdaten werden in [Azure Log Analytics](../log-analytics/log-analytics-overview.md) gespeichert und im Security Center verfügbar gemacht. Bei der Verwaltung der Benutzerdaten haben Sie auch die Möglichkeit, Daten zu löschen oder zu exportieren.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Suchen nach und Identifizieren von personenbezogenen Daten
Im Azure-Portal können Sie das [Untersuchungsfeature](../security-center/security-center-investigation.md) des Security Center nutzen, um nach personenbezogenen Daten zu suchen. Das Untersuchungsfeature ist unter **Sicherheitswarnungen** verfügbar.

Das Untersuchungsfeature zeigt alle Entitäten, Benutzerinformationen und Daten unter der Registerkarte **Entitäten** an.

## <a name="securing-and-controlling-access-to-personal-information"></a>Sichern und Steuern des Zugriffs auf personenbezogene Informationen
Ein Security Center-Benutzer, dem die Rolle „Leser“, „Besitzer“, „Mitwirkender“ oder „Kontoadministrator“ zugewiesen wird, kann mit dem Tool auf die Kundendaten zugreifen.

Weitere Informationen zu den Rollen „Leser“, „Besitzer“ und „Mitwirkender“ finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/built-in-roles.md). Weitere Informationen zur Rolle „Kontoadministrator“ finden Sie unter [Azure-Abonnementadministratoren](../billing/billing-add-change-azure-subscription-administrator.md).

## <a name="deleting-personal-data"></a>Löschen von personenbezogenen Daten
Ein Security Center-Benutzer, der die Rolle des Besitzers, Mitwirkenden oder Kontoadministrators zugewiesen bekommt, kann die Untersuchungsinformationen löschen.

Um eine Untersuchung zu löschen, können Sie eine `DELETE`-Anforderung an die REST-API des Azure Resource Manager senden:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

Die `incidentName`-Eingabe finden Sie durch die Auflistung aller Vorfälle mithilfe einer `GET`-Anforderung:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Exportieren von personenbezogenen Daten
Ein Security Center-Benutzer, der die Rolle des Besitzers, Mitwirkenden oder Kontoadministrators zugewiesen bekommt, kann die Untersuchungsinformationen exportieren. Wechseln Sie zur Registerkarte **Entitäten**, um Untersuchungsinformationen zu exportieren. Kopieren Sie die relevanten Informationen, und fügen Sie sie ein.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwalten von Benutzerdaten finden Sie unter [Manage user data in Azure Security Center (Verwalten von Benutzerdaten in Azure Security Center)](security-center-privacy.md).
