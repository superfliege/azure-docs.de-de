---
title: Aufteilen von Spalten anhand eines Beispiels mithilfe von Azure Machine Learning Workbench
description: "Das Referenzdokument zur Transformation „Aufteilen von Spalten anhand eines Beispiels“"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 013c99045621e4651a44ab99c9f695fff6004654
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="split-column-by-example-transformation"></a>Transformation „Aufteilen von Spalten anhand eines Beispiels“
Diese Transformation teilt den Inhalt einer Spalte vorausschauend auf aussagekräftige Begrenzungen auf, ohne dass eine Benutzereingabe erforderlich ist. Der Splitalgorithmus wählt die Begrenzungen aus, nachdem der Inhalt der Spalte analysiert wurde. Diese Begrenzungen können von Folgendem definiert werden:
* einem festgelegten Trennzeichen
* mehreren beliebigen Trennzeichen, die in bestimmten Kontexten auftreten oder
* Datenmustern oder bestimmten Entitätstypen

Benutzer können auch das Aufteilungsverhalten im Modus „Advanced“ (Erweitert) steuern. Sie können so die Trennzeichen angeben oder Beispiele der gewünschten Aufteilung bereitstellen.

Theoretisch können Splitvorgänge auch in Workbench mithilfe einer Reihe von *Derive Column by Example*-Transformationen (Spalte nach Beispiel ableiten) ausgeführt werden. Wenn jedoch mehrere Spalten vorhanden sind, kann das Ableiten der einzelnen Spalten sehr viel Zeit in Anspruch nehmen, auch wenn Sie einen By-Example-Ansatz verwenden. Mithilfe der vorhersagbaren Teilung kann das einfache Splitting durchgeführt werden, ohne dass der Benutzer Beispiele für jede Spalte bereitstellen muss. 

## <a name="how-to-perform-this-transformation"></a>Vorgehensweise zum Durchführen dieser Transformation

1. Wählen Sie die Spalte aus, die Sie teilen möchten. 
2. Wählen Sie im Menü **Transformationen** die Option **Split Column by Example** (Spalte nach Beispiel teilen). Oder klicken Sie mit der rechten Maustaste auf den Kopf der ausgewählten Spalte, und wählen Sie im Kontextmenü die Option **Split Column by Example** (Spalte nach Beispiel teilen) aus. Der Editor für Transformationen wird geöffnet, und neben der ausgewählten Spalte wird eine neue Spalte hinzugefügt. Zu diesem Zeitpunkt analysiert Workbench die Eingabespalte, bestimmt die Trennbegrenzungen und erzeugt ein Programm, um die Spalte wie im Raster dargestellt aufzuteilen. Das Syntheseprogramm wird für alle Zeilen in der Spalte ausgeführt. Trennzeichen, falls vorhanden, werden vom Endergebnis ausgeschlossen.
3. Sie können für eine präzisere Kontrolle über die Trenntransformation zum Modus **Advanced** (Erweitert) wechseln. 
4. Überprüfen Sie die Ausgabe, und klicken Sie auf **OK**, um die Transformation zu akzeptieren. 

Die Transformation möchte die gleiche Anzahl resultierender Spalten für alle Zeilen erzeugen. Wenn eine Zeile nicht auf den festgelegten Begrenzungen geteilt werden kann, wird standardmäßig für alle Spalten *NULL* erzeugt. Dieses Verhalten kann im **Modus „Erweitert“** geändert werden.

### <a name="transform-editor-advanced-mode"></a>Editor für Transformationen: Erweiterter Modus
Im **erweiterten Modus** können Sie zum Teilen von Spalten eine umfangreichere Benutzeroberfläche verwenden. 

Durch Auswählen der **Spalten zum Beibehalten von Trennzeichen** sind die Trennzeichen im Endergebnis enthalten. Trennzeichen sind standardmäßig ausgeschlossen.

Das Angeben von **Trennzeichen** setzt die automatische Auswahllogik für Trennzeichen außer Kraft. Mehrere Trennzeichen, eines pro Zeile, können als **Trennzeichen** angegeben werden. All diese Zeichen werden als Trennzeichen für die Teilung der Spalte verwendet.

Manchmal erzeugt das Aufteilen eines Wertes auf festgelegten Begrenzungen eine geringere Anzahl von Spalten als für die übrigen Zeilen. In diesen Fällen wird die **Füllrichtung** verwenden, um die Reihenfolge festzulegen, in der die Spalten gefüllt werden sollen.

Wenn Sie auf **Show suggested examples** (Vorgeschlagene Beispiele anzeigen) klicken, werden die jeweiligen Zeilen angezeigt, für die der Benutzer ein Beispiel der Aufteilung bereitstellen soll. Benutzer können rechts von der vorgeschlagenen Zeile auf den **Nach oben**-Pfeil klicken, um die Zeile als Beispiel hochzustufen. 

Benutzer können **Spalten löschen** oder **neue Spalten einfügen**, indem Sie einen Rechtsklick auf den Kopf der **Tabelle mit den Beispielen** ausführen.

Benutzer können Werte aus einer Zelle kopieren und in eine andere einfügen, um ein Beispiel der Teilung bereitzustellen.

Benutzer können zwischen dem **Standardmodus**  und dem **erweiterten Modus** wechseln, indem Sie im Editor für Transformationen auf die entsprechenden Links klicken.

### <a name="editing-an-existing-transformation"></a>Bearbeiten einer vorhandenen Transformation

Benutzer können eine vorhandene Transformation vom Typ **Split Column By Example** bearbeiten, indem sie während des Transformationsschritts die Option **Bearbeiten** wählen. Wenn Sie auf **Bearbeiten** klicken, wird der Editor für Transformationen im **erweiterten Modus** geöffnet, und es werden alle Beispiele angezeigt, die während der Erstellung der Transformation angegeben wurden.

## <a name="examples-of-splitting-on-a-fixed-single-character-delimiter"></a>Beispiele für des Teilen auf einem festgelegten Trennzeichen mit einem einzelnen Zeichen

Datenfelder werden häufig durch ein einzelnes festgelegtes Trennzeichen getrennt, z.B. ein Komma im CSV-Format. Die Split-Transformation versucht diese Trennzeichen automatisch abzuleiten. Im folgenden Szenario wird beispielsweise automatisch ein Punkt („.“) als Trennzeichen abgeleitet.

### <a name="splitting-ip-addresses"></a>Teilen von IP-Adressen

Die Werte in der ersten Spalte werden vorausschauend in vier Spalten aufgeteilt

|IP|IP_1|IP_2|IP_3|IP_4|
|:-----|:-----|:-----|:-----|:-----|
|192.168.0.102|192|168|0|102|
|192.138.0.101|192|138|0|101|
|192.168.0.102|192|168|0|102|
|192.158.1.202|192|158|1|202|
|192.168.0.102|192|168|0|102|
|192.169.1.102|192|169|1|102|

## <a name="examples-of-splitting-on-multiple-delimiters-within-particular-contexts"></a>Beispiele für das Aufteilen auf mehrere Trennzeichen innerhalb eines bestimmten Kontexts

Die Benutzerdaten enthalten womöglich viele unterschiedliche Trennzeichen, die verschiedene Felder aufteilen. Des Weiteren können nur einige Vorkommen einer Trennzeichenfolge ein Trennzeichen sein, jedoch nicht alle. Im folgenden Fall sind die erforderlichen Trennzeichen „-“, „,“ und „:“. Damit können Sie die gewünschte Ausgabe erzeugen. Jedoch darf nicht jedes Vorkommen des Trennzeichens „:“ ein Aufteilungspunkt sein, da wir nicht die Zeit aufteilen möchten, sondern diese in einer einzelnen Spalte beibehalten wollen. Die Split-Transformation leitet Trennzeichen innerhalb der Kontexte ab, in denen Sie in den Eingabedaten vorkommen, anstatt jedes mögliche Vorkommen des Trennzeichens. Die Transformation berücksichtigt auch allgemeine Datentypen, z.B. Datums- und Zeitangaben.   

### <a name="splitting-store-opening-timings"></a>Aufteilen von Ladenöffnungszeiten

Die Werte in der folgenden *Zeitangaben*-Spalte werden vorausschauend in neun Spalten, wie in der Tabelle darunter gezeigt, aufgeteilt.

|Zeitangaben|
|:-----|
|Montag – Freitag: 7:00 bis 18:00, Samstag: 9:00 bis 17:00, Sonntag: Geschlossen|
|Montag – Freitag: 9:00 bis 18:00, Samstag: 4:00 bis 16:00, Sonntag: Geschlossen|
|Montag – Freitag: 8:30 bis 17:00, Samstag: 3:00 bis 14:30, Sonntag: Geschlossen|
|Montag – Freitag: 8:00 bis 18:00, Samstag: 2:00 bis 15:00, Sonntag: Geschlossen|
|Montag – Freitag: 4:00 bis 19:00, Samstag: 9:00 bis 16:00, Sonntag: Geschlossen|
|Montag – Freitag: 8:30 bis 16:30, Samstag: 9:00 bis 17:00, Sonntag: Geschlossen|
|Montag – Freitag: 5:30 bis 18:30, Samstag: 5:00 bis 16:00, Sonntag: Geschlossen|
|Montag – Freitag: 8:30 bis 20:30, Samstag: 6:00 bis 17:00, Sonntag: Geschlossen|
|Montag – Freitag: 8:00 bis 21:00, Samstag: 9:00 bis 20:00, Sonntag: Geschlossen|
|Montag – Freitag: 10:00 bis 21:30, Samstag: 9:30 bis 15:00, Sonntag: Geschlossen|

|Timings_1|Timings_2|Timings_3|Timings_4|Timings_5|Timings_6|Timings_7|Timings_8|Timings_9|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Montag|Freitag|7:00|18:00|Samstag|9:00|17:00|Sonntag|Geschlossen|
|Montag|Freitag|9:00|18:00|Samstag|4:00|16:00|Sonntag|Geschlossen|
|Montag|Freitag|8:30|19:00|Samstag|3:00|14:30|Sonntag|Geschlossen|
|Montag|Freitag|8:00|18:00|Samstag|2:00|15:00|Sonntag|Geschlossen|
|Montag|Freitag|4:00|19:00|Samstag|9:00|16:00|Sonntag|Geschlossen|
|Montag|Freitag|8:30|16:30|Samstag|9:00|17:00|Sonntag|Geschlossen|
|Montag|Freitag|5:30|18:30|Samstag|5:00|16:00|Sonntag|Geschlossen|
|Montag|Freitag|8:30|20:30|Samstag|6:00|17:00|Sonntag|Geschlossen|
|Montag|Freitag|8:00|21:00|Samstag|9:00|20:00|Sonntag|Geschlossen|
|Montag|Freitag|10:00|21:30|Samstag|9:30|15:00|Sonntag|Geschlossen|

### <a name="splitting-iis-log"></a>Aufteilen des IIS-Protokolls

Hier sehen Sie ein anderes Beispiel von mehreren beliebigen Trennzeichen. Dieses Beispiel enthält auch ein kontextbezogenes Trennzeichen, „/“, das nicht innerhalb der URLs oder Dateipfade aufgeteilt werden darf. Es ist schwierig, diese Aufteilung mithilfe vieler *Derive Column by Example*-Transformationen (Spalte nach Beispiel ableiten) durchzuführen und Beispiele für jedes Feld bereitzustellen. Mithilfe der Split-Transformation können wir die vorausschauende Aufteilung ohne das Bereitstellen von Beispielen ausführen.

|logtext|
|:-----|
|192.128.138.20 - - [16/Oct/2016 16:22:33 -0200] "GET /images/picture.gif HTTP/1.1" 234 343 www.yahoo.com "http://www.example.com/" "Mozilla/4.0 (compatible; MSIE 4)" "-"|
|10.128.72.213 - - [17/Oct/2016 12:43:12 +0300] "GET /news/stuff.html HTTP/1.1" 200 6233 www.aol.com "http://www.sample.com/" "Mozilla/5.0 (MSIE)" "-"|
|192.165.71.165 - - [12/Nov/2016 14:22:44 -0500] "GET /sample.ico HTTP/1.1" 342 7342 www.facebook.com "-" "Mozilla/5.0 (Windows; U; Windows NT 5.1; rv:1.7.3)" "-"|
|10.166.64.165 - - [23/Nov/2016 01:52:45 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.167.1.193 - - [16/Jan/2017 22:34:56 +0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.147.76.193 - - [28/Jan/2017 26:36:16 +0800] "GET /search.php HTTP/1.1" 400 1777 www.bing.com "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)" "-"|
|192.166.64.165 - - [23/Mar/2017 01:55:25 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|11.167.1.193 - - [16/Apr/2017 11:34:36 +0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "Mozilla/5.0 (Windows)" "-"|

Wird wie Folgt aufgeteilt:

|logtext_1|logtext_2|logtext_3|logtext_4|logtext_5|logtext_6|logtext_7|logtext_8|logtext_9|logtext_10|logtext_11|logtext_12|logtext_13|logtext_14|logtext_15|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|192.128.138.20|16/Oct/2016|16:22:33|-0200|GET|images/picture.gif|HTTP|1.1|234|343|www.yahoo.com|http://www.example.com/|Mozilla|4,0|kompatibel, MSIE 4|
|10.128.72.213|17/Oct/2016|12:43:12|+0300|GET|news/stuff.html|HTTP|1.1|200|6233|www.aol.com|http://www.sample.com/|Mozilla|5.0|MSIE|
|192.165.71.165|12/Nov/2016|14:22:44|-0500|GET|sample.ico|HTTP|1.1|342|7342|www.facebook.com|-|Mozilla|5.0|Windows; U; Windows NT 5.1; rv:1.7.3|
|10.166.64.165|23/Nov/2016|01:52:45|-0800|GET|style.css|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|192.167.1.193|16/Jan/2017|22:34:56|+0200|GET|js/ads.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|
|192.147.76.193|28/Jan/2017|26:36:16|+0800|GET|search.php|HTTP|1.1|400|1777|www.bing.com|-|Mozilla|4,0|compatible; MSIE 6.0; Windows NT 5.1|
|192.166.64.165|23/Mar/2017|01:55:25|-0800|GET|style.css|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|11.167.1.193|16/Apr/2017|11:34:36|+0200|GET|js/ads.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|

## <a name="examples-of-splitting-without-delimiters"></a>Beispiele für das Aufteilen ohne Trennzeichen
In einigen Fällen gibt es keine tatsächlichen Trennzeichen, und Datenfelder erscheinen zusammenhängend nebeneinander. In diesem Fall erkennt die Split-Transformation automatisch Muster in den Daten, um wahrscheinliche Aufteilungspunkte abzuleiten. Beispielsweise möchten wir im folgenden Szenario die Menge der Währungsart aufteilen, und Split leitet automatisch die Begrenzung zwischen den numerischen und nicht-numerischen Daten als Aufteilungspunkt ab.

### <a name="splitting-amount-with-currency-symbol"></a>Aufteilen von Beträgen mit dem Währungssymbol

|Amount|Amount_1|Amount_2|
|:-----|:-----|:-----|
|\$14|$|14|
|£9|£|9|
|\$34|$|34|
|€ 18|€ |18|
|\$42|$|42|
|\$7|$|7|
|£42|£|42|
|\$16|$|16|
|€ 16|€ |16|
|\$15|$|15|
|\$16|$|16|
|€ 64|€ |64|

Im folgenden Beispiel möchten wir Gewichtswerte von den Maßeinheiten trennen. Wieder erkennt der Split-Rückschluss die bedeutende Begrenzung automatisch und bevorzugt diese vor den anderen möglichen Trennzeichen, wie dem Zeichen „.“. 

### <a name="splitting-weights-with-units"></a>Aufteilen von Gewichten in Einheiten

|Weight|Weight_1|Weight_2|
|:-----|:-----|:-----|
|2,27 KG|2.27|KG|
|1 L|1|L|
|2,5 KG|2.5|KG|
|2 KG|2|KG|
|1,7 KGA|1.7|KGA|
|3 KG|3|KG|
|2 KG|2|KG|
|125 KG|125|G|
|500 G|500|G|
|1,5 KGA|1.5|KGA|

## <a name="technical-notes"></a>Technische Hinweise

Die Split-Transformationsfunktion basiert auf der Technologie der **Predictive Program Synthesis** (Vorausschauenden Programmsynthese). Mit dieser Technologie werden Datenumwandlungsprogramme automatisch basierend auf den Eingabedaten erlernt. Die Programme werden in einer domänenspezifischen Sprache synthetisiert. Die DSL basiert auf Trennzeichen und Feldern, die in bestimmten Kontexten regulärer Ausdrücke vorkommen. Weitere Informationen zu dieser Technologie finden Sie in einer [neuen Veröffentlichung zu diesem Thema](https://www.microsoft.com/en-us/research/publication/automated-data-extraction-using-predictive-program-synthesis/). 

