---
title: Speichern von Daten mit dem SQL-Modul in Azure IoT Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit einem SQL Server-Modul Daten lokal auf Ihrem IoT Edge-Gerät speichern.
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: f10b8e34c653add6f93b0234a9efff43d78036f3
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237211"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Tutorial: Speichern von Daten im Edge-Bereich mit SQL Server-Datenbanken

Verwenden Sie Azure IoT Edge und SQL Server, um Daten im Edge-Bereich zu speichern und abzufragen. In Azure IoT Edge sind grundlegende Speicherfeatures bereits integriert. Mithilfe dieser Features werden Nachrichten zwischengespeichert, wenn ein Gerät offline geschaltet wird, und weitergeleitet, wenn die Verbindung wiederhergestellt wurde. Sie benötigen jedoch unter Umständen komplexere Speicherfunktionen, beispielsweise für die lokale Datenabfrage. Durch die Integration von lokalen Datenbanken können IoT Edge-Geräte komplexere Computevorgänge ausführen, ohne dass eine Verbindung mit IoT Hub bestehen muss. Beispiel: Ein Techniker kann die Sensordaten der letzten Tage lokal auf einem Computer visuell darstellen, auch wenn diese Daten zur Verbesserung eines Machine Learning-Modells nur einmal pro Woche in die Cloud hochgeladen werden.

Dieser Artikel enthält Anweisungen zum Bereitstellen einer SQL Server-Datenbank auf einem IoT Edge-Gerät. Mithilfe von Azure Functions, das auf dem IoT Edge-Gerät ausgeführt wird, werden die eingehenden Daten strukturiert und dann an die Datenbank gesendet. Die Schritte in diesem Artikel können auch auf andere Datenbanken angewendet werden, die in Containern ausgeführt werden, z. B. MySQL oder PostgreSQL.

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Erstellen einer Azure-Funktion mit Visual Studio Code
> * Bereitstellen einer SQL-Datenbank auf Ihrem IoT Edge-Gerät
> * Erstellen und Bereitstellen von Modulen auf Ihrem IoT Edge-Gerät mithilfe von Visual Studio Code
> * Anzeigen generierter Daten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.

Cloudressourcen:

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) im kostenlosen Tarif in Azure. 

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#-Erweiterung für Visual Studio Code (unterstützt von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) für Visual Studio Code 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für VSCode zum Entwickeln eines Moduls und zum Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Container Registry** aus.

    ![Erstellen einer Containerregistrierung](./media/tutorial-deploy-function/create-container-registry.png)

2. Benennen Sie die Registrierung, und wählen Sie ein Abonnement aus.
3. Es wird empfohlen, für die Ressourcengruppe den gleichen Namen wie für die Ressourcengruppe zu verwenden, die Ihre IoT Hub-Instanz enthält. Indem Sie alle Ressourcen in der gleichen Gruppe zusammenfassen, können Sie sie zusammen verwalten. Wenn Sie beispielsweise die zum Testen verwendete Ressourcengruppe löschen, werden alle Testressourcen in dieser Gruppe gelöscht. 
4. Legen Sie die SKU auf **Basic** fest, und schalten Sie **Administratorbenutzer** auf **Aktivieren** um. 
5. Klicken Sie auf **Create**.
6. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und wählen Sie **Zugriffsschlüssel** aus. 
7. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte später im Tutorial. 

## <a name="create-a-function-project"></a>Erstellen eines Funktionsprojekts

Zum Senden von Daten an eine Datenbank benötigen Sie ein Modul, das die Daten entsprechend strukturieren kann und dann in einer Tabelle speichert. 

Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung eine IoT Edge-Funktion erstellen.

1. Öffnen Sie Visual Studio Code.
2. Öffnen Sie das in VS Code integrierte Terminal über **Ansicht** > **Terminal**.
3. Öffnen Sie die VS Code-Befehlspalette, indem Sie auf **Ansicht** > **Befehlspalette** klicken.
4. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** (Azure: Anmelden) ein, und führen Sie ihn aus. Befolgen Sie die Anweisungen zum Anmelden beim Azure-Konto. Wenn Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.
3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: Neue IoT Edge-Projektmappe) ein, und führen Sie ihn aus. Geben Sie in der Befehlspalette die folgenden Informationen an, um die Projektmappe zu erstellen: 
   1. Wählen Sie den Ordner aus, in dem die Projektmappe erstellt werden soll. 
   2. Geben Sie einen Namen für Ihre Projektmappe ein, oder übernehmen Sie den Standardnamen **EdgeSolution**.
   3. Wählen Sie als Modulvorlage die Option **Azure Functions - C#** aus. 
   4. Nennen Sie das Modul **sqlFunction**. 
   5. Geben Sie die Azure Container Registry-Instanz an, die Sie im vorherigen Abschnitt als Imagerepository für das erste Modul erstellt haben. Ersetzen Sie **localhost:5000** durch den kopierten Wert für den Anmeldeserver. Die endgültige Zeichenfolge sieht wie folgt aus: **\<Registrierungsname\>.azurecr.io/sqlFunction**.

4. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Er enthält einen Ordner für **Module**, den Ordner **.vscode** und eine Bereitstellungsmanifest-Vorlagendatei. Öffnen Sie **modules** > **sqlFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

5. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

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
           const string str = "<sql connection string>";
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

6. Ersetzen Sie in Zeile 24 die Zeichenfolge **\<sql connection string\>** durch die folgende Zeichenfolge. Die Eigenschaft **Data Source** verweist auf den SQL Server-Containernamen, den Sie im nächsten Abschnitt mit dem Namen **SQL** erstellen. 

   ```C#
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Speichern Sie die Datei **run.csx**. 

## <a name="add-a-sql-server-container"></a>Hinzufügen eines SQL Server-Containers

In einem [Bereitstellungsmanifest](module-composition.md) wird deklariert, welche Module von der IoT Edge-Runtime auf dem IoT Edge-Gerät installiert werden. Sie haben im vorherigen Abschnitt den Code zum Erstellen eines angepassten Functions-Modul hinzugefügt. Das SQL Server-Modul ist jedoch bereits erstellt. Sie müssen lediglich die IoT Edge-Runtime anweisen, es aufzunehmen, und es dann auf Ihrem Gerät konfigurieren. 

1. Öffnen Sie im Visual Studio Code-Explorer die Datei **deployment.template.json**. 
2. Suchen Sie den Abschnitt **moduleContent.$edgeAgent.properties.desired.modules**. Zwei Module sollten aufgeführt sein: das Modul **tempSensor**, das simulierte Daten generiert, und das Modul **sqlFunction**.
3. Ändern Sie bei Verwendung von Windows-Containern den Abschnitt **sqlFunction.settings.image**.
    ```json
    "image": "${MODULES.sqlFunction.windows-amd64}"
    ```
4. Fügen Sie den folgenden Code hinzu, um ein drittes Modul zu deklarieren. Fügen Sie nach dem Abschnitt „sqlFunction“ ein Komma und dann Folgendes ein:

   ```json
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
   ```

   Das folgende Beispiel veranschaulicht die Vorgehensweise für das Hinzufügen eines JSON-Elements: ![Hinzufügen eines SQL Server-Containers](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. Aktualisieren Sie abhängig vom Typ des Docker-Containers auf Ihrem IoT Edge-Gerät die **sql.settings**-Parameter mit dem folgenden Code:

   * Windows-Container:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Linux-Container:

      ```json
      "image": "mcr.microsoft.com/mssql/server:latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   >[!Tip]
   >Bei jedem Erstellen eines SQL Server-Containers in einer Produktionsumgebung sollten Sie [das Standardkennwort für den Systemadministrator ändern](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

6. Speichern Sie die Datei **deployment.template.json**.

## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

In den vorherigen Abschnitten haben Sie eine Projektmappe mit einem Modul erstellt und anschließend ein weiteres Modul zur Bereitstellungsmanifestvorlage hinzugefügt. Nun müssen Sie die Projektmappe sowie Containerimages für die Module erstellen und die Images per Push an die Containerregistrierung übertragen. 

1. Stellen Sie in der ENV-Datei für die IoT Edge-Runtime Ihre Registrierungsanmeldeinformationen bereit, sodass sie auf die Modulimages zugreifen kann. Suchen Sie die Abschnitte **CONTAINER_REGISTRY_USERNAME** und **CONTAINER_REGISTRY_PASSWORD**, und fügen Sie nach dem Gleichheitszeichen Ihre Anmeldeinformationen ein: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourContainerReg=<username>
   CONTAINER_REGISTRY_PASSWORD_yourContainerReg=<password>
   ```

2. Speichern Sie die ENV-Datei.
3. Melden Sie sich bei der Containerregistrierung in Visual Studio Code an, sodass Sie die Images per Push an die Registrierung übertragen können. Verwenden Sie die gleichen Anmeldeinformationen, die Sie der ENV-Datei hinzugefügt haben. Geben Sie den folgenden Befehl in das integrierte Terminal ein:

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Sie werden zur Eingabe des Kennworts aufgefordert. Fügen Sie Ihr Kennwort in die Eingabeaufforderung ein, und drücken Sie die **EINGABETASTE**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

4. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie anschließend auf **Build and Push IoT Edge solution** (IoT Edge-Projektmappe erstellen und übertragen). 

## <a name="deploy-the-solution-to-a-device"></a>Bereitstellen der Projektmappe auf einem Gerät

Sie können über IoT Hub Module auf einem Gerät festlegen, können jedoch auch über Visual Studio Code auf IoT Hub und Geräte zugreifen. In diesem Abschnitt richten Sie den Zugriff auf Ihre IoT Hub-Instanz ein und stellen dann mithilfe von VS Code Ihre Projektmappe auf dem IoT Edge-Gerät bereit. 

1. Wählen Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: IoT Hub auswählen) aus.
2. Folgen Sie den Aufforderungen auf dem Bildschirm, um sich an Ihrem Azure-Konto anzumelden. 
3. Wählen Sie in der Befehlspalette Ihr Azure-Abonnement und dann Ihre IoT Hub-Instanz aus. 
4. Erweitern Sie im VS Code-Explorer den Abschnitt **Azure IoT Hub-Geräte**. 
5. Klicken Sie mit der rechten Maustaste auf das Gerät, das als Ziel für die Bereitstellung festgelegt werden soll, und klicken Sie auf **Create deployment for single device** (Bereitstellung für einzelnes Gerät erstellen). 
6. Navigieren Sie im Datei-Explorer zum Ordner **config** der Projektmappe, und klicken Sie auf **deployment.json**. Klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen). 

War die Bereitstellung erfolgreich, wird eine Bestätigungsnachricht in VS Code ausgegeben. Sie können auch überprüfen, ob alle Module auf dem Gerät ausgeführt werden. 

Führen Sie auf dem IoT Edge-Gerät den folgenden Befehl aus, um den Status der Module anzuzeigen. Dies kann einige Minuten dauern.

   ```PowerShell
   iotedge list
   ```

## <a name="create-the-sql-database"></a>Erstellen der SQL-Datenbank

Wenn Sie das Bereitstellungsmanifest auf Ihr Gerät anwenden, werden drei Module ausgeführt. Das Modul „tempSensor“ generiert simulierte Umgebungsdaten. Das Modul „sqlFunction“ formatiert die Daten für eine Datenbank. 

Dieser Abschnitt führt Sie durch das Einrichten der SQL-Datenbank zum Speichern der Temperaturdaten. 

1. Stellen Sie in einem Befehlszeilentool eine Verbindung mit Ihrer Datenbank her. 
   * Windows-Container:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Linux-Container: 

      ```bash
      docker exec -it sql bash
      ```

2. Öffnen Sie das SQL-Befehlstool.
   * Windows-Container:

      ```cmd
      sqlcmd -S localhost -U SA -P "Strong!Passw0rd"
      ```

   * Linux-Container: 

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Erstellen Sie die Datenbank: 

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

4. Definieren Sie die Tabelle.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

Sie können die SQL Server-Docker-Datei so anpassen, dass SQL Server automatisch für die Bereitstellung auf mehreren IoT Edge-Geräten eingerichtet wird. Weitere Informationen finden Sie im [Demoprojekt für einen Microsoft SQL Server-Container](https://github.com/twright-msft/mssql-node-docker-demo-app). 

## <a name="view-the-local-data"></a>Anzeigen der lokalen Daten

Nach der Erstellung der Tabelle beginnt das Modul „sqlFunction“ mit dem Speichern von Daten in einer lokalen SQL Server 2017-Datenbank auf dem IoT Edge-Gerät. 

Führen Sie im SQL-Befehlstool den folgenden Befehl aus, um die formatierten Tabellendaten anzuzeigen: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Anzeigen lokaler Daten](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen. 

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Azure Functions-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie eigene Module erstellen möchten, finden Sie entsprechende Informationen unter [Use Visual Studio Code to develop and debug Azure Functions for Azure IoT Edge](how-to-develop-csharp-function.md) (Entwickeln und Debuggen von Azure Functions für Azure IoT Edge mithilfe von Visual Studio Code). 

Sie können mit den nächsten Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure Stream Analytics als IoT Edge-Modul – Vorschau](tutorial-deploy-stream-analytics.md)
