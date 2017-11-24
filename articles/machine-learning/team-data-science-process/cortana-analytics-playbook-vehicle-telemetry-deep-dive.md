---
title: "Ausführliche Informationen zum Vorhersagen von Fahrzeugzustand und Fahrgewohnheiten – Azure | Microsoft-Dokumentation"
description: "Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: cdde0c8dc2fd1189970c0782769a609ca8142372
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Lösungs-Playbook zur Fahrzeugtelemetrieanalyse: Ausführliche Betrachtung der Lösung
Dieses Menü enthält Links zu den Abschnitten des Playbooks: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

In diesem Dokument werden die einzelnen in der Lösungsarchitektur dargestellten Stufen beschrieben. Es enthält Anweisungen und Hinweise für die Anpassung. 

## <a name="data-sources"></a>Datenquellen
Für die Lösung werden zwei unterschiedliche Datenquellen verwendet:

* Datasets mit simulierten Fahrzeugsignalen und Diagnosedaten
* Fahrzeugkatalog

Zu dieser Lösung gehört ein Simulator für Fahrzeugtelematik, wie im folgenden Bildschirmfoto gezeigt. Diese Anwendung gibt Diagnosedaten und Signale entsprechend dem Zustand des Fahrzeugs und Fahrmuster zu einem bestimmten Zeitpunkt aus. Besuchen Sie die Webseite [Vehicle Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075), um die Visual Studio-Projektmappe „Vehicle Telematics Simulator“ herunterzuladen, wenn Sie Anpassungen nach Ihren Anforderungen vornehmen möchten. Der Fahrzeugkatalog enthält ein Verweisdataset, in dem Fahrzeugidentifizierungsnummern (Vehicle Identification Numbers, VINs) Modellen zugeordnet sind.

![Vehicle Telematics Simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)


Dieses Dataset im JSON-Format enthält das folgendes Schema.

| Column | Beschreibung | Werte |
| --- | --- | --- |
| VIN (Fahrzeug-Identifizierungsnummer) |Zufällig generierte VIN |Aus einer Masterliste von 10.000 nach dem Zufallsprinzip generierten VINs |
| Outside temperature (Außentemperatur) |Die Außentemperatur an dem Ort, an dem sich das Fahrzeug befindet |Zufällig generierte Zahl von 0 bis 100 |
| Engine temperature (Motortemperatur) |Motortemperatur des Fahrzeugs |Zufällig generierte Zahl von 0 bis 500 |
| Geschwindigkeit |Geschwindigkeit, mit der sich das Fahrzeug bewegt |Zufällig generierte Zahl von 0 bis 100 |
| Fuel (Kraftstoff) |Kraftstoffmenge des Fahrzeugs |Zufällig generierte Zahl von 0 bis 100 (Kraftstoffstand in Prozent) |
| EngineOil (Motoröl) |Motorölstand des Fahrzeugs |Zufällig generierte Zahl von 0 bis 100 (Motorölstand in Prozent) |
| Reifendruck |Reifendruck des Fahrzeugs |Zufällig generierte Zahl von 0 bis 50 (Reifendruck in Prozent) |
| Odometer (Kilometerzähler) |Kilometerstand des Fahrzeugs |Zufällig generierte Zahl von 0 bis 200.000 |
| Accelerator_pedal_position (Position des Gaspedals) |Gaspedalposition des Fahrzeugs |Zufällig generierte Zahl von 0 bis 100 (Gaspedalposition in Prozent) |
| Parking_brake_status (Status der Handbremse) |Gibt an, ob das Fahrzeug geparkt und die Handbremse gezogen ist |„true“ oder „false“ |
| Headlamp_status (Scheinwerferstatus) |Gibt an, ob die Scheinwerfer eingeschaltet sind |„true“ oder „false“ |
| Brake_pedal_status (Status des Bremspedals) |Gibt an, ob das Bremspedal betätigt wird |„true“ oder „false“ |
| Transmission_gear_position (Position des Ganghebels) |Gangposition des Fahrzeugs |Positionen: Erster, Zweiter, Dritter, Vierter, Fünfter, Sechster, Siebter, Achter |
| Ignition_status (Status der Zündung) |Gibt an, ob das Fahrzeug fährt oder ob die Zündung ausgeschaltet ist |„true“ oder „false“ |
| Windshield_wiper_status (Scheibenwischerstatus) |Gibt an, ob die Scheibenwischer eingeschaltet sind |„true“ oder „false“ |
| ABS (ABS) |Gibt an, ob ABS aktiviert ist |„true“ oder „false“ |
| Zeitstempel |Zeitstempel für die Erstellung des Datenpunkts |Date |
| City (Ort) |Standort des Fahrzeugs |Diese Projektmappe enthält vier Orte : Bellevue, Redmond, Sammamish, Seattle |

Im Fahrzeugmodell-Verweisdataset werden VINs Modellen zugeordnet. 

| VIN (Fahrzeug-Identifizierungsnummer) | Modell |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Limousine |
| 8J0U8XCPRGW4Z3NQE |Hybrid |
| WORG68Z2PLTNZDBI7 |Familienlimousine |
| JTHMYHQTEPP4WBMRN |Limousine |
| W9FTHG27LZN1YWO0Y |Hybrid |
| MHTP9N792PHK08WJM |Familienlimousine |
| EI4QXI2AXVQQING4I |Limousine |
| 5KKR2VB4WHQH97PF8 |Hybrid |
| W9NSZ423XZHAONYXB |Familienlimousine |
| 26WJSGHX4MA5ROHNL |Cabrio |
| GHLUB6ONKMOSI7E77 |Kombi |
| 9C2RHVRVLMEJDBXLP |Kleinwagen |
| BRNHVMZOUJ6EOCP32 |Kleines SUV |
| VCYVW0WUZNBTM594J |Sportwagen |
| HNVCE6YFZSA5M82NY |Mittelgroßes SUV |
| 4R30FOR7NUOBL05GJ |Kombi |
| WYNIIY42VKV6OQS1J |Großes SUV |
| 8Y5QKG27QET1RBK7I |Großes SUV |
| DF6OX2WSRA6511BVG |Coupé |
| Z2EOZWZBXAEW3E60T |Limousine |
| M4TV6IEALD5QDS3IR |Hybrid |
| VHRA1Y2TGTA84F00H |Familienlimousine |
| R0JAUHT1L1R3BIKI0 |Limousine |
| 9230C202Z60XX84AU |Hybrid |
| T8DNDN5UDCWL7M72H |Familienlimousine |
| 4WPYRUZII5YV7YA42 |Limousine |
| D1ZVY26UV2BFGHZNO |Hybrid |
| XUF99EW9OIQOMV7Q7 |Familienlimousine |
| 8OMCL3LGI7XNCC21U |Cabrio |
| ……. | |

## <a name="ingestion"></a>Erfassung
Zum Erfassen der Fahrzeugsignale sowie der Diagnoseereignisse und Echtzeit- und Batchanalysen wird eine Kombination aus Azure Event Hubs, Azure Stream Analytics und Azure Data Factory genutzt. All diese Komponenten werden im Rahmen der Lösungsbereitstellung erstellt und konfiguriert. 

### <a name="real-time-analysis"></a>Echtzeitanalyse
Die vom Vehicle Telematics Simulator generierten Ereignisse werden mit dem Event Hub SDK auf dem Event Hub veröffentlicht.  

![Event Hub-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

Der Stream Analytics-Auftrag erfasst diese Ereignisse aus dem Event Hub und verarbeitet die Daten in Echtzeit, um die Integrität des Fahrzeugs zu analysieren.

![Verarbeitungsdaten für Stream Analytics-Auftrag](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 


Der Stream Analytics-Auftrag

* erfasst Daten aus dem Event Hub.
* führt eine Verknüpfung mit den Referenzdaten durch, um die Fahrzeug-Identifizierungsnummer dem entsprechenden Modell zuzuordnen. 
* speichert diese dauerhaft in der Azure Blob Storage-Einheit, um Batchanalysen durchführen zu können. 

Die folgende Stream Analytics-Abfrage wird verwendet, um die Daten dauerhaft in der Blob Storage-Einheit zu speichern: 

![Stream Analytics-Auftrag – Abfrage zur Datenerfassung](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 


### <a name="batch-analysis"></a>Batchanalyse
Es wird auch ein zusätzliches Volume mit simulierten Fahrzeugsignalen und einem Diagnosedataset generiert, um eine umfassendere Batchanalyse zu ermöglichen. Dieses zusätzliche Volume ist erforderlich, um für die Batchverarbeitung ein gutes, repräsentatives Datenvolume sicherzustellen. Zu diesem Zweck wird im Data Factory-Workflow „PrepareSampleDataPipeline“ verwendet, um simulierte Fahrzeugsignale und ein Diagnosedataset für den Zeitraum eines Jahrs zu generieren. Besuchen Sie die Webseite [Data Factory custom activity](http://go.microsoft.com/fwlink/?LinkId=717077), um die Visual Studio-Projektmappe „Data Factory custom .NET activity“ herunterzuladen, wenn Sie Anpassungen nach Ihren Anforderungen vornehmen möchten. 

In diesem Workflow werden für die Batchverarbeitung vorbereitete Beispieldaten gezeigt.

![Workflow mit für die Batchverarbeitung vorbereiteten Beispieldaten](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 


Die Pipeline umfasst eine benutzerdefinierte Data Factory-.NET-Aktivität.

![PrepareSampleDataPipeline-Aktivität](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

Nachdem die Pipeline erfolgreich ausgeführt wurde und das Dataset „RawCarEventsTable“ mit „Ready“ gekennzeichnet ist, werden simulierte Fahrzeugsignale und Diagnosedaten für den Zeitraum eines Jahrs generiert. Sie sehen, dass in Ihrem Speicherkonto unter dem Container „connectedcar“ der folgende Ordner und die folgende Datei erstellt werden:

![Ausgabe von „PrepareSampleDataPipeline“](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

## <a name="partition-the-data-set"></a>Partitionieren des Datasets
Im Datenvorbereitungsschritt werden die unformatierten halbstrukturierten Fahrzeugsignale und das Diagnosedataset in das Format JAHR/MONAT partitioniert. Diese Partitionierung sorgt für effizientere Abfragen und skalierbaren Langzeitspeicher, indem das Failover aktiviert wird. Wenn beispielsweise das erste Blob-Konto voll ist, erfolgt ein Failover auf das nächste Konto. 

>[!NOTE] 
>Dieser Schritt der Projektmappe gilt nur für die Batchverarbeitung.

Verwaltung von Ein- und Ausgabedaten:

* **Ausgabedaten** (mit der Bezeichnung „PartitionedCarEventsTable“) werden im Data Lake des Kunden über einen längeren Zeitraum als grundlegende „Rohdaten“ aufbewahrt. 
* **Eingabedaten** dieser Pipeline werden in der Regel verworfen, da die Ausgabedaten vollständig mit der Eingabe übereinstimmen. Sie sollten für die nachfolgende Verwendung gespeichert (partitioniert) werden.

Der Workflow „Partition Car Events“

![Workflow „Partition Car Events“](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)


Die unformatierten Daten werden mit einer Azure HDInsight-Hive-Aktivität in „PartitionCarEventsPipeline“ partitioniert, wie im folgenden Bildschirmfoto gezeigt. Die im Datenvorbereitungsschritt für ein Jahr generierten Beispieldaten werden nach JAHR/MONAT partitioniert. Die Partitionen werden zum Generieren von Fahrzeugsignalen und Diagnosedaten für jeden Monat eines Jahrs (insgesamt 12 Partitionen) verwendet. 

![PartitionCarEventsPipeline-Aktivität](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)


**PartitionConnectedCarEvents-Hive-Skript**

Für die Partitionierung wird das Hive-Skript „partitioncarevents.hql“ verwendet. Es befindet sich im Ordner „\demo\src\connectedcar\scripts“ der heruntergeladenen ZIP-Datei. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Nachdem die Pipeline erfolgreich ausgeführt wurde, werden im Speicherkonto unter dem Container „connectedcar“ die unten angegebenen Partitionen generiert:

![Partitionierte Ausgabe](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

Die Daten sind jetzt optimiert, besser verwaltbar und bereit für die weitere Verarbeitung, damit Sie umfassende Erkenntnisse zu den Batches gewinnen können. 

## <a name="data-analysis"></a>Datenanalyse
In diesem Abschnitt erfahren Sie, wie Stream Analytics, Azure Machine Learning, Data Factory und HDInsight für die umfassende erweiterte Analyse des Fahrzeugzustands und des Fahrverhaltens kombiniert werden.

### <a name="machine-learning"></a>Machine Learning
Das Ziel besteht hierbei darin, die Fahrzeuge vorherzusagen, die gewartet oder aufgrund bestimmter Integritätsstatistiken zurückgerufen werden müssen. Dabei gelten die folgenden Prämissen:

* Für Fahrzeuge ist eine Wartung erforderlich, wenn eine der folgenden drei Bedingungen erfüllt ist:
  
  * Niedriger Reifendruck
  * Niedriger Motorölstand
  * Hohe Motortemperatur

* Für Fahrzeuge bestehen unter Umständen ein Sicherheitsproblem und die Notwendigkeit eines Rückrufs, wenn eine der folgenden Bedingungen erfüllt ist:
  
  * Hohe Motortemperatur bei niedriger Außentemperatur
  * Niedrige Motortemperatur bei hoher Außentemperatur

In zwei gesonderten Modellen werden basierend auf den oben genannten Anforderungen Anomalien festgestellt. Ein Modell dient zur Fahrzeugwartungserkennung, und das andere Modell zur Fahrzeugrückruferkennung. Für beide Modelle wird zur Erkennung von Anomalien der integrierte Principal Component Analysis-Algorithmus (PCA) verwendet. 

#### <a name="maintenance-detection-model"></a>**Modell für die Wartungserkennung**

Beim Modell für die Erkennung der Wartung wird eine Anomalie gemeldet, wenn für einen von drei Indikatoren – Reifendruck, Motoröl oder Motortemperatur – die jeweilige Bedingung erfüllt ist. Daher müssen beim Erstellen des Modells nur diese drei Variablen berücksichtigt werden. Für das Experiment in Machine Learning wird das Modul **Select Columns in Dataset** (Spaltenauswahl in Dataset) verwendet, um diese drei Variablen zu extrahieren. Als Nächstes wird das PCA-basierte Modul für die Erkennung von Anomalien verwendet, um das Modell für die Erkennung von Anomalien zu erstellen. 

PCA ist ein bewährtes Machine Learning-Verfahren, das auf die Featureauswahl, Klassifizierung und Erkennung von Anomalien angewendet werden kann. Bei der PCA wird eine Gruppe von Fällen, die unter Umständen korrelierte Variablen enthalten, in eine Gruppe von Werten konvertiert, die als Hauptkomponenten (Principal Components) bezeichnet werden. Der Hauptgedanke der PCA-basierten Modellierung ist das Projizieren von Daten auf eine niedrigere Dimension, damit Merkmale und Anomalien leichter identifiziert werden können.

Für jede neue Eingabe berechnet die Anomalieerkennung zuerst die Projektion auf die Eigenvektoren. Anschließend wird der normalisierte Rekonstruktionsfehler berechnet. Dieser normalisierte Fehler ist das Anomalieergebnis: Je höher die Fehler, desto größer die Anomalie der Instanz. 

Bei der Wartungserkennung wird jeder Datensatz als Punkt in einem dreidimensionalen Raum angesehen, der durch die Koordinaten für Reifendruck, Motoröl und Motortemperatur definiert wird. Zum Erfassen dieser Anomalien werden die Originaldaten per PCA aus dem dreidimensionalen in einen zweidimensionalen Raum projiziert. Daher wird der Parameter für die Anzahl der Komponenten, die für die PCA verwendet werden sollen, auf „2“ festgelegt. Dieser Parameter spielt eine wichtige Rolle beim Anwenden der PCA-basierten Erkennung von Anomalien. Nach dem Projizieren der Daten per PCA werden diese Anomalien leichter erkannt.

#### <a name="recall-anomaly-detection-model"></a>**Modell für die Erkennung von Anomalien in Bezug auf Rückrufe**

Für das Modell zur Erkennung von Anomalien in Bezug auf Rückrufe werden das Modul **Select Columns in Dataset** (Spaltenauswahl in Dataset) und das Modul für die PCA-basierte Erkennung von Anomalien auf ähnliche Weise verwendet. Zuerst werden dabei mit dem Modul **Select Columns in Dataset** (Spaltenauswahl in Dataset) drei Variablen extrahiert: Motortemperatur, Außentemperatur und Geschwindigkeit. Die Geschwindigkeitsvariable wird einbezogen, da die Motortemperatur normalerweise mit der Geschwindigkeit korreliert. Als Nächstes wird das Modul für die PCA-basierte Erkennung von Anomalien verwendet, um die Daten aus dem dreidimensionalen Raum in einen zweidimensionalen Raum zu projizieren. Die Kriterien für den Rückruf werden erfüllt. Für das Fahrzeug ist ein Rückruf erforderlich, wenn die Motortemperatur und die Außentemperatur eine hohe negative Korrelation aufweisen. Nach Durchführung der PCA wird der Algorithmus für die PCA-basierte Erkennung von Anomalien verwendet, um die Anomalien zu erfassen. 

Beim Trainieren beider Modelle werden normale Daten als Eingabedaten zum Trainieren des PCA-basierten Modells zur Erkennung von Anomalien verwendet. (Normale Daten erfordern weder Wartung noch Rückruf.) Beim Bewertungsexperiment wird das trainierte Modell zur Erkennung von Anomalien verwendet, um erkennen zu können, ob für das Fahrzeug eine Wartung oder ein Rückruf erforderlich ist. 

### <a name="real-time-analysis"></a>Echtzeitanalyse
Mit der folgenden Stream Analytics-SQL-Abfrage wird der Durchschnitt aller wichtigen Fahrzeugparameter abgerufen. Diese Parameter umfassen die Geschwindigkeit des Fahrzeugs, Kraftstoffmenge, Motortemperatur, Kilometerstand, Reifendruck, Motorölstand und weitere Parameter. Anhand der Durchschnittswerte können Anomalien erkannt und Warnungen ausgegeben werden. Außerdem kann der Gesamtzustand von Fahrzeugen ermittelt werden, die in einer bestimmten Region bewegt werden. Die Durchschnittswerte werden dann mit demografischen Daten korreliert. 

![Stream Analytics-Abfrage für Echtzeitverarbeitung](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Alle Durchschnittswerte werden mit einem rollierenden Fenster (TumblingWindow) von drei Sekunden berechnet. Ein rollierendes Fenster wird verwendet, da nicht überlappende zusammenhängende Zeitintervalle erforderlich sind. 

Weitere Informationen zu allen „Windowing“-Funktionen in Stream Analytics erhalten Sie auf der Seite [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx) (in englischer Sprache).

#### <a name="real-time-prediction"></a>**Echtzeitvorhersage**

Die Projektmappe enthält eine Anwendung, mit der das Machine Learning-Modell in Echtzeit operationalisiert werden kann. Die Anwendung „RealTimeDashboardApp“ wird im Rahmen der Lösungsbereitstellung erstellt und konfiguriert. Für die Anwendung gilt Folgendes:

* Sie überwacht eine Event Hub-Instanz, in der Stream Analytics die Ereignisse fortlaufend in einem Muster veröffentlicht.

    ![Stream Analytics-Abfrage für die Veröffentlichung der Daten](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) 

* Empfängt Ereignisse. Für jedes Ereignis, das diese Anwendung empfängt, wird Folgendes durchgeführt: 
   
   * Die Daten werden über einen Endpunkt mit Machine Learning-Request-Response-Bewertung (RRS) verarbeitet. Der RRS-Endpunkt wird automatisch als Teil der Bereitstellung veröffentlicht.
   * Die RRS-Ausgabe wird mit den Push-APIs in einem Power BI-Dataset veröffentlicht.

Dieses Muster gilt auch für Szenarien, bei denen Sie eine Branchenanwendung in den Ablauf der Echtzeitanalyse integrieren möchten. Zu diesen Szenarien zählen Warnungen, Benachrichtigungen und Messaging.

Besuchen Sie die Webseite [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078), um die Visual Studio-Projektmappe „RealtimeDashboardApp“ zur Verwendung für Anpassungen herunterzuladen. 

#### <a name="execute-the-real-time-dashboard-application"></a>**Ausführen der Echtzeit-Dashboardanwendung**
1. Extrahieren Sie die Projektmappe „RealtimeDashboardApp“, und speichern Sie sie lokal.

    ![Ordner „RealTimeDashboardApp“](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) 

2. Führen Sie die Anwendung „RealtimeDashboardApp.exe“ aus.

3. Geben Sie gültige Power BI-Anmeldeinformationen ein, und wählen Sie **Anmelden** aus.  

    ![Anmeldefenster für RealTimeDashboardApp](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 
    
4. Wählen Sie **Akzeptieren** aus.

    ![Endgültiges Anmeldefenster für RealTimeDashboardApp](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

>[!NOTE] 
>Zum Leeren der Daten für das Power BI-Dataset führen Sie RealtimeDashboardApp mit dem Parameter „flushdata“ aus. 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Batchanalyse
Hier soll gezeigt werden, wie Contoso Motors die Azure Compute-Funktionen für Big Data nutzt. Diese Daten bieten umfassende Einblicke in Fahrmuster, Nutzungsverhalten und Fahrzeugzustand. Diese Informationen ermöglichen Folgendes:

* Verbessern der Kundenerfahrung und Kostensenkung, indem Erkenntnisse zu Fahrgewohnheiten und kraftstoffsparendem Fahrverhalten gewonnen werden
* Proaktives Gewinnen von Informationen zu Kunden und ihrem Fahrverhalten, um bessere Geschäftsentscheidungen treffen und die besten Produkte und Dienstleistungen anbieten zu können

Bei dieser Lösung liegt der Schwerpunkt auf folgenden Metriken:

* **Aggressives Fahrverhalten**: Identifiziert den Trend in Bezug auf Modelle, Standorte, Fahrbedingungen und Jahreszeiten, um Erkenntnisse zu aggressivem Fahrverhalten zu gewinnen. Diese Daten können von Contoso Motors für Marketingkampagnen, die Einführung neuer personalisierter Funktionen und eine nutzungsabhängige Versicherung eingesetzt werden.
* **Kraftstoffsparendes Fahrverhalten**: Identifiziert den Trend in Bezug auf Modelle, Standorte, Fahrbedingungen und Jahreszeiten, um Erkenntnisse zu kraftstoffsparendem Fahrverhalten zu gewinnen. Diese Daten kann Contoso Motors für Marketingkampagnen, die Einführung neuer Funktionen und das proaktive Bereitstellen von Daten für Fahrer nutzen, um das kostensparende und umweltfreundliche Fahrverhalten zu fördern.
* **Rückrufmodelle**: Identifiziert Modelle, für die ein Rückruf erforderlich ist, indem das Machine Learning-Experiment zur Erkennung von Anomalien operationalisiert wird.

Wir sehen uns nun die Details dieser Metriken an.

#### <a name="aggressive-driving-behavior-patterns"></a>**Aggressive Fahrmuster**

Die partitionierten Fahrzeugsignale und Diagnosedaten werden in der Pipeline „AggresiveDrivingPatternPipeline“ verarbeitet, wie im folgenden Workflow gezeigt. Mithilfe von Hive werden die Modelle, der Standort, die Fahrzeug- und Fahrbedingungen sowie weitere Parameter bestimmt, die auf aggressive Fahrmuster hindeuten.

![Aggressives Fahrmuster – Workflow](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 

***Aggressives Fahrmuster – Hive-Abfrage***

Das Hive-Skript „aggresivedriving.hql“ wird zum Analysieren von aggressiven Fahrmustern verwendet. Es befindet sich im Ordner „\demo\src\connectedcar\scripts“ der heruntergeladenen ZIP-Datei. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Im Skript wird die Kombination aus Ganghebelposition, Bremspedalstatus und Geschwindigkeit des Fahrzeugs verwendet, um ein rücksichtsloses bzw. aggressives Fahrverhalten anhand des Bremsmusters bei hoher Geschwindigkeit zu erkennen. 

Nachdem die Pipeline erfolgreich ausgeführt wurde, werden im Speicherkonto unter dem Container „connectedcar“ die unten angegebenen Partitionen generiert:

![Ausgabe von „AggressiveDrivingPatternPipeline“](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 


#### <a name="fuel-efficient-driving-behavior-patterns"></a>**Kraftstoffsparende Fahrmuster**

Die partitionierten Fahrzeugsignale und Diagnosedaten werden in der Pipeline „FuelEfficientDrivingPatternPipeline“ verarbeitet, wie im folgenden Workflow gezeigt. Mithilfe von Hive werden die Modelle, der Standort, die Fahrzeug- und Fahrbedingungen sowie andere Parameter bestimmt, die auf ein kraftstoffsparendes Fahrmuster hindeuten.

![Kraftstoffsparende Fahrmuster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

***Kraftstoffsparendes Fahrmuster – Hive-Abfrage***

Das Hive-Skript „fuelefficientdriving.hql“ wird zum Analysieren von kraftstoffsparenden Fahrmustern verwendet. Es befindet sich im Ordner „\demo\src\connectedcar\scripts“ der heruntergeladenen ZIP-Datei. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Im Skript wird die Kombination aus Ganghebelposition, Bremspedalstatus, Geschwindigkeit und Gaspedalposition des Fahrzeugs verwendet, um kraftstoffsparendes Fahrverhalten anhand des Musters aus Beschleunigung, Bremsmanövern und Geschwindigkeit zu erkennen. 

Nachdem die Pipeline erfolgreich ausgeführt wurde, werden im Speicherkonto unter dem Container „connectedcar“ die unten angegebenen Partitionen generiert:

![Ausgabe von „FuelEfficientDrivingPatternPipeline“](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

**Rückrufmodellvorhersagen**

Das Machine Learning-Experiment wird im Rahmen der Lösungsbereitstellung als Webdienst bereitgestellt und veröffentlicht. In diesem Workflow wird der Batchbewertungsendpunkt verwendet. Er ist als mit Data Factory verknüpfter Dienst registriert und wird mithilfe der Data Factory-Batchbewertungsaktivität operationalisiert.

![Machine Learning-Endpunkt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

Der registrierte verknüpfte Dienst wird in „DetectAnomalyPipeline“ verwendet, um die Daten mit dem Modell zur Erkennung von Anomalien zu bewerten. 

![Machine Learning-Aktivität für die Batchbewertung in Data Factory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png)  

In dieser Pipeline werden einige Schritte für die Datenvorbereitung ausgeführt, damit die Operationalisierung mit dem Webdienst für die Batchbewertung erfolgen kann. 

![DetectAnomalyPipeline für die Vorhersage von Rückrufen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png)  

***Anomalieerkennung – Hive-Abfrage***

Nach Abschluss der Bewertung werden die vom Modell als Anomalien eingestuften Daten durch eine HDInsight-Aktivität verarbeitet und aggregiert. Das Modell verwendet einen Wahrscheinlichkeitswert von 0,60 oder höher.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Nachdem die Pipeline erfolgreich ausgeführt wurde, werden im Speicherkonto unter dem Container „connectedcar“ die unten angegebenen Partitionen generiert:

![Ausgabe von „DetectAnomalyPipeline“](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

## <a name="publish"></a>Veröffentlichen

### <a name="real-time-analysis"></a>Echtzeitanalyse
Eine der Abfragen im Stream Analytics-Auftrag veröffentlicht die Ereignisse auf einer Event Hub-Ausgabeinstanz. 

![Veröffentlichung auf einer Event Hub-Ausgabeinstanz durch einen Stream Analytics-Auftrag](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

Die folgende Stream Analytics-Abfrage wird zum Veröffentlichen auf der Event Hub-Ausgabeinstanz verwendet:

![Stream Analytics-Abfrage zum Veröffentlichen auf der Event Hub-Ausgabeinstanz](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

Dieser Datenstrom mit Ereignissen wird von der RealTimeDashboardApp genutzt, die in der Projektmappe enthalten ist. Diese Anwendung verwendet zur Echtzeitbewertung den Machine Learning-Request-Response-Webdienst. Die resultierenden Daten werden zur Nutzung in einem Power BI-Dataset veröffentlicht. 

### <a name="batch-analysis"></a>Batchanalyse
Die Ergebnisse der Batch- und Echtzeitverarbeitung werden zur Nutzung in den Tabellen von Azure SQL-Datenbank veröffentlicht. Der SQL Server, die Datenbank und die Tabellen werden im Rahmen des Setupskripts automatisch erstellt. 

Die Ergebnisse der Batchverarbeitung werden in den Data Mart-Workflow kopiert.

![In den Data Mart-Workflow kopierte Ergebnisse der Batchverarbeitung](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

Der Stream Analytics-Auftrag wird im Data Mart veröffentlicht.

![In Data Mart veröffentlichter Stream Analytics-Auftrag](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

Die Data Mart-Einstellung befindet sich im Stream Analytics-Auftrag.

![Data Mart-Einstellung in Stream Analytics-Auftrag](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

## <a name="consume"></a>Nutzen
Power BI bietet für diese Lösung ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und Predictive Analytics. 

Das fertige Dashboard sieht wie in diesem Beispiel aus:

![Power BI-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

## <a name="summary"></a>Zusammenfassung
In diesem Dokument wird die Projektmappe für die Fahrzeugtelemetrieanalyse (Vehicle Telemetry Analytics) ausführlich beschrieben. Für die Echtzeit- und Batchanalyse mit Vorhersagen und Aktionen wird das Lambda-Architekturmuster verwendet. Dieses Muster gilt für eine Vielzahl von Anwendungsfällen, für die „Hot Path“-Analysen (Echtzeit) und „Cold Path“-Analysen (Batch) erforderlich sind. 

### <a name="references"></a>Referenzen

* [Visual Studio-Projektmappe „Vehicle Telematics Simulator“](http://go.microsoft.com/fwlink/?LinkId=717075) 
* [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
* [Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)
* [Azure Event Hub-SDK für Datenstromerfassung](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* [Funktionen zum Verschieben von Azure Data Factory-Daten](../../data-factory/v1/data-factory-data-movement-activities.md)
* [Azure Data Factory-.NET-Aktivität](../../data-factory/v1/data-factory-use-custom-activities.md)
* [Zum Vorbereiten von Beispieldaten verwendete Visual Studio-Projektmappe „Azure Data Factory-.NET-Aktivität“](http://go.microsoft.com/fwlink/?LinkId=717077) 
