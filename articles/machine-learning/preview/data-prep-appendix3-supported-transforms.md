---
title: Verwenden von Datentransformationen zur Datenvorbereitung in Azure Machine Learning | Microsoft-Dokumentation
description: "Dieser Artikel enthält eine vollständige Liste mit Transformationen, die für die Datenvorbereitung von Azure Machine Learning verfügbar sind."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: 4331bb4a16d56f027dd63a97868822fa6ecc92d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Verwenden von Datentransformationen zur Datenvorbereitung in Azure Machine Learning

Eine *Transformation* in Azure Machine Learning nutzt Daten in einem bestimmten Format, wendet einen Vorgang auf die Daten an (z.B. Ändern des Datentyps) und generiert dann Daten im neuen Format. Jede Transformation weist eine eigene Schnittstelle und ein eigenes Verhalten auf. Über Schritte im Datenfluss können Sie mehrere Transformationen miteinander verketten, sodass Sie komplexe und wiederholbare Transformationen auf Ihre Daten anwenden können. Dies ist der Kernpunkt der Datenaufbereitungsfunktionalität.

Es folgt eine Liste der Transformationen, die in Azure Machine Learning verfügbar sind. 

## <a name="column-selection"></a>Spaltenauswahl 
Viele der unten aufgeführten Transformationen funktionieren für eine einzelne oder auch mehrere Spalten. Um mehrere Spalten zu markieren, verwenden Sie die **STRG**-TASTE, oder wählen Sie einen Spaltenbereich mithilfe der **UMSCHALTTASTE** aus.

## <a name="transforms-from-main-menu-andor-grid-header"></a>Transformationen im Hauptmenü bzw. Rasterheader 
Sie können auf Transformationen im Hauptmenü über die Option „Transformationen“ zugreifen. Das Auswählen von Transformationen ist auch möglich, indem Sie im Datenraster mit der rechten Maustaste auf den Spaltennamen klicken. Falls mehrere Spalten ausgewählt sind, wird ein Menü für Transformationen angezeigt, sobald Sie mit der rechten Maustaste auf eine der Spalten klicken.

Im Kontextmenü werden ausschließlich gültige Transformationen für den ausgewählten Datentyp angeboten. Das Hauptmenü bietet alle Transformationen an, deaktiviert aber die Transformationen, die für die markierten Spalten nicht relevant sind.

Eine kleine Teilmenge der kontextbezogenen Transformationen ist verfügbar, wenn Sie mit der rechten Maustaste in eine Zelle klicken. Diese Transformationen sind Kopieren, Ersetzen, und Filtern. Hierbei wird der Datentyp berücksichtigt, sodass sich die Optionen für eine Zahlenspalte und eine Zeichenfolgenspalte unterscheiden.

## <a name="derive-column-by-example"></a>Derive Column by Example (Spalte nach Beispiel ableiten)
Diese Transformation ermöglicht die Erstellung einer neuen Spalte als Ableitung von einer oder mehreren vorhandenen Spalten. Die Transformation untersucht die eingegebenen (ausgewählten) Spalten und das dazugehörige Beispiel und ermittelt dann die gewünschte Ausgabe in der neuen Spalte. 

Wählen Sie mindestens eine Spalte aus, um diese Transformation zu verwenden. Fügen Sie eine neue (leere) abgeleitete Spalte anhand eines Beispiels hinzu. Geben Sie ein Beispiel dafür ein, was in der abgeleiteten Spalte angezeigt werden soll (es wird angenommen, dass sie von anderen Spalten abgeleitet ist). Mit der „By Example“-Technologie, also anhand von Beispielen, wird dann versucht, alle anderen Zellen der Spalte zu füllen. 

Bei komplizierten Beispielen ist es ggf. erforderlich, mehr als ein Beispiel anzugeben. Wählen Sie eine weitere Zelle aus, und geben Sie ein weiteres Beispiel ein.

Bei der „By Example“-Technologie werden ausgewählte Spalten verwendet, um zu versuchen, die Bedeutung eines Beispiels zu ermitteln. Wenn beim Aufrufen dieser Transformation keine Spalten ausgewählt werden, werden alle Zellen für die aktuelle Zeile verwendet. Wenn Sie nur die erforderlichen Spalten auswählen, führt dies zu genaueren Ergebnissen.

Sie können Spalten auswählen, bevor Sie die Transformation aufrufen. Nachdem der Editor für Transformationen gestartet wurde, können Sie anhand von Kontrollkästchen oben in jeder Spalte erkennen, welche Spalten als Eingaben ausgewählt wurden. Mithilfe der Kontrollkästchen in den Spaltenüberschriften können Sie Spalten zur Auswahl hinzufügen oder daraus entfernen.

Eine ausführlichere Beschreibung der Transformation vom Typ **Derive Column By Example** (Spalte nach Beispiel ableiten) sowie weitere Beispiele finden Sie unter [Derive Column by Example Reference](data-prep-derive-column-by-example.md) (Spalte nach Beispiel ableiten – Referenz).  

## <a name="split-column-by-example"></a>Split Column by Example (Spalte nach Beispiel teilen)
Von dieser Transformation wird eine vorhandene Spalte verwendet und dann mit dem „By Example“-Modul versucht, diese Spalte in *n* andere Spalten aufzuteilen. Hierbei ist es möglich, das automatische Teilen für die nachfolgend generierten Spalten auszuführen.

Eine ausführlichere Beschreibung der Transformation **Split Column By Example** (Spalte nach Beispiel teilen) und weitere Beispiele finden Sie unter [Split Column by Example Reference](data-prep-split-column-by-example.md) (Spalte nach Beispiel teilen – Referenz).

## <a name="expand-json"></a>Expand JSON (JSON erweitern)

Mit dieser Transformation können Benutzer mehrere Spalten hinzufügen, indem sie eine Spalte mit gültigem JSON-Text erweitern.

Eine ausführlichere Beschreibung der Transformation **Expand JSON** (JSON erweitern) und weitere Beispiele finden Sie unter [Expand JSON Reference](data-prep-expand-json.md) (JSON erweitern – Referenz).


## <a name="combine-columns-by-example"></a>Combine Columns by Example (Spalten nach Beispiel kombinieren)

Diese Transformation ermöglicht Ihnen das Hinzufügen einer neuen Spalte, indem Werte aus mehreren Spalten kombiniert werden. 

Eine ausführlichere Beschreibung der Transformation vom Typ **Combine Columns by Example** (Spalten nach Beispiel kombinieren) sowie weitere Beispiele finden Sie unter [Combine Columns by Example Reference](data-prep-combine-columns-by-example.md) (Spalten nach Beispiel kombinieren – Referenz).


## <a name="duplicate-column"></a>Duplicate Column (Spalte duplizieren)
Mit dieser Transformation wird eine genaue Kopie einer oder mehrerer ausgewählter Spalten erstellt und dafür ein neuer Name vergeben, der von den ursprünglichen Spalten abgeleitet ist.

## <a name="text-clustering"></a>Text Clustering (Text-Clustering) 
Diese Transformation ist dafür ausgelegt, inkonsistente Werte, die identisch sein sollten, zu gruppieren.  

Beim Ausführen dieser Transformation werden die Werte in einer einzelnen Spalte auf Ähnlichkeit untersucht und in Clustern gruppiert. Für jeden Cluster ist ein kanonischer Wert vorhanden. Dies ist der Wert, durch den alle Instanzen im Cluster und die Instanzwerte ersetzt werden. Vollständige Cluster können entfernt und der kanonische Wert kann bearbeitet werden. Instanzen können aus einem bestimmten Cluster entfernt werden. Außerdem kann der Filter für den Schwellenwert des Ähnlichkeitsergebnisses, der zum Gruppieren von Instanzen in einem Cluster verwendet wird, geändert werden.

Standardmäßig werden mit dieser Transformation alle Clusterinstanzwerte durch den kanonischen Wert für diesen Cluster ersetzt, und für die neuen Werte wird eine neue Spalte erstellt. Es ist auch möglich, das Ähnlichkeitsergebnis für jede Instanz einer neuen Spalte (die benannt werden kann) hinzuzufügen, damit sie später im Datenfluss genutzt werden kann.

## <a name="replace-values"></a>Replace Values (Werte ersetzen)
Mit dieser Transformation kann eine Zeichenfolge durch eine andere Zeichenfolge ersetzt werden. Die Quellzeichenfolge kann eine Teilzeichenfolge oder eine vollständige Zelle sein. Die Ersetzung kann für eine einzelne oder mehrere Spalten gelten. Die Suchzeichenfolge unterstützt das Suchen nach Sonderzeichen und nach regulären Zeichen. 

## <a name="replace-na-values"></a>Replace NA Values (NA-Werte ersetzen)
Diese Transformation ermöglicht das Ersetzen der verschiedenen Formen von „NA“ (Nicht verfügbar, im Englischen: N/A, NA, null, NaN usw.) oder von leeren Zeichenfolgen durch einen bestimmten Wert, um Einheitlichkeit herzustellen. Diese Transformation unterstützt eine oder mehrere Spalten. Diese Transformation wird nur aufgeführt, wenn eine Spalte ausgewählt wird und im Hauptmenü mit den Transformationen nicht vorhanden ist, wenn keine Spalten ausgewählt sind.

## <a name="replace-missing-values"></a>Replace Missing Values (Fehlende Werte ersetzen)
Diese Transformation dient zum Ersetzen fehlender Daten durch einen einzelnen Wert. Diese Transformation unterstützt eine oder mehrere Spalten. Diese Transformation wird nur aufgeführt, wenn eine Spalte ausgewählt wird und im Hauptmenü mit den Transformationen nicht vorhanden ist, wenn keine Spalten ausgewählt sind.

## <a name="replace-error-values"></a>Replace Error Values (Fehlerwerte ersetzen)
Diese Transformation dient zum Ersetzen von Fehlern durch einen einzelnen Wert. Diese Transformation unterstützt eine oder mehrere Spalten. Diese Transformation wird nur aufgeführt, wenn eine Spalte ausgewählt wird und im Hauptmenü mit den Transformationen nicht vorhanden ist, wenn keine Spalten ausgewählt sind.

## <a name="trim-string"></a>Trim String (Zeichenfolge kürzen)
Diese Transformation dient zum Entfernen von vor- und nachgestellten Leerzeichen (Leerstellen, Tabulatoren *usw.*) aus einer oder mehreren Spalten.

## <a name="adjust-precision"></a>Adjust Precision (Genauigkeit anpassen)
Diese Transformation ermöglicht das Festlegen der Anzahl von Dezimalstellen für eine numerische Spalte.

## <a name="rename-column"></a>Rename Column (Spalte umbenennen)
Diese Transformation ändert den Namen der ausgewählten Spalte. Diese Transformation kann auch inline in der Spaltenüberschrift aufgerufen werden, indem Sie auf den Namen der Spalte klicken.

## <a name="remove-column"></a>Remove Column (Spalte entfernen)
Diese Transformation entfernt die ausgewählten Spalten. Diese Transformation kann für eine oder mehrere Spalten verwendet werden. 

## <a name="keep-column"></a>Keep Column (Spalte beibehalten)
Bei dieser Transformation werden nur die ausgewählten Spalten beibehalten. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

## <a name="handle-path-column"></a>Handle Path Column (Pfadspalte verarbeiten)
Beim Importieren einer Datei wird dem Dataset vom Assistenten zum Hinzufügen von Datenquellen automatisch eine Pfadspalte hinzugefügt. Sie enthält den vollqualifizierten Dateinamen, aus dem der Pfad zum Dataset gebildet wird. Mit dieser Transformation wird diese zusätzliche Spalte dem Dataset entweder hinzugefügt oder daraus entfernt.

## <a name="convert-field-type-to-numeric"></a>Convert Field Type to Numeric (Feldtyp in „Numerisch“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „Numerisch“. Sie können das Trennzeichen angeben, falls keine Daten vorhanden sind, die keine ganzen Zahlen sind. Standardmäßig werden Sie bei dieser Transformation nicht zum Angeben des Trennzeichens aufgefordert. Rufen Sie über das Menüelement **Bearbeiten** den Editor auf. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

## <a name="convert-field-type-to-date"></a>Convert Field Type to Date (Feldtyp in „Datum“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „Datum“. Es wird ein Standardformat für Datum/Uhrzeit verwendet, aber Sie können es mit `strftime`-Direktiven außer Kraft setzen. Es ist auch möglich, Zeitwerten ein festes Datum voranzustellen.

Standardmäßig werden Sie bei dieser Transformation nicht zum Angeben des Formats aufgefordert. Verwenden Sie im sich ergebenden Schritt das Menüelement **Bearbeiten**, um den Editor aufzurufen. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

Nur Datumsangaben zwischen 9-22-1677 und 4-11-2262 können in den Datumstyp konvertiert werden.

## <a name="convert-field-type-to-boolean"></a>Convert Field Type to Boolean (Feldtyp in „Boolesch“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „true/false“. Diese Transformation unterstützt die Zuordnung von mehreren Werten zu „true“ oder „false“. Sie können diese Zuordnungen bearbeiten. Außerdem wird eine Konstante unterstützt, der Werte zugeordnet werden können, die in den „true/false“-Zuordnungstabellen nicht vorhanden sind. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

## <a name="convert-field-type-to-string"></a>Convert Field Type to String (Feldtyp in „Zeichenfolge“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „Zeichenfolge“. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

## <a name="convert-unix-timestamp-to-datetime"></a>Convert Unix Timestamp to DateTime (Unix-Zeitstempel in DateTime konvertieren)
Diese Transformation versteht das Unix-Zeitstempelformat auch dann, wenn es in den Daten als Zeichenfolge dargestellt ist, und führt die korrekte Konvertierung in einen Datentyp der Datenvorbereitung durch.

## <a name="filter"></a>Filter
Diese Transformation unterstützt das Filtern von Zeilen anhand der Werte in einer oder mehreren Spalten. Die Bedingungen des Filters richten sich nach dem Datentyp der jeweiligen Spalte. Daher ist es möglich, Zeichenfolgenspalten nach „contains“ (enthält) oder „does not contain“ (enthält nicht) zu filtern. Numerische Spalten können nach den Bedingungen „Größer als“ oder „Kleiner als“ gefiltert werden.

Wenn die Transformation „Filtern“ über das Hauptmenü oder das Kontextmenü eines Spaltenkopfs aufgerufen wird, ist die Option zum Umleiten der fehlerhaften Zeilen in einen anderen Datenfluss verfügbar. Der Hauptdatenfluss wird dann mit den gefilterten fehlerfreien Zeilen (**in**) fortgesetzt, und es wird ein neuer Datenfluss erstellt, der alle herausgefilterten Zeilen (**out**) enthält.

## <a name="use-first-row-as-headers"></a>Use first row as headers (Erste Zeile als Überschriften verwenden)
Mit dieser Transformation wird die erste Zeile des Datasets höher gestuft, um als Spaltenname zu fungieren. Falls einige Spalten in der ersten Zeile keine Daten enthalten, wird ein Name automatisch generiert. Die Nutzung dieser Transformation bedeutet, dass der Import der Daten frühestens in Zeile 2 beginnt. Je nach der Einstellung unter „Skip Rows“ (Zeilen überspringen) kann der Importvorgang auch weiter unten im Dataset beginnen. Außerdem kann hiermit die Höherstufung entfernt werden, sodass nur automatisch generierte Namen vorhanden sind.

## <a name="join"></a>Join
Diese Transformation wird verwendet, um zwei Datenflüsse miteinander zu verknüpfen. Sie können auswählen, welche Ausgabe der Verknüpfung das Ergebnis sein soll: die erfolgreichen Zeilen der Verknüpfung, die fehlerhaften Zeilen auf der linken Seite oder die fehlerhaften Zeilen auf der rechten Seite der Verknüpfung.

Der Join-Assistent wird über einen einzelnen Datenfluss gestartet und wählt diesen Datenfluss als eine Seite des Join-Vorgangs aus. Anschließend werden Sie zum Angeben eines weiteren Datenflusses für die andere Seite des Join-Vorgangs aufgefordert. Nachdem die beiden Datenflüsse ausgewählt wurden, muss für den Assistenten eine einzelne Spalte auf jeder Seite der Verknüpfung ausgewählt werden, die jeweils für den Join-Vorgang genutzt wird. Falls für die Verknüpfung mehr als eine Spalte benötigt wird, sollten Sie vor dem Starten des Assistenten eine abgeleitete Spalte erstellen. So erhalten Sie eine neue (verkettete) Spalte, die ausschließlich für die Verknüpfung dient. Der Assistent versucht, Schlüssel für die Verknüpfung vorzuschlagen und die abgeleitete Spalte automatisch zu generieren, falls dies möglich ist.

Nach Abschluss des Join-Vorgangs wird eine Sankey-Diagrammansicht der Verknüpfung angezeigt. Die relative Breite der Linien zueinander steht für die Anzahl von Zeilen, die diesen Teil des Join-Datenflusses durchlaufen. Im Bereich auf der rechten Seite können Sie auch nur die erfolgreichen Zeilen oder die fehlerhaften Zeilen links oder rechts auswählen. Sie können auch nur eine „Branch“ (Verzweigung) auswählen.

## <a name="append-rows"></a>Append Rows (Zeilen anfügen)
Mit dieser Transformation werden Daten aus einem anderen Datenfluss an den aktuellen Datenfluss angefügt. Die Spalten werden anhand ihrer Position zugeordnet, um die neuen Zeilen am Ende hinzuzufügen.

## <a name="append-columns"></a>Append Columns (Spalten anfügen)
Mit dieser Transformation werden neue Spalten aus einem anderen Datenfluss an den aktuellen Datenfluss angefügt. Die neuen Spalten werden auf der rechten Seite hinzugefügt, und es wird nicht versucht, die Daten in Zeilen auszurichten.

## <a name="summarize"></a>Summarize (Zusammenfassen)
Diese Transformation berechnet Aggregate für die Kombination eindeutiger Werte in einer oder mehreren ausgewählten Spalten. 

Die folgenden Aggregate werden unterstützt: COUNT, SUM, MIN, MAX, MEAN, VARIANCE, STANDARD DEVIATION. Die Liste mit den Aggregaten für eine bestimmte Spalte wird gefiltert, um nur die Aggregate zu erhalten, die für den jeweiligen Datentyp gelten. Aggregate, die ungültig sind, sind deaktiviert. Es ist beispielsweise nicht möglich, den Mittelwert einer Zeichenfolgenspalte zu berechnen, doch der niedrigste und höchste Wert können berechnet werden.

Sobald der Editor verfügbar ist, können Sie einen Ziehvorgang aus dem Spaltenkopf in den Bereich oben links durchführen. Hier werden die zu aggregierenden Spalten angezeigt. Da dieser Bereich hierarchisch aufgebaut ist, sind geschachtelte Aggregate möglich. Im Editor-Bereich oben rechts wird ausgewählt, welche Aggregate auf eine Spalte angewendet werden sollen. Eine einzelne Spalte kann einmal oder mehrfach aggregiert werden. Nachdem mindestens ein Aggregat ausgewählt wurde, werden die Daten im Raster unten rechts in Aggregatform als Vorschau angezeigt. 

Diese Transformation ist analog zu `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Remove Duplicates (Duplikate entfernen)
Mit dieser Transformation wird die gesamte Zeile entfernt, wenn sie in einer oder mehreren ausgewählten Spalten doppelte Werte enthält. Wenn keine Spalten ausgewählt werden, werden nur die Spalten entfernt, in denen alle Spaltenwerte identisch sind.

## <a name="sort"></a>Sortieren
Mit dieser Transformation werden die Daten sortiert. Dies kann basierend auf einer einzelnen oder auf mehreren Spalten erfolgen, und jede Spalte kann aufsteigend (Standardeinstellung) oder absteigend sortiert werden, was im Editor geändert werden kann.

Diese Transformation ist analog zu `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Transformationen für die Ausgabe
Mit dieser Transformation werden Ausgabedaten transformiert. In einem einzelnen Fluss können mehrere Schreibblöcke enthalten sein, damit die Daten an unterschiedlichen Punkten geschrieben werden können.

### <a name="write-to-csv"></a>Write To CSV (In CSV schreiben)
Mit dieser Transformation werden die Daten im CSV-Format vom aktuellen Punkt des Datenflusses geschrieben. Sie ermöglicht die Steuerung des Speicherorts (lokal oder remote) und der verschiedenen Einstellungen für die Datei.

### <a name="write-to-parquet"></a>Write to Parquet (In Parquet schreiben)
Mit dieser Transformation werden die Daten im Parquet-Format vom aktuellen Punkt des Datenflusses geschrieben. Sie ermöglicht die Steuerung des Speicherorts (lokal oder remote) und der verschiedenen Einstellungen für die Datei.

## <a name="script-based-transforms"></a>Skriptbasierte Transformationen
Für die folgenden Transformationen werden Skripts (Python) verwendet, um Funktionen durchzuführen, die im Hauptprodukt fehlen. Lesen Sie vor der Verwendung dieser Transformationen das Dokument zur [Erweiterbarkeit von Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Add Column (Spalte hinzufügen) (Skript)
Diese Transformation ermöglicht das Hinzufügen einer Spalte zu den Daten mit einem Python-Ausdruck.
Weitere Informationen finden Sie unter [Beispiel für benutzerdefinierte Spaltentransformationen (Python)](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Advanced Filter (Erweiterter Filter) (Skript)
Diese Transformation ermöglicht das Schreiben eines Filters für die Python-Zeilenebene.
Weitere Informationen finden Sie unter [Beispiel für Filterausdrücke (Python)](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Transform Dataflow (Datenfluss transformieren) (Skript)
Diese Transformation ermöglicht die Anwendung von Python auf das gesamte Dataset.
Weitere Informationen finden Sie unter [Beispiel für benutzerdefinierte Datenflusstransformationen (Python)](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="transform-dataflow-script"></a>Transform Dataflow (Datenfluss transformieren) (Skript)
Diese Transformation ermöglicht die Anwendung von Python auf eine gesamte Datenpartition.
Weitere Informationen finden Sie unter [Beispiel für benutzerdefinierte Datenflusstransformationen (Python)](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Write Dataflow (Datenfluss schreiben) (Skript)
Diese Transformation ermöglicht die Anwendung von Python auf das Ausgeben eines gesamten Datasets.
Weitere Informationen finden Sie unter [Beispiele für Zielverbindungen (Python)](data-prep-appendix9-sample-destination-connections-python.md).



