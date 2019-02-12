---
title: Beispiele für die Integration von Azure Service Bus in Event Grid | Microsoft-Dokumentation
description: Dieser Artikel enthält Beispiele für die Integration von Service Bus-Messaging und Event Grid.
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 09/15/2018
ms.author: spelluru
ms.openlocfilehash: a1c0078cf2359de7fa018af749c61880caa8fcdd
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821806"
---
# <a name="azure-service-bus-to-azure-event-grid-integration-examples"></a>Beispiele für die Integration von Azure Service Bus in Azure Event Grid

In diesem Artikel wird beschrieben, wie Sie eine Azure-Funktion und eine Logik-App einrichten, die jeweils basierend auf dem Empfang eines Ereignisses von Azure Event Grid Nachrichten erhalten. Hierbei gehen Sie wie folgt vor:
 
* Erstellen Sie eine einfache Azure-Testfunktion zum Debuggen und Anzeigen des ursprünglichen Ereignisflusses aus Event Grid. Führen Sie diesen Schritt unabhängig davon aus, ob Sie die anderen Schritte ausführen.
* Erstellen Sie eine Azure-Funktion zum Empfangen und Verarbeiten von Azure Service Bus-Nachrichten basierend auf Event Grid-Ereignissen.
* Nutzen Sie das Logic Apps-Feature von Azure App Service.

Im von Ihnen erstellten Beispiel wird vorausgesetzt, dass das Service Bus-Thema über zwei Abonnements verfügt. Außerdem wird im Beispiel davon ausgegangen, dass das Event Grid-Abonnement erstellt wurde, um nur für ein Service Bus-Abonnement Ereignisse zu senden. 

Im Beispiel senden Sie Nachrichten an das Service Bus-Thema und vergewissern sich anschließend, dass das Ereignis für dieses Service Bus-Abonnement generiert wurde. Die Funktion oder Logik-App empfängt die Nachrichten vom Service Bus-Abonnement und führt dann den Vorgang durch.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie zunächst sicher, dass Sie die Schritte der nächsten beiden Abschnitte ausgeführt haben.

### <a name="create-a-service-bus-namespace"></a>Erstellen eines Service Bus-Namespace

Erstellen Sie einen Service Bus Premium-Namespace und ein Service Bus-Thema mit zwei Abonnements.

### <a name="send-a-message-to-the-service-bus-topic"></a>Senden einer Nachricht an das Service Bus-Thema

Sie können beliebige Verfahren zum Senden einer Nachricht an Ihr Service Bus-Thema verwenden. Im Beispielcode am Ende dieses Verfahrens wird vorausgesetzt, dass Sie Visual Studio 2017 verwenden.

1. Klonen Sie das [GitHub-Repository „azure-service-bus“](https://github.com/Azure/azure-service-bus/).

1. Navigieren Sie in Visual Studio zum Ordner *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration*, und öffnen Sie die Datei *SBEventGridIntegration.sln*.

1. Navigieren Sie zum Projekt **MessageSender**, und wählen Sie **Program.cs**.

   ![8][]

1. Geben Sie Ihren Themennamen und die Verbindungszeichenfolge ein, und führen Sie dann den folgenden Konsolenanwendungscode aus:

    ```CSharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```

## <a name="set-up-a-test-function"></a>Einrichten einer Testfunktion

Richten Sie vor dem Durcharbeiten des gesamten Szenarios mindestens eine kleine Testfunktion ein, mit der Sie das Debuggen durchführen und verfolgen können, welche Ereignisse eintreten.

1. Erstellen Sie im Azure-Portal eine neue Azure Functions-Anwendung. Informationen zu den Grundlagen von Azure Functions finden Sie in der [Azure Functions-Dokumentation](https://docs.microsoft.com/azure/azure-functions/).

1. Wählen Sie in der neu erstellten Funktion das kleine Pluszeichen (+) aus, um eine HTTP-Triggerfunktion hinzuzufügen:

    ![2][]
    
    Das Fenster **Erste Schritte mit einer vorgefertigten Funktion** wird geöffnet.

    ![3][]

1. Wählen Sie die Schaltfläche **Webhook + API**, die Option **CSharp** und dann **Diese Funktion erstellen**.
 
1. Fügen Sie den folgenden Code in die Funktion ein:

    ```CSharp
    #r "Newtonsoft.Json"
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
        // parse query parameter
        var content = req.Content;
    
        string jsonContent = await content.ReadAsStringAsync(); 
        log.Info($"Received Event with payload: {jsonContent}");
    
    IEnumerable<string> headerValues;
    if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
    {
    var validationHeaderValue = headerValues.FirstOrDefault();
    if(validationHeaderValue == "SubscriptionValidation")
    {
    var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
         var code = events[0].Data["validationCode"];
         return req.CreateResponse(HttpStatusCode.OK,
         new { validationResponse = code });
    }
    }
    
        return jsonContent == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    ```

1. Klicken Sie auf **Speichern und ausführen**.

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Verbinden der Funktion und des Namespace per Event Grid

In diesem Abschnitt verknüpfen Sie die Funktion und den Service Bus-Namespace. In diesem Beispiel wird das Azure-Portal verwendet. Informationen zur Verwendung von PowerShell oder der Azure CLI für dieses Verfahren finden Sie unter [Übersicht über die Integration von Azure Service Bus in Azure Event Grid](service-bus-to-event-grid-integration-concept.md).

Gehen Sie wie folgt vor, um ein Azure Event Grid-Abonnement zu erstellen:
1. Navigieren Sie im Azure-Portal zu Ihrem Namespace, und wählen Sie im Bereich auf der linken Seite dann **Event Grid**.  
    Ihr Namespacefenster wird geöffnet, und im rechten Bereich werden zwei Event Grid-Abonnements angezeigt.

    ![20][]

1. Klicken Sie auf **Ereignisabonnement**.  
    Das Fenster **Ereignisabonnement** wird geöffnet. In der folgenden Abbildung ist ein Formular zum Abonnieren einer Azure-Funktion oder eines Webhooks ohne Anwendung von Filtern dargestellt.

    ![21][]

1. Füllen Sie das Formular wie gezeigt aus, und denken Sie daran, im Feld **Suffixfilter** den relevanten Filter einzugeben.

1. Klicken Sie auf **Erstellen**.

1. Senden Sie eine Nachricht an Ihr Service Bus-Thema, wie im Abschnitt „Voraussetzungen“ erwähnt, und vergewissern Sie sich mithilfe des Überwachungsfeatures von Azure Functions dann, dass Ereignisse fließen.

Im nächsten Schritt werden die Funktion und der Service Bus-Namespace miteinander verbunden. In diesem Beispiel wird das Azure-Portal verwendet. Informationen zur Verwendung von PowerShell oder der Azure CLI für diesen Schritt finden Sie unter [Übersicht über die Integration von Azure Service Bus in Azure Event Grid](service-bus-to-event-grid-integration-concept.md).

![9][]

### <a name="receive-messages-by-using-azure-functions"></a>Empfangen von Nachrichten per Azure Functions

Im vorherigen Abschnitt haben Sie ein einfaches Test- und Debuggingszenario verwendet und sich vergewissert, dass Ereignisse fließen. 

In diesem Abschnitt erfahren Sie, wie Sie nach dem Erhalt eines Ereignisses Nachrichten empfangen und verarbeiten.

Sie fügen eine Azure-Funktion hinzu, wie im folgenden Beispiel gezeigt, da die Service Bus-Funktionen in Azure Functions noch keine native Unterstützung der neuen Event Grid-Integration aufweisen.

1. Wählen Sie in der Visual Studio-Projektmappe, die Sie bereits in der Vorbereitung geöffnet haben, **ReceiveMessagesOnEvent.cs** aus. 

    ![10][]

1. Geben Sie Ihre Verbindungszeichenfolge im folgenden Code ein:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```

1. Laden Sie im Azure-Portal das Veröffentlichungsprofil für die Azure-Funktion herunter, die Sie im Abschnitt „Einrichten einer Testfunktion“ erstellt haben.

    ![11][]

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf **SBEventGridIntegration**, und wählen Sie anschließend die Option **Veröffentlichen**. 

1. Wählen Sie im Bereich **Veröffentlichen** für das Veröffentlichungsprofil, das Sie zuvor heruntergeladen haben, die Option **Profil importieren** und dann **Veröffentlichen**.

    ![12][]

1. Erstellen Sie nach dem Veröffentlichen der neuen Azure-Funktion ein neues Azure Event Grid-Abonnement, das auf die neue Azure-Funktion verweist.  
    Achten Sie darauf, im Feld **Endet mit** den richtigen Filter anzuwenden. Hierbei muss es sich um den Namen Ihres Service Bus-Abonnements handeln.

1. Senden Sie eine Nachricht an das Azure Service Bus-Thema, das Sie zuvor erstellt haben. Überwachen Sie anschließend das Azure Functions-Protokoll im Azure-Portal, um sicherzustellen, dass Ereignisse fließen und Nachrichten empfangen werden.

    ![12-1][]

### <a name="receive-messages-by-using-logic-apps"></a>Empfangen von Nachrichten mithilfe von Logik-Apps

Gehen Sie wie folgt vor, um für eine Logik-App eine Verbindung mit Azure Service Bus und Azure Event Grid herzustellen:

1. Erstellen Sie im Azure-Portal eine neue Logik-App, und wählen Sie **Event Grid** als Startaktion aus.

    ![13][]

    Das Fenster für den Logik-App-Designer wird geöffnet.

    ![14][]

1. Fügen Sie Ihre Informationen wie folgt hinzu:

    a. Geben Sie im Feld **Ressourcenname** Ihren eigenen Namespacenamen ein. 

    b. Geben Sie unter **Erweiterte Optionen** im Feld **Suffixfilter** den Filter für Ihr Abonnement ein.

1. Fügen Sie eine Service Bus-Empfangsaktion hinzu, um Nachrichten von einem Themenabonnement zu empfangen.  
    Die fertige Aktion ist in der folgenden Abbildung dargestellt:

    ![15][]

1. Fügen Sie wie hier gezeigt ein vollständiges Ereignis hinzu:

    ![16][]

1. Speichern Sie die Logik-App, und senden Sie eine Nachricht an Ihr Service Bus-Thema, wie im Abschnitt „Voraussetzungen“ erwähnt.  
    Verfolgen Sie die Ausführung der Logik-App. Wählen Sie zum Anzeigen von weiteren Daten für die Ausführung die Option **Übersicht**, und zeigen Sie dann die Daten unter **Ausführungsverlauf** an.

    ![17][]

    ![18][]

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/).
* Erfahren Sie mehr über [Azure Functions](https://docs.microsoft.com/azure/azure-functions/).
* Weitere Informationen zum [Logic Apps-Feature von Azure App Service](https://docs.microsoft.com/azure/logic-apps/)
* Weitere Informationen zu [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/)


[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
