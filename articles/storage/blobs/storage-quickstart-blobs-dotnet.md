---
title: "Azure-Schnellstart – Hochladen, Herunterladen und Auflisten von Blobs in Azure Storage mit .NET | Microsoft-Dokumentation"
description: "In diesem Schnellstart erstellen Sie ein Speicherkonto und einen Container. Anschließend verwenden Sie die Speicherclientbibliothek für .NET, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten."
services: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 03/01/2018
ms.author: tamram
ms.openlocfilehash: 8d1f09a39e865500aa8e4d093473d4989f134c3d
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-net"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit .NET

In diesem Schnellstart erfahren Sie, wie Sie die .NET-Clientbibliothek für Azure Storage zum Hochladen, Herunterladen und Auflisten von Blockblobs in einem Container verwenden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie zum Durchführen dieser Schnellstartanleitungen zuerst über das [Azure-Portal](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) ein Azure-Speicherkonto. Hilfe zur Erstellung des Kontos finden Sie unter [Erstellen eines Speicherkontos](../common/storage-quickstart-create-account.md).

Laden Sie als Nächstes .NET Core 2.0 für Ihr Betriebssystem herunter, und führen Sie die Installation durch. Sie können auch einen Editor für die Verwendung mit Ihrem Betriebssystem installieren.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

- Installieren Sie [.NET Core für Windows](https://www.microsoft.com/net/download/windows/build). 
- Optional können Sie [Visual Studio für Windows](https://www.visualstudio.com/) installieren. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

- Installieren Sie [.NET Core für Linux](https://www.microsoft.com/net/download/linux/build).
- Optional können Sie [Visual Studio Code](https://www.visualstudio.com/) und die [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp&dotnetid=963890049.1518206068) installieren.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

- Installieren Sie [.NET Core für macOS](https://www.microsoft.com/net/download/macos/build).
- Optional können Sie [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) installieren.

---

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete Beispielanwendung ist eine einfache Konsolenanwendung. Sie können die Beispielanwendung auf [GitHub](https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart) erkunden.

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Suchen Sie zum Öffnen der Visual Studio-Projektmappe nach dem Ordner „storage-blobs-dotnet-quickstart“ öffnen Sie ihn, und doppelklicken Sie auf „storage-blobs-dotnet-quickstart.sln“. 

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Zum Ausführen der Anwendung müssen Sie die Verbindungszeichenfolge für das Speicherkonto angeben. Sie können diese Verbindungszeichenfolge in einer Umgebungsvariablen auf dem lokalen Computer speichern, auf dem die Anwendung ausgeführt wird. Erstellen Sie die Umgebungsvariable, indem Sie je nach Betriebssystem einen der folgenden Befehle verwenden. Ersetzen Sie `<yourconnectionstring>` durch die tatsächliche Verbindungszeichenfolge.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export storageconnectionstring=<yourconnectionstring>
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Bearbeiten Sie Ihr „.bash_profile“, und fügen Sie die Umgebungsvariable hinzu:

```
export STORAGE_CONNECTION_STRING=
```

Melden Sie sich nach dem Hinzufügen der Umgebungsvariablen ab und dann wieder an, damit die Änderungen wirksam werden. Alternativ dazu können Sie im Terminal „source .bash_profile“ eingeben.

---

## <a name="run-the-sample"></a>Ausführen des Beispiels

In diesem Beispiel wird in Ihrem lokalen Ordner **MyDocuments** eine Testdatei erstellt und in Blobspeicher hochgeladen. Anschließend werden im Beispiel die Blobs im Container aufgelistet, und die Datei wird mit einem neuen Namen heruntergeladen, damit Sie die alte und neue Datei vergleichen können. 

Navigieren Sie zu Ihrem Anwendungsverzeichnis, und führen Sie die Anwendung mit dem Befehl `dotnet run` aus.

```
dotnet run
```

Die Ausgabe sieht in etwa wie im folgenden Beispiel aus:

```
Azure Blob storage quick start sample
Temp file = /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Uploading to Blob storage as blob 'QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt'
List blobs in container.
https://mystorageaccount.blob.core.windows.net/quickstartblobs/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Downloading blob to /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374_DOWNLOADED.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

Wenn Sie die **EINGABETASTE** drücken, löscht die Anwendung den Speichercontainer und die Dateien. Überprüfen Sie den Ordner **MyDocuments** vor dem Löschen auf die beiden Dateien. Sie können sie öffnen und sich vergewissern, dass sie identisch sind. Kopieren Sie die Blob-URL aus dem Konsolenfenster, und fügen Sie sie in einen Browser ein, um den Blobinhalt anzuzeigen.

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie eine beliebige Taste, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen das Beispiel ausführt, öffnen Sie die Datei „Program.cs“, um den Code zu betrachten. 

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Erkunden Sie als Nächstes den Beispielcode, um sich mit der Funktionsweise vertraut zu machen.

### <a name="try-parsing-the-connection-string"></a>Versuchtes Analysieren der Verbindungszeichenfolge

Im Beispiel wird als Erstes überprüft, ob die Umgebungsvariable eine Verbindungszeichenfolge enthält, die analysiert werden kann, um ein [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount)-Objekt zu erstellen, das auf das Speicherkonto verweist. Verwenden Sie die [TryParse](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.tryparse)-Methode, um zu überprüfen, ob die Verbindungszeichenfolge gültig ist. Wenn **TryParse** erfolgreich ist, wird die Variable *storageAccount* initialisiert, und **true** wird zurückgegeben.

```csharp
// Retrieve the connection string for use with the application. The storage connection string is stored
// in an environment variable on the machine running the application called storageconnectionstring.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring", EnvironmentVariableTarget.User);

// Check whether the connection string can be parsed.
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // If the connection string is valid, proceed with operations against Blob storage here.
    ...
}
else
{
    // Otherwise, let the user know that they need to define the environment variable.
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add a environment variable named 'storageconnectionstring' with your storage " +
        "connection string as a value.");
    Console.WriteLine("Press any key to exit the sample application.");
    Console.ReadLine();
}
```

### <a name="create-the-container-and-set-permissions"></a>Erstellen des Containers und Festlegen von Berechtigungen

Als Nächstes wird ein Container erstellt, und die Berechtigungen dafür werden so festgelegt, dass alle Blobs im Container öffentlich sind. Wenn ein Blob öffentlich ist, kann darauf über jeden Client anonym zugegriffen werden.

Erstellen Sie bei der Containererstellung zuerst eine Instanz des [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient)-Objekts, das auf Blobspeicher in Ihrem Speicherkonto verweist. Erstellen Sie als Nächstes eine Instanz des [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer)-Objekts und anschließend den Container. 

In diesem Fall wird im Beispiel die [CreateAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createasync)-Methode aufgerufen, um den Container zu erstellen. Ein GUID-Wert wird an den Containernamen angehängt, um sicherzustellen, dass er eindeutig ist. In einer Produktionsumgebung ist es häufig besser, die [CreateIfNotExistsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexistsasync)-Methode nur dann zum Erstellen eines Containers zu nutzen, wenn er nicht bereits vorhanden ist, um Namenskonflikte zu vermeiden.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zum Benennen von Containern und Blobs finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).


```csharp
// Create the CloudBlobClient that represents the Blob storage endpoint for the storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs' and append a GUID value to it to make the name unique. 
cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Als Nächstes wird im Beispiel eine lokale Datei in ein Blockblob hochgeladen. Im Codebeispiel wird ein Verweis auf ein **CloudBlockBlob**-Objekt abgerufen, indem die [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference)-Methode für den Container aufgerufen wird, der im vorherigen Abschnitt erstellt wurde. Anschließend wird die ausgewählte Datei in das Blob hochgeladen, indem die [UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync)-Methode aufgerufen wird. Mit dieser Methode wird das Blob erstellt, falls es nicht vorhanden ist, oder überschrieben, sofern es bereits vorhanden ist. 

```csharp
// Create a file in your local MyDocuments folder to upload to a blob.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

Console.WriteLine("Temp file = {0}", sourceFile);
Console.WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the blob address, then upload the file to the blob.
// Use the value of localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);
```

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Im Beispiel werden die Blobs im Container mit der [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync)-Methode aufgelistet. In diesem Beispiel wurde dem Container nur ein Blob hinzugefügt, sodass beim Auflisten auch nur ein Blob zurückgegeben wird.

Falls die Anzahl von zurückzugebenden Blobs für einen Aufruf zu hoch ist (standardmäßig > 5000), gibt die **ListBlobsSegmentedAsync**-Methode ein Segment des gesamten Resultsets und ein Fortsetzungstoken zurück. Zum Abrufen des nächsten Blobsegments geben Sie das vom vorherigen Aufruf zurückgegebene Fortsetzungstoken an und fahren damit fort, bis das Fortsetzungstoken null ist. Ein Fortsetzungstoken von „null“ gibt an, dass alle Blobs abgerufen wurden. Im Beispielcode wird angezeigt, wie Sie das Fortsetzungstoken im Rahmen der bewährten Methoden einsetzen.

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    // Get the value of the continuation token returned by the listing call.
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
    blobContinuationToken = results.ContinuationToken;
} while (blobContinuationToken != null); // Loop while the continuation token is not null. 

```

### <a name="download-blobs"></a>Herunterladen von Blobs

Als Nächstes wird im Beispiel das zuvor erstellte Blob mit der [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync)-Methode in Ihr lokales Dateisystem heruntergeladen. Im Beispielcode wird das Suffix „_DOWNLOADED“ an den Blobnamen angehängt, damit beide Dateien im lokalen Dateisystem angezeigt werden können.

```csharp
// Download the blob to a local file, using the reference created earlier. 
// Append the string "_DOWNLOADED" before the .txt extension so that you can see both files in MyDocuments.
destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);  
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Im Beispiel werden die erstellten Ressourcen bereinigt, indem der gesamte Container per [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync) gelöscht wird. Sie können bei Bedarf auch die lokalen Dateien löschen.

```csharp
Console.WriteLine("Press the 'Enter' key to delete the sample files, example container, and exit the application.");
Console.ReadLine();
// Clean up resources. This includes the container and the two temp files.
Console.WriteLine("Deleting the container");
if (cloudBlobContainer != null)
{
    await cloudBlobContainer.DeleteIfExistsAsync();
}
Console.WriteLine("Deleting the source, and downloaded files");
File.Delete(sourceFile);
File.Delete(destinationFile);
```

## <a name="resources-for-developing-net-applications-with-blobs"></a>Ressourcen für die Entwicklung von .NET-Anwendungen mit Blobs

Sehen Sie sich diese zusätzlichen Ressourcen zur .NET-Entwicklung mit Blobspeicher an:

### <a name="binaries-and-source-code"></a>Binärdateien und Quellcode

- Laden Sie das NuGet-Paket für die aktuelle Version der [Storage-.NET-Clientbibliothek](https://www.nuget.org/packages/WindowsAzure.Storage/) herunter. 
- Zeigen Sie den [Quellcode der Storage-.NET-Clientbibliothek](https://github.com/Azure/azure-storage-net) auf GitHub an.

### <a name="client-library-reference-and-samples"></a>Clientbibliothek – Referenz und Beispiele

- Weitere Informationen zur Clientbibliothek finden Sie in der [Referenz zur Storage-.NET-API](https://docs.microsoft.com/dotnet/api/overview/azure/storage).
- Erkunden Sie die [Beispiele für Blobspeicher](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob), die mit der Storage-.NET-Clientbibliothek geschrieben wurden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie Blobs per .NET hochladen, herunterladen und auflisten. 

Informationen zum Erstellen einer Web-App, mit der ein Bild in Blobspeicher hochgeladen wird, erhalten Sie, wenn Sie mit [Hochladen von Bilddaten in die Cloud mit Azure Storage](storage-upload-process-images.md) fortfahren.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](storage-dotnet-how-to-use-blobs.md)

- Weitere Informationen zu .NET Core finden Sie unter [Get started with .NET in 10 minutes](https://www.microsoft.com/net/learn/get-started/) (Einstieg in .NET in 10 Minuten).
- Eine Beispielanwendung, die Sie über Visual Studio für Windows bereitstellen können, finden Sie unter [.NET Photo Gallery Web Application Sample with Azure Blob Storage](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/) (Beispiel für .NET-Fotogalerie-Anwendung mit Azure Blob Storage).
 