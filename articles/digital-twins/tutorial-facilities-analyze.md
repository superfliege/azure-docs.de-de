---
title: Analysieren von Ereignissen aus dem Azure Digital Twins-Setup | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Ereignisse in Ihren Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights visualisieren und analysieren.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1366cafe5d2c526e86905c108b9c7b865aac8f69
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323299"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-using-time-series-insights"></a>Tutorial: Visualisieren und Analysieren von Ereignissen in den Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights

Nachdem Sie Ihre Azure Digital Twins-Instanz sowie Ihre Gebäudebereiche bereitgestellt und die benutzerdefinierte Funktion zum Überwachen bestimmter Bedingungen implementiert haben, können Sie die Ereignisse und Daten aus den Gebäudebereichen analysieren, um Trends und Abweichungen zu erkennen. 

Im [ersten Tutorial](tutorial-facilities-setup.md) haben Sie den Raumgraphen eines imaginären Gebäudes mit einem Raum mit Bewegungs-, Kohlendioxid- und Temperatursensoren konfiguriert. Im [zweiten Tutorial](tutorial-facilities-udf.md) haben Sie Ihren Graphen und eine benutzerdefinierte Funktion bereitgestellt. Die Funktion überwacht die Sensorwerte und löst Benachrichtigungen aus, wenn die richtigen Bedingungen erfüllt sind, d. h. wenn der Raum leer ist und die Temperatur- und Kohlendioxidwerte normal sind. In diesem Tutorial erfahren Sie, wie Sie die Benachrichtigungen und Daten aus Ihrem Digital Twins-Setup in Azure Time Series Insights (TSI) integrieren. Sie können die Sensorwerte dann im Zeitverlauf visualisieren und nach Trends suchen, beispielsweise welcher Raum am häufigsten genutzt wird, zu welcher Tageszeit der Raum am häufigsten belegt ist usw. Sie können auch Abweichungen erkennen, beispielsweise in welchen Räumen es wärmer und die Luftqualität schlecht ist oder ob ein Bereich im Gebäude durchgängig hohe Temperaturwerte meldet, die auf eine defekte Klimaanlage hinweisen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Streamen von Daten mithilfe von Event Hubs
> * Analysieren mit Time Series Insights

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie das Azure Digital Twins-Setup [konfiguriert](tutorial-facilities-setup.md) and [bereitgestellt](tutorial-facilities-udf.md) haben. Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie fortfahren:
- Ein [Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Instanz von Azure Digital Twins.
- Die [C#-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) (auf den Arbeitscomputer heruntergeladen und extrahiert).
- [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer zum Ausführen des Beispiels. Führen Sie `dotnet --version` aus, um zu überprüfen, ob die richtige Version installiert ist. 


## <a name="stream-data-using-event-hubs"></a>Streamen von Daten mithilfe von Event Hubs
Mit dem [Event Hubs](../event-hubs/event-hubs-about.md)-Dienst können Sie eine Pipeline zum Streamen Ihrer Daten erstellen. In diesem Abschnitt wird erläutert, wie Sie den Event Hub als Konnektor zwischen Ihren Digital Twins- und Time Series Insights-Instanzen erstellen.

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

1. Klicken Sie im linken Navigationsbereich auf **Ressource erstellen**. 

1. Suchen Sie nach der Option **Event Hubs**, und wählen Sie sie aus. Klicken Sie auf **Create**.

1. Geben Sie einen **Namen** für Ihren Event Hubs-Namespace ein, und wählen Sie den **Tarif** *Standard*, Ihr **Abonnement**, die **Ressourcengruppe**, die Sie für Ihre Digital Twins-Instanz verwendet haben, sowie den **Standort** aus. Klicken Sie auf **Create**. 

1. Navigieren Sie nach Abschluss der Bereitstellung zur *Bereitstellung* des Event Hubs-Namespaces, und klicken Sie auf den Namespace unter **RESSOURCE**.

    ![Event Hub-Namespace](./media/tutorial-facilities-analyze/open-event-hub-ns.png)


1. Klicken Sie oben im Bereich **Übersicht** für den Event Hubs-Namespace auf die Schaltfläche **Event Hub**. 
    ![Event Hub](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Geben Sie einen **Namen** für den Event Hub ein, und klicken Sie auf **Erstellen**. Nach der Bereitstellung wird der Event Hub im Bereich **Event Hubs** des Event Hubs-Namespaces mit dem **STATUS** *Aktiv* angezeigt. Klicken Sie auf den Event Hub, um den Bereich **Übersicht** zu öffnen.

1. Klicken Sie im oberen Bereich auf die Schaltfläche **Consumergruppe**, und geben Sie einen Namen für die Consumergruppe ein, beispielsweise *tsievents*. Klicken Sie auf **Create**.
    ![Event Hub-Consumergruppe](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Nachdem die Consumergruppe erstellt wurde, wird sie in der Liste unten im Bereich **Übersicht** des Event Hubs angezeigt. 

1. Öffnen Sie den Bereich **Freigegebene Zugriffsrichtlinien** für Ihren Event Hub, und klicken Sie auf die Schaltfläche **Hinzufügen**. **Erstellen** Sie eine Richtlinie namens *ManageSend*, und stellen Sie sicher, dass alle Kontrollkästchen aktiviert sind. 

    ![Event Hub-Verbindungszeichenfolgen](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Öffnen Sie die soeben erstellte Richtlinie *ManageSend*, und kopieren Sie die Werte für **Verbindungszeichenfolge – Primärschlüssel** und **Verbindungszeichenfolge – Sekundärschlüssel** in eine temporäre Datei. Sie benötigen diese Werte im nächsten Abschnitt, um einen Endpunkt für den Event Hub zu erstellen.

### <a name="create-endpoint-for-the-event-hub"></a>Erstellen eines Endpunkts für den Event Hub

1. Vergewissern Sie sich im Befehlsfenster, dass Sie sich im Ordner **occupancy-quickstart\src** des Digital Twins-Beispiels befinden.

1. Öffnen Sie die Datei **_actions\createEndpoints.yaml_** im Editor. Ersetzen Sie den Inhalt durch den folgenden Code:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    ```

1. Ersetzen Sie die Platzhalter `Primary_connection_string_for_your_event_hub` durch den Wert von **Verbindungszeichenfolge – Primärschlüssel** für den Event Hub. Stellen Sie sicher, dass die Verbindungszeichenfolge das folgende Format aufweist:
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
```

1. Ersetzen Sie die Platzhalter `Secondary_connection_string_for_your_event_hub` durch den Wert von **Verbindungszeichenfolge – Sekundärschlüssel** für den Event Hub. Stellen Sie sicher, dass die Verbindungszeichenfolge das folgende Format aufweist: 
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
```

1. Ersetzen Sie die Platzhalter `Name_of_your_Event_Hubs_namespace` durch den Namen Ihres Event Hubs-Namespaces.

    > [!IMPORTANT]
    > Geben Sie alle Werte ohne Anführungszeichen ein. Stellen Sie sicher, dass nach den Doppelpunkten in der *YAML*-Datei mindestens ein Leerzeichen vorhanden ist. Sie können den Inhalt der *YAML*-Datei auch mit einer beliebigen YAML-Onlinevalidierung überprüfen, beispielsweise mit [diesem Tool](https://onlineyamltools.com/validate-yaml).


1. Speichern und schließen Sie die Datei. Führen Sie im Befehlsfenster den folgenden Befehl aus, und melden Sie sich mit Ihrem Azure-Konto an, wenn Sie dazu aufgefordert werden.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```
   
   Dieser Befehl erstellt zwei Endpunkte für Ihren Event Hub.

   ![Endpunkte für Event Hubs](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Analysieren mit Time Series Insights

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Ressource erstellen**. 

1. Suchen Sie nach einer neuen **Time Series Insights**-Ressource, und wählen Sie sie aus. Klicken Sie auf **Create**.

1. Geben Sie einen **Namen** für Ihre Time Series Insights-Instanz ein, und wählen Sie dann Ihr **Abonnement** aus. Wählen Sie die **Ressourcengruppe**, die Sie für Ihre Digital Twins-Instanz verwendet haben, und den **Standort** aus. Klicken Sie auf **Create**.

    ![Erstellen einer TSI-Ressource](./media/tutorial-facilities-analyze/create-tsi.png)

1. Öffnen Sie nach Abschluss der Bereitstellung die Time Series Insights-Umgebung und dann den Bereich **Ereignisquellen**. Klicken Sie im oberen Bereich auf die Schaltfläche **Hinzufügen**, um eine Consumergruppe hinzuzufügen.

1. Geben Sie im Bereich **Neue Ereignisquelle** einen **Namen** ein, und stellen Sie sicher, dass die anderen Werte korrekt ausgewählt sind. Wählen Sie *ManageSend* als **Event Hub-Richtliniennamen** und anschließend die im vorherigen Abschnitt erstellte *Consumergruppe* als **Event Hub-Consumergruppe** aus. Klicken Sie auf **Create**.

    ![TSI-Ereignisquelle](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. Öffnen Sie den Bereich **Übersicht** für Ihre Time Series Insights-Umgebung, und klicken Sie oben auf die Schaltfläche **Zur Umgebung wechseln**. Falls eine *Datenzugriffswarnung* angezeigt wird, öffnen Sie den Bereich **Datenzugriffsrichtlinien** für Ihre Time Series Insights-Instanz, klicken Sie auf **Hinzufügen**, wählen Sie die Rolle **Mitwirkender** und anschließend den gewünschten Benutzer aus.

1. Die Schaltfläche **Zur Umgebung wechseln** öffnet den [Time Series Insights-Explorer](../time-series-insights/time-series-insights-explorer.md). Wenn keine Ereignisse angezeigt werden, simulieren Sie Geräteereignisse, indem Sie zum Projekt **_device-connectivity_** des Digital Twins-Beispiels navigieren und `dotnet run` ausführen.

1. Nachdem einige simulierte Ereignisse generiert wurden, kehren Sie zum Time Series Insights-Explorer zurück, und klicken Sie oben auf die Schaltfläche „Aktualisieren“. Daraufhin sollten Analysediagramme für Ihre simulierten Sensordaten erstellt werden. 

    ![TSI-Explorer](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. Im Time Series Insights-Explorer können Sie dann Diagramme und Wärmebilder für verschiedene Ereignisse und Daten von Ihren Räumen, Sensoren und anderen Ressourcen generieren. Klicken Sie auf der linken Seite auf die Dropdownfelder **MEASURE** und **TEILEN NACH**, um eigene Visualisierungen zu erstellen. Wählen Sie beispielsweise im Feld **MEASURE** die Option *Ereignisse* und im Feld **TEILEN NACH** die Option *DigitalTwins-SensorHardwareId* aus, um wie in der folgenden Abbildung dargestellt ein Wärmebild für jeden Sensor zu generieren:

    ![TSI-Explorer](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Klicken Sie im [Azure-Portal](http://portal.azure.com) im Menü auf der linken Seite auf **Alle Ressourcen**, wählen Sie Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie dann auf **Löschen**.
2. Sie können auch die Beispielanwendungen auf Ihrem Arbeitscomputer löschen, falls dies notwendig ist. 


## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie mehr über Raumintelligenzgraphen und Objektmodelle in Azure Digital Twins. 
> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen)

