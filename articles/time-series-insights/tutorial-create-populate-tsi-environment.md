---
title: 'Tutorial: Erstellen einer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie eine Time Series Insights-Umgebung erstellen, die mit Daten von simulierten Geräten aufgefüllt wird.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 3c097ec74f45248a41c6dd4df80cbbd927a9b5ed
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827437"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Tutorial: Erstellen einer Azure Time Series Insights-Umgebung

In diesem Tutorial wird der Prozess zum Erstellen einer Time Series Insights-Umgebung beschrieben, die mit Daten von simulierten Geräten aufgefüllt wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Time Series Insights-Umgebung 
> * Erstellen einer Lösung für die Gerätesimulation mit einem IoT Hub
> * Herstellen einer Verbindung der Time Series Insights-Umgebung mit dem IoT-Hub
> * Ausführen einer Gerätesimulation zum Streamen von Daten in die Time Series Insights-Umgebung
> * Überprüfen der simulierten Telemetriedaten

## <a name="video"></a>Video

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Hier wird gezeigt, wie Sie einen Azure IoT Solution Accelerator verwenden, um Daten zu generieren und die ersten Schritte mit Time Series Insights auszuführen. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

* Ihr Azure-Anmeldekonto muss außerdem Mitglied der Rolle **Besitzer** des Abonnements sein. Ausführliche Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und dem Azure-Portal](/azure/role-based-access-control/role-assignments-portal).

## <a name="overview"></a>Übersicht

Die Time Series Insights-Umgebung ist der Ort, an dem Gerätedaten erfasst und gespeichert werden. Einmal in der Time Series Insights-Umgebung gespeichert, können der [Time Series Insights-Explorer](time-series-quickstart.md) und die [Time Series Insights-Abfrage-API](/rest/api/time-series-insights/ga-query-api) verwendet werden, um die Daten abzufragen und zu analysieren. IoT Hub ist der Verbindungspunkt, der von allen (simulierten oder physischen) Geräten zum sicheren Herstellen einer Verbindung und zum Übertragen von Daten in die Azure-Cloud verwendet wird. Wie in der [Übersicht über Time Series Insights](time-series-insights-overview.md) beschrieben, dient Azure IoT Hub auch als Ereignisquelle zum Streamen von Daten in eine Time Series Insights-Umgebung. In diesem Tutorial wird ein [IoT Solution Accelerator](/azure/iot-accelerators/) genutzt, um Beispieltelemetriedaten zu generieren und an IoT Hub zu streamen.

>[!TIP]
> Mit IoT Solution Accelerators werden für Unternehmen geeignete vorkonfigurierte Lösungen bereitgestellt, mit denen Sie die Entwicklung von benutzerdefinierten IoT-Lösungen beschleunigen können.

## <a name="create-an-environment"></a>Erstellen einer Umgebung

Erstellen Sie zunächst unter Ihrem Azure-Abonnement eine Time Series Insights-Umgebung:

1. Melden Sie sich unter Verwendung Ihres Azure-Abonnementkontos am [Azure-Portal](https://portal.azure.com) an.  
1. Wählen Sie oben links die Option **+ Ressource erstellen**.  
1. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.  

   [![Auswählen der Time Series Insights-Umgebungsressource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Geben Sie auf der Seite **Time Series Insights-Umgebung** die erforderlichen Parameter ein:

   Parameter|BESCHREIBUNG
   ---|---
   **Umgebungsname** | Wählen Sie einen eindeutigen Namen für die Time Series Insights-Umgebung aus. Der Name wird vom Time Series Insights-Explorer und von Abfrage-APIs verwendet.
   **Abonnement** | Abonnements sind Container für Azure-Ressourcen. Wählen Sie ein Abonnement aus, für das die Time Series Insights-Umgebung erstellt werden soll.
   **Ressourcengruppe** | Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Wählen Sie eine vorhandene Ressourcengruppe für die Time Series Insights-Umgebungsressource aus, oder erstellen Sie eine neue.
   **Location** | Wählen Sie eine Rechenzentrumsregion für Ihre Time Series Insights-Umgebung aus. Um zusätzliche Bandbreitenkosten und Latenz zu vermeiden, sollten Sie die Time Series Insights-Umgebung in derselben Region wie die anderen IoT-Ressourcen belassen.
   **Preise für SKUs** | Wählen Sie den erforderlichen Durchsatz aus. Wählen Sie `S1` aus, um die niedrigsten Kosten zu erzielen und die geringste Anfangskapazität zu erhalten.
   **Capacity** | Die Kapazität ist der Multiplikator, der auf die Eingangsrate, Speicherkapazität und Kosten der ausgewählten SKU angewendet wird.  Sie können die Kapazität nach der Erstellung ändern. Wählen Sie die Kapazität „1“ aus, um die niedrigsten Kosten zu erzielen.

   Klicken Sie nach Abschluss des Vorgangs auf **Erstellen**, um mit dem Bereitstellungsprozess zu beginnen.

   [![Erstellen einer Time Series Insights-Umgebungsressource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Im Bereich **Benachrichtigungen** können Sie den Abschluss der Bereitstellung überwachen.  

   [![Bereitstellung der Time Series Insights-Umgebung erfolgreich abgeschlossen](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Erstellen einer Gerätesimulation

Erstellen Sie als Nächstes die Lösung für die Gerätesimulation, mit der Testdaten zum Auffüllen Ihrer Time Series Insights-Umgebung generiert werden:

1. Rufen Sie in einem separaten Fenster/auf einer separaten Registerkarte [azureiotsolutions.com](https://www.azureiotsolutions.com) auf. Melden Sie sich mit demselben Azure-Abonnementkonto an, und wählen Sie den Accelerator **Gerätesimulation** aus:

   [![Ausführen des Accelerators „Gerätesimulation“](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Geben Sie auf der Lösungsseite **Gerätesimulation erstellen** die erforderlichen Parameter ein:

   Parameter|BESCHREIBUNG
   ---|---
   **Lösungsname** | Ein eindeutiger Wert, der für die Erstellung einer neuen Ressourcengruppe verwendet wird. Die aufgelisteten Azure-Ressourcen werden erstellt und der Ressourcengruppe zugewiesen.
   **Abonnement** | Geben Sie dasselbe Abonnement an, das Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung im vorherigen Abschnitt verwendet haben.
   **Region** | Geben Sie dieselbe Region an, die Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung im vorherigen Abschnitt verwendet haben.
   **Optionale Azure-Ressourcen bereitstellen** | Lassen Sie die Option **IoT Hub** aktiviert, da sie von den simulierten Geräten für die Verbindungsherstellung und zum Streamen von Daten genutzt wird.

   Klicken Sie nach Abschluss des Vorgangs auf **Lösung erstellen**, um die Azure-Ressourcen der Lösung bereitzustellen. Dieser Vorgang kann 6 bis 7 Minuten dauern.

   [![Bereitstellen der Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Nachdem die Bereitstellung abgeschlossen ist, ändert sich der Text über Ihrer neuen Lösung von **Wird bereitgestellt...** in **Bereit**:

   >[!IMPORTANT]
   > Klicken Sie noch nicht auf die Schaltfläche **Starten**! Lassen Sie diese Webseite aber geöffnet, da Sie später hierhin zurückkehren.

   [![Bereitstellung der Gerätesimulationslösung abgeschlossen](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Wechseln Sie zurück zum Azure-Portal, und sehen Sie sich die neu erstellten Ressourcen unter Ihrem Abonnement an. Auf der Seite **Ressourcengruppen** des Portals wurde eine neue Ressourcengruppe mit dem **Lösungsnamen** erstellt, den Sie im letzten Schritt angegeben haben. Beachten Sie auch alle Ressourcen, die zur Unterstützung der Gerätesimulationslösung erstellt wurden:

   [![Ressourcen für die Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>Herstellen der Verbindung mit dem IoT Hub

Sie haben jetzt gelernt, wie Sie zwei Gruppen von Ressourcen erstellen, die sich jeweils in einer eigenen Ressourcengruppe befinden:

- Eine leere Time Series Insights-Umgebung.
- Per Solution Accelerator generierte Ressourcen für die Gerätesimulationslösung, einschließlich IoT Hub

Beachten Sie, dass die simulierten Geräte für das Streamen von Gerätedaten über eine Verbindung mit einem IoT Hub verfügen müssen. Zum Einrichten des Datenflusses in die Time Series Insights-Umgebung müssen Sie sowohl Konfigurationsänderungen an Ihrem IoT Hub als auch an der Time Series Insights-Umgebung vornehmen.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IoT Hub-Konfiguration: Definieren einer Consumergruppe

Der IoT Hub stellt verschiedene Endpunkte für das Teilen von Funktionalität mit anderen Akteuren bereit. Der Endpunkt „Ereignisse“ ermöglicht anderen Anwendungen die Nutzung der Daten, wenn diese an eine IoT Hub-Instanz gestreamt werden. „Consumergruppen“ verfügen über einen speziellen Mechanismus, über den Anwendungen lauschen und Daten per Pullvorgang vom IoT Hub übertragen können.

Als Nächstes definieren Sie die neue Eigenschaft **Consumergruppe** auf dem IoT Hub-Endpunkt **Ereignisse** der Gerätesimulationslösung.

1. Navigieren Sie im Azure-Portal auf die Seite **Übersicht** der Ressourcengruppe, die Sie für die Gerätesimulationslösung erstellt haben, und wählen Sie anschließend die IoT Hub-Ressource aus:

   [![Ressourcengruppe für die Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Notieren Sie sich auch den **Namen** der IoT Hub-Ressource, die für die Lösung generiert wurde, da Sie ihn später noch benötigen.

1. Scrollen Sie nach unten, und wählen Sie die Seite **Endpunkte** und dann den Endpunkt **Ereignisse** aus. Geben Sie auf der Seite mit den **Eigenschaften** für den Endpunkt unter der Consumergruppe „$Default“ einen eindeutigen Namen für Ihren Endpunkt ein, und klicken Sie anschließend auf **Speichern**:

   [![IoT Hub-Endpunkte für Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Konfiguration der Umgebung: Definieren einer Ereignisquelle

Stellen Sie jetzt eine Verbindung zwischen dem neuen IoT Hub-Ereignisendpunkt **Consumergruppe** und der Time Series Insights-Umgebung (als **Ereignisquelle**) her.

1. Navigieren Sie zur Seite **Übersicht** der Ressourcengruppe, die Sie für die Time Series Insights-Umgebung erstellt haben, und wählen Sie dann die Time Series Insights-Umgebung aus:

   [![Time Series Insights-Umgebung: Ressourcengruppe und Umgebung](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Wählen Sie auf der Seite für die Time Series Insights-Umgebung die Option **Ereignisquellen** aus, und klicken Sie anschließend auf **+ Hinzufügen**.

   [![Time Series Insights-Umgebung: Übersicht](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Geben Sie auf der Seite **Neue Ereignisquelle** die erforderlichen Parameter ein:

   Parameter|BESCHREIBUNG
   ---|---
   **Name der Ereignisquelle** | Hier ist ein eindeutiger Wert erforderlich, der zum Benennen der Ereignisquelle verwendet wird.
   **Quelle** | Wählen Sie **IoT Hub** aus.
   **Importoption** | Wählen Sie den Standardwert `Use IoT hub from available subscriptions` aus. Diese Option bewirkt, dass die nächste Dropdownliste mit den verfügbaren Abonnements aufgefüllt wird.
   **Abonnement** | Wählen Sie dasselbe Abonnement aus, in dem Sie die Time Series Insights-Umgebung und die Ressourcen für die Gerätesimulation erstellt haben.
   **IoT Hub-Name** | Hier sollte standardmäßig der zuvor notierte IoT Hub-Name angezeigt werden. Wählen Sie den richtigen IoT Hub aus, falls dies nicht der Fall ist.
   **IoT Hub-Richtlinienname** | Wählen Sie **iothubowner** aus.
   **IoT Hub-Consumergruppe** | Hier sollte standardmäßig der Name der zuvor erstellten IoT Hub-Consumergruppe angezeigt werden. Wählen Sie den richtigen Consumergruppennamen aus, wenn dies nicht der Fall ist.
   **Ereignisserialisierungsformat** | Behalten Sie den Standardwert `JSON` bei.
   **Name der Timestamp-Eigenschaft** | Geben Sie `timestamp` an.

   Wenn Sie fertig sind, können Sie auf **Erstellen** klicken, um die Ereignisquelle hinzuzufügen. Wenn Sie auf die Seite **Übersicht** zurückkehren, wird zusammen mit Ihrer Time Series Insights-Umgebung die neue Ressource „Time Series Insights-Ereignisquelle“ angezeigt.

   [![Time Series Insights-Umgebung: Neue Ereignisquelle](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Ausführen einer Gerätesimulation zum Streamen von Daten

Nachdem nun die gesamte Konfigurationsarbeit abgeschlossen ist, ist es an der Zeit, die Time Series Insights-Umgebung mit Beispieldaten von den simulierten Geräten aufzufüllen.

Im [Abschnitt „Erstellen einer Gerätesimulation“](#create-a-device-simulation) wurden per Accelerator mehrere Azure-Ressourcen zur Unterstützung der Lösung erstellt. Zusammen mit dem oben beschriebenen IoT Hub wurde eine Azure App Service-Webanwendung generiert, um Telemetriedaten für simulierte Geräte zu erstellen und zu übertragen.

1. Wechseln Sie zurück zum [Solution Accelerators-Dashboard](https://www.azureiotsolutions.com/Accelerators#dashboard). Melden Sie sich an, falls dies erforderlich ist, indem Sie dasselbe Azure-Konto wie im gesamten bisherigen Tutorial verwenden. Sie können jetzt unter Ihrer Lösung für die „Gerätesimulation“ auf die Schaltfläche **Starten** klicken:

     [![Solution Accelerators-Dashboard](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. Die Web-App für die Gerätesimulation wird gestartet. Der erste Ladevorgang kann einige Sekunden dauern. Sie werden auch zum Erteilen der Zustimmung zur Gewährung der Berechtigung „Sie anmelden und Ihr Profil lesen“ für die Webanwendung aufgefordert. Mit dieser Berechtigung kann die Anwendung die Benutzerprofilinformationen abrufen, die zur Unterstützung der Funktionsweise einer Anwendung erforderlich sind:

     [![Gerätesimulation – Zustimmung für Webanwendung](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Geben Sie die erforderlichen Parameter ein, nachdem die Seite mit dem **Simulationssetup** geladen wurde:

   Parameter|BESCHREIBUNG
   ---|---
   **IoT-Zielhub** | Wählen Sie **Use pre-provisioned IoT Hub** (Vorab bereitgestellten IoT Hub verwenden) aus.
   **Gerätemodell** | Wählen Sie **Chiller** aus.
   **Anzahl von Geräten**  | Geben Sie unter **Menge** die Zahl `1000` ein.
   **Telemetriefrequenz** | Geben Sie `10` Sekunden ein.
   **Simulationsdauer** | Wählen Sie **End in:** (Endet in:) aus, und geben Sie einen Wert von `5` Minuten ein.

   Klicken Sie auf **Simulation starten**, wenn Sie fertig sind. Die Simulation wird fünf Minuten lang ausgeführt, und alle zehn Sekunden werden Daten von 1.000 simulierten Geräten generiert.  

   [![Setup der Gerätesimulation](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Während der Ausführung der Simulation sehen Sie, dass die Felder **Total messages** (Gesamte Nachrichten) und **Messages per second** (Nachrichten pro Sekunde) ca. alle zehn Sekunden aktualisiert werden. Die Simulation wird nach ca. fünf Minuten beendet, und das **Simulationssetup** wird wieder angezeigt.

   [![Aktive Gerätesimulation](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Überprüfen der Telemetriedaten

In diesem letzten Abschnitt überprüfen Sie, ob die Telemetriedaten in der Time Series Insights-Umgebung generiert und gespeichert wurden. Für die Überprüfung der Daten verwenden Sie den Time Series Insights-Explorer, mit dem Telemetriedaten abgefragt und analysiert werden können.

1. Kehren Sie zur Seite **Übersicht** der Ressourcengruppe in der Time Series Insights-Umgebung zurück. Wählen Sie die Time Series Insights-Umgebung aus.

   [![Time Series Insights-Umgebung: Ressourcengruppe und Umgebung](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Klicken Sie in der Time Series Insights-Umgebung auf der Seite **Übersicht** auf **Time Series Insights-Explorer-URL**, um den Time Series Insights-Explorer zu öffnen.

   [![Time Series Insights-Explorer](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. Der Time Series Insights-Explorer wird über Ihr Azure-Portalkonto geladen und authentifiziert. Beim ersten Anzeigen sehen Sie im Diagrammbereich, dass die Time Series Insights-Umgebung mit den Telemetriedaten der simulierten Geräte aufgefüllt wurde. Verwenden Sie oben links die Dropdownliste, um den Zeitbereich einzugrenzen. Geben Sie dann einen Zeitbereich ein, der groß genug ist, um die Dauer der Gerätesimulation abzudecken. Klicken Sie anschließend auf das Vergrößerungsglas für die Suche:

   [![Time Series Insights-Explorer: Zeitbereichsfilter](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Durch das Eingrenzen des Zeitbereichs kann das Diagramm auf die einzelnen Datenübertragungsspitzen (Bursts), auf den IoT Hub und die Time Series Insights-Umgebung vergrößert werden. Beachten Sie auch oben rechts den Text **Das Streaming wurde abgeschlossen**, unter dem die Gesamtzahl von gefundenen Ereignissen angezeigt wird. Sie können auch den Schieberegler für die **Intervallgröße** verschieben, um die Granularität im Diagramm zu steuern:

   [![Time Series Insights-Explorer: Nach Zeitbereich gefilterte Ansicht](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Darüber hinaus können Sie auch mit der linken Maustaste auf eine Region klicken, um nach einem Bereich zu filtern, und dann mit der rechten Maustaste klicken und die Option **Ereignisse untersuchen** verwenden, um Ereignisdetails in der tabellarischen Ansicht **Ereignisse** anzuzeigen:

   [![Time Series Insights-Explorer: Nach Zeitbereich gefilterte Ansicht mit Ereignissen](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden mehrere ausgeführte Azure-Dienste erstellt, um die Time Series Insights-Umgebung und die Gerätesimulationslösung zu unterstützen. Falls Sie diese Tutorialreihe abbrechen oder später durcharbeiten möchten, empfehlen wir Ihnen, alle Ressourcen zu löschen, um das Anfallen von unnötigen Kosten zu vermeiden.

Gehen Sie im Azure-Portal im Menü auf der linken Seite wie folgt vor:

1. Klicken Sie auf das **Ressourcengruppen**-Symbol, und wählen Sie dann die Ressourcengruppe aus, die Sie für die Time Series Insights-Umgebung erstellt haben. Klicken Sie oben auf der Seite auf **Ressourcengruppe löschen**, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie dann auf **Löschen**.

1. Klicken Sie auf das Symbol **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe aus, die mit dem Solution Accelerator für die Gerätesimulation erstellt wurde. Klicken Sie oben auf der Seite auf **Ressourcengruppe löschen**, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Time Series Insights-Umgebung 
> * Erstellen einer Lösung für die Gerätesimulation mit einem IoT Hub
> * Herstellen einer Verbindung der Time Series Insights-Umgebung mit dem IoT-Hub
> * Ausführen einer Gerätesimulation zum Streamen von Daten in die Time Series Insights-Umgebung
> * Überprüfen der simulierten Telemetriedaten

Sie haben erfahren, wie Sie Ihre eigene Time Series Insights-Umgebung erstellen. Lernen Sie nun, wie Sie eine Webanwendung entwickeln, die Daten aus einer Time Series Insights-Umgebung nutzt:

> [!div class="nextstepaction"]
> [Erstellen einer einseitigen Azure Time Series Insights-Web-App](tutorial-create-tsi-sample-spa.md)
