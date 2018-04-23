---
title: SQL-Modul in Azure IoT Edge | Microsoft-Dokumentation
description: Speichern von Daten im Edge-Bereich mit Microsoft SQL-Modulen, einschließlich Azure Functions zum Formatieren der Daten
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: ebertrams
ms.date: 02/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d464bbfb9f38b184e47911a7224be8ec8679f0be
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="store-data-at-the-edge-with-sql-server-databases"></a>Speichern von Daten im Edge-Bereich mit SQL Server-Datenbanken

Verwenden Sie Azure IoT Edge-Geräte zum Speichern von Daten, die im Edge-Bereich geniert werden. Geräte mit zeitweiligen Internetverbindungen können eigene Datenbanken verwalten und Änderungen nur dann an die Cloud zurückmelden, wenn eine Verbindung besteht. Geräte, die so programmiert sind, dass nur wichtige Daten an die Cloud gesendet werden, können die restlichen Daten für reguläre Massenuploads speichern. Sobald sich die strukturierten Daten in der Cloud befinden, können sie für andere Azure-Dienste freigegeben werden, um beispielsweise ein Machine Learning-Modell zu erstellen. 

Dieser Artikel enthält Anweisungen zum Bereitstellen einer SQL Server-Datenbank auf einem IoT Edge-Gerät. Mithilfe von Azure Functions, das auf dem IoT Edge-Gerät ausgeführt wird, werden die eingehenden Daten strukturiert und dann an die Datenbank gesendet. Die Schritte in diesem Artikel können auch auf andere Datenbanken angewendet werden, die in Containern ausgeführt werden, z. B. MySQL oder PostgreSQL. 

## <a name="prerequisites"></a>Voraussetzungen 

Bevor Sie mit den Anweisungen in diesem Artikel beginnen, sollten Sie die folgenden Tutorials durcharbeiten:
* Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows](tutorial-simulate-device-windows.md) oder [Linux](tutorial-simulate-device-linux.md)
* [Bereitstellen einer Azure-Funktion als IoT Edge-Modul](tutorial-deploy-function.md)

Die folgenden Artikel sind zwar für einen erfolgreichen Abschluss dieses Tutorials nicht erforderlich, können aber hilfreichen Kontext bieten:
* [Ausführen des SQL Server 2017-Containerimages mit Docker](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker)
* [Verwenden von Visual Studio Code zum Entwickeln und Bereitstellen von Azure Functions in Azure IoT Edge](how-to-vscode-develop-azure-function.md)

Nach Abschluss der erforderlichen Tutorials sollten alle benötigten Komponenten auf Ihrem Computer bereitstehen: 
* Ein aktiver Azure IoT Hub
* Ein IoT Edge-Gerät mit mindestens 2 GB RAM und einem Laufwerk mit 2 GB
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Erweiterung C# for Visual Studio Code mit Unterstützung von OmniSharp](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker](https://docs.docker.com/engine/installation/)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
* [Python 2.7](https://www.python.org/downloads/)
* [IoT Edge-Steuerskript](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
* AzureIoTEdgeFunction-Vorlage (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
* Ein aktiver IoT Hub mit mindestens einem IoT Edge-Gerät

Es können sowohl Windows- als auch Linux-Container in X64-Prozessorarchitekturen für dieses Tutorial verwendet werden. SQL Server unterstützt keine ARM-Prozessoren.

## <a name="deploy-a-sql-server-container"></a>Bereitstellen eines SQL Server-Containers

In diesem Abschnitt fügen Sie dem simulierten IoT Edge-Gerät eine MS SQL-Datenbank hinzu. Verwenden Sie das SQL Server 2017-Docker-Containerimage, das als [Windows](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/)-Container und [Linux](https://hub.docker.com/r/microsoft/mssql-server-linux/)-Container verfügbar ist. 

### <a name="deploy-sql-server-2017"></a>Bereitstellen von SQL Server 2017

Standardmäßig wird mit dem Code in diesem Abschnitt ein Container mit der kostenlosen Developer Edition von SQL Server 2017 erstellt. Wenn Sie stattdessen Produktionseditionen ausführen möchten, finden Sie ausführliche Informationen dazu unter [Ausführen von Produktionscontainerimages](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#production). 

In Schritt 3 fügen Sie Erstellungsoptionen zum SQL Server-Container hinzu, die für das Erstellen von Umgebungsvariablen und permanentem Speicher wichtig sind. Mit den konfigurierten [Umgebungsvariablen](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-environment-variables) wird der Endbenutzer-Lizenzvertrag akzeptiert und ein Kennwort definiert. Der [permanente Speicher](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#persist) wird mithilfe von [Einbindungen](https://docs.docker.com/storage/volumes/) konfiguriert. Durch Einbindungen wird der SQL Server 2017-Container mit einem angefügten Volumecontainer *sqlvolume* erstellt, damit die Daten erhalten bleiben, selbst wenn der Container gelöscht wird. 

1. Öffnen Sie die Datei `deployment.json` in Visual Studio Code. 
2. Ersetzen Sie den Abschnitt **modules** der Datei durch den folgenden Code: 

   ```json
   "modules": {
          "filterFunction": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "<docker registry address>/filterfunction:latest",
              "createOptions": "{}"
            }
          },
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
              "createOptions": "{}"
            }
          },
          "sql": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "",
              "createOptions": ""
             }
          }
        }
   ```

3. Ersetzen Sie `<docker registry address>` durch die Adresse, die Sie im Tutorial [Bereitstellen einer Azure-Funktion als IoT Edge-Modul – Vorschau](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-deploy-function) angegeben haben.

   >[!NOTE]
   >Die Adresse der Containerregistrierung ist identisch mit dem Anmeldeserver, den Sie aus der Registrierung kopiert haben. Sie muss im folgenden Format angegeben werden: `<your container registry name>.azurecr.io`

4. Aktualisieren Sie die Einstellungen für das SQL-Modul mit dem folgenden Code entsprechend dem verwendeten Betriebssystem: 

   * Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

5. Speichern Sie die Datei . 
6. Wählen Sie in der Befehlspalette von Visual Studio Code den Befehl **Edge: Bereitstellung für Edge-Gerät erstellen** aus. 
7. Wählen Sie die ID Ihres IoT Edge-Geräts aus.
8. Wählen Sie die von Ihnen aktualisierte Datei `deployment.json` aus. Im Ausgabefenster können Sie die entsprechenden Ausgaben für Ihre Bereitstellung sehen. 
9. Zum Starten der Edge-Runtime wählen Sie in der Befehlspalette **Edge: Edge starten** aus.

>[!TIP]
>Bei jedem Erstellen eines SQL Server-Containers in einer Produktionsumgebung sollten Sie [das Standardkennwort für den Systemadministrator ändern](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

## <a name="create-the-sql-database"></a>Erstellen der SQL-Datenbank

Dieser Abschnitt führt Sie durch das Einrichten der SQL-Datenbank zum Speichern der Temperaturdaten, die von den an das IoT Edge-Gerät angeschlossenen Sensoren empfangen werden. Wenn Sie ein simuliertes Gerät verwenden, stammen diese Daten aus dem Container *tempSensor*. 

Stellen Sie in einem Befehlszeilentool eine Verbindung mit Ihrer Datenbank her: 

* Windows-Container
   ```cmd
   docker exec -it sql cmd
   ```

* Linux-Container
   ```bash
   docker exec -it sql bash
   ```

Öffnen Sie das SQL-Befehlstool: 

* Windows-Container
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Linux-Container
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Erstellen Sie die Datenbank: 

* Windows-Container
   ```sql
   CREATE DATABASE MeasurementsDB
   ON
   (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
   GO
   ```

* Linux-Container
   ```sql
   CREATE DATABASE MeasurementsDB
   ON
   (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
   GO
   ```

Definieren Sie die Tabelle: 

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

Sie können die SQL Server-Docker-Datei so anpassen, dass SQL Server automatisch für die Bereitstellung auf mehreren IoT Edge-Geräten eingerichtet wird. Weitere Informationen finden Sie im [Demoprojekt für einen Microsoft SQL Server-Container](https://github.com/twright-msft/mssql-node-docker-demo-app).

## <a name="understand-the-sql-connection"></a>Grundlegendes zur SQL-Verbindung

In anderen Tutorials werden Routen verwendet, damit Container kommunizieren können, während sie weiterhin voneinander isoliert sind. Beim Arbeiten mit einer SQL Server-Datenbank ist jedoch eine engere Beziehung erforderlich. 

IoT Edge erstellt beim Starten automatisch ein Bridge- (Linux) oder NAT-Netzwerk (Windows). Das Netzwerk hat den Namen **azure-iot-edge**. Wenn Sie diese Verbindung debuggen müssen, können Sie die Eigenschaften in der Befehlszeile nachschlagen:

* Windows

   ```cmd
   docker network inspect azure-iot-edge
   ```

* Linux

   ```bash
   sudo docker network inspect azure-iot-edge
   ```

IoT Edge kann auch den DNS eines Containernamens über Docker auflösen, sodass Sie nicht mit der IP-Adresse auf die SQL Server-Datenbank verweisen müssen. 

Hier folgt als Beispiel die Verbindungszeichenfolge, die im nächsten Abschnitt verwendet wird: 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

Wie Sie sehen, wird in der Verbindungszeichenfolge mit dem Namen (**sql**) auf den Container verwiesen. Wenn Sie den Modulnamen geändert haben, aktualisieren Sie auch diese Verbindungszeichenfolge. Andernfalls fahren Sie mit dem nächsten Abschnitt fort. 

## <a name="update-your-azure-function"></a>Aktualisieren der Azure-Funktion
Um Daten an Ihre Datenbank zu senden, aktualisieren Sie die Azure-Funktion „FilterFunction“, die Sie im vorherigen Tutorial erstellt haben. Ändern Sie diese Datei so, dass die von den Sensoren empfangenen Daten strukturiert und dann in einer SQL-Tabelle gespeichert werden. 

1. Öffnen Sie in Visual Studio Code den Ordner „FilterFunction“. 
2. Ersetzen Sie die Datei „run.csx“ durch den folgenden Code, der die SQL-Verbindungszeichenfolge aus dem vorherigen Abschnitt enthält: 

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"
   #r "System.Data.SqlClient"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;
   using System.Threading.Tasks;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
       const int temperatureThreshold = 25;
       byte[] messageBytes = messageReceived.GetBytes();
       var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

       if (!string.IsNullOrEmpty(messageString))
       {
           // Get the body of the message and deserialize it
           var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

           //Store the data in SQL db
           const string str = "Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;";
           using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
           {
           conn.Open();
           var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
           var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
               using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
               {
               //Execute the command and log the # rows affected.
               var rows = await cmd.ExecuteNonQueryAsync();
               log.Info($"{rows} rows were updated");
               }
           }

           if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
           {
               // Send the message to the output as the temperature value is greater than the threshold
               var filteredMessage = new Message(messageBytes);
               // Copy the properties of the original message into the new Message object
               foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
               {
                   filteredMessage.Properties.Add(prop.Key, prop.Value);
               }
               // Add a new property to the message to indicate it is an alert
               filteredMessage.Properties.Add("MessageType", "Alert");
               // Send the message        
               await output.AddAsync(filteredMessage);
               log.Info("Received and transferred a message with temperature above the threshold");
           }
       }
   }

   //Define the expected schema for the body of incoming messages
   class MessageBody
   {
       public Machine machine {get;set;}
       public Ambient ambient {get; set;}
       public string timeCreated {get; set;}
   }
   class Machine
   {
      public double temperature {get; set;}
      public double pressure {get; set;}         
   }
   class Ambient
   {
      public double temperature {get; set;}
      public int humidity {get; set;}         
   }
   ```

## <a name="update-your-container-image"></a>Aktualisieren des Containerimages

Zum Anwenden der vorgenommenen Änderungen aktualisieren und veröffentlichen Sie das Containerimage, und starten Sie IoT Edge erneut.

1. Erweitern Sie in Visual Studio Code den Ordner **Docker**. 
2. Je nach verwendeter Plattform erweitern Sie entweder den Ordner **windows-nano** oder den Ordner **linux-x64**. 
3. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Docker-Image für IoT Edge-Modul erstellen**.
4. Navigieren Sie zum Projektordner **FilterFunction**, und klicken Sie dann auf **Ordner als EXE_DIR auswählen**.
5. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filterfunction:latest`. Wenn Sie eine Bereitstellung für eine lokale Registrierung durchführen, sollte der Name `<localhost:5000/filterfunction:latest>` lauten.
6. Wählen Sie in der Befehlspalette von Visual Studio Code den Befehl **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen** aus. 
7. Geben Sie im Popuptextfeld denselben Imagenamen ein. 
8. Wählen Sie in der Befehlspalette von Visual Studio Code den Befehl **Edge: Edge neu starten** aus.

## <a name="view-the-local-data"></a>Anzeigen der lokalen Daten

Sobald die Container neu gestartet sind, werden die von den Temperatursensoren empfangenen Daten in einer lokalen SQL Server 2017-Datenbank auf Ihrem IoT Edge-Gerät gespeichert. 

Stellen Sie in einem Befehlszeilentool eine Verbindung mit Ihrer Datenbank her: 

* Windows-Container
   ```cmd
   docker exec -it sql cmd
   ```

* Linux-Container
   ```bash
   docker exec -it sql bash
   ```

Öffnen Sie das SQL-Befehlstool: 

* Windows-Container
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Linux-Container
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Zeigen Sie die Daten an: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [SQL Server 2017-Containerimages unter Docker konfigurieren](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker).

* Informationen zu Ressourcen, Feedback und bekannte Probleme finden Sie im [GitHub-Repository für mssql-docker](https://github.com/Microsoft/mssql-docker).
