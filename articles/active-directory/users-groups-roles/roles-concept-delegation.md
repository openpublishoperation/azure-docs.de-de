---
title: Delegieren von Administratorrollen in Azure Active Directory | Microsoft-Dokumentation
description: Delegierungsmodelle, Beispiele und Rollensicherheit in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/09/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: ad772a2e0c036c30aca2918496ac01f31157fc64
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "40209201"
---
# <a name="delegate-administration-in-azure-active-directory"></a>Delegieren der Administration in Azure Active Directory (Azure AD)

Das Wachstum einer Organisation bringt weitere Komplexität mit sich. Ein häufiger Ansatz besteht in der Reduzierung des Aufwands bei der Zugriffsverwaltung mit Azure Active Directory-Administratorrollen. Sie können Benutzern die geringstmögliche Berechtigung zuweisen, damit diese auf ihre Apps zugreifen und ihre Aufgaben durchführen können. Sie sollten die globale Administratorrolle nicht jedem Besitzer von Anwendungen zuweisen. Der Nachteil dabei ist jedoch, dass Sie die Aufgaben für die Anwendungsverwaltung dem vorhandenen globalen Administrator zuweisen müssen. Es gibt viele Gründe dafür, die Organisation auf eine dezentralisierte Verwaltung umzustellen. Durch den Inhalt dieses Artikels können Sie die Delegierung in Ihrer Organisation besser planen.

<!--What about reporting? Who has which role and how do I audit?-->


## <a name="centralized-versus-delegated-permissions"></a>Zentralisierte und delegierte Berechtigungen im Vergleich

Wenn eine Organisation wächst, kann es sich schwierig gestalten, den Überblick darüber zu behalten, welche Benutzer bestimmte Administratorrollen besitzen. Eine Organisation kann für Sicherheitsverletzungen anfällig werden, wenn ein Mitarbeiter Administratorrechte besitzt, die er nicht besitzen sollte. Im Allgemeinen hängt die Anzahl der Administratoren und die Granularität der Berechtigungen von der Größe und Komplexität der Bereitstellung ab.

* In kleinen oder Proof-of-Concept-Bereitstellungen wird die Arbeit von einem oder wenigen Administratoren erledigt, sodass keine Delegierung nötig ist. Erstellen Sie in diesem Fall jeden Administrator mit der globalen Administratorrolle.
* In größeren Bereitstellung mit mehr Computern, Anwendungen und Desktops ist eine Delegierung erforderlicher. In diesem Fall haben möglicherweise mehrere Administratoren bestimmte funktionale Aufgaben (Rollen). Einige können beispielsweise Privileged Identity-Administratoren und andere Anwendungsadministratoren sein. Ein Administrator kann zusätzlich auch nur bestimmte Objektgruppen (z.B. Geräte) verwalten.
* Noch größere Bereitstellungen erfordern möglicherweise noch präzisere Berechtigungen und Administratoren mit ungewöhnlichen oder hybriden Rollen.

Im Azure AD-Portal können Sie [alle Mitglieder einer Rolle anzeigen](directory-manage-roles-portal.md) und dadurch die Bereitstellung schnell überprüfen und Berechtigungen delegieren.

Wenn Sie den Zugriff auf Azure-Ressourcen, nicht aber den Administratorzugriff in Azure AD delegieren möchten, finden Sie weitere Informationen unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](../../role-based-access-control/role-assignments-portal.md).

## <a name="delegation-planning"></a>Planen der Delegierung

Die Herausforderung besteht zwar darin, ein Delegierungsmodell zu entwickeln, das den speziellen Anforderungen Ihrer Organisation entspricht, es gibt jedoch sehr einfache Modelle, die mit geringfügigen Änderungen angewendet werden können. Das Entwickeln eines Delegierungsmodells ist ein iterativer Entwurfsprozess. Es wird empfohlen, folgende Schritte durchzuführen:

* Definieren Sie die benötigten Rollen.
* Delegieren Sie die Verwaltung der App.
* Erteilen Sie die Berechtigung, Anwendungen zu registrieren.
* Delegieren Sie den Besitz der App.
* Entwickeln Sie einen Sicherheitsplan.
* Erstellen Sie Notfallkonten.
* Schützen Sie Ihre Administratorrollen.
* Legen Sie die privilegierte Erhöhung der Rechte auf temporär fest.

## <a name="define-roles"></a>Definieren von Rollen

Bestimmen Sie, welche Active Directory-Aufgaben von Administratoren durchgeführt werden und wie diese Rollen zugeordnet werden. Die Erstellung von Active Directory-Standorten ist beispielsweise eine Aufgabe für Dienstadministratoren, während die Änderung der Mitgliedschaft in Sicherheitsgruppen üblicherweise von Datenadministratoren durchgeführt wird. Sie können [detaillierte Rollenbeschreibungen](directory-manage-roles-portal.md) im Azure-Portal anzeigen.

Jede Aufgabe sollte nach Häufigkeit, Wichtigkeit und Schwierigkeit bewertet werden. Diese Aspekte sind bei der Aufgabendefinition wichtig, weil sie bestimmen, ob eine Berechtigung delegiert werden soll. Aufgaben, die routinemäßig und einfach durchgeführt werden können und begrenzte Risiken aufweisen, sind gut für die Delegierung geeignet. Im Gegensatz dazu sollten bei Aufgaben, die selten durchgeführt werden, aber einen großen Einfluss auf die Organisation haben und ausgeprägte Fähigkeiten erfordern, sorgfältig überlegt werden, ob diese delegiert werden können. Stattdessen können Sie [ein Konto vorübergehend auf die erforderliche Rolle erhöhen](../active-directory-privileged-identity-management-configure.md) oder die Aufgabe neu zuweisen.

## <a name="delegate-app-administration"></a>Delegieren Sie die Verwaltung der App.

Die Verbreitung von Apps in Ihrer Organisation kann Ihr Delegierungsmodell komplizierter gestalten. Wenn der Aufwand der Zugriffsverwaltung für die Anwendung ausschließlich dem globalen Administrator zufällt, erhöht sich die Verwaltung des Modells wahrscheinlich im Lauf der Zeit. Wenn Sie Benutzern die globale Administratorrolle für Aufgaben wie das Konfigurieren von Geschäftsanwendungen zugeteilt haben, können Sie diese nun an folgende weniger privilegierte Rollen auslagern. So können Sie den Sicherheitsstatus verbessern und das Fehlerpotenzial verringern. Folgende Rollen für Anwendungsadministratoren besitzen die meisten Privilegien:

* Die Rolle **Anwendungsadministrator**, durch die alle Anwendungen im Verzeichnis verwaltet werden können, einschließlich Registrierungen, Einstellungen für einmaliges Anmelden, Benutzer- und Gruppenzuweisungen und -lizenzierung, Anwendungsproxyeinstellungen und Zustimmungen. Durch diese Rolle kann der bedingte Zugriff nicht verwaltet werden.
* Die Rolle **Cloudanwendungsadministrator**, die alle Berechtigungen des Anwendungsadministrators außer dem Zugriff auf die Anwendungsproxyeinstellungen gewährt, da sie nicht über lokale Berechtigungen verfügt. ### Delegieren von App-Besitzerberechtigungen pro App

## <a name="delegate-app-registration"></a>Delegieren der App-Registrierung

Standardmäßig können alle Benutzer App-Registrierungen erstellen. Wenn Sie die Berechtigung zum Erstellen von Anwendungsregistrierungen selektiv gewähren möchten, müssen Sie die Option **Benutzer können Anwendungen registrieren** in den Benutzereinstellungen auf „Nein“ festlegen und anschließend die Rolle „Anwendungsentwickler“ zuweisen. Diese Rolle gewährt die Berechtigung zum Erstellen von Registrierungen nur, wenn die Option **Benutzer können Anwendungen registrieren** deaktiviert ist. Anwendungsentwickler können sich außerdem selbst Zustimmung erteilen, wenn die Option **Users can consent to applications accessing company data on their behalf** (Benutzer können die Zustimmung für den Zugriff auf Unternehmensdaten durch Anwendungen selbst erteilen) auf „Nein“ festgelegt ist. Wenn ein Anwendungsentwickler eine neue Anwendungsregistrierung erstellt, wird dieser automatisch als erster Besitzer hinzugefügt.

## <a name="delegate-app-ownership"></a>Delegieren Sie den Besitz der App.

Für eine noch präzisere Delegierung des Zugriffs auf Apps können Sie den Besitz für einzelne Unternehmensanwendungen zuweisen. Dadurch wird die vorhandene Unterstützung für das Zuweisen von Besitzern für Anwendungsregistrierungen ergänzt. Der Besitz wird pro Unternehmensanwendung auf dem Blatt „Unternehmens-Apps“ zugewiesen. Der Vorteil besteht darin, dass Besitzer nur die Unternehmensanwendungen verwalten können, die Sie besitzen. Sie können beispielsweise einen Besitzer für die Salesforce-Anwendung zuweisen. Dieser Besitzer kann dann den Zugriff auf Salesforce sowie die Konfiguration verwalten, jedoch keine anderen Anwendungen. Eine Unternehmensanwendung kann viele Besitzer aufweisen, und ein Benutzer kann mehrere Unternehmensanwendungen besitzen. Es gibt zwei Rollen für App-Besitzer:

* Die Rolle **Unternehmens-App-Besitzer** erteilt die Berechtigung zum Verwalten der Unternehmens-Apps im Besitz, einschließlich Einstellungen für das einmalige Anmelden, Benutzer- und Gruppenzuweisungen und das Hinzufügen zusätzlicher Besitzer. Die Berechtigung zum Verwalten von Anwendungsproxyeinstellungen oder bedingtem Zugriff wird nicht gewährt.
* Die Rolle **Anwendungsregistrierungsbesitzer** erteilt die Berechtigung zum Verwalten von Anwendungsregistrierungen für die Apps, die der Benutzer besitzt, einschließlich des Anwendungsmanifests und des Hinzufügens zusätzlicher Besitzer.

## <a name="develop-a-security-plan"></a>Entwickeln Sie einen Sicherheitsplan.

Azure AD bietet eine umfassende Anleitung zum Planen und Ausführen eines Sicherheitsplans für Ihre Azure AD-Administratorrollen, diese finden Sie unter [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](directory-admin-roles-secure.md). 

## <a name="establish-emergency-accounts"></a>Erstellen Sie Notfallkonten.

Bereiten Sie gemäß [Verwalten von Administratorkonten für den Notfallzugriff in Azure AD](directory-emergency-access.md) Konten für den Notfallzugriff vor, um den Zugriff auf Identity Management-Speicher nicht zu verlieren, wenn Probleme auftreten.

## <a name="secure-your-administrator-roles"></a>Schützen Sie Ihre Administratorrollen.

Wenn Angreifer den Zugriff auf privilegierte Konten erlangen können, können diese enormen Schaden ausrichten. Deshalb müssen diese Konten zunächst mithilfe einer [Baselinezugriffsrichtlinie](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/22/baseline-security-policy-for-azure-ad-admin-accounts-in-public-preview/) geschützt werden, die standardmäßig für alle Azure AD-Mandanten (in der öffentlichen Vorschauversion) verfügbar ist. Die Richtlinie erzwingt die mehrstufige Authentifizierung für privilegierte Azure AD-Konten. Folgende Azure AD-Rollen werden von der Azure AD-Baselinerichtlinie abgedeckt:

* Globaler Administrator
* SharePoint-Administrator
* Exchange-Administrator
* Administrator für bedingten Zugriff
* Sicherheitsadministrator

## <a name="elevate-privilege-temporarily"></a>Temporäres Erhöhen von Berechtigungen

Für die meisten der täglich anfallenden Aufgaben benötigen nicht alle Benutzer globale Administratorrechte. Außerdem sollte nicht allen Benutzern die globale Administratorrolle dauerhaft zugewiesen werden. Wenn Benutzer als globale Administratoren fungieren müssen, sollte die Rollenzuweisung in Azure AD [Privileged Identity Management](../active-directory-privileged-identity-management-configure.md) aktiviert werden – entweder für ihr eigenes Konto oder ein alternatives Administratorkonto.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen und Beschreibungen zu den Azure AD-Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](directory-assign-admin-roles.md).