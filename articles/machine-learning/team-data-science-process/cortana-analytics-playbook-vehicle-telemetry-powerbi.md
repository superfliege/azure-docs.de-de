---
title: "Power BI-Dashboard für Fahrzeugzustand und Fahrgewohnheiten – Azure | Microsoft-Dokumentation"
description: "Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 4a28ef3765518fe2948853d1c6334cf034b40d34
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Lösungsvorlage „Analyse von Telemetriedaten für Fahrzeuge“ – Anweisungen zur Einrichtung des Power BI-Dashboards
Dieses Menü bietet Links zu den Kapiteln dieses Playbooks: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Die Lösung zur Fahrzeugtelemetrieanalyse zeigt, wie Autohändler, Hersteller und Versicherungsunternehmen die Funktionen von Cortana Intelligence verwenden können. Sie können Echtzeit- und Prognosedaten zu Fahrzeugzustand und Fahrgewohnheiten abrufen um so die Kundenzufriedenheit, Forschung und Entwicklung und Marketingkampagnen optimieren. Diese schrittweisen Anleitungen zeigen, wie Sie Power BI-Berichte und das Dashboard konfigurieren können, nachdem die Lösung in Ihrem Abonnement bereitgestellt wurde. 

## <a name="prerequisites"></a>Voraussetzungen
* Stellen Sie die Lösung [Analyse von Telemetriedaten für Fahrzeuge](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) bereit. 
* Installieren Sie [Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331).
* Schließen Sie ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/) ab. Wenn Sie kein Abonnement haben, verwenden Sie zunächst ein kostenloses Azure-Abonnement.
* Erstellen Sie ein Power BI-Konto.

## <a name="cortana-intelligence-suite-components"></a>Komponenten der Cortana Intelligence Suite
Die nachstehenden Cortana Intelligence-Dienste werden als Teil der Lösungsvorlage für Fahrzeugtelemetrieanalyse in Ihrem Abonnement bereitgestellt:

* **Azure Event Hubs** für das Erfassen von Millionen von Telemetrie-Ereignissen zu Fahrzeugen in Azure.
* **Azure Stream Analytics** für das Gewinnen von Einblicken in Echtzeit zum Fahrzeugzustand und Speichern dieser Daten in beständigem Speicher zur umfangreicheren Batchanalyse.
* **Azure Machine Learning** erkennt Anomalien in Echtzeit und stellt mithilfe der Batchverarbeitung Prognoseinformationen bereit.
* **Azure HDInsight** transformiert Daten auch in großem Umfang.
* **Azure Data Factory** übernimmt die Orchestrierung, Planung, Ressourcenverwaltung und Überwachung der Batchverarbeitungs-Pipeline.

**Power BI** bietet dieser Lösung ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und Predictive Analytics. 

Für die Lösung werden zwei unterschiedliche Datenquellen verwendet:

* Datasets mit simulierten Fahrzeugsignalen und Diagnosedaten
* Fahrzeugkatalog

Zu dieser Lösung gehört ein Simulator für Fahrzeugtelematik. Diese Anwendung gibt Diagnosedaten und Signale entsprechend dem Zustand des Fahrzeugs und dem Fahrmuster zu einem bestimmten Zeitpunkt aus. 

Der Fahrzeugkatalog ist ein Verweisdataset, das VINs Modellen zuordnet.

## <a name="power-bi-dashboard-preparation"></a>Vorbereitung des Power BI-Dashboards
### <a name="set-up-the-power-bi-real-time-dashboard"></a>Einrichten des Power BI-Echtzeitdashboards

#### <a name="start-the-real-time-dashboard-application"></a>Starten der Echtzeit-Dashboardanwendung
Nachdem die Bereitstellung abgeschlossen ist, folgen Sie den Anweisungen für den manuellen Betrieb.

1. Laden Sie die Echtzeit-Dashboardanwendung „RealtimeDashboardApp.zip“ herunter, und entzippen Sie sie.

2.  Öffnen Sie im entzippten Ordner die App-config-Datei „RealtimeDashboardApp.exe.config“. Ersetzen Sie „appSettings“ für Event Hubs-, Azure Blob Storage- und Azure Machine Learning-Dienstverbindungen durch die Werte in den Anweisungen für den manuellen Betrieb. Speichern Sie die Änderungen.

3. Führen Sie die Anwendung „RealtimeDashboardApp.exe“ aus. Geben Sie im Anmeldefenster gültige Power BI-Anmeldeinformationen ein. 

   ![Power BI-Anmeldefenster](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
4. Wählen Sie **Akzeptieren** aus. Die App wird ausgeführt.

   ![Power BI-Dashboardberechtigungen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

5. Melden Sie sich auf der Power BI-Website an, und erstellen Sie ein Echtzeitdashboard.

Nun können Sie das Power BI-Dashboard konfigurieren.  

### <a name="configure-power-bi-reports"></a>Konfigurieren von Power BI-Berichten
Die Erstellung der Echtzeitberichte und des Dashboards dauert ca. 30 bis 45 Minuten. 

1. Navigieren Sie zur [Power BI](http://powerbi.com)-Webseite, und melden Sie sich an.

    ![Power BI-Anmeldeseite](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

2. Ein neues Dataset wird in Power BI generiert. Wählen Sie das Dataset **ConnectedCarsRealtime** aus.

    ![Dataset „ConnectedCarsRealtime“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

3. Speichern Sie den leeren Bericht durch Drücken von STRG+S.

    ![Leerer Bericht](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

4. Geben Sie den Berichtsnamen **Vehicle Telemetry Analytics Real Time – Reports** (Fahrzeugtelemetrieanalyse in Echtzeit – Berichte) ein.

    ![Berichtsname](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Echtzeitberichte
Diese Lösung enthält drei Echtzeitberichte:

* Vehicles in Operation (Fahrzeuge im Einsatz)
* Vehicles Requiring Maintenance (Fahrzeuge, für die die Wartung erforderlich ist)
* Vehicles Health Statistics (Fahrzeugzustandsstatistik)

Sie können alle drei Echtzeitberichte konfigurieren oder den Vorgang nach jeder Phase beenden. Sie können dann mit dem nächsten Abschnitt zum Konfigurieren von Batchberichten fortfahren. Wir empfehlen, alle drei Berichte zu erstellen, um den Echtzeitpfad der Lösung in vollem Umfang zu visualisieren.  

### <a name="vehicles-in-operation-report"></a>Bericht „Vehicles in Operation“ (Fahrzeuge im Einsatz)
1. Doppelklicken Sie auf **Seite 1**, und benennen Sie sie in **Vehicles in Operation** (Fahrzeuge im Einsatz) um.

    ![Vehicles in Operation (Fahrzeuge im Einsatz)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

2. Wählen Sie auf der Registerkarte **Felder** den Eintrag **vin** aus. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Karte** aus.  

    Die Visualisierung **Karte** wird wie in der Abbildung gezeigt erstellt:

    ![„vin“ auswählen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

3. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

4. Wählen Sie auf der Registerkarte **Felder** die Einträge **city** und **vin** aus. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Karte** aus. Ziehen Sie **vin** in den Bereich **Werte**. Ziehen Sie **city** in den Bereich **Legende**. 

    ![Visualisierung „Karte“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

5. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Format** aus. Wählen Sie **Titel** aus, und ändern Sie **Text** in **Vehicles in operation by city** (Fahrzeuge im Einsatz nach Stadt).

    ![Vehicles in operation by city (Fahrzeuge im Einsatz nach Stadt)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

    Die fertige Visualisierung sieht wie im folgenden Beispiel aus:

    ![Fertige Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

6. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

7. Wählen Sie auf der Registerkarte **Felder** die Einträge **city** und **vin** aus. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Säulendiagramm (gruppiert)** aus. Ziehen Sie **city** in den Bereich **Achse**. Ziehen Sie **vin** in den Bereich **Wert**.

8. Sortieren Sie das Diagramm nach **Count of vin**.

    ![Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

9. Ändern Sie den **Titel** des Diagramms in **Vehicles in operation by city** (Fahrzeuge im Einsatz nach Stadt). 

10. Wählen Sie den Abschnitt **Format** und dann **Datenfarben** aus. Ändern Sie **Alle anzeigen** in **Ein**.

    ![Datenfarben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

11. Ändern Sie die Farbe einer einzelnen Stadt, indem Sie das Farbsymbol auswählen.

    ![Farbe ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

12. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

13. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Säulendiagramm (gruppiert)** aus. Ziehen Sie auf der Registerkarte **Felder** den Eintrag **city** in den Bereich **Achse**. Ziehen Sie **Model** in den Bereich **Legende**. Ziehen Sie **vin** in den Bereich **Wert**.

    ![Gruppiertes Säulendiagramm](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)

    Das Diagramm sieht in etwa wie folgt aus:

    ![Darstellung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

14. Ordnen Sie alle Visualisierungen so an, dass die Seite wie im folgenden Beispiel aussieht:

    ![Dashboard mit Visualisierungen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Sie haben den Echtzeitbericht „Vehicles in Operation“ (Fahrzeuge im Einsatz) erfolgreich konfiguriert. Sie können den nächsten Echtzeitbericht erstellen oder hier aufhören und das Dashboard konfigurieren. 

### <a name="vehicles-requiring-maintenance-report"></a>Bericht „Vehicles Requiring Maintenance“ (Fahrzeuge, für die die Wartung erforderlich ist)

1. Wählen Sie ![Hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) aus, um einen neuen Bericht hinzuzufügen. Benennen Sie ihn in **Vehicles Requiring Maintenance** (Fahrzeuge, für die die Wartung erforderlich ist) um.

    ![Vehicles Requiring Maintenance (Fahrzeuge, für die die Wartung erforderlich ist)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

2. Wählen Sie auf der Registerkarte **Felder** den Eintrag **vin** aus. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Karte** aus.

    ![Kartenvisualisierung für „vin“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

    Das Dataset enthält das Feld **MaintenanceLabel**. Dieses Feld kann den Wert „0“ oder „1“ aufweisen. Der Wert wird vom Machine Learning-Modell festgelegt, das als Teil der Lösung bereitgestellt wird. Er ist in den Echtzeitpfad integriert. Der Wert „1“ gibt an, dass ein Fahrzeug gewartet werden muss. 

3. So fügen Sie einen **Filter auf Seitenebene** hinzu, um Daten für die Fahrzeuge anzuzeigen, die gewartet werden müssen: 

   a. Ziehen Sie das Feld **MaintenanceLabel** in **Filter auf Seitenebene**.
  
      ![Filter auf Seitenebene](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)

    b. Wählen Sie unten in **Filter auf Seitenebene – MaintenanceLabel** die Option **Einfaches Filtern** aus.

      ![Einfaches Filtern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png) 

    c. Legen Sie den Filterwert auf **1** fest.

      ![Filterwert](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

4. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

5. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Säulendiagramm (gruppiert)** aus. 

    ![Karte für „vin“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)

    ![Gruppiertes Säulendiagramm](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

6. Ziehen Sie auf der Registerkarte **Felder** den Eintrag **Model** in den Bereich **Achse**. Ziehen Sie **vin** in den Bereich **Wert**. Sortieren Sie die Visualisierung dann nach **Count of vin**. Ändern Sie den **Titel** des Diagramms in **Vehicles requiring maintenance by model** (Fahrzeuge, für die die Wartung erforderlich ist, nach Modell). 

7. Ziehen Sie im Abschnitt **Felder** ![Felder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) der Registerkarte **Visualisierungen** den Eintrag **vin** zu **Farbsättigung**.

    ![Farbsättigung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

8. Ändern Sie im Abschnitt **Format** den Eintrag **Datenfarben** in der Visualisierung: 

    a. Ändern Sie die Farbe **Minimum** in **F2C812**.

    b. Ändern Sie die Farbe **Maximum** in **FF6300**.

    ![Neue Datenfarben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)

    Die neue Visualisierung sieht wie im folgenden Beispiel aus:

    ![Neue Visualisierungsfarben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

9. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

10. Wählen Sie auf der Registerkarte **Visualisierungen** die Option **Säulendiagramm (gruppiert)** aus. Ziehen Sie **vin** in den Bereich **Wert**. Ziehen Sie **city** in den Bereich **Achse**. Sortieren Sie das Diagramm nach **Count of vin**. Ändern Sie den **Titel** des Diagramms in **Vehicles requiring maintenance by city** (Fahrzeuge, für die die Wartung erforderlich ist, nach Stadt).

    ![Vehicles requiring maintenance by city (Fahrzeuge, für die die Wartung erforderlich ist, nach Stadt)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

11. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

12. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Mehrzeilige Zuordnung** aus. Ziehen Sie **Model** und **vin** in den Bereich **Felder**.

    ![Mehrzeilige Zuordnung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

13. Ordnen Sie alle Visualisierungen so an, dass der Bericht wie im folgenden Beispiel aussieht: 

    ![Neu angeordneter fertiger Bericht](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Sie haben den Echtzeitbericht „Vehicles Requiring Maintenance“ (Fahrzeuge, für die die Wartung erforderlich ist) erfolgreich konfiguriert. Sie können den nächsten Echtzeitbericht erstellen oder hier aufhören und das Dashboard konfigurieren. 

### <a name="vehicle-health-statistics-report"></a>Bericht „Vehicles Health Statistics“ (Fahrzeugzustandsstatistik)

1. Wählen Sie ![Hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) aus, um einen neuen Bericht hinzuzufügen. Benennen Sie ihn in **Vehicles Health Statistics** (Fahrzeugzustandsstatistik) um. 

2. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Maßstab** aus. Ziehen Sie **speed** in die Bereiche **Wert**, **Mindestwert** und **Maximalwert**.

   ![Vehicles Health Statistics (Fahrzeugzustandsstatistik)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

3. Ändern Sie die Standardaggregation **speed** im Bereich **Wert** in **Durchschnitt**.

4. Ändern Sie die Standardaggregation **speed** im Bereich **Mindestwert** in **Minimum**.

5. Ändern Sie die Standardaggregation **speed** im Bereich **Maximalwert** in **Maximum**.

   ![Geschwindigkeitswerte](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

6. Benennen Sie den Titel von **Maßstab** in **Average speed** um.

   ![Maßstab](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

7. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.  

    Fügen Sie auf gleiche Weise einen **Maßstab** für **Average engine oil**, **Average fuel** und **Average engine temperate** hinzu.  

8. Ändern Sie die Standardaggregation von Feldern in jedem Maßstab wie bei den zuvor für den Maßstab **Average speed** ausgeführten Schritten.

    ![Weitere Maßstäbe](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

9. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.

10. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Linien- und gruppiertes Säulendiagramm** aus. Ziehen Sie **city** auf **Gemeinsame Achse**. Ziehen Sie **tirepressure**, **engineoil** und **speed** in den Bereich **Spaltenwerte**. Ändern Sie den Aggregationstyp in **Durchschnitt**. 

11. Ziehen Sie **engineTemperature** in den Bereich **Zeilenwerte**. Ändern Sie den Aggregationstyp in **Durchschnitt**. 

    ![Spalten- und Zeilenwerte](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

12. Ändern Sie den **Titel** des Diagramms in **Average speed, tire pressure, engine oil and engine temperature** (Durchschnitt für Geschwindigkeit, Reifendruck, Motoröl und Motortemperatur).  

    ![Titel des Linien- und gruppierten Säulendiagramms](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

13. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.

14. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Treemap** aus. Ziehen Sie **Model** in den Bereich **Gruppe**. Ziehen Sie **MaintenanceProbability** in den Bereich **Werte**.

15. Ändern Sie den **Titel** des Diagramms in **Vehicle models requiring maintenance** (Fahrzeugmodelle, für die Wartung erforderlich ist).

    ![Treemap-Titel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

16. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.

17. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Gestapeltes Balkendiagramm (100 %)** aus. Ziehen Sie **city** in den Bereich **Achse**. Ziehen Sie **MaintenanceProbability** und **RecallProbability** in den Bereich **Wert**.

    ![Bereiche „Achse“ und „Wert“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

18. Wählen Sie im Abschnitt **Format** die Option **Datenfarben** aus. Legen Sie die Farbe für **MaintenanceProbability** auf den Wert **F2C80F** fest.

19. Ändern Sie den **Titel** des Diagramms in **Probability of Vehicle Maintenance & Recall by City** (Wahrscheinlichkeit für Fahrzeugwartung und -rückruf nach Ort).

    ![Titel des gestapelten Balkendiagramms (100 %)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

20. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.

21. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Flächendiagramm** aus. Ziehen Sie **Model** in den Bereich **Achse**. Ziehen Sie **engineOil**, **tirepressure**, **speed** und **MaintenanceProbability** in den Bereich **Werte**. Ändern Sie den Aggregationstyp in **Durchschnitt**. 

    ![Aggregationstyp](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

22. Ändern Sie den **Titel** des Diagramms in **Average engine oil, tire pressure, speed and maintenance probability by model** (Durchschnitt für Motoröl, Reifendruck, Geschwindigkeit und Wartungswahrscheinlichkeit nach Modell).

    ![Titel des Flächendiagramms](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

23. Wählen Sie den leeren Bereich aus, um eine neue Visualisierung hinzuzufügen.

24. Wählen Sie auf der Registerkarte **Visualisierungen** die Visualisierung **Punktdiagramm** aus. Ziehen Sie **Model** in die Bereiche **Details** und **Legende**. Ziehen Sie **fuel** in den Bereich **X-Achse**. Ändern Sie die Aggregation in **Durchschnitt**. Ziehen Sie **engineTemperature** in den Bereich **Y-Achse**. Ändern Sie die Aggregation in **Durchschnitt**. Ziehen Sie **vin** in den Bereich **Größe**.

    ![Bereiche „Details“, „Legende“, „Achse“ und „Größe“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

25. Ändern Sie den **Titel** des Diagramms in **Average of fuel, Average of engineTemperature, and Count of vin by Model and Model** (Durchschnittlicher Verbrauch, durchschnittliche Motortemperatur und Anzahl der Fahrzeugnummern nach Modell und Modell).

    ![Titel des Punktdiagramms](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

    Der fertige Bericht sieht in etwa wie im folgenden Beispiel aus:

    ![Fertiger Bericht](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Anheften von Visualisierungen der Berichte an das Echtzeitdashboard
1. Erstellen Sie ein leeres Dashboard. Klicken Sie dazu neben **Dashboards** auf das Pluszeichen. Geben als Namen **Vehicle Telemetry Analytics Dashboard** (Dashboard zur Fahrzeugtelemetrieanalyse) ein.

    ![Pluszeichen für Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

2. Heften Sie die Visualisierungen aus den vorherigen Berichten an das Dashboard an. 

    ![Dashboardsymbol zum Anheften](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

    Wenn alle drei Berichte an das Dashboard angeheftet wurden, sollte es wie im folgenden Beispiel aussehen. Ihr Dashboard sieht unter Umständen anders aus, wenn Sie nicht alle Berichte erstellt haben. 

    ![Dashboard mit Berichten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Sie haben das Echtzeit-Dashboard erfolgreich erstellt. Wenn Sie „CarEventGenerator.exe“ und „RealtimeDashboardApp.exe“ weiter ausführen, sehen Sie im Dashboard Live-Updates. Das Ausführen der folgenden Schritte dauert ungefähr 10 bis 15 Minuten.

## <a name="set-up-the-power-bi-batch-processing-dashboard"></a>Einrichten des Power BI-Dashboards für die Batchverarbeitung
> [!NOTE]
> Es dauert ca. zwei Stunden (ab dem erfolgreichen Abschluss der Bereitstellung), bis die Pipeline für die End-to-End-Batchverarbeitung die Ausführung beendet und generierte Daten für den Zeitraum eines Jahres verarbeitet hat. Warten Sie auf den Abschluss der Verarbeitung, bevor Sie die folgenden Schritte ausführen. 
> 
> 

### <a name="download-the-power-bi-designer-file"></a>Herunterladen der Power BI-Designerdatei

1. Eine vorkonfigurierte Power BI-Designerdatei ist in den Anweisungen für den manuellen Betrieb in der Bereitstellung enthalten. Suchen Sie nach „2. Einrichten des Power BI-Dashboards für die Batchverarbeitung“.

2. Sie können hier die Power BI-Vorlage mit dem Namen **ConnectedCarsPbiReport.pbix** für das Batchverarbeitungsdashboard herunterladen.

3. Speichern Sie sie lokal.

### <a name="configure-power-bi-reports"></a>Konfigurieren von Power BI-Berichten

1. Öffnen Sie mit Power BI Desktop die Designerdatei **ConnectedCarsPbiReport.pbix**. Falls noch nicht geschehen, installieren Sie Power BI Desktop von der [Installationswebsite für Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331).

2. Wählen Sie **Abfragen bearbeiten** aus.

    ![Abfragen bearbeiten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

3. Doppelklicken Sie auf **Quelle**.

    ![Quelle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

4. Aktualisieren Sie die Serververbindungszeichenfolge mit dem Azure SQL Server, der im Rahmen der Bereitstellung eingerichtet wurde. Suchen Sie in den Anweisungen für den manuellen Betrieb im Abschnitt zur Azure SQL-Datenbank folgende Einträge:

    * Server: somethingsrv.database.windows.net
    * Datenbank: connectedcar
    * Benutzername: username
    * Kennwort: Sie können Ihr SQL Server-Kennwort über das Azure-Portal verwalten.

5. Behalten Sie für **Datenbank** die Einstellung **connectedcar**bei.

    ![Datenbank](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

6. Klicken Sie auf **OK**.

7. Die Registerkarte **Windows-Anmeldeinformationen** ist standardmäßig ausgewählt. Ändern Sie die Option in **Datenbank-Anmeldeinformationen**, indem Sie rechts die Registerkarte **Datenbank** auswählen.

8. Geben Sie den **Benutzernamen** und das **Kennwort** Ihrer Azure SQL-Datenbank ein, die beim Einrichten der Bereitstellung angegeben wurde.

    ![Datenbank-Anmeldeinformationen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

9. Wählen Sie **Verbinden**aus.

10. Wiederholen Sie die vorherigen Schritte für jede der drei verbleibenden Abfragen im rechten Bereich. Aktualisieren Sie dann die Details der Datenquellenverbindung.

11. Klicken Sie auf **Close and Load**(Schließen und laden). Power BI Desktop-Dateidatasets sind mit SQL-Datenbanktabellen verbunden.

12. Wählen Sie **Close** (Schließen) aus, um die Power BI Desktop-Datei zu schließen.

    ![Close (Schließen)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

13. Klicken Sie zum Speichern der Änderungen auf **Speichern**. 

Sie haben jetzt alle Berichte konfiguriert, die dem Batchverarbeitungspfad in der Lösung entsprechen. 

## <a name="upload-to-powerbicom"></a>Hochladen auf powerbi.com
1. Navigieren Sie zum [Power BI-Webportal](http://powerbi.com), und melden Sie sich an.

2. Wählen Sie **Daten abrufen** aus.

3. Laden Sie die Power BI Desktop-Datei hoch. Wählen Sie **Daten abrufen** > **Dateien abrufen** > **Lokale Datei** aus.

4. Navigieren Sie zur Datei **ConnectedCarsPbiReport.pbix**.

5. Nachdem die Datei hochgeladen wurde, navigieren Sie zurück zu Ihrem Power BI-Arbeitsbereich. Ein Dataset, ein Bericht und ein leeres Dashboard werden für Sie erstellt.  

6. Heften Sie Diagramme an ein neues Dashboard mit dem Namen **Vehicle Telemetry Analytics Dashboard** (Dashboard zur Fahrzeugtelemetrieanalyse) in Power BI an. Wählen Sie das leere Dashboard aus, das zuvor erstellt wurde, und navigieren Sie zum Abschnitt **Berichte**. Wählen Sie den gerade hochgeladenen Bericht aus.  

    ![Neues Power BI-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

    Der Bericht hat sechs Seiten:

    Seite 1: Vehicle density  
    Seite 2: Real time vehicle health  
    Seite 3: Aggressively driven vehicles   
    Seite 4: Recalled vehicles  
    Seite 5: Fuel efficiently driven vehicles  
    Seite 6: Contoso Motors-Logo  

    ![Power BI-Bericht mit sechs Seiten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

7. Pinnen Sie von **Seite 3** folgende Inhalte an:  

    a. **Count of vin**  

   ![Seite 3 – Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png)

    b. **Aggressively driven vehicles by model (Aggressiv gefahrene Fahrzeuge nach Modell) – Wasserfalldiagramm** 

   ![Seite 3, Diagramm 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

8. Pinnen Sie von **Seite 5** folgende Inhalte an: 

    a. **Count of vin**

   ![Seite 5, Diagramm 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)

    b. **Fuel efficient vehicles by model (Kraftstoffeffiziente Fahrzeuge nach Modell) – Gruppiertes Säulendiagramm**

   ![Seite 5, Diagramm 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

9. Pinnen Sie von **Seite 4** folgende Inhalte an:  

    a. **Count of vin** 

   ![Seite 4, Diagramm 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

    b. **Recalled vehicles by city, model (Zurückgerufene Fahrzeuge nach Ort, Modell) – Treemap**

   ![Seite 4, Diagramm 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

10. Pinnen Sie von **Seite 6** folgende Inhalte an:  

    * **Contoso Motors-Logo**

    ![Contoso Motors-Logo ](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

### <a name="organize-the-dashboard"></a>Organisieren des Dashboards  

1. Wechseln Sie zum Dashboard.

2. Halten Sie den Mauszeiger auf die einzelnen Diagramme. Benennen Sie jedes Diagramm entsprechend den Namen im folgenden Beispiel für ein fertiges Dashboard um:

   ![Dashboard-Organisation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png) 
   
3. Ordnen Sie die Diagramme so an, dass sie wie im nachstehenden Beispiel-Dashboard aussehen:

    ![Neu angeordnetes Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)

4. Nachdem Sie alle in diesem Dokument beschriebenen Berichte erstellt haben, sieht das fertige Dashboard wie im folgenden Beispiel aus: 

   ![Fertiges Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Sie haben die Berichte und das Dashboard erfolgreich erstellt, um in Bezug auf den Fahrzeugzustand und das Fahrverhalten Informationen in den Bereichen Echtzeit, vorbeugende Wartung und Batchprozesse zu erhalten.  
