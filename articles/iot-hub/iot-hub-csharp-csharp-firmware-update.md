---
title: "Durchführen eines Firmwareupdates mit Azure IoT Hub (.NET/:NET) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Geräteverwaltung in Azure IoT Hub verwendet wird, um ein Firmwareupdate zu initiieren. Sie können das Azure IoT-Geräte-SDK für .NET verwenden, um eine simulierte Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK, um eine Service-App zu implementieren, die das Firmwareupdate auslöst."
services: iot-hub
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: v-jamebr
ms.openlocfilehash: bd0a227861d75dc66af8fb4865a17a3b6d0f70ba
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnet"></a>Initiieren eines Firmwareupdates mithilfe der Geräteverwaltung (.NET/.NET)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Einführung
Im Tutorial [Get started with device management][lnk-dm-getstarted] (Erste Schritte mit der Geräteverwaltung) wurde erläutert, wie Sie mit einem [Gerätezwilling][lnk-devtwin] und [direkten Methoden][lnk-c2dmethod] Grundtypen für den Remoteneustart eines Geräts verwenden können. In diesem Tutorial werden die gleichen IoT Hub-Grundtypen verwendet, und Sie sehen, wie Sie ein simuliertes End-to-End-Firmwareupdate durchführen.  Dieses Muster wird in der Firmwareupdateimplementierung für das [Implementierungsbeispiel mit dem Raspberry Pi-Gerät][lnk-rpi-implementation] verwendet.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen einer .NET-Konsolen-App, die die direkte **firmwareUpdate**-Methode in der simulierten Geräte-App über Ihre IoT Hub-Instanz aufruft
* Erstellen Sie eine simulierte Gerät-App, die eine direkte **FirmwareUpdate**-Methode implementiert. Diese Methode löst einen mehrstufigen Prozess aus, der auf das Herunterladen des Firmware-Images wartet, das Firmware-Image herunterlädt und schließlich das Firmware-Image anwendet. In allen Phasen des Updates nutzt das Gerät die gemeldeten Eigenschaften zum Berichten des Fortschritts.

Am Ende dieses Tutorials verfügen Sie über eine .NET-Konsolen-Geräte-App (C#) und eine .NET-Konsolen-Back-End-App (C#):

* **SimulatedDeviceFwUpdate**, die über die zuvor erstellte Geräteidentität eine Verbindung mit dem IoT Hub herstellt, die direkte **firmwareUpdate**-Methode empfängt und einen mehrstufigen Prozess zum Simulieren eines Firmwareupdates durchläuft, bei dem auf das Herunterladen des Images gewartet, das Image heruntergeladen und das Image schließlich übernommen wird.

* **TriggerFWUpdate**, die mit dem Dienst-SDK auf dem simulierten Gerät die direkte **firmwareUpdate**-Methode aufruft, die Antwort anzeigt und in regelmäßigen Abständen (alle 500 ms) die aktualisierten gemeldeten Eigenschaften anzeigt.

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Befolgen Sie die Schritte im Artikel [Erste Schritte mit der Geräteverwaltung](iot-hub-csharp-csharp-device-management-get-started.md), um Ihren IoT Hub zu erstellen und die IoT Hub-Verbindungszeichenfolge abzurufen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen eines Remotefirmwareupdates auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie (mit C#) eine .NET Konsolen-App, die auf einem Gerät ein Remotefirmwareupdate auslöst. Die App verwendet eine direkte Methode zum Auslösen des Updates und Gerätzwillingsabfragen, um in regelmäßigen Abständen den Status des aktiven Firmwareupdates abzurufen.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **TriggerFWUpdate**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createserviceapp]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **TriggerFWUpdate**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das NuGet-Paket [Azure IoT-Dienst-SDK][lnk-nuget-service-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.

    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:

    ```csharp   
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie die mehrfachen Platzhalterwerte durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben, und die ID Ihres Geräts.
   
    ```csharp   
    static RegistryManager registryManager;
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static string targetDevice = "{deviceIdForTargetDevice}";
    ```
        
6. Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Diese Methode fragt den Gerätezwilling alle 500 Millisekunden nach dem aktualisierten Status ab. Nur wenn sich der Status tatsächlich geändert hat, wird in die Konsole geschrieben. Das Abrufen wird beendet, wenn das Gerät den Status **applyComplete** oder einen Fehler meldet, um für dieses Beispiel zu verhindern, dass zusätzliche IoT Hub-Nachrichten in Ihrem Abonnement verbraucht werden.  
   
    ```csharp   
    public static async Task QueryTwinFWUpdateReported(DateTime startTime)
    {
        DateTime lastUpdated = startTime;

        while (true)
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);

            if (twin.Properties.Reported.GetLastUpdated().ToUniversalTime() > lastUpdated.ToUniversalTime())
            {
                lastUpdated = twin.Properties.Reported.GetLastUpdated().ToUniversalTime();
                Console.WriteLine("\n" + twin.Properties.Reported["iothubDM"].ToJson());

                var status = twin.Properties.Reported["iothubDM"]["firmwareUpdate"]["status"].Value;
                if ((status == "downloadFailed") || (status == "applyFailed") || (status == "applyComplete"))
                {
                    Console.WriteLine("\nStop polling.");
                    return;
                }
            }
            await Task.Delay(500);
        }
    }
    ```
        
7. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp   
    public static async Task StartFirmwareUpdate()
    {
        client = ServiceClient.CreateFromConnectionString(connString);
        CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
        method.ResponseTimeout = TimeSpan.FromSeconds(30);
        method.SetPayloadJson(
            @"{
               fwPackageUri : 'https://someurl'
            }");

        CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

        Console.WriteLine("Invoked firmware update on device.");
    }
    ```

8. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu. Dadurch wird ein Registrierung-Manager erstellt, um damit den Gerätezwilling zu lesen, eine Abrufaufgabe für einen Arbeitsthread zu starten und dann das Firmwareupdate auszulösen.
   
    ```csharp   
    registryManager = RegistryManager.CreateFromConnectionString(connString);

    Task queryTask = Task.Run(() => (QueryTwinFWUpdateReported(DateTime.Now)));

    StartFirmwareUpdate().Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```
        
9. Erstellen Sie die Projektmappe.

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen einer .NET-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet
* Simulieren eines Firmwareupdates, das über eine direkte Methode von einem Back-End-Dienst ausgelöst wird
* Ermöglichen von Gerätezwillingabfragen mit den gemeldeten Eigenschaften, um Geräte und den Zeitpunkt ihres letzten abgeschlossenen Firmwareupdates zu identifizieren

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **SimulateDeviceFWUpdate**.
   
    ![Neue klassische Visual C#-App für Windows][img-createdeviceapp]
    
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SimulateDeviceFWUpdate**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, und suchen Sie dann nach **microsoft.azure.devices.client**. Wählen Sie **Installieren** aus, um das Paket **Microsoft.Azure.Devices.Client** zu installieren, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das [NuGet-Paket mit dem Azure IoT-Geräte-SDK][lnk-nuget-client-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.
   
    ![Fenster „NuGet-Paket-Manager“ mit Client-App][img-clientnuget]
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
    ```csharp   
    using Newtonsoft.Json.Linq;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Geräteverbindungszeichenfolge, die Sie sich im Abschnitt **Erstellen einer Geräteidentität** notiert haben.
   
    ```csharp   
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Fügen Sie die folgende Methode hinzu, um den Status über den Gerätezwilling wieder an die Cloud zu melden: 

    ```csharp   
    static async Task reportFwUpdateThroughTwin(Twin twin, TwinCollection fwUpdateValue)
    {
        try
        {
            TwinCollection patch = new TwinCollection();
            TwinCollection iothubDM = new TwinCollection();

            iothubDM["firmwareUpdate"] = fwUpdateValue;
            patch["iothubDM"] = iothubDM;

            await Client.UpdateReportedPropertiesAsync(patch);
            Console.WriteLine("Twin state reported: {0}", fwUpdateValue["status"]);
        }
        catch 
        {
            Console.WriteLine("Error updating device twin");
            throw;
        }
    }
    ```

7. Fügen Sie die folgende Methode hinzu, um das Herunterladen des Firmwareimages zu simulieren:
        
    ```csharp   
    static async Task<byte[]> simulateDownloadImage(string imageUrl)
    {
        var image = "[fake image data]";

        Console.WriteLine("Downloading image from " + imageUrl);

        await Task.Delay(4000);

        return Encoding.ASCII.GetBytes(image);
            
    }
    ```

8. Fügen Sie die folgende Methode hinzu, um das Anwenden des Firmwareimages auf das Gerät zu simulieren:
        
    ```csharp   
    static async Task simulateApplyImage(byte[] imageData)
    {
        if (imageData == null)
        {
            throw new ArgumentNullException();
        }

        await Task.Delay(4000);

    }
    ```
 
9.  Fügen Sie die folgende Methode hinzu, um das Warten auf das Herunterladen des Firmwareimages zu simulieren. Aktualisieren Sie den Status zu **Warten**, und deaktivieren Sie andere Eigenschaften des Firmwareupdates auf dem Gerätezwilling. Diese Eigenschaften werden deaktiviert, um vorhandene Werte von vorherigen Firmwareupdates zu entfernen. Dies ist erforderlich, da gemeldete Eigenschaften als PATCH-Vorgang (Delta) und nicht als PUT-Vorgang (ein vollständiger Satz von Eigenschaften, der alle vorherigen Werte ersetzt) gesendet werden. Geräte werden üblicherweise über verfügbare Updates informiert, und eine vom Administrator definierte Richtlinie sorgt dafür, dass das Gerät das Update herunterlädt und anwendet. Diese Funktion ist die Stelle, an der die Logik zum Aktivieren dieser Richtlinie ausgeführt werden muss. 
        
    ```csharp   
    static async Task waitToDownload(Twin twin, string fwUpdateUri)
    {
        var now = DateTime.Now;
        TwinCollection status = new TwinCollection();
        status["fwPackageUri"] = fwUpdateUri;
        status["status"] = "waiting";
        status["error"] = null;
        status["startedWaitingTime"] = DateTime.Now;
        status["downloadCompleteTime"] = null;
        status["startedApplyingImage"] = null;
        status["lastFirmwareUpdate"] = null;

        await reportFwUpdateThroughTwin(twin, status);

        await Task.Delay(2000);
    }
    ```

10. Fügen Sie die folgende Methode hinzu, um den Download durchzuführen. Sie aktualisiert den Status über den Gerätezwilling zu **Herunterladen**, ruft die Methode zum Simulieren des Downloads auf und meldet den Status **downloadComplete** oder **downloadFailed** in Abhängigkeit von den Ergebnissen beim Herunterladen über den Gerätezwilling. 
        
    ```csharp   
    static async Task<byte[]> downloadImage(Twin twin, string fwUpdateUri)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloading";
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            byte[] imageData = await simulateDownloadImage(fwUpdateUri);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadComplete";
            statusUpdate["downloadCompleteTime"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            return imageData;
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

11. Fügen Sie die folgende Methode hinzu, um das Image anzuwenden. Sie aktualisiert den Status über den Gerätezwilling zu **Anwenden**, ruft die Methode zum Simulieren der Imageanwendung auf und aktualisiert den Status zu **applyComplete** oder **applyFailed** in Abhängigkeit von den Ergebnissen beim Anwenden über den Gerätezwilling. 
        
    ```csharp   
    static async Task applyImage(Twin twin, byte[] imageData)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applying";
            statusUpdate["startedApplyingImage"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            await simulateApplyImage(imageData);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyComplete";
            statusUpdate["lastFirmwareUpdate"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

12. Fügen Sie die folgende Methode hinzu, um den Firmwareupdate-Vorgang vom Warten auf das Herunterladen des Images bis zum Anwenden des Images auf das Gerät in eine Reihenfolge zu bringen:
        
    ```csharp   
    static async Task doUpdate(string fwUpdateUrl)
    {
        try
        {
            Twin twin = await Client.GetTwinAsync();
            await waitToDownload(twin, fwUpdateUrl);
            byte[] imageData = await downloadImage(twin, fwUpdateUrl);
            await applyImage(twin, imageData);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error during update: {0}", ex.Message);
        }
    }
    ```

13. Fügen Sie die folgende Methode hinzu, um die direkte **updateFirmware**-Methode aus der Cloud zu behandeln. Sie extrahiert die URL zum Firmwareupdate aus dem Nachrichtenaufkommen und übergibt sie an die Aufgabe **doUpdate**, die einen weiteren Thread des Threadpools startet. Sie gibt die Antwort der Methode dann sofort an die Cloud zurück.
        
    ```csharp   
    static Task<MethodResponse> onFirmwareUpdate(MethodRequest methodRequest, object userContext)
    {
        string fwUpdateUrl = (string)JObject.Parse(methodRequest.DataAsJson)["fwPackageUri"];
        Console.WriteLine("\nMethod: {0} triggered by service, URI is: {1}", methodRequest.Name, fwUpdateUrl);

        Task updateTask = Task.Run(() => (doUpdate(fwUpdateUrl)));

        string result = "'FirmwareUpdate started.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```
> [!NOTE]
> Diese Methode löst die Ausführung des simulierten Updates als **Aufgabe** aus und antwortet dann sofort auf den Methodenaufruf, wobei der Dienst informiert wird, dass das Firmwareupdate gestartet wurde. Aktualisierungsstatus und -abschluss werden über die gemeldeten Eigenschaften des Gerätezwillings an den Dienst gesendet. Wir antworten beim Starten des Updates auf den Methodenaufruf, anstatt auf seinen Abschluss zu warten. Dies hat folgende Gründe:
> * Ein realer Updatevorgang dauert wahrscheinlich länger als der Timeout für den Methodenaufruf.
> * Ein realer Updatevorgang erfordert wahrscheinlich einen Neustart, wodurch diese App neu gestartet wird und dann das **MetodRequest**-Objekt nicht mehr verfügbar ist. (Das Aktualisieren gemeldeter Eigenschaften ist jedoch auch nach einem Neustart möglich.) 

14. Fügen Sie abschließend der **Main**-Methode folgenden Code hinzu, um die Verbindung mit dem IoT Hub zu öffnen und den Listener der Methode zu initialisieren:
   
    ```csharp   
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
        
        // setup callback for "firmware update" method
        Client.SetMethodHandlerAsync("firmwareUpdate", onFirmwareUpdate, null).Wait();
        Console.WriteLine("Waiting for firmware update direct method call\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        // as a good practice, remove the firmware update handler
        Client.SetMethodHandlerAsync("firmwareUpdate", null, null).Wait();
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
15. Erstellen Sie die Projektmappe.       

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.


## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.
1. Führen Sie die .NET-Geräte-App **SimulatedDeviceFWUpdate** aus.  Klicken Sie mit der rechten Maustaste auf das **SimulatedDeviceFWUpdate**-Projekt , wählen Sie **Debuggen** aus, und wählen Sie anschließend **Neue Instanz starten** aus. Er sollte auf die Methodenaufrufe aus Ihrem IoT Hub lauschen: 

2. Nachdem das Gerät verbunden ist und auf Methodenaufrufe wartet, führen Sie die .NET-App **TriggerFWUpdate** aus, um die Firmwareupdatemethode in der simulierten Geräte-App aufzurufen. Klicken Sie mit der rechten Maustaste auf das **TriggerFWUpdate**-Projekt, wählen Sie **Debuggen** aus, und wählen Sie anschließend **Neue Instanz starten** aus. Daraufhin sollte die Updatesequenz in die **SimulatedDeviceFWUpdate**-Konsole und auch die über die gemeldeten Eigenschaften des Geräts gemeldete Sequenz in die **TriggerFWUpdate**-Konsole geschrieben werden. Beachten Sie, dass der Vorgang einige Sekunden dauert. 
   
    ![Ausführen der Dienst- und Geräte-App][img-combinedrun]


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät ausgelöst und mithilfe der gemeldeten Eigenschaften den Fortschritt der Firmwareaktualisierung überprüft.

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-firmware-update/servicesdknuget.png
[img-clientnuget]: media/iot-hub-csharp-csharp-firmware-update/clientsdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-firmware-update/createserviceapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-firmware-update/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-firmware-update/combinedrun.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-csharp-csharp-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-csharp-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/