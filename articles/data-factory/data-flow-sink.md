---
title: 'Azure Data Factory Mapping Data Flow: Sink-Transformation'
description: 'Azure Data Factory Mapping Data Flow: Sink-Transformation'
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 3829fb3c045b149552d3f022e31f30f9cfae8182
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57852439"
---
# <a name="mapping-data-flow-sink-transformation"></a>Mapping Data Flow: Sink-Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Sink-Optionen](media/data-flow/windows1.png "Senke 1")

Nach Abschluss Ihrer Datenflusstransformation können Sie die transformierten Daten in ein Zieldataset senken. In der Sink-Transformation können Sie die Datasetdefinition auswählen, die Sie für die Zielausgabedaten verwenden möchten. Sie können so viele Sink-Transformationen haben, wie Ihr Datenfluss erfordert.

Eine gängige Praxis, Änderung eingehender Daten und Schemaabweichung zu berücksichtigen, besteht darin, die Ausgabedaten ohne ein definiertes Schema im Ausgabedataset in einen Ordner zu senken. Darüber hinaus können Sie allen Spaltenänderungen in Ihren Quellen durch Auswahl von „Schemaabweichung zulassen“ an der Quelle Rechnung tragen und dann automatisch alle Felder in Sink zuordnen.

Sie können den Datenfluss auch beim Senken in ein Dataset überschreiben, anfügen oder verwerfen.

Sie können auch „automatisch zuordnen“ auswählen, um alle eingehenden Felder in der Senke unterzubringen. Wenn Sie die Felder auswählen möchten, die Sie in das Ziel senken möchten, oder wenn Sie die Namen der Felder im Ziel ändern möchten, wählen Sie „Aus“ für „automatisch zuordnen“ aus, und klicken Sie dann auf die Registerkarte „Zuordnung“, um Ausgabefelder zuzuordnen:

![Sink-Optionen](media/data-flow/sink2.png "Senke 2")

## <a name="output-to-one-file"></a>Ausgabe in einer Datei
Für Senken des Typs Azure Storage Blob oder Data Lake geben Sie die transformierten Daten in einen Ordner aus. Spark generiert partitionierte Ausgabedatendateien basierend auf dem Partitionierungsschema, das in der Sink-Transformation verwendet wird. Sie können das Partitionierungsschema festlegen, indem Sie auf die Registerkarte „Optimieren“ klicken. Wenn ADF Ihre Ausgabe in einer einzelnen Datei zusammenführen soll, klicken Sie auf das Optionsfeld „Einzelpartition“.

![Sink-Optionen](media/data-flow/opt001.png "Sink-Optionen")

## <a name="field-mapping"></a>Feldzuordnung

Auf der Registerkarte „Zuordnung“ Ihrer Sink-Transformation können Sie die eingehenden Spalten (linke Seite) dem Ziel (rechte Seite) zuordnen. Wenn Sie Datenflüsse in Dateien senken, schreibt ADF immer neue Dateien in einen Ordner. Bei Zuordnung zu einem Datenbankdataset können Sie entweder eine neue Tabelle mit diesem Schema generieren (Speicherrichtlinie auf „Überschreiben“ festlegen) oder neue Zeilen in eine vorhandene Tabelle einfügen und die Felder dem vorhandenen Schema zuordnen.

Sie können mit der Mehrfachauswahl in der Zuordnungstabelle mehrere Spalten mit nur einem Klick verknüpfen, die Verknüpfung mehrerer Spalten aufheben oder mehrere Zeilen demselben Spaltennamen zuordnen.

Wenn Sie die eingehenden Felder immer übernehmen und einem Ziel wie vorhanden zuordnen möchten, legen Sie die Einstellung „Schemaabweichung zulassen“ fest.

![Feldzuordnung](media/data-flow/multi1.png "mehrere Optionen")

Wenn Sie Ihre Spaltenzuordnungen zurücksetzen möchten, wählen Sie die Schaltfläche „Neu zuordnen“ aus, um die Zuordnungen zurückzusetzen.

![Sink-Optionen](media/data-flow/sink1.png "Senke Eins")

![Sink-Optionen](media/data-flow/sink2.png "Senken")

* Die Optionen „Schemaabweichung zulassen“ und „Schema prüfen“ stehen jetzt in Sink zur Verfügung. Dadurch können Sie ADF anweisen, flexible Schemadefinitionen (Schemaabweichung) entweder vollständig zu akzeptieren, oder die Senke zu verwerfen, wenn das Schema sich ändert (Schema prüfen).

* Löschen Sie den Ordner. ADF wird den Inhalt des Senkenordners abschneiden, bevor die Zieldateien in diesen Zielordner geschrieben werden.

## <a name="file-name-options"></a>Dateinamenoptionen

   * Standardwert: Lassen Sie zu, dass Spark Dateien basierend auf den PART-Standardeinstellungen benennt.
   * Muster: Geben Sie einen Namen für Ihre Ausgabedateien ein.
   * Pro Partition: Geben Sie einen Dateinamen pro Partition ein.
   * Wie Daten in Spalte: Legen Sie die Ausgabedatei auf den Wert einer Spalte fest.

> [!NOTE]
> Dateivorgänge werden nur ausgeführt, wenn Sie die Aktivität „Datenfluss ausführen“ außerhalb des Data Flow-Debugmodus ausführen.

## <a name="database-options"></a>Datenbankoptionen

* Einfüge-, Aktualisierungs-, Lösch- und Upsert-Vorgänge von Daten zulassen. Die Standardeinstellung ist das Zulassen von Einfügevorgängen. Wenn Sie für Zeilen Aktualisierungs-, Upsert- oder Einfügevorgänge verwenden möchten, müssen Sie zunächst eine Transformation zur Änderung von Zeilen hinzufügen, um Zeilen für diese spezifischen Aktionen zu kennzeichnen.
* Tabelle abschneiden (entfernt vor dem Abschließen des Datenflusses alle Zeilen aus der Zieltabelle)
* Tabelle neu erstellen (wendet vor dem Abschließen des Datenflusses Lösch-/Erstellvorgänge auf die Zieltabelle an)
* Batchgröße für große Datenmengen. Geben Sie eine Zahl ein, um Schreibvorgänge in Blöcke zu unterteilen.
* Staging aktivieren: Dadurch wird ADF angewiesen, Polybase zu verwenden, wenn Sie Azure Data Warehouse als Ihren Senkendatensatz laden.

![SQL-Senkenoptionen](media/data-flow/alter-row2.png "Senkenoptionen")

> [!NOTE]
> Beim Aktualisieren oder Löschen von Zeilen in Ihrer Datenbanksenke müssen Sie die Schlüsselspalte festlegen. Auf diese Weise kann ALTER ROW die eindeutige Zeile in der DML bestimmen.

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun Ihren Datenfluss erstellt haben, fügen Sie eine [Aktivität zur Ausführung eines Datenflusses Ihrer Pipeline hinzu](concepts-data-flow-overview.md).
