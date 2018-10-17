---
title: 'Schnellstart: Erstellen eines Blobs im Objektspeicher mithilfe von .NET – Azure Storage'
description: In dieser Schnellstartanleitung erfahren Sie, wie mithilfe der Azure Storage-Clientbibliothek für .NET ein Container und ein Blob in Blob Storage (Objekt) erstellt werden. Als Nächstes erfahren Sie, wie Sie den Blob auf Ihren lokalen Computer herunterladen, und wie Sie alle Blobs in einem Container auflisten.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 08/27/2018
ms.author: tamram
ms.openlocfilehash: cde8516b93c2aac0cb74d9c9599168c8e6a70472
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734132"
---
# <a name="quickstart-use-net-to-create-a-blob-in-object-storage"></a>Schnellstart: Erstellen eines Blobs im Objektspeicher mithilfe von .NET

In dieser Schnellstartanleitung erfahren Sie, wie mithilfe der Azure Storage-Clientbibliothek für .NET ein Container und ein Blob in Blob Storage (Objekt) erstellt werden. Als Nächstes erfahren Sie, wie Sie den Blob auf Ihren lokalen Computer herunterladen, und wie Sie alle Blobs in einem Container auflisten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Erstellen Sie zum Durchführen dieser Schnellstartanleitungen zuerst über das [Azure-Portal](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) ein Azure-Speicherkonto. Hilfe zur Erstellung des Kontos finden Sie unter [Erstellen eines Speicherkontos](../common/storage-quickstart-create-account.md).

Laden Sie als Nächstes .NET Core 2.0 für Ihr Betriebssystem herunter, und führen Sie die Installation durch. Beim Ausführen von Windows können Sie Visual Studio installieren und bei Bedarf .NET Framework verwenden. Sie können auch einen Editor für die Verwendung mit Ihrem Betriebssystem installieren.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

- Installieren Sie [.NET Core für Windows](https://www.microsoft.com/net/download/windows) oder das [.NET Framework](https://www.microsoft.com/net/download/windows) (in Visual Studio für Windows enthalten).
- Installieren Sie [Visual Studio für Windows](https://www.visualstudio.com/). Bei Verwendung von .NET Core ist die Installation von Visual Studio optional.  

Informationen zur Wahl zwischen .NET Core und .NET Framework finden Sie unter [Wahl zwischen .NET Core und .NET Framework für Server-Apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

- Installieren Sie [.NET Core für Linux](https://www.microsoft.com/net/download/linux).
- Optional können Sie [Visual Studio Code](https://www.visualstudio.com/) und die [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp&dotnetid=963890049.1518206068) installieren.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

- Installieren Sie [.NET Core für macOS](https://www.microsoft.com/net/download/macos).
- Optional können Sie [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) installieren.

---

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete Beispielanwendung ist eine einfache Konsolenanwendung. Sie können die Beispielanwendung auf [GitHub](https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart) erkunden.

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Suchen Sie zum Öffnen der Visual Studio-Projektmappe nach dem Ordner *storage-blobs-dotnet-quickstart*, öffnen Sie ihn, und doppelklicken Sie auf *storage-blobs-dotnet-quickstart.sln*. 

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Zum Ausführen der Anwendung müssen Sie die Verbindungszeichenfolge für das Speicherkonto angeben. Die Beispielanwendung liest die Verbindungszeichenfolge aus einer Umgebung und verwendet sie zum Autorisieren von Anforderungen an Azure Storage.

Schreiben Sie die Verbindungszeichenfolge nach dem Kopieren in eine neue Umgebungsvariable auf dem lokalen Computer, auf dem die Anwendung ausgeführt wird. Öffnen Sie zum Festlegen der Umgebungsvariablen ein Konsolenfenster, und befolgen Sie die Anleitung für Ihr Betriebssystem. Ersetzen Sie `<yourconnectionstring>` durch Ihre Verbindungszeichenfolge:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

Nachdem Sie die Umgebungsvariable hinzugefügt haben, müssen Sie unter Umständen alle ausgeführten Programme neu starten, von denen die Umgebungsvariable gelesen werden muss, z.B. das Konsolenfenster. Wenn Sie beispielsweise Visual Studio als Editor verwenden, müssen Sie Visual Studio neu starten, bevor Sie das Beispiel ausführen. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export storageconnectionstring=<yourconnectionstring>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Bearbeiten Sie Ihr „.bash_profile“, und fügen Sie die Umgebungsvariable hinzu:

```bash
export STORAGE_CONNECTION_STRING=<yourconnectionstring>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.

---

## <a name="run-the-sample"></a>Ausführen des Beispiels

In diesem Beispiel wird in Ihrem lokalen Ordner **MyDocuments** eine Testdatei erstellt und in Blobspeicher hochgeladen. Anschließend werden im Beispiel die Blobs im Container aufgelistet, und die Datei wird mit einem neuen Namen heruntergeladen, damit Sie die alte und neue Datei vergleichen können. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Wenn Sie Visual Studio als Editor verwenden, können Sie **F5** drücken, um die Ausführung zu starten. 

Navigieren Sie andernfalls zu Ihrem Anwendungsverzeichnis, und führen Sie die Anwendung mit dem Befehl `dotnet run` aus.

```
dotnet run
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Navigieren Sie zu Ihrem Anwendungsverzeichnis, und führen Sie die Anwendung mit dem Befehl `dotnet run` aus.

```
dotnet run
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Navigieren Sie zu Ihrem Anwendungsverzeichnis, und führen Sie die Anwendung mit dem Befehl `dotnet run` aus.

```
dotnet run
```

---

Die Ausgabe der Beispielanwendung ähnelt dem folgenden Beispiel:

```
Azure Blob storage - .NET Quickstart sample

Created container 'quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f'

Temp file = C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt
Uploading to Blob storage as blob 'QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt'

Listing blobs in container.
https://storagesamples.blob.core.windows.net/quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f/QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt

Downloading blob to C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db_DOWNLOADED.txt

Press any key to delete the sample files and example container.
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
// Studio, the shell or application needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");

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

- Laden Sie das NuGet-Paket für die aktuelle Version der [.NET-Clientbibliothek](https://www.nuget.org/packages/WindowsAzure.Storage/) für Azure Storage herunter. 
- Zeigen Sie den [Quellcode der .NET-Clientbibliothek](https://github.com/Azure/azure-storage-net) auf GitHub an.

### <a name="client-library-reference-and-samples"></a>Clientbibliothek – Referenz und Beispiele

- Weitere Informationen zur .NET-Clientbibliothek finden Sie in der [Referenz zur .NET-API](https://docs.microsoft.com/dotnet/api/overview/azure/storage).
- Erkunden Sie die [Beispiele für Blobspeicher](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob), die mit der .NET-Clientbibliothek geschrieben wurden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie Blobs per .NET hochladen, herunterladen und auflisten. 

Informationen zum Erstellen einer Web-App, mit der ein Bild in Blobspeicher hochgeladen wird, erhalten Sie, wenn Sie mit [Hochladen von Bilddaten in die Cloud mit Azure Storage](storage-upload-process-images.md) fortfahren.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](storage-dotnet-how-to-use-blobs.md)

- Weitere Informationen zu .NET Core finden Sie unter [Get started with .NET in 10 minutes](https://www.microsoft.com/net/learn/get-started/) (Einstieg in .NET in 10 Minuten).
- Eine Beispielanwendung, die Sie über Visual Studio für Windows bereitstellen können, finden Sie unter [.NET Photo Gallery Web Application Sample with Azure Blob Storage](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/) (Beispiel für .NET-Fotogalerie-Anwendung mit Azure Blob Storage).
 