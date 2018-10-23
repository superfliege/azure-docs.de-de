---
title: Erfassen von Ereignissen in einem Azure Digital Twins-Gebäudebereich | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Benachrichtigungen von Ihren Gebäudebereichen empfangen, indem Sie Azure Digital Twins in Logic Apps integrieren.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 91dd16938efbd1e24419352f66e3238646a77e8a
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323283"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-using-logic-apps"></a>Tutorial: Empfangen von Benachrichtigungen von den Azure Digital Twins-Gebäudebereichen mit Azure Logic Apps

Nachdem Sie Ihre Azure Digital Twins-Instanz sowie Ihre Gebäudebereiche bereitgestellt und benutzerdefinierte Funktionen zum Überwachen bestimmter Bedingungen implementiert haben, können Sie Ihren Büroadministrator per E-Mail benachrichtigen, wenn die überwachten Bedingungen auftreten. 

Im [ersten Tutorial](tutorial-facilities-setup.md) haben Sie den Raumgraphen eines imaginären Gebäudes mit einem Raum mit Bewegungs-, Kohlendioxid- und Temperatursensoren konfiguriert. Im [zweiten Tutorial](tutorial-facilities-udf.md) haben Sie den Graphen und eine benutzerdefinierte Funktion bereitgestellt, um diese Sensorwerte zu überwachen und Benachrichtigungen auszulösen, wenn der Raum leer ist und die Temperatur- und Kohlendioxidwerte im zulässigen Bereich liegen. In diesem Tutorial erfahren Sie, wie Sie diese Benachrichtigungen in Azure Logic Apps integrieren, um E-Mails zu senden, wenn ein solcher Raum verfügbar ist. Mithilfe dieser Informationen kann ein Büroadministrator die Mitarbeiter beim Buchen des am besten geeigneten Besprechungsraums unterstützen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren von Ereignissen in Event Grid
> * Senden von Benachrichtigungen über Ereignisse mit einer Logik-App
    
## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie das Azure Digital Twins-Setup [konfiguriert](tutorial-facilities-setup.md) and [bereitgestellt](tutorial-facilities-udf.md) haben. Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie fortfahren:
- Ein [Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Instanz von Azure Digital Twins.
- Die [C#-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) (auf den Arbeitscomputer heruntergeladen und extrahiert).
- [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer zum Ausführen des Beispiels. Führen Sie `dotnet --version` aus, um zu überprüfen, ob die richtige Version installiert ist. 
- Ein Office 365-Konto zum Senden von Benachrichtigungs-E-Mails.

## <a name="integrate-events-with-event-grid"></a>Integrieren von Ereignissen in Event Grid 
In diesem Abschnitt richten Sie ein [Event Grid](../event-grid/overview.md)-Thema ein, um Ereignisse von Ihrer Digital Twins-Instanz zu erfassen, die Sie dann an einen [Ereignishandler](../event-grid/event-handlers.md) wie Logic Apps umleiten.

### <a name="create-event-grid-topic"></a>Erstellen eines Event Grid-Themas
[Event Grid-Themen](../event-grid/concepts.md#topics) stellen eine Schnittstelle zum Weiterleiten der Ereignisse bereit, die von der benutzerdefinierten Funktion generiert werden. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie im linken Navigationsbereich auf **Ressource erstellen**. 

1. Suchen Sie nach der Option **Event Grid-Thema**, und wählen Sie sie aus. Klicken Sie auf **Create**.

1. Geben Sie einen **Namen** für Ihr Event Grid-Thema ein, und wählen Sie das **Abonnement** aus. Wählen Sie die **Ressourcengruppe**, die Sie für Ihre Digital Twins-Instanz verwendet oder erstellt haben, und den **Standort** aus. Klicken Sie auf **Create**. 

    ![Erstellen eines Event Grid-Themas](./media/tutorial-facilities-events/create-event-grid-topic.png)

1. Navigieren Sie in Ihrer Ressourcengruppe zum Event Grid-Thema, klicken Sie auf **Übersicht**, und kopieren Sie den Wert für **Themenendpunkt** in eine temporäre Datei. Diese URL benötigen Sie im nächsten Abschnitt. 

1. Klicken Sie auf **Zugriffsschlüssel**, und kopieren Sie **Schlüssel 1** und **Schlüssel 2** in eine temporäre Datei. Sie benötigen diese Werte im nächsten Abschnitt, um den Endpunkt zu erstellen.

    ![Event Grid-Schlüssel](./media/tutorial-facilities-events/event-grid-keys.png)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Erstellen eines Endpunkts für das Event Grid-Thema

1. Vergewissern Sie sich im Befehlsfenster, dass Sie sich im Ordner **_occupancy-quickstart\src_** des Digital Twins-Beispiels befinden.

1. Öffnen Sie die Datei **_actions\createEndpoints.yaml_** im Visual Studio Code-Editor. Stellen Sie sicher, dass die Datei Folgendes enthält:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_Event_Grid
      secondaryConnectionString: Secondary_connection_string_for_your_Event_Grid
      path: Event_Grid_Topic_Path
    ```

1. Ersetzen Sie den Platzhalter `Primary_connection_string_for_your_Event_Grid` durch den Wert von **Schlüssel 1**. 

1. Ersetzen Sie den Platzhalter `Secondary_connection_string_for_your_Event_Grid` durch den Wert von **Schlüssel 2**.

1. Ersetzen Sie den Platzhalter `Event_Grid_Topic_Path` durch den Pfad des Event Grid-Themas. Diesen Pfad erhalten Sie, indem Sie *https://* und die nachgestellten Ressourcenpfade aus der **Themenendpunkt**-URL entfernen. Der Pfad sollte in etwa wie folgt aussehen: **IhrEventGridName.IhrStandort.eventgrid.azure.net**. 

    > [!IMPORTANT]
    > Geben Sie alle Werte ohne Anführungszeichen ein. Stellen Sie sicher, dass nach den Doppelpunkten in der *YAML*-Datei mindestens ein Leerzeichen vorhanden ist. Sie können den Inhalt der *YAML*-Datei auch mit einer beliebigen YAML-Onlinevalidierung überprüfen, beispielsweise mit [diesem Tool](https://onlineyamltools.com/validate-yaml).

1. Speichern und schließen Sie die Datei. Führen Sie im Befehlsfenster den folgenden Befehl aus, und melden Sie sich an, wenn Sie dazu aufgefordert werden. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Dieser Befehl erstellt den Endpunkt für das Event Grid-Thema. 

   ![Endpunkte für Event Grid](./media/tutorial-facilities-events/dotnet-create-endpoints.png)


## <a name="notify-events-with-logic-app"></a>Senden von Benachrichtigungen über Ereignisse mit einer Logik-App
Der [Azure Logic Apps](../logic-apps/logic-apps-overview.md)-Dienst ermöglicht Ihnen das Erstellen von automatisierten Aufgaben für Ereignisse, die von anderen Diensten empfangen werden. In diesem Abschnitt richten Sie Logic Apps mithilfe eines [Event Grid-Themas](../event-grid/overview.md) zum Erstellen von E-Mail-Benachrichtigungen für Ereignisse ein, die von Ihren Raumsensoren weitergeleitet werden.

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Ressource erstellen**.

1. Suchen Sie nach einer neuen **Logik-App**-Ressource, und wählen Sie sie aus. Klicken Sie auf **Create**.

1. Geben Sie einen **Namen** für die Logik-App ein, und wählen Sie dann Ihr **Abonnement**, Ihre **Ressourcengruppe** sowie Ihren **Standort** aus. Klicken Sie auf **Create**.

    ![Erstellen einer Logik-App](./media/tutorial-facilities-events/create-logic-app.png)

1. Öffnen Sie die Logik-App, nachdem sie bereitgestellt wurde, und öffnen Sie dann den Bereich **Logik-App-Designer**. 

1. Wählen Sie den Trigger **Bei Eintritt eines Event Grid-Ereignisses** aus. **Melden Sie** sich mit Ihrem Azure-Konto bei Ihrem Mandanten an, wenn Sie dazu aufgefordert werden. Wählen Sie **Zugriff zulassen** aus, wenn Sie dazu aufgefordert werden, um zu bestätigen, dass der Zugriff auf Ihr Event Grid-Thema zulässig ist. Klicken Sie auf **Weiter**.

1. Führen Sie im Fenster **When a resource event occurs (Preview)** (Bei Auftreten eines Ressourcenereignisses (Vorschau)) die folgenden Schritte aus: 
    1. Wählen Sie das **Abonnement** aus, das Sie zuvor zum Erstellen des Event Grid-Themas verwendet haben.

    1. Wählen Sie **Microsoft.EventGrid.Topics** als **Ressourcentyp** aus.

    1. Wählen Sie Ihre Event Grid-Ressource im Dropdownfeld **Ressourcenname** aus.

    ![Erstellen einer Logik-App](./media/tutorial-facilities-events/logic-app-resource-event.png)

1. Klicken Sie auf die Schaltfläche **Neuer Schritt**.

1. Führen Sie im Fenster **Aktion auswählen** die folgenden Schritte aus:
    1. Suchen Sie nach dem Ausdruck *json analysieren*, und wählen Sie die Aktion **JSON analysieren** aus.

    1. Klicken Sie in das Feld **Inhalt**, und wählen Sie in der Liste **Dynamischer Inhalt** die Option **Text** aus.

    1. Klicken Sie auf **Beispielnutzlast zum Generieren eines Schemas verwenden**. Fügen Sie die folgende JSON-Nutzlast ein, und klicken Sie dann auf **Fertig**.

        ```JSON
        {
        "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
        "subject": "UdfCustom",
        "data": {
          "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
          "ResourceType": "Space",
          "Payload": "\"Air quality is poor.\"",
          "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
        },
        "eventType": "UdfCustom",
        "eventTime": "0001-01-01T00:00:00Z",
        "dataVersion": "1.0",
        "metadataVersion": "1",
        "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
        }
        ```
    
    Diese Nutzlast enthält fiktive Werte. Die Logik-App verwendet diese Beispielnutzlast zum Generieren eines **Schemas**.
    
    ![Logik-App – „JSON analysieren“ für Event Grid](./media/tutorial-facilities-events/logic-app-parse-json.png)

1. Klicken Sie auf die Schaltfläche **Neuer Schritt**.

1. Führen Sie im Fenster **Aktion auswählen** die folgenden Schritte aus:
    1. Suchen Sie in der Liste **Aktionen** nach der Option **Condition Control** (Bedingungs-Steuerelement), und wählen Sie sie aus. 

    1. Klicken Sie in das erste Textfeld **Wählen Sie einen Wert aus**, und wählen Sie die Option **eventType** in der Liste **Dynamischer Inhalt** für das Fenster **JSON analysieren** aus.

    1. Klicken Sie in das zweite Textfeld **Wählen Sie einen Wert aus**, und geben Sie *UdfCustom* ein.

    ![Logik-App – „JSON analysieren“ für Event Grid](./media/tutorial-facilities-events/logic-app-condition.png)

1. Führen Sie im Fenster **Bei TRUE** die folgenden Schritte aus:
    1. Klicken Sie auf **Aktion hinzufügen**, und wählen Sie *Office 365 Outlook* aus.

    1. Wählen Sie in der Liste **Aktionen** die Option **E-Mail senden** aus. Klicken Sie auf **Anmelden**, und verwenden Sie die Anmeldeinformationen für Ihr E-Mail-Konto. Klicken Sie auf **Zugriff zulassen**, wenn Sie dazu aufgefordert werden.

    1. Geben Sie im Feld **An** Ihre E-Mail-ID ein, um Benachrichtigungen zu erhalten. Geben Sie im Feld **Betreff** den Text *Digital Twins-Benachrichtigung für schlechte Luftqualität in Gebäudebereich* ein, und wählen Sie dann **TopologyObjectId** in der Liste **Dynamischer Inhalt** für **JSON analysieren** aus.

    1. Geben Sie im Feld **Text** des gleichen Fensters einen Text wie den folgenden ein: *In einem Raum wurde eine schlechte Luftqualität erkannt, und die Temperatur muss angepasst werden*. Sie können wie unten dargestellt mit den Elementen in der Liste **Dynamischer Inhalt** genauere Details angeben.

    ![Logik-App – „E-Mail senden“](./media/tutorial-facilities-events/logic-app-send-email.png)

1. Klicken Sie oben im Bereich **Logik-App-Designer** auf **Speichern**.

1. Stellen Sie sicher, dass Sie Sensordaten simulieren. Navigieren Sie dazu in einem Befehlsfenster zum Ordner **_device-connectivity_** des Digital Twins-Beispiels, und führen Sie den Befehl `dotnet run` aus.

Nach einigen Minuten sollten Sie die ersten E-Mail-Benachrichtigungen von der Logik-App erhalten. 

   ![Logik-App – „E-Mail senden“](./media/tutorial-facilities-events/logic-app-notification.png)

Wenn Sie keine E-Mails mehr empfangen möchten, navigieren Sie im Portal zu Ihrer **Logik-App**, und wählen Sie den Bereich **Übersicht** aus. Klicken Sie auf **Deaktivieren**.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Klicken Sie im [Azure-Portal](http://portal.azure.com) im Menü auf der linken Seite auf **Alle Ressourcen**, wählen Sie Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie dann auf **Löschen**.
2. Sie können auch die Beispielanwendungen auf Ihrem Arbeitscomputer löschen, falls dies notwendig ist. 


## <a name="next-steps"></a>Nächste Schritte

Sie können nun mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Sensordaten visualisieren, Trends analysieren und Abweichungen erkennen. 
> [!div class="nextstepaction"]
> [Tutorial: Visualisieren und Analysieren von Ereignissen in den Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights](tutorial-facilities-analyze.md)

Sie können sich auch ausführlicher über die Raumintelligenzgraphen und Objektmodelle in Azure Digital Twins informieren. 
> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen)