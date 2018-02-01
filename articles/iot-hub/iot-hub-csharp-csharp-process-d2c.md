---
title: Weiterleiten von Nachrichten mit Azure IoT Hub (.NET) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Azure IoT Hub-D2C-Nachrichten mithilfe von Routingregeln und benutzerdefinierten Endpunkten verarbeitet werden, um Nachrichten an andere Back-End-Dienste zu senden.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 4794f81787fd575c34fa8a2ef66431d85dcf4a5d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="routing-messages-with-iot-hub-net"></a>Weiterleiten von Nachrichten mit Azure IoT Hub (.NET)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Dieses Tutorial baut auf dem Tutorial [Erste Schritte mit IoT Hub] auf. Das Tutorial bietet Folgendes:

* Zeigt, wie Sie Routingregeln zum Senden von Nachrichten vom Gerät an die Cloud auf einfache Weise und auf Konfigurationsbasis einsetzen.
* Veranschaulicht das Isolieren von interaktiven Nachrichten, die vom Lösungs-Back-End sofortiges Eingreifen zur weiteren Verarbeitung erfordern. Beispielsweise könnte ein Gerät eine Alarmnachricht senden, die das Einfügen eines Tickets in ein CRM-System auslöst. Im Gegensatz dazu werden Datenpunktnachrichten wie Temperaturtelemetrie in eine Analyse-Engine eingegeben.

Am Ende dieses Tutorials führen Sie drei .NET-Konsolen-Apps aus:

* **SimulatedDevice**, eine angepasste Version der im Tutorial [Erste Schritte mit IoT Hub] erstellten App, die jede Sekunde Datenpunktnachrichten vom Gerät an die Cloud und alle 10 Sekunden interaktive Nachrichten vom Gerät an die Cloud sendet.
* **ReadDeviceToCloudMessages** zeigt die nicht schwerwiegende Telemetrie an, die Ihre Geräte-App sendet.
* **ReadCriticalQueue** holt die von Ihrer Geräte-App gesendeten kritischen Nachrichten aus einer Service Bus-Warteschlange. Diese Warteschlange wird IoT Hub angefügt.

> [!NOTE]
> IoT Hub verfügt über SDK-Unterstützung für zahlreiche Geräteplattformen und Sprachen, z.B. C, Java und JavaScript. Informationen zum Ersetzen des simulierten Geräts in diesem Tutorial durch ein physisches Gerät finden Sie im [Azure IoT Developer Center].

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Ein aktives Azure-Konto. <br/>Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

Außerdem sollten Sie sich mit [Azure Storage] und [Azure Service Bus] vertraut machen.

## <a name="send-interactive-messages"></a>Senden von interaktiven Nachrichten

Ändern Sie die Geräte-App, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, um gelegentlich interaktive Nachrichten zu senden.

Ersetzen Sie in Visual Studio im **SimulatedDevice**-Projekt die `SendDeviceToCloudMessagesAsync`-Methode durch den folgenden Code:

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            if (rand.NextDouble() > 0.5)
            {
                messageString = "This is a critical message";
                levelValue = "critical";
            }
            else 
            {
                messageString = "This is a storage message";
                levelValue = "storage";
            }
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

Mit dieser Methode werden vom Gerät gesendeten Nachrichten nach dem Zufallsprinzip die Eigenschaften `"level": "critical"` und `"level": "storage"` hinzugefügt. Dadurch wird eine Nachricht simuliert, die eine sofortige Aktion durch das Anwendungs-Back-End erfordert oder dauerhaft gespeichert werden muss. Die Anwendung unterstützt Weiterleiten von Nachrichten auf Basis des Nachrichtentexts.

> [!NOTE]
> Sie können Nachrichteneigenschaften zum Weiterleiten von Nachrichten für verschiedene Szenarien zusätzlich zu dem hier gezeigten Beispiel des langsamsten Pfads verwenden – einschließlich der Cold-Path-Verarbeitung.

> [!NOTE]
> Es wird dringend empfohlen, eine Wiederholungsrichtlinie zu implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Beheben vorübergehender Fehler] beschrieben.

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a>Weiterleiten von Nachrichten an eine Warteschlange in IoT Hub

In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen einer Service Bus-Warteschlange
* Herstellen einer Verbindung mit Ihrem IoT-Hub
* Konfigurieren Ihres IoT-Hubs zum Senden von Nachrichten an die Warteschlange, sofern eine Eigenschaft in der Nachricht vorhanden ist

Weitere Informationen zum Verarbeiten von Nachrichten von der Service Bus-Warteschlange finden Sie in [Erste Schritte mit Event Hubs][Service Bus queue].

1. Erstellen Sie eine Service Bus-Warteschlange wie in [erste Schritte mit Warteschlangen][Service Bus queue] beschrieben. Die Warteschlange muss sich in dem gleichen Abonnement und der gleichen Region wie der IoT Hub befinden. Notieren Sie den Namespace und den Warteschlangennamen.

    > [!NOTE]
    > Für Service Bus-Warteschlangen und -Themen, die als IoT Hub-Endpunkte verwendet werden, dürfen **Sitzungen** oder **Duplikaterkennung** nicht aktiviert werden. Wenn eine dieser Optionen aktiviert ist, wird der Endpunkt im Azure-Portal als **Nicht erreichbar** angezeigt.

2. Öffnen Sie Ihren IoT-Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.
    
    ![Endpunkte in IoT Hub][30]

3. Klicken Sie oben auf dem Blatt **Endpunkte** auf **Hinzufügen**, um die Warteschlange Ihrem IoT-Hub hinzuzufügen. Benennen Sie den Endpunkt **CriticalQueue**, und wählen Sie mithilfe der Dropdownlisten **Service Bus-Warteschlange**, den Service Bus-Namespace, in dem sich die Warteschlange befindet, und den Namen der Warteschlange. Klicken Sie anschließend am unteren Rand auf **Speichern**.
    
    ![Hinzufügen eines Endpunkts][31]
    
4. Klicken Sie jetzt in Ihrem IoT-Hub auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Routingregel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. Wählen Sie **DeviceTelemetry** als Quelle der Daten. Geben Sie `level="critical"` als Bedingung ein, und wählen Sie die Warteschlange, die Sie gerade als benutzerdefinierten Endpunkt hinzugefügt haben, als Routingregelendpunkt. Klicken Sie anschließend am unteren Rand auf **Speichern**.
    
    ![Hinzufügen einer Route][32]
    
    Stellen Sie sicher, dass die Fallbackroute auf **EIN** festgelegt ist. Dieser Wert ist die Standardkonfiguration für einen IoT-Hub.
    
    ![Fallbackroute][33]

## <a name="read-from-the-queue-endpoint"></a>Lesen aus dem Warteschlangen-Endpunkt

In diesem Abschnitt lesen Sie Nachrichten aus dem Warteschlangen-Endpunkt.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolen-App (.NET Framework)** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **ReadCriticalQueue**.

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadCriticalQueue**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Mit diesem Vorgang wird das Fenster **NuGet-Paket-Manager** angezeigt.

3. Suchen Sie nach **WindowsAzure.ServiceBus**, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Mit diesem Vorgang wird Azure Service Bus mit allen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen.

4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden **using**-Anweisungen hinzu:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu. Ersetzen Sie die Verbindungszeichenfolge durch **Lauschen**-Berechtigungen für die Warteschlange:
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Ausführen der Anwendungen
Sie können jetzt die Anwendung ausführen.

1. Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen**aus. Wählen Sie **Mehrere Startprojekte** aus, und wählen Sie dann für die Projekte **ReadDeviceToCloudMessages**, **SimulatedDevice** und **ReadCriticalQueue** als Aktion die Option **Starten** aus.
2. Drücken Sie **F5**, um die drei Konsolen-Apps zu starten. Die **ReadDeviceToCloudMessages**-App verfügt nur über unkritische, von der **SimulatedDevice**-Anwendung gesendete Nachrichten, und die **ReadCriticalQueue**-App verfügt nur über kritische Nachrichten.
   
   ![Drei Konsolen-Apps][50]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Optional:) Hinzufügen eines Speichercontainers zu Ihrer IoT Hub-Instanz und Weiterleiten von Nachrichten an diese

In diesem Abschnitt erstellen Sie ein Speicherkonto, verbinden es mit Ihrer IoT Hub-Instanz und konfigurieren Ihre IoT Hub-Instanz für das Senden von Nachrichten an das Konto, wenn eine Eigenschaft in der Nachricht vorhanden ist. Weitere Informationen zum Verwalten von Speicher finden Sie unter [Erste Schritte mit Azure Storage][Azure Storage].

 > [!NOTE]
   > Sofern Sie sich nicht auf einen einzelnen **Endpunkt** beschränken müssen, können Sie **StorageContainer** zusätzlich zu **CriticalQueue** einrichten und beides parallel betreiben.

1. Erstellen Sie ein Speicherkonto. Eine entsprechende Anleitung finden Sie in der [Dokumentation für Azure Storage][lnk-storage]. Notieren Sie sich den Kontonamen.

2. Öffnen Sie Ihren IoT-Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.

3. Wählen Sie auf dem Blatt **Endpunkte** den Endpunkt **CriticalQueue** aus, und klicken Sie auf **Löschen**. Klicken Sie auf **Ja** und dann auf **Hinzufügen**. Nennen Sie den Endpunkt **StorageContainer**, wählen Sie über die Dropdownfelder **Azure-Speichercontainer** aus, und erstellen Sie ein **Speicherkonto** und einen **Speichercontainer**.  Notieren Sie sich die Namen.  Klicken Sie abschließend am unteren Rand auf **OK**. 

 > [!NOTE]
   > Wenn Sie sich nicht auf einen einzelnen **Endpunkt** beschränken müssen, ist es nicht notwendig, **CriticalQueue** zu löschen.

4. Klicken Sie in Ihrer IoT Hub-Instanz auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Routingregel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. Wählen Sie **Gerätemeldungen** als Datenquelle aus. Geben Sie `level="storage"` als Bedingung ein, und wählen Sie **StorageContainer** als benutzerdefinierten Endpunkt des Routingregelendpunkts aus. Klicken Sie am unteren Rand auf **Speichern**.  

    Stellen Sie sicher, dass die Fallbackroute auf **EIN** festgelegt ist. Diese Einstellung ist die Standardkonfiguration eines IoT-Hubs.

1. Vergewissern Sie sich, dass Ihre vorherigen Anwendungen weiterhin ausgeführt werden. 

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto, und klicken Sie unter **Blob-Dienst** auf **Blobs durchsuchen**.  Wählen Sie Ihren Container aus, navigieren Sie zu der JSON-Datei, klicken Sie auf die Datei, und klicken Sie anschließend auf **Herunterladen**, um die Daten anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, D2C-Nachrichten zuverlässig mit der Nachrichtenroutingfunktion von IoT Hub zu versenden.

Im Tutorial [Gewusst wie: Senden von C2D-Nachrichten mithilfe von IoT Hub][lnk-c2d] erfahren Sie, wie Sie Nachrichten aus Ihrem Lösungs-Back-End an Ihre Geräte senden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite][lnk-suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [Entwicklungsleitfaden für IoT Hub].

Weitere Informationen zum Nachrichtenrouting in IoT Hub finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[Entwicklungsleitfaden für IoT Hub]: iot-hub-devguide.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[Beheben vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
