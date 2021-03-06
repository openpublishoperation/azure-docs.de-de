---
title: Behandeln von Problemen bei Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über bekannte Probleme im Azure Migrate-Dienst und Problembehandlungstipps für häufige Fehler.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/28/2018
ms.author: raynew
ms.openlocfilehash: 906c6e56b670dfc26b5905a453fd43a3c72086c3
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433496"
---
# <a name="troubleshoot-azure-migrate"></a>Problembehandlung für Azure Migrate

## <a name="troubleshoot-common-errors"></a>Problembehandlung für häufige Fehler

[Azure Migrate](migrate-overview.md) bewertet lokale Workloads für die Migration zu Azure. Verwenden Sie diesen Artikel zur Problembehandlung bei der Bereitstellung und Verwendung von Azure Migrate.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Erstellen des Migrationsprojekts mit dem Fehler *Anforderungen müssen Benutzeridentitätsheader enthalten*

Dieses Problem kann für Benutzer auftreten, die nicht über Zugriff auf den Azure Active Directory (Azure AD)-Mandanten der Organisation verfügen. Wenn ein Benutzer zum ersten Mal zu einem Azure AD-Mandanten hinzugefügt wird, erhält er eine E-Mail-Einladung, dem Mandanten beizutreten. Benutzer müssen die E-Mail aufrufen und die Einladung annehmen, um erfolgreich zum Mandanten hinzugefügt zu werden. Wenn Sie die E-Mail nicht sehen können, wenden Sie sich an einen Benutzer, der bereits auf den Mandanten zugreifen kann, und bitten Sie ihn, die Einladung anhand der [hier](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users) angegebenen Schritte erneut zu senden.

Wenn Sie die Einladung per E-Mail erhalten haben, öffnen Sie die diese und klicken Sie auf den darin angegebenen Link, um die Einladung anzunehmen. Sobald dies geschehen ist, müssen Sie sich beim Azure-Portal ab- und erneut anmelden. Das Aktualisieren des Browsers allein funktioniert nicht. Sie können dann versuchen, das Migrationsprojekt zu erstellen.

### <a name="performance-data-for-disks-and-networks-adapters-shows-as-zeros"></a>Leistungsdaten für Datenträger und Netzwerkadapter werden als NULL-Werte angezeigt

Dies kann auftreten, wenn die Einstellungsebene für Statistiken dem vCenter-Server auf weniger als drei festgelegt ist. Ab Ebene 3 speichert vCenter VM den Leistungsverlauf virtueller Computer für Compute, Speicher und Netzwerk. Für Ebenen unter drei speichert vCenter keine Speicher- und Netzwerkdaten, sondern nur CPU- und Arbeitsspeicherdaten. In diesem Szenario werden Leistungsdaten in Azure Migrate als NULL angezeigt, und Azure Migrate stellt Größenempfehlungen für Datenträger und Netzwerke basierend auf den Metadaten bereit, die auf den lokalen Computern erfasst wurden.

Zum Aktivieren der Erfassung von Datenträger- und Netzwerkleistungsdaten ändern Sie die Einstellungsebene für Statistiken zu drei. Warten Sie dann mindestens einen Tag vor dem Ermitteln und Bewerten ihrer Umgebung.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ich habe Agents installiert und die Visualisierung von Abhängigkeiten verwendet, um Gruppen zu erstellen. Jetzt zeigen die Computer nach dem Failover die Aktion „Agent installieren“ anstelle von „Abhängigkeiten anzeigen“ an.
* Nach einem geplanten oder ungeplanten Failover werden lokale Computer abgeschaltet, und entsprechende Computer in Azure werden hochgefahren. Diese Computer beziehen eine andere MAC-Adresse. Je nachdem, ob der Benutzer die lokale IP-Adresse beibehalten möchte oder nicht, beziehen sie möglicherweise auch eine andere IP-Adresse. Wenn sich sowohl MAC- als auch IP-Adressen unterscheiden, ordnet Azure Migrate die lokalen Computern keinen Dienstzuordnungs-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.
* Nach dem Testfailover bleiben die lokalen Computer erwartungsgemäß eingeschaltet. Entsprechende Computer, die in Azure hochgefahren wurden, erhalten andere MAC-Adressen und u.U. auch andere IP-Adressen. Sofern der Benutzer ausgehenden Log Analytics-Datenverkehr von diesen Computern nicht sperrt, ordnet Azure Migrate die lokalen Computer keinen Dienstzuordnungs-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.

### <a name="i-specified-an-azure-geography-while-creating-a-migration-project-how-do-i-find-out-the-exact-azure-region-where-the-discovered-metadata-would-be-stored"></a>Ich habe beim Erstellen eines Migrationsprojekts eine Azure-Geografie angegeben. Wie ermittle ich die exakte Azure-Region, in der die erkannten Metadaten gespeichert werden?

Sie können zum Abschnitt **Grundlagen** auf der Seite **Übersicht** des Projekts navigieren, um den exakten Speicherort der Metadaten zu ermitteln. Der Speicherort wird von Azure Migrate zufällig innerhalb der Geografie ausgewählt und kann nicht geändert werden. Wenn Sie ein Projekt nur in einer bestimmten Region erstellen möchten, können Sie das Migrationsprojekt mit den REST-APIs erstellen und die gewünschte Region übergeben.

   ![Projektspeicherort](./media/troubleshooting-general/geography-location.png)

## <a name="collector-errors"></a>Collector-Fehler

### <a name="deployment-of-collector-ova-failed"></a>Bereitstellung der Collector-OVA fehlgeschlagen

Dies kann passieren, wenn die OVA nur teilweise heruntergeladen wurde. Der Grund kann aber auch der Browser sein, wenn Sie den vSphere-Webclient zur OVA-Bereitstellung verwenden. Stellen Sie sicher, dass der Download abgeschlossen ist, und wiederholen Sie die OVA-Bereitstellung mit einem anderen Browser.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Collector kann keine Verbindung mit dem Internet herstellen

Dies kann passieren, wenn sich der Computer, den Sie verwenden, hinter einem Proxy befindet. Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
Wenn Sie URL-basierte Firewallproxys zur Steuerung ausgehender Verbindungen verwenden, stellen Sie sicher, dass die folgenden erforderlichen Dienst-URLs zur Whitelist hinzugefügt werden:

**URL** | **Zweck**  
--- | ---
*.portal.azure.com | Erforderlich, um die Konnektivität mit dem Azure-Dienst zu überprüfen und Probleme mit der Zeitsynchronisation zu überprüfen.
*.oneget.org | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen.

**Der Collector kann aufgrund von Fehlern bei der Zertifikatüberprüfung keine Internetverbindung herstellen.**

Dieses Problem kann auftreten, wenn Sie einen abfangenden Proxy verwenden, um eine Internetverbindung herzustellen, und auf dem virtuellen Collectorcomputer das Proxyzertifikat nicht importiert haben. Eine Anleitung zum Importieren des Proxyzertifikats finden Sie [hier](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity).

**Der Collector kann mit der Projekt-ID und dem Schlüssel, die ich aus dem Portal kopiert habe, keine Verbindung mit dem Projekt herstellen.**

Stellen Sie sicher, dass Sie die richtigen Informationen kopiert und eingefügt haben. Installieren Sie zur Problembehandlung Microsoft Monitoring Agent (MMA), und vergewissern Sie sich, dass MMA eine Verbindung mit dem Projekt herstellen kann. Gehen Sie dazu wie folgt vor:

1. Laden Sie [MMA](https://go.microsoft.com/fwlink/?LinkId=828603) auf den virtuellen Collector-Computer herunter.
2. Doppelklicken Sie auf die heruntergeladenen Datei, um die Installation zu starten.
3. Klicken Sie im Setup auf der Seite **Willkommen** auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
4. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, oder ändern Sie ihn, und klicken Sie anschließend auf **Weiter**.
5. Wählen Sie unter **Agent-Setupoptionen** die Optionen **Azure Log Analytics** > **Weiter**.
6. Klicken Sie auf **Hinzufügen**, um einen neuen Log Analytics-Arbeitsbereich hinzuzufügen. Fügen Sie die Projekt-ID und den Schlüssel ein, die Sie kopiert haben. Klicken Sie auf **Weiter**.
7. Stellen Sie sicher, dass der Agent eine Verbindung mit dem Projekt herstellen kann. Ist dies nicht der Fall, überprüfen Sie die Einstellungen. Wenn der Agent eine Verbindung herstellen kann, der Collector jedoch nicht, wenden Sie sich an den Support.


### <a name="error-802-date-and-time-synchronization-error"></a>Fehler 802: Fehler bei der Datums- und Zeitsynchronisierung

Möglicherweise weicht die Serveruhr um mehr als fünf Minuten von der aktuellen Uhrzeit ab. Passen Sie die Uhrzeit auf dem virtuellen Collector-Computer folgendermaßen an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
2. Führen Sie „w32tm /tz“ aus, um die Zeitzone zu überprüfen.
3. Führen Sie „w32tm /resync“ aus, um die Zeit zu synchronisieren.

### <a name="vmware-powercli-installation-failed"></a>VMware PowerCLI-Installation fehlgeschlagen

Azure Migrate-Collector lädt PowerCLI herunter und installiert sie auf der Appliance. Ein Fehler bei der PowerCLI-Installation kann durch nicht erreichbare Endpunkte für das PowerCLI-Repository verursacht werden. Um das Problem zu beheben, versuchen Sie, PowerCLI anhand der folgenden Schritte manuell auf der Collector-VM zu installieren:

1. Öffnen Sie Windows PowerShell im Administratormodus.
2. Wechseln Sie zum Verzeichnis „C:\Programme\ProfilerService\VMWare\Scripts\“.
3. Führen Sie das Skript „InstallPowerCLI.ps1“ aus.

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Fehler: UnhandledException Interner Fehler: System.IO.FileNotFoundException

Dieses Problem kann aufgrund eines Problems mit der VMware PowerCLI-Installation auftreten. Führen Sie die folgenden Schritte aus, um das Problem zu beheben:

1. Falls Sie nicht die aktuelle Version der Collectorappliance verwenden, aktualisieren Sie Ihren Collector auf die [aktuelle Version](https://aka.ms/migrate/col/checkforupdates), und überprüfen Sie, ob das Problem dadurch behoben wurde.
2. Wenn Sie bereits über die aktuelle Version von Collector verfügen, installieren Sie [VMware PowerCLI 6.5.2](https://www.powershellgallery.com/packages/VMware.PowerCLI/6.5.2.6268016) manuell, und überprüfen Sie, ob das Problem dadurch behoben wurde.
3. Wenn mit den weiter oben beschriebenen Schritten das Problem nicht behoben wurde, navigieren Sie zum Ordner „C:\Programme\ProfilerService“, und entfernen Sie die im Ordner vorhandenen Dateien „VMware.dll“ und „VimService65.dll“. Starten Sie anschließend den Dienst „Azure Migrate-Collector“ in Windows Service Manager neu. (Öffnen Sie hierzu „Ausführen“, und geben Sie „services.msc“ ein, um Windows Service Manager aufzurufen.)

### <a name="error-unabletoconnecttoserver"></a>Fehler: UnableToConnectToServer

Aufgrund eines Fehlers konnte keine Verbindung mit vCenter Server „Servername.com:9443“ hergestellt werden: Unter „https://Servername.com:9443/sdk“ hat kein Endpunkt gelauscht, der die Nachricht akzeptieren konnte.

Überprüfen Sie, ob die neueste Version der Collectorappliance ausgeführt wird. Wenn das nicht der Fall ist, aktualisieren Sie auf die [neueste Version](https://docs.microsoft.com/azure/migrate/concepts-collector#how-to-upgrade-collector).

Wenn das Problem auch bei der neuesten Version auftritt, kann der Collectorcomputer den angegebenen vCenter Server-Namen möglicherweise nicht auflösen oder der angegebene Port ist falsch. Wenn der Port nicht angegeben ist, versucht Collector standardmäßig, eine Verbindung mit Port 443 herzustellen.

1. Versuchen Sie, „Servername.com“ vom Collectorcomputer aus per Ping zu erreichen.
2. Wenn Schritt 1 nicht erfolgreich ist, können Sie versuchen, über die IP-Adresse eine Verbindung mit vCenter Server herzustellen.
3. Ermitteln Sie die richtige Portnummer für die Verbindungsherstellung mit vCenter.
4. Überprüfen Sie abschließend, ob vCenter Server ausgeführt wird und betriebsbereit ist.

## <a name="troubleshoot-dependency-visualization-issues"></a>Behandlung von Problemen bei der Abhängigkeitsvisualisierung

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Ich habe Microsoft Monitoring Agent (MMA) und den Dependency-Agent auf meinen lokalen virtuellen Computern installiert. Jetzt werden die Abhängigkeiten jedoch im Azure Migrate-Portal angezeigt.

Nachdem Sie die Agents installiert haben, benötigt Azure Migrate üblicherweise 15 bis 30 Minuten, um die Abhängigkeiten im Portal anzuzeigen. Wenn Sie bereits länger als 30 Minuten warten, stellen Sie sicher, dass MMA mit dem OMS-Arbeitsbereich kommunizieren kann. Gehen Sie hierzu wie folgt vor:

Bei einer virtuellen Windows-VM:
1. Wechseln Sie zur **Systemsteuerung**, und starten Sie **Microsoft Monitoring Agent**
2. Rufen Sie im Popup zu den MMA-Eigenschaften die Registerkarte **Azure Log Analytics (OMS)** auf.
3. Stellen Sie sicher, dass der **Status** für den Arbeitsbereich grün angezeigt wird.
4. Wenn der Status nicht grün angezeigt wird, entfernen Sie den Arbeitsbereich, und fügen Sie ihn zu MMA neu hinzu.
        ![MMA-Status](./media/troubleshooting-general/mma-status.png)

Überprüfen Sie bei einer Linux-VM, ob die Installationsbefehle für MMA und den Dependency-Agent erfolgreich ausgeführt wurden.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>Welche Betriebssysteme werden von MMA unterstützt?

Die Liste der von MMA unterstützten Windows-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
Die Liste der von MMA unterstützten Linux-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Welche Betriebssysteme werden vom Dependency-Agent unterstützt?

Die Liste der vom Dependency-Agent unterstützten Windows-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
Die Liste der vom Dependency-Agent unterstützten Linux-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Ich kann Abhängigkeiten in Azure Migrate nicht länger als eine Stunde visualisieren.
Azure Migrate ermöglicht Ihnen das Visualisieren von Abhängigkeiten für einen Zeitraum von bis zu einer Stunde. Mit Azure Migrate können Sie zwar um bis zu einen Monat zu einem bestimmten Datum im Verlauf zurückgehen. Die Abhängigkeiten können Sie jedoch nur für einen Zeitraum von maximal einer Stunde visualisieren. So können Sie beispielsweise mithilfe der Zeitraumfunktionalität im Abhängigkeitsdiagramm Abhängigkeiten für gestern, jedoch nur für ein Zeitfenster von einer Stunde anzeigen.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>Für Gruppen mit mehr als 10 virtuellen Computern kann ich keine Abhängigkeiten visualisieren.
Sie können [Abhängigkeiten für Gruppen visualisieren](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies), die aus bis zu zehn VMs bestehen. Falls eine Gruppe aus mehr als zehn VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.

## <a name="troubleshoot-readiness-issues"></a>Behandeln von Problemen bei der Bereitschaft

**Problem** | **Behebung**
--- | ---
Nicht unterstützter Starttyp | Azure unterstützt keine virtuellen Computer mit dem Starttyp „EFI“. Vor einer Migration muss der Starttyp in „BIOS“ konvertiert werden. <br/><br/>Die entsprechenden virtuellen Computer können mit [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) migriert werden, da Azure Site Recovery den Starttyp des virtuellen Computers im Rahmen der Migration in „BIOS“ konvertiert.
Bedingt unterstütztes Windows-Betriebssystem | Der Unterstützungszeitraum für das Betriebssystem ist abgelaufen. Für die [Unterstützung in Azure](https://aka.ms/WSosstatement) wird eine benutzerdefinierte Supportvereinbarung (Custom Support Agreement, CSA) benötigt. Führen Sie vor der Migration ggf. ein Upgrade des Betriebssystems durch.
Nicht unterstütztes Windows-Betriebssystem | Azure unterstützt nur [bestimmte Windows-Betriebssystemversionen](https://aka.ms/WSosstatement). Führen Sie vor der Migration zu Azure ggf. ein Upgrade des Betriebssystems durch.
Bedingt unterstütztes Linux-Betriebssystem | Azure unterstützt nur [bestimmte Linux-Betriebssystemversionen](../virtual-machines/linux/endorsed-distros.md). Führen Sie vor der Migration zu Azure ggf. ein Upgrade des Betriebssystems durch.
Nicht unterstütztes Linux-Betriebssystem | Der Computer startet zwar unter Umständen in Azure, das Betriebssystem wird von Azure jedoch nicht unterstützt. Führen Sie vor der Migration zu Azure ggf. ein Upgrade auf eine [unterstützte Linux-Version](../virtual-machines/linux/endorsed-distros.md) durch.
Unbekanntes Betriebssystem | Das Betriebssystem des virtuellen Computers wurde in vCenter Server als „Anderes“ angegeben, und Azure Migrate kann deshalb nicht beurteilen, ob der virtuelle Computer für Azure bereit ist. Vergewissern Sie sich, dass das Betriebssystem auf dem Computer von Azure [unterstützt](https://aka.ms/azureoslist) wird, bevor Sie den Computer migrieren.
Nicht unterstützte Bitanzahl für das Betriebssystem | Virtuelle Computer mit 32-Bit-Betriebssystem starten zwar unter Umständen in Azure, es empfiehlt sich jedoch, vor der Migration zu Azure ein Betriebssystemupgrade auf die 64-Bit-Version durchzuführen.
Erfordert Visual Studio-Abonnement. | Auf dem Computer wird ein Windows-Clientbetriebssystem ausgeführt. Dies wird nur in Visual Studio-Abonnements unterstützt.
Kein virtueller Computer für erforderliche Speicherleistung gefunden. | Die erforderliche Speicherleistung (IOPS/Durchsatz) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie vor der Migration die Speicheranforderungen für den Computer.
Kein virtueller Computer für erforderliche Netzwerkleistung gefunden. | Die erforderliche Netzwerkleistung (ein-/ausgehend) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie die Netzwerkanforderungen für den Computer.
Für den angegebenen Tarif wurde kein virtueller Computer gefunden. | Wenn der Tarif auf „Standard“ festgelegt ist, sollten Sie den virtuellen Computer vor der Migration ggf. herabstufen. Ist der Tarif auf „Basic“ festgelegt, empfiehlt es sich unter Umständen, den Tarif der Bewertung in „Standard“ zu ändern.
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Mindestens ein Datenträger ist ungeeignet. | Mindestens ein mit der VM verbundener Datenträger erfüllt nicht die Azure-Anforderungen. Stellen Sie für jeden mit der VM verbundenen Datenträger sicher, dass die Größe des Datenträgers kleiner als 4 TB ist. Anderenfalls verkleinern Sie die Datenträgergröße, bevor Sie zu Azure migrieren. Stellen Sie sicher, dass die von jedem Datenträger benötigte Leistung (IOPS/Durchsatz) von [Azure-Datenträgern verwalteter virtueller Computer](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) unterstützt wird.   
Mindestens ein Netzwerkadapter ist ungeeignet. | Entfernen Sie nicht verwendete Netzwerkadapter vor der Migration vom Computer.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Azure unterstützt Datenträger mit einer Größe von bis zu 4 TB. Verkleinern Sie Datenträger vor der Migration auf weniger als 4 TB.
Datenträger am angegebenen Speicherort nicht verfügbar | Stellen Sie vor der Migration sicher, dass sich der Datenträger am Zielspeicherort befindet.
Datenträger für die angegebene Redundanz nicht verfügbar | Der Datenträger muss den in den Bewertungseinstellungen definierten Redundanzspeichertyp verwenden (standardmäßig LRS).
Datenträgereignung konnte aufgrund eines internen Fehlers nicht ermittelt werden | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Kein virtueller Computer gefunden, der die Kern- und Arbeitsspeicheranforderungen erfüllt | Azure konnte keinen geeigneten VM-Typ finden. Reduzieren Sie vor der Migration den Arbeitsspeicher und die Anzahl der Kerne auf dem lokalen Computer.
Eignung des virtuellen Computers konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Datenträger aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Netzwerkadapter aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.


## <a name="collect-logs"></a>Erfassen von Protokollen

**Wie erfasse ich Protokolle auf dem virtuellen Collector-Computer?**

Die Protokollierung ist standardmäßig aktiviert. Die Protokolle befinden sich an folgenden Speicherorten:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Führen Sie folgende Schritte aus, um die Ereignisablaufverfolgung für Windows zu erfassen:

1. Öffnen Sie auf dem virtuellen Collector-Computer ein PowerShell-Befehlsfenster.
2. Führen Sie **Get-EventLog -LogName Application | export-csv eventlog.csv** aus.

**Wie erfasse ich im Portal Protokolle des Netzwerkdatenverkehrs?**

1. Öffnen Sie den Browser, navigieren Sie zum [Portal](https://portal.azure.com), und melden Sie sich an.
2. Drücken Sie F12, um die Entwicklertools zu starten. Deaktivieren Sie ggf. die Einstellung **Einträge beim Navigieren löschen**.
3. Klicken Sie auf die Registerkarte **Netzwerk**, und starten Sie die Erfassung von Netzwerkdatenverkehr:
 - Wählen Sie in Chrome die Option **Protokoll speichern**. Die Aufzeichnung sollte automatisch gestartet werden. Ein roter Kreis gibt an, dass der Datenverkehr erfasst wird. Wenn er nicht angezeigt wird, klicken Sie zum Starten auf den schwarzen Kreis.
 - In Edge/IE sollte die Aufzeichnung automatisch gestartet werden. Ist dies nicht der Fall, klicken Sie auf die grüne Wiedergabeschaltfläche.
4. Versuchen Sie, den Fehler zu reproduzieren.
5. Nachdem der Fehler während der Aufzeichnung aufgetreten ist, beenden Sie die Aufzeichnung, und speichern Sie eine Kopie der aufgezeichneten Aktivität:
 - Klicken Sie in Chrome mit der rechten Maustaste, und klicken Sie dann auf **Als HAR mit Inhalt speichern**. Daraufhin werden die Protokolle komprimiert und als HAR-Datei exportiert.
 - In Edge/IE klicken Sie auf das Symbol **Aufgezeichneten Datenverkehr exportieren**. Daraufhin werden die Protokolle komprimiert und exportiert.
6. Navigieren Sie zur Registerkarte **Konsole**, um sie auf Warnungen oder Fehler zu prüfen. So speichern Sie das Konsolenprotokoll:
 - Klicken Sie in Chrome mit der rechten Maustaste auf eine beliebige Stelle im Konsolenprotokoll. Wählen Sie **Speichern als**, um das Protokoll zu exportieren und zu komprimieren.
 - Klicken Sie in Edge/IE mit der rechten Maustaste auf die Fehler, und wählen Sie **Alle kopieren**.
7. Schließen Sie die Entwicklertools.

## <a name="collector-error-codes-and-recommended-actions"></a>Fehlercodes des Collectors und empfohlene Aktionen

|           |                                |                                                                               |                                                                                                       |                                                                                                                                            |
|-----------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Fehlercode | Fehlerbezeichnung                      | Message                                                                       | Mögliche Ursachen                                                                                        | Empfohlene Maßnahme                                                                                                                          |
| 601       | CollectorExpired               | Der Collector ist abgelaufen.                                                        | Collector abgelaufen.                                                                                    | Laden Sie eine neue Version des Collectors herunter, und wiederholen Sie den Vorgang.                                                                                      |
| 751       | UnableToConnectToServer        | Fehler bei der Verbindungsherstellung mit vCenter Server-Instanz „%Name;“. Fehler: %ErrorMessage;     | Weitere Informationen finden Sie in der Fehlermeldung.                                                             | Beheben Sie das Problem, und wiederholen Sie den Vorgang.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | Der Server „%Name;“ ist keine vCenter Server-Instanz.                                  | Geben Sie vCenter Server-Details an.                                                                       | Wiederholen Sie den Vorgang mit den richtigen vCenter Server-Details.                                                                                   |
| 753       | InvalidLoginCredentials        | Fehler bei der Verbindungsherstellung mit vCenter Server-Instanz „%Name;“. Fehler: %ErrorMessage; | Fehler bei der Verbindung mit der vCenter Server-Instanz aufgrund von ungültigen Anmeldeinformationen.                             | Stellen Sie sicher, dass die angegebenen Anmeldeinformationen richtig sind.                                                                                    |
| 754       | NoPerfDataAvaialable           | Keine Leistungsdaten verfügbar.                                               | Überprüfen Sie die Statistikstufe in vCenter Server. Sie muss auf „3“ gesetzt sein, damit Leistungsdaten verfügbar sind. | Ändern Sie die Statistikstufe auf „3“ (für eine Dauer von 5 Minuten, 30 Minuten und 2 Stunden), und versuchen Sie es erneut, nachdem Sie mindestens einen Tag gewartet haben.                   |
| 756       | NullInstanceUUID               | Einen Computer mit InstanceUUID „NULL“ gefunden.                                  | Ein Objekt in der vCenter Server-Instanz ist möglicherweise nicht geeignet.                                                      | Beheben Sie das Problem, und wiederholen Sie den Vorgang.                                                                                                           |
| 757       | VMNotFound                     | Virtueller Computer wurde nicht gefunden.                                                  | Virtueller Computer wurde möglicherweise gelöscht: %VMID;                                                                | Stellen Sie sicher, dass die virtuellen Computer, die beim Scannen des vCenter-Bestands ausgewählt wurden, während der Ermittlung vorhanden sind.                                      |
| 758       | GetPerfDataTimeout             | Timeout bei vCenter-Anforderung. Meldung: %Message;                                  | Die vCenter Server-Anmeldeinformationen sind falsch.                                                              | Überprüfen Sie die vCenter Server-Anmeldeinformationen, und stellen Sie sicher, dass vCenter Server erreichbar ist. Wiederholen Sie den Vorgang. Wenden Sie sich an den Support, wenn das Problem weiterhin besteht. |
| 759       | VmwareDllNotFound              | VMWare.Vim-DLL wurde nicht gefunden.                                                     | PowerCLI wurde nicht ordnungsgemäß installiert.                                                                   | Überprüfen Sie, ob PowerCLI korrekt installiert ist. Wiederholen Sie den Vorgang. Wenden Sie sich an den Support, wenn das Problem weiterhin besteht.                               |
| 800       | ServiceError                   | Der Dienst „Azure Migrate-Collector“ wird nicht ausgeführt.                               | Der Dienst „Azure Migrate-Collector“ wird nicht ausgeführt.                                                       | Verwenden Sie „Services.msc“, um den Dienst zu starten, und wiederholen den Vorgang.                                                                             |
| 801       | PowerCLIError                  | Fehler bei der VMware PowerCLI-Installation.                                          | Fehler bei der VMware PowerCLI-Installation.                                                                  | Wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, führen Sie die Installation manuell aus und wiederholen den Vorgang.                                                   |
| 802       | TimeSyncError                  | Die Uhrzeit ist nicht mit dem Internetzeitserver synchronisiert.                            | Die Uhrzeit ist nicht mit dem Internetzeitserver synchronisiert.                                                    | Stellen Sie sicher, dass die Uhrzeit auf dem Computer genau auf die Zeitzone des Computers eingestellt ist, und wiederholen Sie den Vorgang.                                 |
| 702       | OMSInvalidProjectKey           | Ungültiger Projektschlüssel angegeben.                                                | Ungültiger Projektschlüssel angegeben.                                                                        | Wiederholen Sie den Vorgang mit dem richtigen Projektschlüssel.                                                                                              |
| 703       | OMSHttpRequestException        | Fehler beim Senden der Anforderung. Meldung: %Message;                                | Überprüfen Sie die Projekt-ID und den Projektschlüssel, und stellen Sie sicher, dass der Endpunkt erreichbar ist.                                       | Wiederholen Sie den Vorgang. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht.                                                                     |
| 704       | OMSHttpRequestTimeoutException | Timeout der HTTP-Anforderung. Meldung: %Message;                                     | Überprüfen Sie die Projekt-ID und den Projektschlüssel, und stellen Sie sicher, dass der Endpunkt erreichbar ist.                                       | Wiederholen Sie den Vorgang. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht.                                                                     |
