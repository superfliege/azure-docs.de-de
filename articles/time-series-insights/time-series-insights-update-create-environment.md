---
title: 'Tutorial: Einrichten von Azure Time Series Insights Preview – Einrichten einer Azure Time Series Insights Preview-Umgebung | Microsoft-Dokumentation'
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
ms.openlocfilehash: 9ad957d6378b1279f1ca51939eb4802b0ce7d78f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322617"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Einrichten einer Azure Time Series Insights Preview-Umgebung

Dieses Tutorial führt Sie durch das Erstellen einer Azure Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG). In diesem Tutorial lernen Sie Folgendes:

* Erstellen einer Azure Time Series Insights Preview-Umgebung.
* Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einem Event Hub in Azure Event Hubs.
* Ausführen einer Windfarmsimulation zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
* Durchführen einer grundlegenden Analyse für die Daten
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und die Zuordnung zu Ihren Instanzen.

# <a name="create-a-device-simulation"></a>Erstellen einer Gerätesimulation

In diesem Abschnitt erstellen Sie drei simulierte Geräte, die Daten an einen IoT Hub senden.

1. Gehen Sie zur [Homepage von Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators). Die Homepage von Azure IoT Solution Accelerators enthält mehrere vorgefertigte Beispiele. Melden Sie sich mit Ihrem Azure-Konto an. Wählen Sie dann **Gerätesimulation** aus.

   ![Homepage von Azure IoT Solution Accelerators][1]

   Klicken schließlich auf **Jetzt ausprobieren**.

1. Geben Sie auf der Lösungsseite **Gerätesimulation erstellen** die erforderlichen Parameter ein:

   | Parameter | BESCHREIBUNG |
   | --- | --- |
   | Projektmappenname |    Ein eindeutiger Wert, der für die Erstellung einer neuen Ressourcengruppe verwendet wird. Die aufgelisteten Azure-Ressourcen werden | erstellt und der Ressourcengruppe zugewiesen. |
   | Abonnement | Geben Sie dasselbe Abonnement an, das Sie zum Erstellen Ihrer TSI-Umgebung verwendet haben. |
   | Region |   Geben Sie dieselbe Region an, die Sie zum Erstellen Ihrer TSI-Umgebung verwendet haben. |
   | Bereitstellen der optionalen Azure-Ressourcen    | Lassen Sie die Option „IoT Hub“ aktiviert, da sie von den simulierten Geräten für die Verbindungsherstellung und zum Streamen von Daten genutzt wird. |

   Nachdem Sie die erforderlichen Parameter eingegeben haben, klicken Sie auf **Lösung erstellen**. Warten Sie ca. 10-15 Minuten, bis Ihre Lösung bereitgestellt wird.

   ![Erstellen einer Gerätesimulationslösung][2]

1. Klicken Sie in Ihrem **Solution Accelerator-Dashboard** auf die Schaltfläche **Starten**:

   ![Starten der Gerätesimulationslösung][3]

1. Sie werden zur Seite **Microsoft Azure IoT-Gerätesimulation** weitergeleitet. Klicken Sie oben rechts im Bildschirm auf **+ Neue Simulation**.

   ![Azure IoT-Simulationsseite][4]

1.  Füllen Sie die erforderlichen Parameter wie folgt aus:

    ![Auszufüllender Parameter][5]

    |||
    | --- | --- |
    | **Name** | Geben Sie einen eindeutigen Namen für einen Simulator ein. |
    | **Beschreibung** | Geben Sie eine Definition ein. |
    | **Simulationsdauer** | Legen Sie den Wert `Run indefinitely` |
    | **Gerätemodell** | **Name**: Geben Sie die `Chiller`-**Menge** ein: Geben Sie `3` ein. |
    | **IoT-Zielhub** | Legen Sie den Wert `Use pre-provisioned IoT Hub` |

    Klicken Sie nach dem Ausfüllen der erforderlichen Parameter auf **Simulation starten**.

1. Im Gerätesimulationsdashboard werden **Aktive Geräte** und **Nachrichten pro Sekunde** angezeigt.

    ![Azure IoT-Simulationsdashboard][6]

## <a name="list-device-simulation-properties"></a>Auflisten der Gerätesimulationseigenschaften

Bevor Sie eine Azure Time Series Insights-Umgebung erstellen, benötigen Sie die Namen Ihrer IoT Hub-Instanz, Ihres Abonnements und Ihrer Ressourcengruppe.

1. Wechseln Sie zum **Solution Accelerator-Dashboard**, und melden Sie sich mit dem gleichen Azure-Abonnementkonto an. Suchen Sie die Gerätesimulation, die Sie in den vorherigen Schritten erstellt haben.

1. Klicken Sie auf Ihren Gerätesimulator und dann auf **Starten**. Klicken Sie auf den Link **Azure-Verwaltungsportal**, der auf der rechten Seite angezeigt wird.

    ![Simulatorauflistungen][7]

1. Notieren Sie sich die Namen von IoT Hub-Instanz, Abonnement und Ressourcengruppe.

    ![Azure-Portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Erstellen einer Time Series Insights Preview-PAYG-Umgebung

In diesem Abschnitt wird beschrieben, wie Sie eine Azure Time Series Insights Preview-Umgebung mit dem [Azure-Portal](https://portal.azure.com/) erstellen.

1. Melden Sie sich unter Verwendung Ihres Abonnementkontos beim Azure-Portal an.

1. Wählen Sie **Ressource erstellen**.

1. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.

   ![Wählen Sie „Ressource erstellen“ und dann „Internet der Dinge“ und „Time Series Insights“ aus.][9]

1. Füllen Sie die Felder auf der Seite wie folgt aus:

   | | |
   | --- | ---|
   | **Umgebungsname** | Wählen Sie einen eindeutigen Namen für die Azure Time Series Insights Preview-Umgebung aus. |
   | **Abonnement** | Geben Sie Ihr Abonnement an, in dem Sie die Azure Time Series Insights Preview-Umgebung erstellen möchten. Sie sollten dasselbe Abonnement verwenden, in dem sich die übrigen Ihrer vom Gerätesimulator erstellten IoT-Ressourcen befinden, da sich diese Methode bewährt hat. |
   | **Ressourcengruppe** | Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Wählen Sie eine vorhandene Ressourcengruppe für die Azure Time Series Insights Preview-Umgebungsressource aus, oder erstellen Sie eine neue. Sie sollten dieselbe Ressourcengruppe verwenden, in der sich die übrigen Ihrer vom Gerätesimulator erstellten IoT-Ressourcen befinden, da sich diese Methode bewährt hat. |
   | **Location** | Wählen Sie eine Rechenzentrumsregion für Ihre Azure Time Series Insights Preview-Umgebung aus. Um zusätzliche Bandbreitenkosten und Latenz zu vermeiden, sollten Sie die Azure Time Series Insights Preview-Umgebung in derselben Region wie die anderen IoT-Ressourcen belassen. |
   | **Tier** |  Wählen Sie `PAYG` aus, was für nutzungsbasierte Bezahlung (Pay-As-You-Go) steht. Dies ist die SKU für das Azure Time Series Insights Preview-Produkt. |
   | **Eigenschafts-ID** | Zur eindeutigen Identifizierung Ihrer Zeitreihe. Beachten Sie, dass dieses Feld unveränderlich ist und später nicht geändert werden kann. In diesem Tutorial ist für das Feld `iothub-connection-device-id` festgelegt. Weitere Informationen zur Zeitreihen-ID finden Sie unter [Bewährte Methoden zur Auswahl einer Zeitreihen-ID](./time-series-insights-update-how-to-id.md). |
   | **Speicherkontoname** | Geben Sie einen global eindeutigen Namen für ein neu zu erstellendes Speicherkonto ein. |

   Klicken Sie nach dem Ausfüllen der oben genannten Felder auf **Weiter: Ereignisquelle**.

   ![Klicken Sie auf „Weiter: Ereignisquelle][10]

1. Füllen Sie die Felder auf der Seite wie folgt aus:

   | | |
   | --- | --- |
   | **Ereignisquelle erstellen?** | Geben Sie `Yes` ein.|
   | **Name** | Hier ist ein eindeutiger Wert erforderlich, der zum Benennen der Ereignisquelle verwendet wird.|
   | **Quellentyp** | Geben Sie `IoT Hub` ein. |
   | **Hub auswählen?** | Geben Sie `Select Existing` ein. |
   | **Abonnement** | Geben Sie das Abonnement ein, das Sie für den Gerätesimulator verwendet haben. |
   | **IoT Hub-Name** | Geben Sie den Namen der IoT Hub-Instanz ein, die Sie für den Gerätesimulator erstellt haben. |
   | **Zugriffsrichtlinie für IoT Hub** | Geben Sie `iothubowner` ein. |
   | **IoT Hub-Consumergruppe** | Sie benötigen eine eindeutige Consumergruppe für Azure Time Series Insights Preview. |
   | **Timestamp** | Mit diesem Feld wird die Zeitstempel-Eigenschaft in Ihren eingehenden Telemetriedaten identifiziert. Füllen Sie das Feld in diesem Tutorial nicht aus. Dieser Simulator verwendet den aus IoT Hub eingehenden Zeitstempel, den Time Series Insights standardmäßig verwendet.|

   So erstellen Sie eine eindeutige Consumergruppe:

   1. Klicken Sie auf **Neu** neben dem Feld **IoT Hub-Consumergruppe**:

      ![Klicken Sie auf „Weiter: Ereignisquelle][11]

   1. Geben Sie der Consumergruppe einen eindeutigen Namen, und klicken Sie auf **Hinzufügen**:

      ![Klicken Sie auf "Hinzufügen".][12]

   Nachdem Sie die oben genannten Felder ausgefüllt haben, klicken Sie auf **Überprüfen + erstellen**.

      ![Überprüfen und Erstellen][13]

1. Überprüfen Sie alle Felder auf der Überprüfungsseite, und klicken Sie auf **Erstellen**.

   ![Erstellen][14]

1. Sie können den Status Ihrer Bereitstellung sehen.

   ![Bereitstellung abgeschlossen][15]

1. Wenn Sie den Mandanten besitzen, sollten Sie den Zugriff auf Ihre Zeitreihenumgebung erhalten. So stellen Sie sicher, dass Sie über Zugriff verfügen:

   * Navigieren Sie zu Ihrer neu erstellten Azure Time Series Insights Preview-Umgebung. Sie können hierzu nach Ihrer Ressourcengruppe suchen. Klicken Sie dann auf Ihre Zeitreihenumgebung:

      ![Bereitstellung abgeschlossen][16]

   * Navigieren Sie auf der Azure Time Series Insights Preview-Seite zu **Datenzugriffsrichtlinien**.

     ![Datenzugriffsrichtlinien][17]

   * Stellen Sie sicher, dass Ihre Anmeldeinformationen aufgelistet werden.

     ![Überprüfen Ihrer Anmeldeinformationen][18]

   Wenn Ihre Anmeldeinformationen nicht aufgelistet sind, müssen Sie sich selbst die Erlaubnis zum Zugriff auf die Umgebung geben. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Analysieren von Daten in Ihrer Umgebung

In diesem Abschnitt führen Sie mit dem [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) grundlegende Analysen für Ihre Zeitreihendaten durch.

1. Navigieren Sie zu Ihrem Azure Time Series Insights Preview-Explorer, indem Sie im [Azure-Portal](https://portal.azure.com/) auf der Ressourcenseite auf die URL klicken.

   ![Die Time Series Insights-Explorer-URL][19]

1. Wählen Sie im Explorer den Knoten **Instanzen ohne übergeordnete Elemente** aus, um die gesamte Azure Time Series Insights Preview in der Umgebung anzuzeigen.

   ![Liste von Instanzen ohne übergeordnete Elemente][20]

1. Klicken Sie in der angezeigten Zeitreihe auf die erste Instanz. Klicken Sie dann auf **Show Avg pressure (Durchschnittlichen Druck anzeigen)**.

   ![Show Avg pressure (Durchschnittlichen Druck anzeigen)][21]

1. Ein Zeitreihendiagramm sollte auf der rechten Seite angezeigt werden:

   ![Zeitreihendiagramm][22]

1. Wiederholen Sie **Schritt 3** mit den anderen beiden Zeitreihen. Alle Zeitreihen können dann wie unten dargestellt angezeigt werden:

   ![Diagramm mit allen Zeitreihen][23]

1. Ändern Sie den **Zeitbereich**, um Zeitreihentrends innerhalb der letzten Stunde anzuzeigen. Wählen Sie die Option **Von** wie unten dargestellt aus:

   ![Auswählen der Option „Von“][24]

1. Ändern Sie die Zeit im Optionsfeld **Von**, um Ereignisse der letzten Stunde anzuzeigen:

   ![Auswählen der Option „Von“][25]

1. Anschließend können Sie den Druck aller drei Geräte in der letzten Stunde vergleichen:

   ![Auswählen der Option „Von“][26]

## <a name="define-and-apply-a-model"></a>Definieren und Anwenden eines Modells

In diesem Abschnitt wenden Sie ein Modell zum Strukturieren der Daten an. Um das Modell zu vervollständigen, definieren Sie Typen, Hierarchien und Instanzen. Weitere Informationen zur Datenmodellierung finden Sie unter [Zeitreihenmodelle](./time-series-insights-update-tsm.md).

1. Wählen Sie im Explorer die Registerkarte **Modell** aus:

   ![Auswählen der Registerkarte „Modell“][27]

1. Klicken Sie anschließend auf **+ Add**, um einen Typen hinzuzufügen. Auf der rechten Seite wird ein Typ-Editor geöffnet.

   ![Klicken Sie auf "Hinzufügen".][28]

1. Definieren Sie als Nächstes drei Variablen: Pressure (Druck), Temperature (Temperatur) und Humidity (Luftfeuchtigkeit) in einem Typ. Füllen Sie die folgenden Felder aus:

   | | |
   | --- | ---|
   | **Name** | Geben Sie `Chiller` ein. |
   | **Beschreibung** | Geben Sie `This is a type definition of Chiller` ein. |

   * Definieren Sie nun (Pressure) Druck mit drei Variablen:

      | | |
      | --- | ---|
      | **Name** | Geben Sie `Avg Pressure` ein. |
      | **Wert** | Wählen Sie **pressure (Double) (Druck (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, sobald Azure Time Series Insights beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie `AVG`. |

      ![Hinzufügen einer Variablen][29]

      Klicken Sie auf **+Variable**, um die nächste Variable hinzuzufügen.

   * Definieren Sie nun „Temperature“:

      | | |
      | --- | ---|
      | **Name** | Geben Sie `Avg Temperature` ein. |
      | **Wert** | Wählen Sie **temperature (Double) (Temperatur (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, sobald Azure Time Series Insights beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie `AVG`.|

      ![Definieren von „Temperature“][30]

   * Definieren Sie nun „Humidity“ (Luftfeuchtigkeit):

      | | |
      | --- | ---|
      | **Name** | Geben Sie `Max Humidity` ein. |
      | **Wert** | Wählen Sie **humidity (Double) (Luftfeuchtigkeit (Double))** aus. Beachten Sie, dass es einige Minuten dauern kann, bis dieses Feld aufgefüllt wird, sobald Azure Time Series Insights beginnt, Ereignisse zu empfangen. |
      | **Aggregationsvorgang** | Wählen Sie `MAX`.|

      ![Definieren von „Temperature“][31]

   Klicken Sie nach dem Definieren der Variablen auf **Erstellen**.

1. Sie sehen Ihren hinzugefügten Typ:

   ![Anzeige des hinzugefügten Typs][32]

1. Der nächste Schritt ist das Hinzufügen einer Hierarchie. Wählen Sie im Abschnitt **Hierarchien** zum Erstellen einer neuen Hierarchie **+ Hinzufügen** aus:

   ![Hinzufügen einer Hierarchie][33]

1. Definieren Sie eine Hierarchie. Füllen Sie die Felder wie folgt aus:

   | | |
   | --- | ---|
   | **Name** | Geben Sie `Location Hierarchy` ein. |
   | **Ebene 1** | Geben Sie `Country` ein. |
   | **Ebene 2** | Geben Sie `City` ein. |
   | **Ebene 3** | Geben Sie `Building` ein. |

   Klicken Sie nach dem Ausfüllen der oben genannten Felder auf **Erstellen**.

   ![Definieren einer Hierarchie][34]

1. Sie können die erstellte Hierarchie sehen:

   ![Anzeigen Ihrer Hierarchie][35]

1. Klicken Sie nach dem Definieren der Hierarchie auf der linken Seite auf **Instanzen**. Sobald die Instanzen angezeigt werden, klicken Sie auf die erste Instanz und wählen **Bearbeiten**:

   ![Bearbeiten einer Instanz][36]

1. Auf der rechten Seite wird ein Text-Editor geöffnet. Fügen Sie folgende Felder hinzu:

   | | |
   | --- | --- |
   | **Typ** | Wählen Sie `Chiller`. |
   | **Beschreibung** | Geben Sie `Instance for Chiller-01.1` ein. |
   | **Hierarchien** | Aktivieren Sie `Location Hierarchy`. |
   | **Country** | Geben Sie `USA` ein. |
   | **City (Ort)** | Geben Sie `Seattle` ein. |
   | **Building (Gebäude)** | Geben Sie `Space Needle` ein. |

    Klicken Sie nach dem Ausfüllen der oben genannten Felder auf **Speichern**.

   ![Speichern einer Kältemaschine][37]

1. Wiederholen Sie den vorherigen Schritt für die anderen Sensoren. Verwenden Sie die folgenden Felder:

   * Für Chiller 01.2:

     | | |
     | --- | --- |
     | **Typ** | Wählen Sie `Chiller`. |
     | **Beschreibung** | Geben Sie `Instance for Chiller-01.2` ein. |
     | **Hierarchien** | Aktivieren Sie `Location Hierarchy`. |
     | **Country** | Geben Sie `USA` ein. |
     | **City (Ort)** | Geben Sie `Seattle` ein. |
     | **Building (Gebäude)** | Geben Sie `Pacific Science Center` ein. |

   * Für Chiller 01.3:

     | | |
     | --- | --- |
     | **Typ** | Wählen Sie `Chiller`. |
     | **Beschreibung** | Geben Sie `Instance for Chiller-01.1` ein. |
     | **Hierarchien** | Aktivieren Sie `Location Hierarchy`. |
     | **Country** | Geben Sie `USA` ein. |
     | **City (Ort)** | Geben Sie `New York` ein. |
     | **Building (Gebäude)** | Geben Sie `Empire State Building` ein. |

1. Wechseln Sie zur Registerkarte **Analysieren**, und aktualisieren Sie die Seite. Erweitern Sie alle Hierarchieebenen, um die Zeitreihe zu finden.

   ![Anzeigen der Registerkarte „Analysieren“][38]

1. Um die Zeitreihen der letzten Stunde zu untersuchen, ändern Sie **Kurze Zeitangaben** in die letzte Stunde:

   ![Untersuchen der letzten Stunde][39]

1. Klicken Sie auf die Zeitreihen unter **Pacific Science Center** und dann auf **Show Max Humidity (Max. Luftfeuchtigkeit anzeigen)**.

   ![Show Max Humidity (Max. Luftfeuchtigkeit anzeigen)][40]

1. Die Zeitreihe für **Max Humidity (Max. Luftfeuchtigkeit)** mit einer Intervallgröße von 1 Minute wird geöffnet. Klicken Sie auf eine Region, um einen Bereich zu filtern. Klicken Sie dann mit der rechten Maustaste, und zoomen Sie zum Analysieren der Ereignisse innerhalb des Zeitrahmens:

   ![Anzeigen, Filtern und Zoomen][41]

   ![Anzeigen, Filtern und Zoomen][42]

1. Sie können auch auf eine Region klicken und dann mit der rechten Maustaste klicken, um die Ereignisdetails anzuzeigen:

   ![Anzeigen, Filtern und Zoomen][43]

   ![Anzeigen, Filtern und Zoomen][44]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:  

* Erstellen und Verwenden eines Accelerators zur Gerätesimulation.
* Erstellen einer Azure Time Series Insights Preview-PAYG-Umgebung.
* Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einer Event Hub-Instanz.
* Durchführen einer Windfarmsimulation zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
* Durchführen einer grundlegenden Analyse der Daten.
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und deren Zuordnung zu Ihren Instanzen.

Da Sie nun Ihre eigene Azure Time Series Insights Preview-Umgebung erstellen können, erfahren Sie mehr über die wichtigsten Konzepte in Azure Time Series Insights.

Informieren Sie sich über die Azure Time Series Insights-Speicherkonfiguration:

> [!div class="nextstepaction"]
> [Speicherung und Eingang in Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)

Erfahren Sie mehr über Zeitreihenmodelle:

> [!div class="nextstepaction"]
> [Datenmodellierung in Azure Time Series Insights](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/device-one-accelerator.png
[2]: media/v2-update-provision/device-two-create.png
[3]: media/v2-update-provision/device-three-launch.png
[4]: media/v2-update-provision/device-four-iot-sim-page.png
[5]: media/v2-update-provision/device-five-params.png
[6]: media/v2-update-provision/device-six-listings.png
[7]: media/v2-update-provision/device-seven-dashboard.png
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