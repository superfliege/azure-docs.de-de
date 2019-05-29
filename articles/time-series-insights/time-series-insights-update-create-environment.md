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
ms.date: 04/25/2019
ms.custom: seodec18
ms.openlocfilehash: f83063a88207f51f9d481447923fd8a8498692a2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713914"
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

   [![Seite „Azure IoT Solution Accelerators“](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Wählen Sie **Jetzt testen** aus.

1. Geben Sie auf der Seite **Gerätesimulationslösung erstellen** die erforderlichen Parameter ein:

   | Parameter | BESCHREIBUNG |
   | --- | --- |
   | **Lösungsname** |    Geben Sie einen eindeutigen Wert für die Erstellung einer neuen Ressourcengruppe ein. Die aufgelisteten Azure-Ressourcen werden erstellt und der Ressourcengruppe zugewiesen. |
   | **Abonnement** | Geben Sie das Abonnement an, das Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung verwendet haben. |
   | **Region** |   Geben Sie die Region an, die Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung verwendet haben. |
   | **Optionale Azure-Ressourcen bereitstellen**    | Lassen Sie die Option „IoT Hub“ aktiviert, da sie von den simulierten Geräten für die Verbindungsherstellung und zum Streamen von Daten genutzt wird. |

   Wählen Sie dann **Lösung erstellen** aus. Warten Sie 10 bis 15 Minuten, bis Ihre Lösung bereitgestellt wurde.

   [![Seite zum Erstellen der Gerätesimulationslösung](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. Wählen Sie auf Ihrem Solution Accelerator-Dashboard die Schaltfläche **Starten** aus:

   [![Starten der Gerätesimulationslösung](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. Sie werden zur Seite **Microsoft Azure IoT-Gerätesimulation** weitergeleitet. Wählen Sie rechts oben auf der Seite die Option **+ Neue Simulation** aus.

   [![Azure IoT-Simulationsseite](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. Füllen Sie die erforderlichen Parameter wie folgt aus:

    [![Auszufüllender Parameter](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    |||
    | --- | --- |
    | **Name** | Geben Sie einen eindeutigen Namen für einen Simulator ein. |
    | **Beschreibung** | Geben Sie eine Definition ein. |
    | **Simulationsdauer** | Verwenden Sie die Option **Run indefinitely** (Unbegrenzt). |
    | **Gerätemodell** | **Name**: Geben Sie `Chiller` ein. </br>**Menge**: Geben Sie `3` ein. |
    | **IoT-Zielhub** | Verwenden Sie die Option **Use pre-provisioned IoT Hub** (Vorab bereitgestellten IoT-Hub verwenden). |

    Wählen Sie dann **Simulation starten** aus.

1. Auf dem Gerätesimulationsdashboard werden **Aktive Geräte** und **Nachrichten pro Sekunde** angezeigt.

    [![Azure IoT-Simulationsdashboard](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="list-device-simulation-properties"></a>Auflisten der Gerätesimulationseigenschaften

Für die Erstellung einer Azure Time Series Insights-Umgebung benötigen Sie den Namen Ihres IoT-Hubs, Ihres Abonnements und Ihrer Ressourcengruppe.

1. Wechseln Sie zum Solution Accelerator-Dashboard, und melden Sie sich mit dem gleichen Azure-Abonnementkonto an. Suchen Sie die Gerätesimulation, die Sie in den vorherigen Schritten erstellt haben.

1. Wählen Sie Ihren Gerätesimulator und anschließend **Starten** aus. Wählen Sie auf der rechten Seite den Link zum **Azure-Verwaltungsportal** aus.

    [![Simulatorauflistungen](media/v2-update-provision/device-six-listings.png)](media/v2-update-provision/device-six-listings.png#lightbox)

1. Notieren Sie sich die Namen von IoT-Hub, Abonnement und Ressourcengruppe.

    [![Azure-Portal](media/v2-update-provision/device-eight-portal.png)](media/v2-update-provision/device-eight-portal.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Erstellen einer Time Series Insights Preview-PAYG-Umgebung

In diesem Abschnitt wird beschrieben, wie Sie eine Azure Time Series Insights Preview-Umgebung mit dem [Azure-Portal](https://portal.azure.com/) erstellen.

1. Melden Sie sich unter Verwendung Ihres Abonnementkontos beim Azure-Portal an.

1. Wählen Sie **Ressource erstellen**.

1. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.

   [![„Internet der Dinge“ auswählen und dann „Time Series Insights“](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. Füllen Sie die Felder auf der Seite wie folgt aus:

   | | |
   | --- | ---|
   | **Umgebungsname** | Wählen Sie einen eindeutigen Namen für die Azure Time Series Insights Preview-Umgebung aus. |
   | **Abonnement** | Geben Sie Ihr Abonnement an, in dem Sie die Azure Time Series Insights Preview-Umgebung erstellen möchten. Es empfiehlt sich, das gleiche Abonnement zu verwenden wie für die übrigen vom Gerätesimulator erstellten IoT-Ressourcen. |
   | **Ressourcengruppe** | Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Wählen Sie eine vorhandene Ressourcengruppe für die Azure Time Series Insights Preview-Umgebungsressource aus, oder erstellen Sie eine neue. Es empfiehlt sich, die gleiche Ressourcengruppe zu verwenden wie für die übrigen vom Gerätesimulator erstellten IoT-Ressourcen. |
   | **Location** | Wählen Sie eine Datencenterregion für Ihre Azure Time Series Insights Preview-Umgebung aus. Um zusätzliche Bandbreitenkosten und Latenz zu vermeiden, sollten Sie die Azure Time Series Insights Preview-Umgebung in derselben Region wie die anderen IoT-Ressourcen belassen. |
   | **Tier** |  Wählen Sie **PAYG** (Pay-As-You-Go; nutzungsbasierte Bezahlung) aus. Dies ist die SKU für das Azure Time Series Insights Preview-Produkt. |
   | **Eigenschafts-ID** | Geben Sie etwas ein, was Ihre Zeitreihe eindeutig identifiziert. Beachten Sie, dass dieses Feld unveränderlich ist und später nicht mehr geändert werden kann. Verwenden Sie für dieses Tutorial `iothub-connection-device-id`. Weitere Informationen zur Zeitreihen-ID finden Sie unter [Bewährte Methoden zur Auswahl einer Zeitreihen-ID](./time-series-insights-update-how-to-id.md). |
   | **Speicherkontoname** | Geben Sie einen global eindeutigen Namen für ein neu zu erstellendes Speicherkonto ein. |

   Wählen Sie anschließend **Weiter: Ereignisquelle**.

   [![Seite zum Erstellen einer Time Series Insights-Umgebung](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. Füllen Sie die Felder auf der Seite für die Ereignisquelle wie folgt aus:

   | | |
   | --- | --- |
   | **Ereignisquelle erstellen?** | Geben Sie `Yes` ein.|
   | **Name** | Geben Sie einen eindeutigen Namenswert für die Ereignisquelle ein.|
   | **Quellentyp** | Wählen Sie **IoT Hub** aus. |
   | **Hub auswählen** | Wählen Sie **Vorhandene auswählen** aus. |
   | **Abonnement** | Wählen Sie das Abonnement aus, das Sie für den Gerätesimulator verwendet haben. |
   | **IoT Hub-Name** | Wählen Sie den Namen des IoT-Hubs aus, den Sie für den Gerätesimulator erstellt haben. |
   | **Zugriffsrichtlinie für IoT Hub** | Wählen Sie **iothubowner** aus. |
   | **IoT Hub-Consumergruppe** | Sie benötigen eine eindeutige Consumergruppe für Azure Time Series Insights Preview. Wählen Sie **Neu** aus, geben Sie einen eindeutigen Namen ein, und wählen Sie dann **Hinzufügen** aus. |
   | **Timestamp property** (Timestamp-Eigenschaft) | Mit diesem Feld wird die Zeitstempel-Eigenschaft in Ihren eingehenden Telemetriedaten identifiziert. Lassen das Feld in diesem Tutorial leer. Dieser Simulator verwendet den aus IoT Hub eingehenden Zeitstempel, der standardmäßig von Time Series Insights verwendet wird.|

   Wählen Sie **Überprüfen + erstellen** aus.

   [![Konfigurieren einer Ereignisquelle](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Überprüfen Sie alle Felder auf der Überprüfungsseite, und wählen Sie anschließend **Erstellen** aus.

   [![Seite „Überprüfen + erstellen“ mit Schaltfläche „Erstellen“](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

1. Sie können den Status Ihrer Bereitstellung sehen.

   [![Benachrichtigung, dass die Bereitstellung abgeschlossen ist](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Wenn Sie Besitzer des Mandanten sind, sollten Sie auf Ihre Azure Time Series Insights Preview-Umgebung zugreifen können. So stellen Sie sicher, dass Sie über Zugriff verfügen:

   a. Suchen Sie nach Ihrer Ressourcengruppe, und wählen Sie Ihre Azure Time Series Insights Preview-Umgebung aus:

      [![Ausgewählte Umgebung](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   b. Navigieren Sie auf der Seite „Azure Time Series Insights Preview“ zu **Datenzugriffsrichtlinien**.

     [![Datenzugriffsrichtlinien](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   c. Stellen Sie sicher, dass Ihre Anmeldeinformationen aufgelistet werden.

     [![Aufgeführte Anmeldeinformationen](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Sollten Ihre Anmeldeinformationen nicht aufgelistet sein, müssen Sie sich selbst die Erlaubnis zum Zugriff auf die Umgebung geben. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [Gewähren von Datenzugriff für eine Umgebung](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Analysieren von Daten in Ihrer Umgebung

In diesem Abschnitt führen Sie mit dem [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) grundlegende Analysen für Ihre Zeitreihendaten durch.

1. Navigieren Sie zu Ihrem Azure Time Series Insights Preview-Explorer, indem Sie im [Azure-Portal](https://portal.azure.com/) auf der Ressourcenseite die URL auswählen.

   [![Die URL des Time Series Insights Preview-Explorers](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. Wählen Sie im Explorer den Knoten **Time Series-Instanzen**  aus, um die gesamten Azure Time Series Insights Preview-Instanzen in der Umgebung anzuzeigen.

   [![Liste von Instanzen ohne übergeordnete Elemente](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Wählen Sie in der angezeigten Zeitreihe die erste Instanz aus. Wählen Sie dann **Show Avg pressure** (Durchschnittlichen Druck anzeigen) aus.

   [![Ausgewählte Instanz mit Menübefehl zum Anzeigen des durchschnittlichen Drucks](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

   Ein Zeitreihendiagramm sollte auf der rechten Seite angezeigt werden. Passen Sie das **Intervall** auf `15s` an.

   [![Zeitreihendiagramm](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Wiederholen Sie **Schritt 3** mit den anderen beiden Zeitreihen. Anschließend können Sie alle Zeitreihen anzeigen, wie im folgenden Diagramm zu sehen:

   [![Diagramm mit allen Zeitreihen](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Ändern Sie den Zeitbereich, um Zeitreihentrends innerhalb der letzten Stunde anzuzeigen.

   a. Aktivieren Sie das Optionsfeld **Zeitrahmen**:

      [![Legen Sie den Zeitbereich auf eine Stunde fest](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Definieren und Anwenden eines Modells

In diesem Abschnitt wenden Sie ein Modell zum Strukturieren der Daten an. Um das Modell zu vervollständigen, definieren Sie Typen, Hierarchien und Instanzen. Weitere Informationen zur Datenmodellierung finden Sie unter [Time Series-Modell](./time-series-insights-update-tsm.md).

1. Wählen Sie im Explorer die Registerkarte **Modell** aus:

   [![Registerkarte „Modell“ im Explorer](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Wählen Sie **+ Hinzufügen** aus, um einen Typ hinzuzufügen. Auf der rechten Seite wird ein Typ-Editor geöffnet.

   [![Schaltfläche „Hinzufügen“ für Typen](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Definieren Sie drei Variablen für den Typ: Druck, Temperatur und Luftfeuchtigkeit. Geben Sie Folgendes ein:

   | | |
   | --- | ---|
   | **Name** | Geben Sie `Chiller` ein. |
   | **Beschreibung** | Geben Sie `This is a type definition of Chiller` ein. |

   * Definieren Sie nun den Druck mit drei Variablen:

      | | |
      | --- | ---|
      | **Name** | Geben Sie `Avg Pressure` ein. |
      | **Wert** | Wählen Sie **pressure (Double) (Druck (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, nachdem Azure Time Series Insights Preview damit beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie **AVG** aus. |

      [![Auswahloptionen zum Definieren des Drucks](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Wählen Sie **+Variable hinzufügen** aus, um die nächste Variable hinzuzufügen.

   * Definieren Sie die Temperatur:

      | | |
      | --- | ---|
      | **Name** | Geben Sie `Avg Temperature` ein. |
      | **Wert** | Wählen Sie **temperature (Double) (Temperatur (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, nachdem Azure Time Series Insights Preview damit beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie **AVG** aus.|

      [![Auswahloptionen zum Definieren der Temperatur](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

   * Definieren Sie die Luftfeuchtigkeit:

      | | |
      | --- | ---|
      | **Name** | Geben Sie `Max Humidity` ein. |
      | **Wert** | Wählen Sie **humidity (Double) (Luftfeuchtigkeit (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, nachdem Azure Time Series Insights Preview damit beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie **MAX** aus.|

      [![Auswahloptionen zum Definieren der Temperatur](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

   Wählen Sie anschließend **Erstellen**.

1. Ihr hinzugefügter Typ wird angezeigt:

   [![Informationen zum hinzugefügten Typ](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. Im nächsten Schritt wird eine Hierarchie hinzugefügt. Wählen Sie im Abschnitt **Hierarchien** die Option **+ Hinzufügen** aus:

   [![Registerkarte „Hierarchien“ mit Schaltfläche „Hinzufügen“](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. Definieren Sie die Hierarchie. Füllen Sie die Felder wie folgt aus:

   | | |
   | --- | ---|
   | **Name** | Geben Sie `Location Hierarchy` ein. |
   | **Ebene 1** | Geben Sie `Country` ein. |
   | **Ebene 2** | Geben Sie `City` ein. |
   | **Ebene 3** | Geben Sie `Building` ein. |

   Wählen Sie anschließend **Erstellen**.

   [![Hierarchiefelder mit Schaltfläche „Erstellen“](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

1. Die erstellte Hierarchie wird angezeigt:

   [![Informationen zur Hierarchie](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Wählen Sie auf der linken Seite die Option **Instanzen** aus. Wenn die Instanzen angezeigt werden, wählen Sie die erste Instanz und anschließend **Bearbeiten** aus:

   [![Auswählen der Schaltfläche „Bearbeiten“ für eine Instanz](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. Auf der rechten Seite wird ein Text-Editor angezeigt. Fügen Sie die folgenden Informationen hinzu:

   | | |
   | --- | --- |
   | **Typ** | Wählen Sie **Chiller** aus. |
   | **Beschreibung** | Geben Sie `Instance for Chiller-01.1` ein. |
   | **Hierarchien** | Wählen Sie **Location Hierarchy** aus. |
   | **Country** | Geben Sie `USA` ein. |
   | **City (Ort)** | Geben Sie `Seattle` ein. |
   | **Building (Gebäude)** | Geben Sie `Space Needle` ein. |

    Wählen Sie anschließend **Speichern** aus.

   [![Instanzfelder mit Schaltfläche „Speichern“](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

1. Wiederholen Sie den vorherigen Schritt für die anderen Sensoren. Verwenden Sie die folgenden Felder:

   * Für Chiller 01.2:

     | | |
     | --- | --- |
     | **Typ** | Wählen Sie **Chiller** aus. |
     | **Beschreibung** | Geben Sie `Instance for Chiller-01.2` ein. |
     | **Hierarchien** | Wählen Sie **Location Hierarchy** aus. |
     | **Country** | Geben Sie `USA` ein. |
     | **City (Ort)** | Geben Sie `Seattle` ein. |
     | **Building (Gebäude)** | Geben Sie `Pacific Science Center` ein. |

   * Für Chiller 01.3:

     | | |
     | --- | --- |
     | **Typ** | Wählen Sie **Chiller** aus. |
     | **Beschreibung** | Geben Sie `Instance for Chiller-01.3` ein. |
     | **Hierarchien** | Wählen Sie **Location Hierarchy** aus. |
     | **Country** | Geben Sie `USA` ein. |
     | **City (Ort)** | Geben Sie `New York` ein. |
     | **Building (Gebäude)** | Geben Sie `Empire State Building` ein. |

1. Wechseln Sie zur Registerkarte **Analysieren**, und aktualisieren Sie die Seite. Erweitern Sie alle Hierarchieebenen, um die Zeitreihe zu finden.

   [![Registerkarte „Analysieren“](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Um die Zeitreihen der letzten Stunde zu untersuchen, ändern Sie **Kurze Zeitangaben** in **Letzte Stunde**:

    [![Feld „Kurze Zeitangaben“ mit ausgewählter Option „Letzte Stunde“](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Wählen Sie die Zeitreihe unter **Pacific Science Center** und anschließend **Show Max Humidity** (Max. Luftfeuchtigkeit anzeigen) aus.

    [![Ausgewählte Zeitreihe mit Menüauswahl „Show Max Humidity“ (Max. Luftfeuchtigkeit anzeigen)](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. Die Zeitreihe für **Max Humidity** (Max. Luftfeuchtigkeit) mit einer Intervallgröße von **1 Minute** wird geöffnet. Wählen Sie eine Region aus, um einen Bereich zu filtern. Klicken Sie dann mit der rechten Maustaste, und wählen Sie **Zoom** aus, um Ereignisse innerhalb des Zeitrahmens zu analysieren:

   [![Ausgewählter Bereich mit Zoombefehl in einem Kontextmenü](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. Sie können auch eine Region auswählen und dann mit der rechten Maustaste klicken, um die Ereignisdetails anzuzeigen:

   [![Detaillierte Ereignisliste](media/v2-update-provision/define-eighteen.png)](media/v2-update-provision/define-eighteen.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:  

> [!div class="checklist"]
> * Erstellen und Verwenden eines Accelerators zur Gerätesimulation.
> * Erstellen einer Azure Time Series Insights Preview-PAYG-Umgebung.
> * Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einer Event Hub-Instanz.
> * Ausführen eines Solution Accelerator-Beispiels zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
> * Durchführen einer grundlegenden Analyse der Daten.
> * Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und deren Zuordnung zu Ihren Instanzen.

Da Sie nun Ihre eigene Azure Time Series Insights Preview-Umgebung erstellen können, erfahren Sie mehr über die wichtigsten Konzepte in Azure Time Series Insights.

Informieren Sie sich über die Azure Time Series Insights-Speicherkonfiguration:

> [!div class="nextstepaction"]
> [Speicherung und Datenerfassung in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md)

Erfahren Sie mehr über Zeitreihenmodelle:

> [!div class="nextstepaction"]
> [Datenmodellierung in Azure Time Series Insights](./time-series-insights-update-tsm.md)