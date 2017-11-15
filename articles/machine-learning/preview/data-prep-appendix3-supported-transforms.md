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
ms.openlocfilehash: d0b62a63d381239e17b7dccceb89171bca15a68e
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Verwenden von Datentransformationen zur Datenvorbereitung in Azure Machine Learning

Eine *Transformation* in Azure Machine Learning nutzt Daten in einem bestimmten Format, wendet einen Vorgang auf die Daten an (z.B. Ändern des Datentyps) und generiert dann Daten im neuen Format. Jede Transformation weist eine eigene Schnittstelle und ein eigenes Verhalten auf. Durch Verketten mehrerer Transformationen anhand von Schritten im Datenfluss können Sie komplexe und wiederholbare Transformationen auf Ihre Daten anwenden. Dies ist der Kernpunkt der Datenaufbereitungsfunktionalität.

Es folgen die in Azure Machine Learning verfügbaren Transformationen. 

## <a name="column-selection"></a>Spaltenauswahl 
Viele Transformationen in dieser Liste funktionieren mit einer einzelnen Spalte oder mit mehreren. Um mehrere Spalten auszuwählen, verwenden Sie die STRG-Taste. Um einen Bereich von Spalten auszuwählen, verwenden Sie die UMSCHALTTASTE.

## <a name="transforms-from-the-main-menu-or-the-grid-header"></a>Transformationen im Hauptmenü oder Rasterheader 
Greifen Sie im Hauptmenü über die Option „Transformationen“ auf Transformationen zu. Sie können Transformationen auch durch Klicken mit der rechten Maustaste auf den Spaltennamen im Datenraster auswählen. Wenn mehrere Spalten ausgewählt sind, wird durch Klicken mit der rechten Maustaste ein Menü für Transformationen angezeigt.

Im Kontextmenü werden ausschließlich gültige Transformationen für den ausgewählten Datentyp angeboten. Das Hauptmenü bietet alle Transformationen an, deaktiviert aber die für die markierten Spalten nicht relevanten.

Eine kleine Teilmenge der kontextbezogenen Transformationen ist verfügbar, wenn Sie mit der rechten Maustaste in eine Zelle klicken. Diese Transformationen sind Kopieren, Ersetzen, und Filtern. Hierbei wird der Datentyp berücksichtigt, sodass sich die Optionen für eine Zahlenspalte und eine Zeichenfolgenspalte unterscheiden.

## <a name="derive-column-by-example"></a>Derive Column by Example (Spalte nach Beispiel ableiten)
Mit dieser Transformation können Sie eine neue Spalte als Ableitung von vorhandenen Spalten erstellen. Die Transformation untersucht die eingegebenen (ausgewählten) Spalten und das zugehörige Beispiel und ermittelt dann die gewünschte Ausgabe in der neuen Spalte. 

Wählen Sie mindestens eine Spalte aus, um diese Transformation zu verwenden. Fügen Sie eine neue (leere) abgeleitete Spalte anhand eines Beispiels hinzu. Geben Sie ein Beispiel dafür ein, was in der abgeleiteten Spalte angezeigt werden soll (angenommen, dass sie von anderen Spalten abgeleitet ist). Mit der „By Example“-Technologie, also anhand von Beispielen, wird dann versucht, alle anderen Zellen der Spalte zu füllen. 

Bei komplizierten Beispielen müssen Sie eventuell mehrere Beispiele angeben. Wählen Sie eine weitere Zelle aus, und geben Sie ein weiteres Beispiel ein.

Bei der „By Example“-Technologie werden ausgewählte Spalten verwendet, um zu versuchen, die Bedeutung eines Beispiels zu ermitteln. Wenn beim Aufrufen dieser Transformation keine Spalten ausgewählt sind, werden alle Zellen für die aktuelle Zeile verwendet. Wenn Sie nur die erforderlichen Spalten auswählen, führt dies zu genaueren Ergebnissen.

Sie können Spalten auswählen, bevor Sie die Transformation aufrufen. Wenn der Editor für Transformationen geöffnet wurde, können Sie anhand von Kontrollkästchen oben in jeder Spalte erkennen, welche Spalten als Eingaben ausgewählt wurden. Mithilfe der Kontrollkästchen in den Spaltenüberschriften können Sie Spalten zur Auswahl hinzufügen oder daraus entfernen.

Eine ausführlichere Beschreibung der Transformation **Derive Column by Example** (Spalte nach Beispiel ableiten) sowie weitere Beispiele finden Sie unter [Transformation „Derive Column by Example“](data-prep-derive-column-by-example.md).  

## <a name="split-column-by-example"></a>Split Column by Example (Spalte nach Beispiel teilen)
Bei dieser Transformation wird eine vorhandene Spalte verwendet und dann mit dem „By Example“-Modul versucht, diese Spalte in *n* andere Spalten aufzuteilen. Sie können das automatische Teilen für die nachfolgend generierten Spalten ausführen.

Eine ausführlichere Beschreibung der Transformation **Split Column by Example** (Spalte nach Beispiel teilen) und weitere Beispiele finden Sie unter [Transformation „Aufteilen von Spalten anhand eines Beispiels“](data-prep-split-column-by-example.md).

## <a name="expand-json"></a>Expand JSON (JSON erweitern)

Mit dieser Transformation können Benutzer mehrere Spalten hinzufügen, indem sie eine Spalte mit gültigem JSON-Text erweitern.

Eine ausführlichere Beschreibung der Transformation **JSON erweitern** und weitere Beispiele finden Sie unter [Transformation „JSON erweitern“](data-prep-expand-json.md).


## <a name="combine-columns-by-example"></a>Combine Columns by Example (Spalten nach Beispiel kombinieren)

Diese Transformation fügt eine neue Spalte hinzu, indem Werte aus mehreren Spalten kombiniert werden. 

Eine ausführlichere Beschreibung der Transformation vom Typ **Combine Columns by Example** (Spalten nach Beispiel kombinieren) sowie weitere Beispiele finden Sie unter [Transformation „Spalten nach Beispiel kombinieren“](data-prep-combine-columns-by-example.md).


## <a name="duplicate-column"></a>Duplicate Column (Spalte duplizieren)
Mit dieser Transformation wird eine genaue Kopie einer oder mehrerer ausgewählter Spalten erstellt und dafür ein neuer Name vergeben, der von den ursprünglichen Spalten abgeleitet ist.

## <a name="text-clustering"></a>Text Clustering (Text-Clustering) 
Diese Transformation ist dafür ausgelegt, inkonsistente Werte, die identisch sein sollten, zu gruppieren.  

Mit dieser Transformation werden die Werte in einer einzelnen Spalte auf Ähnlichkeit untersucht und in Clustern gruppiert. Für jeden Cluster ist ein kanonischer Wert vorhanden. Dies ist der Wert, durch den alle Instanzen im Cluster und alle Instanzwerte ersetzt werden. Es können vollständige Cluster entfernt werden, und der kanonische Wert kann bearbeitet werden. Instanzen können aus einem bestimmten Cluster entfernt werden. Der Filter für den Schwellenwert des Ähnlichkeitsergebnisses, der zum Gruppieren von Instanzen in einem Cluster verwendet wird, kann geändert werden.

Standardmäßig werden mit dieser Transformation alle Clusterinstanzwerte durch den kanonischen Wert für diesen Cluster ersetzt, und für die neuen Werte wird eine neue Spalte erstellt. Sie können das Ähnlichkeitsergebnis für jede Instanz auch einer neuen Spalte (die benannt werden kann) hinzuzufügen, damit es später im Datenfluss genutzt werden kann.

## <a name="replace-values"></a>Replace Values (Werte ersetzen)
Verwenden Sie diese Transformation, um eine Zeichenfolge durch eine andere zu ersetzen. Die Quellzeichenfolge kann eine Teilzeichenfolge oder eine vollständige Zelle sein, und die Ersetzung kann für eine einzelne oder mehrere Spalten gelten. Die Suchzeichenfolge unterstützt das Suchen nach Sonderzeichen und nach regulären Zeichen. 

## <a name="replace-na-values"></a>Replace NA Values (NA-Werte ersetzen)
Verwenden Sie diese Transformation, um verschiedene Formen von „NA“ (Nicht zutreffend, im Englischen: N/A, NA, null, NaN usw.) oder von leeren Zeichenfolgen durch einen bestimmten Wert zu ersetzen, sodass sie einheitlich sind. Diese Transformation unterstützt eine oder mehrere Spalten und wird nur aufgelistet, wenn eine Spalte ausgewählt ist. Sie ist nicht im Hauptmenü für Transformationen enthalten, wenn keine Spalten ausgewählt sind.

## <a name="replace-missing-values"></a>Replace Missing Values (Fehlende Werte ersetzen)
Diese Transformation dient zum Ersetzen fehlender Daten durch einen einzelnen Wert und unterstützt eine oder mehrere Spalten. Diese Transformation wird nur aufgelistet, wenn eine Spalte ausgewählt ist. Sie ist nicht im Hauptmenü für Transformationen enthalten, wenn keine Spalten ausgewählt sind.

## <a name="replace-error-values"></a>Replace Error Values (Fehlerwerte ersetzen)
Diese Transformation dient zum Ersetzen von Fehlern durch einen einzelnen Wert und unterstützt eine oder mehrere Spalten. Diese Transformation wird nur aufgelistet, wenn eine Spalte ausgewählt ist. Sie ist nicht im Hauptmenü für Transformationen enthalten, wenn keine Spalten ausgewählt sind.

## <a name="trim-string"></a>Trim String (Zeichenfolge kürzen)
Diese Transformation dient zum Entfernen von vor- und nachgestellten Leerstellen (Leerzeichen, Tabstopps usw.) aus einer oder mehreren Spalten.

## <a name="adjust-precision"></a>Adjust Precision (Genauigkeit anpassen)
Diese Transformation legt die Anzahl von Dezimalstellen für eine numerische Spalte fest.

## <a name="rename-column"></a>Rename Column (Spalte umbenennen)
Diese Transformation ändert den Namen der ausgewählten Spalte. Sie können sie auch inline in der Spaltenüberschrift aufrufen, indem Sie auf den Namen der Spalte klicken.

## <a name="remove-column"></a>Remove Column (Spalte entfernen)
Diese Transformation entfernt die ausgewählten Spalten und kann für eine oder mehrere Spalten verwendet werden. 

## <a name="keep-column"></a>Keep Column (Spalte beibehalten)
Diese Transformation behält ausschließlich die ausgewählten Spalten bei und kann für eine oder mehrere Spalten verwendet werden.

## <a name="handle-path-column"></a>Handle Path Column (Pfadspalte verarbeiten)
Beim Importieren einer Datei wird dem DataSet durch das Feature **Datenquelle hinzufügen** automatisch eine Pfadspalte hinzugefügt. Sie enthält den vollqualifizierten Dateinamen, aus dem der Pfad zum DataSet gebildet wird. Mit dieser Transformation wird diese zusätzliche Spalte entweder dem DataSet hinzugefügt oder aus diesem entfernt.

## <a name="convert-field-type-to-numeric"></a>Convert Field Type to Numeric (Feldtyp in „Numerisch“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „Numerisch“. Sie können das Trennzeichen für nicht ganzzahlige Daten angeben. Standardmäßig werden Sie bei dieser Transformation nicht zum Angeben des Trennzeichens aufgefordert. Rufen Sie daher über die Menüoption **Bearbeiten** den Editor auf. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

## <a name="convert-field-type-to-date"></a>Convert Field Type to Date (Feldtyp in „Datum“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „Datum“. Es wird ein Standardformat für Datum/Uhrzeit verwendet, dieses kann jedoch mit `strftime`-Direktiven außer Kraft gesetzt werden. Sie können Zeitwerten auch ein festes Datum voranstellen.

Standardmäßig werden Sie bei dieser Transformation nicht zum Angeben des Formats aufgefordert. Verwenden Sie im sich ergebenden Schritt daher die Menüoption **Bearbeiten**, um den Editor aufzurufen. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

Nur Datumsangaben zwischen 9-22-1677 und 4-11-2262 können in den Datumstyp konvertiert werden.

## <a name="convert-field-type-to-boolean"></a>Convert Field Type to Boolean (Feldtyp in „Boolesch“ konvertieren)
Diese Transformation dient zum Ändern des Spaltentyps in „true/false“. Sie unterstützt die Zuordnung von mehreren Werten zu TRUE oder FALSE. Sie können diese Zuordnungen auch bearbeiten. Außerdem wird eine Konstante unterstützt, der Werte zugeordnet werden können, die in den TRUE/FALSE-Zuordnungstabellen nicht vorhanden sind. Diese Transformation kann für eine oder mehrere Spalten verwendet werden.

## <a name="convert-field-type-to-string"></a>Convert Field Type to String (Feldtyp in „Zeichenfolge“ konvertieren)
Diese Transformation ändert den Spaltentyp in „Zeichenfolge“ und kann auf eine einzelne oder mehrere Spalten angewendet werden.

## <a name="convert-unix-timestamp-to-datetime"></a>Convert Unix Timestamp to DateTime (Unix-Zeitstempel in DateTime konvertieren)
Diese Transformation versteht das Unix-Zeitstempelformat, auch wenn es in den Daten als Zeichenfolge dargestellt ist. Der Zeitstempel wird ordnungsgemäß in einen Datentyp der Datenvorbereitung konvertiert.

## <a name="filter"></a>Filter
Diese Transformation unterstützt das Filtern von Zeilen anhand der Werte in einer oder mehreren Spalten. Die Bedingungen des Filters richten sich nach dem Datentyp der jeweiligen Spalte, sodass Sie Zeichenfolgenspalten nach „enthält“ oder „enthält nicht“ filtern können. Numerische Spalten können nach den Bedingungen „Größer als“ oder „Kleiner als“ gefiltert werden.

Wenn die Transformation **Filtern** über das Hauptmenü oder das Kontextmenü eines Spaltenkopfs aufgerufen wird, ist die Option zum Umleiten der fehlerhaften Zeilen in einen anderen Datenfluss verfügbar. Der Hauptdatenfluss wird mit den gefilterten Zeilen (**in**) fortgesetzt, und es wird ein neuer Datenfluss erstellt, der alle herausgefilterten Zeilen (**out**) enthält.

## <a name="use-first-row-as-headers"></a>Erste Zeile als Überschriften verwenden
Mit dieser Transformation wird die erste Zeile des Datasets höher gestuft, um als Spaltenname zu fungieren. Wenn einige Spalten in der ersten Zeile keine Daten enthalten, wird automatisch ein Name generiert. Die Nutzung dieser Transformation bedeutet, dass der Import der Daten frühestens in Zeile 2 beginnt. Je nach der Einstellung unter **Zeilen überspringen** kann der Importvorgang auch weiter unten im DataSet beginnen. Außerdem kann hiermit die Höherstufung entfernt werden, sodass nur automatisch generierte Namen verwendet werden.

## <a name="join"></a>Join
Diese Transformation wird verwendet, um zwei Datenflüsse miteinander zu verknüpfen. Sie können auswählen, welche Ausgabe der Verknüpfung das Ergebnis sein soll: die erfolgreichen Zeilen der Verknüpfung, die fehlerhaften Zeilen auf der linken Seite oder die fehlerhaften Zeilen auf der rechten Seite der Verknüpfung.

Die Transformation **Verknüpfen** wird über einen einzelnen Datenfluss gestartet und wählt diesen Datenfluss als eine Seite der Verknüpfung aus. Anschließend werden Sie zum Auswählen eines weiteren Datenflusses für die andere Seite der Verknüpfung aufgefordert. Nachdem Sie die beiden Datenflüsse ausgewählt haben, muss für die Transformation eine einzelne Spalte auf jeder Seite der Verknüpfung ausgewählt werden, die jeweils für die Verknüpfung genutzt wird. Wenn für die Verknüpfung mehr als eine Spalte benötigt wird, sollten Sie vor dem Starten der Transformation eine abgeleitete Spalte erstellen. So erhalten Sie eine neue verkettete Spalte, die ausschließlich für die Verknüpfung dient. Die Transformation versucht, Schlüssel für die Verknüpfung vorzuschlagen und die abgeleitete Spalte automatisch zu generieren, falls dies möglich ist.

Nach Abschluss der Verknüpfung wird eine Sankey-Diagrammansicht der Verknüpfung angezeigt. Die relative Breite der Linien zueinander steht für die Anzahl von Zeilen, die diesen Teil des Join-Datenflusses durchlaufen. Über den Bereich auf der rechten Seite können Sie nur die erfolgreichen Zeilen oder die fehlerhaften Zeilen links oder rechts auswählen. Sie können auch eine einzelne Verzweigung auswählen.

## <a name="append-rows"></a>Append Rows (Zeilen anfügen)
Mit dieser Transformation werden Daten aus einem anderen Datenfluss an den aktuellen Datenfluss angefügt. Die Spalten werden anhand ihrer Position zugeordnet, um die neuen Zeilen am Ende hinzuzufügen.

## <a name="append-columns"></a>Append Columns (Spalten anfügen)
Mit dieser Transformation werden neue Spalten aus einem anderen Datenfluss an den aktuellen Datenfluss angefügt. Die neuen Spalten werden rechts hinzugefügt. Es wird nicht versucht, Daten in Zeilen auszurichten.

## <a name="summarize"></a>Summarize (Zusammenfassen)
Diese Transformation berechnet Aggregate für die Kombination eindeutiger Werte in einer oder mehreren ausgewählten Spalten. 

Die folgenden Aggregate werden unterstützt: COUNT, SUM, MIN, MAX, MEAN, VARIANCE, STANDARD DEVIATION. Die Liste mit den Aggregaten für eine bestimmte Spalte wird gefiltert, um nur die Aggregate zu erhalten, die für den jeweiligen Datentyp gelten. Aggregate, die ungültig sind, sind deaktiviert. Es ist beispielsweise nicht möglich, den Mittelwert einer Zeichenfolgenspalte zu berechnen, der niedrigste und höchste Wert können jedoch berechnet werden.

Wenn der Editor verfügbar ist, ziehen Sie vom Spaltenkopf nach oben in den Bereich links oben, wo die zu aggregierenden Spalten angezeigt werden. Da dieser Bereich hierarchisch aufgebaut ist, sind geschachtelte Aggregate möglich. Im Editor-Bereich rechts oben wird ausgewählt, welche Aggregate auf eine Spalte angewendet werden sollen. Eine einzelne Spalte kann einmal oder mehrfach aggregiert werden. Nachdem mindestens ein Aggregat ausgewählt wurde, werden die Daten im Raster rechts unten in Aggregatform als Vorschau angezeigt. 

Diese Transformation ist analog zu `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Remove Duplicates (Duplikate entfernen)
Mit dieser Transformation wird die gesamte Zeile entfernt, wenn sie in einer oder mehreren ausgewählten Spalten doppelte Werte enthält. Wenn keine Spalten ausgewählt sind, werden nur die Spalten entfernt, in denen alle Spaltenwerte identisch sind.

## <a name="sort"></a>Sortieren
Mit dieser Transformation werden die Daten sortiert. Dies kann basierend auf einer einzelnen oder auf mehreren Spalten erfolgen, und jede Spalte kann aufsteigend (Standardeinstellung) oder absteigend sortiert werden (kann im Editor geändert werden).

Diese Transformation ist analog zu `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Transformationen für die Ausgabe
Mit dieser Transformation werden Ausgabedaten transformiert. Es können mehrere Schreibblöcke in einem einzelnen Fluss enthalten sein, damit die Daten an unterschiedlichen Punkten geschrieben werden.

### <a name="write-to-csv"></a>In CSV schreiben
Mit dieser Transformation werden die Daten im CSV-Format vom aktuellen Punkt des Datenflusses geschrieben. Sie steuert den Speicherort (lokal oder remote) und verschiedene Einstellungen für die Datei.

### <a name="write-to-parquet"></a>Write to Parquet (In Parquet schreiben)
Mit dieser Transformation werden die Daten im Parquet-Format vom aktuellen Punkt des Datenflusses geschrieben. Sie steuert den Speicherort (lokal oder remote) und verschiedene Einstellungen für die Datei.

## <a name="script-based-transforms"></a>Skriptbasierte Transformationen
Für die folgenden Transformationen werden Skripts (Python) verwendet, um Funktionen durchzuführen, die im Hauptprodukt fehlen. 

>[!NOTE]
>Lesen Sie vor der Verwendung dieser Transformationen das Dokument [Nutzen der Erweiterbarkeit von Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Spalte hinzufügen (Skript)
Diese Transformation fügt den Daten mit einem Python-Ausdruck eine Spalte hinzu.
Weitere Informationen finden Sie unter [Beispiel für benutzerdefinierte Spaltentransformationen (Python)](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Erweiterter Filter (Skript)
Verwenden Sie diese Transformation, um einen Filter auf Zeilenebene in Python zu schreiben.
Weitere Informationen finden Sie unter [Beispiel für Filterausdrücke (Python)](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Datenfluss transformieren (Skript)
Diese Transformation wendet Python auf das gesamte DataSet an.
Weitere Informationen finden Sie unter [Beispiel für benutzerdefinierte Datenflusstransformationen (Python)](data-prep-appendix7-sample-transform-data-flow-python.md).

Diese Transformation kann Python auch auf eine gesamte Datenpartition anwenden.
Weitere Informationen finden Sie unter [Beispiel für benutzerdefinierte Datenflusstransformationen (Python)](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Datenfluss schreiben (Skript)
Diese Transformation schreibt mit Python ein ganzes DataSet.
Weitere Informationen finden Sie unter [Beispiele für Zielverbindungen (Python)](data-prep-appendix9-sample-destination-connections-python.md).



