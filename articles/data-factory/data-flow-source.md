---
title: 'Azure Data Factory Mapping Data Flow: Quelltransformation'
description: 'Azure Data Factory Mapping Data Flow: Quelltransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 54302f97913fd01dc8f8e4a8d987a407c8bdf9a7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369168"
---
# <a name="mapping-data-flow-source-transformation"></a>Mapping Data Flow – Quelltransformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Die Quelltransformation konfiguriert eine Datenquelle, die Sie verwenden möchten, um Daten in Ihre Data Flow-Instanz zu übertragen. Sie können mehr als eine Quellentransformation in einer einzigen Data Flow-Instanz haben. Beginnen Sie immer mit dem Entwerfen Ihrer Data Flows mit einer Quellentransformation.

> [!NOTE]
> Jeder Data Flow erfordert mindestens eine Quelltransformation. Fügen Sie so viele zusätzliche Quellen hinzu, wie Sie benötigen, um Ihre Datentransformationen abzuschließen. Sie können diese Quellen zusammen mit einer Join- oder Union-Transformation verwenden. Beim Debuggen des Datenflusses in Debugsitzungen werden Daten mithilfe der Sampling-Einstellung oder der Debugquellenlimits aus der Quelle gelesen. Es werden jedenfalls erst Daten in eine Senke geschrieben, wenn Sie Ihren Datenfluss aus einer Pipeline-Datenflussaktivität heraus ausführen. 

![Optionen für die Quelltransformation](media/data-flow/source.png "Quelle")

Jede Datenfluss-Quelltransformation muss genau einem Data Factory-Dataset zugeordnet sein. Das Dataset definiert die Form und Position Ihrer Daten, in die geschrieben oder aus denen gelesen werden soll. Sie können Platzhalter und Dateilisten in Ihrer Quelle verwenden, um mit mehr als einer Datei gleichzeitig zu arbeiten, wenn Dateiquellen verwendet werden.

## <a name="data-flow-staging-areas"></a>Data Flow-Stagingbereich

Data Flow arbeitet mit „Staging“-Datasets, die Sie alle in Azure finden. Diese Data Flow-Datasets werden für die Datenbereitstellung verwendet, um Ihre Datentransformationen durchzuführen. Data Factory hat Zugriff auf fast 80 verschiedene native Konnektoren. Um Daten aus diesen anderen Quellen in Ihren Data Flow einzubeziehen, sollten Sie im ersten Schritt diese Daten anhand der Kopieraktivität in eine dieser Staging-Bereiche des Data Flow-Datasets aufnehmen.

## <a name="options"></a>Optionen

### <a name="allow-schema-drift"></a>Schemaabweichung zulassen
Wählen Sie „Schemaabweichung zulassen“, wenn sich die Quellspalten häufig ändern. Diese Einstellung ermöglicht es, dass alle eingehenden Felder von Ihrer Quelle durch die Transformationen zur Senke fließen.

### <a name="validate-schema"></a>Schema überprüfen

![Öffentliche Quelle](media/data-flow/source1.png "Öffentliche Quelle 1")

Wenn die eingehende Version der Quelldaten nicht mit dem definierten Schema übereinstimmt, schlägt die Ausführung des Datenflusses fehl.

### <a name="sampling"></a>Stichproben
Verwenden Sie Stichproben, um die Anzahl der Zeilen aus der Quelle zu beschränken.  Dies ist hilfreich beim Testen oder der Stichprobenentnahme von Daten aus Ihrer Quelle zu Debugzwecken.

## <a name="define-schema"></a>Schema definieren

![Quelltransformation](media/data-flow/source2.png "Quelle 2")

Für Quelldateitypen, die nicht stark typisiert sind (d.h. Flatfile-Dateien im Gegensatz zu Parquet-Dateien), sollten Sie hier in der Quelltransformation die Datentypen für jedes Feld definieren. Sie können die Spaltennamen in einer Auswahltransformation und die Datentypen in einer abgeleiteten Spaltentransformation nachträglich ändern. 

![Quelltransformation](media/data-flow/source003.png "Datentypen")

Für stark typisierte Quellen können Sie die Datentypen in einer nachfolgenden Auswahltransformation ändern. 

### <a name="optimize"></a>Optimierung

![Quellpartitionen](media/data-flow/sourcepart.png "Partitionierung")

Auf der Registerkarte „Optimieren“ für die Quelltransformation sehen Sie einen zusätzlichen Partitionierungstyp namens „Source“. Dieser wird nur angezeigt, wenn Sie „Azure SQL DB“ als Quelle ausgewählt haben. Dies liegt daran, dass ADF Verbindungen parallelisieren möchte, um große Abfragen für Ihre Azure SQL DB-Quelle auszuführen.

Die Partitionierung von Daten auf Ihrer SQL-Datenbankquelle ist optional, aber für große Abfragen nützlich. Sie haben zwei Möglichkeiten:

### <a name="column"></a>Column

Wählen Sie eine Spalte aus Ihrer Quelltabelle aus, auf der partitioniert werden soll. Sie müssen auch die maximale Anzahl von Verbindungen festlegen.

### <a name="query-condition"></a>Abfragebedingung

Sie können optional wählen, ob Sie die Verbindungen basierend auf einer Abfrage partitionieren möchten. Fügen Sie bei dieser Option einfach den Inhalt eines WHERE-Prädikats ein. z.B. Jahr > 1980

## <a name="source-file-management"></a>Quelldateiverwaltung
![Neue Quelleinstellungen](media/data-flow/source2.png "Neue Einstellungen")

* Platzhalterpfad, um eine Reihe von Dateien aus Ihrem Quellordner auszuwählen, die einem Muster entsprechen. Dadurch wird jede Datei überschrieben, die Sie in Ihrer Datasetdefinition festgelegt haben.
* Liste der Dateien. Identisch mit der Dateigruppe. Zeigen Sie auf eine Textdatei, die Sie mit einer Liste der zu verarbeitenden relativen Pfaddateien erstellen.
* Die Spalte zum Speichern des Dateinamens speichert den Namen der Datei aus der Quelle in einer Spalte in Ihren Daten. Geben Sie hier einen neuen Namen ein, um die Zeichenfolge für den Dateinamen zu speichern.
* Nach der Fertigstellung können Sie wählen, ob Sie nach dem Ausführen des Datenflusses nichts mit der Quelldatei machen, die Quelldatei(en) löschen oder die Quelldateien verschieben möchten. Die Pfade für das Verschieben sind relative Pfade.

### <a name="sql-datasets"></a>SQL-Datasets

Wenn Sie Azure SQL DB oder Azure SQL DW als Quelle verwenden, haben Sie zusätzliche Optionen.

* Abfrage: Geben Sie eine SQL-Abfrage für die Quelle ein. Durch das Festlegen einer Abfrage wird jede Tabelle, die Sie im Dataset ausgewählt haben, überschrieben. Beachten Sie, dass die Order By-Klauseln hier nicht unterstützt werden. Sie können hier jedoch eine vollständige SELECT FROM-Anweisung festlegen.

* Batchgröße: Geben Sie eine Batchgröße ein, um große Datenmengen Leseblöcke zu segmentieren.

> [!NOTE]
> Die Einstellungen für den Dateivorgang werden nur ausgeführt, wenn der Datenfluss anhand der Aktivität zum Ausführen des Data Flow in einer Pipeline über eine Pipelineausführung ausgeführt wird (Debuggen der Pipeline oder Ausführung). Dateivorgänge werden NICHT im Data Flow-Debugmodus ausgeführt werden.

### <a name="projection"></a>Projektion

![Projektion](media/data-flow/source3.png "Projektion")

Ähnlich wie Schemas in Datasets definiert die Projektion in Quelle die Datenspalten, Datentypen und Datenformate aus den Quelldaten. Wenn Sie eine Textdatei ohne definiertes Schema haben, klicken Sie auf „Datentyp erkennen“, damit ADF versucht, die Datentypen zu erfassen und abzuleiten. Über die Schaltfläche „Standardformat definieren“ können Sie die Standarddatenformate für die automatische Erkennung festlegen. Sie können die Spaltendatentypen in einer nachfolgenden abgeleiteten Spaltentransformation ändern. Die Spaltennamen können über die Auswahltransformation geändert werden.

![Standardformate](media/data-flow/source2.png "Standardformate")

## <a name="next-steps"></a>Nächste Schritte

Erstellen Ihrer Transformation mit [Abgeleitete Spalte](data-flow-derived-column.md) und [Auswahl](data-flow-select.md).
