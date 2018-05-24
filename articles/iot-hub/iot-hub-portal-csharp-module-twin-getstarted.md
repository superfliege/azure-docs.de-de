---
title: Erste Schritte mit der Modulidentität und dem Modulzwilling von Azure IoT Hub (Portal und .NET) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit dem Portal und .NET eine Modulidentität erstellen und den Modulzwilling aktualisieren.
services: iot-hub
documentationcenter: .net
author: chrissie926
manager: timlt
editor: ''
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30a53620a640b0122286a6ac69c0f98cd0bbde40
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361687"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Erste Schritte mit der Modulidentität und dem Modulzwilling von IoT Hub unter Verwendung des Portals und eines .NET-Geräts

> [!NOTE]
> [Modulidentitäten und Modulzwillinge](iot-hub-devguide-module-twins.md) ähneln den Geräteidentitäten und Gerätezwillingen von Azure IoT Hub, ermöglichen jedoch eine feinere Granularität. Azure IoT Hub-Geräteidentitäten und -Gerätezwillinge ermöglichen der Back-End-Anwendung die Konfiguration eines Geräts und geben Aufschluss über den Gerätezustand. Modulidentitäten und Modulzwillinge bieten diese Funktionalität hingegen für einzelne Komponenten eines Geräts. Auf Geräten mit mehreren Komponenten (etwa auf betriebssystembasierten Geräten oder Firmwaregeräten) ermöglichen sie isolierte Konfigurationen und Zustände für die einzelnen Komponenten.

In diesem Tutorial lernen Sie Folgendes: 
1. Erstellen einer Modulidentität im Portal 
2. Aktualisieren des Modulzwillings auf Ihrem Gerät mit den .NET-Geräte-SDKs

> [!NOTE]
> Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können, finden Sie unter [Azure IoT SDKs][lnk-hub-sdks].

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Ein aktives Azure-Konto. (Wenn Sie noch kein Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

## <a name="create-a-device-identity-in-the-portal"></a>Erstellen einer Geräteidentität im Portal

Sie besitzen eine IoT Hub-Instanz. Öffnen Sie das [Portal](https://portal.azure.com), und navigieren Sie zu Ihrer IoT Hub-Instanz. Klicken Sie auf „IoT-Geräte“ und dann auf „Hinzufügen“, um eine Geräteidentität zu erstellen. Nennen Sie sie **MyFirstDevice**. 

![Erstellen einer Geräteidentität][8]

Nach dem Speichern sehen Sie in Ihrer Geräteidentitätsliste, dass die Identität „MyFirstDevice“ erfolgreich erstellt wurde.

![Geräte-ID erstellt][11]

Klicken Sie nun auf die Zeile. Gerätedetails werden angezeigt.

![Gerätedetails][10]

## <a name="create-a-module-identity-in-the-portal"></a>Erstellen einer Modulidentität im Portal

Innerhalb einer Geräteidentität können Sie bis zu 20 Modulidentitäten erstellen. Klicken Sie oben auf die Schaltfläche **Add Module Identity** (Modulidentität hinzufügen), um die erste Modulidentität namens **myFirstModule** zu erstellen. 

![Gerätedetails][9]

Speichern Sie die soeben erstellte Modulidentität, und klicken Sie dann darauf. Die Details der Modulidentität werden angezeigt. Speichern Sie die Verbindungszeichenfolge für den Primärschlüssel. Sie wird im nächsten Abschnitt zum Einrichten Ihres Moduls auf dem Gerät verwendet.

![Gerätedetails][12]

<a id="D2C_csharp"></a>
## <a name="update-the-module-twin-using-net-device-sdk"></a>Aktualisieren des Modulzwillings mithilfe des .NET-Geräte-SDKs

Sie haben die Modulidentität in Ihrer IoT Hub-Instanz erstellt. Testen wir nun die Kommunikation zwischen Cloud und simuliertem Gerät. Sobald eine Modulidentität erstellt wurde, wird implizit ein Gerätezwilling in IoT Hub erstellt. In diesem Abschnitt erstellen Sie eine .NET Konsolen-App auf Ihrem simulierte Gerät, die die vom Modulzwilling gemeldeten Eigenschaften aktualisiert.

1. **Erstellen eines Visual Studio-Projekts:** Fügen Sie in Visual Studio in der vorhandenen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung (.NET Framework)** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET Framework, Version 4.6.1 oder höher, verwendet wird. Nennen Sie das Projekt **UpdateModuleTwinReportedProperties**.

    ![Erstellen eines Visual Studio-Projekts][13]

2. **Installieren des Azure IoT Hub-.NET-Geräte-SDKs V1.16.0-preview-005:** Modulidentität und Modulzwilling sind als öffentliche Vorschauversion verfügbar. Sie sind nur in der Vorabversion der IoT Hub-Geräte-SDKs verfügbar. Navigieren Sie in Visual Studio zu „Tools“ > „NuGet-Paket-Manager“ > „NuGet-Pakete für Projektmappe verwalten“. Suchen Sie nach „Microsoft.Azure.Devices.Client“. Vergewissern Sie sich, dass das Kontrollkästchen „Vorabversion einbeziehen“ aktiviert ist. Wählen Sie Version „V1.16.0-preview-005“ aus, und installieren Sie sie. Sie haben nun Zugriff auf alle Modulfeatures. 

    ![Installieren des Azure IoT Hub-.NET-Dienst-SDKs V1.16.0-preview-005][14]

3. **Abrufen der Modulverbindungszeichenfolge:** Melden Sie sich beim [Azure-Portal][lnk-portal] an. Navigieren Sie zu Ihrer IoT Hub-Instanz, und klicken Sie auf „IoT-Geräte“. Suchen Sie nach „myFirstDevice“, und öffnen Sie den Eintrag. Sie sehen, dass „myFirstModule“ erfolgreich erstellt wurde. Kopieren Sie die Modulverbindungszeichenfolge. Sie wird im nächsten Schritt benötigt.

    ![Moduldetails im Azure-Portal][15]

4. **Erstellen der Konsolen-App „UpdateModuleTwinReportedProperties“:** Fügen Sie die folgenden `using`-Anweisungen oben in der Datei **Program.cs** hinzu:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

    Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge des Moduls.

    ```csharp
    private const string ModuleConnectionString = "<Your module connection string>“;
    private static ModuleClient Client = null;
    ```

    Fügen Sie die folgende Methode **OnDesiredPropertyChanged** der Klasse **Program** hinzu:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

    Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin));

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Client.CloseAsync().Wait();
    }
    ```

    In diesem Codebeispiel wird gezeigt, wie Sie den Modulzwilling abrufen und die gemeldeten Eigenschaften mit dem AMQP-Protokoll aktualisieren. In der öffentlichen Vorschau wird nur AMQP für Modulzwillingsvorgänge unterstützt.
    ```

## Run the apps

You are now ready to run the apps. In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**. Select **Multiple startup projects**, and then select **Start** as the action for the console app. And then press F5 to start both apps running. 

## Next steps

To continue getting started with IoT Hub and to explore other IoT scenarios, see:

* [Get started with IoT Hub module identity and module twin using .NET backup and .NET device][lnk-csharp-csharp-getstarted]
* [Getting started with IoT Edge][lnk-iot-edge]


<!-- Images. -->
[8]:./media\iot-hub-portal-csharp-module-twin-getstarted/create-device-id.JPG
[9]:./media\iot-hub-portal-csharp-module-twin-getstarted/create-module-id.JPG
[10]:./media\iot-hub-portal-csharp-module-twin-getstarted/device-details.JPG
[11]:./media\iot-hub-portal-csharp-module-twin-getstarted/device-id-created.JPG
[12]:./media\iot-hub-portal-csharp-module-twin-getstarted/module-details.JPG
[13]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.JPG
[14]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png
[15]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-csharp-csharp-getstarted]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
