---
title: 'Tutorial: Einrichten einer Azure Time Series Insights Preview-Umgebung | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Ihre Umgebung in Azure Time Series Insights Preview einrichten.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 1b09c0e31b217d7d67f936aefe9045d190241389
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633112"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Einrichten einer Azure Time Series Insights Preview-Umgebung

Dieses Tutorial führt Sie durch das Erstellen einer Azure Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG). In diesem Tutorial lernen Sie Folgendes:

* Erstellen einer Azure Time Series Insights Preview-Umgebung.
* Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einem Event Hub in Azure Event Hubs.
* Ausführen eines Solution Accelerator-Beispiels zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
* Durchführen einer grundlegenden Analyse für die Daten
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und Zuordnen zu Ihren Instanzen

# <a name="create-a-device-simulation"></a>Erstellen einer Gerätesimulation

In diesem Abschnitt erstellen Sie drei simulierte Geräte, die Daten an einen IoT-Hub senden.

1. Öffnen Sie die Seite [Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators). Auf dieser Seite finden Sie mehrere vorgefertigte Beispiele. Melden Sie sich mit Ihrem Azure-Konto an. Wählen Sie dann **Gerätesimulation** aus.

   ![Seite „Azure IoT Solution Accelerators“][1]

   Wählen Sie **Jetzt testen** aus.

1. Geben Sie auf der Seite **Gerätesimulationslösung erstellen** die erforderlichen Parameter ein:

   | Parameter | Beschreibung |
   | --- | --- |
   | **Lösungsname** |    Geben Sie einen eindeutigen Wert für die Erstellung einer neuen Ressourcengruppe ein. Die aufgelisteten Azure-Ressourcen werden erstellt und der Ressourcengruppe zugewiesen. |
   | **Abonnement** | Geben Sie das Abonnement an, das Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung verwendet haben. |
   | **Region** |   Geben Sie die Region an, die Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung verwendet haben. |
   | **Optionale Azure-Ressourcen bereitstellen**    | Lassen Sie die Option „IoT Hub“ aktiviert, da sie von den simulierten Geräten für die Verbindungsherstellung und zum Streamen von Daten genutzt wird. |

   Wählen Sie dann **Lösung erstellen** aus. Warten Sie 10 bis 15 Minuten, bis Ihre Lösung bereitgestellt wurde.

   ![Seite zum Erstellen der Gerätesimulationslösung][2]

1. Wählen Sie auf Ihrem Solution Accelerator-Dashboard die Schaltfläche **Starten** aus:

   ![Starten der Gerätesimulationslösung][3]

1. Sie werden zur Seite **Microsoft Azure IoT-Gerätesimulation** weitergeleitet. Wählen Sie rechts oben auf der Seite die Option **+ Neue Simulation** aus.

   ![Azure IoT-Simulationsseite][4]

1.  Füllen Sie die erforderlichen Parameter wie folgt aus:

    ![Auszufüllender Parameter][5]

    |||
    | --- | --- |
    | **Name** | Geben Sie einen eindeutigen Namen für einen Simulator ein. |
    | **Beschreibung** | Geben Sie eine Definition ein. |
    | **Simulationsdauer** | Verwenden Sie die Option **Run indefinitely** (Unbegrenzt). |
    | **Gerätemodell** | **Name**: Geben Sie **Chiller** ein. </br>**Menge**: Geben Sie **3** ein. |
    | **IoT-Zielhub** | Verwenden Sie die Option **Use pre-provisioned IoT Hub** (Vorab bereitgestellten IoT-Hub verwenden). |

    Wählen Sie dann **Simulation starten** aus.

1. Auf dem Gerätesimulationsdashboard werden **Aktive Geräte** und **Nachrichten pro Sekunde** angezeigt.

    ![Azure IoT-Simulationsdashboard][6]

## <a name="list-device-simulation-properties"></a>Auflisten der Gerätesimulationseigenschaften

Für die Erstellung einer Azure Time Series Insights-Umgebung benötigen Sie den Namen Ihres IoT-Hubs, Ihres Abonnements und Ihrer Ressourcengruppe.

1. Wechseln Sie zum Solution Accelerator-Dashboard, und melden Sie sich mit dem gleichen Azure-Abonnementkonto an. Suchen Sie die Gerätesimulation, die Sie in den vorherigen Schritten erstellt haben.

1. Wählen Sie Ihren Gerätesimulator und anschließend **Starten** aus. Wählen Sie auf der rechten Seite den Link zum **Azure-Verwaltungsportal** aus.

    ![Simulatorauflistungen][7]

1. Notieren Sie sich die Namen von IoT-Hub, Abonnement und Ressourcengruppe.

    ![Azure-Portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Erstellen einer Time Series Insights Preview-PAYG-Umgebung

In diesem Abschnitt wird beschrieben, wie Sie eine Azure Time Series Insights Preview-Umgebung mit dem [Azure-Portal](https://portal.azure.com/) erstellen.

1. Melden Sie sich unter Verwendung Ihres Abonnementkontos beim Azure-Portal an.

1. Wählen Sie **Ressource erstellen**.

1. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.

   ![Auswählen von „Internet der Dinge“ und „Time Series Insights“][9]

1. Füllen Sie die Felder auf der Seite wie folgt aus:

   | | |
   | --- | ---|
   | **Umgebungsname** | Wählen Sie einen eindeutigen Namen für die Azure Time Series Insights Preview-Umgebung aus. |
   | **Abonnement** | Geben Sie Ihr Abonnement an, in dem Sie die Azure Time Series Insights Preview-Umgebung erstellen möchten. Es empfiehlt sich, das gleiche Abonnement zu verwenden wie für die übrigen vom Gerätesimulator erstellten IoT-Ressourcen. |
   | **Ressourcengruppe** | Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Wählen Sie eine vorhandene Ressourcengruppe für die Azure Time Series Insights Preview-Umgebungsressource aus, oder erstellen Sie eine neue. Es empfiehlt sich, die gleiche Ressourcengruppe zu verwenden wie für die übrigen vom Gerätesimulator erstellten IoT-Ressourcen. |
   | **Standort** | Wählen Sie eine Datencenterregion für Ihre Azure Time Series Insights Preview-Umgebung aus. Um zusätzliche Bandbreitenkosten und Latenz zu vermeiden, sollten Sie die Azure Time Series Insights Preview-Umgebung in derselben Region wie die anderen IoT-Ressourcen belassen. |
   | **Tier** |  Wählen Sie **PAYG** (Pay-As-You-Go; nutzungsbasierte Bezahlung) aus. Dies ist die SKU für das Azure Time Series Insights Preview-Produkt. |
   | **Eigenschafts-ID** | Geben Sie etwas ein, was Ihre Zeitreihe eindeutig identifiziert. Beachten Sie, dass dieses Feld unveränderlich ist und später nicht mehr geändert werden kann. Verwenden Sie für dieses Tutorial **iothub-connection-device-id**. Weitere Informationen zur Zeitreihen-ID finden Sie unter [Bewährte Methoden zur Auswahl einer Zeitreihen-ID](./time-series-insights-update-how-to-id.md). |
   | **Speicherkontoname** | Geben Sie einen global eindeutigen Namen für ein neu zu erstellendes Speicherkonto ein. |

   Wählen Sie anschließend **Weiter: Ereignisquelle** aus.

   ![Seite zum Erstellen einer Time Series Insights-Umgebung][10]

1. Füllen Sie die Felder auf der Seite für die Ereignisquelle wie folgt aus:

   | | |
   | --- | --- |
   | **Ereignisquelle erstellen?** | Geben Sie **Ja** ein.|
   | **Name** | Geben Sie einen eindeutigen Namenswert für die Ereignisquelle ein.|
   | **Quellentyp** | Geben Sie **IoT Hub** ein. |
   | **Hub auswählen** | Verwenden Sie die **Option zum**Auswählen eines vorhandenen Hubs. |
   | **Abonnement** | Geben Sie das Abonnement ein, das Sie für den Gerätesimulator verwendet haben. |
   | **IoT Hub-Name** | Geben Sie den Namen des IoT-Hubs ein, den Sie für den Gerätesimulator erstellt haben. |
   | **Zugriffsrichtlinie für IoT Hub** | Geben Sie **iothubowner** ein. |
   | **IoT Hub-Consumergruppe** | Sie benötigen eine eindeutige Consumergruppe für Azure Time Series Insights Preview. Wählen Sie **Neu** aus, geben Sie einen eindeutigen Namen ein, und wählen Sie dann **Hinzufügen** aus. |
   | **Timestamp property** (Timestamp-Eigenschaft) | Mit diesem Feld wird die Zeitstempel-Eigenschaft in Ihren eingehenden Telemetriedaten identifiziert. Lassen das Feld in diesem Tutorial leer. Dieser Simulator verwendet den aus IoT Hub eingehenden Zeitstempel, der standardmäßig von Time Series Insights verwendet wird.|

   Wählen Sie **Überprüfen + erstellen** aus.

   ![Seite zum Einrichten einer Ereignisquelle][13]

1. Überprüfen Sie alle Felder auf der Überprüfungsseite, und wählen Sie anschließend **Erstellen** aus.

   ![Seite „Überprüfen + erstellen“ mit Schaltfläche „Erstellen“][14]

1. Sie können den Status Ihrer Bereitstellung sehen.

   ![Benachrichtigung, dass die Bereitstellung abgeschlossen ist][15]

1. Wenn Sie Besitzer des Mandanten sind, sollten Sie auf Ihre Azure Time Series Insights Preview-Umgebung zugreifen können. So stellen Sie sicher, dass Sie über Zugriff verfügen:

   a. Suchen Sie nach Ihrer Ressourcengruppe, und wählen Sie Ihre Azure Time Series Insights Preview-Umgebung aus:

      ![Ausgewählte Umgebung][16]

   b. Navigieren Sie auf der Seite „Azure Time Series Insights Preview“ zu **Datenzugriffsrichtlinien**.

     ![Datenzugriffsrichtlinien][17]

   c. Stellen Sie sicher, dass Ihre Anmeldeinformationen aufgelistet werden.

     ![Aufgeführte Anmeldeinformationen][18]

   Sollten Ihre Anmeldeinformationen nicht aufgelistet sein, müssen Sie sich selbst die Erlaubnis zum Zugriff auf die Umgebung geben. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [Gewähren von Datenzugriff für eine Umgebung](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Analysieren von Daten in Ihrer Umgebung

In diesem Abschnitt führen Sie mit dem [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) grundlegende Analysen für Ihre Zeitreihendaten durch.

1. Navigieren Sie zu Ihrem Azure Time Series Insights Preview-Explorer, indem Sie im [Azure-Portal](https://portal.azure.com/) auf der Ressourcenseite die URL auswählen.

   ![Die URL des Time Series Insights Preview-Explorers][19]

1. Wählen Sie im Explorer den Knoten **Instanzen ohne übergeordnete Elemente** aus, um die gesamten Azure Time Series Insights Preview-Instanzen in der Umgebung anzuzeigen.

   ![Liste von Instanzen ohne übergeordnete Elemente][20]

1. Wählen Sie in der angezeigten Zeitreihe die erste Instanz aus. Wählen Sie dann **Show Avg pressure** (Durchschnittlichen Druck anzeigen) aus.

   ![Ausgewählte Instanz mit Menübefehl zum Anzeigen des durchschnittlichen Drucks][21]

   Ein Zeitreihendiagramm sollte auf der rechten Seite angezeigt werden:

   ![Zeitreihendiagramm][22]

1. Wiederholen Sie Schritt 3 mit den anderen beiden Zeitreihen. Anschließend können Sie alle Zeitreihen anzeigen, wie im folgenden Diagramm zu sehen:

   ![Diagramm mit allen Zeitreihen][23]

1. Ändern Sie den Zeitbereich, um Zeitreihentrends innerhalb der letzten Stunde anzuzeigen. 

   a. Wählen Sie das Optionsfeld **Von** aus:

      ![Optionsfeld „Von“][24]

   b. Ändern Sie die Zeit im Feld, um Ereignisse der letzten Stunde anzuzeigen:

      ![Zeitanpassung][25]

1. Anschließend können Sie den Druck aller drei Geräte in der letzten Stunde vergleichen:

   ![Vergleich dreier Geräte][26]

## <a name="define-and-apply-a-model"></a>Definieren und Anwenden eines Modells

In diesem Abschnitt wenden Sie ein Modell zum Strukturieren der Daten an. Um das Modell zu vervollständigen, definieren Sie Typen, Hierarchien und Instanzen. Weitere Informationen zur Datenmodellierung finden Sie unter [Time Series-Modell](./time-series-insights-update-tsm.md).

1. Wählen Sie im Explorer die Registerkarte **Modell** aus:

   ![Registerkarte „Modell“ im Explorer][27]

1. Wählen Sie **+ Hinzufügen** aus, um einen Typ hinzuzufügen. Auf der rechten Seite wird ein Typ-Editor geöffnet.

   ![Schaltfläche „Hinzufügen“ für Typen][28]

1. Definieren Sie drei Variablen für den Typ: Druck, Temperatur und Luftfeuchtigkeit. Geben Sie Folgendes ein:

   | | |
   | --- | ---|
   | **Name** | Geben Sie **Chiller** ein. |
   | **Beschreibung** | Geben Sie **This is a type definition of Chiller** ein. |

   * Definieren Sie nun den Druck mit drei Variablen:

      | | |
      | --- | ---|
      | **Name** | Geben Sie **Avg Pressure** ein. |
      | **Wert** | Wählen Sie **pressure (Double) (Druck (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, nachdem Azure Time Series Insights Preview damit beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie **AVG** aus. |

      ![Auswahloptionen zum Definieren des Drucks][29]

      Wählen Sie **+Variable hinzufügen** aus, um die nächste Variable hinzuzufügen.

   * Definieren Sie die Temperatur:

      | | |
      | --- | ---|
      | **Name** | Geben Sie **Avg Temperature** ein. |
      | **Wert** | Wählen Sie **temperature (Double) (Temperatur (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, nachdem Azure Time Series Insights Preview damit beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie **AVG** aus.|

      ![Auswahloptionen zum Definieren der Temperatur][30]

   * Definieren Sie die Luftfeuchtigkeit:

      | | |
      | --- | ---|
      | **Name** | Geben Sie **Max Humidity** ein. |
      | **Wert** | Wählen Sie **humidity (Double) (Luftfeuchtigkeit (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, nachdem Azure Time Series Insights Preview damit beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie **MAX** aus.|

      ![Auswahloptionen zum Definieren der Temperatur][31]

   Wählen Sie anschließend **Erstellen**.

1. Ihr hinzugefügter Typ wird angezeigt:

   ![Informationen zum hinzugefügten Typ][32]

1. Im nächsten Schritt wird eine Hierarchie hinzugefügt. Wählen Sie im Abschnitt **Hierarchien** die Option **+ Hinzufügen** aus:

   ![Registerkarte „Hierarchien“ mit Schaltfläche „Hinzufügen“][33]

1. Definieren Sie die Hierarchie. Füllen Sie die Felder wie folgt aus:

   | | |
   | --- | ---|
   | **Name** | Geben Sie **Location Hierarchy** ein. |
   | **Ebene 1** | Geben Sie **Land** ein. |
   | **Ebene 2** | Geben Sie **Ort** ein. |
   | **Ebene 3** | Geben Sie **Gebäude** ein. |

   Wählen Sie anschließend **Erstellen**.

   ![Hierarchiefelder mit Schaltfläche „Erstellen“][34]

1. Die erstellte Hierarchie wird angezeigt:

   ![Informationen zur Hierarchie][35]

1. Wählen Sie auf der linken Seite die Option **Instanzen** aus. Wenn die Instanzen angezeigt werden, wählen Sie die erste Instanz und anschließend **Bearbeiten** aus:

   ![Auswählen der Schaltfläche „Bearbeiten“ für eine Instanz][36]

1. Auf der rechten Seite wird ein Text-Editor angezeigt. Fügen Sie die folgenden Informationen hinzu:

   | | |
   | --- | --- |
   | **Typ** | Wählen Sie **Chiller** aus. |
   | **Beschreibung** | Geben Sie **Instance for Chiller-01.1** ein. |
   | **Hierarchien** | Aktivieren Sie **Location Hierarchy**. |
   | **Country** | Geben Sie **USA** ein. |
   | **City (Ort)** | Geben Sie **Seattle** ein. |
   | **Building (Gebäude)** | Geben Sie **Space Needle** ein. |

    Wählen Sie anschließend **Speichern** aus.

   ![Instanzfelder mit Schaltfläche „Speichern“][37]

1. Wiederholen Sie den vorherigen Schritt für die anderen Sensoren. Verwenden Sie die folgenden Felder:

   * Für Chiller 01.2:

     | | |
     | --- | --- |
     | **Typ** | Wählen Sie **Chiller** aus. |
     | **Beschreibung** | Geben Sie **Instance for Chiller-01.2** ein. |
     | **Hierarchien** | Aktivieren Sie **Location Hierarchy**. |
     | **Country** | Geben Sie **USA** ein. |
     | **City (Ort)** | Geben Sie **Seattle** ein. |
     | **Building (Gebäude)** | Geben Sie **Pacific Science Center** ein. |

   * Für Chiller 01.3:

     | | |
     | --- | --- |
     | **Typ** | Wählen Sie **Chiller** aus. |
     | **Beschreibung** | Geben Sie **Instance for Chiller-01.1** ein. |
     | **Hierarchien** | Aktivieren Sie **Location Hierarchy**. |
     | **Country** | Geben Sie **USA** ein. |
     | **City (Ort)** | Geben Sie **New York** ein. |
     | **Building (Gebäude)** | Geben Sie **Empire State Building** ein. |

1. Wechseln Sie zur Registerkarte **Analysieren**, und aktualisieren Sie die Seite. Erweitern Sie alle Hierarchieebenen, um die Zeitreihe zu finden.

   ![Registerkarte „Analysieren“][38]

1. Um die Zeitreihen der letzten Stunde zu untersuchen, ändern Sie **Kurze Zeitangaben** in **Letzte Stunde**:

   ![Feld „Kurze Zeitangaben“ mit der Option „Letzte Stunde“][39]

1. Wählen Sie die Zeitreihe unter **Pacific Science Center** und anschließend **Show Max Humidity** (Max. Luftfeuchtigkeit anzeigen) aus.

   ![Ausgewählte Zeitreihe mit Menüauswahl „Show Max Humidity“ (Max. Luftfeuchtigkeit anzeigen)][40]

1. Die Zeitreihe für **Max Humidity** (Max. Luftfeuchtigkeit) mit einer Intervallgröße von einer Minute wird geöffnet. Wählen Sie eine Region aus, um einen Bereich zu filtern. Klicken Sie dann mit der rechten Maustaste, und wählen Sie **Zoom** aus, um Ereignisse innerhalb des Zeitrahmens zu analysieren:

   ![Ausgewählter Bereich mit Zoombefehl in einem Kontextmenü][41]

1. Sie können auch eine Region auswählen und dann mit der rechten Maustaste klicken, um die Ereignisdetails anzuzeigen:

   ![Detaillierte Ereignisliste][44]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:  

* Erstellen und Verwenden eines Accelerators zur Gerätesimulation.
* Erstellen einer Azure Time Series Insights Preview-PAYG-Umgebung.
* Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einer Event Hub-Instanz.
* Ausführen eines Solution Accelerator-Beispiels zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
* Durchführen einer grundlegenden Analyse der Daten.
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und deren Zuordnung zu Ihren Instanzen.

Da Sie nun Ihre eigene Azure Time Series Insights Preview-Umgebung erstellen können, erfahren Sie mehr über die wichtigsten Konzepte in Azure Time Series Insights.

Informieren Sie sich über die Azure Time Series Insights-Speicherkonfiguration:

> [!div class="nextstepaction"]
> [Speicherung und Datenerfassung in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md)

Erfahren Sie mehr über Zeitreihenmodelle:

> [!div class="nextstepaction"]
> [Datenmodellierung in Azure Time Series Insights](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/device-one-accelerator.png
[2]: media/v2-update-provision/device-two-create.png
[3]: media/v2-update-provision/device-three-launch.png
[4]: media/v2-update-provision/device-four-iot-sim-page.png
[5]: media/v2-update-provision/device-five-params.png
[6]: media/v2-update-provision/device-seven-dashboard.png
[7]: media/v2-update-provision/device-six-listings.png
[8]: media/v2-update-provision/device-eight-portal.png

[9]: media/v2-update-provision/payg-one-azure.png
[10]: media/v2-update-provision/payg-two-create.png
[11]: media/v2-update-provision/payg-three-new.png
[12]: media/v2-update-provision/payg-four-add.png
[13]: media/v2-update-provision/payg-five-event-source.png
[14]: media/v2-update-provision/payg-six-review.png
[15]: media/v2-update-provision/payg-seven-deploy.png
[16]: media/v2-update-provision/payg-eight-environment.png
[17]: media/v2-update-provision/payg-nine-data-access.png
[18]: media/v2-update-provision/payg-ten-verify.png

[19]: media/v2-update-provision/analyze-one-portal.png
[20]: media/v2-update-provision/analyze-two-unparented.png
[21]: media/v2-update-provision/analyze-three-show-pressure.png
[22]: media/v2-update-provision/analyze-four-chart.png
[23]: media/v2-update-provision/analyze-five-chart.png
[24]: media/v2-update-provision/analyze-six-from.png
[25]: media/v2-update-provision/analyze-seven-change-from.png
[26]: media/v2-update-provision/analyze-eight-all.png

[27]: media/v2-update-provision/define-one-model.png
[28]: media/v2-update-provision/define-two-add.png
[29]: media/v2-update-provision/define-three-variable.png
[30]: media/v2-update-provision/define-four-avg.png
[31]: media/v2-update-provision/define-five-humidity.png
[32]: media/v2-update-provision/define-six-type.png
[33]: media/v2-update-provision/define-seven-hierarchy.png
[34]: media/v2-update-provision/define-eight-add-hierarchy.png
[35]: media/v2-update-provision/define-nine-created.png
[36]: media/v2-update-provision/define-ten-edit.png
[37]: media/v2-update-provision/define-eleven-chiller.png
[38]: media/v2-update-provision/define-twelve.png
[39]: media/v2-update-provision/define-thirteen-explore.png
[40]: media/v2-update-provision/define-fourteen-show-max.png
[41]: media/v2-update-provision/define-fifteen-filter.png
[42]: media/v2-update-provision/define-sixteen.png
[43]: media/v2-update-provision/define-seventeen.png
[44]: media/v2-update-provision/define-eighteen.png