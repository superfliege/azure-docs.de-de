---
title: "Bikesharing-Tutorial – Erweiterte Datenvorbereitung mit der Azure Machine Learning Workbench"
description: End-to-End-Tutorial zur Datenvorbereitung mithilfe der Azure Machine Learning Workbench
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: article
ms.date: 09/21/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 722657c9bbae23a051a63972a8800d3cc40e7e40
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Bikesharing-Tutorial: Erweiterte Datenvorbereitung mit der Azure Machine Learning Workbench
Azure Machine Learning-Dienste (Vorschau) ist eine integrierte, End-to-End Data Science- und erweiterte Analyselösung für professionelle Datenspezialisten zum Vorbereiten von Daten, Entwickeln von Experimenten und Bereitstellen von Modellen auf Cloudniveau.

In diesem Tutorial verwenden Sie Azure Machine Learning-Dienste (Vorschauversion), um Folgendes zu lernen:
> [!div class="checklist"]
> * Interaktives Vorbereiten von Daten mit dem Azure Machine Learning-Datenvorbereitungstool
> * Importieren, Transformieren und Erstellen eines Testdatasets
> * Generieren eines Datenvorbereitungspakets
> * Ausführen des Datenvorbereitungspakets mit Python
> * Generieren eines Trainingsdatasets durch Wiederverwenden des Datenvorbereitungspakets für weitere Eingabedateien

> [!IMPORTANT]
> In diesem Tutorial werden lediglich die Daten vorbereitet; das Prognosemodell wird nicht erstellt.
>
> Sie können anhand der vorbereiteten Daten Ihre eigenen Prognosemodelle trainieren. Sie können beispielsweise ein Modell für die Vorhersage der Nachfrage nach Fahrrädern während eines 2-Stunden-Zeitfensters erstellen.

## <a name="prerequisites"></a>Voraussetzungen
* Azure Machine Learning Workbench muss lokal installiert sein. Wenn Sie weitere Informationen benötigen, befolgen Sie die Anweisungen im [Schnellstart zur Installation](quickstart-installation.md).
* Kenntnisse in Bezug auf das Erstellen eines neuen Projekts in der Workbench.

## <a name="data-acquisition"></a>Datenerfassung
In diesem Tutorial werden das [Boston Hubway-Dataset](https://s3.amazonaws.com/hubway-data/index.html) und Boston-Wetterdaten von [NOAA](http://www.noaa.gov/) verwendet.

1. Laden Sie die Datendateien über die folgenden Links in Ihre lokale Entwicklungsumgebung herunter. 
   * [Boston-Wetterdaten](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway-Fahrtdaten von der Hubway-Website.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Entpacken Sie jede heruntergeladene ZIP-Datei.

## <a name="learn-about-the-datasets"></a>Weitere Informationen zu den Datasets
1. Die Datei __Boston weather__ enthält die folgenden wetterbezogenen Felder mit stündlich gemeldeten Werten:
   * DATE
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. Die __Hubway__ Daten sind in Dateien nach Jahr und Monat organisiert. Die Datei `201501-hubway-tripdata.zip` enthält beispielsweise eine CSV-Datei mit Daten zum Januar 2015. Die Daten enthalten die folgenden Felder, wobei jede Zeile eine Fahrradstrecke darstellt:

   * Trip Duration (in seconds) (Dauer der Fahrt (in Sekunden))
   * Start Time and Date (Startzeit und -datum)
   * Stop Time and Date (Endzeit und -datum)
   * Start Station Name & ID (Name und ID der Startstation)
   * Name und ID der Endstation (Name und ID der Endstation)
   * Bike ID (Rad-ID)
   * User Type (Nutzertyp): Casual (Gelegentlich) = Nutzer mit 24-Stunden- oder 72-Stunden-Pass; Member (Mitglied) = Jahres- oder Monatsmitglied
   * ZIP Code (Postleitzahl), sofern der Nutzer Mitglied ist
   * Gender (Geschlecht), Selbstauskunft des Mitglieds

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts
1. Starten Sie die **Azure Machine Learning Workbench** über das Startmenü oder das Startprogramm.

2. Erstellen Sie ein neues Azure Machine Learning-Projekt.  Klicken Sie auf die Schaltfläche **+** auf der Seite **Projekte**, oder klicken Sie auf **Datei** > **Neu**.
   - Verwenden Sie die Vorlage **Blank Project** (Leeres Projekt).
   - Benennen Sie das Projekt **BikeShare**. 

## <a id="newdatasource"></a>Erstellen einer neuen Datenquelle

1. Erstellen Sie eine neue Datenquelle. Klicken Sie auf der linken Symbolleiste auf die Schaltfläche **Daten** (Zylindersymbol). Dadurch wird die **Datenansicht**  geöffnet.

   ![Abbildung der Registerkarte „Datenansicht“](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Fügen Sie eine Datenquelle hinzu. Klicken Sie auf das Symbol **+**, und wählen Sie **Datenquelle hinzufügen** aus.

   ![Abbildung des Eintrags „Datenquelle hinzufügen“](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Hinzufügen der Wetterdaten

1. **Datenspeicher**: Wählen Sie den Datenspeicher aus, der die Daten enthält. Da Sie Dateien verwenden, wählen Sie **File(s)/Directory** (Datei(en)/Verzeichnis) aus. Wählen Sie **Weiter**, um fortzufahren.

   ![Abbildung des Eintrags „File(s)/Directory“ (Datei(en)/Verzeichnis)](media/tutorial-bikeshare-dataprep/datasources.png)

2. **File Selection** (Dateiauswahl): Fügen Sie die Wetterdaten hinzu. Suchen Sie nach der zuvor heruntergeladenen Datei `BostonWeather.csv`, und wählen Sie sie aus. Klicken Sie auf **Weiter**.


   ![Abbildung der Dateiauswahl mit ausgewählter Datei „BostonWeater.csv“](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **File Details** (Dateidetails): Überprüfen Sie das erkannte Dateischema. Azure Machine Learning Workbench analysiert die Dateien in der Datei und leitet das zu verwendende Schema ab.

   ![Abbildung der Dateidetails](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Die Workbench erkennt in einigen Fällen möglicherweise nicht das richtige Schema. Vergewissern Sie sich stets, dass die Parameter für das jeweilige Dataset korrekt sind. Stellen Sie für die Wetterdaten sicher, dass sie auf die folgenden Werte festgelegt sind:
   >
   > * __File Type__ (Dateityp): Delimited File (csv, tsv, txt, etc.) (Durch Trennzeichen getrennte Datei (CSV, TSV, TXT usw.))
   > * __Separator__ (Trennzeichen): Komma [,]
   > * __Comment Line Character__ (Kommentarzeilenzeichen): Kein Wert festgelegt.
   > * __Skip Lines Mode__ (Modus „Zeilen überspringen“): Don't skip (Nicht überspringen)
   > * __File Encoding__ (Dateicodierung): UTF-8
   > * __Promote Headers Mode__ (Modus „Header heraufstufen“): Use Headers From First File (Header aus erster Datei verwenden)

   In der Vorschau der Daten sollten die folgenden Spalten angezeigt werden:
   * **Path**
   * **DATE**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Klicken Sie auf **Weiter**, um fortzufahren.

4. **Datentypen**: Überprüfen Sie die Datentypen, die automatisch erkannt werden. Azure Machine Learning Workbench analysiert die Dateien in der Datei und leitet die zu verwendenden Datentypen ab.

   Ändern Sie für diese Daten den `DATA TYPE` aller Spalten in `String`.

   > [!NOTE]
   > `String` wird verwendet, um die Funktionen der Workbench später in diesem Tutorial hervorzuheben. 

   ![Abbildung der Datentypen](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Klicken Sie auf __Weiter__, um fortzufahren. 

5. **Sampling** (Stichproben): Klicken Sie zum Erstellen eines Stichprobenschemas auf die Schaltfläche **+ New** (+ Neu). Wählen Sie die hinzugefügte neue Zeile __Top 10000__ aus, und wählen Sie dann __Edit__ (Bearbeiten) aus. Legen Sie die __Sample Strategy__ (Stichprobenstrategie) auf **Full File** (Vollständige Datei) fest, und wählen Sie dann **Anwenden** aus.

   ![Abbildung des Hinzufügens einer neuen Stichprobenstrategie](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Wählen Sie zum Verwenden der Strategie __Full File__ (Vollständige Datei) den Eintrag __Full File__ und anschließend __Set as Active__ (Als aktiv festlegen) aus. Ein Stern wird neben __Full File__ (Vollständige Datei) angezeigt; damit wird sie als aktive Strategie gekennzeichnet.

   ![Abbildung von „Full File“ (Vollständige Datei) als aktive Strategie](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Klicken Sie auf **Weiter**, um fortzufahren.

6. **Path Column** (Pfad-Spalte): Im Abschnitt __Path Column__ können Sie den vollständigen Dateipfad als Spalte in den importierten Daten einschließen. Wählen Sie __Do Not Include Path Column__ (Pfad-Spalte nicht einschließen).

   > [!TIP]
   > Das Einschließen des Pfads als Spalte ist hilfreich, wenn Sie einen Ordner mit vielen Dateien mit unterschiedlichen Dateinamen importieren. Es empfiehlt sich auch, wenn die Dateinamen Informationen enthalten, die Sie später extrahieren möchten.

   ![Abbildung mit Einstellung, dass Pfad-Spalte nicht eingeschlossen werden soll](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Finish** (Fertig stellen): Klicken Sie zum Fertigstellen der Datenquelle auf die Schaltfläche **Finish** (Fertig stellen).

    Eine Registerkarte für eine neue Datenquelle mit dem Namen __BostonWeather__ wird geöffnet. Eine Stichprobe der Daten wird in einer Rasteransicht angezeigt. Die Stichprobe basiert auf dem angegebenen aktiven Stichprobenschema.

    Beachten Sie, dass im Bereich **Steps** (Schritte) rechts im Bildschirm die einzelnen Aktionen angezeigt werden, die beim Erstellen dieser Datenquelle ausgeführt wurden.

   ![Abbildung der Datenquelle, der Stichprobe und der Schritte](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Anzeigen der Datenquellenmetriken

Klicken Sie oben links von der Rasteransicht der Registerkarte auf __Metrics__ (Metriken). In dieser Ansicht werden die Verteilung sowie andere aggregierte Statistiken der Stichprobendaten angezeigt.

![Abbildung der Metriken](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Verwenden Sie die Dropdownliste **Choose Metric** (Metrik auswählen), um die Sichtbarkeit der Statistiken zu konfigurieren. Aktivieren und deaktivieren Sie Metriken, um die Rasteransicht zu ändern.

Wenn Sie zur __Datenansicht__ zurückkehren möchten, wählen Sie oben links auf der Seite __Daten__ aus.

## <a name="add-data-source-to-data-preparation-package"></a>Hinzufügen einer Datenquelle zum Datenvorbereitungspaket

1. Wählen Sie __Prepare__ (Vorbereiten) aus, um mit dem Vorbereiten der Daten zu beginnen. 

2. Geben Sie bei einer entsprechenden Aufforderung einen Namen für das Datenvorbereitungspaket ein, z.B. `BikeShare Data Prep`. 

3. Klicken Sie auf __OK__ , um fortzufahren.

   ![Abbildung des Dialogfelds „Prepare“ (Vorbereiten)](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Ein neues Paket mit dem Namen **BikeShare Data Prep** wird im Abschnitt __Data Preparation__ (Datenvorbereitung) der Registerkarte __Daten__ angezeigt. 

   Wählen Sie diesen Eintrag aus, um das Paket anzuzeigen. 

5. Klicken Sie auf die Schaltfläche **>>** zum Erweitern, um die im Paket enthaltenen __Dataflows__ (Datenflüsse) einzublenden. In diesem Beispiel ist __BostonWeather__ der einzige Datenfluss.

   > [!IMPORTANT]
   > Ein Paket kann mehrere Datenflüsse enthalten.

   ![Abbildung der im Paket enthaltenen Datenflüsse](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtern von Daten nach Wert
1. Klicken Sie zum Filtern der Daten mit der rechten Maustaste auf eine Zelle mit einem bestimmten Wert, und wählen Sie __Filter__ und anschließend den Filtertyp aus.

2. Wählen Sie für dieses Tutorial eine Zelle mit dem Wert `FM-15` aus, und legen Sie den Filter auf den Typ **Ist gleich** fest.  Nun werden die Daten so gefiltert, dass nur Zeilen zurückgegeben werden, bei denen der __REPORTTYPE__ gleich `FM-15` ist.

   ![Abbildung des Filter-Dialogfelds](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 ist ein Typ von METAR (Meteorological Terminal Aviation Routine Weather Report). Die FM-15-Berichte sind empirisch betrachtet die vollständigsten Daten, bei denen die wenigsten Daten fehlen.

## <a name="remove-a-column"></a>Entfernen einer Spalte

Die Spalte __REPORTTYPE__ wird nicht mehr benötigt. Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift, und wählen Sie **Spalte entfernen** aus.

   ![Abbildung der Option „Spalte entfernen“](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Ändern von Datentypen und Entfernen von Fehlern
1. Wenn Sie beim Auswählen von Spaltenüberschriften __STRG__ drücken, können Sie mehrere Spalten gleichzeitig auswählen. Verwenden Sie diese Option, um die folgenden Spaltenüberschriften auswählen:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Klicken Sie mit der rechten Maustaste** auf eine der ausgewählten Spaltenüberschriften, und wählen Sie die Option **Convert Field Type to Numeric** (Feldtyp in Numerisch konvertieren) aus. Dadurch wird der Datentyp der Spalten in „Numerisch“ geändert.

   ![Konvertieren mehrerer Spalten in „Numerisch“](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filtern Sie die Fehlerwerte heraus. Bei einigen Spalten treten Probleme beim Konvertieren des Datentyps auf. Dieses Problem wird durch die rote Hervorhebung der __Datenqualitätsleiste__ für die Spalte angezeigt.

   Klicken Sie zum Entfernen der Spalten mit Fehlern mit der rechten Maustaste auf die Spaltenüberschrift **HOURLYDRYBULBTEMPF**. Wählen Sie **Filter Column** (Spalte filtern) aus. Übernehmen Sie für **I Want To** (Ich möchte) den Standardwert **Keep Rows** (Zeilen beibehalten). Ändern Sie die Auswahl in der Dropdownliste **Conditions** (Bedingungen) in **is not error** (Ist kein Fehler). Klicken Sie auf **OK**, um den Filter anzuwenden.

4. Um die verbleibenden Fehlerzeilen in den anderen Spalten zu entfernen, wiederholen Sie diesen Filtervorgang für die Spalten **HOURLYRelativeHumidity** und **HOURLYWindSpeed**.

## <a name="use-by-example-transformations"></a>Verwenden von Transformationen nach Beispiel

Zum Verwenden der Daten in einer Prognose für Zwei-Stunden-Zeitblöcke müssen Sie die durchschnittlichen Wetterbedingungen für Zwei-Stunden-Zeiträume berechnen. Hierzu können Sie folgende Aktionen ausführen:

* Teilen Sie die Spalte **DATE** in separate Spalten **Date** und **Time**. Die ausführlichen Schritte hierzu finden Sie im nächsten Abschnitt.

* Leiten Sie eine Spalte **Hour_Range** von der Spalte **Time** ab. Die ausführlichen Schritte hierzu finden Sie im folgenden Abschnitt.

* Leiten Sie eine Spalte **Date\_Hour\_Range** von den Spalten **DATE** und **Hour_Range** ab. Die ausführlichen Schritte hierzu finden Sie im folgenden Abschnitt.

### <a name="split-column-by-example"></a>Split column by example (Spalte nach Beispiel teilen)

1. Teilen Sie die Spalte **DATE** in separate Spalten für Datum und Uhrzeit. Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift **DATE**, und wählen Sie die Option **Split Column by Example** (Spalte nach Beispiel teilen) aus.

   ![Abbildung des Eintrags „Split Column by Example“ (Spalte nach Beispiel teilen)](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning Workbench bestimmt automatisch ein sinnvolles Trennzeichen und erstellt zwei Spalten, indem die Daten in Datums- und Uhrzeitwerte geteilt werden. 

3. Wählen Sie __OK__ aus, um die Ergebnisse des Teilungsvorgangs zu akzeptieren.

   ![Abbildung der geteilten Spalten DATE_1 und DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Derive column by example (Spalte nach Beispiel ableiten)

1. Klicken Sie zum Ableiten eines Zwei-Stunden-Zeitraums mit der rechten Maustaste auf die Spaltenüberschrift __DATE\_2__, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus.

   ![Abbildung des Eintrags „Derive Column by Example“ (Spalte nach Beispiel ableiten)](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Eine neue leere Spalte mit NULL-Werten wird hinzugefügt.

2. Klicken Sie auf die erste leere Zelle in der neuen Spalte. Geben Sie ein Beispiel für den gewünschten Zeitraum an, indem Sie in der neuen Spalte `12AM-2AM` eingeben und die EINGABETASTE drücken.

   ![Abbildung der neuen Spalte mit dem Wert 12AM-2AM](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML Workbench erzeugt ein Programm auf der Grundlage der von Ihnen angegebenen Beispiele und wendet dasselbe Programm auf die restlichen Zeilen an. Alle übrigen Zeilen werden gemäß dem angegebenen Beispiel automatisch aufgefüllt. Die Workbench analysiert zudem die Daten und versucht, Grenzfälle zu ermitteln. 

3. Der Text **Analyzing Data** (Daten werden analysiert) über dem Raster zeigt an, dass die Workbench versucht, Grenzfälle zu ermitteln. Nach Abschluss des Vorgangs wechselt der Status zu **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) oder **No suggestions** (Keine Vorschläge). In diesem Beispiel wird **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) zurückgegeben.

4. Wählen Sie zum Prüfen der vorgeschlagenen Änderungen die Option **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) aus. Die zu prüfende und ggf. zu korrigierende Zelle wird in der Anzeige hervorgehoben.

   ![Abbildung zum Prüfen der Zeile](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Wählen Sie __OK__ aus, um die Transformation zu akzeptieren.
 
5. Sie kehren zur Rasteransicht der Daten für __BostonWeather__ zurück. Das Raster enthält nun die drei kürzlich hinzugefügten Spalten.

   ![Abbildung der Rasteransicht mit hinzugefügten Zeilen](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Alle vorgenommenen Änderungen werden im Bereich **Steps** (Schritte) dokumentiert. Wechseln Sie zum erstellten Schritt im Bereich **Steps** (Schritte), klicken Sie auf den nach unten zeigenden Pfeil, und wählen Sie **Edit** (Bearbeiten) aus. Das erweiterte Fenster für **Derive Column by Example** (Spalte nach Beispiel ableiten) wird angezeigt. Alle Ihre Beispiele sind hier aufgeführt. Sie können Beispiele auch manuell hinzufügen, indem Sie unten im Raster auf eine Zeile doppelklicken. Wählen Sie **Cancel** (Abbrechen) aus, um zum Hauptraster zurückzukehren, ohne dass die Änderungen angewendet werden. Sie können diese Ansicht auch aufrufen, indem Sie die Option **Advanced Mode** (Erweiterter Modus) auswählen, während Sie eine Transformation vom Typ **Derive Column by Example** (Spalte nach Beispiel ableiten) ausführen.

6. Doppelklicken Sie zum Umbenennen der Spalte auf die Spaltenüberschrift, und geben Sie **Hour Range** ein. Drücken Sie die **EINGABETASTE**, um die Änderung zu speichern.

   ![Umbenennen der Spalte](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Wählen Sie zum Ableiten des Datums- und Stundenbereichs die Spalten **Date\_1** und **Hour Range** gleichzeitig aus, klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Derive column by example** (Spalte nach Beispiel ableiten) aus.

   ![Teilen der Spalte anhand eines Beispiels](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Geben Sie `Jan 01, 2015 12AM-2AM` als Beispiel für die erste Zeile ein, und drücken Sie die **EINGABETASTE**.

   Die Workbench bestimmt die Transformation anhand des angegebenen Beispiels. In diesem Beispiel erhalten wir folgendes Ergebnis: Das Datumsformat wird geändert, und das Datum wird mit dem Zwei-Stunden-Fenster verkettet.

   ![Abbildung des Beispiels „Jan 01, 2015 12AM-2AM“](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

8. Warten Sie, bis der Status von **Analyzing Data** (Daten werden analysiert) zu **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) wechselt. Dies kann einige Sekunden dauern. Klicken Sie auf den Statuslink, um zur vorgeschlagenen Zeile zu wechseln. 

   ![Abbildung der zur Prüfung vorgeschlagenen Zeile](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Die Zeile enthält einen NULL-Wert, da der Quelldatenwert sich entweder auf dd/mm/yyyy oder auf mm/dd/yyyy beziehen kann. Geben Sie den richtigen Wert `Jan 13, 2015 2AM-4AM` ein, und drücken Sie die **EINGABETASTE**. Die Workbench verbessert anhand der beiden Beispiele die Ableitung für die übrigen Zeilen.

   ![Abbildung der ordnungsgemäß formatierten Daten](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Wählen Sie **OK** aus, um die Transformation zu akzeptieren.

   ![Abbildung des vollständig transformierten Rasters](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Sie können nun den erweiterten Modus von **Derive column by example** (Spalte nach Beispiel ableiten) für diesen Schritt verwenden, indem Sie im Bereich **Steps** (Schritte) auf den Pfeil nach unten klicken. Im Datenraster befinden sich Kontrollkästchen neben den Namen der Spalten **DATE\_1** und **Hour Range**. Deaktivieren Sie das Kontrollkästchen neben der Spalte **Hour Range**, um festzustellen, wie sich dadurch die Ausgabe ändert. Wenn die Spalte **Hour Range** nicht mehr als Eingabe vorhanden ist, wird **12AM-2AM** als Konstante behandelt und an die abgeleiteten Werte angefügt. Wählen Sie **Cancel** (Abbrechen) aus, um zum Hauptraster zurückzukehren, ohne dass die vorgenommenen Änderungen angewendet werden.

10. Doppelklicken Sie auf die Spaltenüberschrift, um die Spalte umzubenennen. Ändern Sie den Namen in **Date Hour Range**, und drücken Sie anschließend die **EINGABETASTE**.

11. Wählen Sie gleichzeitig die Spalten **DATE**, **DATE\_1**, **DATE\_2** und **Hour Range** aus. Klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Remove column** (Spalte entfernen) aus.

## <a name="summarize-data-mean"></a>Zusammenfassen von Daten (Mittelwert)

Im nächsten Schritt werden die Wetterbedingungen zusammengefasst, indem der Mittelwert der Werte berechnet wird, gruppiert nach Stundenbereich.

1. Wählen Sie die Spalte **Date Hour Range** aus, und wählen Sie anschließend im Menü **Transforms** (Transformationen) die Option **Summarize** (Zusammenfassen) aus.

    ![Menü „Transforms“ (Transformationen)](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Zum Zusammenfassen der Daten ziehen Sie Spalten aus dem Raster am unteren Rand der Seite in den linken und den rechten Bereich am oberen Rand. Im linken Bereich befindet sich der Text **Drag columns here to group data** (Spalten hierher ziehen, um Daten zu gruppieren). Im rechten Bereich befindet sich der Text **Drag columns here to summarize data** (Spalten hierher ziehen, um Daten zusammenzufassen). 

    Ziehen Sie die Spalte **Date Hour Range** aus dem Raster unten in den linken Bereich. Ziehen Sie die Spalten **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** und **HOURLYWindSpeed** in den rechten Bereich. 

    Wählen Sie im rechten Bereich **Mean** (Mittelwert) als Measure **Aggregate** (Aggregat) für jede Spalte aus. Klicken Sie auf **OK**, um das Zusammenfassen abzuschließen.

   ![Abbildung des Bildschirms mit zusammengefassten Daten](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Transformieren des Datenflusses mithilfe eines Skripts

Durch das Ändern der Daten in den numerischen Spalten in einen Bereich von 0-1 können einige Modelle schnell konvergiert werden. Derzeit gibt es keine integrierte Transformation, um diese Transformation generisch auszuführen, der Vorgang kann jedoch mithilfe eines Python-Skripts ausgeführt werden.

1. Wählen Sie im Menü **Transforms** (Transformationen) die Option **Transform Dataflow** (Datenfluss transformieren) aus.

2. Geben Sie im angezeigten Textfeld den folgenden Code ein. Wenn Sie die Spaltennamen verwendet haben, sollte der Code funktionieren, ohne dass er geändert werden muss. Sie schreiben eine einfache Min-Max-Normalisierungslogik in Python.

    > [!WARNING]
    > Das Skript erwartet die bisher in diesem Tutorial verwendeten Spaltennamen. Sollten Sie andere Spaltennamen angegeben haben, müssen Sie die Namen im Skript ändern.

   ```python
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   minVal = min(df["HOURLYDRYBULBTEMPF_Mean"])
   df["HOURLYDRYBULBTEMPF_Mean"] = (df["HOURLYDRYBULBTEMPF_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYDRYBULBTEMPF_Mean":"N_DryBulbTemp"},inplace=True)

   maxVal = max(df["HOURLYRelativeHumidity_Mean"])
   minVal = min(df["HOURLYRelativeHumidity_Mean"])
   df["HOURLYRelativeHumidity_Mean"] = (df["HOURLYRelativeHumidity_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYRelativeHumidity_Mean":"N_RelativeHumidity"},inplace=True)

   maxVal = max(df["HOURLYWindSpeed_Mean"])
   minVal = min(df["HOURLYWindSpeed_Mean"])
   df["HOURLYWindSpeed_Mean"] = (df["HOURLYWindSpeed_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYWindSpeed_Mean":"N_WindSpeed"},inplace=True)

   df
   ```

    > [!TIP]
    > Das Python-Skript muss am Ende `df` zurückgeben. Mit diesem Wert wird das Raster gefüllt.
    
    ![Skriptdialogfeld „Transform Dataflow“ (Datenfluss transformieren)](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Klicken Sie auf __OK__, um das Skript zu verwenden. Die numerischen Spalten im Raster enthalten nun Werte im Bereich 0-1.

    ![Raster mit Werten zwischen 0 und 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Sie haben die Vorbereitung der Wetterdaten abgeschlossen. Als Nächstes bereiten Sie die Fahrtdaten vor.

## <a name="load-trip-data"></a>Laden der Fahrtdaten

1. Führen Sie zum Importieren der Datei `201701-hubway-tripdata.csv` die Schritte im Abschnitt [Erstellen Sie eine neue Datenquelle](#newdatasource) aus. Verwenden Sie während des Importvorgangs die folgenden Optionen:

    * __Sampling scheme__ (Stichprobenschema): Stichprobenschema **Full File** (Vollständige Datei), legen Sie die Stichprobe als aktiv fest, und 
    * __Data Type__ (Datentyp): Übernehmen Sie die Standardwerte.

2. Klicken Sie nach dem Importieren der Daten auf die Schaltfläche __Prepare__ (Vorbereiten), um mit dem Vorbereiten der Daten zu beginnen. Wählen Sie das vorhandene Paket **BikeShare Data Prep.dprep** aus, und klicken Sie anschließend auf __OK__.

    Durch diesen Prozess wird der vorhandenen **Data Preparation**-Datei ein **Datenfluss** hinzugefügt und kein neuer erstellt.

    ![Abbildung: Auswählen des vorhandenen Pakets](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Erweitern Sie nach dem Laden des Rasters __DATAFLOWS__ (DATENFLÜSSE). Nun sind zwei Datenflüsse vorhanden: **BostonWeather** und **201701-hubway-tripdata**. Wählen Sie den Eintrag **201701-hubway-tripdata** aus.

    ![Abbildung des Eintrags „201701-hubway-tripdata“](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Verwenden des Karteninspektors

Für die Datenvorbereitung ist eine Reihe von hilfreichen Visualisierungen (als **Inspectors** (Inspektoren) bezeichnet) für Zeichenfolgendaten, numerische Daten und geografische Daten verfügbar, anhand derer Sie die Daten besser verstehen und Ausreißer identifizieren können. Führen Sie die folgenden Schritte aus, um den Karteninspektor zu verwenden:

1. Wählen Sie die Spalten **start station latitude** und **start station longitude** gleichzeitig aus. Klicken Sie mit der rechten Maustaste auf eine der Spalten, und wählen Sie die Option **Map** (Karte) aus.

    > [!TIP]
    > Halten Sie für die Mehrfachauswahl __STRG__ gedrückt, und wählen Sie die Spaltenüberschriften der Spalten aus.

    ![Abbildung der Kartenvisualisierung](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Klicken Sie zum Maximieren der Kartenvisualisierung auf das Symbol **Maximieren**. Wählen Sie zum Anpassen der Karte an die Fenstergröße das Symbol **E** oben links in der Visualisierung aus.

    ![Maximiertes Bild](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Klicken Sie auf die Schaltfläche zum **Minimieren**, um zur Rasteransicht zurückzukehren.

## <a name="use-column-statistics-inspector"></a>Verwenden des Spaltenstatistik-Inspektors

Klicken Sie zum Verwenden des Spaltenstatistik-Inspektors mit der rechten Maustaste auf die Spalte __tripduration__, und wählen Sie die Option __Column Statistics__ (Spaltenstatistiken) aus.

![Eintrag „Column Statistics“ (Spaltenstatistiken)](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Durch diesen Vorgang wird die neue Visualisierung __tripduration Statistics__ im Bereich __INSPECTORS__ (INSPEKTOREN) hinzugefügt.

![Abbildung des Inspektors „tripduration Statistics“](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Der maximale Wert der Fahrtdauer beträgt **961.814 Minute** (ca. zwei Jahre). Anscheinend sind im Dataset einige Ausreißer enthalten.

## <a name="use-histogram-inspector"></a>Verwenden des Histogramm-Inspektors

Klicken Sie zum Ermitteln der Ausreißer mit der rechten Maustaste auf die Spalte __tripduration__, und wählen Sie die Option __Histogram__ (Histogramm) aus.

![Histogramm-Inspektor](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Das Histogramm ist nicht hilfreich, da das Diagramm durch die Ausreißer schief ist.

## <a name="add-column-using-script"></a>Hinzufügen einer Spalte mithilfe eines Skripts

1. Klicken Sie mit der rechten Maustaste auf die Spalte **tripduration**, und wählen Sie die Option **Add Column (Script)** (Spalte hinzufügen (Skript)) aus.

    ![Abbildung der Menüoption „Add Column (Script)“ (Spalte hinzufügen (Skript))](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. Geben Sie im Dialogfeld __Add Column (Script)__ (Spalte hinzufügen (Skript)) die folgenden Werte an:

    * __New Column Name__ (Name der neuen Spalte): logtripduration
    * __Insert this New Column After__ (Diese neue Spalte einfügen nach): tripduration
    * __New Column Code__ (Code der neuen Spalte): `math.log(row.tripduration)`
    * __Code Block Type__ (Codeblocktyp): Expression (Ausdruck)

   ![Dialogfeld „Add Column (Script)“ (Spalte hinzufügen (Skript))](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Klicken Sie auf __OK__, um die Spalte **logtripduration** hinzuzufügen.

4. Klicken Sie mit der rechten Maustaste auf die Spalte, und wählen Sie die Option **Histogram** (Histogramm) aus.

    ![Histogramm der Spalte „logtripduration“](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Visuell ähnelt dieses Histogramm einer Normalverteilung mit einem außergewöhnlichen Randbereich.

## <a name="use-advanced-filter"></a>Verwenden von erweiterten Filtern

Durch Anwenden eines Filters auf die Daten werden die Inspektoren mit der neuen Verteilung aktualisiert. Klicken Sie mit der rechten Maustaste auf die Spalte **logtripduration**, und wählen Sie die Option **Filter Column** (Spalte filtern) aus. Geben Sie im Dialogfeld __Edit__ (Bearbeiten) die folgenden Werte an:

* __Filter this Number Column__ (Diese Zahlenspalte filtern): logtripduration
* __I Want To__ (Ich möchte): Keep Rows (Zeilen beibehalten)
* __When__ (Wenn): Any of the Conditions below are True (logical OR) (Eine der Bedingungen unten True ist (logisches OR))
* __If this Column__ (Wenn diese Spalte): less than (kleiner als)
* __The Value__ (Wert): 9

![Filteroptionen](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Klicken Sie auf __OK__, um den Filter anzuwenden.

![Aktualisierte Histogramme nach dem Anwenden des Filters](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Der Halo-Effekt

1. Maximieren Sie das Histogramm **logtripduration**. Es ist ein blaues Histogramm vorhanden, das ein graues Histogramm überlagert. Eine solche Anzeige wird als **Halo-Effekt** bezeichnet:

    * Das **graue Histogramm** stellt die Verteilung vor dem Vorgang (in diesem Fall dem Filtervorgang) dar.
    * Das **blaue Histogramm** stellt das Histogramm nach dem Vorgang dar. 

   Mit dem Halo-Effekt wird die Auswirkung eines Vorgangs auf die Daten veranschaulicht.

   ![Abbildung des Halo-Effekts](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Beachten Sie, dass das blaue Histogramm anscheinend kürzer als das vorherige ist. Dies ist auf die automatische Neugruppierung der Daten im neuen Bereich zurückzuführen.

2. Wählen Sie zum Entfernen des Halo-Effekts __Edit__ (Bearbeiten) aus, und deaktivieren Sie die Option __Show halo__ (Halo anzeigen).

    ![Optionen für das Histogramm](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Wählen Sie **OK** aus, um den Halo-Effekt zu deaktivieren, und minimieren Sie anschließend das Histogramm.

### <a name="remove-columns"></a>Entfernen von Spalten

In den Fahrtdaten stellt jede Zeile ein Fahrradabholereignis dar. Für dieses Tutorial benötigen Sie lediglich die Spalten **starttime** und **start station**. Entfernen Sie die anderen Spalten, indem Sie diese beiden Spalten gleichzeitig auswählen, mit der rechten Maustaste auf die Spaltenüberschrift klicken und anschließend die Option **Keep Column** (Spalte beibehalten) auswählen. Die übrigen Spalten werden entfernt.

![Abbildung der Option „Keep Column“ (Spalte beibehalten)](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Zusammenfassen von Daten (Anzahl)

Verwenden Sie abgeleitete Spalten, um die Nachfrage nach Fahrrädern für einen 2-Stunden-Zeitraum zusammenzufassen.

1. Klicken Sie mit der rechten Maustaste auf die Spalte **starttime**, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus.

    ![Abbildung der Option „Derive Column by Example“ (Spalte nach Beispiel ableiten)](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Geben Sie für das Beispiel den Wert `Jan 01, 2017 12AM-2AM` für die erste Zeile ein.

    > [!IMPORTANT]
    > Im vorherigen Beispiel zum Ableiten von Spalten haben Sie mehrere Schritte ausgeführt, um eine Spalte mit dem Datums-und Uhrzeitbereich abzuleiten. In diesem Beispiel werden Sie feststellen, dass dieser Vorgang auch in einem einzigen Schritt ausgeführt werden kann, indem ein Beispiel für die Endausgabe angegeben wird.

    > [!NOTE]
    > Sie können ein Beispiel für eine beliebige Zeile angeben. In diesem Beispiel ist der Wert `Jan 01, 2017 12AM-2AM` gültig für die erste Datenzeile.

    ![Abbildung der Beispieldaten](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

3. Warten Sie, bis die Anwendung die Werte für alle Zeilen berechnet hat. Dies kann einige Sekunden dauern. Klicken Sie nach Abschluss der Analyse auf den Link __Review next suggested row__ (Nächste vorgeschlagene Zeile prüfen), um die Daten zu überprüfen.

   ![Abbildung der abgeschlossenen Analyse mit Link für Überprüfung](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Vergewissern Sie sich, dass die berechneten Werte korrekt sind. Ist dies nicht der Fall, aktualisieren Sie den Wert mit dem erwarteten Wert, und drücken Sie die EINGABETASTE. Warten Sie anschließend, bis die Analyse abgeschlossen ist. Führen Sie den Vorgang **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) aus, bis der Status **No suggestions** (Keine Vorschläge) angezeigt wird. Sobald **No suggestions** (Keine Vorschläge) angezeigt wird, hat die Anwendung die Grenzfälle untersucht, und das erstellte Programm wird als zufriedenstellend erachtet. Bewährte Methode: Führen Sie eine Sichtprüfung der transformierten Daten durch, bevor Sie die Transformation akzeptieren. 

4. Wählen Sie **OK** aus, um die Transformation zu akzeptieren. Benennen Sie die neu erstellte Spalte in **Date Hour Range** um.

    ![Abbildung der umbenannten Spalte](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift **starttime**, und wählen Sie die Option **Remove column** (Spalte entfernen) aus.

6. Wählen Sie zum Zusammenfassen der Daten im Menü __Transforms__ (Transformationen) die Option __Summarize__ (Zusammenfassen) aus. Führen Sie die folgenden Aktionen aus, um die Transformation zu erstellen:

    * Ziehen Sie die Spalten __Date Hour Range__ und __start station id__ in den linken Bereich „Group by“ (Gruppieren nach).
    * Ziehen Sie __start station id__ in den rechten Bereich (zum Zusammenfassen der Daten).

   ![Abbildung der Zusammenfassungsoptionen](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Wählen Sie **OK** aus, um das Ergebnis der Zusammenfassung zu akzeptieren.

## <a name="join-dataflows"></a>Verknüpfen von Datenflüssen

Führen Sie die folgenden Schritte aus, um die Wetterdaten mit den Fahrtdaten zu verknüpfen:

1. Wählen Sie im Menü __Transforms__ (Transformationen) die Option __Join__ (Verknüpfen) aus.

2. __Tables__ (Tabellen): Wählen Sie **BostonWeather** als linken Datenfluss und **201701-hubway-tripdata** als rechten Datenfluss aus. Klicken Sie auf **Weiter**, um fortzufahren.

    ![Abbildung der Tabellenauswahl](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Key Columns__ (Schlüsselspalten): Wählen Sie die Spalte **Date Hour Range** in beiden Tabellen aus, und wählen Sie anschließend __Next__ (Weiter) aus.

    ![Abbildung der Verknüpfung von Schlüsselspalten](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Join Type__ (Verknüpfungstyp): Wählen Sie als Verknüpfungstyp __Matching rows__ (Übereinstimmende Zeilen) aus, und klicken Sie dann auf __Finish__ (Fertig stellen).

    ![Verknüpfungstyp „Matching rows“ (Übereinstimmende Zeilen)](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Durch diesen Vorgang wird ein neuer Datenfluss mit dem Namen __Join Result__ erstellt.

## <a name="create-additional-features"></a>Erstellen zusätzlicher Funktionen

1. Klicken Sie zum Erstellen einer Spalte mit dem Wochentag mit der rechten Maustaste auf die Spalte **Date Hour Range**, und wählen Sie die Option **Derive column by Example** (Spalte nach Beispiel ableiten) aus. Geben Sie den Wert __Sun__ für ein Datum aus, das auf einen Sonntag fällt. Beispiel: **Jan 01, 2017 12AM-2AM**. Drücken Sie die **EINGABETASTE**, und wählen Sie **OK** aus. Benennen Sie diese Spalte in __Weekday__ um.

    ![Abbildung: Erstellen einer neuen Spalte mit dem Wochentag](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Klicken Sie zum Erstellen einer Spalte mit dem Zeitraum für eine Zeile mit der rechten Maustaste auf die Spalte **Date Hour Range**, und wählen Sie die Option **Derive column by example** (Spalte nach Beispiel ableiten) aus. Verwenden Sie den Wert **12AM-2AM** für die Zeile mit dem Inhalt **Jan 01, 2017 12AM-2AM**. Drücken Sie die **EINGABETASTE**, und wählen Sie **OK** aus. Benennen Sie diese Spalte in **Period** um.

    ![Abbildung der Spalte „Period“](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Drücken Sie zum Entfernen der Spalten **Date Hour Range** und **Date Hour Range** die **STRG-TASTE**, und wählen Sie die beiden Spaltenüberschriften aus. Klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Remove column** (Spalte entfernen) aus.

## <a name="read-data-from-python"></a>Auslesen der Daten aus Python

Sie können ein Datenvorbereitungspaket aus Python oder PySpark ausführen und das Ergebnis als **Datenrahmen** abrufen.

Klicken Sie zum Generieren eines Python-Beispielskripts mit der rechten Maustaste auf __BikeShare Data Prep__, und wählen Sie die Option __Generate Data Access Code File__ (Datenzugriffs-Codedatei generieren) aus. Die Python-Beispieldatei wird in Ihrem **Projektordner** erstellt und außerdem auf einer Registerkarte in der Workbench geladen. Das folgende Python-Skript zeigt ein Beispiel für den generierten Code:

```python
# Use the Azure Machine Learning data preparation package
from azureml.dataprep import package

# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# This call will load the referenced package and return a DataFrame.
# If run in a PySpark environment, this call returns a
# Spark DataFrame. If not, it will return a Pandas DataFrame.
df = package.run('BikeShare Data Prep.dprep', dataflow_idx=0)

# Remove this line and add code that uses the DataFrame
df.head(10)
```

Für dieses Tutorial lautet der Name der Datei `BikeShare Data Prep.py`. Diese Datei wird später im Tutorial verwendet.

## <a name="save-test-data-as-a-csv-file"></a>Speichern von Testdaten als CSV-Datei

Zum Speichern des Datenflusses **Join Result** in einer CSV-Datei müssen Sie das Skript `BikeShare Data Prep.py` ändern. Aktualisieren Sie das Python-Skript mit dem folgenden Code:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Wählen Sie am oberen Bildschirmrand das Symbol **Ausführen** aus. Das Skript wird auf dem lokalen Computer als **Auftrag** übermittelt. Sobald der Auftragsstatus zu __Completed__ (Abgeschlossen) wechselt, wurde die Datei an den angegebenen Speicherort geschrieben.

## <a name="substitute-data-sources"></a>Ersetzen von Datenquellen

In den vorherigen Schritten haben Sie mit den Datenquellen `201701-hubway-tripdata.csv` und `BostonWeather.csv` die Testdaten vorbereitet. Führen Sie die folgenden Schritte aus, um das Paket mit anderen Fahrtdaten-Dateien zu verwenden:

1. Erstellen Sie eine neue **Datenquelle**, indem Sie die oben erläuterten Schritte ausführen; nehmen Sie dabei am Vorgang die folgenden Änderungen vor:

    * __File Selection__ (Dateiauswahl): Wählen Sie gleichzeitig die sechs verbleibenden CSV-Dateien mit Fahrtdaten aus.

        ![Laden der sechs verbleibenden Dateien](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > Der Eintrag __+5__ zeigt an, dass neben der aufgelisteten Datei noch fünf weitere Dateien vorhanden sind.

    * __File Details__ (Dateidetails): Legen Sie __Promote Headers Mode__ (Modus „Header heraufstufen“) auf **All Files Have The Same Headers** (Alle Dateien weisen den gleichen Header auf) fest. Mit diesem Wert wird angegeben, dass alle Dateien den gleichen Header aufweisen.

        ![Ausgewählte Dateidetails](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Speichern Sie den Namen dieser Datenquelle, da sie in späteren Schritten verwendet wird.

2. Wählen Sie das Ordnersymbol aus, um die Dateien im Projekt anzuzeigen. Erweitern Sie das Verzeichnis __aml\_config__, und wählen Sie die Datei `local.runconfig` aus.

    ![Abbildung des Speicherorts von „local.runconfig“](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. Fügen Sie am Ende der Datei `local.runconfig` die folgenden Zeilen hinzu, und klicken Sie anschließend auf das Datenträgersymbol, um die Datei zu speichern.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Mit dieser Änderung wird die ursprüngliche Datenquelle durch die Datenquelle mit den sechs Fahrtdaten-Dateien ersetzt.

## <a name="save-training-data-as-a-csv-file"></a>Speichern der Trainingsdaten als CSV-Datei

Navigieren Sie zu der zuvor bearbeiteten Python-Datei `BikeShare Data Prep.py`, und geben Sie einen anderen Dateipfad zum Speichern der Trainingsdaten an.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Wählen Sie zum Senden eines neuen Auftrags das Symbol **Ausführen** am oberen Rand der Seite aus. Ein **Auftrag** mit der neuen Konfiguration wird gesendet. Die Ausgabe dieses Auftrags sind die Trainingsdaten. Diese Daten werden anhand derselben Datenvorbereitungsschritte wie oben erstellt. Das Abschließen des Auftrags kann einige Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte
Sie haben das Tutorial zum Vorbereiten von Bikesharing-Daten durchgearbeitet. In diesem Tutorial haben Sie Azure Machine Learning-Dienste (Vorschauversion) verwendet, um Folgendes zu lernen:
> [!div class="checklist"]
> * Interaktives Vorbereiten von Daten mit dem Azure Machine Learning-Datenvorbereitungstool
> * Importieren, Transformieren und Erstellen eines Testdatasets
> * Generieren eines Datenvorbereitungspakets
> * Ausführen des Datenvorbereitungspakets mit Python
> * Generieren eines Trainingsdatasets durch Wiederverwenden des Datenvorbereitungspakets für weitere Eingabedateien

Nun erfahren Sie mehr über die Datenvorbereitung:
> [!div class="nextstepaction"]
> [Anleitung zur Datenvorbereitung](data-prep-user-guide.md)

