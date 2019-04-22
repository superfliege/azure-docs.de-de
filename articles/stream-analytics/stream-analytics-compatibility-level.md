---
title: Verstehen des Kompatibilitätsgrads für Azure Stream Analytics-Aufträge
description: Erfahren Sie, wie Sie einen Kompatibilitätsgrad für einen Azure Stream Analytics-Auftrag festlegen und welche größeren Änderungen am neuesten Kompatibilitätsgrad vorgenommen wurden.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 6fb93152263d253de983b17d25f02f4c68a172fd
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361392"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Kompatibilitätsgrad für Azure Stream Analytics-Aufträge
 
Der Kompatibilitätsgrad bezieht sich auf das releasespezifischen Verhalten des Azure Stream Analytics-Diensts. Azure Stream Analytics ist ein verwalteter Dienst mit regelmäßigen Featureupdates und Leistungsverbesserungen. Normalerweise sind Updates automatisch für Endbenutzer verfügbar. Allerdings können einige neue Features zu grundlegenden Änderungen führen, z.B. zu Veränderungen am Verhalten eines bestehenden Auftrags, Änderungen bei den Prozessen, die Daten aus diesen Aufträgen nutzen usw. Mithilfe eines Kompatibilitätsgrads wird eine wesentliche Änderung bei Stream Analytics dargestellt. Bei grundlegenden Änderungen wird immer ein neuer Kompatibilitätsgrad eingeführt. 

Der Kompatibilitätsgrad stellt sicher, dass vorhandene Aufträge ohne Fehler ausgeführt werden. Wenn Sie einen neuen Stream Analytics-Auftrag erstellen, gilt es als bewährte Methode, diesen mit dem neuesten Kompatibilitätsgrad zu erstellen. 
 
## <a name="set-a-compatibility-level"></a>Festlegen eines Kompatibilitätsgrads 

Der Kompatibilitätsgrad steuert das Laufzeitverhalten eines Stream Analytics-Auftrags. Sie können den Kompatibilitätsgrad für einen Stream Analytics-Auftrag über das Portal oder mithilfe des [REST-API-Aufrufs „create job“](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job) festlegen. Azure Stream Analytics unterstützt zurzeit zwei Kompatibilitätsgrade: „1.0“ und „1.1“. Standardmäßig ist der Kompatibilitätsgrad auf „1.0“ festgelegt, der mit der allgemeinen Verfügbarkeit von Azure Stream Analytics eingeführt wurde. Navigieren Sie zum Aktualisieren des Standardwerts zum vorhandenen Stream Analytics-Auftrag. Wählen Sie die Option **Kompatibilitätsgrad** im Abschnitt **Konfigurieren** aus, und ändern Sie den Wert. 

Sie müssen den Auftrag beenden, bevor Sie den Kompatibilitätsgrad aktualisieren. Der Kompatibilitätsgrad kann nicht aktualisiert werden, wenn der Auftrag ausgeführt wird. 

![Kompatibilitätsgrad von Stream Analytics im Azure-Portal](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

 
Wenn Sie den Kompatibilitätsgrad aktualisieren, überprüft der T-SQL-Compiler den Auftrag mit der Syntax, die dem ausgewählten Kompatibilitätsgrad entspricht. 

## <a name="major-changes-in-the-latest-compatibility-level-12"></a>Wichtige Änderungen beim aktuellen Kompatibilitätsgrad (1.2)

Beim Kompatibilitätsgrad 1.2 wurden die folgenden grundlegenden Änderungen eingeführt:

### <a name="geospatial-functions"></a>Geofunktionen 

**Vorherige Versionen:** In Azure Stream Analytics wurden geografische Berechnungen verwendet.

**Aktuelle Version:** Mit Azure Stream Analytics können Sie geometrisch geplante Geokoordinaten berechnen. Es gibt keine Änderung in der Signatur der Geofunktionen. Ihre Semantik ist jedoch etwas anders, sodass eine genauere Berechnung ermöglicht wird als bisher.

Azure Stream Analytics unterstützt die Indizierung von räumlichen Verweisdaten. Verweisdaten, die Geoelemente enthalten, können für eine schnellere Berechnung von Verknüpfungen indiziert werden.

Die aktualisierten Geofunktionen ermöglichen die volle Ausdrucksfähigkeit des Geoformats Well Known Text (WKT). Sie können andere Geokomponenten angeben, die bisher mit GeoJson nicht unterstützt wurden.

Weitere Informationen finden Sie unter [Updates to geospatial features in Azure Stream Analytics – Cloud and IoT Edge](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/) (Aktualisierungen für Geofunktionen in Azure Stream Analytics – Cloud und IoT Edge).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Ausführung paralleler Abfragen für Eingabequellen mit mehreren Partitionen 

**Vorherige Versionen:** Für Azure Stream Analytics-Abfragen war die Verwendung der Klausel „PARTITION BY“ erforderlich, um die Verarbeitung von Abfragen in Partitionen von Eingabequellen zu parallelisieren.

**Aktuelle Version:** Wenn die Abfragelogik in Partitionen von Eingabequellen parallelisiert werden kann, werden in Azure Stream Analytics separate Abfrageinstanzen erstellt und Berechnungen parallel ausgeführt.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>Native Integration der Bulk-API in die Cosmos DB-Ausgabe

**Vorherige Versionen:** Das Upsertverhalten war durch *Einfügen oder Zusammenführen*  definiert.

**Aktuelle Version:** Durch die native Integration der Bulk-API in die Cosmos DB-Ausgabe werden der Durchsatz maximiert und Drosselungsanforderungen effizient verarbeitet.

Das Upsertverhalten ist durch *Einfügen oder Ersetzen*  definiert.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>DateTimeOffset beim Schreiben in SQL-Ausgabe

**Vorherige Versionen:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017)-Typen wurden an UTC angepasst.

**Aktuelle Version:** DateTimeOffset wird nicht mehr angepasst.

### <a name="strict-validation-of-prefix-of-functions"></a>Strikte Überprüfung des Präfixes von Funktionen

**Vorherige Versionen:** Es wurde keine strikte Überprüfung von Funktionspräfixen durchgeführt.

**Aktuelle Version:** In Azure Stream Analytics erfolgt eine strikte Überprüfung von Funktionspräfixen. Das Hinzufügen eines Präfixes zu einer integrierten Funktion verursacht einen Fehler. `myprefix.ABS(…)` wird beispielsweise nicht unterstützt.

Auch das Hinzufügen eines Präfixes zu integrierten Aggregaten führt zu einem Fehler. `myprefix.SUM(…)` wird beispielsweise nicht unterstützt.

Die Verwendung des Präfixes „system“ für benutzerdefinierte Funktionen führt zu einem Fehler.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>„Array“ und „Object“ als Schlüsseleigenschaften in Cosmos DB-Ausgabeadapter nicht zugelassen

**Vorherige Versionen:** Die Typen „Array“ und „Object“ wurden als Schlüsseleigenschaft unterstützt.

**Aktuelle Version:** Die Typen „Array“ und „Object“ werden nicht mehr als Schlüsseleigenschaft unterstützt.


## <a name="next-steps"></a>Nächste Schritte
* [Problembehandlung von Azure Stream Analytics-Eingaben](stream-analytics-troubleshoot-input.md)
* [Stream Analytics-Ressourcenintegrität](stream-analytics-resource-health.md)
