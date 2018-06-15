---
title: Continuous Integration und Continuous Deployment für Azure IoT Edge | Microsoft-Dokumentation
description: Übersicht über Continuous Integration und Continuous Deployment für Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 04/30/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a8b58eae9aa08d8f6539370fa6e78a7a4813c18f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631019"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge---preview"></a>Continuous Integration und Continuous Deployment für Azure IoT Edge – Vorschau
In diesem Tutorial wird gezeigt, wie Sie die Features für Continuous Integration und Continuous Deployment von Visual Studio Team Services (VSTS) und Microsoft Team Foundation Server (TFS) zum schnellen und effizienten Erstellen, Testen und Bereitstellen von Anwendungen in Azure IoT Edge verwenden können. 

In diesem Lernprogramm lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen und Einchecken einer IoT Edge-Beispielprojektmappe, die Komponententests enthält
> * Installieren der Azure IoT Edge-Erweiterung für Ihre VSTS
> * Konfigurieren von Continuous Integration (CI) zum Erstellen der Projektmappe und Ausführen der Komponententests
> * Konfigurieren von Continuous Deployment (CD) zum Bereitstellen der Projektmappe und Anzeigen von Antworten

Das Durcharbeiten dieses Tutorials dauert 30 Minuten.

![CI und CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Erstellen einer Azure IoT Edge-Beispielprojektmappe mit Visual Studio Code

In diesem Abschnitt erstellen Sie eine IoT Edge-Beispielprojektmappe mit Komponententests, die als Teil des Buildprozesses ausgeführt werden können. Führen Sie die Schritte unter [Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) aus, bevor Sie die Anleitung in diesem Abschnitt befolgen.

1. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Wählen Sie dann Ihren Arbeitsbereichsordner aus, geben Sie den Namen der Projektmappe ein (der Standardname ist **EdgeSolution**), und erstellen Sie ein C#-Modul (**FilterModule**) als erstes Benutzermodul in dieser Projektmappe. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (`localhost:5000/filtermodule`). Sie müssen es in Azure Container Registry (`<your container registry address>/filtermodule`) oder Docker Hub für die weitere Continuous Integration ändern.

    ![Einrichten von ACR](./media/how-to-ci-cd/acr.png)

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Sie können optional den Befehl **Edge: Add IoT Edge module** eingeben und ausführen, um weitere Module hinzuzufügen. Im Stammordner befinden sich ein Ordner `modules`, ein Ordner `.vscode` und eine Bereitstellungsmanifest-Vorlagendatei. Jeglicher Code von Benutzermodulen wird in Unterordnern des Ordners `modules` gespeichert. `deployment.template.json` ist die Vorlage für das Bereitstellungsmanifest. Einige der Parameter in dieser Datei werden durch Analyse von `module.json` gewonnen, die in jedem Modulordner vorhanden ist.

3. Nun ist Ihre IoT Edge-Beispielprojektmappe bereit. Das C#-Standardmodul fungiert als Nachrichten-Pipe-Modul. In der `deployment.template.json` können Sie sehen, dass diese Projektmappe zwei Module enthält. Die Nachricht wird vom `tempSensor`-Modul generiert, direkt über `FilterModule` weitergeleitet und dann an Ihren IoT-Hub gesendet. Ersetzen Sie die gesamte Datei **Program.cs** durch den folgenden Inhalt. Weitere Informationen zu diesem Codeausschnitt finden Sie unter [Erstellen eines IoT Edge-Modulprojekts](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

    ```csharp
    namespace FilterModule
    {
        using System;
        using System.IO;
        using System.Runtime.InteropServices;
        using System.Runtime.Loader;
        using System.Security.Cryptography.X509Certificates;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using System.Collections.Generic;     // for KeyValuePair<>
        using Microsoft.Azure.Devices.Shared; // for TwinCollection
        using Newtonsoft.Json;                // for JsonConvert

        public class MessageBody
        {
            public Machine machine { get; set; }
            public Ambient ambient { get; set; }
            public string timeCreated { get; set; }
        }
        public class Machine
        {
            public double temperature { get; set; }
            public double pressure { get; set; }
        }
        public class Ambient
        {
            public double temperature { get; set; }
            public int humidity { get; set; }
        }

        public class Program
        {
            static int counter;
            static int temperatureThreshold { get; set; } = 25;

            static void Main(string[] args)
            {
                // The Edge runtime gives us the connection string we need -- it is injected as an environment variable
                string connectionString = Environment.GetEnvironmentVariable("EdgeHubConnectionString");

                // Cert verification is not yet fully functional when using Windows OS for the container
                bool bypassCertVerification = RuntimeInformation.IsOSPlatform(OSPlatform.Windows);
                if (!bypassCertVerification) InstallCert();
                Init(connectionString, bypassCertVerification).Wait();

                // Wait until the app unloads or is cancelled
                var cts = new CancellationTokenSource();
                AssemblyLoadContext.Default.Unloading += (ctx) => cts.Cancel();
                Console.CancelKeyPress += (sender, cpe) => cts.Cancel();
                WhenCancelled(cts.Token).Wait();
            }

            /// <summary>
            /// Handles cleanup operations when app is cancelled or unloads
            /// </summary>
            public static Task WhenCancelled(CancellationToken cancellationToken)
            {
                var tcs = new TaskCompletionSource<bool>();
                cancellationToken.Register(s => ((TaskCompletionSource<bool>)s).SetResult(true), tcs);
                return tcs.Task;
            }

            /// <summary>
            /// Add certificate in local cert store for use by client for secure connection to IoT Edge runtime
            /// </summary>
            static void InstallCert()
            {
                string certPath = Environment.GetEnvironmentVariable("EdgeModuleCACertificateFile");
                if (string.IsNullOrWhiteSpace(certPath))
                {
                    // We cannot proceed further without a proper cert file
                    Console.WriteLine($"Missing path to certificate collection file: {certPath}");
                    throw new InvalidOperationException("Missing path to certificate file.");
                }
                else if (!File.Exists(certPath))
                {
                    // We cannot proceed further without a proper cert file
                    Console.WriteLine($"Missing path to certificate collection file: {certPath}");
                    throw new InvalidOperationException("Missing certificate file.");
                }
                X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
                store.Open(OpenFlags.ReadWrite);
                store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
                Console.WriteLine("Added Cert: " + certPath);
                store.Close();
            }
            /// <summary>
            /// Initializes the DeviceClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init(string connectionString, bool bypassCertVerification = false)
            {
                Console.WriteLine("Connection String {0}", connectionString);

                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                // During dev you might want to bypass the cert verification. It is highly recommended to verify certs systematically in production
                if (bypassCertVerification)
                {
                    mqttSetting.RemoteCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) => true;
                }
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                DeviceClient ioTHubModuleClient = DeviceClient.CreateFromConnectionString(connectionString, settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

                // Read TemperatureThreshold from Module Twin Desired Properties
                var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
                var moduleTwinCollection = moduleTwin.Properties.Desired;
                try {
                    temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
                } catch(ArgumentOutOfRangeException) {
                    Console.WriteLine("Proerty TemperatureThreshold not exist");
                }

                // Attach callback for Twin desired properties updates
                await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
            }

            static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
            {
                try
                {
                    Console.WriteLine("Desired property change:");
                    Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                    if (desiredProperties["TemperatureThreshold"] != null)
                        temperatureThreshold = desiredProperties["TemperatureThreshold"];

                }
                catch (AggregateException ex)
                {
                    foreach (Exception exception in ex.InnerExceptions)
                    {
                        Console.WriteLine();
                        Console.WriteLine("Error when receiving desired property: {0}", exception);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
                }
                return Task.CompletedTask;
            }

            public static Message filter(Message message)
            {
                var counterValue = Interlocked.Increment(ref counter);

                var messageBytes = message.GetBytes();
                var messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message {counterValue}: [{messageString}]");

                // Get message body
                var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                {
                    Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                        $"exceeds threshold {temperatureThreshold}");
                    var filteredMessage = new Message(messageBytes);
                    foreach (KeyValuePair<string, string> prop in message.Properties)
                    {
                        filteredMessage.Properties.Add(prop.Key, prop.Value);
                    }

                    filteredMessage.Properties.Add("MessageType", "Alert");
                    return filteredMessage;
                }
                return null;
            }

            static async Task<MessageResponse> FilterMessages(Message message, object userContext)
            {
                try
                {
                    DeviceClient deviceClient = (DeviceClient)userContext;

                    var filteredMessage = filter(message);
                    if (filteredMessage != null)
                    {
                        await deviceClient.SendEventAsync("output1", filteredMessage);
                    }

                    // Indicate that the message treatment is completed
                    return MessageResponse.Completed;
                }
                catch (AggregateException ex)
                {
                    foreach (Exception exception in ex.InnerExceptions)
                    {
                        Console.WriteLine();
                        Console.WriteLine("Error in sample: {0}", exception);
                    }
                    // Indicate that the message treatment is not completed
                    var deviceClient = (DeviceClient)userContext;
                    return MessageResponse.Abandoned;
                }
                catch (Exception ex)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", ex.Message);
                    // Indicate that the message treatment is not completed
                    DeviceClient deviceClient = (DeviceClient)userContext;
                    return MessageResponse.Abandoned;
                }
            }
        }
    }
    ```

4. Erstellen Sie ein .Net Core-Komponententestprojekt. Erstellen Sie im Datei-Explorer von Visual Studio Code einen neuen Ordner mit dem Namen **tests\FilterModuleTest** in Ihrem Arbeitsbereich. Führen Sie dann im integrierten Terminal von Visual Studio Code (**STRG+`**) die folgenden Befehle aus, um ein xUnit-Testprojekt zu erstellen und einen Verweis auf das **FilterModule**-Projekt hinzuzufügen.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Ordnerstruktur](./media/how-to-ci-cd/add-test-project.png)

5. Aktualisieren Sie im Ordner **FilterModuleTest** den Dateinamen **UnitTest1.cs** in **FilterModuleTest.cs**. Wählen Sie **FilterModuleTest.cs** aus, und öffnen Sie die Datei. Ersetzen Sie den gesamten Code durch den folgenden Codeausschnitt, der die Komponententests für das FilterModule-Projekt enthält.

    ```csharp
    using Xunit;
    using FilterModule;
    using Newtonsoft.Json;
    using System;
    using System.IO;
    using System.Runtime.InteropServices;
    using System.Runtime.Loader;
    using System.Security.Cryptography.X509Certificates;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;

    namespace FilterModuleTest
    {
        public class FilterModuleTest
        {
            [Fact]
            public void filterLessThanThresholdTest()
            {
                var source = createMessage(25 - 1);
                var result = Program.filter(source);
                Assert.True(result == null);
            }

            [Fact]
            public void filterMoreThanThresholdAlertPropertyTest()
            {
                var source = createMessage(25 + 1);
                var result = Program.filter(source);
                Assert.True(result.Properties["MessageType"] == "Alert");
            }

            [Fact]
            public void filterMoreThanThresholdCopyPropertyTest()
            {
                var source = createMessage(25 + 1);
                source.Properties.Add("customTestKey", "customTestValue");
                var result = Program.filter(source);
                Assert.True(result.Properties["customTestKey"] == "customTestValue");
            }

            private Message createMessage(int temperature)
            {
                var messageBody = createMessageBody(temperature);
                var messageString = JsonConvert.SerializeObject(messageBody);
                var messageBytes = Encoding.UTF8.GetBytes(messageString);
                return new Message(messageBytes);
            }

            private MessageBody createMessageBody(int temperature)
            {
                var messageBody = new MessageBody
                {
                    machine = new Machine
                    {
                        temperature = temperature,
                        pressure = 0
                    },
                    ambient = new Ambient
                    {
                        temperature = 0,
                        humidity = 0
                    },
                    timeCreated = string.Format("{0:O}", DateTime.Now)
                };

                return messageBody;
            }
        }
    }
    ```

6. Im integrierten Terminal können Sie die folgenden Befehle zum lokalen Ausführen von Komponententests eingeben. 
    ```cmd
    dotnet test
    ```

    ![Komponententest](./media/how-to-ci-cd/unit-test.png)

7. Speichern Sie diese Projekte, und checken Sie sie dann in Ihr VSTS- oder TFS-Repository ein.
    

> [!NOTE]
> Weitere Informationen zur Verwendung von VSTS -Coderepositorys finden Sie unter [Freigeben von Code mit Visual Studio und VSTS Git](https://docs.microsoft.com/vsts/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Konfigurieren von Continuous Integration
In diesem Abschnitt erstellen Sie eine Builddefinition, die so konfiguriert ist, dass sie beim Einchecken von Änderungen der IoT Edge-Beispielprojektmappe automatisch ausgeführt wird und auch die darin enthaltenen Komponententests automatisch ausgeführt werden.

1. Melden Sie sich bei Ihrem VSTS-Konto an (**https://**_Ihr_Konto_**.visualstudio.com**), und öffnen Sie das Projekt, in das Sie die Beispiel-App eingecheckt haben.

    ![Einchecken von Code](./media/how-to-ci-cd/init-project.png)

1. Rufen Sie [Azure IoT Edge für VSTS](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) im VSTS Marketplace auf. Klicken Sie auf die Option für **Kostenlos erhalten**, und führen Sie die Schritte im Assistenten aus, um diese Erweiterung in Ihrem VSTS-Konto zu installieren oder auf Ihren TFS herunterzuladen.

    ![Installieren der Erweiterung](./media/how-to-ci-cd/install-extension.png)

1. Öffnen Sie in VSTS den Hub **Build &amp; Release**, und wählen Sie auf der Registerkarte **Builds** die Option **+ Neue Definition** aus. Wenn Sie bereits über Builddefinitionen verfügen, wählen Sie die Schaltfläche **+ Neu** aus. 

    ![Neuer Build](./media/how-to-ci-cd/add-new-build.png)

1. Wenn Sie dazu aufgefordert werden, wählen Sie den Quellentyp **VSTS-Git** aus. Anschließend wählen Sie das Projekt, das Repository und den Branch aus, in dem sich der Code befindet. Klicken Sie auf **Weiter**.

    ![Auswählen des VSTS-Git](./media/how-to-ci-cd/select-vsts-git.png)

1. Wählen Sie im Fenster **Vorlage auswählen** die Option zum **Starten mit einem leeren Prozess** aus.

    ![Leer starten](./media/how-to-ci-cd/start-with-empty.png)

1. Klicken Sie auf **+** rechts neben **Phase 1**, um eine Aufgabe zur Phase hinzuzufügen. Suchen Sie dann nach **.Net Core**, wählen Sie diesen Eintrag aus, und klicken Sie auf **Hinzufügen**, um diese Aufgabe zur Phase hinzuzufügen.

    ![Dotnet-Test](./media/how-to-ci-cd/add-dot-net-core.png)

1. Aktualisieren Sie **Anzeigename** in **dotnet test**, und wählen Sie in der Dropdownliste **Befehl** den Eintrag **test** aus. Fügen Sie dem **Pfad zu Projekten** den unten angegebenen Pfad hinzu.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![Konfigurieren von „dotnet test“](./media/how-to-ci-cd/dotnet-test.png)

1. Klicken Sie auf **+** rechts neben **Phase 1**, um eine Aufgabe zur Phase hinzuzufügen. Suchen Sie nach **Azure IoT Edge**, wählen Sie diesen Eintrag aus, und klicken Sie **zweimal** auf die Schaltfläche **Hinzufügen**, um diese Aufgaben zur Phase hinzuzufügen.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. Aktualisieren Sie in der ersten Azure IoT Edge-Aufgabe **Anzeigename** in **Modul erstellen und pushen**, und wählen Sie in der Dropdownliste **Aktion** den Eintrag **Erstellen und Pushen** aus. Fügen Sie im Textfeld **Module.json-Datei** den unten angegebenen Pfad hinzu. Wählen Sie dann den **Containerregistrierungstyp** aus, und stellen Sie sicher, dass Sie die gleiche Registrierung in Ihrem Code konfigurieren und auswählen. Mit dieser Aufgabe werden alle Module in der Projektmappe erstellt und mithilfe von Push übertragen sowie in der von Ihnen angegebenen Containerregistrierung veröffentlicht. 

    ```
    **/module.json
    ```

    ![Modul erstellen und pushen](./media/how-to-ci-cd/module-build-push.png)

1. Aktualisieren Sie in der zweiten Azure IoT Edge-Aufgabe **Anzeigename** in **Auf IoT Edge-Gerät bereitstellen**, und wählen Sie in der Dropdownliste **Aktion** den Eintrag **Auf IoT Edge-Gerät bereitstellen** aus. Wählen Sie Ihr Azure-Abonnement aus, und geben Sie Ihren IoT Hub-Namen ein. Sie können eine IoT Edge-Bereitstellungs-ID und die Bereitstellungspriorität angeben. Sie können auch eine Bereitstellung auf einem einzelnen oder mehreren Geräten auswählen. Bei einer Bereitstellung auf mehreren Geräten müssen Sie die Zielbedingung des Geräts angeben. Wenn Sie beispielsweise Gerätetags als Bedingung verwenden möchten, müssen Sie die entsprechenden Gerätetags vor der Bereitstellung aktualisieren. 

    ![Bereitstellung in Edge](./media/how-to-ci-cd/deploy-to-edge.png)

1. Klicken Sie auf den **Prozess**, und stellen Sie sicher, dass für Ihre **Agent-Warteschlange** die Option **Gehostete Linux-Vorschau** festgelegt ist.

    ![Konfigurieren](./media/how-to-ci-cd/configure-env.png)

1. Öffnen Sie die Registerkarte **Trigger**, und aktivieren Sie den Trigger **Continuous Integration**. Stellen Sie sicher, dass der Branch mit Ihrem Code enthalten ist.

    ![Trigger](./media/how-to-ci-cd/configure-trigger.png)

1. Speichern Sie die neue Builddefinition, und fügen Sie einen neuen Build zur Warteschlange hinzu. Klicken Sie auf die Schaltfläche **Speichern und in Warteschlange einreihen**.

1. Wählen Sie in der daraufhin angezeigten Meldungsleiste den Link für den Build aus. Sie können auch zur Builddefinition wechseln, um den zuletzt in die Warteschlange gestellten Buildauftrag anzuzeigen.

    ![Entwickeln](./media/how-to-ci-cd/build-def.png)

1. Nachdem der Build abgeschlossen ist, werden die Zusammenfassung für jede Aufgabe und die Ergebnisse in der Liveprotokolldatei angezeigt. 
    
    ![Abgeschlossen](./media/how-to-ci-cd/complete.png)

1. Sie können zu Visual Studio Code zurückkehren und den Device Explorer von IoT Hub überprüfen. Das Edge-Gerät mit dem Modul sollte ausgeführt werden. (Stellen Sie sicher, dass Sie Registrierungsanmeldeinformationen zur Edge-Runtime hinzugefügt haben.)

    ![Edge wird ausgeführt](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>Continuous Deployment für IoT Edge-Geräte

Für Continuous Deployment müssen Sie CI-Aufträge mit geeigneten IoT Edge-Geräten einrichten und dadurch die **Trigger** für Ihre Branches in Ihrem Projekt aktivieren. Bei der klassischen DevOps-Methode enthält ein Projekt zwei Hauptbranches. Der Masterbranch sollte die stabile Version des Codes sein, und der Entwicklerbranch enthält die neuesten Codeänderungen. Jeder Entwickler im Team sollte beim Aktualisieren des Codes den Entwicklerbranch in einen eigenen Featurebranch verzweigen, sodass alle Commits auf Featurebranches außerhalb des Entwicklerbranches stattfinden. Außerdem sollten alle mithilfe von Push übertragenen Commits über das CI-System getestet werden. Nachdem der Code lokal vollständig getestet wurde, sollte der Featurebranch über einen Pull Request mit dem Entwicklerbranch zusammengeführt werden. Nachdem der Code im Entwicklerbranch über das CI-System getestet wurde, kann er über einen Pull Request mit dem Masterbranch zusammengeführt werden.

Es sind also bei der Bereitstellung auf IoT Edge-Geräten drei Hauptumgebungen vorhanden.
- Für den Featurebranch kann ein simuliertes IoT Edge-Gerät auf dem Entwicklungscomputer verwendet werden oder eine Bereitstellung auf einem physischen IoT Edge-Gerät erfolgen.
- Für den Entwicklerbranch sollte eine Bereitstellung auf einem physischen IoT Edge-Gerät erfolgen.
- Beim Masterbranch sollten die IoT Edge-Zielgeräte die Produktionsgeräte sein.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wird gezeigt, wie Sie die Features für Continuous Integration und Continuous Deployment von VSTS oder TFS verwenden können. 

* Grundlegende Informationen zur IoT Edge-Bereitstellung finden Sie unter [Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig](module-deployment-monitoring.md).
* Führen Sie die Schritte zum Erstellen, Aktualisieren oder Löschen einer Bereitstellung in [Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab][how-to-deploy-monitor.md] aus.









