---
title: "Transformation „Derive Column by Example“ mit Azure Machine Learning Workbench"
description: "Dies ist das Referenzdokument zur Transformation „Derive Column by Example“ (Spalte nach Beispiel ableiten)."
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
ms.openlocfilehash: a02f5e827345a1d28f01d691e1b6fbccfc03ae8a
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="derive-column-by-example-transformation"></a>Transformation „Derive Column by Example“

Mit der Transformation **Derive Column by Example** (Spalte nach Beispiel ableiten) können Benutzer eine Ableitung von einer oder mehreren Spalten erstellen, indem sie die von Benutzern bereitgestellten Beispiele des abgeleiteten Ergebnisses verwenden. Die Ableitung kann eine beliebige Kombination aus den unterstützten Transformationen „String“ (Zeichenfolge), „Date“ (Datum) und „Number“ (Zahl) sein. 

Die folgenden Transformationen vom Typ „String“, „Date“ und „Number“ werden unterstützt:

**Transformationen vom Typ „String“:** 

Unterzeichenfolge mit intelligenter Extraktion von Zahlen und Datumsangaben, Verkettung, Änderung der Groß-/Kleinschreibung, Zuordnung von Konstantenwerten.

**Transformationen vom Typ „Date“:** 

Änderung des Datumsformats, Extraktion von Datumsteilen, Zuordnung von Zeit zu Zeitcontainern.

Die Transformationen vom Typ „Date“ sind relativ generisch und verfügen nur über einige nennenswerte Einschränkungen:
* Zeitzonen werden nicht unterstützt.
* Hier sind einige häufige Formate angegeben, die nicht unterstützt werden:
    * ISO 8601-Format für Woche des Jahres (z.B. „2009-W53-7“) 
    * Unix-Epochenzeit.
* Bei allen Formaten wird die Groß-/Kleinschreibung berücksichtigt („4am“ wird nicht als Uhrzeit erkannt, aber „4AM“ wird erkannt).

**Transformationen vom Typ „Number“:** 

Runden, Untergrenze, Obergrenze, Diskretisierung, Auffüllung (mit Nullen oder Leerstellen), Division oder Multiplikation (hoch 1.000).

**Zusammengesetzte Transformationen:** 

Eine beliebige Kombination aus Transformationen vom Typ „String“, „Number“ oder „Date“.

## <a name="how-to-use-this-transformation"></a>Verwenden dieser Transformation

Führen Sie diese Schritte aus, um diese Transformation durchzuführen:
1. Wählen Sie eine oder mehrere Spalten aus, von denen Sie den Wert ableiten möchten. 
2. Wählen Sie im Menü **Transformationen** die Option **Derive Column by Example** (Spalte nach Beispiel ableiten). Oder klicken Sie mit der rechten Maustaste auf den Kopf einer ausgewählten Spalte, und wählen Sie im Kontextmenü die Option **Derive Column by Example** (Spalte nach Beispiel ableiten). Der Editor für Transformationen wird geöffnet, und neben der äußersten rechten ausgewählten Spalte wird eine neue Spalte hinzugefügt. Ausgewählte Spalten können mit den Kontrollkästchen in den Spaltenköpfen identifiziert werden. Das Hinzufügen und Entfernen von Spalten für die Auswahl ist über die Kontrollkästchen in den Spaltenköpfen möglich.
3. Geben Sie ein Beispiel für die *Ausgabe* einer Zeile ein, und drücken Sie die EINGABETASTE. An diesem Punkt analysiert die Workbench die Eingabespalte und die bereitgestellte Ausgabe, um per Synthese ein Programm zu erstellen, mit dem die Eingaben in eine Ausgabe transformiert werden können. Das Syntheseprogramm wird für alle Zeilen im Datenraster ausgeführt. Für mehrdeutige und komplizierte Fälle werden unter Umständen mehrere Beispiele benötigt. Je nachdem, ob Sie sich im einfachen oder erweiterten Modus befinden, können auf unterschiedliche Weise mehrere Beispiele angegeben werden.
4. Überprüfen Sie die Ausgabe, und klicken Sie auf **OK**, um die Transformation zu akzeptieren.

### <a name="transform-editor-basic-mode"></a>Editor für Transformationen: Einfacher Modus

Im einfachen Modus wird im Datenraster eine Inline-Bearbeitungsoberfläche bereitgestellt. Sie können Beispiele für die Ausgabe angeben, indem Sie zur relevanten Zelle navigieren und den Wert eingeben. 

Die Workbench analysiert die Daten und versucht, die Grenzfälle zu identifizieren, die vom Benutzer überprüft werden sollten. Während die Daten analysiert werden, wird im Headerbereich des Editors für Transformationen die Meldung **Daten werden analysiert** angezeigt. Nach Abschluss der Analyse wird im Headerbereich entweder **Keine Vorschläge** oder **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) angezeigt. Sie können durch die Grenzfälle navigieren, indem Sie auf **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) klicken. Falls der Wert für eine Zeile fehlerhaft ist, sollten Sie den richtigen Wert als zusätzliches Beispiel eingeben. 

### <a name="transform-editor-advanced-mode"></a>Editor für Transformationen: Erweiterter Modus

Im erweiterten Modus können Sie zum Ableiten von Spalten nach Beispiel eine umfangreichere Benutzeroberfläche verwenden. Alle Beispiele werden an einem Ort angezeigt. Sie können auch alle Grenzfälle auch einmal prüfen, indem Sie auf **Show suggested examples** (Vorgeschlagene Beispiele anzeigen) klicken. 

Im erweiterten Modus können Sie jede beliebige Zeile als Beispielzeile hinzufügen, indem Sie im Raster auf die Zeile doppelklicken. Nachdem eine Zeile als Beispielzeile kopiert wurde, können Sie die Daten in den Quellspalten auch bearbeiten, um ein Synthesebeispiel zu erstellen. Auf diese Weise können Sie Fälle hinzufügen, die in den Beispieldaten derzeit nicht vorhanden sind.

Benutzer können zwischen dem einfachen Modus (**Basic Mode**) und dem erweiterten Modus (**Advanced Mode**) umschalten, indem sie im Editor für Transformationen auf die entsprechenden Links klicken.

### <a name="editing-existing-transformation"></a>Bearbeiten von vorhandenen Transformationen

Benutzer können eine vorhandene Transformation vom Typ **Derive Column By Example** (Spalte nach Beispiel ableiten) bearbeiten, indem sie während des Transformationsschritts die Option **Bearbeiten** wählen. Wenn Sie auf **Bearbeiten** klicken, wird der Editor für Transformationen im **erweiterten Modus** geöffnet, und es werden alle Beispiele angezeigt, die während der Erstellung der Transformation angegeben wurden.

## <a name="examples-of-string-transformations-by-example"></a>Beispiele für Transformationen von Zeichenfolgen nach Beispiel


>[!NOTE] 
>Einträge in **Fettdruck** stehen für die Beispiele, die angegeben wurden, um die Transformation im angezeigten Dataset durchzuführen.


### <a name="s1-extracting-file-names-from-file-paths"></a>S1. Extrahieren von Dateinamen aus Dateipfaden

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 2

|Eingabe|Ausgabe|
|:-----|:-----|
|C:\Python35\Tools\pynche\TypeinViewer.py|**TypeinViewer.py**|
|C:\Python35\Tools\pynche\webcolors.txt|webcolors.txt|
|C:\Python35\Tools\pynche\websafe.txt|websafe.txt|
|C:\Python35\Tools\pynche\X\rgb.txt|rgb.txt|
|C:\Python35\Tools\pynche\X\xlicense.txt|xlicense.txt|
|C:\Python35\Tools\Scripts\2to3.py|2to3.py|
|C:\Python35\Tools\Scripts\analyze_dxp.py|**analyze_dxp.py**|
|C:\Python35\Tools\Scripts\byext.py|byext.py|
|C:\Python35\Tools\Scripts\byteyears.py|byteyears.py|
|C:\Python35\Tools\Scripts\checkappend.py|checkappend.py|

### <a name="s2-case-manipulation-during-string-extraction"></a>S2. Änderung der Groß-/Kleinschreibung während der Zeichenfolgenextraktion

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 3

|Eingabe|Ausgabe|
|:-----|:-----|
|REINDEER CT & DEAD END;  NEW HANOVER; Station 332; 2015-12-10 @ 17:10:52;|**New Hanover**|
|BRIAR PATH & WHITEMARSH LN;  HATFIELD TOWNSHIP; Station 345; 2015-12-10 @ 17:29:21;|Hatfield Township|
|HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-Station:STA27;|**Norristown**|
|AIRY ST & SWEDE ST;  NORRISTOWN; Station 308A; 2015-12-10 @ 16:47:36;|**Norristown**|
|CHERRYWOOD CT & DEAD END;  LOWER POTTSGROVE; Station 329; 2015-12-10 @ 16:56:52;|Lower Pottsgrove|
|CANNON AVE & W 9TH ST;  LANSDALE; Station 345; 2015-12-10 @ 15:39:04;|Lansdale|
|LAUREL AVE & OAKDALE AVE;  HORSHAM; Station 352; 2015-12-10 @ 16:46:48;|Horsham|
|COLLEGEVILLE RD & LYWISKI RD;  SKIPPACK; Station 336; 2015-12-10 @ 16:17:05;|Skippack|
|MAIN ST & OLD SUMNEYTOWN PIKE;  LOWER SALFORD; Station 344; 2015-12-10 @ 16:51:42;|Lower Salford|
|BLUEROUTE  & RAMP I476 NB TO CHEMICAL RD; PLYMOUTH; 2015-12-10 @ 17:35:41;|Plymouth|
|RT202 PKWY & KNAPP RD; MONTGOMERY; 2015-12-10 @ 17:33:50;|Montgomery|
|BROOK RD & COLWELL LN; PLYMOUTH; 2015-12-10 @ 16:32:10;|Plymouth|

### <a name="s3-date-format-manipulation-during-string-extraction"></a>S3. Änderung des Datumsformats während der Zeichenfolgenextraktion

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 1

|Eingabe|Ausgabe|
|:-----|:-----|
|MONTGOMERY AVE & WOODSIDE RD;  LOWER MERION; Station 313; 2015-12-11 @ 04:11:35;|**12 Nov 2015 4AM**|
|DREYCOTT LN & W LANCASTER AVE;  LOWER MERION; Station 313; 2015-12-11 @ 01:29:52;|12 Nov 2015 1AM|
|E LEVERING MILL RD & CONSHOHOCKEN STATE RD; LOWER MERION; 2015-12-11 @ 07:29:58;|12 Nov 2015 7AM|
|PENN VALLEY RD & MANOR RD;  LOWER MERION; Station 313; 2015-12-10 @ 20:53:30;|12 Oct 2015 8PM|
|BELMONT AVE & OVERHILL RD; LOWER MERION; 2015-12-10 @ 23:02:27;|12 Oct 2015 11PM|
|W MONTGOMERY AVE & PENNSWOOD RD; LOWER MERION; 2015-12-10 @ 19:25:22;|12 Oct 2015 7PM|
|ROSEMONT AVE & DEAD END;  LOWER MERION; Station 313; 2015-12-10 @ 18:43:07;|12 Oct 2015 6PM|
|AVIGNON DR & DEAD END; LOWER MERION; 2015-12-10 @ 20:01:29-Station:STA24;|12 Oct 2015 8PM|

### <a name="s4-concatenating-strings"></a>S4. Verketten von Zeichenfolgen

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 1

>[!NOTE] 
>In diesem Beispiel steht das Sonderzeichen „·“ in der Spalte „Ausgabe“ für Leerstellen.

|First Name (Vorname)|Middle Initial (Zweiter Vorname)|Last Name (Nachname)|Ausgabe|
|:-----|:-----|:-----|:-----|
|Laquanda||Lohmann|Laquanda··Lohmann|
|Claudio|A|Chew|**Claudio·A·Chew**|
|Sarah-Jane|S|Smith|Sarah-Jane·S·Smith|
|Brandi||Blumenthal|Brandi··Blumenthal|
|Jesusita|R|Journey|Jesusita·R·Journey|
|Hermina||Hults|Hermina··Hults|
|Anne-Marie|W|Jones|Anne-Marie·W·Jones|
|Rico||Ropp|Rico··Ropp|
|Lauren-May||Fullmer|Lauren-May··Fullmer|
|Marc|T|Maine|Marc·T·Maine|
|Angie||Adelman|Angie··Adelman|
|John-Paul||Smith|John-Paul··Smith|
|Song|W|Staller|Song·W·Staller|
|Jill||Jefferies|Jill··Jefferies|
|Ruby-Grace|M|Simmons|Ruby-Grace·M·Simmons|

### <a name="s5-generating-initials"></a>S5. Generieren von Initialen

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 2

|Vollständiger Name|Ausgabe|
|:-----|:-----|
|Laquanda Lohmann|**L.L.**|
|Claudio Chew|C.C.|
|Sarah-Jane Smith|S.S.|
|Brandi Blumenthal|B.B.|
|Jesusita Journey|J.J.|
|Hermina Hults|H.H.|
|Anne-Marie Jones|A.J.|
|Rico Ropp|R.R.|
|Lauren-May Fullmer|L.F.|
|Marc Maine|M.M.|
|Angie Adelman|A.A.|
|John-Paul Smith|**J.S.**|
|Song Staller|S.S.|
|Jill Jefferies|J.J.|
|Ruby-Grace Simmons|R.S.|


### <a name="s6-mapping-constant-values"></a>S6. Zuordnen von Konstantenwerten

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 3

|Geschlecht (Administration)|Ausgabe|
|:-----|:-----:|
|Male|**0**|
|Female|**1**|
|Unknown|**2**|
|Female|1|
|Female|1|
|Male|0|
|Unknown|2|
|Male|0|
|Female|1|

## <a name="examples-of-number-transformations-by-example"></a>Beispiele für Transformationen von Zahlen nach Beispiel

>[!NOTE] 
>Einträge in **Fettdruck** stehen für die Beispiele, die angegeben wurden, um die Transformation im angezeigten Dataset durchzuführen.


### <a name="n1-rounding-to-nearest-10"></a>N1. Runden auf den nächsten Zehnerwert

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 1

|Eingabe|Ausgabe|
|-----:|-----:|
|112|**110**|
|117|120|
|11112|11110|
|11119|11120|
|548|550|

### <a name="n2-rounding-down-to-nearest-10"></a>N2. Abrunden auf den nächsten Zehnerwert

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 2

|Eingabe|Ausgabe|
|-----:|-----:|
|112|**110**|
|117|**110**|
|11112|11110|
|11119|11110|
|548|540|

### <a name="n3-rounding-to-nearest-005"></a>N3. Runden auf die nächsten 0,05

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 2

|Eingabe|Ausgabe|
|-----:|-----:|
|-75.5812935|**-75.60**|
|-75.2646799|-75.25|
|-75.3519752|-75.35|
|-75.343513|**-75.35**|
|-75.6033497|-75.60|
|-75.283245|-75.30|

### <a name="n4-binning"></a>N4. Diskretisieren

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 1

|Eingabe|Ausgabe|
|-----:|:-----:|
|20.16|**20-25**|
|14,32|10-15|
|5.44|5-10|
|3.84|0-5|
|3.73|0-5|
|7.36|5-10|

### <a name="n5-scaling-by-1000"></a>N5. Skalieren auf 1.000

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 1

|Eingabe|Ausgabe|
|-----:|-----:|
|-243|**-243000**|
|-12.5|-12500|
|-2345.23292|-2345232.92|
|-1202.3433|-1202343.3|
|1202.3433|1202343.3|

### <a name="n6-padding"></a>N6. Auffüllen

Anzahl von Beispielen, die für diesen Fall erforderlich waren: 1

|Code|Ausgabe|
|-----:|-----:|
|5828|**05828**|
|44130|44130|
|49007|49007|
|29682|29682|
|4759|04759|
|10029|10029|
|7204|07204|

## <a name="examples-of-date-transformations-by-example"></a>Beispiele für Transformationen von Datumsangaben nach Beispiel

>[!NOTE] 
>Einträge in **Fettdruck** stehen für die Beispiele, die angegeben wurden, um die Transformation im angezeigten Dataset durchzuführen.


### <a name="d1-extracting-date-parts"></a>D1. Extrahieren von Datumsteilen

Diese Teile von Datumsangaben wurden mithilfe von verschiedenen By-Example-Transformationen für dasselbe Dataset extrahiert. Fettgedruckte Zeichenfolgen stehen für die Beispiele, die während der jeweiligen Transformation angegeben wurden.

|DateTime|Wochentag|Datum|Monat|Jahr|Stunde|Minute|Sekunde|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**Fri**|**31**|**Jan**|**2031**|**5**|**54**|**18**|
|17-Jan-1990 13:32:01|Wed|17|Jan|1990|13|32|01|
|14-Feb-2034 05:36:07|Tue|14|Feb|2034|5|36|07|
|14-Mar-2002 13:16:16|Thu|14|Mar|2002|13|16|16|
|21-Jan-1985 05:44:43|Mon|21|Jan|1985|5|44|**43**|
|16-Aug-1985 01:11:56|Fri|16|Aug|1985|1|11|56|
|20-Dec-2033 18:36:29|Tue|20|Dec|2033|18|36|29|
|16-Jul-1984 10:21:59|Mon|16|Jul|1984|10|21|59|
|13-Jan-2038 10:59:36|Wed|13|Jan|2038|10|59|36|
|14-Aug-1982 15:13:54|Sat|14|Aug|1982|15|13|54|
|22-Nov-2030 08:18:08|Fri|22|Nov|2030|8|18|08|
|21-Oct-1997 08:42:58|Tue|21|Oct|1997|8|42|58|
|28-Nov-2006 14:19:15|Tue|28|Nov|2006|14|19|15|
|29-Apr-2031 04:59:45|Tue|29|Apr|2031|4|59|45|
|29-Jan-2032 02:38:36|Thu|29|Jan|2032|2|38|36|
|11-May-2028 15:31:52|Thu|11|May|2028|15|31|52|
|15-Jul-1977 12:45:39|Fri|15|Jul|1977|12|45|11,9|
|27-Jan-2029 05:55:41|Sat|27|Jan|2029|5|55|41|
|03-Mar-2024 10:17:49|Sun|3|Mar|2024|10|17|49|
|14-Apr-2010 00:23:13|Wed|14|Apr|2010|0|23|13|

### <a name="d2-formatting-dates"></a>D2. Formatieren von Datumsangaben

Diese Datumsformatierungen wurden mithilfe von verschiedenen By-Example-Transformationen für dasselbe Dataset durchgeführt. Fettgedruckte Zeichenfolgen stehen für die Beispiele, die während der jeweiligen Transformation angegeben wurden.

|DateTime|Format1|Format2|Format3|Format4|Format5|
|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**1/31/2031**|**Friday, January 31, 2031**|**01312031 5:54**|**31/1/2031 5:54 AM**|**Q1 2031**|
|17-Jan-1990 13:32:01|1/17/1990|Wednesday, January 17, 1990|01171990 13:32|17/1/1990 1:32 PM|Q1 1990|
|14-Feb-2034 05:36:07|2/14/2034|Tuesday, February 14, 2034|02142034 5:36|14/2/2034 5:36 AM|Q1 2034
|14-Mar-2002 13:16:16|3/14/2002|Thursday, March 14, 2002|03142002 13:16|14/3/2002 1:16 PM|Q1 2002
|21-Jan-1985 05:44:43|1/21/1985|Monday, January 21, 1985|01211985 5:44|21/1/1985 5:44 AM|Q1 1985
|16-Aug-1985 01:11:56|8/16/1985|Friday, August 16, 1985|08161985 1:11|16/8/1985 1:11 AM|Q3 1985
|20-Dec-2033 18:36:29|12/20/2033|Tuesday, December 20, 2033|12202033 18:36|20/12/2033 6:36 PM|Q4 2033
|16-Jul-1984 10:21:59|7/16/1984|Monday, July 16, 1984|07161984 10:21|16/7/1984 10:21 AM|Q3 1984
|13-Jan-2038 10:59:36|1/13/2038|Wednesday, January 13, 2038|01132038 10:59|13/1/2038 10:59 AM|Q1 2038
|14-Aug-1982 15:13:54|8/14/1982|Saturday, August 14, 1982|08141982 15:13|14/8/1982 3:13 PM|Q3 1982
|22-Nov-2030 08:18:08|11/22/2030|Friday, November 22, 2030|11222030 8:18|22/11/2030 8:18 AM|Q4 2030
|21-Oct-1997 08:42:58|10/21/1997|Tuesday, October 21, 1997|10211997 8:42|21/10/1997 8:42 AM|Q4 1997
|28-Nov-2006 14:19:15|11/28/2006|Tuesday, November 28, 2006|11282006 14:19|28/11/2006 2:19 PM|Q4 2006
|29-Apr-2031 04:59:45|4/29/2031|Tuesday, April 29, 2031|04292031 4:59|29/4/2031 4:59 AM|Q2 2031
|29-Jan-2032 02:38:36|1/29/2032|Thursday, January 29, 2032|01292032 2:38|29/1/2032 2:38 AM|Q1 2032
|11-May-2028 15:31:52|5/11/2028|Thursday, May 11, 2028|05112028 15:31|11/5/2028 3:31 PM|Q2 2028
|15-Jul-1977 12:45:39|7/15/1977|Friday, July 15, 1977|07151977 12:45|15/7/1977 12:45 PM|Q3 1977
|27-Jan-2029 05:55:41|1/27/2029|Saturday, January 27, 2029|01272029 5:55|27/1/2029 5:55 AM|Q1 2029
|03-Mar-2024 10:17:49|3/3/2024|Sunday, March 3, 2024|03032024 10:17|3/3/2024 10:17 AM|Q1 2024
|14-Apr-2010 00:23:13|4/14/2010|Wednesday, April 14, 2010|04142010 0:23|14/4/2010 12:23 AM|Q2 2010


### <a name="d3-mapping-time-to-time-periods"></a>D3. Zuordnen von Zeit zu Zeiträumen

Diese Zuordnungen von DateTime-Werten zu Zeiträumen wurden mithilfe von verschiedenen By-Example-Transformationen für dasselbe Dataset durchgeführt. Fettgedruckte Zeichenfolgen stehen für die Beispiele, die während der jeweiligen Transformation angegeben wurden.

|DateTime|Period(Seconds)|Period(Minutes)|Period(Two Hours)|Period(30 Minutes)|
|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**0 - 20**|**45 - 60**|**5AM - 7AM**|**5:30 - 6:00**|
|17-Jan-1990 13:32:01|**0 - 20**|30 - 45|1PM - 3PM|13:30 - 14:00|
|14-Feb-2034 05:36:07|0 - 20|30 - 45|5AM - 7AM|5:30 - 6:00|
|14-Mar-2002 13:16:16|0 - 20|15 - 30|1PM - 3PM|13:00 - 13:30|
|21-Jan-1985 05:44:43|40 - 60|30 - 45|5AM - 7AM|5:30 - 6:00|
|16-Aug-1985 01:11:56|40 - 60|0 - 15|1AM - 3AM|1:00 - 1:30|
|20-Dec-2033 18:36:29|20 - 40|30 - 45|5PM - 7PM|18:30 - 19:00|
|16-Jul-1984 10:21:59|40 - 60|15 - 30|9AM - 11AM|10:00 - 10:30|
|13-Jan-2038 10:59:36|20 - 40|45 - 60|9AM - 11AM|10:30 - 11:00|
|14-Aug-1982 15:13:54|40 - 60|0 - 15|3PM - 5PM|15:00 - 15:30|
|22-Nov-2030 08:18:08|0 - 20|15 - 30|7AM - 9AM|8:00 - 8:30|
|21-Oct-1997 08:42:58|40 - 60|30 - 45|7AM - 9AM|8:30 - 9:00|
|28-Nov-2006 14:19:15|0 - 20|15 - 30|1PM - 3PM|14:00 - 14:30|
|29-Apr-2031 04:59:45|40 - 60|45 - 60|3AM - 5AM|4:30 - 5:00|
|29-Jan-2032 02:38:36|20 - 40|30 - 45|1AM - 3AM|2:30 - 3:00|
|11-May-2028 15:31:52|40 - 60|30 - 45|3PM - 5PM|15:30 - 16:00|
|15-Jul-1977 12:45:39|20 - 40|45 - 60|11AM - 1PM|12:30 - 13:00|
|27-Jan-2029 05:55:41|40 - 60|45 - 60|5AM - 7AM|5:30 - 6:00|
|03-Mar-2024 10:17:49|40 - 60|15 - 30|9AM - 11AM|10:00 - 10:30|
|14-Apr-2010 00:23:13|0 - 20|15 - 30|11PM - 1AM|0:00 - 0:30|

## <a name="examples-of-composite-transformations-by-example"></a>Beispiele für zusammengesetzte Transformationen nach Beispiel

|tripduration|starttime|start station id|start station latitude|start station longitude|usertype|Column|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|61|2016-01-08 16:09:32|107|42.3625|-71.08822|Subscriber|**A Subscriber picked a bike from station 107, lat/long (42.363,-71.088), on Jan 08, 2016 at around 4PM. The trip duration was 61 minutes**|
|61|2016-01-17 09:28:10|74|42.373268|-71.118579|Customer|A Customer picked a bike from station 74, lat/long (42.373,-71.119), on Jan 17, 2016 at around 9AM. The trip duration was 61 minutes|
|62|2016-01-25 08:10:26|176|42.386748020450561|-71.119018793106079|Subscriber|A Subscriber picked a bike from station 176, lat/long (42.387,-71.119), on Jan 25, 2016 at around 8AM. The trip duration was 62 minutes|
|63|2016-01-08 10:10:29|107|42.3625|-71.08822|Subscriber|A Subscriber picked a bike from station 107, lat/long (42.363,-71.088), on Jan 08, 2016 at around 10AM. The trip duration was 63 minutes|
|64|2016-01-15 19:42:08|68|42.36507|-71.1031|Subscriber|A Subscriber picked a bike from station 68, lat/long (42.365,-71.103), on Jan 15, 2016 at around 7PM. The trip duration was 64 minutes|
|64|2016-01-22 18:16:13|115|42.387995|-71.119084|Subscriber|A Subscriber picked a bike from station 115, lat/long (42.388,-71.119), on Jan 22, 2016 at around 6PM. The trip duration was 64 minutes|
|68|2016-01-18 09:51:52|178|42.359573201090441|-71.101294755935669|Subscriber|A Subscriber picked a bike from station 178, lat/long (42.360,-71.101), on Jan 18, 2016 at around 9AM. The trip duration was 68 minutes|
|69|2016-01-14 08:57:55|176|42.386748020450561|-71.119018793106079|Subscriber|A Subscriber picked a bike from station 176, lat/long (42.387,-71.119), on Jan 14, 2016 at around 8AM. The trip duration was 69 minutes|
|69|2016-01-13 22:12:55|141|42.363560158429884|-71.08216792345047|Subscriber|A Subscriber picked a bike from station 141, lat/long (42.364,-71.082), on Jan 13, 2016 at around 10PM. The trip duration was 69 minutes|
|69|2016-01-15 08:13:09|176|42.386748020450561|-71.119018793106079|Subscriber|A Subscriber picked a bike from station 176, lat/long (42.387,-71.119), on Jan 15, 2016 at around 8AM. The trip duration was 69 minutes|


## <a name="technical-notes"></a>Technische Hinweise

### <a name="conditional-transformations"></a>Bedingte Transformationen
In einigen Fällen kann es vorkommen, dass keine einzelne Transformation gefunden wird, die für die angegebenen Beispiele geeignet ist. In diesen Situationen wird über die Transformation „Derive Column by Example“ (Spalte nach Beispiel ableiten) versucht, die Eingaben anhand eines Musters zu gruppieren und eine separate Transformation für jede Gruppe zu erlernen. Dies wird als **bedingte Transformation** bezeichnet. Es wird nur versucht, die **bedingte Transformation** für Transformationen mit einer einzelnen Eingabespalte durchzuführen. 

### <a name="reference"></a>Referenz
Weitere Informationen zur Technologie „Transformationen von Zeichenfolgen nach Beispiel“ finden Sie in [dieser Veröffentlichung](https://www.microsoft.com/en-us/research/publication/automating-string-processing-spreadsheets-using-input-output-examples/).

