---
title: Laden von Daten in Azure Data Lake Storage Gen2 (Vorschauversion) mit Azure Data Factory
description: Kopieren von Daten in Azure Data Lake Storage Gen2 (Vorschauversion) mithilfe von Azure Data Factory
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: jingwang
ms.openlocfilehash: 558b426ea85decb0309390e36910eb18719e6e99
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002526"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-preview-with-azure-data-factory"></a>Laden von Daten in Azure Data Lake Storage Gen2 (Vorschauversion) mit Azure Data Factory

Mit [Azure Data Lake Storage Gen2 (Vorschauversion)](../storage/data-lake-storage/introduction.md) wird Azure Blob Storage ein Protokoll mit einem Namespace und Sicherheitsfeatures eines hierarchischen Dateisystems hinzugefügt und damit das Herstellen einer Verbindung zwischen Analyseframeworks und einer robusten Speicherebene erleichtert. In Data Lake Storage Gen2 (Vorschauversion) bleiben alle Vorzüge des Objektspeichers erhalten, während gleichzeitig die Vorteile einer Dateisystemschnittstelle zum Tragen kommen.

Azure Data Factory ist ein vollständig verwalteter, cloudbasierter Datenintegrationsdienst. Mithilfe dieses Diensts können Sie den Lake mit Daten aus zahlreichen lokalen und cloudbasierten Datenspeichern füllen und Zeit beim Erstellen von Analyselösungen sparen. Eine ausführliche Liste der unterstützten Connectors finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory bietet eine Lösung zur horizontalen Skalierung und Verschiebung verwalteter Daten. Aufgrund der horizontal skalierbaren Architektur von ADF können Daten mit hohem Durchsatz erfasst werden. Weitere Informationen finden Sie unter [Leistung der Kopieraktivität](copy-activity-performance.md).

In diesem Artikel erfahren Sie, wie Sie das Tool zum Kopieren von Daten in Data Factory zum Laden von Daten aus _Amazon Web Services S3_ in _Azure Data Lake Storage Gen2_ verwenden. Sie können ähnliche Schritte zum Kopieren von Daten aus anderen Typen von Datenspeichern ausführen.

>[!TIP]
>Informationen zum Kopieren von Daten aus Azure Data Lake Storage Gen1 nach Gen2 finden Sie in [dieser exemplarischen Vorgehensweise](load-azure-data-lake-storage-gen2-from-gen1.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Azure Storage-Konto mit aktiviertem Data Lake Storage Gen2-Dienst: Falls Sie kein Storage-Konto besitzen, klicken Sie [hier](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM), um eins zu erstellen.
* AWS-Konto mit S3-Bucket, der Daten enthält: In diesem Artikel wird gezeigt, wie Sie Daten aus Amazon S3 kopieren. Sie können andere Datenspeicher verwenden, indem Sie ähnliche Schritte ausführen.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im linken Menü auf **Neu** > **Daten + Analysen** > **Data Factory**:
   
   ![Erstellen einer neuen Data Factory](./media/load-azure-data-lake-storage-gen2/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** die in der folgenden Abbildung gezeigten Werte für die Felder an: 
      
   ![Seite „Neue Data Factory“](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Name**: Geben Sie einen global eindeutigen Namen für die Azure Data Factory ein. Wenn die Fehlermeldung „Data Factory mit dem Namen \"LoadADLSDemo\" ist nicht verfügbar“ angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. Sie können beispielsweise den Namen _**IhrName**_**ADFTutorialDataFactory** verwenden. Versuchen Sie erneut, die Data Factory zu erstellen. Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](naming-rules.md).
    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
    * **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus der Dropdownliste aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie dann den Namen einer Ressourcengruppe ein. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
    * **Version:** Wählen Sie **V2** aus.
    * **Standort**: Wählen Sie den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher können sich an anderen Standorten bzw. in anderen Regionen befinden. 

3. Klicken Sie auf **Erstellen**.
4. Nach Abschluss der Erstellung navigieren Sie zu Ihrer Data Factory. Die Startseite **Data Factory** wird wie in der folgenden Abbildung dargestellt angezeigt: 
   
   ![Data Factory-Startseite](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Wählen Sie die Kachel **Erstellen und überwachen** aus, um die Datenintegrationsanwendung auf einer separaten Registerkarte zu starten.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Laden von Daten in Azure Data Lake Storage Gen2

1. Wählen Sie auf der Seite **Erste Schritte** die Kachel **Daten kopieren** aus, um das Tool zum Kopieren von Daten zu starten: 

   ![Kachel für das Tool zum Kopieren von Daten](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** im Feld **Aufgabenname** den Namen **CopyFromAmazonS3ToADLS** ein, und klicken Sie dann auf **Weiter**:

    ![Eigenschaftenseite](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. Klicken Sie auf der Seite **Quelldatenspeicher** auf **+ Neue Verbindung erstellen**:

    ![Seite „Quelldatenspeicher“](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Wählen Sie im Connectorkatalog **Amazon S3** aus, und klicken Sie auf **Weiter**.
    
    ![Seite „Quelldatenspeicher“ für S3](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. Führen Sie auf der Seite **Amazon S3-Verbindung angeben** die folgenden Schritte aus:
   1. Geben Sie den Wert für die **Zugriffsschlüssel-ID** an.
   2. Geben Sie den Wert für den **geheimen Zugriffsschlüssel** an.
   3. Klicken Sie auf **Verbindung testen**, um die Einstellungen zu überprüfen, und wählen Sie dann **Fertig stellen** aus.
   
   ![Angeben des Amazon S3-Kontos](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
   
   4. Eine neue Verbindung wird erstellt. Klicken Sie auf **Weiter**.
   
5. Navigieren Sie auf der Seite **Eingabedatei oder -ordner auswählen** zu dem Ordner und der Datei, die Sie kopieren möchten. Wählen Sie den Ordner/die Datei aus, und klicken Sie auf **Auswählen**:

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. Geben Sie das Kopierverhalten an, indem Sie die Optionen **Dateien rekursiv kopieren** und **Binärkopie** aktivieren. Klicken Sie auf **Weiter**:

    ![Angeben des Ausgabeordners](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. Klicken Sie auf der Seite **Zieldatenspeicher** auf **+ Neue Verbindung erstellen**, und wählen Sie anschließend **Azure Data Lake Storage Gen2 (Preview)** (Azure Data Lake Storage Gen2 (Vorschauversion)) und dann **Weiter** aus:

    ![Seite „Zieldatenspeicher“](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. Führen Sie auf der Seite **Specify Azure Data Lake Storage connection** (Azure Data Lake Storage-Verbindung angeben) die folgenden Schritte aus:

   1. Wählen Sie in der Dropdownliste „Speicherkontoname“ das Data Lake Storage Gen2-fähige Konto aus.
   2. Klicken Sie auf **Weiter**.
   
   ![Angeben eines Azure Data Lake Storage Gen2-Kontos](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. Geben Sie auf der Seite **Ausgabedatei oder -ordner auswählen** die Zeichenfolge **copyfroms3** als Name für den Ausgabeordner ein, und klicken Sie dann auf **Weiter**: 

    ![Angeben des Ausgabeordners](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**, um die Standardeinstellungen zu verwenden:

    ![Seite "Einstellungen"](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie dann auf **Weiter**:

    ![Seite „Zusammenfassung“](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline zu überwachen:

    ![Bereitstellungsseite](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt:

    ![Überwachen der Pipelineausführungen](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Klicken Sie in der Spalte **Aktionen** auf den Link **Aktivitätsausführungen anzeigen**, um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Klicken Sie oben auf den Link **Pipelines**, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 

    ![Überwachung der Aktivitätsausführungen](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Zum Überwachen der Ausführungsdetails jeder Kopieraktivität klicken Sie in der Aktivitätsüberwachungsansicht unter **Aktionen** auf den Link **Details** (Brillensymbol). Sie können Details wie die Menge der Daten, die aus der Quelle in die Senke kopiert wurden, den Datendurchsatz, die Ausführungsschritte mit entsprechender Dauer sowie die verwendeten Konfigurationen überwachen:

    ![Überwachen der Details zur Aktivitätsausführung](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Stellen Sie sicher, dass die Daten in Ihr Data Lake Storage Gen2-Konto kopiert werden.

## <a name="best-practices"></a>Bewährte Methoden

Beim Kopieren großer Datenmengen aus einem dateibasierten Datenspeicher wird Folgendes empfohlen:

- Partitionieren Sie die Dateien jeweils in Dateigruppen mit 10 TB bis 30 TB.
- Lösen Sie nicht zu viele gleichzeitige Kopiervorgänge aus, um die Drosselung von Quellen- oder Senkendatenspeichern zu vermeiden. Sie können mit einem Kopiervorgang beginnen und den Durchsatz überwachen. Fügen Sie dann bei Bedarf nach und nach weitere Vorgänge hinzu.

## <a name="next-steps"></a>Nächste Schritte

* [Kopieraktivität – Übersicht](copy-activity-overview.md)
* [Azure Data Lake Storage Gen2-Connector](connector-azure-data-lake-storage.md)