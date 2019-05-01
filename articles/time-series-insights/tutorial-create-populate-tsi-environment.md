---
title: 'Tutorial: Erstellen einer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie eine Time Series Insights-Umgebung erstellen, die mit Daten von simulierten Geräten aufgefüllt wird.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 12/05/2018
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: c974e011e6f101eab617a370b24306f80c249b90
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718419"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Tutorial: Erstellen einer Azure Time Series Insights-Umgebung

In diesem Tutorial wird der Prozess zum Erstellen einer Time Series Insights-Umgebung (TSI) beschrieben, die mit Daten von simulierten Geräten aufgefüllt wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer TSI-Umgebung 
> * Erstellen einer Lösung für die Gerätesimulation mit einem IoT Hub
> * Herstellen einer Verbindung für die TSI-Umgebung mit dem IoT Hub
> * Ausführen einer Gerätesimulation zum Streamen von Daten in die TSI-Umgebung
> * Überprüfen der simulierten Telemetriedaten

## <a name="video"></a>Video

### <a name="in-this-video-we-show-you-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-that-can-be-used-to-get-started-with-time-series-insights-br"></a>In diesem Video zeigen wir, wie Sie einen Azure IoT Solution Accelerator verwenden, um Daten zu generieren, die für den Einstieg mit Time Series Insights verwendet werden können. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. 

Ihr Azure-Anmeldekonto muss außerdem Mitglied der Rolle „Besitzer“ des Abonnements sein. Informationen finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](/azure/role-based-access-control/role-assignments-portal).

## <a name="overview"></a>Übersicht

In der TSI-Umgebung werden die Daten gesammelt und gespeichert. Nach dem Speichern in der TSI-Umgebung können Sie den [TSI-Explorer](time-series-quickstart.md) und die [TSI-Abfrage-API](/rest/api/time-series-insights/ga-query-api) nutzen, um die Daten abzufragen und zu analysieren.

Wie bei allen Geräten – ob simuliert oder physisch – ist der IoT Hub der Verbindungspunkt, der von Geräten zum sicheren Herstellen einer Verbindung und zum Übertragen von Daten in die Azure-Cloud verwendet wird. Wie in der [Übersicht über TSI](time-series-insights-overview.md) beschrieben, dient der IoT Hub auch als Ereignisquelle zum Streamen von Daten in die TSI-Umgebung. 

In diesem Tutorial wird auch ein [IoT Solution Accelerator](/azure/iot-accelerators/) genutzt, um Beispieltelemetriedaten zu generieren und an den IoT Hub zu streamen. Mit IoT Solution Accelerators werden für Unternehmen geeignete vorkonfigurierte Lösungen bereitgestellt, mit denen Sie die Entwicklung von benutzerdefinierten IoT-Lösungen beschleunigen können. 

## <a name="create-a-tsi-environment"></a>Erstellen einer TSI-Umgebung

Erstellen Sie zunächst unter Ihrem Azure-Abonnement eine TSI-Umgebung:

1. Melden Sie sich unter Verwendung Ihres Azure-Abonnementkontos am [Azure-Portal](https://portal.azure.com) an.  
2. Wählen Sie oben links die Option **+ Ressource erstellen**.  
3. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.  
   
   [![Auswählen der Time Series Insights-Umgebungsressource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

4. Geben Sie auf der Seite **Time Series Insights-Umgebung** die erforderlichen Parameter ein:
   
   Parameter|BESCHREIBUNG
   ---|---
   **Umgebungsname** | Wählen Sie einen eindeutigen Namen für die TSI-Umgebung. Der Name wird vom TSI-Explorer und von der Abfrage-API verwendet.
   **Abonnement** | Abonnements sind Container für Azure-Ressourcen. Wählen Sie das Abonnement aus, unter dem Sie die TSI-Umgebung erstellen möchten.
   **Ressourcengruppe** | Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Wählen Sie eine vorhandene Ressourcengruppe für die TSI-Umgebungsressource aus, oder erstellen Sie eine neue.
   **Location** | Wählen Sie eine Rechenzentrumsregion für Ihre TSI-Umgebung aus. Um zusätzliche Bandbreitenkosten und Latenz zu vermeiden, ist es ratsam, die TSI-Umgebung in derselben Region wie die anderen IoT-Ressourcen zu belassen.
   **Preise für SKUs** | Wählen Sie den erforderlichen Durchsatz aus. Wählen Sie „S1“, um die niedrigsten Kosten zu erzielen und die geringste Anfangskapazität zu erhalten.
   **Kapazität** | Die Kapazität ist der Multiplikator, der auf die Eingangsrate, Speicherkapazität und Kosten der ausgewählten SKU angewendet wird.  Die Kapazität einer Umgebung kann nach der Erstellung geändert werden. Wählen Sie die Kapazität „1“ aus, um die niedrigsten Kosten zu erzielen. 

   Klicken Sie nach Abschluss des Vorgangs auf **Erstellen**, um mit dem Bereitstellungsprozess zu beginnen.

   ![Erstellen einer Time Series Insights-Umgebungsressource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)

5. Sie können die Durchführung der Bereitstellung im Panel **Benachrichtigungen** überwachen. Der Vorgang sollte weniger als eine Minute dauern:  

   ![Bereitstellung der Time Series Insights-Umgebung erfolgreich abgeschlossen](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)

## <a name="create-a-device-simulation"></a>Erstellen einer Gerätesimulation

Erstellen Sie als Nächstes die Lösung für die Gerätesimulation, mit der Testdaten zum Auffüllen Ihrer TSI-Umgebung generiert werden:

1. Navigieren Sie in einem separaten Fenster bzw. auf einer Registerkarte zu https://www.azureiotsolutions.com, melden Sie sich mit demselben Azure-Abonnementkonto an, und wählen Sie den Accelerator „Gerätesimulation“:

   ![Ausführen des Accelerators „Gerätesimulation“](media/tutorial-create-populate-tsi-environment/sa-main.png)

2. Geben Sie auf der Seite **Gerätesimulationslösung erstellen** die erforderlichen Parameter ein:

   Parameter|BESCHREIBUNG
   ---|---
   **Lösungsname** | Ein eindeutiger Wert, der für die Erstellung einer neuen Ressourcengruppe verwendet wird. Die aufgelisteten Azure-Ressourcen werden erstellt und der Ressourcengruppe zugewiesen.
   **Abonnement** | Geben Sie dasselbe Abonnement an, das Sie im vorherigen Abschnitt zum Erstellen Ihrer TSI-Umgebung verwendet haben.
   **Region** | Geben Sie dieselbe Region an, die Sie im vorherigen Abschnitt zum Erstellen Ihrer TSI-Umgebung verwendet haben. 
   **Optionale Azure-Ressourcen bereitstellen** | Lassen Sie die Option **IoT Hub** aktiviert, da sie von den simulierten Geräten für die Verbindungsherstellung und zum Streamen von Daten genutzt wird.

   Klicken Sie nach Abschluss des Vorgangs auf **Lösung erstellen**, um die Azure-Ressourcen der Lösung bereitzustellen. Dieser Vorgang dauert ca. 6 bis 7 Minuten:

   ![Bereitstellen der Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)

3. Nachdem die Bereitstellung abgeschlossen ist, ändert sich der Text über Ihrer neuen Lösung von „Wird bereitgestellt...“ in „Bereit“:

   >[!IMPORTANT]
   > Klicken Sie noch nicht auf die Schaltfläche **Starten**! Lassen Sie diese Webseite aber geöffnet, da Sie später hierhin zurückkehren.

   ![Bereitstellung der Gerätesimulationslösung abgeschlossen](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)

4. Wechseln Sie zurück zum Azure-Portal, und sehen Sie sich die neu erstellten Ressourcen unter Ihrem Abonnement an. Auf der Seite **Ressourcengruppen** des Portals wurde eine neue Ressourcengruppe mit dem **Lösungsnamen** erstellt, den Sie im letzten Schritt angegeben haben. Beachten Sie auch alle Ressourcen, die zur Unterstützung der Gerätesimulationslösung erstellt wurden:

   [![Ressourcen für die Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-tsi-environment-to-the-iot-hub"></a>Herstellen einer Verbindung für die TSI-Umgebung mit dem IoT Hub 

Sie haben jetzt gelernt, wie Sie zwei Gruppen von Ressourcen erstellen, die sich jeweils in einer eigenen Ressourcengruppe befinden: 
- Leere TSI-Umgebung 
- Per Solution Accelerator generierte Ressourcen für die Gerätesimulationslösung, einschließlich IoT Hub

Beachten Sie, dass die simulierten Geräte für das Streamen von Gerätedaten über eine Verbindung mit einem IoT Hub verfügen müssen. Zum Einrichten des Datenflusses in die TSI-Umgebung müssen Sie sowohl Konfigurationsänderungen an Ihrem IoT Hub als auch an der TSI-Umgebung vornehmen. 

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IoT Hub-Konfiguration: Definieren einer Consumergruppe

Der IoT Hub stellt verschiedene Endpunkte für das Teilen von Funktionalität mit anderen Akteuren bereit. Der Endpunkt „Ereignisse“ ermöglicht anderen Anwendungen die Nutzung der Daten, wenn diese an eine IoT Hub-Instanz gestreamt werden. „Consumergruppen“ verfügen über einen speziellen Mechanismus, über den Anwendungen lauschen und Daten per Pullvorgang vom IoT Hub übertragen können. 

Als Nächstes definieren Sie die neue Eigenschaft „Consumergruppe“ auf dem IoT Hub-Endpunkt „Ereignisse“ der Gerätesimulationslösung. 

1. Navigieren Sie im Azure-Portal auf die Seite **Übersicht** der Ressourcengruppe, die Sie für die Gerätesimulationslösung erstellt haben, und wählen Sie anschließend die IoT Hub-Ressource aus:

   [![Ressourcengruppe für die Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Notieren Sie sich auch den **Namen** der IoT Hub-Ressource, die für die Lösung generiert wurde, da Sie ihn später noch benötigen.

2. Scrollen Sie nach unten, und wählen Sie die Seite **Endpunkte** und dann den Endpunkt **Ereignisse** aus. Geben Sie auf der Seite mit den **Eigenschaften** für den Endpunkt unter der Consumergruppe „$Default“ einen eindeutigen Namen für Ihren Endpunkt ein, und klicken Sie anschließend auf **Speichern**:

   [![IoT Hub-Endpunkte für Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="tsi-environment-configuration-define-an-event-source"></a>Konfiguration der TSI-Umgebung: Definieren einer Ereignisquelle

Stellen Sie jetzt eine Verbindung für den neuen IoT Hub-Ereignisendpunkt „Consumergruppe“ zur TSI-Umgebung her (als „Ereignisquelle“).

1. Navigieren Sie zur Seite **Übersicht** der Ressourcengruppe, die Sie für die TSI-Umgebung erstellt haben, und wählen Sie dann die TSI-Umgebung aus:

   [![TSI-Umgebung – Ressourcengruppe und Umgebung](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

2. Wählen Sie auf der Seite für die TSI-Umgebung die Option **Ereignisquellen**, und klicken Sie anschließend auf **+ Hinzufügen**:

   ![TSI-Umgebung – Übersicht](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)

3. Geben Sie auf der Seite **Neue Ereignisquelle** die erforderlichen Parameter ein: 

   Parameter|BESCHREIBUNG
   ---|---
   **Name der Ereignisquelle** | Hier ist ein eindeutiger Wert erforderlich, der zum Benennen der Ereignisquelle verwendet wird.
   **Quelle** | Wählen Sie „IoT Hub“.
   **Importoption** | Verwenden Sie den Standardwert „IoT Hub aus verfügbaren Abonnements verwenden“. Diese Option bewirkt, dass die nächste Dropdownliste mit den verfügbaren Abonnements aufgefüllt wird.
   **Abonnement** | Wählen Sie das gleiche Abonnement aus, unter dem Sie die TSI-Umgebung und die Ressourcen für die Gerätesimulation erstellt haben.
   **IoT Hub-Name** | Hier sollte standardmäßig der zuvor notierte IoT Hub-Name angezeigt werden. Wählen Sie den richtigen IoT Hub aus, falls dies nicht der Fall ist.
   **IoT Hub-Richtlinienname** | Behalten Sie den Standardwert „iothubowner“ bei.
   **IoT Hub-Consumergruppe** | Hier sollte standardmäßig der Name der zuvor erstellten IoT Hub-Consumergruppe angezeigt werden. Wählen Sie den richtigen Consumergruppennamen aus, wenn dies nicht der Fall ist. 
   **Ereignisserialisierungsformat** | Behalten Sie den Standardwert „JSON“ bei.
   **Name der Timestamp-Eigenschaft** | Geben Sie „timestamp“ an.

   Wenn Sie fertig sind, können Sie auf **Erstellen** klicken, um die Ereignisquelle hinzuzufügen. Wenn Sie auf die Seite **Übersicht** zurückkehren, wird zusammen mit Ihrer TSI-Umgebung die neue Ressource „Time Series Insights-Ereignisquelle“ angezeigt.

   ![Neue Ereignisquelle für TSI-Umgebung](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)

## <a name="run-a-device-simulation-to-stream-data-into-the-tsi-environment"></a>Ausführen einer Gerätesimulation zum Streamen von Daten in die TSI-Umgebung

Nachdem nun die gesamte Konfigurationsarbeit abgeschlossen ist, ist es an der Zeit, die TSI-Umgebung mit Beispieldaten von den simulierten Geräten aufzufüllen.

Im [Abschnitt „Erstellen einer Gerätesimulation“](#create-a-device-simulation) wurden per Accelerator mehrere Azure-Ressourcen zur Unterstützung der Lösung erstellt. Zusammen mit dem oben beschriebenen IoT Hub wurde eine Azure App Service-Webanwendung generiert, um Telemetriedaten für simulierte Geräte zu erstellen und zu übertragen.

1. Wechseln Sie zurück zum [Solution Accelerators-Dashboard](https://www.azureiotsolutions.com/Accelerators#dashboard). Melden Sie sich an, falls dies erforderlich ist, indem Sie dasselbe Azure-Konto wie im gesamten bisherigen Tutorial verwenden. Sie können jetzt unter Ihrer Lösung für die „Gerätesimulation“ auf die Schaltfläche **Starten** klicken:

     ![Solution Accelerators-Dashboard](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)

2. Die Web-App für die Gerätesimulation wird gestartet. Der erste Ladevorgang kann einige Sekunden dauern. Sie werden auch zum Erteilen der Zustimmung zur Gewährung der Berechtigung „Sie anmelden und Ihr Profil lesen“ für die Webanwendung aufgefordert. Mit dieser Berechtigung kann die Anwendung die Benutzerprofilinformationen abrufen, die zur Unterstützung der Funktionsweise einer Anwendung erforderlich sind:

     ![Gerätesimulation – Zustimmung für Webanwendung](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)

3. Geben Sie die erforderlichen Parameter ein, nachdem die Seite mit dem **Simulationssetup** geladen wurde: 

   Parameter|BESCHREIBUNG
   ---|---
   **IoT-Zielhub** | Wählen Sie „Use pre-provisioned IoT Hub“ (Vorab bereitgestellten IoT Hub verwenden).
   **Gerätemodell** | Wählen Sie „Chiller“ (Kältemaschine).
   **Anzahl von Geräten**  | Geben Sie unter **Menge** die Zahl „1.000“ ein.
   **Telemetriefrequenz** | Geben Sie 10 Sekunden ein.
   **Simulationsdauer** | Wählen Sie **End in:** (Endet in:), und geben Sie einen Wert von 5 Minuten ein.

   Klicken Sie auf **Simulation starten**, wenn Sie fertig sind. Die Simulation wird fünf Minuten lang ausgeführt, und alle zehn Sekunden werden Daten von 1.000 simulierten Geräten generiert.  

   ![Setup der Gerätesimulation](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)

4. Während der Ausführung der Simulation sehen Sie, dass die Felder **Total messages** (Gesamte Nachrichten) und **Messages per second** (Nachrichten pro Sekunde) ca. alle zehn Sekunden aktualisiert werden. Die Simulation wird nach ca. fünf Minuten beendet, und das **Simulationssetup** wird wieder angezeigt.

   ![Aktive Gerätesimulation](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)

## <a name="verify-the-telemetry-data"></a>Überprüfen der Telemetriedaten

In diesem letzten Abschnitt überprüfen Sie, ob die Telemetriedaten in der TSI-Umgebung generiert und gespeichert wurden. Für die Überprüfung der Daten verwenden Sie den Time Series Insights-Explorer, mit dem Telemetriedaten abgefragt und analysiert werden können.

1. Wechseln Sie zurück zur Seite **Übersicht** für die Ressourcengruppe der TSI-Umgebung, und wählen Sie dann erneut die TSI-Umgebung aus:

   [![TSI-Umgebung – Ressourcengruppe und Umgebung](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

2. Klicken Sie in der TSI-Umgebung auf der Seite **Übersicht** auf **Time Series Insights-Explorer-URL**, um den TSI-Explorer zu öffnen:

   [![TSI-Explorer](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

3. Der TSI-Explorer wird über Ihr Azure-Portalkonto geladen und authentifiziert. Beim ersten Anzeigen sehen Sie im Diagrammbereich, dass die TSI-Umgebung mit den Telemetriedaten der simulierten Geräte aufgefüllt wurde. Verwenden Sie oben links die Dropdownliste, um den Zeitbereich einzugrenzen. Geben Sie dann einen Zeitbereich ein, der groß genug ist, um die Dauer der Gerätesimulation abzudecken. Klicken Sie anschließend auf das Vergrößerungsglas für die Suche:

   [![TSI-Explorer – Filter für Zeitbereich](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

4. Durch das Eingrenzen des Zeitbereichs kann das Diagramm auf die einzelnen Datenübertragungsspitzen (Bursts), auf den IoT Hub und die TSI-Umgebung vergrößert werden. Beachten Sie auch oben rechts den Text **Das Streaming wurde abgeschlossen**, unter dem die Gesamtzahl von gefundenen Ereignissen angezeigt wird. Sie können auch den Schieberegler für die **Intervallgröße** verschieben, um die Granularität im Diagramm zu steuern:

   [![TSI-Explorer – Ansicht mit gefiltertem Zeitbereich](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

5. Darüber hinaus können Sie auch mit der linken Maustaste auf eine Region klicken, um nach einem Bereich zu filtern, und dann mit der rechten Maustaste klicken und die Option **Ereignisse untersuchen** verwenden, um Ereignisdetails in der tabellarischen Ansicht **Ereignisse** anzuzeigen:

   [![TSI-Explorer – Ansicht mit gefiltertem Zeitbereich und Ereignissen](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden mehrere ausgeführte Azure-Dienste erstellt, um die TSI-Umgebung und die Gerätesimulationslösung zu unterstützen. Falls Sie diese Tutorialreihe abbrechen oder später durcharbeiten möchten, empfehlen wir Ihnen, alle Ressourcen zu löschen, um das Anfallen von unnötigen Kosten zu vermeiden. 

Gehen Sie im Azure-Portal im Menü auf der linken Seite wie folgt vor:

1. Klicken Sie auf das Symbol **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe aus, die Sie für die TSI-Umgebung erstellt haben. Klicken Sie oben auf der Seite auf **Ressourcengruppe löschen**, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie dann auf **Löschen**. 
2. Klicken Sie auf das Symbol **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe aus, die mit dem Solution Accelerator für die Gerätesimulation erstellt wurde. Klicken Sie oben auf der Seite auf **Ressourcengruppe löschen**, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie dann auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer TSI-Umgebung 
> * Erstellen einer Lösung für die Gerätesimulation mit einem IoT Hub
> * Herstellen einer Verbindung für die TSI-Umgebung mit dem IoT Hub
> * Ausführen einer Gerätesimulation zum Streamen von Daten in die TSI-Umgebung
> * Überprüfen der simulierten Telemetriedaten

Sie haben erfahren, wie Sie Ihre eigene TSI-Umgebung erstellen. Lernen Sie nun, wie Sie eine Webanwendung entwickeln, die Daten aus einer TSI-Umgebung nutzt:

> [!div class="nextstepaction"]
> [Erstellen einer einseitigen Azure Time Series Insights-Web-App](tutorial-create-tsi-sample-spa.md)


