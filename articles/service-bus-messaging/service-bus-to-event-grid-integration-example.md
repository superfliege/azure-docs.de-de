---
title: "Beispiele für die Integration von Azure Service Bus in Event Grid | Microsoft-Dokumentation"
description: "Beispiele für die Integration von Service Bus-Messaging in Event Grid"
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 8aff62dd32395c1ef292942e977467779f945931
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-bus-to-azure-event-grid-examples"></a>Beispiele für die Integration von Azure Service Bus in Azure Event Grid

In diesem Dokument erfahren Sie, wie Sie Azure-Funktionen und eine Logik-App einrichten, die Nachrichten auf der Grundlage des Eingangs eines Ereignisses von Event Grid empfangen. In dem Beispiel wird ein Service Bus-Thema mit zwei Abonnements vorausgesetzt, und das Event Grid-Abonnement muss so erstellt worden sein, dass es nur Ereignisse für ein einzelnes Service Bus-Abonnement sendet. Anschließend senden Sie Nachrichten an das Service Bus-Thema und vergewissern sich, dass das Ereignis für das Service Bus-Abonnement generiert wird, woraufhin die Funktion oder Logik-App die Nachrichten von diesem Service Bus-Abonnement empfängt und den Vorgang abschließt.

* Stellen Sie zunächst sicher, dass alle [Voraussetzungen](#prerequisites) erfüllt sind.
* Erstellen Sie eine [einfache Azure-Testfunktion](#test-function-setup) für das Debugging, und sehen Sie sich den ursprünglichen Fluss von Ereignissen aus Event Grid an.  **Dieser Schritt sollte unabhängig von den Schritten 3 und 4 ausgeführt werden.**
* Erstellen Sie eine [Azure-Funktion zum Empfangen und Verarbeiten von Service Bus-Nachrichten](#receive-messages-using-azure-function) auf der Grundlage von Event Grid-Ereignissen.
* Verwenden Sie [Logic Apps](#receive-messages-using-azure-logic-app).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="service-bus-namespace"></a>Service Bus- Namespace

Erstellen Sie einen Service Bus Premium-Namespace. Erstellen Sie ein Service Bus-Thema mit zwei Abonnements.

### <a name="code-to-send-message-to-the-service-bus-topic"></a>Code zum Senden einer Nachricht an das Service Bus-Thema

Sie können auf beliebige Weise eine Nachricht an Ihr Service Bus-Thema senden. Alternativ können Sie das weiter unten bereitgestellte Beispiel verwenden. Für den Beispielcode wird die Verwendung von Visual Studio 2017 vorausgesetzt.

Klonen Sie [dieses GitHub-Repository](https://github.com/Azure/azure-service-bus/).

Navigieren Sie zum folgenden Ordner:

\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration. Öffnen Sie die Datei „SBEventGridIntegration.sln“.

Navigieren Sie dann zum Projekt „MessageSender“, und öffnen Sie „Program.cs“.

![8][]

Geben Sie den Namen des Themas sowie die Verbindungszeichenfolge an, und führen Sie die Konsolenanwendung aus:

```CSharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
const string TopicName = "YOUR TOPIC NAME";
```

## <a name="test-function-setup"></a>Testen der Funktionseinrichtung

Bevor Sie sich mit dem gesamten End-to-End-Szenario beschäftigen, sollten Sie mindestens eine kleine Testfunktion erstellen, die Sie zum Debuggen sowie zur Betrachtung des Ereignisflusses verwenden können.

Erstellen Sie im Portal eine neue Azure Functions-Anwendung. Grundlegende Informationen zu Azure Functions finden Sie [hier](https://docs.microsoft.com/en-us/azure/azure-functions/).

Klicken Sie in der neu erstellten Funktion auf das kleine Plussymbol, um eine HTTP-Triggerfunktion hinzuzufügen:

![2][]

![3][]

Kopieren Sie als Nächstes den folgenden Code in diese Funktion:

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

Klicken Sie auf „Speichern und ausführen“.

## <a name="connect-function-and-namespace-via-event-grid"></a>Verbinden von Funktion und Namespace über Event Grid

Im nächsten Schritt werden die Funktion und der Service Bus-Namespace miteinander verbunden. In diesem Beispiel wird das Azure-Portal verwendet. Auf der Seite [Konzepte](service-bus-to-event-grid-integration-concept.md) erfahren Sie, wie Sie das gleiche Ergebnis mit PowerShell oder über die Azure-Befehlszeilenschnittstelle erzielen.

Navigieren Sie zum Erstellen eines neuen Azure Event Grid-Abonnements im Azure-Portal zu Ihrem Namespace, und klicken Sie auf das Blatt „Event Grid“. Klicken Sie auf „+ Ereignisabonnement“.

Der folgende Screenshot zeigt einen Namespace, der bereits über einige Event Grid-Abonnements verfügt:

![20][]

Der folgende Screenshot zeigt, wie Sie eine Azure-Funktion oder einen Webhook ohne spezielle Filterung abonnieren. Vergessen Sie nicht, den Betrefffilter für Ihr Service Bus-Abonnement als „Suffixfilter“ hinzuzufügen:

![21][]

Senden Sie eine Nachricht an Ihr Service Bus-Thema, wie in den Voraussetzungen erwähnt, und vergewissern Sie sich mithilfe des Überwachungsfeatures der Azure-Funktion, dass Ereignisse fließen.

![9][]

### <a name="receive-messages-using-azure-function"></a>Empfangen von Nachrichten per Azure-Funktion

Im vorherigen Abschnitt haben Sie sich ein einfaches Test- und Debuggingszenario angesehen und sich vergewissert, dass Ereignisse fließen. In diesem Teil der Dokumentation erfahren Sie, wie Sie Nachrichten empfangen und verarbeiten, wenn ein Ereignis empfangen wird.

Die Azure-Funktion wird auf die folgende Weise hinzugefügt, da die neue Event Grid-Integration von den Service Bus-Funktionen in Azure Functions noch nicht nativ unterstützt wird.

Wählen Sie in der Visual Studio-Projektmappe, die Sie bereits in der Vorbereitung geöffnet haben, „ReceiveMessagesOnEvent.cs“ aus. Geben Sie Ihre Verbindungszeichenfolge in den Code ein:

![10][]

```Csharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
```

Laden Sie als Nächstes im Azure-Portal das Veröffentlichungsprofil für die Azure-Funktion herunter, die Sie in [2. Testen der Funktionseinrichtung](#2-test-function-setup) erstellt haben.

![11][]

Klicken Sie in Visual Studio mit der rechten Maustaste auf „SBEventGridIntegration“, und klicken Sie anschließend auf „Veröffentlichen“. Wählen Sie das zuvor heruntergeladene Veröffentlichungsprofil aus, klicken Sie auf „Profil importieren“, und klicken Sie anschließend auf „Veröffentlichen“.

![12][]

Erstellen Sie nach dem Veröffentlichen der neuen Azure-Funktion ein neues Azure Event Grid-Abonnement, und verweisen Sie dabei auf die neue Azure-Funktion. Achten Sie darauf, den korrekten Filter vom Typ „Endet mit“ anzuwenden. Hierbei muss es sich um den Namen Ihres Service Bus-Abonnements handeln.

Senden Sie dann eine Nachricht an das zuvor erstellte Azure Service Bus-Thema, und vergewissern Sie sich im Portal anhand des Protokolls der Azure-Funktion, dass Ereignisse fließen und Nachrichten empfangen werden.

![12-1][]

### <a name="receive-messages-using-azure-logic-app"></a>Empfangen von Nachrichten per Azure-Logik-App

Die folgende Anleitung zeigt, wie Sie eine Azure-Logik-App mit Azure Service Bus und Azure Event Grid verbinden:

Erstellen Sie im Azure-Portal zunächst eine neue Logik-App, und wählen Sie Event Grid als Startaktion aus.

![13][]

Die Ansicht im Designer für Logik-Apps sollte zu Beginn wie auf dem folgenden Screenshot aussehen. Der Ressourcenname muss allerdings durch Ihren eigenen Namespacenamen ersetzt werden. Blenden Sie außerdem die erweiterten Optionen ein, und fügen Sie den Suffixfilter für Ihr Abonnement hinzu:

![14][]

Fügen Sie dann eine Service Bus-Empfangsaktion hinzu, um den Empfang aus einem Themaabonnement zu ermöglichen. Die endgültige Aktion sollte wie im folgenden Screenshot aussehen:

![15][]

Fügen Sie ein vollständiges Ereignis hinzu. Das sollte wie folgt aussehen:

![16][]

Speichern Sie die Logik-App, und senden Sie eine Nachricht an Ihr Service Bus-Thema, wie in den Voraussetzungen erwähnt. Beobachten Sie dann die Ausführung der Logik-Apps. Wenn Sie auf „Übersicht“ und anschließend auf „Ausführungsverlauf“ klicken, erhalten Sie weitere Daten zur Ausführung.

![17][]

![18][]

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Erfahren Sie mehr über [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Erfahren Sie mehr über [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/).
* Weitere Informationen zu [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/)

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