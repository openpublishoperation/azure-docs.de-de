---
title: Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C | Microsoft-Dokumentation
description: Dieses Thema veranschaulicht, wie Sie die E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C deaktivieren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e008fb87b57b92f8f7e914e6b4344b52d42f9ef8
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263928"
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern
Wenn diese Funktionalität aktiviert ist, ermöglicht Azure Active Directory B2C es Endbenutzern, sich durch Angeben einer E-Mail-Adresse und Erstellen eines lokalen Kontos für Anwendungen zu registrieren. Azure AD B2C stellt sicher, dass gültige E-Mail-Adressen verwendet werden, indem Endbenutzer diese während des Registrierungsvorgangs verifizieren müssen. B2C verhindert auch, dass ein automatisierter Prozess böswillig gefälschte Konten für die Anwendungen generiert.

Einige Anwendungsentwickler bevorzugen, die E-Mail-Überprüfung während des Registrierungsvorgangs zu überspringen und diese erst später vom Endbenutzer anzufordern. Um dies zu unterstützen, kann Azure AD B2C so konfiguriert werden, dass die E-Mail-Überprüfung deaktiviert wird. Diese Vorgehensweise sorgt für einen einfacheren Registrierungsvorgang und bietet Entwicklern die Flexibilität, zwischen Endbenutzern zu unterscheiden, die ihre E-Mail-Adresse bereits verifiziert haben, und solchen, die dies noch nicht getan haben.

Standardmäßig ist die E-Mail-Überprüfung in Registrierungsrichtlinien aktiviert. Führen Sie zum Aktivieren der Funktion folgende Schritte aus:

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf **Registrierungsrichtlinien** oder auf **Registrierungs- oder Anmelderichtlinien**, je nachdem, welche Art Richtlinien Sie für die Registrierung konfiguriert haben.
3. Klicken Sie auf Ihre Richtlinie (z.B. „B2C_1_SiUp“), um sie zu öffnen. 
4. Klicken Sie oben auf dem Blatt auf **Bearbeiten**.
5. Klicken Sie auf **Seite für die Benutzeroberflächenanpassung**.
6. Klicken Sie auf **Registrierungsseite für lokales Konto**.
7. Klicken Sie im Abschnitt **Registrierungsattribute** in der Spalte **Name** auf **E-Mail-Adresse**.
8. Legen Sie die Option **Überprüfung anfordern** auf **Nein** fest.
9. Klicken Sie im unteren Bereich auf **OK**, bis Sie zum Blatt **Richtlinie bearbeiten** gelangen.
10. Klicken Sie oben auf dem Blatt auf **Speichern** . Sie haben es geschafft!

> [!NOTE]
> Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. Wenn Sie den Standardprozess deaktivieren, empfiehlt es sich, ein eigenes Überprüfungssystem einzurichten.
> 
> 

Wir sind stets offen für Feedback und Vorschläge. Wenn Sie Probleme mit diesem Thema oder Vorschläge zur Verbesserung dieses Inhalts haben, würden wir uns über Ihr Feedback unten auf der Seite freuen. Anforderungen neuer Features geben Sie bitte unter [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) ein.
