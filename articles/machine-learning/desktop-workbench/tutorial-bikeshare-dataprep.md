---
title: Bikesharing-Tutorial – Erweiterte Datenvorbereitung mit der Azure Machine Learning Workbench
description: In diesem Tutorial führen Sie mithilfe von Azure Machine Learning Workbench eine vollständige Vorbereitungsaufgabe durch.
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: 951ce8947d113eaad2ea0e3b5df5e9714aa33dd8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723186"
---
# <a name="tutorial-use-azure-machine-learning-workbench-for-advanced-data-preparation-bike-share-data"></a>Tutorial: Erweiterte Datenvorbereitung (Bikesharing-Daten) mit Azure Machine Learning Workbench
Azure Machine Learning (Vorschauversion) ist eine integrierte End-to-End-Lösung für Data Science-Anwendungen und erweiterte Analysen, die sich an professionelle Datenspezialisten richtet und zum Vorbereiten von Daten, Entwickeln von Experimenten und Bereitstellen von Modellen auf Cloudniveau verwendet werden kann.

In diesem Tutorial verwenden Sie Machine Learning (Vorschauversion), um Folgendes zu lernen:
> [!div class="checklist"]
> * Interaktives Vorbereiten von Daten mit dem Machine Learning-Datenvorbereitungstool
> * Importieren, Transformieren und Erstellen eines Testdatasets
> * Generieren eines Datenvorbereitungspakets
> * Ausführen des Datenvorbereitungspakets mit Python
> * Generieren eines Trainingsdatasets durch Wiederverwenden des Datenvorbereitungspakets für weitere Eingabedateien
> * Ausführen von Skripts in einem lokalen Azure CLI-Fenster
> * Ausführen von Skripts in einer Azure HDInsight-Cloudumgebung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine lokale Installation von Azure Machine Learning Workbench. Weitere Informationen finden Sie in der [Schnellstartanleitung für die Installation](../service/quickstart-installation.md).
* Sollte die Azure-Befehlszeilenschnittstelle bei Ihnen nicht installiert sein, führen Sie die Schritte unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) aus.
* Ein in Azure erstellter [HDInsight Spark-Cluster](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).
* Ein Azure-Speicherkonto.
* Erfahrung mit dem Erstellen eines neuen Projekts in Workbench.
* Die Installation von [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) ist zwar nicht erforderlich, erleichtert jedoch das Hochladen, Herunterladen und Anzeigen der Blobs im Speicherkonto.

## <a name="data-acquisition"></a>Datenerfassung
In diesem Tutorial werden das [Boston Hubway-Dataset](https://s3.amazonaws.com/hubway-data/index.html) und Boston-Wetterdaten von [NOAA](http://www.noaa.gov/) verwendet.

1. Laden Sie die Datendateien über die folgenden Links in Ihre lokale Entwicklungsumgebung herunter:

   * [Boston-Wetterdaten](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv)

   * Hubway-Fahrtdaten von der Hubway-Website:

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Entpacken Sie jede heruntergeladene ZIP-Datei.

## <a name="upload-data-files-to-azure-blob-storage"></a>Hochladen von Datendateien in Azure Blob Storage
Die Datendateien können mit Azure Blob Storage gehostet werden.

1. Verwenden Sie das Speicherkonto, das Sie auch für Ihren HDInsight-Cluster verwenden.

    ![Speicherkonto des HDInsight-Clusters](media/tutorial-bikeshare-dataprep/hdinsightstorageaccount.png)

2. Erstellen Sie zum Speichern der **BikeShare**-Datendateien einen neuen Container namens **data-files**.

3. Laden Sie die Datendateien hoch. Laden Sie `BostonWeather.csv` in einen Ordner namens `weather` hoch. Laden Sie die Fahrtdatendateien in einen Ordner namens `tripdata` hoch.

    ![Hochladen von Datendateien](media/tutorial-bikeshare-dataprep/azurestoragedatafile.png)

> [!TIP]
> Blobs können auch über den Storage-Explorer hochgeladen werden. Verwenden Sie dieses Tool, wenn Sie auch den Inhalt der im Tutorial generierten Dateien anzeigen möchten.

## <a name="learn-about-the-datasets"></a>Weitere Informationen zu den Datasets
1. Die Datei __Boston weather__ enthält die folgenden wetterbezogenen Felder mit stündlich gemeldeten Werten:

   * **DATE**

   * **REPORTTPYE**

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

2. Die __hubway__-Daten sind in Dateien nach Jahr und Monat strukturiert. Die Datei `201501-hubway-tripdata.zip` enthält beispielsweise eine CSV-Datei mit Daten für Januar 2015. Die Daten enthalten die folgenden Felder, wobei jede Zeile eine Fahrradstrecke darstellt:

   * **Trip Duration (in seconds)** (Dauer der Fahrt (in Sekunden))

   * **Start Time and Date** (Startzeit und -datum)

   * **Stop Time and Date** (Endzeit und -datum)

   * **Start Station Name & ID** (Name und ID der Startstation)

   * **End Station Name & ID** (Name und ID der Endstation)

   * **Bike ID** (Rad-ID)

   * **User Type (Casual = 24-Hour or 72-Hour Pass user; Member = Annual or Monthly Member)** (Benutzertyp – Gelegentlich: Benutzer mit 24-Stunden- oder 72-Stunden-Pass; Mitglied: Jahres- oder Monatsmitglied)

   * **ZIP Code** (Postleitzahl) – sofern der Benutzer Mitglied ist

   * **Gender** (Geschlecht) – Selbstauskunft des Mitglieds

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts
1. Starten Sie **Machine Learning Workbench** über das Startmenü oder mithilfe des Startprogramms.

2. Erstellen Sie ein neues Machine Learning-Projekt. Klicken Sie auf der Seite **Projekte** auf die Schaltfläche **+**, oder klicken Sie auf **Datei** > **Neu**.

   * Verwenden Sie die **Bike Share**-Vorlage.

   * Benennen Sie das Projekt **BikeShare**. 

## <a id="newdatasource"></a>Erstellen einer neuen Datenquelle

1. Erstellen Sie eine neue Datenquelle. Klicken Sie auf der linken Symbolleiste auf die Schaltfläche **Daten** (Zylindersymbol), um die **Datenansicht** anzuzeigen.

   ![Registerkarte mit der Datenansicht](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Fügen Sie eine Datenquelle hinzu. Klicken Sie auf das Symbol **+** und anschließend auf **Datenquelle hinzufügen**.

   ![Option „Datenquelle hinzufügen“](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Hinzufügen der Wetterdaten

1. **Datenspeicher**: Wählen Sie den Datenspeicher aus, der die Daten enthält. Da Sie Dateien verwenden, wählen Sie **File(s)/Directory** (Datei(en)/Verzeichnis) aus. Klicken Sie auf **Weiter**, um fortzufahren.

   ![Eintrag „File(s)/Directory“ (Datei(en)/Verzeichnis)](media/tutorial-bikeshare-dataprep/datasources.png)

2. **File Selection** (Dateiauswahl): Fügen Sie die Wetterdaten hinzu. Navigieren Sie zur Datei `BostonWeather.csv`, die Sie zuvor in Blob Storage hochgeladen haben, und wählen Sie sie aus. Klicken Sie auf **Weiter**.

   ![Dateiauswahl mit ausgewählter Datei „BostonWeather.csv“](media/tutorial-bikeshare-dataprep/azureblobpickweatherdatafile.png)

3. **File Details** (Dateidetails): Überprüfen Sie das erkannte Dateischema. Machine Learning Workbench analysiert die Dateien in der Datei und leitet das zu verwendende Schema ab.

   ![Überprüfen der Dateidetails](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Manchmal erkennt Workbench unter Umständen nicht das richtige Schema. Vergewissern Sie sich stets, dass die Parameter für das jeweilige Dataset korrekt sind. Stellen Sie für die Wetterdaten sicher, dass sie auf die folgenden Werte festgelegt sind:
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

4. **Datentypen**: Überprüfen Sie die Datentypen, die automatisch erkannt werden. Machine Learning Workbench analysiert die Dateien in der Datei und leitet die zu verwendenden Datentypen ab.

   a. Ändern Sie für diese Daten den **DATENTYP** für alle Spalten in **Zeichenfolge**.

   > [!NOTE]
   > „Zeichenfolge“ wird verwendet, um im weiteren Verlauf dieses Tutorials die Funktionen von Workbench hervorzuheben. 

   ![Überprüfen der Datentypen](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   b. Klicken Sie auf __Weiter__, um fortzufahren. 

5. **Stichprobenentnahme** (Stichproben): Klicken Sie zum Erstellen eines Stichprobenschemas auf **Edit** (Bearbeiten). Wählen Sie die hinzugefügte neue Zeile __Top 10000__ aus, und wählen Sie dann __Edit__ (Bearbeiten) aus. Legen Sie die __Sample Strategy__ (Stichprobenstrategie) auf **Full File** (Vollständige Datei) fest, und wählen Sie dann **Anwenden** aus.

   ![Hinzufügen einer neuen Stichprobenstrategie](media/tutorial-bikeshare-dataprep/weatherdatasamplingfullfile.png)

   Wählen Sie zum Verwenden der Strategie __Full File__ (Vollständige Datei) den Eintrag __Full File__ (Vollständige Datei) und anschließend __Set as Active__ (Als aktiv festlegen) aus. Neben __Full File__ (Vollständige Datei) wird ein Stern angezeigt, um die Strategie als aktive Strategie zu kennzeichnen.

   ![„Full File“ (Vollständige Datei) als aktive Strategie](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Klicken Sie auf **Weiter**, um fortzufahren.

6. **Path Column** (Pfad-Spalte): Verwenden Sie den Abschnitt __Path Column__ (Pfad-Spalte), um den vollständigen Dateipfad als Spalte in die importierten Daten einzuschließen. Wählen Sie __Do Not Include Path Column__ (Pfad-Spalte nicht einschließen).

   > [!TIP]
   > Das Einschließen des Pfads als Spalte ist hilfreich, wenn Sie einen Ordner mit vielen Dateien mit unterschiedlichen Dateinamen importieren. Es empfiehlt sich auch, wenn die Dateinamen Informationen enthalten, die Sie später extrahieren möchten.

   ![Einstellung, dass „Path Column“ (Pfad-Spalte) nicht eingeschlossen werden soll](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Finish** (Fertig stellen): Klicken Sie zum Fertigstellen der Datenquelle auf **Finish** (Fertig stellen).

    Eine Registerkarte für eine neue Datenquelle mit dem Namen __BostonWeather__ wird geöffnet. Eine Stichprobe der Daten wird in einer Rasteransicht angezeigt. Die Stichprobe basiert auf dem zuvor angegebenen aktiven Stichprobenschema.

    Beachten Sie, dass im Bereich **Steps** (Schritte) rechts im Bildschirm die einzelnen Aktionen angezeigt werden, die beim Erstellen dieser Datenquelle ausgeführt wurden.

   ![Anzeigen von Datenquelle, Stichprobe und Schritten](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Anzeigen der Datenquellenmetriken

Klicken Sie links oben in der Rasteransicht der Registerkarte auf __Metrics__ (Metriken). In dieser Ansicht werden die Verteilung sowie andere aggregierte Statistiken der Stichprobendaten angezeigt.

![Anzeigen von Metriken](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Verwenden Sie die Dropdownliste **Choose Metric** (Metrik auswählen), um die Sichtbarkeit der Statistik zu konfigurieren. Aktivieren und deaktivieren Sie Metriken, um die Rasteransicht zu ändern.

Wenn Sie zur __Datenansicht__ zurückkehren möchten, klicken Sie links oben auf der Seite auf __Data__ (Daten).

## <a name="add-a-data-source-to-the-data-preparation-package"></a>Hinzufügen einer Datenquelle zum Datenvorbereitungspaket

1. Wählen Sie __Prepare__ (Vorbereiten) aus, um mit dem Vorbereiten der Daten zu beginnen. 

2. Geben Sie bei entsprechender Aufforderung einen Namen für das Datenvorbereitungspaket ein (beispielsweise **BikeShare Data Prep**). 

3. Klicken Sie auf __OK__ , um fortzufahren.

   ![Dialogfeld „Prepare“ (Vorbereiten)](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Ein neues Paket namens **BikeShare Data Prep** wird im Abschnitt __Data Preparation__ (Datenvorbereitung) der Registerkarte __Data__ (Daten) angezeigt. 

   Wählen Sie diesen Eintrag aus, um das Paket anzuzeigen. 

5. Klicken Sie auf die Schaltfläche **>>**, um __Dataflows__ (Datenflüsse) zu erweitern und die im Paket enthaltenen Datenflüsse einzublenden. In diesem Beispiel ist __BostonWeather__ der einzige Datenfluss.

   > [!IMPORTANT]
   > Ein Paket kann mehrere Datenflüsse enthalten.

   ![Datenflüsse im Paket](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtern von Daten nach Wert
1. Klicken Sie zum Filtern der Daten mit der rechten Maustaste auf eine Zelle mit einem bestimmten Wert, und wählen Sie __Filter__ aus. Wählen Sie dann die Art des Filters aus.

2. Wählen Sie für dieses Tutorial eine Zelle mit dem Wert `FM-15` aus. Legen Sie den Filter auf **equals** (Ist gleich) fest.  Nun werden die Daten so gefiltert, dass nur Zeilen zurückgegeben werden, bei denen der __REPORTTYPE__ gleich `FM-15` ist.

   ![Filterdialogfeld](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 ist eine Art von METAR-Wetterbericht (Meteorological Terminal Aviation Routine). Die FM-15-Berichte sind empirisch betrachtet die vollständigsten Daten, bei denen am wenigsten Daten fehlen.

## <a name="remove-a-column"></a>Entfernen einer Spalte

Die Spalte __REPORTTYPE__ wird nicht mehr benötigt. Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift, und wählen Sie **Remove Column** (Spalte entfernen) aus.

   ![Option „Remove Column“ (Spalte entfernen)](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Ändern von Datentypen und Entfernen von Fehlern
1. Drücken Sie auf STRG (⌘ auf einem Mac), während Sie die Spaltenüberschriften zum Auswählen mehrerer Spalten gleichzeitig auswählen. Wählen Sie so die folgenden Spaltenüberschriften aus:

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

2. Klicken Sie mit der rechten Maustaste auf eine der ausgewählten Spaltenüberschriften, und wählen Sie die Option **Convert Field Type to Numeric** (Feldtyp in Numerisch konvertieren) aus. Dadurch wird der Datentyp der Spalten in „Numerisch“ geändert.

   ![Konvertieren mehrerer Spalten in „Numerisch“](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filtern Sie die Fehlerwerte heraus. Bei einigen Spalten treten Probleme beim Konvertieren des Datentyps auf. Dieses Problem wird durch die rote Hervorhebung der __Datenqualitätsleiste__ für die Spalte angezeigt.

   Klicken Sie zum Entfernen der fehlerhaften Spalten mit der rechten Maustaste auf die Spaltenüberschrift **HOURLYDRYBULBTEMPF**. Wählen Sie **Filter Column** (Spalte filtern) aus. Übernehmen Sie für **I Want To** (Ich möchte) den Standardwert **Keep Rows** (Zeilen beibehalten). Ändern Sie die Auswahl in der Dropdownliste **Conditions** (Bedingungen) in **is not error** (Ist kein Fehler). Klicken Sie auf **OK**, um den Filter anzuwenden.

   ![Filtern nach Fehlerwerten](media/tutorial-bikeshare-dataprep/filtererrorvalues.png)

4. Wiederholen Sie den Filtervorgang für die Spalten **HOURLYRelativeHumidity** und **HOURLYWindSpeed**, um die verbleibenden fehlerhaften Zeilen in den anderen Spalten zu entfernen.

## <a name="use-by-example-transformations"></a>Verwenden von Transformationen nach Beispiel

Zum Verwenden der Daten in einer Prognose für Zwei-Stunden-Zeitblöcke müssen Sie die durchschnittlichen Wetterbedingungen für Zwei-Stunden-Zeiträume berechnen. Führen Sie folgende Aktionen durch:

* Teilen Sie die Spalte **DATE** in separate Spalten **Date** und **Time**. Die ausführlichen Schritte hierzu finden Sie im folgenden Abschnitt.

* Leiten Sie eine Spalte **Hour_Range** von der Spalte **Time** ab. Die ausführlichen Schritte hierzu finden Sie im folgenden Abschnitt.

* Leiten Sie eine Spalte **Date\_Hour\_Range** von den Spalten **DATE** und **Hour_Range** ab. Die ausführlichen Schritte hierzu finden Sie im folgenden Abschnitt.

### <a name="split-column-by-example"></a>Split column by example (Spalte nach Beispiel teilen)

1. Teilen Sie die Spalte **DATE** in separate Spalten **Date** und **Time**. Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift **DATE**, und wählen Sie die Option **Split Column by Example** (Spalte nach Beispiel teilen) aus.

   ![Eintrag „Split Column by Example“ (Spalte nach Beispiel teilen)](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Machine Learning Workbench bestimmt automatisch ein sinnvolles Trennzeichen und erstellt zwei Spalten, indem die Daten in Datums- und Uhrzeitwerte geteilt werden. 

3. Wählen Sie __OK__ aus, um die Ergebnisse des Teilungsvorgangs zu akzeptieren.

   ![Geteilte Spalten „DATE_1“ und „DATE_2“](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Derive column by example (Spalte nach Beispiel ableiten)

1. Klicken Sie zum Ableiten eines Zwei-Stunden-Zeitraums mit der rechten Maustaste auf die Spaltenüberschrift __DATE\_2__, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus.

   ![Eintrag „Derive Column by Example“ (Spalte nach Beispiel ableiten)](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Eine neue leere Spalte mit NULL-Werten wird hinzugefügt.

2. Klicken Sie auf die erste leere Zelle in der neuen Spalte. Geben Sie ein Beispiel für den gewünschten Zeitraum an, indem Sie in der neuen Spalte **12AM-2AM** eingeben und die EINGABETASTE drücken.

   ![Neue Spalte mit dem Wert „12AM-2AM“](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Machine Learning Workbench erzeugt ein Programm auf der Grundlage der von Ihnen angegebenen Beispiele und wendet dieses Programm auf die restlichen Zeilen an. Alle übrigen Zeilen werden gemäß dem angegebenen Beispiel automatisch aufgefüllt. Die Workbench analysiert zudem die Daten und versucht, Grenzfälle zu ermitteln. 

   > [!IMPORTANT]
   > Auf einem Mac werden Grenzfälle in der aktuellen Workbench-Version möglicherweise nicht ordnungsgemäß erkannt. Überspringen Sie bei Verwendung eines Macs die Schritte 3 und 4. Klicken Sie stattdessen auf __OK__, nachdem alle Zeilen mit den abgeleiteten Werten aufgefüllt wurden.
   
3. Der Text **Analyzing Data** (Daten werden analysiert) über dem Raster zeigt an, dass die Workbench versucht, Grenzfälle zu ermitteln. Nach Abschluss des Vorgangs wechselt der Status zu **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) oder **No suggestions** (Keine Vorschläge). In diesem Beispiel wird **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) zurückgegeben.

4. Wählen Sie zum Prüfen der vorgeschlagenen Änderungen die Option **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) aus. Die zu prüfende und ggf. zu korrigierende Zelle wird in der Anzeige hervorgehoben.

   ![Review next suggested row (Nächste vorgeschlagene Zeile prüfen)](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Wählen Sie __OK__ aus, um die Transformation zu akzeptieren.
 
5. Sie kehren zur Rasteransicht der Daten für __BostonWeather__ zurück. Das Raster enthält nun die drei kürzlich hinzugefügten Spalten.

   ![Rasteransicht mit hinzugefügten Zeilen](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   > Alle vorgenommenen Änderungen werden im Bereich **Steps** (Schritte) dokumentiert. Navigieren Sie im Bereich **Steps** (Schritte) zu dem Schritt, den Sie erstellt haben, klicken Sie auf den Abwärtspfeil, und klicken Sie auf **Edit** (Bearbeiten). Das erweiterte Fenster für **Derive Column by Example** (Spalte nach Beispiel ableiten) wird angezeigt. Alle Ihre Beispiele sind hier aufgeführt. Sie können Beispiele auch manuell hinzufügen, indem Sie im folgenden Raster auf eine Zeile doppelklicken. Wählen Sie **Cancel** (Abbrechen) aus, um zum Hauptraster zurückzukehren, ohne dass die Änderungen angewendet werden. Sie können diese Ansicht auch aufrufen, indem Sie auf **Advanced mode** (Erweiterter Modus) klicken, während Sie eine Transformation vom Typ **Derive Column by Example** (Spalte nach Beispiel ableiten) ausführen.

6. Doppelklicken Sie zum Umbenennen der Spalte auf die Spaltenüberschrift, und geben Sie **Hour Range** ein. Drücken Sie die EINGABETASTE, um die Änderung zu speichern.

   ![Umbenennen der Spalte](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Wählen Sie zum Ableiten des Datums- und Stundenbereichs gleichzeitig die Spalten **Date\_1** und **Hour Range** aus, klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus.

   ![Derive Column by Example (Spalte nach Beispiel ableiten)](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Geben Sie **Jan 01, 2015 12AM-2AM** als Beispiel für die erste Zeile ein, und drücken Sie die EINGABETASTE.

   Workbench bestimmt die Transformation anhand des angegebenen Beispiels. In diesem Beispiel erhalten wir folgendes Ergebnis: Das Datumsformat wird geändert und mit dem Zwei-Stunden-Fenster verkettet.

   ![Beispiel „Jan 01, 2015 12AM-2AM“](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Führen Sie auf einem Mac anstelle von Schritt 8 den folgenden Schritt aus:
   >
   > * Navigieren Sie zur ersten Zeile, die **Feb 01, 2015 12AM-2AM** enthält. Das müsste Zeile 15 sein. Ändern Sie den Wert in **Jan 02, 2015 12AM-2AM**, und drücken Sie die EINGABETASTE. 
   

8. Warten Sie, bis der Status von **Analyzing Data** (Daten werden analysiert) zu **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) wechselt. Diese Änderung kann einige Sekunden dauern. Klicken Sie auf den Statuslink, um zur vorgeschlagenen Zeile zu gelangen. 

   ![Zur Prüfung vorgeschlagenen Zeile](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Die Zeile enthält einen NULL-Wert, da der Quelldatenwert sich entweder auf dd/mm/yyyy oder auf mm/dd/yyyy beziehen kann. Geben Sie den korrekten Wert (**Jan 13, 2015 2AM-4AM**) ein, und drücken Sie die EINGABETASTE. Workbench verbessert anhand der beiden Beispiele die Ableitung für die übrigen Zeilen.

   ![Ordnungsgemäß formatierte Daten](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Wählen Sie **OK** aus, um die Transformation zu akzeptieren.

   ![Abgeschlossenes Transformationsraster](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Klicken Sie zum Verwenden des **erweiterten Modus** von **Derive Column by Example** (Spalte nach Beispiel ableiten) für diesen Schritt im Bereich **Steps** (Schritte) auf den Abwärtspfeil. Im Datenraster befinden sich Kontrollkästchen neben den Spalten **DATE\_1** und **Hour Range**. Deaktivieren Sie das Kontrollkästchen neben der Spalte **Hour Range**, und beobachten Sie, wie sich die Ausgabe verändert. Wenn die Spalte **Hour Range** nicht mehr als Eingabe vorhanden ist, wird **12AM-2AM** als Konstante behandelt und an die abgeleiteten Werte angefügt. Wählen Sie **Cancel** (Abbrechen) aus, um zum Hauptraster zurückzukehren, ohne dass die vorgenommenen Änderungen angewendet werden.
   ![Erweiterter Modus](media/tutorial-bikeshare-dataprep/derivedcolumnadvancededitdeselectcolumn.png)

10. Doppelklicken Sie auf die Spaltenüberschrift, um die Spalte umzubenennen. Ändern Sie den Namen in **Date Hour Range**, und drücken Sie anschließend die EINGABETASTE.

11. Wählen Sie gleichzeitig die Spalten **DATE**, **DATE\_1**, **DATE\_2** und **Hour Range** aus. Klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Remove column** (Spalte entfernen) aus.

## <a name="summarize-data-mean"></a>Zusammenfassen von Daten (Mittelwert)

Im nächsten Schritt werden die Wetterbedingungen zusammengefasst, indem der Mittelwert der Werte berechnet wird, gruppiert nach Stundenbereich.

1. Wählen Sie die Spalte **Date Hour Range** aus, und wählen Sie anschließend im Menü **Transforms** (Transformationen) die Option **Summarize** (Zusammenfassen) aus.

    ![Menü „Transforms“ (Transformationen)](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Ziehen Sie Spalten aus dem Raster am unteren Seitenrand in den linken und rechten Bereich am oberen Rand, um die Daten zusammenzufassen. Im linken Bereich befindet sich der Text **Drag columns here to group data** (Spalten hierher ziehen, um Daten zu gruppieren). Im rechten Bereich befindet sich der Text **Drag columns here to summarize data** (Spalten hierher ziehen, um Daten zusammenzufassen). 

    a. Ziehen Sie die Spalte **Date Hour Range** aus dem Raster unten in den linken Bereich. Ziehen Sie die Spalten **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** und **HOURLYWindSpeed** in den rechten Bereich. 

    b. Wählen Sie im rechten Bereich **Mean** (Mittelwert) als Measure **Aggregate** (Aggregat) für jede Spalte aus. Klicken Sie auf **OK**, um die Zusammenfassung abzuschließen.

    ![Bildschirm mit zusammengefassten Daten](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-by-using-script"></a>Transformieren des Datenflusses mithilfe eines Skripts

Durch Festlegen der Daten in den numerischen Spalten auf einen Bereich von 0 bis 1 können einige Modelle schnell konvergiert werden. Zur generischen Durchführung dieser Transformation steht derzeit keine integrierte Transformation zur Verfügung. Verwenden Sie daher für diesen Vorgang ein Python-Skript.

1. Wählen Sie im Menü **Transforms** (Transformationen) die Option **Transform Dataflow (Script)** (Datenfluss transformieren (Skript)) aus.

2. Geben Sie im angezeigten Textfeld den folgenden Code ein. Wenn Sie die Spaltennamen verwendet haben, sollte der Code ohne weitere Anpassungen funktionieren. Sie schreiben eine einfache Min-Max-Normalisierungslogik in Python.

    > [!WARNING]
    > Das Skript erwartet die bisher in diesem Tutorial verwendeten Spaltennamen. Sollten Sie andere Spaltennamen angegeben haben, müssen Sie die Namen im Skript ändern.

   ```python
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
    
   ![Dialogfeld „Transform Dataflow (Script)“ (Datenfluss transformieren (Skript))](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Klicken Sie auf __OK__, um das Skript zu verwenden. Die numerischen Spalten im Raster enthalten nun Werte im Bereich von 0 bis 1.

    ![Raster mit Werten zwischen 0 und 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Sie haben die Vorbereitung der Wetterdaten abgeschlossen. Als Nächstes bereiten Sie die Fahrtdaten vor.

## <a name="load-trip-data"></a>Laden der Fahrtdaten

1. Führen Sie zum Importieren der Datei `201701-hubway-tripdata.csv` die Schritte im Abschnitt [Erstellen einer neuen Datenquelle](#newdatasource) aus. Verwenden Sie während des Importvorgangs die folgenden Optionen:

    * __File Selection__ (Dateiauswahl): Wählen Sie **Azure Blob** aus, um die Datei auszuwählen.

    * __Sampling scheme__ (Stichprobenschema): Wählen Sie das Stichprobenschema **Full File** (Vollständige Datei) aus, und aktivieren Sie die Stichprobe.

    * __Data Type__ (Datentyp): Übernehmen Sie die Standardwerte.

2. Klicken Sie nach dem Importieren der Daten auf __Prepare__ (Vorbereiten), um mit der Vorbereitung der Daten zu beginnen. Wählen Sie das vorhandene Paket **BikeShare Data Prep.dprep** aus, und klicken Sie anschließend auf __OK__.

    Durch diesen Prozess wird der vorhandenen **Data Preparation**-Datei ein **Datenfluss** hinzugefügt und kein neuer erstellt.

    ![Auswählen des vorhandenen Pakets](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Erweitern Sie nach dem Laden des Rasters __DATAFLOWS__ (DATENFLÜSSE). Nun sind zwei Datenflüsse vorhanden: **BostonWeather** und **201701-hubway-tripdata**. Wählen Sie den Eintrag **201701-hubway-tripdata** aus.

    ![Eintrag „201701-hubway-tripdata“](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-the-map-inspector"></a>Verwenden des Karteninspektors

Für die Datenvorbereitung stehen praktische Visualisierungen (so genannte Inspektoren) für Zeichenfolgendaten, numerische Daten und geografische Daten zur Verfügung. Sie verbessern die Nachvollziehbarkeit der Daten sowie die Erkennung von Ausreißern. Gehen Sie zur Verwendung des Karteninspektors wie folgt vor:

1. Wählen Sie die Spalten **start station latitude** und **start station longitude** gleichzeitig aus. Klicken Sie mit der rechten Maustaste auf eine der Spalten, und wählen Sie die Option **Map** (Karte) aus.

    > [!TIP]
    > Halten Sie für die Mehrfachauswahl STRG gedrückt (⌘ auf einem Mac), und wählen Sie die Überschrift für jede Spalte aus.

    ![Kartenvisualisierung](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Klicken Sie zum Maximieren der Kartenvisualisierung auf das Symbol **Maximieren**. Klicken Sie links oben in der Visualisierung auf das Symbol **E**, um die Karte an die Fenstergröße anzupassen.

    ![Maximiertes Bild](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Klicken Sie auf die Schaltfläche **Minimieren**, um zur Rasteransicht zurückzukehren.

## <a name="use-the-column-statistics-inspector"></a>Verwenden des Spaltenstatistik-Inspektors

Klicken Sie zum Verwenden des Spaltenstatistik-Inspektors mit der rechten Maustaste auf die Spalte __tripduration__, und wählen Sie die Option __Column Statistics__ (Spaltenstatistiken) aus.

![Eintrag „Column Statistics“ (Spaltenstatistiken)](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Durch diesen Vorgang wird die neue Visualisierung __tripduration Statistics__ im Bereich __INSPECTORS__ (INSPEKTOREN) hinzugefügt.

![Statistikinspektor für „tripduration“](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Der maximale Wert der Fahrtdauer beträgt 961.814 Minuten (ca. zwei Jahre). Anscheinend sind im Dataset einige Ausreißer enthalten.

## <a name="use-the-histogram-inspector"></a>Verwenden des Histogramm-Inspektors

Klicken Sie zum Ermitteln von Ausreißern mit der rechten Maustaste auf die Spalte __tripduration__, und wählen Sie die Option __Histogram__ (Histogramm) aus.

![Histogramm-Inspektor](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Das Histogramm ist nicht hilfreich, da die Ausreißer das Diagramm verzerren.

## <a name="add-a-column-by-using-script"></a>Hinzufügen einer Spalte mithilfe eines Skripts

1. Klicken Sie mit der rechten Maustaste auf die Spalte **tripduration**, und wählen Sie die Option **Add Column (Script)** (Spalte hinzufügen (Skript)) aus.

    ![Menü „Add Column (Script)“ (Spalte hinzufügen (Skript))](media/tutorial-bikeshare-dataprep/computecolscript.png)

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

## <a name="use-an-advanced-filter"></a>Verwenden eines erweiterten Filters

Durch Anwenden eines Filters auf die Daten werden die Inspektoren mit der neuen Verteilung aktualisiert. 

1. Klicken Sie mit der rechten Maustaste auf die Spalte **logtripduration**, und wählen Sie die Option **Filter Column** (Spalte filtern) aus. 

2. Geben Sie im Dialogfeld __Edit__ (Bearbeiten) die folgenden Werte an:

    * __Filter this Number Column__ (Diese Zahlenspalte filtern): logtripduration

    * __I Want To__ (Ich möchte): Keep Rows (Zeilen beibehalten)

    * __When__ (Wenn): Any of the Conditions below are True (logical OR) (Eine der Bedingungen unten True ist (logisches OR))

    * __If this Column__ (Wenn diese Spalte): less than (kleiner als)

    * __The Value__ (Wert): 9

    ![Filteroptionen](media/tutorial-bikeshare-dataprep/loftripfilter.png)

3. Klicken Sie auf __OK__, um den Filter anzuwenden.

    ![Aktualisierte Histogramme nach dem Anwenden des Filters](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Der Halo-Effekt

1. Maximieren Sie das Histogramm **logtripduration**. Ein blaues Histogramm wird über ein graues Histogramm gelegt. Eine solche Anzeige wird als **Halo-Effekt** bezeichnet:

    * Das graue Histogramm stellt die Verteilung vor dem Vorgang (in diesem Fall dem Filtervorgang) dar.

    * Das blaue Histogramm stellt das Histogramm nach dem Vorgang dar. 

   Mit dem Halo-Effekt wird die Auswirkung eines Vorgangs auf die Daten veranschaulicht.

   ![Halo-Effekt](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Das blaue Histogramm ist anscheinend kürzer als das vorherige. Diese Abweichung ist auf die automatische Neugruppierung der Daten im neuen Bereich zurückzuführen.

2. Klicken Sie zum Entfernen des Halo-Effekts auf __Edit__ (Bearbeiten), und deaktivieren Sie die Option __Show halo__ (Halo anzeigen).

    ![Optionen für das Histogramm](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Klicken Sie auf **OK**, um den Halo-Effekt zu deaktivieren. Minimieren Sie anschließend das Histogramm.

### <a name="remove-columns"></a>Entfernen von Spalten

In den Fahrtdaten stellt jede Zeile ein Fahrradabholereignis dar. Für dieses Tutorial benötigen Sie lediglich die Spalten **starttime** und **start station id**. Entfernen Sie die anderen Spalten, indem Sie diese beiden Spalten gleichzeitig auswählen, mit der rechten Maustaste auf die Spaltenüberschrift klicken und anschließend die Option **Keep Column** (Spalte beibehalten) auswählen. Die übrigen Spalten werden entfernt.

![Option „Keep Column“ (Spalte beibehalten)](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Zusammenfassen von Daten (Anzahl)

Verwenden Sie abgeleitete Spalten, um die Nachfrage nach Fahrrädern für einen Zeitraum von zwei Stunden zusammenzufassen.

1. Klicken Sie mit der rechten Maustaste auf die Spalte **starttime**, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus.

    ![Option „Derive Column by Example“ (Spalte nach Beispiel ableiten)](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Geben Sie für das Beispiel den Wert **Jan 01, 2017 12AM-2AM** für die erste Zeile ein.

    > [!IMPORTANT]
    > Im vorherigen Beispiel zum Ableiten von Spalten haben Sie mehrere Schritte ausgeführt, um eine Spalte mit dem Datums-und Uhrzeitbereich abzuleiten. In diesem Beispiel werden Sie feststellen, dass dieser Vorgang auch in einem einzigen Schritt ausgeführt werden kann, indem ein Beispiel für die Endausgabe angegeben wird.

    > [!NOTE]
    > Sie können ein Beispiel für eine beliebige Zeile angeben. In diesem Beispiel ist der Wert **Jan 01, 2017 12AM-2AM** für die erste Datenzeile gültig.

    ![Beispieldaten](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Führen Sie auf einem Mac anstelle von Schritt 3 den folgenden Schritt aus:
   >
   > * Navigieren Sie zur ersten Zeile, die **Jan 01, 2017 1AM-2AM** enthält. Das müsste Zeile 14 sein. Ändern Sie den Wert in **Jan 01, 2017 12AM-2AM**, und drücken Sie die EINGABETASTE. 

3. Warten Sie, bis die Anwendung die Werte für alle Zeilen berechnet hat. Dieser Vorgang kann einige Sekunden dauern. Klicken Sie nach Abschluss der Analyse auf den Link __Review next suggested row__ (Nächste vorgeschlagene Zeile prüfen), um die Daten zu überprüfen.

   ![Abgeschlossene Analyse mit Überprüfungslink](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Vergewissern Sie sich, dass die berechneten Werte korrekt sind. Aktualisieren Sie den Wert andernfalls mit dem erwarteten Wert, und drücken Sie die EINGABETASTE. Warten Sie anschließend, bis die Analyse abgeschlossen ist. Führen Sie den Vorgang **Review next suggested row** (Nächste vorgeschlagene Zeile prüfen) aus, bis der Status **No suggestions** (Keine Vorschläge) angezeigt wird. **No suggestions** (Keine Vorschläge) bedeutet, dass die Anwendung die Grenzfälle untersucht hat und mit dem erstellten Programm zufrieden ist. Es empfiehlt sich, eine Sichtprüfung der transformierten Daten durchzuführen, bevor Sie die Transformation akzeptieren. 

4. Wählen Sie **OK** aus, um die Transformation zu akzeptieren. Benennen Sie die neu erstellte Spalte in **Date Hour Range** um.

    ![Umbenannte Spalte](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift **starttime**, und wählen Sie die Option **Remove Column** (Spalte entfernen) aus.

6. Wählen Sie zum Zusammenfassen der Daten im Menü __Transform__ (Transformieren) die Option __Summarize__ (Zusammenfassen) aus. Führen Sie die folgenden Schritte aus, um die Transformation zu erstellen:

    * Ziehen Sie __Date Hour Range__ und __start station id__ in den Bereich **Group by** (Gruppieren nach) auf der linken Seite.

    * Ziehen Sie __start station id__ in den Bereich **summarize data** (Daten zusammenfassen) auf der rechten Seite.

   ![Zusammenfassungsoptionen](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Wählen Sie **OK** aus, um das Ergebnis der Zusammenfassung zu akzeptieren.

## <a name="join-dataflows"></a>Verknüpfen von Datenflüssen

Führen Sie die folgenden Schritte aus, um die Wetterdaten mit den Fahrtdaten zu verknüpfen:

1. Wählen Sie im Menü __Transforms__ (Transformationen) die Option __Join__ (Verknüpfen) aus.

2. __Tables__ (Tabellen): Wählen Sie **BostonWeather** als **linken** Datenfluss und **201701-hubway-tripdata** als **rechten** Datenfluss aus. Klicken Sie auf **Weiter**, um fortzufahren.

    ![Tabellenauswahl](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Key Columns__ (Schlüsselspalten): Wählen Sie die Spalte **Date Hour Range** in beiden Tabellen aus, und wählen Sie anschließend __Next__ (Weiter) aus.

    ![Schlüsselspaltenauswahl](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Join Type__ (Verknüpfungstyp): Wählen Sie als Verknüpfungstyp __Matching rows__ (Übereinstimmende Zeilen) aus, und klicken Sie dann auf __Finish__ (Fertig stellen).

    ![Verknüpfungstyp „Matching rows“ (Übereinstimmende Zeilen)](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Durch diesen Vorgang wird ein neuer Datenfluss mit dem Namen __Join Result__ erstellt.

## <a name="create-additional-features"></a>Erstellen zusätzlicher Funktionen

1. Klicken Sie zum Erstellen einer Spalte, die den Wochentag enthält, mit der rechten Maustaste auf die Spalte **Date Hour Range**, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus. Geben Sie den Wert __Sun__ für ein Datum aus, das auf einen Sonntag fällt. Beispiel: **Jan 01, 2017 12AM-2AM**. Drücken Sie die EINGABETASTE, und klicken Sie auf **OK**. Benennen Sie diese Spalte in __Weekday__ um.

    ![Erstellen einer neuen Spalte für den Wochentag](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Klicken Sie zum Erstellen einer Spalte, die den Zeitraum für eine Zeile enthält, mit der rechten Maustaste auf die Spalte **Date Hour Range**, und wählen Sie die Option **Derive Column by Example** (Spalte nach Beispiel ableiten) aus. Verwenden Sie den Wert **12AM-2AM** für die Zeile mit dem Inhalt **Jan 01, 2017 12AM-2AM**. Drücken Sie die EINGABETASTE, und klicken Sie auf **OK**. Benennen Sie diese Spalte in **Period** um.

    ![Spalte „Period“](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Drücken Sie zum Entfernen der Spalten **Date Hour Range** und **r_Date Hour Range** die STRG-TASTE (⌘ auf einem Mac), und wählen Sie die beiden Spaltenüberschriften aus. Klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Remove Column** (Spalte entfernen) aus.

## <a name="read-data-from-python"></a>Auslesen der Daten aus Python

Sie können ein Datenvorbereitungspaket aus Python oder PySpark ausführen und das Ergebnis als **Datenrahmen** abrufen.

Klicken Sie zum Generieren eines Python-Beispielskripts mit der rechten Maustaste auf __BikeShare Data Prep__, und wählen Sie die Option __Generate Data Access Code File__ (Datenzugriffs-Codedatei generieren) aus. Die Python-Beispieldatei wird in Ihrem **Projektordner** erstellt und außerdem auf einer Registerkarte in Workbench geladen. Das folgende Python-Skript zeigt ein Beispiel für den generierten Code:

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

Wenn Sie den Datenfluss **Join Result** in einer CSV-Datei speichern möchten, müssen Sie das Skript `BikeShare Data Prep.py` ändern. 

1. Öffnen Sie das Projekt zur Bearbeitung in Visual Studio Code.

    ![Öffnen des Projekts in Visual Studio Code](media/tutorial-bikeshare-dataprep/openprojectinvscode.png)

2. Aktualisieren Sie das Python-Skript in der Datei `BikeShare Data Prep.py` mit dem folgenden Code:

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/testata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

3. Ersetzen Sie `Your Azure Storage blob path` durch den Pfad zu der Ausgabedatei, die erstellt werden soll. Ersetzen Sie bei beiden die Variablen `blobfolder` und `csvfiles`.

## <a name="create-an-hdinsight-run-configuration"></a>Erstellen einer HDInsight-Laufzeitkonfiguration

1. Öffnen Sie in Machine Learning Workbench das Befehlszeilenfenster, klicken Sie auf das Menü **File** (Datei), und klicken Sie anschließend auf die Option **Open Command Prompt** (Eingabeaufforderung öffnen). Die Eingabeaufforderung wird im Projektordner mit der Aufforderung `C:\Projects\BikeShare>` geöffnet.

    ![Öffnen der Eingabeaufforderung](media/tutorial-bikeshare-dataprep/opencommandprompt.png)

   >[!IMPORTANT]
   >Für die folgenden Schritte muss das (über Workbench geöffnete) Befehlszeilenfenster verwendet werden.

2. Verwenden Sie die Eingabeaufforderung, um sich bei Azure anzumelden. 

   Die Workbench-App und die Befehlszeilenschnittstelle verwenden unabhängige Anmeldeinformationscaches, wenn Sie sich bei Azure-Ressourcen authentifizieren. Dieser Vorgang muss bis zum Ablauf des zwischengespeicherten Tokens nicht erneut durchgeführt werden. Mit dem Befehl `az account list` wird die Liste der für Ihre Anmeldung verfügbaren Abonnements zurückgegeben. Verwenden Sie den ID-Wert aus dem gewünschten Abonnement, wenn mehr als ein Abonnement vorhanden ist. Legen Sie dieses Abonnement als Standardkonto für die Verwendung mit dem Befehl `az account set -s` fest, und geben Sie den ID-Wert des Abonnements an. Bestätigen Sie die Einstellung mit dem Befehl `show`.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Erstellen Sie die HDInsight-Ausführungskonfiguration. Sie benötigen den Namen des Clusters und das `sshuser`-Kennwort.

    ```azurecli
    az ml computetarget attach cluster --name hdinsight --address <yourclustername>.azurehdinsight.net --username sshuser --password <your password>
    az ml experiment prepare -c hdinsight
    ```
> [!NOTE]
> Beim Erstellen eines leeren Projekts werden die Standardkonfigurationen **local** und **docker** verwendet. In diesem Schritt wird eine Laufzeitkonfiguration erstellt, die beim Ausführen Ihrer Skripts in Workbench verfügbar ist. 

## <a name="run-in-an-hdinsight-cluster"></a>Ausführen in einem HDInsight-Cluster

Kehren Sie zur Machine Learning Workbench-Anwendung zurück, um Ihr Skript im HDInsight-Cluster auszuführen.

1. Klicken Sie auf der linken Seite auf das Symbol **Startseite**, um zur Startseite Ihres Projekts zurückzukehren.

2. Wählen Sie in der Dropdownliste die Option **hdinsight** aus, um Ihr Skript im HDInsight-Cluster auszuführen.

3. Klicken Sie auf **Run** (Ausführen). Das Skript wird als Auftrag übermittelt. Wenn die Datei am angegebenen Speicherort in Ihren Speichercontainer geschrieben wurde, wechselt der Auftragsstatus zu __Completed__ (Abgeschlossen).

    ![HDInsight: Ausführen des Skripts](media/tutorial-bikeshare-dataprep/hdinsightrunscript.png)


## <a name="substitute-data-sources"></a>Ersetzen von Datenquellen

In den vorherigen Schritten haben Sie zur Vorbereitung der Testdaten die Datenquellen `201701-hubway-tripdata.csv` und `BostonWeather.csv` verwendet. Führen Sie die folgenden Schritte aus, um das Paket mit anderen Fahrtdaten-Dateien zu verwenden:

1. Erstellen Sie mithilfe der weiter oben erläuterten Schritte eine neue Datenquelle, nehmen Sie dabei jedoch folgende Änderungen vor:

    * __File Selection__ (Dateiauswahl): Wählen Sie gleichzeitig die sechs verbleibenden CSV-Dateien mit Fahrtdaten aus.

    ![Laden der sechs verbleibenden Dateien](media/tutorial-bikeshare-dataprep/browseazurestoragefortripdatafiles.png)

    > [!NOTE]
     > Der Eintrag __+5__ zeigt an, dass neben der aufgelisteten Datei noch fünf weitere Dateien vorhanden sind.

    * __File Details__ (Dateidetails): Legen Sie __Promote Headers Mode__ (Modus „Header heraufstufen“) auf **All Files Have The Same Headers** (Alle Dateien weisen den gleichen Header auf) fest. Mit diesem Wert wird angegeben, dass alle Dateien den gleichen Header aufweisen.

    ![Ausgewählte Dateidetails](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Speichern Sie den Namen dieser Datenquelle, da sie in späteren Schritten verwendet wird.

2. Wählen Sie das Ordnersymbol aus, um die Dateien im Projekt anzuzeigen. Erweitern Sie das Verzeichnis __aml\_config__, und wählen Sie die Datei `hdinsight.runconfig` aus.

    ![Speicherort von „hdinsight.runconfig“](media/tutorial-bikeshare-dataprep/hdinsightsubstitutedatasources.png) 

3. Klicken Sie auf die Schaltfläche **Edit** (Bearbeiten), um die Datei in Visual Studio Code zu öffnen.

4. Fügen Sie am Ende der Datei `hdinsight.runconfig` die folgenden Zeilen hinzu, und klicken Sie anschließend auf das Datenträgersymbol, um die Datei zu speichern.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Mit dieser Änderung wird die ursprüngliche Datenquelle durch die Datenquelle mit den sechs Fahrtdaten-Dateien ersetzt.

## <a name="save-training-data-as-a-csv-file"></a>Speichern der Trainingsdaten als CSV-Datei

1. Navigieren Sie zur Python-Datei `BikeShare Data Prep.py`, die Sie zuvor bearbeitet haben. Geben Sie einen anderen Dateipfad zum Speichern der Trainingsdaten an.

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/traindata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

2. Verwenden Sie den Ordner `traindata` für die Ausgabe der Trainingsdaten.

3. Klicken Sie auf **Run** (Ausführen), um einen neuen Auftrag zu senden. Stellen Sie sicher, dass **hdinsight** ausgewählt ist. Ein Auftrag mit der neuen Konfiguration wird gesendet. Bei der Ausgabe dieses Auftrags handelt es sich um die Trainingsdaten. Diese Daten werden mit den gleichen Datenvorbereitungsschritten erstellt, die Sie auch zuvor verwendet haben. Die Ausführung des Auftrags kann einige Minuten dauern.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte
Das Tutorial zur Vorbereitung der Bikesharing-Daten ist hiermit abgeschlossen. In diesem Tutorial haben Sie anhand von Machine Learning (Vorschauversion) Folgendes gelernt:
> [!div class="checklist"]
> * Interaktives Vorbereiten von Daten mit dem Machine Learning-Datenvorbereitungstool
> * Importieren, Transformieren und Erstellen eines Testdatasets
> * Generieren eines Datenvorbereitungspakets
> * Ausführen des Datenvorbereitungspakets mit Python
> * Generieren eines Trainingsdatasets durch Wiederverwenden des Datenvorbereitungspakets für weitere Eingabedateien

Nun erfahren Sie mehr über die Datenvorbereitung:
> [!div class="nextstepaction"]
> [Anleitung zur Datenvorbereitung](data-prep-user-guide.md)
