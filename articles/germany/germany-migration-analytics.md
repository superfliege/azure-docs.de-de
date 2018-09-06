---
title: Migration von Analytics-Ressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Analytics-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 2a64d0f1b9379b58c0e4ec348ad81ba8ec4d6746
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346257"
---
# <a name="migration-of-analytics-resources-from-azure-germany-to-global-azure"></a>Migration von Analytics-Ressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Analytics-Ressourcen von Azure Deutschland zu Azure weltweit.
  
## <a name="event-hubs"></a>Event Hubs

Sie können Event Hubs nicht direkt von Azure Deutschland zu Azure weltweit migrieren, da die Event Hub-Dienste keine Datenexport- oder -importfunktionen bereitstellen. Sie können die Event Hub-Ressourcen jedoch [als eine Vorlage](../azure-resource-manager/resource-manager-export-template-powershell.md) exportieren. Übernehmen Sie dann die exportierte Vorlage für Azure weltweit, und erstellen Sie die Ressourcen erneut.

> [!NOTE]
> Dabei werden die Daten (z.B. Nachrichten) nicht kopiert, sondern nur die Metadaten erneut erstellt.

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="metadata-event-hubs"></a>Metadaten Event Hubs

- Namespaces
- Event Hubs
- Consumergruppen
- Autorisierungsregeln (siehe weiter unten)

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Event Hubs anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/event-hubs/#step-by-step-tutorials) auf.
- Überprüfen Sie die Migrationsschritte für [ServiceBus](./germany-migration-integration.md#service-bus)
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).


### <a name="references"></a>Referenzen

- [Übersicht über Event Hubs](../event-hubs/event-hubs-about.md)










## <a name="hdinsight"></a>HDInsight

HDInsight-Cluster können von Azure Deutschland zu Azure weltweit mithilfe der folgenden Schritte migriert werden:

- Beenden Sie den HDI-Cluster.
- Migrieren Sie die Daten im Speicher mit AzCopy oder einem ähnlichen Tool zur neuen Region.
- Erstellen Sie neue Computeressourcen in Azure weltweit, und fügen Sie die migrierten Speicherressourcen als primären zugeordneten Speicher an.

Für spezialisiertere Cluster mit langer Ausführungszeit (Kafka, Spark-Streaming, Storm oder HBase) wird empfohlen, den Übergang von Workloads in die neue Region zu orchestrieren.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu HDInsight anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/hdinsight/#step-by-step-tutorials) auf.
- Lesen Sie [Verwalten von HDInsight mithilfe von PowerShell](../hdinsight/hdinsight-administer-use-powershell.md), um weitere Unterstützung beim [Skalieren von HDInsight-Clustern](../hdinsight/hdinsight-administer-use-powershell.md#scale-clusters) zu erhalten.
- Informieren Sie sich, wie [AzCopy](../storage/common/storage-use-azcopy.md) verwendet wird.

### <a name="references"></a>Referenzen

- [Azure HD Insight-Dokumentation](https://docs.microsoft.com/azure/hdinsight/)







## <a name="stream-analytics"></a>Stream Analytics

Um Stream Analytics-Dienste von Azure Deutschland zu Azure weltweit zu migrieren, erstellen Sie das gesamte Setup in einer Azure weltweit-Region manuell mit dem Portal oder mit PowerShell erneut. Die Eingangs- und Ausgangsquellen für jeden Stream Analytics-Auftrag können sich in jeder beliebigen Region befinden.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Stream Analytics anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/stream-analytics/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen
- [Stream Analytics: Übersicht](../stream-analytics/stream-analytics-introduction.md)
- [Erstellen eines Stream Analytics-Auftrags mithilfe von PowerShell](../stream-analytics/stream-analytics-quick-create-powershell.md)






## <a name="sql-data-warehouse"></a>SQL Data Warehouse

Um Azure SQL-Datenbanken zu migrieren, können Sie (für kleinere Workloads) die Exportfunktion verwenden, um eine BACPAC-Datei zu erstellen. Eine BACPAC-Datei ist eine komprimierte Datei (ZIP-Datei) mit Metadaten und Daten aus der SQL Server-Datenbank. Nach der Erstellung können Sie sie in die Zielumgebung kopieren (z.B. mit AzCopy) und die Importfunktion zum erneuten Erstellen der Datenbank verwenden. Beachten Sie die folgenden Überlegungen (weitere Informationen finden Sie in den unten angegebenen Links):

- Damit ein Export transaktionskonsistent ist, stellen Sie sicher, dass entweder
  - während des Exports keine Schreibaktivität auftritt oder
  - der Export aus einer transaktionskonsistenten Kopie der Azure SQL-Datenbank erfolgt.
- Für den Export in Blobspeicher ist die Größe der BACPAC-Datei auf 200 GB beschränkt. Führen Sie für größere BACPAC-Datei einen Export in lokalen Speicher aus.
- Falls der Exportvorgang aus der Azure SQL-Datenbank länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Suchen Sie in den folgenden Links nach Hinweisen, wie Sie die Leistung erhöhen können.

> [!NOTE]
> Die Verbindungszeichenfolge ändert sich, da sich der DNS-Name des Servers ändert.

### <a name="next-steps"></a>Nächste Schritte

- [Exportieren der Datenbank in eine BACPAC-Datei](../sql-database/sql-database-export.md)
- [Importieren der BACPAC-Datei in eine Datenbank](../sql-database/sql-database-import.md)

### <a name="references"></a>Referenzen

- [Dokumentation zu Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/)













## <a name="azure-analysis-service"></a>Azure Analysis Services

Um Ihre Modelle von Azure Deutschland zu Azure weltweit zu migrieren, können Sie Sicherung/Wiederherstellung wie in [diesem Dokument beschrieben](../analysis-services/analysis-services-backup.md) verwenden.

Wenn Sie nur die Modellmetadaten und nicht die Daten migrieren möchten, kann das [erneute Bereitstellen des Modells über SSDT](../analysis-services/analysis-services-deploy.md) eine Alternative darstellen.

### <a name="next-steps"></a>Nächste Schritte
- [Analysis Services-Sicherung](../analysis-services/analysis-services-backup.md)

### <a name="references"></a>Referenzen
- [Analysis Services: Übersicht](../analysis-services/analysis-services-overview.md)
