---
title: Erste Schritte mit Log Analytics im Azure-Portal | Microsoft-Dokumentation
description: Dieser Artikel ist ein Tutorial zum Schreiben von Abfragen mithilfe von Log Analytics im Azure-Portal.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 3bcc5368a99dc7c7c32381ca0226119d81fc2c0a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978198"
---
# <a name="get-started-with-log-analytics-in-the-azure-portal"></a>Erste Schritte mit Log Analytics im Azure-Portal

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

In diesem Tutorial erfahren Sie, wie Sie mithilfe der Log Analytics-Seite im Azure-Portal (derzeit in der Vorschauphase) Log Analytics-Abfragen schreiben können. Es wird Folgendes vermittelt:

- Schreiben von einfachen Abfragen
- Grundlegendes zum Schema Ihrer Daten
- Filtern, Sortieren und Gruppieren von Ergebnissen
- Anwenden eines Zeitbereichs
- Erstellen von Diagrammen
- Speichern und Laden von Abfragen
- Exportieren und Freigeben von Abfragen


## <a name="meet-the-log-analytics-page"></a>Einführung in die Log Analytics-Seite 
Die Log Analytics-Seite ist ein Webtool, das zum Schreiben und Ausführen von Azure Log Analytics-Abfragen verwendet wird. Öffnen Sie die Seite, indem Sie im Log Analytics-Menü die Option **Protokolle (Vorschau)** auswählen. Zu Beginn wird eine neue leere Abfrage angezeigt.

![Startseite](media/get-started-analytics-portal/homepage.png)



## <a name="basic-queries"></a>Grundlegende Abfragen
Mithilfe von Abfragen können Suchbegriffe verwendet, Trends identifiziert, Muster analysiert und viele andere Informationen basierend auf Ihren Daten ermittelt werden. Beginnen Sie mit einer einfachen Abfrage:

```Kusto
Event | search "error"
```

Diese Abfrage durchsucht die _Event_-Tabelle nach Datensätzen, die die Benennung „error“ in einer Eigenschaft enthalten.

Abfragen können entweder mit einem Tabellennamen oder einem **search**-Befehl beginnen. Das obige Beispiel beginnt mit dem Tabellennamen _Event_, der den Gültigkeitsbereich der Abfrage definiert. Der senkrechte Strich (|) trennt Befehle, d.h. die Ausgabe des ersten Befehls in der Eingabe vom folgenden Befehl. Sie können eine beliebige Anzahl von Befehlen zu einer einzelnen Abfrage hinzufügen.

Eine andere Möglichkeit, die gleiche Abfrage zu schreiben, wäre Folgende:

```Kusto
search in (Event) "error"
```

In diesem Beispiel bezieht sich **search** auf die _Event_-Tabelle und in allen Datensätze in dieser Tabelle wird die Benennung „error“ gesucht.

## <a name="running-a-query"></a>Ausführen einer Abfrage
Führen Sie eine Abfrage aus, indem Sie auf die Schaltfläche **Ausführen** klicken oder die **UMSCHALT+EINGABETASTE** drücken. Beachten Sie die folgenden Details, die den Code, der ausgeführt wird, und die zurückgegebenen Daten bestimmen:

- Zeilenumbrüche: Ein einzelner Umbruch sorgt für mehr Übersichtlichkeit in Ihrer Abfrage. Mehrere Zeilenumbrüche teilen diese in separate Abfragen auf.
- Cursor: Platzieren Sie den Cursor an einer beliebigen Stelle in der Abfrage, um sie auszuführen. Der Code ist die aktuelle Abfrage bis zum einer leeren Zeile.
- Zeitbereich: Standardmäßig ist der Zeitbereich _Letzte 24 Stunden_ festgelegt. Wenn Sie einen anderen Bereich verwenden möchten, verwenden Sie die Zeitauswahl, oder fügen Sie Ihrer Abfrage einen expliziten Zeitbereichsfilter hinzu.


## <a name="understand-the-schema"></a>Grundlegendes zum Schema
Das Schema ist eine Auflistung von Tabellen, die visuell unter einer logischen Kategorie gruppiert sind. Mehrere Kategorien stammen aus Überwachungslösungen. Die _LogManagement_-Kategorie enthält gemeinsame Daten wie Windows- und Syslog-Ereignisse, Leistungsdaten und Clienttakte.

![Schema](media/get-started-analytics-portal/schema.png)

In jeder Tabelle sind Daten in Spalten mit unterschiedlichen Datentypen angeordnet, was durch Symbole neben dem Spaltennamen angegeben wird. Die im Screenshot gezeigte _Event_-Tabelle enthält Spalten wie _Computer_, bei der es sich um Text handelt, _EventCategory_, bei der es sich um eine Zahl handelt, und _TimeGenerated_, bei der es sich um Datum/Uhrzeit handelt.

## <a name="filter-the-results"></a>Filtern der Ergebnisse
Fügen Sie zunächst alle Elemente in die _Event_-Tabelle ein.

```Kusto
Event
```

Die Log Analytics-Seite beschränkt die Ergebnisse automatisch auf Folgendes:

- Zeitbereich: Abfragen werden standardmäßig auf die letzten 24 Stunden beschränkt.
- Anzahl von Ergebnissen: Ergebnisse sind auf maximal 10.000 Datensätze beschränkt.

Diese Abfrage ist sehr allgemein gehalten und gibt zu viele Ergebnisse zurück. Sie können die Ergebnisse entweder über die Tabellenelemente oder explizit durch Hinzufügen eines Filters zur Abfrage filtern. Das Filtern von Ergebnissen durch die Tabellenelemente gilt für das vorhandene Resultset, während ein Filter für die Abfrage selbst ein neues gefiltertes Resultset zurückgibt und daher genauere Ergebnisse liefern kann.

### <a name="add-a-filter-to-the-query"></a>Hinzufügen eines Filters zur Abfrage
Links neben den einzelnen Datensätzen wird ein Pfeil angezeigt. Klicken Sie auf diesen Pfeil, um die Details für einen bestimmten Datensatz zu öffnen.

Zeigen Sie auf einen Spaltennamen für die Symbole „+“ und „-“, um diese anzuzeigen. Um einen Filter hinzuzufügen, der nur Datensätze mit dem gleichen Wert zurückgibt, klicken Sie auf das „+“-Zeichen. Klicken Sie auf „-“, um Datensätze mit diesem Wert auszuschließen, und klicken Sie dann auf **Ausführen**, um die Abfrage erneut auszuführen.

![Hinzufügen eines Filters zur Abfrage](media/get-started-analytics-portal/add-filter.png)

### <a name="filter-through-the-table-elements"></a>Filtern über die Tabellenelemente
Konzentrieren wir uns nun auf Ereignisse mit dem Schweregrad _Error_. Dieser wird in einer Spalte namens _EventLevelName_ angegeben. Sie müssen nach rechts scrollen, um diese Spalte anzuzeigen.

Klicken Sie auf das Filtersymbol neben dem Spaltentitel, und wählen Sie im Popupfenster Werte aus, die bei _Beginnt mit_ den Text _error_ aufweisen:

![Filter](media/get-started-analytics-portal/filter.png)


## <a name="sort-and-group-results"></a>Sortieren und Gruppieren von Ergebnissen
Die Ergebnisse werden nun nur auf Fehlerereignisse von SQL Server eingegrenzt, die in den letzten 24 Stunden erstellt wurden. Die Ergebnisse werden jedoch in keinerlei Weise sortiert. Um die Ergebnisse nach einer bestimmten Spalte wie _timestamp_ zu sortieren, klicken Sie beispielsweise auf den Spaltentitel. Mit einem Klick erfolgt die Sortierung in aufsteigender Reihenfolge, während diese mit einem zweiten Klick in die absteigende Sortierung geändert wird.

![Sortieren einer Spalte](media/get-started-analytics-portal/sort-column.png)

Eine weitere Möglichkeit zum Organisieren von Ergebnissen sind Gruppen. Zum Gruppieren von Ergebnissen nach einer bestimmten Spalte ziehen Sie einfach die Spaltenüberschrift über die anderen Spalten. Um Untergruppen zu erstellen, ziehen Sie andere Spalten ebenfalls zur oberen Leiste.

![Gruppen](media/get-started-analytics-portal/groups.png)

## <a name="select-columns-to-display"></a>Auswählen der anzuzeigenden Spalten
Die Ergebnistabelle enthält oft viele Spalten. Einige der zurückgegebenen Spalten werden möglicherweise standardmäßig nicht angezeigt, oder eventuell möchten Sie einige der Spalten entfernen, die angezeigt werden. Um die anzuzeigenden Spalten auszuwählen, klicken Sie auf die Schaltfläche „Spalten“:

![Spalten auswählen](media/get-started-analytics-portal/select-columns.png)


## <a name="select-a-time-range"></a>Auswählen eines Zeitbereichs
Standardmäßig wendet die Log Analytics-Seite den Zeitbereich _Letzte 24 Stunden_ an. Um einen anderen Bereich zu verwenden, wählen Sie einen anderen Wert über die Zeitauswahl aus, und klicken Sie auf **Ausführen**. Neben den vordefinierten Werten können Sie die Option _Benutzerdefinierter Zeitbereich_ verwenden, um einen absoluten Bereich für Ihre Abfrage auszuwählen.

![Zeitauswahl](media/get-started-analytics-portal/time-picker.png)

Wenn Sie einen benutzerdefinierten Zeitbereich auswählen, werden die Werte im UTC-Format angezeigt, das von Ihrer lokalen Zeitzone abweichen könnte.

Wenn die Abfrage explizit einen Filter für _TimeGenerated_ enthält, zeigt der Titel der Zeitauswahl _In Abfrage festlegen_ an. Die manuelle Auswahl wird deaktiviert, um einen Konflikt zu verhindern.


## <a name="charts"></a>Diagramme
Ergebnisse können nicht nur in einer Tabelle zurückgegeben werden, sondern auch in visuellen Formaten dargestellt werden. Verwenden Sie als Beispiel folgende Abfrage:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Standardmäßig werden Ergebnisse in einer Tabelle angezeigt. Klicken Sie auf _Diagramm_, um die Ergebnisse in einer grafischen Ansicht anzuzeigen:

![Balkendiagramm](media/get-started-analytics-portal/bar-chart.png)

Die Ergebnisse werden in einem gestapelten Balkendiagramm angezeigt. Klicken Sie auf _Gestapelte Säule_, und wählen Sie _Kreis_ aus, um eine andere Ansicht der Ergebnisse anzuzeigen:

![Kreisdiagramm](media/get-started-analytics-portal/pie-chart.png)

Die verschiedenen Eigenschaften der Ansicht, z.B. x- und y-Achsen, oder die Gruppierung und Aufteilung von Einstellungen kann manuell über die Steuerleiste geändert werden.

Mit dem render-Operator können Sie auch die bevorzugte Ansicht in der Abfrage selbst festlegen.

### <a name="smart-diagnostics"></a>Intelligente Diagnose
Wenn in einem Zeitdiagramm eine abrupte Spitze oder ein Sprung in Ihren Daten dargestellt ist, wird unter Umständen ein hervorgehobener Punkt auf der Linie angezeigt. Dieser gibt an, dass die _intelligente Diagnose_ eine Kombination von Eigenschaften ermittelt hat, die die abrupte Änderung herausfiltert. Klicken Sie auf den Punkt, um weitere Details zum Filter zu erhalten und die gefilterte Version anzuzeigen. Dadurch können Sie vielleicht ermitteln, was die Änderung verursacht hat:

![Intelligente Diagnose](media/get-started-analytics-portal/smart-diagnostics.png)

## <a name="pin-to-dashboard"></a>An Dashboard anheften
Um ein Diagramm oder eine Tabelle an eines Ihrer freigegebenen Azure-Dashboards anzuheften, klicken Sie auf das Stecknadelsymbol.

![An Dashboard anheften](media/get-started-analytics-portal/pin-dashboard.png)

Auf ein Diagramm werden bestimmte Vereinfachungen angewendet, wenn Sie es an ein Dashboard anheften:

- Tabellenspalten und -zeilen: Damit eine Tabelle an das Dashboard angeheftet werden kann, darf sie maximal vier Spalten enthalten. Nur die ersten sieben Zeilen werden angezeigt.
- Zeiteinschränkung: Abfragen sind automatisch auf die letzten 14 Tage begrenzt.
- Maximale Anzahl von Containern: Wenn Sie ein Diagramm mit zahlreichen separaten Containern anzeigen, werden die Container mit wenig Daten automatisch in einem einzelnen _anderen_ Container zusammengefasst.

## <a name="save-queries"></a>Speichern von Abfragen
Nachdem Sie eine nützliche Abfrage erstellt haben, sollten Sie diese speichern oder für andere Benutzer freigeben. Das Symbol zum **Speichern** befindet sich in der oberen Leiste.

Sie können entweder die gesamte Abfrageseite oder eine einzelne Abfrage als Funktion speichern. Funktionen sind Abfragen, auf die auch durch andere Abfragen verwiesen werden kann. Um eine Abfrage als Funktion zu speichern, müssen Sie einen Funktionsalias angeben. Dies ist der Name zum Aufrufen dieser Abfrage, wenn durch andere Abfragen auf diese verwiesen wird.

![Funktion speichern](media/get-started-analytics-portal/save-function.png)

Log Analytics-Abfragen werden immer in einem ausgewählten Arbeitsbereich gespeichert und für andere Benutzer des Arbeitsbereichs freigegeben.

## <a name="load-queries"></a>Laden von Abfragen
Das Abfrage-Explorer-Symbol wird im oberen rechten Bereich angezeigt. Hiermit werden alle gespeicherten Abfragen nach Kategorie aufgeführt. Darüber hinaus können Sie bestimmte Abfragen als Favoriten markieren, um diese später schnell finden zu können. Doppelklicken Sie auf eine gespeicherte Abfrage, um sie zum aktuellen Fenster hinzuzufügen.

![Abfrage-Explorer](media/get-started-analytics-portal/query-explorer.png)

## <a name="export-and-share-as-link"></a>Exportieren und Freigeben als Link
Die Log Analytics-Seite unterstützt mehrere Exportmethoden:

- Excel: Speichern Sie die Ergebnisse als CSV-Datei.
- Power BI: Exportieren Sie die Ergebnisse in Power BI. Einzelheiten finden Sie unter [Importieren von Log Analytics-Daten in Power BI](../log-analytics-powerbi.md).
- Freigabe eines Links: Die Abfrage selbst kann als Link freigegeben werden, der dann von anderen Benutzern, die Zugriff auf den gleichen Arbeitsbereich haben, gesendet und ausgeführt werden kann.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Schreiben von Log Analytics-Abfragen](get-started-queries.md).
