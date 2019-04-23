---
title: Kopieren von Daten aus Azure Data Lake Storage Gen1 in Gen2 mit Azure Data Factory
description: Kopieren von Daten aus Azure Data Lake Storage Gen1 in Gen2 mithilfe von Azure Data Factory
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: jingwang
ms.openlocfilehash: e3a27ab15c72289dd28e31d832b81407a66dc754
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59256786"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Kopieren von Daten aus Azure Data Lake Storage Gen1 in Gen2 mit Azure Data Factory

Azure Data Lake Storage Gen2 baut auf [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) auf und bietet eine Reihe von Funktionen für die Big Data-Analyse. Er ermöglicht Ihnen die Arbeit mit Ihren Daten über das Dateisystem und den Objektspeicher.

Wenn Sie zurzeit Azure Data Lake Storage Gen1 verwenden, können Sie die neue Gen2-Funktion auswerten, indem Sie mithilfe von Azure Data Factory Daten aus Data Lake Storage Gen1 in Gen2 kopieren.

Azure Data Factory ist ein vollständig verwalteter, cloudbasierter Datenintegrationsdienst. Mithilfe dieses Diensts können Sie den Lake mit Daten aus zahlreichen lokalen und cloudbasierten Datenspeichern füllen und Zeit beim Erstellen von Analyselösungen sparen. Eine ausführliche Liste der unterstützten Connectors finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory bietet eine Lösung zur horizontalen Skalierung und Verschiebung verwalteter Daten. Aufgrund der horizontal skalierbaren Architektur von ADF können Daten mit hohem Durchsatz erfasst werden. Weitere Informationen finden Sie unter [Leistung der Kopieraktivität](copy-activity-performance.md).

In diesem Artikel erfahren Sie, wie Sie das Tool zum Kopieren von Daten in Data Factory zum Kopieren von Daten aus _Azure Data Lake Storage Gen1_ in _Azure Data Lake Storage Gen2_ verwenden. Sie können ähnliche Schritte zum Kopieren von Daten aus anderen Typen von Datenspeichern ausführen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Sie müssen ein Azure Data Lake Storage Gen1-Konto auswählen, das Daten enthält.
* Azure Storage-Konto mit aktiviertem Data Lake Storage Gen2: [Erstellen Sie ein Speicherkonto](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM), wenn Sie noch keines besitzen.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Wählen Sie im Menü auf der linken Seite **Ressource erstellen** > **Daten + Analysen** > **Data Factory** aus:
   
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Geben Sie auf der Seite **Neue Data Factory** die in der folgenden Abbildung gezeigten Werte für die Felder an: 
      
   ![Seite „Neue Data Factory“](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Name**: Geben Sie einen global eindeutigen Namen für die Azure Data Factory ein. Wenn die Fehlermeldung „Data Factory mit dem Namen \"LoadADLSDemo\" ist nicht verfügbar“ angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. Sie können beispielsweise den Namen _**IhrName**_**ADFTutorialDataFactory** verwenden. Versuchen Sie erneut, die Data Factory zu erstellen. Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](naming-rules.md).
    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
    * **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus der Dropdownliste aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie dann den Namen einer Ressourcengruppe ein. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
    * **Version**: Wählen Sie **V2** aus.
    * **Standort**: Wählen Sie den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher können sich an anderen Standorten bzw. in anderen Regionen befinden. 

3. Klicken Sie auf **Erstellen**.
4. Nach Abschluss der Erstellung navigieren Sie zu Ihrer Data Factory. Die Startseite **Data Factory** wird wie in der folgenden Abbildung dargestellt angezeigt: 
   
   ![Data Factory-Startseite](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

   Wählen Sie die Kachel **Erstellen und überwachen** aus, um die Datenintegrationsanwendung auf einer separaten Registerkarte zu starten.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Laden von Daten in Azure Data Lake Storage Gen2

1. Wählen Sie auf der Seite **Erste Schritte** die Kachel **Daten kopieren** aus, um das Tool zum Kopieren von Daten zu starten: 

   ![Kachel für das Tool zum Kopieren von Daten](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** im Feld **Taskname** den Namen **CopyFromADLSGen1ToGen2** ein, und klicken Sie dann auf **Weiter**:

    ![Eigenschaftenseite](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. Klicken Sie auf der Seite **Quelldatenspeicher** auf **+ Neue Verbindung erstellen**:

    ![Seite „Quelldatenspeicher“](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
    Wählen Sie im Connectorkatalog **Azure Data Lake Storage Gen1** aus, und klicken Sie auf **Weiter**.
    
    ![Seite „Quelldatenspeicher von Azure Data Lake Storage Gen1“](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
4. Führen Sie auf der Seite **Azure Data Lake Storage Gen1-Verbindung angeben** die folgenden Schritte aus:
   1. Wählen Sie Ihr Data Lake Storage Gen1 für den Kontonamen aus, und geben Sie den **Mandanten** an oder überprüfen Sie den angegebenen Mandanten.
   2. Klicken Sie auf **Verbindung testen**, um die Einstellungen zu überprüfen, und wählen Sie dann **Fertig stellen** aus.
   3. Eine neue Verbindung wird erstellt. Klicken Sie auf **Weiter**.
   
   > [!IMPORTANT]
   > In dieser exemplarischen Vorgehensweise verwenden Sie eine verwaltete Identität für Azure-Ressourcen, um Data Lake Storage Gen1 zu authentifizieren. Achten Sie darauf, dass Sie der verwalteten Dienstidentität (Managed Service Identity, MSI) die entsprechenden Berechtigungen in Azure Data Lake Storage Gen1 erteilen. Befolgen Sie dazu [diese Anweisungen](connector-azure-data-lake-store.md#managed-identity).
   
   ![Angeben eines Azure Data Lake Storage Gen1-Kontos](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
5. Navigieren Sie auf der Seite **Eingabedatei oder -ordner auswählen** zu dem Ordner und der Datei, die Sie kopieren möchten. Wählen Sie den Ordner/die Datei aus, und klicken Sie auf **Auswählen**:

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

6. Geben Sie das Kopierverhalten an, indem Sie die Optionen **Dateien rekursiv kopieren** und **Binärkopie** aktivieren. Klicken Sie auf **Weiter**:

    ![Angeben des Ausgabeordners](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
7. Klicken Sie auf der Seite **Zieldatenspeicher** auf **+ Neue Verbindung erstellen**, und wählen Sie anschließend **Azure Data Lake Storage Gen2** und dann **Weiter** aus:

    ![Seite „Zieldatenspeicher“](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

8. Führen Sie auf der Seite **Azure Data Lake Storage Gen2-Verbindung angeben** die folgenden Schritte aus:

   1. Wählen Sie in der Dropdownliste „Speicherkontoname“ das Data Lake Storage Gen2-fähige Konto aus.
   2. Wählen Sie **Fertig stellen** aus, um die Verbindung zu erstellen. Klicken Sie anschließend auf **Weiter**.
   
   ![Angeben eines Azure Data Lake Storage Gen2-Kontos](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

9. Geben Sie auf der Seite **Ausgabedatei oder -ordner auswählen** die Zeichenfolge **copyfromadlsgen1** als Name für den Ausgabeordner ein, und klicken Sie dann auf **Weiter**. Die ADF erstellt das entsprechende ADLS Gen2-Dateisystem und die Unterordner während des Kopierens, wenn diese noch nicht existieren.

    ![Angeben des Ausgabeordners](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

10. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**, um die Standardeinstellungen zu verwenden.

11. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie dann auf **Weiter**:

    ![Seite „Zusammenfassung“](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
12. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline zu überwachen:

    ![Bereitstellungsseite](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
13. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt:

    ![Überwachen der Pipelineausführungen](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

14. Klicken Sie in der Spalte **Aktionen** auf den Link **Aktivitätsausführungen anzeigen**, um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Klicken Sie oben auf den Link **Pipelines**, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 

    ![Überwachung der Aktivitätsausführungen](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

15. Zum Überwachen der Ausführungsdetails jeder Kopieraktivität klicken Sie in der Aktivitätsüberwachungsansicht unter **Aktionen** auf den Link **Details** (Brillensymbol). Sie können Details wie die Menge der Daten, die aus der Quelle in die Senke kopiert wurden, den Datendurchsatz, die Ausführungsschritte mit entsprechender Dauer sowie die verwendeten Konfigurationen überwachen:

    ![Überwachen der Details zur Aktivitätsausführung](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

16. Stellen Sie sicher, dass die Daten in Ihr Data Lake Storage Gen2-Konto kopiert werden.

## <a name="best-practices"></a>Bewährte Methoden

Beim Kopieren großer Datenmengen aus einem dateibasierten Datenspeicher wird Folgendes empfohlen:

- Partitionieren Sie die Dateien jeweils in Dateigruppen mit 10 TB bis 30 TB.
- Lösen Sie nicht zu viele gleichzeitige Kopiervorgänge aus, um die Drosselung von Quellen- oder Senkendatenspeichern zu vermeiden. Sie können mit einem Kopiervorgang beginnen und den Durchsatz überwachen. Fügen Sie dann bei Bedarf nach und nach weitere Vorgänge hinzu.

## <a name="next-steps"></a>Nächste Schritte

* [Kopieraktivität – Übersicht](copy-activity-overview.md)
* [Azure Data Lake Storage Gen2-Connector](connector-azure-data-lake-storage.md)
