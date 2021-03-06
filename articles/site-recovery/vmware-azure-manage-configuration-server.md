---
title: Verwalten des Konfigurationsservers für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen vorhandenen Konfigurationsserver für die VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery verwalten.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 35cce4e9e0b722e8ee1b2ea42a79f18a987033f0
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078643"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Verwalten des Konfigurationsservers für virtuelle VMware-Computer

Sie richten einen lokalen Konfigurationsserver ein, wenn Sie [Azure Site Recovery](site-recovery-overview.md) für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure verwenden. Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure und verwaltet die Datenreplikation. In diesem Artikel werden häufige Aufgaben zur Verwaltung des Konfigurationsservers nach dessen Bereitstellung zusammengefasst.

## <a name="access-configuration-server"></a>Zugreifen auf den Konfigurationsserver

Sie können wie folgt auf den Konfigurationsserver zugreifen:

* Melden Sie sich bei dem virtuellen Computer an, auf dem er bereitgestellt wird, und starten Sie den **Azure Site Recovery Configuration Manager** über die Desktopverknüpfung.
* Alternativ können Sie remote über „https://*Name des Konfigurationsservers*/:44315/“ auf den Konfigurationsserver zugreifen. Melden Sie sich mit Administratoranmeldeinformationen an.

## <a name="modify-vmware-server-settings"></a>Ändern von VMware-Servereinstellungen

1. Klicken Sie nach dem [Anmelden](#access-configuration-server) auf **vCenter-Server/vSphere ESXi-Server hinzufügen**, um dem Konfigurationsserver einen anderen VMware-Server zuzuordnen.
2. Geben Sie die Eigenschaften ein, und wählen Sie dann **OK** aus.

## <a name="modify-credentials-for-automatic-discovery"></a>Ändern der Anmeldeinformationen für die automatische Ermittlung

1. Wenn Sie die Anmeldeinformationen für die Verbindungsherstellung mit dem VMware-Server für die automatische Ermittlung von VMware-VMs ändern möchten, wählen Sie nach dem [Anmelden](#access-configuration-server) das Konto aus, und klicken Sie auf **Bearbeiten**.
2. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie dann auf **OK**.

    ![Ändern von VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)

Die Anmeldeinformationen können auch über „CSPSConfigtool.exe“ geändert werden.

1. Melden Sie sich beim Konfigurationsserver an, und starten Sie „CSPSConfigtool.exe“.
2. Wählen Sie das Konto aus, das Sie ändern möchten, und klicken Sie auf **Bearbeiten**.
3. Geben Sie die geänderten Anmeldeinformationen ein, und klicken Sie auf **OK**.

## <a name="modify-credentials-for-mobility-service-installation"></a>Ändern der Anmeldeinformationen für die Installation von Mobility Service

Ändern Sie die Anmeldeinformationen für die automatische Installation von Mobility Service auf VMware-VMs, die Sie für die Replikation aktivieren möchten.

1. Klicken Sie nach dem [Anmelden](#access-configuration-server) auf **VM-Anmeldeinformationen verwalten**.
2. Wählen Sie das Konto aus, das Sie ändern möchten, und klicken Sie auf **Bearbeiten**.
3. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie dann auf **OK**.

    ![Ändern der Anmeldeinformationen für Mobility Service](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

Anmeldeinformationen können auch über „CSPSConfigtool.exe“ geändert werden.

1. Melden Sie sich beim Konfigurationsserver an, und starten Sie „CSPSConfigtool.exe“.
2. Wählen Sie das Konto aus, das Sie ändern möchten, und klicken Sie auf **Bearbeiten**.
3. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie auf **OK**.

## <a name="add-credentials-for-mobility-service-installation"></a>Hinzufügen von Anmeldeinformationen für die Installation von Mobility Service

Gehen Sie wie folgt vor, falls Sie im Rahmen der OVF-Bereitstellung des Konfigurationsservers keine Anmeldeinformationen hinzugefügt haben:

1. Klicken Sie nach dem [Anmelden](#access-configuration-server) auf **VM-Anmeldeinformationen verwalten**.
2. Klicken Sie auf **VM-Anmeldeinformationen hinzufügen**.
    ![add-mobility-credentials](media/vmware-azure-manage-configuration-server/add-mobility-credentials.png)
3. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie auf **Hinzufügen**.

Anmeldeinformationen können auch über „CSPSConfigtool.exe“ hinzugefügt werden.

1. Melden Sie sich beim Konfigurationsserver an, und starten Sie „CSPSConfigtool.exe“.
2. Klicken Sie auf **Hinzufügen**, geben Sie die neuen Anmeldeinformationen ein, und klicken Sie anschließend auf **OK**.

## <a name="modify-proxy-settings"></a>Ändern von Proxyeinstellungen

Ändern Sie die Proxyeinstellungen, die vom Konfigurationsservercomputer für den Internetzugriff in Azure verwendet werden. Ändern Sie die Einstellungen auf beiden Computern, wenn Sie zusätzlich zum Standardprozessserver, der auf dem Konfigurationsservercomputer ausgeführt wird, über einen weiteren Prozessservercomputer verfügen.

1. Klicken Sie nach der [Anmeldung](#access-configuration-server) beim Konfigurationsserver auf **Konnektivität verwalten**.
2. Aktualisieren Sie die Proxywerte. Klicken Sie auf **Speichern**, um die Einstellungen zu aktualisieren.

## <a name="add-a-network-adapter"></a>Hinzufügen eines Netzwerkadapters

Die OVF-Vorlage (Open Virtualization Format) stellt die Konfigurationsserver-VM mit einem einzelnen Netzwerkadapter bereit.

- Sie können [der VM einen zusätzlichen Adapter hinzufügen](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), müssen diesen Schritt aber vor der Registrierung des Konfigurationsservers im Tresor durchführen.
- Nach dem Registrieren des Konfigurationsservers im Tresor fügen Sie einen Adapter in den Eigenschaften des virtuellen Computers hinzu. Danach müssen Sie den Server [erneut im Tresor registrieren](#reregister-a-configuration-server-in-the-same-vault).


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Erneutes Registrieren eines Konfigurationsservers im selben Tresor

Sie können den Konfigurationsserver bei Bedarf im selben Tresor erneut registrieren. Wenn Sie zusätzlich zum Standardprozessserver, der auf dem Konfigurationsservercomputer ausgeführt wird, über einen weiteren Prozessservercomputer verfügen, registrieren Sie beide Computer erneut.


  1. Öffnen Sie im Tresor **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
  2. Klicken Sie unter **Server** auf **Registrierungsschlüssel herunterladen**, um die Datei mit den Tresoranmeldeinformationen herunterzuladen.
  3. Melden Sie sich auf dem Konfigurationsservercomputer an.
  4. Öffnen Sie in **%ProgramData%\ASR\home\svsystems\bin** die Datei **cspsconfigtool.exe**.
  5. Klicken Sie auf der Registerkarte **Tresorregistrierung** auf **Durchsuchen**, und suchen Sie die Datei mit den Anmeldeinformationen für den Tresor, die Sie heruntergeladen haben.
  6. Geben Sie bei Bedarf die Proxyserverdetails an. Klicken Sie anschließend auf **Registrieren**.
  7. Öffnen Sie als Administrator ein PowerShell-Eingabefenster, und führen Sie den folgenden Befehl aus:
   ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

      >[!NOTE]
      >Um das **neueste Zertifikat** per Pull vom Konfigurationsserver auf den Prozessserver für die horizontale Skalierung abzurufen, führen Sie den folgenden Befehl aus: *"<Installation Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe>" --registermt*

  8. Starten Sie den OBEngine-Dienst neu, indem Sie den folgenden Befehl ausführen.
  ```
          net stop obengine
          net start obengine
   ```


## <a name="register-a-configuration-server-with-a-different-vault"></a>Registrieren eines Konfigurationsservers in einem anderen Tresor

> [!WARNING]
> In den nachfolgenden Schritten wird die Zuordnung des Konfigurationsservers zum aktuellen Tresor getrennt und die Replikation aller geschützten virtuellen Computer unter dem Konfigurationsserver beendet.

1. Melden Sie sich beim Konfigurationsserver an.
2. Öffnen Sie als Administrator ein PowerShell-Eingabefenster, und führen Sie den folgenden Befehl aus:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Starten Sie das Browserportal der Konfigurationsserver-Appliance über die Verknüpfung auf Ihrem Desktop.
4. Führen Sie die Registrierungsschritte aus. Diese sind ähnlich wie bei der [Registrierung](vmware-azure-tutorial.md#register-the-configuration-server) eines neuen Konfigurationsservers.

## <a name="upgrade-the-configuration-server"></a>Aktualisieren Sie den Konfigurationsserver

Sie führen Updaterollups aus, um den Konfigurationsserver zu aktualisieren. Updates können für maximal N-4 Versionen angewendet werden. Beispiel: 

- Wenn Sie Version 9.7, 9.8, 9.9 oder 9.10 ausführen, können Sie direkt auf 9.11 aktualisieren.
- Falls Sie Version 9.6 oder eine ältere Version ausführen und auf 9.11 aktualisieren möchten, müssen Sie zuerst das Upgrade auf Version 9.7 durchführen, bevor das Upgrade auf 9.11 möglich ist.

Links zu Updaterollups zum Aktualisieren aller Versionen des Konfigurationsservers sind auf der [Wiki-Seite zu den Updates](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) verfügbar.

Aktualisieren Sie den Server wie folgt:

1. Gehen Sie im Tresor zu **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
2. Wenn ein Update verfügbar ist, wird ein Link in der Spalte **Agent-Version** angezeigt.
    ![Aktualisieren](./media/vmware-azure-manage-configuration-server/update2.png)
3. Laden Sie die Datei mit dem Update-Installer auf den Konfigurationsserver herunter.

    ![Aktualisieren](./media/vmware-azure-manage-configuration-server/update1.png)

4. Doppelklicken Sie auf die Datei, um das Installationsprogramm auszuführen.
5. Das Installationsprogramm erkennt die aktuelle Version, die auf dem Computer ausgeführt wird. Klicken Sie auf **Ja**, um das Upgrade zu starten.
6. Nach Abschluss des Upgrades wird die Serverkonfiguration überprüft.

    ![Aktualisieren](./media/vmware-azure-manage-configuration-server/update3.png)

7. Klicken Sie auf **Fertig stellen**, um das Installationsprogramm zu schließen.

## <a name="delete-or-unregister-a-configuration-server"></a>Löschen oder Aufheben der Registrierung eines Konfigurationsservers

1. Wählen Sie [Schutz deaktivieren](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) für alle virtuellen Computer unter dem Konfigurationsserver.
2. [Heben Sie die Zuordnung von allen Replikationsrichtlinien zum Konfigurationsserver auf](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy), oder [löschen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) Sie sie.
3. [Löschen](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) Sie alle vCenter-Server/vSphere-Hosts, die dem Konfigurationsserver zugeordnet sind.
4. Öffnen Sie im Tresor **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
5. Wählen Sie den Konfigurationsserver aus, den Sie entfernen möchten. Klicken Sie auf der Seite **Details** auf **Löschen**.

    ![Löschen von Konfigurationsservern](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)


### <a name="delete-with-powershell"></a>Löschen mit PowerShell

Optional können Sie den Konfigurationsserver mithilfe von PowerShell löschen:

1. [Installieren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Sie das Azure PowerShell-Modul.
2. Melden Sie sich mit diesem Befehl bei Ihrem Azure-Konto an:

    `Connect-AzureRmAccount`
3. Wählen Sie das Tresorabonnement aus:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Legen Sie den Tresorkontext fest.

    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Rufen Sie den Konfigurationsserver ab:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Löschen Sie den Konfigurationsserver.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Sie können die Option **-Force** im Cmdlet Remove-AzureRmSiteRecoveryFabric verwenden, um das Löschen des Konfigurationsservers zu erzwingen.

## <a name="generate-configuration-server-passphrase"></a>Generieren der Passphrase des Konfigurationsservers

1. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Um das Verzeichnis in den Ordner „bin“ zu ändern, führen Sie den Befehl **cd %ProgramData%\ASR\home\svsystems\bin** aus.
3. Um die Passphrasedatei zu generieren, führen Sie **genpassphrase.exe -v > MobSvc.passphrase** aus.
4. Die Passphrase wird in der Datei gespeichert, die sich unter **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase** befindet.

## <a name="renew-ssl-certificates"></a>Erneuern von SSL-Zertifikaten

Der Konfigurationsserver verfügt über einen integrierten Webserver, der die Aktivitäten von Mobility Service, Prozessservern und Masterzielservern, die mit dem Konfigurationsserver verbunden sind, orchestriert. Der Webserver verwendet ein SSL-Zertifikat, um Clients zu authentifizieren. Das Zertifikat läuft nach drei Jahren ab und kann jederzeit erneuert werden.

### <a name="check-expiry"></a>Überprüfen des Ablaufs

Für Bereitstellungen von Konfigurationsservern vor dem Mai 2016 wurde die Zertifikatablaufzeit auf ein Jahr festgelegt. Wenn eines Ihrer Zertifikate demnächst abläuft, geschieht Folgendes:

- Wenn das Ablaufdatum höchstens zwei Monate in der Zukunft liegt, sendet der Dienst Benachrichtigungen im Portal und per E-Mail (wenn Sie Site Recovery-Benachrichtigungen abonniert haben).
- Auf der Ressourcenseite des Tresors wird ein Benachrichtigungsbanner angezeigt. Klicken Sie auf das Banner, um ausführlichere Informationen zu erhalten.
- Wenn die Schaltfläche **Jetzt Upgrade durchführen** angezeigt wird, wurde für einige Komponenten in Ihrer Umgebung noch kein Upgrade auf 9.4.xxxx.x oder höhere Versionen durchgeführt. Aktualisieren Sie die Komponenten, bevor Sie das Zertifikat erneuern. Sie können bei älteren Versionen keine Erneuerung durchführen.

### <a name="renew-the-certificate"></a>Erneuern von Zertifikaten

1. Öffnen Sie im Tresor **Site Recovery-Infrastruktur** > **Konfigurationsserver**. Wählen Sie den erforderlichen Konfigurationsserver aus.
2. Das Ablaufdatum wird unter **Integrität des Konfigurationsservers** angezeigt.
3. Klicken Sie auf **Zertifikate erneuern**.

## <a name="update-windows-licence"></a>Aktualisieren der Windows-Lizenz

Bei der mit der OVF-Vorlage bereitgestellten Lizenz handelt es sich um eine Evaluierungslizenz mit einer Gültigkeit von 180 Tagen. Zur ununterbrochenen Nutzung müssen Sie Windows mit einer käuflich erworbenen Lizenz aktivieren.

## <a name="failback-requirements"></a>Failbackanforderungen

Während des erneuten Schützens und des Failbacks muss der lokale Konfigurationsserver ausgeführt werden und verbunden sein. Für ein erfolgreiches Failback muss der betreffende virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein.

Stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Konfigurationsservers durchführen. Wenn der Konfigurationsserver bei einem Notfall verloren geht, müssen Sie zunächst den Konfigurationsserver aus einer Sicherungskopie wiederherstellen und dann sicherstellen, dass der wiederhergestellte Konfigurationsserver über die gleiche IP-Adresse verfügt, mit der er bei dem Tresor registriert wurde. Das Failback funktioniert nicht, wenn für den wiederhergestellten Konfigurationsserver eine andere IP-Adresse verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Tutorials zum Einrichten der Notfallwiederherstellung von [VMware-VMs](vmware-azure-tutorial.md) in Azure an.
