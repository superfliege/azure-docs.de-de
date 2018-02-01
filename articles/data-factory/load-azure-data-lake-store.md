---
title: Laden von Daten in Azure Data Lake Store mit Azure Data Factory | Microsoft Docs
description: Kopieren von Daten in Azure Data Lake Store mithilfe von Azure Data Factory
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 3f73cd65b0ceb3148ce8ceb83d7b4e1be1280077
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-data-lake-store-using-azure-data-factory"></a>Laden von Daten in Azure Data Lake Store mit Azure Data Factory

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) ist ein unternehmensweites riesiges Repository für Big Data-Analyseworkloads. Azure Data Lake bietet Ihnen die Möglichkeit, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen an einem einzigen Ort zu erfassen.

Azure Data Factory ist ein vollständig verwalteter cloudbasierter Datenintegrationsdienst, der verwendet werden kann, um den Data Lake mit den Daten Ihres vorhandenen Systems aufzufüllen. Dieser Dienst spart Ihnen wertvolle Zeit beim Erstellen von Analyselösungen. Die folgenden wichtigen Vorteile ergeben sich beim Laden von Daten in Azure Data Lake Store mithilfe von Azure Data Factory:

* **Mühelose Einrichtung**: Intuitiver Assistent mit 5 Schritten. Keine Skripterstellung erforderlich.
* **Unterstützung für umfangreiche Datenspeicher**: Integrierte Unterstützung für umfangreiche lokale und cloudbasierte Datenspeicher. Eine ausführliche Liste finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
* **Sicher und kompatibel**: Daten werden über HTTPS oder ExpressRoute übertragen, und globale Dienste stellen sicher, dass Ihre Daten nie die geografische Grenze verlassen.
* **Hohe Leistung**: Bis zu 1 GBit/s Datenladegeschwindigkeit in Azure Data Lake Store. Weitere Informationen finden Sie unter [Leistung der Kopieraktivität](copy-activity-performance.md).

Diesem Artikel erfahren Sie, wie Sie das Tool zum Kopieren von Daten von Data Factory zum **Laden von Daten aus Amazon S3 in Azure Data Lake Store** verwenden können. Sie können ähnliche Schritte zum Kopieren von Daten aus anderen Typen von Datenspeichern befolgen.

> [!NOTE]
>  Im Artikel [Kopieren von Daten in und aus Azure Data Lake Store mithilfe von Azure Data Factory](connector-azure-data-lake-store.md) finden Sie allgemeine Informationen zu den Funktionen von Data Factory beim Kopieren von Daten aus/in Azure Data Lake Store.
>
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Kopieraktivität in V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure Data Lake Store**. Wenn Sie über kein Data Lake Store-Konto verfügen, finden Sie im Artikel [Erstellen eines Data Lake Store-Kontos](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) die erforderlichen Schritte zum Erstellen eines solchen Kontos.
* **Amazon S3**. Dieser Artikel zeigt, wie Daten aus Amazon S3 kopiert werden. Sie können andere Datenspeicher verwenden, indem Sie ähnliche Schritte ausführen.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite auf **NEU**, und klicken Sie auf **Data + Analytics** und dann auf **Data Factory**. 
   
   ![Neu -> Data Factory](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** die Werte wie im folgenden Screenshot gezeigt an: 
      
     ![Seite „Neue Data Factory“](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
   * **Name**. Geben Sie einen globalen, eindeutigen Namen für die Data Factory ein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
            `Data factory name "LoadADLSDemo" is not available`

    * **Abonnement**. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
    * **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe aus der Dropdownliste aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie dann den Namen einer Ressourcengruppe ein. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
    * **Version**. Wählen Sie **V2 (Vorschau)** aus.
    * **Standort**. Wählen Sie den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher (Azure Data Lake Store, Azure Storage, Azure SQL-Datenbank usw.) können sich an anderen Standorten bzw. in anderen Regionen befinden.

3. Klicken Sie auf **Create**.
4. Nach Abschluss der Erstellung navigieren Sie zu Ihrer Data Factory. Die Seite **Data Factory** wird wie in der Abbildung gezeigt angezeigt. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Datenintegrationsanwendung auf einer separaten Registerkarte zu starten. 
   
   ![Data Factory-Startseite](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

## <a name="load-data-into-azure-data-lake-store"></a>Laden von Daten in Azure Data Lake Store

1. Klicken Sie auf der Seite mit den ersten Schritten auf die Kachel **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** des Tools zum Kopieren von Daten als **Aufgabenname** den Namen **CopyFromAmazonS3ToADLS** ein, und klicken Sie dann auf **Weiter**. 

    ![Tool zum Kopieren von Daten – Seite „Eigenschaften“](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. Wählen Sie auf der Seite **Quelldatenspeicher** die Option **Amazon S3** aus, und klicken Sie dann auf **Weiter**.

    ![Seite „Quelldatenspeicher“](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. Führen Sie auf der Seite **Amazon S3-Verbindung angeben** die folgenden Schritte aus: 
    1. Geben Sie die **Zugriffsschlüssel-ID** an.
    2. Geben Sie den **geheimen Zugriffsschlüssel** an.
    3. Klicken Sie auf **Weiter**. 

        ![Angeben des Amazon S3-Kontos](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. Navigieren auf der Seite **Eingabedatei oder -ordner auswählen** zu dem Ordner bzw. der Datei, der oder die kopiert werden soll, wählen Sie ihn oder sie aus, klicken Sie auf **Auswählen**, und klicken Sie dann auf **Weiter**. 

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. Wählen Sie auf der Seite **Zieldatenspeicher** die Option **Azure Data Lake Store** aus, und klicken Sie dann auf **Weiter**. 

    ![Seite „Zieldatenspeicher“](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Wählen Sie auf dieser Seite das Kopierverhalten durch Aktivieren der Optionen **Dateien rekursiv kopieren** und **Binärkopie** (unverändertes Kopieren von Dateien) aus. Klicken Sie auf **Weiter**.

    ![Angeben des Ausgabeordners](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. Führen Sie auf der Seite **Data Lake Store-Verbindung angeben** die folgenden Schritte aus: 

    1. Wählen Sie Ihren Data Lake Store für den **Data Lake Store-Kontonamen** aus.
    2. Geben Sie die Dienstprinzipalinformationen einschließlich **Mandant**, **Dienstprinzipal-ID** und **Dienstprinzipalschlüssel** an.
    3. Klicken Sie auf **Weiter**. 

    > [!IMPORTANT]
    > In dieser exemplarischen Vorgehensweise verwenden Sie einen **Dienstprinzipal**, um Data Lake Store zu authentifizieren. Befolgen Sie die [hier](connector-azure-data-lake-store.md#using-service-principal-authentication) beschriebenen Anleitungen, und stellen Sie sicher, dass Sie dem Dienstprinzipal die richtige Berechtigung in Azure Data Lake Store erteilen.

    ![Angeben des Azure Data Lake Store-Kontos](./media/load-data-into-azure-data-lake-store/specify-adls.png)

9. Geben Sie **copyfroms3** auf der Seite **Ausgabedatei oder -ordner auswählen** an, und klicken Sie dann auf **Weiter**. 

    ![Angeben des Ausgabeordners](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)


10. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**. 

    ![Seite "Einstellungen"](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie auf **Weiter**.

    ![Seite „Zusammenfassung“](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline (Aufgabe) zu überwachen.

    ![Seite „Bereitstellung“](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt. 

    ![Überwachen der Pipelineausführungen](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf **View Activity Runs** (Aktivitätsausführungen anzeigen). Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Klicken Sie auf **Pipelines**, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 

    ![Überwachung der Aktivitätsausführungen](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Sie können die Ausführungsdetails jeder Kopieraktivität weiter überwachen, indem Sie in der Aktivitätsüberwachungsansicht unter **Aktionen** auf den Link **Details** klicken. Es werden Informationen wie z.B. die Menge der Daten, die aus der Quelle in die Senke kopiert wurden, der Durchsatz, die Schritte, die durchlaufen werden mit der entsprechenden Dauer sowie die verwendeten Konfigurationen angezeigt.

    ![Überwachen der Details zur Aktivitätsausführung](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Stellen Sie sicher, dass die Daten in Ihren Azure Data Lake Store kopiert werden. 

    ![Überprüfen der Data Lake Store-Ausgabe](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den folgenden Artikel, um mehr über die Unterstützung von Azure Data Lake Store zu erfahren: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store-Connector](connector-azure-data-lake-store.md)