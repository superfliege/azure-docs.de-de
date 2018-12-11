---
title: Azure Time Series Insights (Vorschauversion) – Tutorial | Microsoft-Dokumentation
description: Enthält eine Beschreibung von Azure Time Series Insights (Vorschauversion).
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: ed25d03f7c592476b9284790ac12f9954661a42b
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872304"
---
# <a name="azure-time-series-insights-preview-tutorial"></a>Azure Time Series Insights (Vorschauversion) – Tutorial

In diesem Tutorial wird der Prozess zum Erstellen einer Azure Time Series Insights-Umgebung (TSI) (Vorschauversion) beschrieben, die mit Daten von simulierten Geräten gefüllt wird. In diesem Tutorial lernen Sie Folgendes:

* Erstellen einer TSI-Umgebung (Vorschauversion)
* Verbinden der TSI-Umgebung (Vorschauversion) mit einem Event Hub
* Ausführen einer Windfarmsimulation zum Streamen von Daten in die TSI-Umgebung (Vorschauversion)
* Durchführen einer grundlegenden Analyse für die Daten
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und Zuordnen zu Ihren Instanzen

## <a name="create-a-time-series-insights-preview-environment"></a>Erstellen einer Time Series Insights-Umgebung (Vorschauversion)

In diesem Abschnitt wird beschrieben, wie Sie mit dem [Azure-Portal](https://portal.azure.com/) eine Azure TSI-Umgebung (Vorschauversion) erstellen.

1. Anmelden am Azure-Portal mit Ihrem Abonnementkonto
1. Wählen Sie oben links die Option **+ Ressource erstellen**.
1. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.

  ![tutorial-one][1]

1. Geben Sie auf der Seite „Time Series Insights-Umgebung“ die erforderlichen Parameter ein, und klicken Sie auf **Weiter: Ereignisquelle**.

  ![tutorial-two][2]

1. Fügen Sie auf der Seite  **Ereignisquelle** die erforderlichen Parameter ein, und klicken Sie auf **Überprüfen + erstellen**.

  ![tutorial-three][3]

1. Überprüfen Sie alle Details, und klicken Sie auf **Erstellen**, um mit der Bereitstellung Ihrer Umgebung zu beginnen.

  ![tutorial-four][4]

1. Sie erhalten eine Benachrichtigung, nachdem die Bereitstellung erfolgreich abgeschlossen wurde.

  ![tutorial-five][5]

## <a name="send-events-to-your-tsi-environment"></a>Senden von Ereignissen an Ihre TSI-Umgebung

In diesem Abschnitt verwenden Sie einen Windenergieanlagen-Simulator, um Ereignisse über einen Event Hub an Ihre TSI-Umgebung zu senden.

  1. Navigieren Sie im Azure-Portal zu Ihrer Event Hub-Ressource, und stellen Sie dafür eine Verbindung mit Ihrer TSI-Umgebung her. Informieren Sie sich, [wie Sie für Ihre Ressource eine Verbindung mit einem vorhandenen Event Hub herstellen](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. Navigieren Sie auf der Seite mit den Event Hub-Ressourcen zu **Freigegebene Zugriffsrichtlinien** und dann zu **RootManageSharedAccessKey**. Kopieren Sie den hier angezeigten Wert von **Verbindungszeichenfolge – Primärschlüssel** .

      ![tutorial-six][6]

  1. Navigieren Sie zu [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html). Mit dieser Web-App werden Windenergieanlagen simuliert.
  1. Fügen Sie die in Schritt 3 kopierte Verbindungszeichenfolge unter **Event Hub-Verbindungszeichenfolge** ein.

      ![tutorial-seven][7]

  1. Klicken Sie auf **Click to Start** (Zum Starten hier klicken), um Ereignisse per Pushübertragung an Ihren Event Hub zu senden. Eine Datei mit dem Namen `instances.json` wird auf Ihren Computer heruntergeladen. Speichern Sie diese Datei, da wir sie später noch benötigen.

  1. Navigieren Sie zurück zu Ihrem Event Hub. Sie sollten jetzt die neuen Ereignisse verfolgen können, die vom Hub empfangen werden.

     ![tutorial-eight][8]

## <a name="analyze-data-in-your-environment"></a>Analysieren von Daten in Ihrer Umgebung

In diesem Abschnitt führen Sie grundlegende Analysen für Ihre Zeitreihendaten durch, indem Sie den Explorer für Time Series Insights-Updates verwenden.

  1. Navigieren Sie zu Ihrem Explorer für Time Series Insights-Updates, indem Sie im Azure-Portal auf der Ressourcenseite auf die URL klicken.

      ![tutorial-nine][9]

  1. Klicken Sie im Explorer auf die Knoten vom Typ **Unparented Instances** (Nicht übergeordnete Instanzen), um alle Zeitreiheninstanzen der Umgebung anzuzeigen.

     ![tutorial-ten][10]

  1. In diesem Tutorial analysieren Sie die Daten, die innerhalb des letzten Tags gesendet wurden. Klicken Sie hierzu auf **Kurze Zeitangaben**, und wählen Sie die Option **Letzte 24 Stunden**.

     ![tutorial-eleven][11]

  1. Wählen Sie **Sensor_0** und dann **Show Avg Value** (Durchschnittswert anzeigen), um Daten zu visualisieren, die von dieser Time Series-Instanz gesendet werden.

     ![tutorial-twelve][12]

  1. Auf ähnliche Weise können Sie Daten plotten, die von anderen Zeitreiheninstanzen stammen, um grundlegende Analysen durchzuführen.

     ![tutorial-thirteen][13]

## <a name="define-a-type--hierarchy"></a>Definieren eines Typs und einer Hierarchie 

In diesem Abschnitt erstellen Sie einen Typ und eine Hierarchie und ordnen sie Ihren Zeitreiheninstanzen zu. Erfahren Sie mehr über [Zeitreihenmodelle](./time-series-insights-update-tsm.md).

  1. Klicken Sie im Explorer in der App-Leiste auf die Registerkarte **Modell**.

     ![tutorial-fourteen][14]

  1. Klicken Sie im Abschnitt „Typen“ auf **+ Hinzufügen**. Sie können jetzt einen neuen Time Series-Modelltyp erstellen.

     ![tutorial-fifteen][15]

  1. Geben Sie im Typ-Editor einen **Namen** und eine **Beschreibung** ein, und erstellen Sie wie unten gezeigt Variablen für die Werte **Durchschnitt**, **Min** und **Max**. Klicken Sie auf **Erstellen**, um den Typ zu speichern.

     ![tutorial-sixteen][16]

     ![tutorial-seventeen][17]

  1. Klicken Sie im Abschnitt **Hierarchien** auf **+ Hinzufügen**. Sie können jetzt eine neue Time Series-Modellhierarchie erstellen.

     ![tutorial-eighteen][18]

  1. Geben Sie im Hierarchie-Editor einen **Namen** ein, und fügen Sie wie unten gezeigt Hierarchieebenen hinzu. Klicken Sie auf **Erstellen**, um die Hierarchie zu speichern.

     ![tutorial-nineteen][19]

     ![tutorial-twenty][20]

  1. Wählen Sie im Abschnitt **Instanzen** eine Instanz aus, und klicken Sie auf **Bearbeiten**. Sie können dieser Instanz dann einen Typ und eine Hierarchie zuordnen.

     ![tutorial-twenty-one][21]

  1. Wählen Sie im Instanz-Editor wie gezeigt den Typ und die Hierarchie aus, der bzw. die oben in den Schritten 3 und 5 definiert wurden.

     ![tutorial-twenty-two][22]

  1. Um dies für alle Instanzen durchzuführen, können Sie auch die Datei `instances.json` bearbeiten, die zuvor heruntergeladen wurde. Ersetzen Sie in dieser Datei alle **typeId**- und **hierarchyId**-Felder jeweils durch die ID, die in den Schritten 3 und 5 oben ermittelt wurde.

  1. Klicken Sie im Abschnitt **Instanzen** auf **JSON hochladen**, und laden Sie die bearbeitete Datei `instances.json` wie unten gezeigt hoch.

     ![tutorial-twenty-three][23]

  1. Navigieren Sie zur Registerkarte **Analyse**, und aktualisieren Sie Ihren Browser. Nun sollten alle Instanzen angezeigt werden, denen der oben definierte Typ bzw. die Hierarchie zugeordnet ist.

     ![tutorial-twenty-four][24]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:  

* Erstellen einer TSI-Umgebung (Vorschauversion)
* Verbinden der TSI-Umgebung (Vorschauversion) mit einem Event Hub
* Ausführen einer Windfarmsimulation zum Streamen von Daten in die TSI-Umgebung (Vorschauversion)
* Durchführen einer grundlegenden Analyse für die Daten
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und Zuordnen zu Ihren Instanzen

Da Sie jetzt wissen, wie Sie Ihre eigene TSI-Updateumgebung erstellen, können Sie sich weiter über die wichtigen Konzepte in TSI informieren.

Erfahren Sie mehr zur TSI-Speicherkonfiguration:

> [!div class="nextstepaction"]
> [Azure TSI (Preview) storage and ingress](./time-series-insights-update-storage-ingress.md) (Azure TSI (Vorschauversion) – Speicherung und Erfassung)

Erfahren Sie mehr über Zeitreihenmodelle:

> [!div class="nextstepaction"]
> [Azure TSI (Preview) Data modeling](./time-series-insights-update-tsm.md) (Azure TSI (Vorschauversion) – Datenmodellierung)

<!-- Images -->
[1]: media/v2-update-provision/tutorial-one.png
[2]: media/v2-update-provision/tutorial-two.png
[3]: media/v2-update-provision/tutorial-three.png
[4]: media/v2-update-provision/tutorial-four.png
[5]: media/v2-update-provision/tutorial-five.png
[6]: media/v2-update-provision/tutorial-six.png
[7]: media/v2-update-provision/tutorial-seven.png
[8]: media/v2-update-provision/tutorial-eight.png
[9]: media/v2-update-provision/tutorial-nine.png
[10]: media/v2-update-provision/tutorial-ten.png
[11]: media/v2-update-provision/tutorial-eleven.png
[12]: media/v2-update-provision/tutorial-twelve.png
[13]: media/v2-update-provision/tutorial-thirteen.png
[14]: media/v2-update-provision/tutorial-fourteen.png
[15]: media/v2-update-provision/tutorial-fifteen.png
[16]: media/v2-update-provision/tutorial-sixteen.png
[17]: media/v2-update-provision/tutorial-seventeen.png
[18]: media/v2-update-provision/tutorial-eighteen.png
[19]: media/v2-update-provision/tutorial-nineteen.png
[20]: media/v2-update-provision/tutorial-twenty.png
[21]: media/v2-update-provision/tutorial-twenty-one.png
[22]: media/v2-update-provision/tutorial-twenty-two.png
[23]: media/v2-update-provision/tutorial-twenty-three.png
[24]: media/v2-update-provision/tutorial-twenty-four.png