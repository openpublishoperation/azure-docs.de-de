---
title: Migrieren von BizTalk Server-EDI-Lösungen zu BizTalk Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Ihre Microsoft BizTalk Server-EDI-Lösungen zu Microsoft Azure BizTalk Services (MABS) migrieren.
services: biztalk-services
ms.service: biztalk-services
author: jonfancey
ms.author: jonfan
manager: jeconnoc
ms.topic: article
ms.date: 07/31/2018
ms.reviewer: jonfan, LADocs
ms.suite: integration
ms.openlocfilehash: 681cafc60661e16d70deb862da71f6baf80509fd
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856529"
---
# <a name="migrate-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migrieren von BizTalk Server-EDI-Lösungen zu BizTalk Services: Technische Anleitung

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Autoren: Tim Wieman und Nitin Mehrotra

Bearbeitung: Karthik Bharthy

Geschrieben mit: Microsoft Azure BizTalk Services (Version vom Februar 2014).

## <a name="introduction"></a>Einführung
EDI (Electronic Data Interchange) ist eines der am häufigsten verwendeten Mittel für den elektronischen Datenaustausch von Unternehmen. Dieser Austausch wird auch als Business-to-Business-Transaktionen (oder B2B-Transaktionen) bezeichnet. EDI wird von BizTalk Server bereits seit der ersten Version (und damit seit mehr als zehn Jahren) unterstützt. Mit BizTalk Services setzt Microsoft die Unterstützung von EDI-Lösungen auf der Microsoft Azure Platform fort. B2B-Transaktionen finden größtenteils außerhalb einer Organisation statt und lassen sich daher einfacher über eine Cloudplattform implementieren. Microsoft Azure bietet diese Möglichkeit über BizTalk Services.

Für einige Kunden ist BizTalk Services zwar eine „jungfräuliche“ Plattform für neue EDI-Lösungen, aber viele Kunden verfügen bereits über BizTalk Server-EDI-Lösungen, die sie ggf. zu Azure migrieren möchten. Da die EDI-Architektur von BizTalk Services auf den gleichen zentralen Entitäten basiert wie die EDI-Architektur von BizTalk Server (Handelspartner, Entitäten, Vereinbarungen), ist es möglich, BizTalk Server-EDI-Artefakte zu BizTalk Services zu migrieren.

Dieses Dokument geht auf einige der Unterschiede bei der Migration von BizTalk Server-EDI-Artefakten zu BizTalk Services ein. Dabei wird vorausgesetzt, dass Sie angemessen mit der EDI-Verarbeitung von BizTalk Server sowie mit Handelspartnerverträgen vertraut sind. Weitere Informationen zu EDI in BizTalk Server finden Sie unter [Handelspartnerverwaltung mit BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welche Version von BizTalk Server EDI-Artefakten kann zu BizTalk Services migriert werden?
Das BizTalk Server-EDI-Modul wurde für BizTalk Server 2010 erheblich verbessert und um Partner, Profile und Vereinbarungen erweitert. BizTalk Services verwendet das gleiche Modell, um die Handelspartner und die Geschäftsbereiche innerhalb dieser Handelspartner zu organisieren. Daher ist es deutlich einfacher, EDI-Artefakte aus BizTalk Server 2010 und höheren Versionen zu BizTalk Services zu migrieren. Wenn Sie EDI-Artefakte aus Versionen vor BizTalk Server 2010 migrieren möchten, müssen Sie zunächst ein Upgrade auf BizTalk Server 2010 durchführen und anschließend Ihre EDI-Artefakte zu BizTalk Services migrieren.

## <a name="scenariosmessage-flow"></a>Szenarien/Nachrichtenfluss
Die EDI-Verarbeitung in BizTalk Services basiert genau wie bei BizTalk Server auf einer Lösung für die Handelspartnerverwaltung (Trading Partner Management, TPM). Die TPM-Lösung umfasst folgende zentrale Komponenten:

* Handelspartner: die Organisation in einer B2B-Transaktion
* Profile: Geschäftsbereiche innerhalb eines Handelspartners
* Handelspartnerverträge (oder Vereinbarungen): die geschäftliche Vereinbarung zwischen zwei Partnern/Profilen

Die folgende Abbildung zeigt die Ähnlichkeiten und die Unterschiede zwischen einer BizTalk Server-EDI-Lösung und der BizTalk Services-EDI-Lösung:

![][EDImessageflow]

Die wichtigsten Unterschiede und Ähnlichkeiten zwischen einem EDI-Lösungsfluss in BizTalk Server und BizTalk Services:

* BizTalk Server verwendet eine EDIReceive-Pipeline, um EDI-Nachrichten zu empfangen, und eine EDISend-Pipeline, um EDI-Nachrichten zu senden. Analog dazu verwendet BizTalk Services eine EDI-Empfangs- und eine EDI-Sende-Bridge, um EDI-Nachrichten zu empfangen bzw. zu senden. In BizTalk Server werden die Pipelines einer Vereinbarung über Sende- oder Empfangsports zugeordnet. In BizTalk Services stellt die Vereinbarung selbst die Sende- oder Empfangs-Bridge dar.
* In BizTalk Server wird die EDI-Nachricht nach der Verarbeitung durch die EDIReceive-Pipeline in eine SQL Server-Datenbank ausgegeben. Daraufhin ruft die EDISend-Pipeline die Nachricht aus der SQL Server-Datenbank ab, verarbeitet sie und sendet sie anschließend an den Handelspartner.
  
    In BizTalk Services wird die EDI-Nachricht nach der Verarbeitung durch die EDI-Empfangs-Bridge an einen externen Prozess weitergeleitet. Der externe Prozess kann in Microsoft Azure oder lokal ausgeführt werden. Durch den externen Prozess wird die Nachricht an die EDI-Sende-Bridge weitergeleitet. Die Sende-Bridge ruft die Nachricht aber nicht selbsttätig ab. Nach der Verarbeitung der Nachricht leitet die EDI-Sende-Bridge die Nachricht an den Handelspartner weiter.

BizTalk Services bietet eine benutzerfreundliche Konfigurationsumgebung, in der Benutzer schnell und ohne Konfiguration von Microsoft Azure-Compute-Instanzen (Web- oder Workerrollen), Microsoft Azure SQL-Datenbanken oder Microsoft Azure-Speicherkonten eine B2B-Vereinbarung zwischen Handelspartnern erstellen und bereitstellen können. Bei komplexeren Szenarien müssen Workflows oder andere Arten der Dienstverarbeitung rund um einen Handelspartnervertrag (also vor oder nach der EDI-Bridgeverarbeitung des Handelspartnervertrags) eingebunden werden. Im Einzelnen umfasst die EDI-Nachrichtenverarbeitung in BizTalk Services folgende Ereignisse:

1. Eine EDI-Nachricht wird vom Handelspartner Fabrikam empfangen.  Für den EDI-Nachrichtenempfang von Handelspartnern unterstützt BizTalk Services Transportprotokolle wie FTP, SFTP, AS2 und HTTP/S.
2. Die empfangsseitige Verarbeitung des Handelspartnervertrags disassembliert die EDI-Nachricht in das XML-Format.  Die disassemblierte EDI-Nachricht (im XML-Format) kann an Service Bus-Endpunkte (etwa an einen Service Bus Relay-Endpunkt, an ein Service Bus-Thema, an eine Service Bus-Warteschlange oder an eine BizTalk Services-Bridge) weitergeleitet werden.
3. Die disassemblierten XML-Nachrichten können dann vom Endpunkt zur weiteren benutzerdefinierten Verarbeitung empfangen werden.  Die Endpunkte können von einer lokalen Komponente oder von einer Microsoft Azure-Compute-Instanz verarbeitet werden, um die Nachricht in einem Windows Workflow (WF)- oder Windows Communication Foundation (WCF)-Dienst weiterzuverarbeiten.
4. Die sendeseitige Verarbeitung des Handelspartnervertrags assembliert die XML-Nachricht im EDI-Format und sendet sie an den Handelspartner Contoso.  Für den Versand von EDI-Nachrichten an Handelspartner unterstützt BizTalk Services die gleichen Protokolle wie für den Empfang von EDI-Nachrichten.

Dieses Dokument enthält auch grundlegende Hinweise zur Migration einiger der verschiedenen BizTalk Server-EDI-Artefakte zu BizTalk Services.

## <a name="sendreceive-ports-to-trading-partners"></a>Sende-/Empfangsports für Handelspartner
In BizTalk Server werden Empfangsspeicherorte und Empfangsports zum Empfangen von EDI-/XML-Nachrichten von Handelspartnern sowie Sendeports zum Senden von EDI-/XML-Nachrichten an Handelspartner eingerichtet. Diese Ports werden anschließend mithilfe der BizTalk Server-Verwaltungskonsole in einem Handelspartnervertrag zusammengefasst. In BizTalk Services werden die Orte, an denen Sie Nachrichten von Handelspartnern empfangen und an Handelspartner senden, im BizTalk Services-Portal als Teil des Handelspartnervertrags (im Rahmen der Transporteinstellungen) konfiguriert.  Das Konzept von Sendeports und Empfangsspeicherorten gibt es in BizTalk Services also eigentlich nicht. Weitere Informationen finden Sie unter [Erstellen von Vereinbarungen](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pipelines (Bridges)
Pipelines sind bei EDI in BizTalk Server nachrichtenverarbeitende Entitäten, die je nach Bedarf der Anwendung auch mit benutzerdefinierter Logik für bestimmte Verarbeitungsfunktionen ausgestattet sein können. Das Äquivalent in BizTalk Services ist eine EDI-Bridge. Die EDI-Bridges in BizTalk Services sind jedoch vorerst geschlossen.  Sie können also keine eigenen benutzerdefinierten Aktivitäten hinzufügen. Jede benutzerdefinierte Verarbeitung muss außerhalb der EDI-Bridge in Ihrer Anwendung erfolgen – entweder vor oder nach Eingang der Nachricht bei der im Rahmen des Handelspartnervertrags konfigurierten Bridge. Bei EAI-Bridges besteht die Möglichkeit zur benutzerdefinierten Verarbeitung. Wenn Sie eine benutzerdefinierte Verarbeitung benötigen, können Sie vor oder nach der Verarbeitung der Nachricht durch die EDI-Bridge EAI-Bridges verwenden. Weitere Informationen finden Sie unter [Einschließen von benutzerdefiniertem Code in Bridges](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Sie können einen Veröffentlichen/Abonnieren-Fluss mit benutzerdefiniertem Code versehen und/oder Service Bus-Messagingwarteschlangen und -Themen verwenden, bevor der Handelspartnervertrag die Nachricht empfängt oder nachdem der Vertrag die Nachricht verarbeitet und an einen Service Bus-Endpunkt weitergeleitet hat.

Das Nachrichtenflussmuster finden Sie in diesem Artikel unter **Szenarien/Nachrichtenfluss**.

## <a name="agreements"></a>Vereinbarungen
Wenn Sie mit den Handelspartnerverträgen für die EDI-Verarbeitung in BizTalk Server 2010 vertraut sind, werden Sie sich bei den Handelspartnerverträgen von BizTalk Services schnell zurechtfinden. Die meisten Vereinbarungseinstellungen sind identisch und verwenden dieselbe Terminologie. In einigen Fällen sind die Vereinbarungseinstellungen erheblich einfacher als in BizTalk Server. Microsoft Azure BizTalk Services unterstützt X12, EDIFACT und AS2 für den Transport.

Microsoft Azure BizTalk Services bietet auch ein **TPM-Datenmigrationstool** , mit dem Handelspartner und Vereinbarungen aus dem Handelspartnermodul von BizTalk Server zum BizTalk Services-Portal migriert werden können. Das TPM-Datenmigrationstool ist Teil eines Toolpakets, das unter [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)heruntergeladen werden kann. Das Paket enthält auch eine Infodatei mit Anweisungen zur Verwendung des Tools sowie mit grundlegenden Informationen zur Problembehandlung für das Tool.

## <a name="schemas"></a>Schemas
BizTalk Services bietet EDI-Schemas für die Verwendung in BizTalk Services-Lösungen.  Darüber hinaus können mit BizTalk Services auch BizTalk Server-EDI-Schemas verwendet werden, da der Stammknoten des EDI-Schemas bei BizTalk Server und BizTalk Services identisch ist. Folglich können Sie Ihre BizTalk Server-EDI-Schemas direkt in den EDI-Lösungen verwenden, die Sie mit BizTalk Services entwickeln. Die Schemas können ebenfalls unter [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)heruntergeladen werden.

## <a name="maps-transforms"></a>Zuordnungen (Transformationen)
Die Zuordnungen aus BizTalk Server werden in BizTalk Services als Transformationen bezeichnet. Die Migration von Zuordnungen aus BizTalk Server zu BizTalk Services kann sich je nach Zuordnungskomplexität als etwas komplizierter erweisen. Das für BizTalk Services verwendete Zuordnungstool unterscheidet sich vom BizTalk-Mapper. Der Mapper sieht zwar größtenteils gleich aus, verwendet aber ein anderes Zuordnungsformat. Die Funktoide ( **Zuordnungsvorgänge** in BizTalk Services), die den Benutzern zur Verfügung stehen, unterscheiden sich ebenfalls.  Eine BizTalk-Zuordnung kann daher nicht direkt in BizTalk Services verwendet werden. Zudem stehen nicht alle in BizTalk Server verfügbaren Funktoide als Zuordnungsvorgänge in BizTalk Services zur Verfügung.

### <a name="new-transform-operations"></a>Neue Transformationsvorgänge
Die Liste der verfügbaren Transformationszuordnungsvorgänge scheint sich zwar stark vom BizTalk Server-Mapper zu unterscheiden, mit den Transformationen von BizTalk Services werden jedoch die gleichen Aufgaben auf neue Weise bewältigt. So stehen bei den Transformationen von BizTalk Services beispielsweise **Listenvorgänge** zur Verfügung. Diese waren im BizTalk-Mapper nicht verfügbar.  Mithilfe von **Listenvorgängen** können Sie Listen erstellen und verwenden. Eine Liste ist hierbei ein Satz von Elementen (Zeilen), und jedes Element kann mehrere Unterelemente (Spalten) besitzen.  Sie können die Liste sortieren, Elemente auf der Grundlage einer Bedingung auswählen usw.

Ein weiteres Beispiel für neue Funktionen in BizTalk Services-Transformationen sind **Schleifenvorgänge**.  Die Erstellung geschachtelter Schleifen ist im BizTalk Server-Mapper recht kompliziert.  Daher werden für BizTalk Services-Transformationen die Schleifenzuordnungsvorgänge hinzugefügt.

Ein weiteres Beispiel ist der Ausdruckszuordnungsvorgang **If-Then-Else** .  Im BizTalk-Mapper konnte zwar ein If-Then-else-Vorgang ausgeführt werden, für eine eigentlich recht einfache Aufgabe wurden allerdings mehrere Funktoide benötigt.

### <a name="migrating-biztalk-server-maps"></a>Migrieren von BizTalk Server-Zuordnungen
Microsoft Azure BizTalk Services bietet ein Tool zum Migrieren von BizTalk Server-Zuordnungen zu BizTalk Services-Transformationen. Das **BTMMigrationTool** ist als Teil des **Tool**-Pakets verfügbar, das mit dem [Download für das BizTalk Services-SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057) verfügbar ist. Weitere Informationen zum Tool finden Sie unter [Konvertieren einer BizTalk-Zuordnung in eine BizTalk Services-Transformation](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Sie können sich auch das Beispiel von BizTalk-MVP Sandro Pereira zum [Migrieren von BizTalk Server-Zuordnungen zu BizTalk Services-Transformationen](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx)ansehen.

## <a name="orchestrations"></a>Orchestrierungen
Wenn Sie BizTalk Server-Orchestrierungen zu Microsoft Azure migrieren möchten, müssen die Orchestrierungen neu geschrieben werden, da Microsoft Azure die Ausführung von BizTalk Server-Orchestrierungen nicht unterstützt.  Die Orchestrierungsfunktion kann zu einem Windows Workflow Foundation 4.0 (WF4)-Dienst umgeschrieben werden.  Dies wäre eine vollständige Überarbeitung, da derzeit keine Migration von BizTalk Server-Orchestrierungen zu WF4 zur Verfügung steht. Im Anschluss finden Sie einige Ressourcen für Windows Workflow:

* [*How to integrate a WCF Workflow Service with Service Bus Queues and Topics (in englischer Sprache)*](https://blogs.msdn.microsoft.com/paolos/2013/04/09/how-to-integrate-a-wcf-workflow-service-with-service-bus-queues-and-topics/) von Paolo Salvatori. 
* [*Erstellen von Apps mit Windows Workflow Foundation und Azure*](http://go.microsoft.com/fwlink/p/?LinkId=237314) von der Build 2011-Konferenz.
* [*Windows Workflow Foundation Developer Center*](http://go.microsoft.com/fwlink/p/?LinkId=237315) auf MSDN.
* [*Dokumentation zu Windows Workflow Foundation 4 (WF4)*](https://msdn.microsoft.com/library/dd489441.aspx) auf MSDN.

## <a name="other-considerations"></a>Weitere Überlegungen
Im Anschluss finden Sie einige Überlegungen, die Sie bei Verwendung von BizTalk Services anstellen müssen.

### <a name="fallback-agreements"></a>Ausweichvereinbarungen
Bei der EDI-Verarbeitung in BizTalk Server gibt es das Konzept der Ausweichvereinbarungen.  Bei BizTalk Services gibt es dieses Konzept bislang **nicht** .  Informationen zur Verwendung von Ausweichvereinbarungen in BizTalk Server finden Sie den BizTalk-Dokumentationsthemen [Die Rolle von Vereinbarungen bei der EDI-Verarbeitung](http://go.microsoft.com/fwlink/p/?LinkId=237317) und [Konfigurieren von globalen Vereinbarungseigenschaften oder Eigenschaften für Ausweichvereinbarungen](https://msdn.microsoft.com/library/bb245981.aspx).

### <a name="routing-to-multiple-destinations"></a>Routing an mehrere Ziele
BizTalk Services-Bridges unterstützen derzeit kein Routing von Nachrichten an mehrere Ziele mit einem Veröffentlichen/Abonnieren-Modell. Stattdessen können Nachrichten aus einer BizTalk Services-Bridge an ein Service Bus-Thema geroutet werden. Dieses kann mehrere Abonnements für den Nachrichtenempfang an mehreren Endpunkten besitzen.

## <a name="see-also"></a>Siehe auch
[Branchenanwendungen in Azure](https://azure.microsoft.com/solutions/lob-applications)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
