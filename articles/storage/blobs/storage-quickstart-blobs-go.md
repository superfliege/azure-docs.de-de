---
title: "Azure-Schnellstart – Übertragen von Objekten nach/aus Azure Blob Storage mit Go | Microsoft-Dokumentation"
description: "Hier lernen Sie schnell, wie Sie mit der Programmiersprache „Go“ Objekte nach/aus Azure Blob Storage übertragen."
services: storage
author: seguler
manager: jahogg
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: go
ms.topic: quickstart
ms.date: 01/29/2018
ms.author: seguler
ms.openlocfilehash: 4ba9721dc12bc50b20ad85019b1df51a56b52ebc
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-go"></a>Übertragen von Objekten nach/aus Azure Blob Storage mit Go
In diesem Schnellstart erfahren Sie, wie Sie mit der Programmiersprache „Go“ Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten. 

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch: 
* Installieren von [Go 1.8 oder höher](https://golang.org/dl/)
* Laden Sie mithilfe von `go get -u github.com/azure/azure-storage-blob-go/2016-05-31/azblob` das [Azure Storage Blob-SDK für Go](https://github.com/azure/azure-storage-blob-go/) herunter, und installieren Sie es. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung
Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-go-quickstart.git) ist eine einfache Go-Anwendung.  

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-go-quickstart 
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Zum Öffnen des Go-Beispiels für Blob Storage suchen Sie nach der Datei „storage-quickstart.go“.  

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
Für diese Lösung müssen der Name und der Schlüssel Ihres Speicherkontos sicher in Umgebungsvariablen gespeichert sein, die sich auf dem Computer befinden, auf dem das Beispiel ausgeführt wird. Befolgen Sie je nach Betriebssystem die Schritte für eines der unten angegebenen Beispiele, um die Umgebungsvariablen zu erstellen.

# <a name="linuxtablinux"></a>[Linux](#tab/Linux)

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

# <a name="windowstabwindows"></a>[Windows](#tab/Windows)

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-sample"></a>Ausführen des Beispiels
Dieses Beispiel erstellt eine Testdatei im aktuellen Ordner, lädt die Testdatei in den Blobspeicher hoch, listet die Blobs im Container auf und lädt die Datei in einen Puffer herunter. 

Geben Sie zum Ausführen des Beispiels den folgenden Befehl aus: 

```go run storage-quickstart.go```

Die folgende Ausgabe ist ein Beispiel der Ausgabe, die zurückgegeben wird, wenn die Anwendung ausgeführt wird.
  
```
Azure Blob storage quick start sample
Creating a container named quickstart-5568059279520899415
Creating a dummy file to test the upload and download
Uploading the file with blob name: 630910657703031215
Blob name: 630910657703031215
Downloaded the blob: hello world
this is a blob
Press the enter key to delete the sample files, example container, and exit the application.
```
Wenn Sie die Taste zum Fortzusetzen drücken, löscht das Beispielprogramm den Speichercontainer und die Dateien. 

> [!TIP]
> Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](http://storageexplorer.com), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 
>

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Als Nächstes gehen wir schrittweise durch den Beispielcode, damit Sie verstehen können, wie er funktioniert.

### <a name="create-containerurl-and-bloburl-objects"></a>Erstellen von ContainerURL- und BlobURL-Objekten
Zunächst müssen die Verweise auf die Objekte „ContainerURL“ und „ContainerURL“ erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bieten Low-Level-APIs, z.B. Create-, PutBlob- und GetBlob-Vorgänge zum Ausgeben von REST-APIs.

* Verwenden Sie die Struktur **SharedKeyCredential** zum Speichern Ihrer Anmeldeinformationen. 

* Erstellen Sie mithilfe der Optionen und Anmeldeinformationen eine **Pipeline**. Mit der Pipeline werden u.a. Wiederholungsrichtlinien, Protokollierung und Deserialisierung von HTTP-Antwortnutzlasten festgelegt.  

* Instanziieren Sie ein neues ContainerURL- und ein neues BlobURL-Objekt, um Vorgänge für Container (Create) und Blobs (PutBlob und GetBlob) auszuführen.


Sobald das ContainerURL-Objekt vorhanden ist, können Sie das **BlobURL**-Objekt instanziieren, das auf einen Blob verweist, und Vorgänge wie Hochladen, Herunterladen und Kopieren ausführen.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Benennen von Containern, Blobs und Metadaten und Verweisen auf diese](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

In diesem Abschnitt erstellen Sie einen neuen Container. Der Name des Containers lautet**quickstartblobs-[zufällige Zeichenfolge]**. 

```go 
// From the Azure portal, get your storage account name and key and set environment variables.
accountName, accountKey := os.Getenv("AZURE_STORAGE_ACCOUNT"), os.Getenv("AZURE_STORAGE_ACCESS_KEY")
if len(accountName) == 0 || len(accountKey) == 0 {
    log.Fatal("Either the AZURE_STORAGE_ACCOUNT or AZURE_STORAGE_ACCESS_KEY environment variable is not set")
}

// Create a default request pipeline using your storage account name and account key.
credential := azblob.NewSharedKeyCredential(accountName, accountKey)
p := azblob.NewPipeline(credential, azblob.PipelineOptions{})

// Create a random string for the quick start container
containerName := fmt.Sprintf("quickstart-%s", randomString())

// From the Azure portal, get your storage account blob service URL endpoint.
URL, _ := url.Parse(
    fmt.Sprintf("https://%s.blob.core.windows.net/%s", accountName, containerName))

// Create a ContainerURL object that wraps the container URL and a request
// pipeline to make requests.
containerURL := azblob.NewContainerURL(*URL, p)

// Create the container
fmt.Printf("Creating a container named %s\n", containerName)
ctx := context.Background() // This example uses a never-expiring context
_, err := containerURL.Create(ctx, azblob.Metadata{}, azblob.PublicAccessNone)
handleErrors(err)
```
### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blobspeicher unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten und auch in diesem Schnellstart verwendet.  

Zum Hochladen einer Datei in ein Blob öffnen Sie die Datei mit **os.Open**. Anschließend können Sie die Datei mithilfe einer der REST-APIs – PutBlob, PutBlock/PutBlockList – zum angegebenen Pfad hochladen. 

Als Alternative bietet das SDK [High-Level-APIs](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go), die auf den Low-Level-REST-APIs aufbauen. Beispielsweise verwendet die ***UploadFileToBlockBlob***-Funktion PutBlock-Vorgänge zum gleichzeitigen Hochladen einer Datei in Blöcken, um den Durchsatz zu optimieren. Wenn die Datei kleiner als 256 MB ist, wird stattdessen PutBlob verwendet, um die Übertragung in einer Einzeltransaktion abzuschließen.

Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartblobs-[zufällige Zeichenfolge]** hochgeladen.

```go
// Here's how to upload a blob.
blobURL := containerURL.NewBlockBlobURL(fileName)
file, err := os.Open(fileName)
handleErrors(err)

// You can use the low-level PutBlob API to upload files. Low-level APIs are simple wrappers for the Azure Storage REST APIs.
// Note that PutBlob can upload up to 256MB data in one shot. Details: https://docs.microsoft.com/en-us/rest/api/storageservices/put-blob
// Following is commented out intentionally because we will instead use UploadFileToBlockBlob API to upload the blob
// _, err = blobURL.PutBlob(ctx, file, azblob.BlobHTTPHeaders{}, azblob.Metadata{}, azblob.BlobAccessConditions{})
// handleErrors(err)

// The high-level API UploadFileToBlockBlob function uploads blocks in parallel for optimal performance, and can handle large files as well.
// This function calls PutBlock/PutBlockList for files larger 256 MBs, and calls PutBlob for any file smaller
fmt.Printf("Uploading the file with blob name: %s\n", fileName)
_, err = azblob.UploadFileToBlockBlob(ctx, file, blobURL, azblob.UploadToBlockBlobOptions{
    BlockSize:   4 * 1024 * 1024,
    Parallelism: 16})
handleErrors(err)
```

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Rufen Sie eine Liste mit Dateien im Container ab, indem Sie die **ListBlobs**-Methode auf ein **ContainerURL**-Objekt anwenden. ListBlobs gibt ein einzelnes Segment von Blobs (bis zu 5000) beginnend mit dem angegebenen **Marker** zurück. Verwenden Sie einen leeren Marker, um die Enumeration von vorn zu beginnen. Blobnamen werden in lexikografischer Reihenfolge zurückgegeben. Nach dem Abrufen eines Segments verarbeiten Sie dieses und rufen anschließend erneut ListBlobs auf, um den zuvor zurückgegebenen Marker zu übergeben.  

```go
// List the blobs in the container
for marker := (azblob.Marker{}); marker.NotDone(); {
    // Get a result segment starting with the blob indicated by the current Marker.
    listBlob, err := containerURL.ListBlobs(ctx, marker, azblob.ListBlobsOptions{})
    handleErrors(err)

    // ListBlobs returns the start of the next segment; you MUST use this to get
    // the next segment (after processing the current result segment).
    marker = listBlob.NextMarker

    // Process the blobs returned in this result segment (if the segment is empty, the loop body won't execute)
    for _, blobInfo := range listBlob.Blobs.Blob {
        fmt.Print("Blob name: " + blobInfo.Name + "\n")
    }
}
```

### <a name="download-the-blob"></a>Herunterladen des Blobs

Laden Sie Blobs herunter, indem Sie die Low-Level-Methode **GetBlob** auf ein BlobURL-Objekt anwenden. Alternativ können Sie einen Stream erstellen und mithilfe der in [highlevel.go](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go) bereitgestellten High-Level-API **NewDownloadStream** Bereiche daraus lesen. Die NewDownloadStream-Funktion unternimmt nach einem Verbindungsfehler einen Wiederholungsversuch, wohingegen die GetBlob-API nur bei HTTP-Statuscodes wie 503 (Server ausgelastet) einen Wiederholungsversuch durchführt. Der folgende Code lädt das Blob mithilfe der **NewDownloadStream**-Funktion herunter. Die Inhalte des Blobs werden in einen Puffer geschrieben und auf der Konsole angezeigt.

```go
// Here's how to download the blob. NOTE: This method automatically retries if the connection fails
// during download (the low-level GetBlob function does NOT retry errors when reading from its stream).
stream := azblob.NewDownloadStream(ctx, blobURL.GetBlob, azblob.DownloadStreamOptions{})
downloadedData := &bytes.Buffer{}
_, err = downloadedData.ReadFrom(stream)
handleErrors(err)
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit **Delete**-Methode den gesamten Container löschen. 

```go
// Cleaning up the quick start by deleting the container and the file created locally
fmt.Printf("Press the enter key to delete the sample files, example container, and exit the application.\n")
bufio.NewReader(os.Stdin).ReadBytes('\n')
fmt.Printf("Cleaning up.\n")
containerURL.Delete(ctx, azblob.ContainerAccessConditions{})
file.Close()
os.Remove(fileName)
```

## <a name="next-steps"></a>Nächste Schritte
 
In diesem Schnellstart haben Sie gelernt, wie Sie mit Go Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Azure Storage Blob-SDK finden Sie im [Quellcode](https://github.com/Azure/azure-storage-blob-go/) und in der [API-Referenz](https://godoc.org/github.com/Azure/azure-storage-blob-go/2016-05-31/azblob).
