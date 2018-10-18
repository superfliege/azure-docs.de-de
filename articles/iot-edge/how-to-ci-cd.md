---
title: Continuous Integration und Continuous Deployment für Azure IoT Edge | Microsoft-Dokumentation
description: Übersicht über Continuous Integration und Continuous Deployment für Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5099ca70503ba2ed4ae8f4969a9199816c4986fb
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302570"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continuous Integration und Continuous Deployment für Azure IoT Edge

In diesem Artikel wird gezeigt, wie Sie die Features für Continuous Integration und Continuous Deployment von Azure DevOps Services und Microsoft Team Foundation Server (TFS) zum schnellen und effizienten Erstellen, Testen und Bereitstellen von Anwendungen in Azure IoT Edge verwenden können. 

In diesem Artikel wird Folgendes behandelt:
* Erstellen und Einchecken einer IoT Edge-Beispielprojektmappe, die Komponententests enthält
* Installieren der Azure IoT Edge-Erweiterung für Ihr Azure DevOps
* Konfigurieren von Continuous Integration (CI) zum Erstellen der Projektmappe und Ausführen der Komponententests
* Konfigurieren von Continuous Deployment (CD) zum Bereitstellen der Projektmappe und Anzeigen von Antworten

Das Abschließen der Schritte in diesem Artikel dauert ca. 30 Minuten.

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
                Init().Wait();

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
            /// Initializes the ModuleClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init()
            {
                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessage, ioTHubModuleClient);
            }

            /// <summary>
            /// This method is called whenever the module is sent a message from the EdgeHub. 
            /// It just pipe the messages without any change.
            /// It prints all the incoming messages.
            /// </summary>
            static async Task<MessageResponse> FilterMessage(Message message, object userContext)
            {
                int counterValue = Interlocked.Increment(ref counter);

                var moduleClient = userContext as ModuleClient;
                if (moduleClient == null)
                {
                    throw new InvalidOperationException("UserContext doesn't contain " + "expected values");
                }

                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                var filteredMessage = filter(message);

                if (filteredMessage != null && !string.IsNullOrEmpty(messageString))
                {
                    var pipeMessage = new Message(messageBytes);
                    foreach (var prop in message.Properties)
                    {
                        pipeMessage.Properties.Add(prop.Key, prop.Value);
                    }
                    await moduleClient.SendEventAsync("output1", pipeMessage);
                    Console.WriteLine("Received message sent");
                }
                return MessageResponse.Completed;
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

7. Speichern Sie diese Projekte, und checken Sie sie dann in Ihr Azure DevOps- oder TFS-Repository ein.
    

> [!NOTE]
> Weitere Informationen zur Verwendung von Azure Repos finden Sie unter [Freigeben von Code mit Visual Studio und Azure-Repositorys](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Konfigurieren von Continuous Integration
In diesem Abschnitt erstellen Sie eine Buildpipeline, die so konfiguriert ist, dass sie beim Einchecken von Änderungen der IoT Edge-Beispielprojektmappe automatisch ausgeführt wird und auch die darin enthaltenen Komponententests automatisch ausgeführt werden.

1. Melden Sie sich bei Ihrer Azure DevOps-Organisation (**https://**_Ihr_Konto_**.visualstudio.com**) an, und öffnen Sie das Projekt, in das Sie die Beispiel-App eingecheckt haben.

    ![Einchecken von Code](./media/how-to-ci-cd/init-project.png)

1. Besuchen Sie [Azure IoT Edge für Azure DevOps](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) im Azure DevOps Marketplace. Klicken Sie auf die Option **Kostenlos erhalten**, und führen Sie die Schritte im Assistenten aus, um diese Erweiterung in Ihrer Azure DevOps-Organisation zu installieren oder auf Ihren TFS herunterzuladen.

    ![Installieren der Erweiterung](./media/how-to-ci-cd/install-extension.png)

1. Öffnen Sie in Azure DevOps den Hub **Build &amp; Release** (Erstellen und freigeben), und wählen Sie auf der Registerkarte **Builds** die Option **+ Neue Pipeline** aus. Wenn Sie bereits über Buildpipelines verfügen, wählen Sie die Schaltfläche **+ Neu** aus. 

    ![Neuer Build](./media/how-to-ci-cd/add-new-build.png)

1. Wenn Sie dazu aufgefordert werden, wählen Sie den Quellentyp **Azure DevOps-Git** aus. Anschließend wählen Sie das Projekt, das Repository und den Branch aus, in dem sich der Code befindet. Klicken Sie auf **Weiter**.

    ![Auswählen des Azure DevOps-Git-Repositorys](./media/how-to-ci-cd/select-vsts-git.png)

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

1. Aktualisieren Sie in der ersten Azure IoT Edge-Aufgabe **Anzeigename** in **Modul erstellen und pushen**, und wählen Sie in der Dropdownliste **Aktion** den Eintrag **Erstellen und Pushen** aus. Fügen Sie im Textfeld **Module.json-Datei** den unten angegebenen Pfad hinzu. Wählen Sie dann den **Containerregistrierungstyp** aus, und stellen Sie sicher, dass Sie die gleiche Registrierung in Ihrem Code konfigurieren und auswählen. Mit dieser Aufgabe werden alle Module in der Projektmappe erstellt und mithilfe von Push übertragen sowie in der von Ihnen angegebenen Containerregistrierung veröffentlicht. Wenn Ihre Module in verschiedene Registrierungen übertragen werden, verfügen Sie über mehrere Aufgaben zum **Erstellen und Pushen von Modulen**.

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

1. Speichern Sie die neue Buildpipeline, und fügen Sie der Warteschlange einen neuen Build hinzu. Klicken Sie auf die Schaltfläche **Speichern und in Warteschlange einreihen**.

1. Wählen Sie in der daraufhin angezeigten Meldungsleiste den Link für den Build aus. Sie können auch zur Buildpipeline wechseln, um den zuletzt in die Warteschlange gestellten Buildauftrag anzuzeigen.

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

* Grundlegende Informationen zur IoT Edge-Bereitstellung finden Sie unter [Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig](module-deployment-monitoring.md).
* Führen Sie die Schritte zum Erstellen, Aktualisieren oder Löschen einer Bereitstellung in [Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab](how-to-deploy-monitor.md) aus.
