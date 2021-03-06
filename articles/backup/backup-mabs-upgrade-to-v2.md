---
title: Installieren von Azure Backup Server v2
description: Azure Backup Server v2 bietet Ihnen erweiterte Sicherungsfunktionen für den Schutz von u.a. virtuellen Computern, Dateien, Ordner und Workloads. Erfahren Sie, wie Sie Azure Backup Server v2 installieren oder ein Upgrade auf diese Version ausführen.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: adigan
ms.openlocfilehash: a458a46f3775a593f369d5acb967fc90d61efde8
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628340"
---
# <a name="install-azure-backup-server-v2"></a>Installieren von Azure Backup Server v2

Azure Backup Server schützt Ihre virtuellen Computer (VMs), Workloads, Dateien und Ordner und vieles mehr. Azure Backup Server v2 baut auf Azure Backup Server v1 auf und bietet neue Funktionen, die in v1 nicht verfügbar sind. Einen Vergleich der Features von v1 und v2 finden Sie unter [Azure Backup Server-Schutzmatrix](backup-mabs-protection-matrix.md). 

Die zusätzlichen Funktionen in Backup Server v2 sind ein Upgrade von Backup Server v1. Allerdings ist Backup Server v1 keine Voraussetzung für die Installation von Backup Server v2. Wenn Sie Backup Server v1 auf Backup Server v2 aktualisieren, installieren Sie Backup Server v2 auf dem Backup Server-Schutzserver. Ihre vorhandenen Backup Server-Einstellungen bleiben intakt.

Sie können Backup Server v2 unter Windows Server 2016 oder Windows Server 2012 R2 installieren. Um neue System Center 2016 Data Protection Manager-Funktionen wie Modern Backup Storage nutzen zu können, müssen Sie Backup Server v2 unter Windows Server 2016 installieren. Machen Sie sich vor der Installation von oder einem Upgrade auf Backup Server v2 mit den [Installationsvoraussetzungen](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites) vertraut.

> [!NOTE]
> Azure Backup Server und System Center Data Protection Manager haben dieselbe Codebasis. Backup Server v1 ist mit Data Protection Manager 2012 R2 äquivalent. Backup Server v2 ist mit Data Protection Manager 2016 äquivalent. In diesem Artikel wird gelegentlich auf die Data Protection Manager-Dokumentation verwiesen.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Upgrade von Azure Backup Server auf v2
Stellen Sie für ein Upgrade von Backup Server v1 auf Backup Server v2 sicher, dass die Installation über die erforderlichen Updates verfügt:

- [Aktualisieren Sie die Schutz-Agents](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent) auf den geschützten Servern.
- Aktualisieren Sie Windows Server 2012 R2 auf Windows Server 2016.
- Aktualisieren Sie den Azure Backup Server-Remoteadministrator auf allen Produktionsservern.
- Stellen Sie sicher, dass Sicherungen auf Fortsetzen ohne Neustarten Ihres Produktionsservers festgelegt sind.


### <a name="upgrade-steps-for-backup-server-v2"></a>Upgradeschritte für Backup Server v2

1. Sie müssen im Download Center [das Installationsprogramm des Upgrades herunterladen](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Nachdem Sie den Setup-Assistenten extrahiert haben, stellen sicher, dass **setup.exe ausführen** ausgewählt ist, und wählen Sie dann **Fertig stellen**.

  ![Installationsprogramm für Setup: Setup ausführen](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. Wählen Sie im Microsoft Azure Backup Server-Assistenten unter **Installieren** die Option **Microsoft Azure Backup Server** aus.

  ![Installationsprogramm für Setup: „Installieren“ auswählen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Überprüfen Sie auf der **Startseite** die Warnungen, und klicken Sie dann auf **Weiter**.

  ![Installationsprogramm für Setup: Startseite](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. Der Setup-Assistent führt Voraussetzungsprüfungen aus, um sicherzustellen, dass ein Upgrade Ihrer Umgebung erfolgen kann. Wählen Sie auf der Seite **Überprüfungen der Voraussetzungen** die Option **Prüfen** aus.

  ![Installationsprogramm für Setup: Seite „Überprüfungen der Voraussetzungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Ihre Umgebung muss die Voraussetzungsprüfungen bestehen. Falls Ihre Umgebung die Prüfungen nicht besteht, bestimmen Sie die Probleme, und korrigieren Sie sie. Wählen Sie dann **Erneut prüfen** aus. Sobald Sie die Überprüfungen der Voraussetzungen bestanden haben, klicken Sie auf **Weiter**.

  ![Installationsprogramm für Setup: Schaltfläche „Erneut prüfen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Wählen Sie auf der Seite **SQL-Einstellungen** die Ihrer SQL-Installation entsprechende Option aus, und klicken Sie dann auf **Prüfen und installieren**.

  ![Installationsprogramm für Setup: Seite „SQL-Einstellungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  Die Prüfungen können einige Minuten dauern. Wenn die Prüfungen abgeschlossen sind, klicken Sie auf **Weiter**.

  ![Installationsprogramm für Setup: Schaltfläche „Prüfen und installieren“ auf der Seite „SQL-Einstellungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and-fix-settings.png)

8. Ändern Sie auf der Seite **Installationseinstellungen** den Speicherort in den Installationspfad von Backup Server oder in das Scratchverzeichnis. Klicken Sie auf **Weiter**.

  ![Installationsprogramm für Setup: Seite „Installationseinstellungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Klicken Sie zum Beenden des Setup-Assistenten auf **Fertig stellen**.

  ![Installationsprogramm für Setup: Fertig stellen](./media/backup-mabs-upgrade-to-v2/run-setup.png)

## <a name="add-storage-for-modern-backup-storage"></a>Hinzufügen von Speicher für Modern Backup Storage

Um die Effizienz der Speicherung von Sicherungen zu verbessern, unterstützt Backup Server v2 nun Volumes. Backup Server v1 und v2 unterstützen beide Datenträger.

### <a name="add-volumes-and-disks"></a>Hinzufügen von Volumes und Datenträgern

Wenn Sie Backup Server v2 unter Windows Server 2016 ausführen, können Sie zum Speichern von Sicherungsdaten Volumes nutzen. Volumes ermöglichen Speichereinsparungen und schnellere Sicherungen. Da Volumes in Backup Server neu sind, müssen Sie sie hinzufügen. 

Wenn Sie Backup Server ein Volume hinzufügen, können Sie dem Volume einen Anzeigenamen geben. Markieren Sie die Spalte **Anzeigename** des Volumes, um es zu benennen. Sie können den Namen später bei Bedarf ändern. Sie können auch mit PowerShell Anzeigenamen für Volumes hinzufügen oder ändern.

So fügen Sie in der Administratorkonsole ein Volume hinzu

1. Wählen Sie in der Azure Backup Server-Administratorkonsole **Verwaltung** > **Datenspeicher** > **Hinzufügen** aus.

  ![Öffnen des Assistenten „Datenspeicher hinzufügen“](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

  Der Assistent „Datenspeicher hinzufügen“ wird geöffnet.

2. Wählen Sie auf der Seite **Datenspeicher hinzufügen** im Feld **Verfügbare Volumes** ein Volume aus, und klicken Sie dann auf **Hinzufügen**.

3. Geben Sie in das Feld **Ausgewählte Volumes** einen Anzeigenamen für das Volume ein, und klicken Sie dann auf **OK**.

  ![Assistent „Datenspeicher hinzufügen“: Volume hinzufügen](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Wenn Sie einen Datenträger hinzufügen möchten, muss der Datenträger zu einer Schutzgruppe gehören, die über Legacyspeicher verfügt. Sie können diese Datenträger nur für diese Schutzgruppen verwenden. Falls Backup Server keine Quellen mit Legacyschutz hat, wird der Datenträger nicht aufgeführt.

  Weitere Informationen zum Hinzufügen von Datenträgern finden Sie unter [Durchführen eines Upgrades für Ihre DPM-Installation](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Ein Datenträger kann nicht mit einem Anzeigenamen versehen werden.


### <a name="assign-workloads-to-volumes"></a>Zuweisen von Workloads zu Volumes

Geben Sie Backup Server an, welche Workloads welchen Volumes zugewiesen werden. Sie können beispielsweise kostenintensive Volumes festlegen, die eine hohe Anzahl von Ein- und Ausgabevorgängen pro Sekunde (IOPS) unterstützen, um nur Workloads zu speichern, die häufige Sicherungen mit großem Volumen erfordern. Ein Beispiel ist SQL Server mit Transaktionsprotokollen.

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Um die Eigenschaften eines Volumes im Speicherpool in Backup Server zu aktualisieren, verwenden Sie das PowerShell-Cmdlet **Update-DPMDiskStorage**.

Syntax:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [<CommonParameters>]
```

Alle Änderungen, die Sie mithilfe von PowerShell vornehmen, werden auf der Benutzeroberfläche übernommen.


## <a name="protect-data-sources"></a>Schützen von Datenquellen
Erstellen Sie eine Schutzgruppe, um mit dem Schützen von Datenquellen zu beginnen. In den folgenden Schritten werden Änderungen und Ergänzungen am Assistenten für neue Schutzgruppen vorgestellt.

So erstellen Sie eine Schutzgruppe

1. Wählen Sie in der Backup Server-Verwaltungskonsole **Schutz** aus.

2. Wählen Sie auf dem Menüband **Neu** aus.

  Der Assistent zum Erstellen einer neuen Schutzgruppe wird geöffnet.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. Wählen Sie auf der Seite **Willkommen** die Option **Weiter** aus.

4. Wählen Sie auf der Seite **Schutzgruppentyp auswählen** den Typ der zu erstellenden Schutzgruppe aus, und klicken Sie dann auf **Weiter**.

  ![Seite „Schutzgruppentyp auswählen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Auf der Seite **Gruppenmitglieder auswählen** werden im Feld **Verfügbare Mitglieder** die Mitglieder mit Schutz-Agents aufgelistet. Wählen Sie in diesem Beispiel die Volumes „D:\“ und „E:\“\, aus, und fügen Sie beide dem Bereich **Ausgewählte Mitglieder** hinzu. Klicken Sie auf **Weiter**.

  ![Seite „Gruppenmitglieder auswählen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Geben Sie auf der Seite **Datenschutzmethode auswählen** den **Namen der Schutzgruppe** ein, wählen Sie die Schutzmethode aus, und klicken Sie dann auf **Weiter**. Wenn Sie kurzfristigen Schutz wünschen, wählen Sie die Sicherungsmethode **Datenträger** aus.

  ![Seite „Datenschutzmethode auswählen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Wählen Sie auf der Seite **Kurzfristige Ziele angeben** die Details für **Beibehaltungsdauer** und **Synchronisierungsfrequenz** aus. Klicken Sie anschließend auf **Weiter**. Um den Zeitplan für das Erstellen von Wiederherstellungspunkten zu ändern, wählen Sie optional **Ändern** aus.

  ![Seite „Kurzfristige Ziele angeben“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Überprüfen Sie auf der Seite **Datenspeicherzuordnung überprüfen** die Details der Datenquellen, die Sie ausgewählt haben, einschließlich ihrer Größe, der Werte für den Speicherplatz, der bereitgestellt werden soll, und des Zielspeichervolumes.

  ![Seite „Datenspeicherzuordnung überprüfen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Speichervolumes basieren auf der (mithilfe von PowerShell) festgelegten Zuordnung von Volumes für Workloads und dem verfügbaren Speicherplatz. Sie können die Speichervolumes ändern, indem Sie im Dropdownmenü andere Volumes auswählen. Wenn Sie den Wert für **Zielspeicher** ändern, wird der Wert von **Verfügbarer Datenspeicher** dynamisch zum Wiedergeben der Werte unter **Freier Speicherplatz** und **Underprovisioned Space** (Nicht ausreichend bereitgestellter Speicherplatz) geändert.

  Wenn die Datenquellen wie geplant anwachsen, gibt der Wert in der Spalte **Underprovisioned Space** (Nicht ausreichend bereitgestellter Speicherplatz) unter **Verfügbarer Datenspeicher** die Menge an zusätzlichem Speicher an, die erforderlich ist. Planen Sie mithilfe dieses Werts Ihre Speicheranforderungen für reibungslose Sicherungen. Wenn der Wert 0 (null) ist, sind in naher Zukunft keine potenziellen Probleme mit Speicher zu erwarten. Wenn der Wert ungleich 0 (null) ist, haben Sie nicht genügend Speicher (basierend auf Ihrer Schutzrichtlinie und der Datengröße Ihre geschützten Mitglieder) zugeteilt.

  ![Nicht ausreichend zugeordneter Datenträgerspeicher](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

  Schließen Sie den Assistenten ab, um das Erstellen Ihrer Schutzgruppe fertig zu stellen.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Migrieren von Legacyspeicher zu Modern Backup Storage
Nach dem Upgrade auf oder der Installation von Backup Server v2 und dem anschließenden Upgrade des Betriebssystems auf Windows Server 2016 müssen Sie Ihre Schutzgruppen für die Verwendung von Modern Backup Storage aktualisieren. Standardmäßig werden Schutzgruppen nicht geändert. Sie funktionieren weiterhin, wie sie ursprünglich eingerichtet wurden. 

Das Aktualisieren von Schutzgruppen zur Verwendung von Modern Backup Storage ist optional. Beenden Sie zum Aktualisieren der Schutzgruppe den Schutz aller Datenquellen mithilfe der Option „Daten beibehalten“. Fügen Sie dann die Datenquellen einer neuen Schutzgruppe hinzu.

1. Wählen Sie in der System Center 2016 DPM-Verwaltungskonsole das **Schutz**-Feature aus. Klicken Sie in der Liste **Schutzgruppenmitglied** mit der rechten Maustaste auf das Mitglied, und wählen Sie dann **Schutz des Mitglieds beenden** aus.

  ![Schutz des Mitglieds beenden](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. Überprüfen Sie im Dialogfeld **Aus Gruppe entfernen** den belegten Speicherplatz und den verfügbaren freien Speicherplatz für den Speicherpool. Die Standardeinstellung ist das Belassen der Wiederherstellungspunkte auf dem Datenträger und sie gemäß der zugehörigen Beibehaltungsrichtlinie ablaufen zu lassen. Klicken Sie auf **OK**.

  Wenn der belegte Speicherplatz sofort an den Pool mit freiem Speicherplatz zurückgegeben werden soll, aktivieren Sie das Kontrollkästchen **Replikat auf Datenträger löschen**, um die Sicherungsdaten (und Wiederherstellungspunkte) zu löschen, die zu diesem Mitglied gehören.

  ![Dialogfeld „Aus Gruppe entfernen“](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Erstellen Sie eine Schutzgruppe, die Modern Backup Storage verwendet. Fügen Sie die ungeschützten Datenquellen hinzu.


## <a name="add-disks-to-increase-legacy-storage"></a>Hinzufügen von Datenträgern zum Vergrößern des Legacyspeichers

Wenn Sie Legacyspeicher mit Backup Server verwenden möchten, müssen Sie möglicherweise Datenträger hinzufügen, um den Legacyspeicher zu vergrößern. 

So fügen Sie Datenspeicher hinzu

1. Wählen Sie in der System Center 2016 DPM-Verwaltungskonsole **Verwaltung** > **Datenspeicher** > **Hinzufügen** aus.

  ![Dialogfeld „Datenspeicher hinzufügen“](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

2. Wählen Sie im Dialogfeld **Datenspeicher hinzufügen** den Befehl **Datenträger hinzufügen** aus.

3. Wählen Sie in der Liste der verfügbaren Datenträger die Datenträger aus, die Sie hinzufügen möchten. Klicken Sie auf **Hinzufügen** und dann auf **OK**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Aktualisieren des Data Protection Manager-Schutz-Agents

Backup Server verwendet für Updates den System Center Data Protection Manager-Schutz-Agent. Wenn Sie ein Upgrade für einen Schutz-Agent durchführen, der nicht mit dem Netzwerk verbunden ist, können Sie ein Upgrade für mit dem Netzwerk verbundene Agents nicht mithilfe der Data Protection Manager-Verwaltungskonsole durchführen. Sie müssen den Schutz-Agent in einer nicht aktiven Domänenumgebung aktualisieren. Bis zum Verbinden des Clientcomputers mit dem Netzwerk wird in der Data Protection Manager-Administratorkonsole angezeigt, dass das Update des Schutz-Agents aussteht.

In den folgenden Abschnitten wird beschrieben, wie Schutz-Agents für Clientcomputer aktualisiert werden, die mit dem Netzwerk verbunden bzw. nicht verbunden sind.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Aktualisieren eines Schutz-Agents auf einem mit dem Netzwerk verbundenen Clientcomputer

1. Wählen Sie in der Backup Server-Administratorkonsole **Verwaltung** > **Agents** aus.

2. Wählen Sie im Anzeigebereich die Clientcomputer aus, für die Sie den Schutz-Agent aktualisieren möchten.

  > [!NOTE]
  > Die Spalte **Agent-Updates** gibt an, wann ein Update des Schutz-Agents für jeden geschützten Computer verfügbar ist. Im Bereich **Aktionen** ist die Aktion **Aktualisieren** nur verfügbar, wenn ein geschützter Computer ausgewählt ist und Updates verfügbar sind.

3. Zum Installieren aktualisierter Schutz-Agents auf den ausgewählten Computern wählen Sie im Bereich **Aktionen** den Befehl **Aktualisieren** aus.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Aktualisieren eines Schutz-Agents auf einem Clientcomputer, der nicht mit dem Netzwerk verbunden ist

1. Wählen Sie in der Backup Server-Administratorkonsole **Verwaltung** > **Agents** aus.

2. Wählen Sie im Anzeigebereich die Clientcomputer aus, für die Sie den Schutz-Agent aktualisieren möchten.

  > [!NOTE]
  > Die Spalte **Agent-Updates** gibt an, wann ein Update des Schutz-Agents für jeden geschützten Computer verfügbar ist. Im Bereich **Aktionen** ist die Aktion **Aktualisieren** nicht verfügbar, wenn ein geschützter Computer ausgewählt ist, es sei denn, Updates sind verfügbar.

3. Zum Installieren aktualisierter Schutz-Agents auf den ausgewählten Computern wählen Sie **Aktualisieren** aus.

4. Für einen Clientcomputer, der nicht mit dem Netzwerk verbunden ist, enthält die Spalte **Agent-Status** so lange den Status **Update ausstehend**, bis der Computer mit dem Netzwerk verbunden wird.

  Wenn ein Clientcomputer mit dem Netzwerk verbunden ist, enthält die Spalte **Agent-Updates** für den Clientcomputer den Status **Wird aktualisiert**.
  
### <a name="move-legacy-protection-groups-from-the-old-version-and-sync-the-new-version-with-azure"></a>Verschieben von älteren Schutzgruppen aus der alten Version und Synchronisieren der neuen Version mit Azure

Nachdem sowohl Azure Backup Server als auch das Betriebssystem aktualisiert wurden, können Sie neue Datenquellen per Modern Backup Storage schützen. Datenquellen, die bereits geschützt, sind weiterhin geschützt, da sie sich in Azure Backup Server (Legacy) befanden. Alle neuen Schutzgruppen verwenden Modern Backup Storage.

Um Datenquellen aus dem Legacyschutzmodus zu Modern Backup Storage zu migrieren, gehen Sie wie folgt vor:

1.  Fügen Sie die neuen Volumes dem Data Protection Manager-Speicherpool hinzu. Sie können auch einen Anzeigenamen zuweisen und Datenquelltags auswählen.

2. Beenden Sie für alle Datenquellen, die sich im Legacymodus befinden, den Schutz der Datenquellen. Wählen Sie dann **Geschützte Daten beibehalten** aus.  Dies ermöglicht die Wiederherstellung alter Wiederherstellungspunkte nach der Migration.

3. Erstellen Sie eine neue Schutzgruppe. Wählen Sie dann die Datenquellen aus, die im neuen Format gespeichert werden sollen.

  Data Protection Manager speichert lokal eine Replikatkopie aus dem Legacysicherungsspeicher auf dem Modern Backup Storage-Volume.
    > [!NOTE] 
    > Die Erstellung der Kopie wird als Auftrag nach dem Wiederherstellungsvorgang angezeigt.

  Alle neuen Synchronisierungs- und Wiederherstellungspunkte werden dann in Modern Backup Storage gespeichert.

  Alte Wiederherstellungspunkte werden gelöscht, wenn sie abgelaufen sind. Durch das Löschen alter Wiederherstellungspunkte wird Speicherplatz auf dem Datenträger freigegeben.

  Wenn alle Legacyvolumes aus dem alten Speicher gelöscht wurden, können Sie den Datenträger aus Azure Backup entfernen. Anschließend können Sie den Datenträger aus dem System entfernen.

4. Erstellen Sie eine Sicherung der Data Protection Manager-Datenbank.

  > [!IMPORTANT]
  > - Der neue Servername muss den gleichen Namen wie für die ursprüngliche Azure Backup Server-Instanz aufweisen. Der Name der neuen Azure Backup Server-Instanz kann nicht geändert werden, wenn Sie die Wiederherstellungspunkte über den vorherigen Speicherpool und der Data Protection Manager-Datenbank speichern möchten.
  > - Sie müssen über eine Sicherung der Data Protection Manager-Datenbank verfügen. Sie müssen die Datenbank wiederherstellen.

5. Fahren Sie die ursprüngliche Azure Backup Server-Instanz herunter, oder schalten Sie den Server offline.

6. Setzen Sie das Computerkonto in Active Directory zurück.

7. Installieren Sie Windows Server 2016 auf einem neuen Computer. Verwenden Sie als Servernamen den gleichen Computernamen wie für die ursprüngliche Azure Backup Server-Instanz.

8. Führen Sie den Domänenbeitritt durch.

9. Installieren Sie Azure Backup Server v2. (Entfernen Sie die Datenträger für Data Protection Manager-Speicherpools aus dem alten Server, und importieren Sie sie auf den neuen Server.)

10. Stellen Sie die Data Protection Manager-Datenbank wieder her, die Sie in Schritt 4 erstellt haben.

11. Fügen Sie den Speicher vom ursprünglichen Sicherungsserver an den neuen Server an.

12. Stellen Sie in SQL Server die Data Protection Manager-Datenbank wieder her.

13. Verwenden Sie in der Administratorbefehlszeile auf dem neuen Server `cd`, um zum Microsoft Azure Backup-Installationsordner und dem Ordner „Bin“ zu navigieren.  

  Beispiel:  
  C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\ zu Azure Backup

14. Führen Sie `DPMSYNC -SYNC`aus.
  
  > [!NOTE]
  > Wenn Sie anstelle der alten Datenträger *neue* Datenträger zum Data Protection Manager-Speicherpool hinzugefügt haben, führen Sie `DPMSYNC -Reallocatereplica` aus.

## <a name="new-powershell-cmdlets-in-backup-server-v2"></a>Neue PowerShell-Cmdlets in Backup Server v2

Nach der Installation von Azure Backup Server v2 stehen zwei neue Cmdlets zur Verfügung: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihren Server vorbereiten oder mit dem Schutz einer Workload beginnen:
- [Vorbereiten von Backup Server-Workloads](backup-azure-microsoft-azure-backup.md)
- [Sichern eines VMware-Servers mit Backup Server](backup-azure-backup-server-vmware.md)
- [Sichern von SQL Server mit Backup Server](backup-azure-sql-mabs.md)
- [Verwenden von Modern Backup Storage mit Backup Server](backup-mabs-add-storage.md)

