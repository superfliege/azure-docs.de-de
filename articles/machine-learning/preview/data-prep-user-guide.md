---
title: "Ausführliche Anleitung zur Verwendung der Azure Machine Learning-Datenvorbereitung | Microsoft Docs"
description: "Dieses Dokument enthält eine Übersicht sowie detaillierte Informationen zum Lösen von Datenproblemen mit der Azure Machine Learning-Datenvorbereitung."
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
ms.date: 09/07/2017
ms.openlocfilehash: 1a1e12dbb5e32f62266ee6a3cdca9e781569e58c
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="data-preparations-user-guide"></a>Benutzerleitfaden zur Datenvorbereitung 
Die Benutzeroberfläche der Azure Machine Learning-Datenvorbereitung bietet zahlreiche umfangreiche Funktionen. Dieser Artikel beschreibt die komplexesten Bereiche der Benutzeroberfläche.

### <a name="step-execution-history-and-caching"></a>Schrittausführung, Verlauf und Caching 
Aus Leistungsgründen wird im Schrittverlauf der Datenvorbereitung eine Reihe von Caches verwaltet. Wenn Sie einen Schritt auswählen und ein Cache betroffen ist, wird dieser Schritt nicht erneut ausgeführt. Wenn am Ende des Schrittverlaufs ein Schreibblock verfügbar ist und Sie zwischen den einzelnen Schritten hin- und herwechseln, dabei aber keine Änderungen vornehmen, wird das Schreiben nach dem ersten Mal nicht ausgelöst. Es kommt es zu einem neuen Schreibvorgang, und der vorherige Schreibvorgang wird überschrieben, wenn Sie die folgenden Aktionen ausführen:

- Vornehmen von Änderungen am Schreibblock.
- Hinzufügen eines neuen Transformationsblocks und Verschieben dieses Blocks über den Schreibblock. Hierdurch tritt eine Cacheinvalidierung auf.
- Ändern der Eigenschaften eines Blocks über dem Schreibblock. Hierdurch tritt eine Cacheinvalidierung auf.
- Auswählen von „Aktualisieren“ für eine Stichprobe (hierdurch werden alle Caches für ungültig erklärt).

### <a name="error-values"></a>Fehlerwerte

Datentransformationen können für Eingabewerte möglicherweise fehlschlagen, weil diese Werte nicht angemessen verarbeitet werden können. Beispielsweise schlagen Typkoersionen fehl, wenn der Wert der Eingabezeichenfolge nicht in einen bestimmten Zieltyp umgewandelt werden kann. Ein Typkoersionsvorgang kann z.B. die Konvertierung einer Spalte des Typs „string“ (Zeichenfolge) in einen numerischen oder booleschen Typ sein oder der Versuch, eine Spalte zu duplizieren, die nicht vorhanden ist. (Dieser Fehler tritt auf, wenn der Vorgang *Spalte X löschen* vor den Vorgang *Spalte X duplizieren* verschoben wird.)

In diesen Fällen generiert die Datenvorbereitung einen Fehlerwert als Ausgabe. Fehlerwerte weisen darauf hin, dass ein vorheriger Vorgang für den betreffenden Wert fehlgeschlagen ist. Intern werden sie als erstklassige Werttypen behandelt. Allerdings ändert ihr Vorhandensein selbst dann nicht den zugrunde liegenden Typ einer Spalte, wenn eine Spalte ausschließlich aus Fehlerwerten besteht.

Fehlerwerte sind leicht zu identifizieren. Sie werden rot hervorgehoben und als Fehler gekennzeichnet. Um den Grund für den Fehler zu erfahren, zeigen Sie mit dem Mauszeiger auf einen Fehlerwert, damit eine Textbeschreibung des Fehlers angezeigt wird.

Fehlerwerte verbreiten sich. Wenn ein Fehlerwert auftritt, so tritt der Fehler in den meisten Fällen bei einem Großteil der Vorgänge immer wieder auf. Es gibt drei Möglichkeiten, diese Fehlerwerte zu ersetzen oder zu entfernen:

* Replace (Ersetzen)
    -  Klicken Sie mit der rechten Maustaste auf eine Spalte, und wählen Sie **Replace Error Values** (Fehlerwerte ersetzen) aus. Anschließend können Sie einen Ersatzwert für jeden in der Spalte gefundenen Fehlerwert auswählen.

* Remove (Entfernen)
    - Die Datenvorbereitung beinhaltet interaktive Filter, mit denen Fehlerwerte beibehalten oder entfernt werden können.
    - Klicken Sie mit der rechten Maustaste auf eine Spalte, und wählen Sie dann **Filter Column** (Spalte filtern) aus. Um Fehlerwerte beizubehalten oder zu entfernen, erstellen Sie entweder die Bedingung *"is error"* (Fehler) oder die Bedingung *"is not error"* (kein Fehler).

* Verwenden Sie einen Python-Ausdruck, um Fehlerwerte bedingt zu bearbeiten. Weitere Informationen finden Sie im Abschnitt zu [Python-Erweiterungen](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Stichproben
Eine Datenquelldatei nimmt Rohdaten aus anderen Quellen (aus einem lokalen Dateisystem oder einem Remotestandort) auf. Mithilfe des Stichprobenblocks können Sie festlegen, ob sie nur mit einem Teil der Daten arbeiten möchten, wenn Sie Stichproben erzeugen. Wenn Sie nur mit Stichproben aus den Daten anstelle von großen Datensätzen arbeiten, wirkt sich dies positiv auf die Leistung aus, wenn Sie Vorgänge in späteren Schritten ausführen.

Für jede Datenquelldatei können mehrere Stichproben generiert und gespeichert werden. Allerdings kann nur eine Stichprobe als aktive Stichprobe festgelegt werden. Sie können Stichproben im Datenquellen-Assistenten oder durch die Bearbeitung des Stichprobenblocks erstellen, bearbeiten oder löschen. Alle Datenvorbereitungsdateien, die auf eine Datenquelle verweisen, verwenden grundsätzlich die Stichprobe, die in der Datenquelldatei angegeben wurde.

Es gibt eine Reihe von Strategien zur Stichprobenentnahme, die alle unterschiedliche, konfigurierbare Parameter aufweisen.

#### <a name="top"></a>Top
Diese Strategie kann sowohl auf lokale Dateien als auch auf Remotedateien angewendet werden. Die ersten N Zeilen (durch „Count“ (Anzahl) angegeben) werden in die Datenquelle aufgenommen.

#### <a name="random-n"></a>Random N 
Diese Strategie kann nur auf lokale Dateien angewendet werden. N Zeilen (durch „Count“ (Anzahl) angegeben), die zufällig ausgewählt werden, werden in die Datenquelle aufgenommen. Sie können einen bestimmten Startwert festlegen, um sicherzustellen, dass die gleiche Stichprobe generiert wird, solange auch „Count“ (Anzahl) nicht verändert wird.

#### <a name="random-"></a>Random % 
Diese Strategie kann sowohl auf lokale Dateien als auch auf Remotedateien angewendet werden. In beiden Fällen müssen (ähnlich wie bei der Strategie „Random N“) eine Wahrscheinlichkeit und ein Startwert festgelegt werden.

Für Stichproben aus den Remotedateien müssen zusätzliche Parameter angegeben werden:

- Stichprobengenerator 
  - Wählen Sie einen Spark-Cluster oder ein Docker-Remotecomputeziel aus, die verwendet werden sollen, um Stichproben zu entnehmen. Das Computeziel für das Projekt muss im Voraus festgelegt werden, damit es auf dieser Liste erscheint. Führen Sie die in der Anleitung [Verwenden von GPUs in Azure Machine Learning](how-to-use-gpu.md) im Abschnitt „Erstellen eines neuen Computeziels“ beschriebenen Schritte aus, um Computeziele zu erstellen.
- Speichern der Stichproben 
  - Legen Sie einen zwischengeschalteten Speicherort fest, an dem die Remotestichprobe gespeichert werden soll. Dieser Speicherort muss ein anderer als der Speicherort der Eingabedatei sein.

#### <a name="full-file"></a>Vollständige Datei 
Diese Strategie kann nur auf lokale Dateien angewendet werden, indem Sie die vollständige Datei in die Quelldatei aufnehmen. Wenn die Datei zu groß ist, kann diese Option zukünftige Vorgänge in der App verlangsamen. Möglicherweise ist eine andere Stichprobenstrategie besser geeignet.


### <a name="fork-merge-and-append"></a>Forken, Mergen und Anfügen

Wenn Sie einen Filter auf ein Dataset anwenden, teilt dieser die Daten in zwei Resultsets: ein Resultset stellt Datensätze dar, die dem Filter entsprechen, das andere Resultset stellt Datensätze dar, die dem Filter nicht entsprechen. In beiden Fällen kann der Benutzer auswählen, welches Resultset angezeigt werden soll. Der Benutzer kann das jeweils andere Dataset verwerfen oder in einem neuen Datenfluss platzieren. Die letztgenannte Option wird als Forken bezeichnet.

Zum Forken gehen Sie folgendermaßen vor: 
1. Wählen Sie eine Spalte aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie dann die Spalte **Filter** aus.

2. Wählen Sie unter **I Want To** (Ich möchte) die Option **Keep Rows** (Zeilen beibehalten) aus, damit das Resultset angezeigt wird, das dem Filter genügt.

3. Wählen Sie **Remove Rows** (Zeilen entfernen) aus, um das Resultset anzuzeigen, das dem Filter nicht genügt.

4. Wählen Sie unter **Conditions** (Bedingungen) die Option **Create Dataflow Containing the Filtered Out Rows** (Datenfluss erstellen, der die ausgefilterten Zeilen enthält) aus, um das nicht angezeigte Resultset in einen neuen Datenfluss zu forken.


Dieses Verfahren wird häufig verwendet, um eine Datenmenge zu separieren, die weiterer Vorbereitung bedarf. Im Anschluss an die Analyse des geforkten Datasets werden die Daten für gewöhnlich mit dem Resultset im ursprünglichen Datenfluss gemergt. Um Daten zu mergen (das Gegenteil eines Forkvorgangs), verwenden Sie eine der folgenden Aktionen:

- **Anfügen von Zeilen** Mergen Sie mindestens zwei Datenflüsse vertikal (zeilenweise). 
- **Anfügen von Spalten** Mergen Sie mindestens zwei Datenflüsse horizontal (spaltenweise).


>[!NOTE]
>Bei „Append Columns“ (Spalten anfügen) tritt ein Fehler auf, wenn es zu einer Spaltenkollision kommt.


Nach einem Mergevorgang verweist ein Quelldatenfluss auf mindestens einen Datenfluss. Die Datenvorbereitung benachrichtigt Sie in der unteren rechten Ecke der App unterhalb der Liste mit den Schritten.


Jeder Vorgang für den Datenfluss, auf den verwiesen wird, erfordert, dass im übergeordneten Datenfluss die Stichprobe aktualisiert wird, die aus dem Datenfluss stammt, auf den verwiesen wird. In diesem Fall ersetzt ein Bestätigungsdialogfeld die Datenfluss-Verweisbenachrichtigung in der unteren rechten Ecke. In diesem Dialogfeld wird bestätigt, dass Sie den Datenfluss aktualisieren müssen, um Änderungen an sämtlichen Abhängigkeitsdatenflüssen zu synchronisieren.

### <a name="list-of-appendices"></a>Liste der Anhänge 
* [Unterstützte Datenquellen](data-prep-appendix2-supported-data-sources.md)  
* [Unterstützte Transformationen](data-prep-appendix3-supported-transforms.md)  
* [Unterstützte Inspektoren](data-prep-appendix4-supported-inspectors.md)  
* [Unterstützte Ziele](data-prep-appendix5-supported-destinations.md)  
* [Beispielfilterausdrücke in Python](data-prep-appendix6-sample-filter-expressions-python.md)  
* [Beispielausdrücke für die Datenflusstransformation in Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
* [Beispieldatenquellen in Python](data-prep-appendix8-sample-source-connections-python.md)  
* [Beispielzielverbindungen in Python](data-prep-appendix9-sample-destination-connections-python.md)  
* [Beispielspaltentransformationen in Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  
