---
title: "Azure-Schnellstart – Übertragen von Objekten nach/aus Azure Blob Storage mit .NET | Microsoft-Dokumentation"
description: "Hier lernen Sie schnell, wie Sie mit .NET Objekte nach/aus Azure Blob Storage übertragen."
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/01/2017
ms.author: robinsh
ms.openlocfilehash: fdba4588fbb2c46efb3fc4de1a9e53414264444a
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Übertragen von Objekten nach/aus Azure Blob Storage mit .NET

In diesem Schnellstart erfahren Sie, wie Sie mit C#.NET Blockblobs in einem Container in Azure Blob Storage unter Windows hochladen, herunterladen und auflisten.

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) mit der folgenden Workload:
    - **Azure-Entwicklung**

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-storage-account-using-the-azure-portal"></a>Erstellen eines Speicherkontos mit dem Azure-Portal

Erstellen Sie zunächst ein neues allgemeines Speicherkonto zur Verwendung für diesen Schnellstart. 

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an. 
2. Wählen Sie im Menü „Hub“ die Option **Neu** > **Speicher** > **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus. 
3. Geben Sie einen Namen für Ihr Speicherkonto ein. Der Name muss zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten. Er muss außerdem eindeutig sein.
4. Legen Sie `Deployment model` auf **Ressourcen-Manager** fest.
5. Legen Sie `Account kind` auf **Allgemeiner Zweck** fest.
6. Legen Sie `Performance` auf **Standard** fest. 
7. Legen Sie `Replication` auf **Lokal redundanter Speicher (LRS)** fest.
8. Legen Sie `Storage service encryption` auf **Deaktiviert** fest.
9. Legen Sie `Secure transfer required` auf **Deaktiviert** fest.
10. Wählen Sie Ihr Abonnement aus. 
11. Erstellen Sie für `resource group` eine neue Ressourcengruppe, und geben sie ihr einen eindeutigen Namen. 
12. Wählen Sie unter `Location` den Standort aus, der für das Speicherkonto verwendet werden soll.
13. Aktivieren Sie **An Dashboard anheften**, und klicken Sie auf **Erstellen**, um das Speicherkonto zu erstellen. 

Nachdem das Speicherkonto erstellt wurde, wird es an das Dashboard angeheftet. Klicken Sie darauf, um es zu öffnen. Klicken Sie unter „EINSTELLUNGEN“ auf **Zugriffsschlüssel**. Wählen Sie einen Schlüssel aus, kopieren Sie die VERBINDUNGSZEICHENFOLGE in die Zwischenablage, und fügen Sie sie für die spätere Verwendung in einen Text-Editor ein.

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete Beispielanwendung ist eine einfache Konsolenanwendung. 

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Suchen Sie zum Öffnen der Visual Studio-Projektmappe nach dem Ordner „storage-blobs-dotnet-quickstart“ öffnen Sie ihn, und doppelklicken Sie auf „storage-blobs-dotnet-quickstart.sln“. 

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Sie müssen in der Anwendung die Verbindungszeichenfolge für das Speicherkonto angeben. Öffnen Sie im Projektmappen-Explorer von Visual Studio die Datei `app.config`. Suchen Sie den Eintrag `StorageConnectionString`. Ersetzen Sie bei **value** den gesamten Wert der Verbindungszeichenfolge durch die Verbindungszeichenfolge aus dem Azure-Portal, die Sie gespeichert haben. Ihre `storageConnectionString` sollte etwa folgendermaßen aussehen:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;
    AccountName=<NameHere>;
    AccountKey=<KeyHere>" />
  </appSettings>
</configuration>
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Mit diesem Beispiel wird eine Testdatei in „Dokumente“ erstellt, diese wird in Blob Storage hochgeladen, die Blobs im Container werden aufgelistet, und dann wird die Datei mit einem neuen Namen heruntergeladen, damit Sie die alte und neue Datei vergleichen können. 

Drücken Sie F5, um das Beispiel auszuführen. In einem Konsolenfenster wird etwa die folgende Ausgabe angezeigt: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Wenn Sie eine beliebige Taste drücken, um den Vorgang fortzusetzen, werden der Speichercontainer und die Dateien gelöscht. Überprüfen Sie im Ordner „Eigene Dateien“, ob die beiden Dateien vorhanden sind, bevor Sie fortfahren. Sie können diese öffnen und prüfen, ob sie identisch sind. Kopieren Sie die URL für das Blob aus dem Konsolenfenster, und fügen Sie sie in einem Browser ein, um den Inhalt der Datei in Blob Storage anzuzeigen.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie eine beliebige Taste, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen das Beispiel ausführt, öffnen Sie die Datei „Program.cs“, um den Code zu betrachten. 

## <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte

Zunächst müssen die Verweise auf die Objekte erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf – jedes wird vom jeweils nächsten in der Liste verwendet.

* Erstellen Sie eine Instanz des **CloudStorageAccount**-Objekts, das auf das Speicherkonto verweist. 

* Erstellen Sie eine Instanz des **CloudBlobClient**-Objekts, das auf den Blob-Dienst im Speicherkonto verweist. 

* Erstellen Sie eine Instanz des **CloudBlobContainer**-Objekts, das den Container darstellt, auf den Sie zugreifen. Container werden zum Organisieren der Blobs verwendet, so wie Sie auf Ihrem Computer Ordner zum Organisieren von Dateien verwenden.

Sobald Sie über den **CloudBlobContainer** verfügen, können Sie eine Instanz des **CloudBlockBlob**-Objekts erstellen, das auf das gewünschte Blob verweist, und einen Upload-, Download- oder Kopiervorgang sowie weitere Vorgänge ausführen.

In diesem Abschnitt erstellen Sie eine Instanz der Objekte und einen neuen Container und legen dann Berechtigungen für den Container fest, damit die Blobs öffentlich sind und einfach mit einer URL auf sie zugegriffen werden kann. Der Name des Containers lautet **quickstartblobs**. 

In diesem Beispiel wird **CreateIfNotExists** verwendet, da bei jedem Ausführen des Beispiels ein neuer Container erstellt werden soll. In einer Produktionsumgebung, in der Sie in der gesamten Anwendung den gleichen Container verwenden, empfiehlt es sich hingegen, **CreateIfNotExists** nur einmal aufzurufen. Alternativ können Sie den Container im Voraus erstellen, damit er nicht im Code erstellt werden muss.

```csharp
// Create a CloudStorageAccount instance pointing to your storage account.
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs");
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions();
permissions.PublicAccess = BlobContainerPublicAccessType.Blob;
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

## <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blob Storage unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten verwendet und auch in diesem Schnellstart verwendet. 

Um eine Datei in ein Blob hochzuladen, rufen Sie einen Verweis auf das Blob im Zielcontainer ab. Nachdem Sie den Blobverweis abgerufen haben, können Sie mithilfe von [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync) Daten in das Blockblob hochladen. Wenn noch kein Blob vorhanden ist, wird durch diesen Vorgang eines erstellt. Andernfalls wird das vorhandene Blob überschrieben.

Der Beispielcode erstellt eine lokale Datei für den Upload und Download. Dabei wird die hochzuladende Datei als **fileAndPath** und der Name des Blob in **localFileName** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartblobs** hochgeladen.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string fileAndPath = Path.Combine(localPath, localFileName);
File.WriteAllText(fileAndPath, "Hello, World!");

//Upload the file.
CloudBlockBlob blockBlob = container.GetBlockBlobReference(localFileName);
await blockBlob.UploadFromFileAsync(fileAndPath);
```

Sie können für Blob Storage mehrere Uploadmethoden verwenden. Wenn Sie z.B. über einen Speicherdatenstrom verfügen, können Sie statt „UploadFromFileAsync“ die Methode „UploadFromStreamAsync“ verwenden. 

Blockblobs können jede Art von Text oder Binärdateien darstellen. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Mit [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync) können Sie eine Liste von Dateien im Container abrufen. Der folgende Code ruft die Liste der Blobs ab und durchläuft sie, um die URIs der gefundenen Blobs anzuzeigen. Sie können den URI aus dem Befehlsfenster kopieren und in einen Browser einfügen, um die Datei anzuzeigen.

Bei 5.000 oder weniger Blobs im Container werden in einem einzigen Aufruf von „ListBlobsSegmentedAsync“ alle Blobnamen abgerufen. Wenn der Container mehr als 5.000 Blobs enthält, wird die Liste in Gruppen von 5.000 Blobnamen abgerufen, bis alle Blobnamen abgerufen wurden. Somit werden beim ersten Aufruf dieser API die ersten 5.000 Blobnamen und ein Fortsetzungstoken zurückgegeben. Beim zweiten Aufruf geben Sie das Token an, und die nächste Gruppe von Blobnamen wird abgerufen usw., bis das Fortsetzungstoken NULL ist, was bedeutet, dass alle Blobnamen abgerufen wurden. 

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    foreach(IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null);
```

## <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie mit [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync) Blobs auf den lokalen Datenträger herunter.

Der folgende Code lädt das im vorherigen Abschnitt hochgeladene Blob herunter und fügt dem Blobnamen das Suffix „_DOWNLOADED“ hinzu, damit auf dem lokalen Datenträger beide Dateien angezeigt werden. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync) den gesamten Container löschen. Löschen Sie auch die erstellten Dateien, wenn sie nicht mehr benötigt werden.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie mit .NET Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](storage-dotnet-how-to-use-blobs.md)

Weitere Informationen zum Storage-Explorer und Blobs finden Sie unter [Verwalten von Azure Blob Storage-Ressourcen mit dem Speicher-Explorer (Vorschau)](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
