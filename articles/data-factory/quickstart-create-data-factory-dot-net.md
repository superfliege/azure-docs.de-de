---
title: Erstellen einer Azure Data Factory mithilfe von .NET | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory, um Daten von einem Speicherort in einem Azure Blob Storage in einen anderen Speicherort im selben Blob Storage zu kopieren.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: b430f5932bcd54d5e2ab787fb6c4491b48dbc416
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="create-a-data-factory-and-pipeline-using-net-sdk"></a>Erstellen einer Data Factory und Pipeline mit dem .NET SDK
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](quickstart-create-data-factory-dot-net.md)

Diese Schnellstartanleitung beschreibt, wie Sie das .NET SDK verwenden, um eine Azure Data Factory zu erstellen. Die in dieser Data Factory erstellte Pipeline **kopiert** Daten aus einem Ordner in einen anderen Ordner in einem Azure-Blobspeicher. Ein Tutorial zum **Transformieren** von Daten mithilfe von Azure Data Factory finden Sie im Tutorial [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](transform-data-using-spark.md). 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
>
> Dieser Artikel enthält keine ausführliche Einführung in den Data Factory-Dienst. Eine Einführung in den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](introduction.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-roles"></a>Azure-Rollen
Damit Sie Data Factory-Instanzen erstellen können, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, ein Mitglied der Rolle **Mitwirkender** oder **Besitzer** oder ein **Administrator** des Azure-Abonnements sein. Klicken Sie im Azure-Portal in der oberen rechten Ecke auf Ihren **Benutzernamen** und auf **Berechtigungen**, um Ihre Berechtigungen im Abonnement anzuzeigen. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. Beispielanweisungen zum Hinzufügen eines Benutzers zu einer Rolle finden Sie im Artikel [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="azure-storage-account"></a>Azure Storage-Konto
Sie verwenden in diesem Schnellstart ein Azure Storage-Allzweckkonto (Blob Storage) als Datenspeicher vom Typ **Quelle** und vom Typ **Ziel**. Falls Sie noch nicht über ein Azure Storage-Allzweckkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) weiter. 

#### <a name="get-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Schnellstart verwenden Sie Name und Schlüssel Ihres Azure-Speicherkontos. Das folgende Verfahren enthält die Schritte zum Abrufen des Namens und Schlüssels für Ihr Speicherkonto. 

1. Starten Sie einen Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com). Melden Sie sich mit Ihrem Azure-Benutzernamen und -Kennwort an. 
2. Klicken Sie im Menü auf der linken Seite auf **Weitere Dienste >**, filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie **Speicherkonten**.

    ![Suchen nach einem Speicherkonto](media/quickstart-create-data-factory-dot-net/search-storage-account.png)
3. Filtern Sie in der Liste mit den Speicherkonten nach Ihrem Speicherkonto (falls erforderlich), und wählen Sie **Ihr Speicherkonto** aus. 
4. Wählen Sie auf der Seite **Speicherkonto** im Menü die Option **Zugriffsschlüssel**.

    ![Abrufen des Speicherkontonamens und -schlüssels](media/quickstart-create-data-factory-dot-net/storage-account-name-key.png)
5. Kopieren Sie die Werte für die Felder **Speicherkontoname** und **key1** in die Zwischenablage. Fügen Sie sie in einen Editor ein, und speichern Sie sie.  

#### <a name="create-input-folder-and-files"></a>Erstellen des Eingabeordners und der Dateien
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen **adftutorial** in Ihrem Azure-Blobspeicher. Anschließend erstellen Sie den Ordner **Eingabe** im Container und laden eine Beispieldatei in den Eingabeordner hoch. 

1. Wechseln Sie auf der Seite **Speicherkonto** zu **Übersicht**, und klicken Sie dann auf **Blobs**. 

    ![Option zum Auswählen von Blobs](media/quickstart-create-data-factory-dot-net/select-blobs.png)
2. Klicken Sie auf der Seite **Blob-Dienst** auf der Symbolleiste auf **+ Container**. 

    ![Schaltfläche „Container hinzufügen“](media/quickstart-create-data-factory-dot-net/add-container-button.png)    
3. Geben Sie im Dialogfeld **Neuer Container** als Namen **adftutorial** ein, und klicken Sie auf **OK**. 

    ![Eingeben des Containernamens](media/quickstart-create-data-factory-dot-net/new-container-dialog.png)
4. Klicken Sie in der Liste der Container auf **adftutorial**. 

    ![Auswählen des Containers](media/quickstart-create-data-factory-dot-net/select-adftutorial-container.png)
1. Klicken Sie auf der Seite **Container** auf der Symbolleiste auf **Hochladen**.  

    ![Schaltfläche zum Hochladen](media/quickstart-create-data-factory-dot-net/upload-toolbar-button.png)
6. Klicken Sie auf der Seite **Blob hochladen** auf **Erweitert**.

    ![Klicken auf den Link „Erweitert“](media/quickstart-create-data-factory-dot-net/upload-blob-advanced.png)
7. Starten Sie **Editor**, und erstellen Sie eine Datei mit dem Namen **emp.txt** und folgendem Inhalt: Speichern Sie sie im Ordner **c:\ADFv2QuickStartPSH**: Erstellen Sie den Ordner **ADFv2QuickStartPSH**, sofern er noch nicht vorhanden ist.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. Wählen Sie im Azure-Portal auf der Seite **Blob hochladen** im Feld **Dateien** die Datei **emp.txt** aus. 
9. Geben Sie für das Feld **In Ordner hochladen** den Wert **Eingabe** ein. 

    ![Einstellungen für das Hochladen von Blobs](media/quickstart-create-data-factory-dot-net/upload-blob-settings.png)    
10. Vergewissern Sie sich, dass als Ordner **Eingabe** und als Datei **emp.txt** ausgewählt ist, und klicken Sie auf **Hochladen**.
11. Daraufhin sollten in der Liste die Datei **emp.txt** und der Status des Uploads angezeigt werden. 
12. Schließen Sie die Seite **Blob hochladen**, indem Sie in der Ecke auf **X** klicken. 

    ![Schließen der Seite „Blob hochladen“](media/quickstart-create-data-factory-dot-net/close-upload-blob.png)
1. Lassen Sie die Seite **Container** geöffnet. Sie überprüfen darauf am Ende dieser Schnellstartanleitung die Ausgabe.

### <a name="visual-studio"></a>Visual Studio
In der exemplarischen Vorgehensweise in diesem Artikel wird Visual Studio 2017 verwendet. Sie können auch Visual Studio 2013 oder 2015 verwenden.

### <a name="azure-net-sdk"></a>Azure .NET SDK
Laden Sie das [Azure .NET SDK](http://azure.microsoft.com/downloads/) auf Ihren Computer herunter, und installieren Sie es.

### <a name="create-an-application-in-azure-active-directory"></a>Erstellen einer Anwendung in Azure Active Directory
Befolgen Sie zum Ausführen der folgenden Aufgaben die Anweisungen in [diesem Artikel](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application): 

1. **Erstellen Sie eine Azure Active Directory-Anwendung.** Erstellen Sie eine Anwendung in Azure Active Directory, die die .NET-Anwendung darstellt, die Sie in diesem Tutorial erstellen. Als Anmelde-URL können Sie wie in diesem Artikel gezeigt eine Platzhalter-URL (`https://contoso.org/exampleapp`) angeben.
2. Rufen Sie die **Anwendungs-ID** und den **Authentifizierungsschlüssel**** anhand der Anweisungen im Abschnitt **Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels** im genannten Artikel ab. Notieren Sie sich diese Werte, da Sie sie später in diesem Tutorial benötigen. 
3. Rufen Sie die **Mandanten-ID** anhand der Anweisungen im Abschnitt **Abrufen der Mandanten-ID** im genannten Artikel ab. Notieren Sie sich diesen Wert. 
4. Weisen Sie die Anwendung der Rolle **Mitwirkender** auf Abonnementebene zu, damit die Anwendung Data Factorys im Abonnement erstellen kann. Befolgen Sie die Anweisungen im Abschnitt **Zuweisen einer Anwendung zur Rolle** im genannten Artikel. 

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

Erstellen Sie mithilfe von Visual Studio 2013/2015/2017 eine C# .NET-Konsolenanwendung.

1. Starten Sie **Visual Studio**.
2. Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**.
3. Wählen Sie in der Liste der Projekttypen auf der rechten Seite **Visual C#** -> **Konsolen-App (.NET Framework)** aus. .NET-Version 4.5.2 oder höher ist erforderlich.
4. Geben Sie **ADFv2QuickStart** als Namen ein.
5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

1. Klicken Sie auf **Tools** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.
2. Führen Sie in der **Paket-Manager-Konsole**, um die Pakete zu installieren die folgenden Befehle aus:

    ```
    Install-Package Microsoft.Azure.Management.DataFactory -Prerelease
    Install-Package Microsoft.Azure.Management.ResourceManager -Prerelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

    ```

## <a name="create-a-data-factory-client"></a>Erstellen eines Data Factory-Clients

1. Öffnen Sie **Program.cs**, und fügen Sie die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der die Variablen festlegt. Ersetzen Sie die Platzhalter durch Ihre eigenen Werte. Derzeit können Sie mit Data Factory V2 nur in den Regionen „USA, Osten“, „USA, Osten 2“ und „Europa, Westen“ Data Factorys erstellen. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

    ```csharp
    // Set variables
    string tenantID = "<your tenant ID>";
    string applicationId = "<your application ID>";
    string authenticationKey = "<your authentication key for the application>";
    string subscriptionId = "<your subscription ID where the data factory resides>";
    string resourceGroup = "<your resource group where the data factory resides>";
    string region = "East US 2";
    string dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>";
    string storageAccount = "<your storage account name to copy data>";
    string storageKey = "<your storage account key>";
    // specify the container and input folder from which all files need to be copied to the output folder. 
    string inputBlobPath = "<the path to existing blob(s) to copy data from, e.g. containername/foldername>";
    //specify the contains and output folder where the files are copied
    string outputBlobPath = "<the blob path to copy data to, e.g. containername/foldername>";

    string storageLinkedServiceName = "AzureStorageLinkedService";  // name of the Azure Storage linked service
    string blobDatasetName = "BlobDataset";             // name of the blob dataset
    string pipelineName = "Adfv2QuickStartPipeline";    // name of the pipeline
    ```

3. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine Instanz der **DataFactoryManagementClient**-Klasse erstellt. Sie verwenden dieses Objekt, um eine Data Factory, einen verknüpften Dienst, Datasets und eine Pipeline zu erstellen. Sie verwenden dieses Objekt ebenfalls zum Überwachen der Ausführungsdetails der Pipeline.

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
    ```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine **Data Factory** erstellt. 

```csharp
// Create a data factory
Console.WriteLine("Creating data factory " + dataFactoryName + "...");
Factory dataFactory = new Factory
{
    Location = region,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
Console.WriteLine(SafeJsonConvert.SerializeObject(dataFactory, client.SerializationSettings));

while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
{
    System.Threading.Thread.Sleep(1000);
}
```

## <a name="create-a-linked-service"></a>Erstellen eines verknüpften Diensts

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der einen **verknüpften Azure Storage-Dienst** erstellt.

Um Ihre Datenspeicher und Compute Services mit der Data Factory zu verknüpfen, können Sie verknüpfte Dienste in einer Data Factory erstellen. In dieser Schnellstartanleitung müssen Sie nur einen verknüpften Azure Storage-Dienst für die Quell- und Senkendatenspeicher für den Kopiervorgang erstellen – in diesem Beispiel als „AzureStorageLinkedService“ bezeichnet.

```csharp
// Create an Azure Storage linked service
Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");

LinkedServiceResource storageLinkedService = new LinkedServiceResource(
    new AzureStorageLinkedService
    {
        ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
    }
);
client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, storageLinkedService);
Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
```

## <a name="create-a-dataset"></a>Erstellen eines Datasets

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der ein **Azure-Blobdataset** erstellt.

Sie definieren ein Dataset, das die Daten repräsentiert, die aus einer Quelle in eine Senke kopiert werden sollen. In diesem Beispiel verweist dieses Blobdataset auf den verknüpften Azure Storage-Dienst, den Sie im vorherigen Schritt erstellt haben. Das Dataset akzeptiert einen Parameter, dessen Wert in einer Aktivität festgelegt ist, die das Dataset verwendet. Der Parameter wird verwendet, um den „folderPath“ zu erstellen, der auf den Speicherort der Dateien zeigt.

```csharp
// Create a Azure Blob dataset
Console.WriteLine("Creating dataset " + blobDatasetName + "...");
DatasetResource blobDataset = new DatasetResource(
    new AzureBlobDataset
    {
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        },
        FolderPath = new Expression { Value = "@{dataset().path}" },
        Parameters = new Dictionary<string, ParameterSpecification>
        {
            { "path", new ParameterSpecification { Type = ParameterType.String } }

        }
    }
);
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobDatasetName, blobDataset);
Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine **Pipeline mit einer Kopieraktivität** erstellt.

In diesem Beispiel enthält die Pipeline eine Aktivität und akzeptiert zwei Parameter: Eingabeblobpfad und Ausgabeblobpfad. Die Werte für diese Parameter werden festgelegt, wenn die Pipeline ausgelöst bzw. ausgeführt wird. Die Kopieraktivität verweist für die Eingabe und Ausgabe auf das gleiche Blobdataset, das im vorherigen Schritt erstellt wurde. Wenn das Dataset als Eingabedataset verwendet wird, wird der Eingabepfad angegeben. Wenn das Dataset als Ausgabedataset verwendet wird, wird der Ausgabepfad angegeben. 

```csharp
// Create a pipeline with a copy activity
Console.WriteLine("Creating pipeline " + pipelineName + "...");
PipelineResource pipeline = new PipelineResource
{
    Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "inputPath", new ParameterSpecification { Type = ParameterType.String } },
        { "outputPath", new ParameterSpecification { Type = ParameterType.String } }
    },
    Activities = new List<Activity>
    {
        new CopyActivity
        {
            Name = "CopyFromBlobToBlob",
            Inputs = new List<DatasetReference>
            {
                new DatasetReference()
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.inputPath" }
                    }
                }
            },
            Outputs = new List<DatasetReference>
            {
                new DatasetReference
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.outputPath" }
                    }
                }
            },
            Source = new BlobSource { },
            Sink = new BlobSink { }
        }
    }
};
client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, pipeline);
Console.WriteLine(SafeJsonConvert.SerializeObject(pipeline, client.SerializationSettings));
```

## <a name="create-a-pipeline-run"></a>Erstellen einer Pipelineausführung

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der **eine Pipelineausführung auslöst**.

Dieser Code legt auch die in der Pipeline angegeben Parameter **inputPath** und **outputPath** auf die tatsächlichen Werte der Quell- und Senkenblobpfade fest.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> parameters = new Dictionary<string, object>
{
    { "inputPath", inputBlobPath },
    { "outputPath", outputBlobPath }
};
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>Überwachen einer Pipelineausführung

1. Fügen Sie der **Main**-Methode den folgenden Code hinzu, um den Status kontinuierlich zu überwachen, bis die Pipeline das Kopieren der Daten beendet hat.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der Ausführungsdetails zur Kopieraktivität abruft, z.B. die Größe der gelesenen/geschriebenen Daten.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Ausführen des Codes

Erstellen und starten Sie die Anwendung, und überprüfen Sie dann die Pipelineausführung.

Die Konsole druckt den Status der Erstellung der Data Factory, des verknüpften Diensts, der Datasets, der Pipeline und der Pipelineausführung aus. Danach wird der Status der Pipelineausführung überprüft. Warten Sie, bis Sie die Ausführungsdetails der Kopieraktivität mit der Größe der gelesenen/geschriebenen Daten sehen. Verwenden Sie dann Tools wie z.B. [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), um zu überprüfen, ob die Blobs wie von Ihnen in den Variablen angegeben von „inputBlobPath“ nach „outputBlobPath“ kopiert werden.

### <a name="sample-output"></a>Beispielausgabe: 
```json
Creating data factory SPv2Factory0907...
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>",
        "type": "SecureString"
      }
    }
  }
}
Creating dataset BlobDataset...
{
  "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@{dataset().path}",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "AzureStorageLinkedService",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "path": {
        "type": "String"
      }
    }
  }
}
Creating pipeline Adfv2QuickStartPipeline...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "inputs": [
          {
            "referenceName": "BlobDataset",
            "parameters": {
              "path": "@pipeline().parameters.inputPath"
            },
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "BlobDataset",
            "parameters": {
              "path": "@pipeline().parameters.outputPath"
            },
            "type": "DatasetReference"
          }
        ],
        "name": "CopyFromBlobToBlob"
      }
    ],
    "parameters": {
      "inputPath": {
        "type": "String"
      },
      "outputPath": {
        "type": "String"
      }
    }
  }
}
Creating pipeline run...
Pipeline run ID: 308d222d-3858-48b1-9e66-acd921feaa09
Checking pipeline run status...
Status: InProgress
Status: InProgress
Checking copy activity run details...
{
    "dataRead": 331452208,
    "dataWritten": 331452208,
    "copyDuration": 23,
    "throughput": 14073.209,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 2,
    "billedDuration": 23
}

Press any key to exit...
```

## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Die Pipeline erstellt den Ausgabeordner automatisch im Blobcontainer „adftutorial“. Anschließend wird die Datei „emp.txt“ aus dem Eingabe- in den Ausgabeordner kopiert. 

1. Klicken Sie im Azure-Portal auf der Seite des Containers **adftutorial** auf **Aktualisieren**, um den Ausgabeordner anzuzeigen. 
    
    ![Aktualisieren](media/quickstart-create-data-factory-dot-net/output-refresh.png)
2. Klicken Sie in der Ordnerliste auf **Ausgabe**. 
2. Überprüfen Sie, ob die Datei **emp.txt** in den Ausgabeordner kopiert wurde. 

    ![Aktualisieren](media/quickstart-create-data-factory-dot-net/output-file.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Um die Data Factory programmgesteuert zu löschen, fügen Sie die folgenden Codezeilen zum Programm hinzu: 

```csharp
            Console.WriteLine("Deleting the data factory");
            client.Factories.Delete(resourceGroup, dataFactoryName);
```

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-dot-net.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 
