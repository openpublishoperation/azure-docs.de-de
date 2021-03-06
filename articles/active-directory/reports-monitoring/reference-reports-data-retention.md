---
title: Aufbewahrungsrichtlinien für Azure Active Directory-Berichte | Microsoft Docs
description: Aufbewahrungsrichtlinien für Berichtdaten in Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 68028fd1ba116251860e5c370e9e9ce61fd314bb
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106695"
---
# <a name="azure-active-directory-report-retention-policies"></a>Aufbewahrungsrichtlinien für Azure Active Directory-Berichte


Dieser Artikel enthält Antworten auf die am häufigsten gestellten Fragen zur Datenaufbewahrung für die verschiedenen Aktivitätsberichte in Azure Active Directory. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>F: Wie wird die Erfassung von Aktivitätsdaten gestartet?

**A:**

| Azure AD-Edition | Start der Erfassung |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Beim Registrieren für ein Abonnement |
| Azure AD Free | Beim ersten Öffnen des Blatts [Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oder bei der ersten Verwendung der [Berichterstellungs-APIs](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>F: Wann sind die Aktivitätsdaten im Azure-Portal verfügbar?

**A:**

- **Sofort**, wenn Sie bereits Berichte im Azure-Portal verwendet haben.
- **Innerhalb von 2 Stunden**, wenn Sie die Berichterstellung im Azure-Portal nicht aktiviert haben.

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>F: Wie wird die Erfassung von Sicherheitssignalen gestartet?  

**A:** Bei Sicherheitssignalen wird der Erfassungsprozess gestartet, wenn Sie sich für die Verwendung von Identity Protection Center entscheiden. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>F: Wie lange werden die gesammelten Daten gespeichert?

**A:**

**Aktivitätsberichte**    

| Bericht                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Verzeichnisprüfbericht        | 7 Tage        | 30 Tage             | 30 Tage             |
| Benutzeranmeldeaktivität       | N/V           | 30 Tage             | 30 Tage             |
| Azure MFA-Nutzung        | 30 Tage       | 30 Tage             | 30 Tage             |

**Sicherheitssignale**

| Bericht         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Gefährdete Benutzer  | 7 Tage        | 30 Tage             | 90 Tage             |
| Riskante Anmeldungen | 7 Tage        | 30 Tage             | 90 Tage             |

---
