---
title: "Ausführliche Anleitung zur Verwendung von Azure Machine Learning Data Preparation | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Übersicht und Details zum Beheben von Datenproblemen mit Azure ML Data Prep"
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 22389ba85edb119acdd21b63f2deae2d71f31373
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="data-preparation-user-guide"></a>Anleitung zur Datenvorbereitung 
Data Prep bietet Ihnen umfangreiche Funktionen, die im Folgenden dargestellt werden.

### <a name="step-execution-history-and-caching"></a>Schrittausführung, Verlauf und Caching 
Aus Leistungsgründen wird im Schrittverlauf von Data Prep eine Reihe von Caches verwaltet. Wenn Sie einen Schritt anklicken und ein Cache getroffen wird, wird dieser Schritt nicht erneut ausgeführt. Wenn am Ende des Schrittverlaufs ein Schreibblock verfügbar ist und Sie zwischen den einzelnen Schritten hin- und herwechseln, dabei aber nichts verändern, wird das Schreiben nach dem ersten Mal nicht ausgelöst. Wenn Sie 
- Änderungen an dem Schreibblock vornehmen oder
- einen neuen Transformationsblock hinzufügen und diesen über den Schreibblock bewegen, um eine Cacheinvalidierung zu erzeugen, oder
- Sie die Eigenschaften eines Blocks über dem Schreibblock ändern, um eine Cacheinvalidierung zu erzeugen, oder
- Sie eine Stichprobe aktualisieren (und dadurch sämtliche Caches für ungültig erklären),

kommt es zu einem neuen Schreibvorgang, sodass der vorherige überschrieben wird.

### <a name="error-values"></a>Fehlerwerte

Datentransformationen können für Eingabewerte möglicherweise fehlschlagen, da diese Werte nicht angemessen bearbeitet werden können. Beispielsweise schlagen Typkoersionen fehl, wenn der Wert der Eingabezeichenfolge nicht in einen bestimmten Zieltyp umgewandelt werden kann. Eine Umwandlung kann z.B. die Konvertierung einer Spalte des Typs „string“ (Zeichenfolge) in einen numerischen oder booleschen Typ sein. Genauso kann es sich bei einer Umwandlung um den Versuch handeln, eine Spalte, die nicht vorhanden ist, zu duplizieren (dies ist dann der Fall, wenn die Spalte zum Löschen eines Vorgangs X bewegt wird, bevor die Spalte zum Duplizieren eines Vorgangs X bewegt wird).

In diesen Fällen erzeugt Data Prep einen **Fehlerwert** als Ausgabe. Fehlerwerte weisen darauf hin, dass ein vorheriger Vorgang für den betreffenden Wert fehlgeschlagen ist. Intern werden sie als erstklassige Werttypen behandelt. Allerdings ändert ihr Vorhandensein nicht den zugrunde liegenden Typ einer Spalte – auch nicht, wenn eine Spalte ausschließlich aus Fehlerwerten besteht.

Fehlerwerte sind leicht zu identifizieren. Sie sind rot gekennzeichnet und als „Fehler“ bezeichnet. Um den Grund für den Fehler zu erfahren, zeigen Sie mit dem Mauszeiger auf einen Fehlerwert, damit Ihnen eine Fehlerbeschreibung angezeigt wird.

Fehlerwerte verbreiten sich. Wenn ein Fehlerwert auftritt, so tritt der Fehler in den meisten Fällen bei einem Großteil der Vorgänge immer wieder auf. Derzeit gibt es drei Möglichkeiten, diese Fehlerwerte zu entfernen oder zu ersetzen:

1) Replace
    -  Klicken Sie mit der rechten Maustaste auf eine Spalte und wählen Sie *Replace Error Values* (Fehlerwerte ersetzen) aus. Anschließend können Sie einen Ersatzwert für jeden in der Spalte gefundenen Fehlerwert auswählen.

2) Remove (Entfernen)
    - Data Prep beinhaltet interaktive Filter, mit denen Fehlerwerte beibehalten oder entfernt werden können.
    - Klicken Sie mit der rechten Maustaste auf eine Spalte und dann auf *Filter Column* (Spalte filtern). Um Fehlerwerte beizubehalten oder zu entfernen, erstellen Sie entweder die Bedingung *"is error"* (Fehler) oder die Bedingung *"is not error"* (kein Fehler).

3) Verwenden Sie einen Python-Ausdruck, um Fehlerwerte bedingt zu bearbeiten. Nähere Informationen finden Sie im Abschnitt zu [Python-Erweiterungen](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Stichproben
Eine Datenquelldatei nimmt Rohdaten aus anderen Quellen – entweder aus einem lokalen Dateisystem oder einem Remotestandort – auf. Mithilfe des Stichprobenblocks können Sie festlegen, ob sie nur mit einem Teil der Daten arbeiten möchten, wenn Sie Stichproben erzeugen. Wenn Sie nur mit Stichproben aus den Daten anstelle von großen Datensätzen arbeiten, wirkt sich das positiv auf die Leistung aus, wenn Sie später Schritte vornehmen.

Für jede Datenquelldatei können mehrere Stichproben erzeugt und gespeichert werden. Allerdings kann nur eine Stichprobe als aktiv gekennzeichnet werden. Sie können Stichproben im Datenquellen-Assistenten oder durch die Bearbeitung des Stichprobenblocks erstellen, bearbeiten oder löschen. Jegliche Data Prep-Dateien, die auf Datenquellen verweisen, verwenden grundsätzlich die Stichproben, die in der Datenquelldatei angegeben wurden.

Es gibt eine Reihe von Strategien zur Stichprobenentnahme, die alle unterschiedliche, konfigurierbare Parameter aufweisen.

#### <a name="top"></a>Top
Diese Strategie kann sowohl auf lokale Dateien als auch auf Remotedateien angewendet werden. Die ersten n Zeilen (durch „Count“ (Anzahl) angegeben) werden in die Datenquelle aufgenommen.

#### <a name="random-n"></a>Random N 
Diese Strategie kann nur auf lokale Dateien angewendet werden. N Zeilen (durch „Count“ (Anzahl) angegeben), die zufällig ausgewählt werden, werden in die Datenquelle aufgenommen. Sie können einen bestimmten Ausgangswert festlegen, um sicherzustellen, dass dasselbe Beispiel aus den Stichproben gezogen wird, solange auch „Count“ (Anzahl) nicht verändert wird.

#### <a name="random-"></a>Random % 
Diese Strategie kann sowohl auf lokale Dateien als auch auf Remotedateien angewendet werden. In beiden Fällen müssen, ähnlich wie bei der Strategie „Random N“, eine Wahrscheinlichkeit und ein Ausgangswert festgelegt werden.

Um Stichproben aus den Remotedateien zu entnehmen, müssen zusätzliche Parameter festgelegt werden.

- Stichprobengenerator 
  - Wählen Sie einen Spark-Cluster oder ein Docker-Remotecomputeziel aus, die verwendet werden sollen, um Stichproben zu entnehmen. Das Computeziel für das Projekt muss im Voraus festgelegt werden, damit es auf dieser Liste erscheint. Führen Sie die in der Anleitung [How to use GPU in Azure Machine Learning (Verwendung von neuen Computezielen)](how-to-use-gpu.md) beschriebenen Schritte aus, um Computeziele zu erstellen.
- Speichern der Stichproben 
  - Legen Sie einen zwischengeschalteten Speicherort fest, an dem die Remotestichprobe gespeichert werden soll. Dieser Speicherort muss ein anderer als der Speicherort der Eingabedatei sein.

#### <a name="full-file"></a>Vollständige Datei 
Diese Strategie kann nur auf lokale Dateien angewendet werden, indem Sie die vollständige Datei in die Quelldatei aufnehmen. Sollte die Datei zu groß sein, können spätere Vorgänge der App verlangsamt werden. Darum ist es für Sie möglicherweise sinnvoller, eine andere Strategie zu verwenden, um Stichproben zu entnehmen.


### <a name="forking-merging-and-appending"></a>Spalten, Zusammenführen und Anfügen

Wenn Sie einen Filter auf ein Dataset anwenden, teilt dieser die Daten in zwei Resultsets: eins mit Datensätzen, die dem Filter entsprechen, und eins mit Datensätzen, die dem Filter nicht entsprechen. In beiden Fällen kann der Benutzer auswählen, welches Resultset angezeigt werden soll. Der Benutzer kann das jeweils andere Dataset entweder verwerfen oder in einem neuen Datenfluss platzieren. Letztere Möglichkeit wird als *Forking* (Spalten) bezeichnet.

Wählen Sie zum Spalten eine Spalte aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie „Spalte filtern“ aus.
- Wählen Sie unter „I Want To“ (Ich möchte) *Keep Rows* (Reihen beibehalten) aus, damit das Resultset mit den herausgefilterten Datensätzen angezeigt wird, oder *Remove Rows* (Reihen löschen), um das Resultset mit den fehlgeschlagenen Datensätzen anzuzeigen.
- Wählen Sie unter „Conditions“ (Bedingungen) *Create Dataflow Containing the Filtered Out Rows* (Datenfluss erstellen, der die herausgefilterten Reihen enthält) aus, um das nicht angezeigte Resultset in einen neuen Datenfluss zu spalten.


Dies besteht häufig in der Abtrennung eines Datasets, das weiterer Vorbereitung bedarf. Im Anschluss an die Analyse der gespalteten Datensätze werden die Daten für gewöhnlich mit den Resultsets des ursprünglichen Datenflusses zusammengefügt. Um Daten zusammenzufügen („merge“, also das Gegenteil von Spalten), gehen Sie wie folgt vor:
- *Anfügen von Zeilen* Fügen Sie zwei oder mehr Datenflüsse vertikal zusammen (reihenweise). 
- *Anfügen von Spalten* Fügen Sie zwei oder mehr Datenflüsse horizontal zusammen (spaltenweise).


>[!NOTE]
>Der Vorgang „AppendColumns“ schlägt fehl, wenn es zu einer Spaltenkollision kommt.


Nach dem Zusammenfügen verweist ein Quelldatenfluss auf mindestens einen Datenfluss. DataPrep zeigt Ihnen in der unteren rechten Ecke der App – unterhalb der Liste der Schritte –eine Benachrichtigung an.


Für jeden Vorgang mit dem verwiesenen Datenfluss muss im übergeordneten Datenfluss die Stichprobe aktualisiert werden, die aus dem verwiesenen Datenfluss stammt. In diesem Fall ersetzt ein Bestätigungsdialogfeld die Datenflussbenachrichtigung in der unteren rechten Ecke. In diesem Dialogfeld wird bestätigt, dass Sie den Datenfluss aktualisieren müssen, um Änderungen an sämtlichen Abhängigkeitsdatenflüssen zu synchronisieren.

### <a name="list-of-appendices"></a>Anhänge 
[Appendix 2 - Supported Data Sources (Anhang 2: Unterstützte Datenquellen)](data-prep-appendix2-supported-data-sources.md)  
[Appendix 3 - Supported Transforms (Anhang 3: Unterstützte Transformationen)](data-prep-appendix3-supported-transforms.md)  
[Appendix 4 - Supported Inspectors (Anhang 4: Unterstützte Inspektoren)](data-prep-appendix4-supported-inspectors.md)  
[Appendix 5 - Supported Destinations (Anhang 5: Unterstützte Ziele)](data-prep-appendix5-supported-destinations.md)  
[Appendix 6 - Sample Filter Expressions in Python (Anhang 6: Beispiele für Filterausdrücke in Python)](data-prep-appendix6-sample-filter-expressions-python.md)  
[Appendix 7 - Sample Transform Dataflow Expressions in Python (Anhang 7: Beispiele für Ausdrücke zur Datenflusstransformation in Python)](data-prep-appendix7-sample-transform-data-flow-python.md)  
[Appendix 8 - Sample Data Sources in Python (Anhang 8: Beispiele für Datenquellen in Python)](data-prep-appendix8-sample-source-connections-python.md)  
[Appendix 9 - Sample Destination Connections in Python (Anhang 9: Beispiele für Zielverbindungen in Python)](data-prep-appendix9-sample-destination-connections-python.md)  
[Appendix 10 - Sample Column Transforms in Python (Anhang 10: Beispiele für Spaltentransformationen in Python)](data-prep-appendix10-sample-custom-column-transforms-python.md)  

