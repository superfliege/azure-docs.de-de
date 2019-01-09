---
title: 'Tutorial: Speichern von Daten mit dem SQL-Modul in Azure IoT Edge | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mit einem SQL Server-Modul Daten lokal auf Ihrem IoT Edge-Gerät speichern.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/01/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: a78c4d64761b5eecc5b7ebf8aa0223b862935575
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728192"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Tutorial: Speichern von Daten im Edge-Bereich mit SQL Server-Datenbanken

Verwenden Sie Azure IoT Edge und SQL Server, um Daten im Edge-Bereich zu speichern und abzufragen. Azure IoT Edge verfügt über grundlegende Speicherfunktionen, die Nachrichten zwischenspeichern, wenn ein Gerät offline geht, und weiterleiten, wenn die Verbindung wiederhergestellt wurde. Sie benötigen jedoch unter Umständen komplexere Speicherfunktionen, beispielsweise für die lokale Datenabfrage. Durch die Integration von lokalen Datenbanken können IoT Edge-Geräte komplexere Computevorgänge ausführen, ohne dass eine Verbindung mit IoT Hub bestehen muss. Ein Sensor in einem Gerät lädt beispielsweise einmal im Monat Daten zur Berichterstellung und zur Optimierung eines Machine Learning-Moduls in die Cloud hoch. Wenn ein Techniker am Gerät arbeitet, kann er jedoch lokal auf die Sensordaten der letzten Tage zugreifen.

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

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“. 

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#-Erweiterung für Visual Studio Code (unterstützt von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für Visual Studio Code zum Entwickeln eines Moduls und Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können eine beliebige Docker-kompatible Registrierung zum Speichern Ihrer Containerimages verwenden. Zwei beliebte Docker-Registrierungsdienste sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

Sollten Sie noch keine Containerregistrierung besitzen, führen Sie die folgenden Schritte aus, um eine neue Containerregistrierung in Azure zu erstellen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Container Registry** aus.

2. Geben Sie die folgenden Werte an, um Ihre Containerregistrierung zu erstellen:

   | Feld | Wert | 
   | ----- | ----- |
   | Registrierungsname | Geben Sie einen eindeutigen Namen an. |
   | Abonnement | Wählen Sie ein Abonnement aus der Dropdownliste aus. |
   | Ressourcengruppe | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |
   | Administratorbenutzer | Legen Sie **Aktivieren** fest. |
   | SKU | Wählen Sie **Basic** aus. | 

5. Klicken Sie auf **Erstellen**.

6. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und klicken Sie anschließend auf **Zugriffsschlüssel**. 

7. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte an späterer Stelle des Tutorials, um Zugriff auf die Containerregistrierung zu gewähren.  

## <a name="create-a-function-project"></a>Erstellen eines Funktionsprojekts

Zum Senden von Daten an eine Datenbank benötigen Sie ein Modul, das die Daten entsprechend strukturieren kann und dann in einer Tabelle speichert. 

Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung eine IoT Edge-Funktion erstellen.

1. Öffnen Sie Visual Studio Code.

2. Öffnen Sie die VS Code-Befehlspalette, indem Sie auf **Ansicht** > **Befehlspalette** klicken.

3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus. Geben Sie in der Befehlspalette die folgenden Informationen an, um die Projektmappe zu erstellen: 

   | Feld | Wert |
   | ----- | ----- |
   | Ordner auswählen | Wählen Sie den Speicherort auf Ihrem Entwicklungscomputer aus, an dem Visual Studio Code die Projektmappendateien erstellen soll. |
   | Provide a solution name (Projektmappennamen angeben) | Geben Sie für Ihre Projektmappe einen aussagekräftigen Namen ein (beispielsweise **SqlSolution**), oder übernehmen Sie den Standardnamen. |
   | Select module template (Modulvorlage auswählen) | Wählen Sie **Azure Functions – C#** aus. |
   | Provide a module name (Modulname angeben) | Nennen Sie das Modul **sqlFunction**. |
   | Provide Docker image repository for the module (Docker-Imagerepository für das Modul angeben) | Ein Imagerepository enthält den Namen Ihrer Containerregistrierung und den Namen Ihres Containerimages. Ihr Containerimage wird aus dem vorherigen Schritt übernommen. Ersetzen Sie **localhost:5000** durch den Anmeldeserverwert aus Ihrer Azure-Containerregistrierung. Den Anmeldeserver können Sie im Azure-Portal auf der Übersichtsseite Ihrer Containerregistrierung ermitteln. Die endgültige Zeichenfolge sieht wie folgt aus: „\<Registrierungsname\>.azurecr.io/sqlFunction“. |

   Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. 
   
4. Öffnen Sie in Ihrer IoT Edge-Projektmappe die Datei mit der Erweiterung „\.env“. 

   Wenn Sie eine neue IoT Edge-Projektmappe erstellen, fordert Visual Studio Code Sie auf, Ihre Anmeldeinformationen für die Registrierung in der \.ENV-Datei anzugeben. Diese Datei wird von Git ignoriert und später von der IoT Edge-Erweiterung verwendet, um Ihrem IoT Edge-Gerät Zugriff auf die Registrierung zu gewähren. 

   Falls Sie im vorherigen Schritt keine Containerregistrierung angegeben, sondern den Standardwert „localhost:5000“ übernommen haben, besitzen Sie keine Datei mit der Erweiterung „\.env“.

5. Stellen Sie in der ENV-Datei für die IoT Edge-Runtime Ihre Registrierungsanmeldeinformationen bereit, sodass sie auf die Modulimages zugreifen kann. Suchen Sie die Abschnitte **CONTAINER_REGISTRY_USERNAME** und **CONTAINER_REGISTRY_PASSWORD**, und fügen Sie nach dem Gleichheitszeichen Ihre Anmeldeinformationen ein: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourregistry=<username>
   CONTAINER_REGISTRY_PASSWORD_yourregistry=<password>
   ```

6. Speichern Sie die ENV-Datei.

7. Öffnen Sie im VS Code-Explorer **Module** > **sqlFunction** > **sqlFunction.cs**.

8. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Client;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.EdgeHub;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;

   namespace Functions.Samples
   {
       public static class sqlFunction
       {
           [FunctionName("sqlFunction")]
           public static async Task FilterMessageAndSendMessage(
               [EdgeHubTrigger("input1")] Message messageReceived,
               [EdgeHub(OutputName = "output1")] IAsyncCollector<Message> output,
               ILogger logger)
           {
               const int temperatureThreshold = 20;
               byte[] messageBytes = messageReceived.GetBytes();
               var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

               if (!string.IsNullOrEmpty(messageString))
               {
                   logger.LogInformation("Info: Received one non-empty message");
                   // Get the body of the message and deserialize it.
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
                           logger.LogInformation($"{rows} rows were updated");
                       }
                   }

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threashold.
                       var filteredMessage = new Message(messageBytes);
                       // Copy the properties of the original message into the new Message object.
                       foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                       {filteredMessage.Properties.Add(prop.Key, prop.Value);}
                       // Add a new property to the message to indicate it is an alert.
                       filteredMessage.Properties.Add("MessageType", "Alert");
                       // Send the message.       
                       await output.AddAsync(filteredMessage);
                       logger.LogInformation("Info: Received and transferred a message with temperature above the threshold");
                   }
               }
           }
       }
       //Define the expected schema for the body of incoming messages.
       class MessageBody
       {
           public Machine machine {get; set;}
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
   }
   ```

6. Ersetzen Sie in Zeile 35 die Zeichenfolge **\<sql connection string\>** durch die folgende Zeichenfolge. Die Eigenschaft **Data Source** verweist auf den Namen des SQL Server-Containers, den Sie im nächsten Abschnitt mit dem Namen **SQL** erstellen. 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Speichern Sie die Datei **sqlFunction.cs**. 

8. Öffnen Sie die Datei **sqlFunction.csproj**.

9. Suchen Sie nach der Gruppe der Paketverweise, und fügen Sie einen neuen Include-Verweis für „SqlClient“ hinzu. 

   ```csproj
   <PackageReference Include="System.Data.SqlClient" Version="4.5.1"/>
   ```

10. Speichern Sie die Datei **sqlFunction.csproj**.

## <a name="add-a-sql-server-container"></a>Hinzufügen eines SQL Server-Containers

In einem [Bereitstellungsmanifest](module-composition.md) wird deklariert, welche Module von der IoT Edge-Runtime auf dem IoT Edge-Gerät installiert werden. Sie haben im vorherigen Abschnitt den Code zum Erstellen eines angepassten Functions-Moduls bereitgestellt, das SQL Server-Modul wurde jedoch bereits erstellt. Sie müssen lediglich die IoT Edge-Runtime anweisen, es aufzunehmen, und es dann auf Ihrem Gerät konfigurieren. 

1. Öffnen Sie im Visual Studio Code-Explorer die Datei **deployment.template.json**. 

2. Suchen Sie nach dem Abschnitt **modules**. Zwei Module sollten aufgeführt sein: das Modul **tempSensor**, das simulierte Daten generiert, und das Modul **sqlFunction**.

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
     "env":{},
     "settings": {
       "image": "",
       "createOptions": ""
     }
   }
   ```

   ![Fügen Sie das SQL-Server-Modul dem Manifest inzu](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. Aktualisieren Sie abhängig vom Typ des Docker-Containers auf Ihrem IoT Edge-Gerät die **sql**-Modulparameter mit dem folgenden Code:
   * Windows-Container:

      ```json
      "env": {
        "ACCEPT_EULA": {"value": "Y"},
        "SA_PASSWORD": {"value": "Strong!Passw0rd"}
      },
      "settings": {
        "image": "microsoft/mssql-server-windows-developer",
        "createOptions": {
          "HostConfig": {
            "Mounts": [{"Target": "C:\\\\mssql","Source": "sqlVolume","Type": "volume"}],
            "PortBindings": {
              "1433/tcp": [{"HostPort": "1401"}]
            }
          }
        }
      }
      ```

   * Linux-Container:

      ```json
      "env": {
        "ACCEPT_EULA": {"value": "Y"},
        "SA_PASSWORD": {"value": "Strong!Passw0rd"}
      },
      "settings": {
        "image": "mcr.microsoft.com/mssql/server:latest",
        "createOptions": {
          "HostConfig": {
            "Mounts": [{"Target": "/var/opt/mssql","Source": "sqlVolume","Type": "volume"}],
            "PortBindings": {
              "1433/tcp": [{"HostPort": "1401"}]
            }
          }
        }
      }
      ```

   >[!Tip]
   >Bei jedem Erstellen eines SQL Server-Containers in einer Produktionsumgebung sollten Sie [das Standardkennwort für den Systemadministrator ändern](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

6. Speichern Sie die Datei **deployment.template.json**.

## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

In den vorherigen Abschnitten haben Sie eine Projektmappe mit einem Modul erstellt und anschließend ein weiteres Modul zur Bereitstellungsmanifestvorlage hinzugefügt. Nun müssen Sie die Projektmappe sowie Containerimages für die Module erstellen und die Images per Push an die Containerregistrierung übertragen. 

1. Melden Sie sich bei der Containerregistrierung in Visual Studio Code an, sodass Sie die Images per Push an die Registrierung übertragen können. Verwenden Sie die gleichen Anmeldeinformationen, die Sie der ENV-Datei hinzugefügt haben. Geben Sie den folgenden Befehl in das integrierte Terminal ein:

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    
    Sie werden aufgefordert, das Kennwort einzugeben. Fügen Sie Ihr Kennwort in die Eingabeaufforderung ein (das Kennwort wird aus Sicherheitsgründen ausgeblendet), und drücken Sie die **EINGABETASTE**. 

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie anschließend auf **Build and Push IoT Edge solution** (IoT Edge-Projektmappe erstellen und übertragen). 

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zunächst basierend auf den Informationen in der Bereitstellungsvorlage eine Datei vom Typ „deployment.json“ in einem neuen Ordner namens **config** erstellt. Anschließend werden zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen Ihren Code, packen das Modul in Container und pushen den Code anschließend in die Containerregistrierung, die Sie beim Initialisieren der Projektmappe angegeben haben. 

## <a name="deploy-the-solution-to-a-device"></a>Bereitstellen der Projektmappe auf einem Gerät

Sie können über IoT Hub Module auf einem Gerät festlegen, können jedoch auch über Visual Studio Code auf IoT Hub und Geräte zugreifen. In diesem Abschnitt richten Sie den Zugriff auf Ihre IoT Hub-Instanz ein und stellen dann mithilfe von VS Code Ihre Projektmappe auf dem IoT Edge-Gerät bereit. 

1. Wählen Sie in der Befehlspalette von VS Code **Azure IoT Hub: IoT Hub auswählen** aus.

2. Folgen Sie den Aufforderungen auf dem Bildschirm, um sich an Ihrem Azure-Konto anzumelden. 

3. Wählen Sie in der Befehlspalette Ihr Azure-Abonnement und dann Ihre IoT Hub-Instanz aus. 

4. Erweitern Sie im VS Code-Explorer den Abschnitt **Azure IoT Hub-Geräte**. 

5. Klicken Sie mit der rechten Maustaste auf das Gerät, das als Ziel für die Bereitstellung festgelegt werden soll, und klicken Sie auf **Create deployment for single device** (Bereitstellung für einzelnes Gerät erstellen). 

   ![Erstellen einer Bereitstellung für ein einzelnes Gerät](./media/tutorial-store-data-sql-server/create-deployment.png)

6. Navigieren Sie im Datei-Explorer zum Ordner **config** der Projektmappe, und klicken Sie auf **deployment.amd64**. Klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen). 

War die Bereitstellung erfolgreich, wird in VS Code eine Bestätigungsnachricht ausgegeben. 

Aktualisieren Sie den Status Ihres Geräts im Abschnitt „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte) von VS Code. Die neuen Module werden aufgeführt und in den nächsten Minuten als ausgeführt gemeldet, während die Container installiert und gestartet werden. Sie können auch überprüfen, ob alle Module auf dem Gerät ausgeführt werden. Führen Sie auf dem IoT Edge-Gerät den folgenden Befehl aus, um den Status der Module anzuzeigen. 

   ```cmd/sh
   iotedge list
   ```

## <a name="create-the-sql-database"></a>Erstellen der SQL-Datenbank

Wenn Sie das Bereitstellungsmanifest auf Ihr Gerät anwenden, werden drei Module ausgeführt. Das Modul „tempSensor“ generiert simulierte Umgebungsdaten. Das Modul „sqlFunction“ formatiert die Daten für eine Datenbank. Dieser Abschnitt führt Sie durch das Einrichten der SQL-Datenbank zum Speichern der Temperaturdaten. 

Führen Sie auf Ihrem IoT Edge-Gerät die folgenden Befehle aus. Diese Befehle stellen eine Verbindung mit dem **sql**-Modul her, das auf Ihrem Gerät ausgeführt wird, und erstellt eine Datenbank und eine Tabelle zum Speichern der gesendeten Temperaturdaten. 

1. Stellen Sie in einem Befehlszeilentool auf Ihrem IoT Edge-Gerät eine Verbindung mit Ihrer Datenbank her. 
   * Windows-Container:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Linux-Container: 

      ```bash
      sudo docker exec -it sql bash
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

   ![Inhalt der lokalen Datenbank anzeigen](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen. 

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Azure Functions-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie eigene Module erstellen möchten, finden Sie entsprechende Informationen unter [Use Visual Studio Code to develop and debug Azure Functions for Azure IoT Edge](how-to-develop-csharp-function.md) (Entwickeln und Debuggen von Azure Functions für Azure IoT Edge mithilfe von Visual Studio Code). 

Sie können mit den nächsten Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Filtern von Sensordaten mithilfe von C#-Code](tutorial-csharp-module.md)
