---
title: 'Azure Data Factory Mapping Data Flow: Sink-Transformation'
description: 'Azure Data Factory Mapping Data Flow: Sink-Transformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 381dc2f9f6d3a074af00ba047472719c086f5811
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408407"
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

### <a name="output-settings"></a>Ausgabeeinstellungen

Überschreiben schneidet die Tabelle ab, falls vorhanden; erstellen Sie sie dann neu, und laden Sie die Daten. Durch Anfügen werden die neuen Zeilen eingefügt. Wenn die Tabelle aus dem Datasettabellennamen im Ziel-ADW überhaupt nicht vorhanden ist, erstellt Data Flow die Tabelle und lädt die Daten.

Wenn Sie die Option „automatisch zuordnen“ deaktivieren, können Sie die Felder manuell der Zieltabelle zuordnen.

![Sink-ADW-Optionen](media/data-flow/adw2.png "ADW Sink")

#### <a name="field-mapping"></a>Feldzuordnung

Auf der Registerkarte „Zuordnung“ Ihrer Sink-Transformation können Sie die eingehenden Spalten (linke Seite) dem Ziel (rechte Seite) zuordnen. Wenn Sie Datenflüsse in Dateien senken, schreibt ADF immer neue Dateien in einen Ordner. Bei Zuordnung zu einem Datenbankdataset können Sie entweder eine neue Tabelle mit diesem Schema generieren (Speicherrichtlinie auf „Überschreiben“ festlegen) oder neue Zeilen in eine vorhandene Tabelle einfügen und die Felder dem vorhandenen Schema zuordnen.

Sie können mit der Mehrfachauswahl in der Zuordnungstabelle mehrere Spalten mit nur einem Klick verknüpfen, die Verknüpfung mehrerer Spalten aufheben oder mehrere Zeilen demselben Spaltennamen zuordnen.

![Feldzuordnung](media/data-flow/multi1.png "mehrere Optionen")

Wenn Sie Ihre Spaltenzuordnungen zurücksetzen möchten, wählen Sie die Schaltfläche „Neu zuordnen“ aus, um die Zuordnungen zurückzusetzen.

![Verbindungen](media/data-flow/maxcon.png "Verbindungen")

### <a name="updates-to-sink-transformation-for-adf-v2-ga-version"></a>Updates für die Sink-Transformation für ADF V2 GA-Version

![Sink-Optionen](media/data-flow/sink1.png "Senke Eins")

![Sink-Optionen](media/data-flow/sink2.png "Senken")

* Die Optionen „Schemaabweichung zulassen“ und „Schema prüfen“ stehen jetzt in Sink zur Verfügung. Dadurch können Sie ADF anweisen, flexible Schemadefinitionen (Schemaabweichung) entweder vollständig zu akzeptieren, oder die Senke zu verwerfen, wenn das Schema sich ändert (Schema prüfen).

* Löschen Sie den Ordner. ADF wird den Inhalt des Senkenordners abschneiden, bevor die Zieldateien in diesen Zielordner geschrieben werden.

* Dateinamenoptionen

   * Standardwert: Lassen Sie zu, dass Spark Dateien basierend auf den PART-Standardeinstellungen benennt.
   * Muster: Geben Sie einen Namen für Ihre Ausgabedateien ein.
   * Pro Partition: Geben Sie einen Dateinamen pro Partition ein.
   * Wie Daten in Spalte: Legen Sie die Ausgabedatei auf den Wert einer Spalte fest.

> [!NOTE]
> Dateivorgänge werden nur ausgeführt, wenn Sie die Aktivität „Datenfluss ausführen“ außerhalb des Data Flow-Debugmodus ausführen.

Mit den SQL-Senkentypen können Sie Folgendes festlegen:

* Tabelle abschneiden.
* Tabelle neu erstellen (führt Löschen/Erstellen aus).
* Batchgröße für große Datenmengen. Eine Zahl eingeben, um Schreibvorgänge in Blöcke zu unterteilen.

![Feldzuordnung](media/data-flow/sql001.png "SQL-Optionen")

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun Ihren Datenfluss erstellt haben, fügen Sie eine [Aktivität zur Ausführung eines Datenflusses Ihrer Pipeline hinzu](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-overview).
