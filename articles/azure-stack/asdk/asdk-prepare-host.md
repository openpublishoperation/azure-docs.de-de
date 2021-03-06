---
title: Vorbereiten des Azure Stack Development Kit-Hostcomputers (ASDK) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie den Azure Stack Development Kit-Hostcomputer für die ASDK-Installation vorbereiten.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: dd02dfe37a1566a3121b3571f72d964621330cd0
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647305"
---
# <a name="prepare-the-asdk-host-computer"></a>Vorbereiten des ASDK-Hostcomputers
Bevor Sie das ASDK auf dem Hostcomputer installieren können, muss die ASDK-Umgebung vorbereitet werden. Nach der Vorbereitung des Development Kit-Hostcomputers startet der Computer von der Festplatte des virtuellen Computers (CloudBuilder.vhdx), sodass die ASDK-Bereitstellung beginnen kann.

## <a name="prepare-the-development-kit-host-computer"></a>Vorbereiten des Development Kit-Hostcomputers
Bevor Sie das ASDK auf dem Hostcomputer installieren können, muss die Umgebung des ASDK-Hostcomputers vorbereitet werden.
1. Melden Sie sich als lokaler Administrator bei Ihrem Development Kit-Hostcomputer an.
2. Stellen Sie sicher, dass die Datei „CloudBuilder.vhdx“ in den Stammordner des Laufwerks C:\ verschoben wurde (C:\CloudBuilder.vhdx).
3. Führen Sie das folgende Skript zum Herunterladen der Development Kit-Installationsdatei („asdk-installer.ps1“) aus dem [GitHub-Repository mit den Azure Stack-Tools](https://github.com/Azure/AzureStack-Tools) in den Ordner **C:\AzureStack_Installer** auf Ihrem Development Kit-Hostcomputer aus:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Enforce usage of TLSv1.2 to download from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Führen Sie in einer PowerShell-Konsole mit erhöhten Rechten das Skript **C:\AzureStack_Installer\asdk-installer.ps1** aus, und klicken Sie anschließend auf **Umgebung vorbereiten**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Navigieren Sie auf der Seite **„CloudBuilder.vhdx“ auswählen** des Installationsprogramms zu der in den [vorherigen Schritten](asdk-download.md) heruntergeladenen und extrahierten **CloudBuilder.vhdx**-Datei, und wählen Sie sie aus. Auf dieser Seite können Sie optional auch das Kontrollkästchen **Treiber hinzufügen** aktivieren, wenn Sie dem Development Kit-Hostcomputer zusätzliche Treiber hinzufügen müssen. Klicken Sie auf **Weiter**.  

    ![](media/asdk-prepare-host/2.PNG)

6. Geben Sie auf der Seite **Optionale Einstellungen** das lokale Administratorkonto für den Development Kit-Hostcomputer an, und klicken Sie auf **Weiter**. Sie können auch Werte für die folgenden optionalen Einstellungen angeben:
  - **Computername**: Diese Option legt den Namen für den Development Kit-Host fest. Der Name muss die FQDN-Anforderungen erfüllen und darf höchstens 15 Zeichen umfassen. Der Standardwert ist ein von Windows generierter zufälliger Computername.
  - **Statische IP-Konfiguration**: Legt für Ihre Bereitstellung die Verwendung einer statischen IP-Adresse fest. Andernfalls werden beim Neustart des Installationsprogramms in „cloudbuilder.vhdx“ die Netzwerkschnittstellen mit DHCP konfiguriert.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Wenn Sie die Anmeldeinformationen des lokalen Administrators in diesem Schritt nicht angeben, benötigen Sie direkten Zugriff oder KVM-Zugriff auf den Host, nachdem der Computer während der Development Kit-Einrichtung neu gestartet wurde.

7. Wenn Sie im vorherigen Schritt eine statische IP-Konfiguration ausgewählt haben, müssen Sie jetzt Folgendes tun:
    - Wählen Sie einen Netzwerkadapter aus. Stellen Sie sicher, dass Sie eine Verbindung mit dem Adapter herstellen können, bevor Sie auf **Weiter** klicken.
    - Stellen Sie sicher, dass die Werte für **IP-Adresse**, **Gateway** und **DNS** richtig sind, und klicken Sie dann auf **Weiter**.
13. Klicken Sie auf **Weiter**, um den Vorbereitungsprozess zu starten.
14. Wenn die Vorbereitung **Abgeschlossen** anzeigt, klicken Sie auf **Weiter**.
15. Klicken Sie auf **Jetzt neu starten**, um den Development Kit-Hostcomputer in „CloudBuilder.vhdx“ zu starten und den [Bereitstellungsprozess fortzusetzen](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Nächste Schritte
[Installieren des ASDK](asdk-install.md)
