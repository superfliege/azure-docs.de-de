---
title: 'Azure-Schnellstart: Erstellen eines Blobs im Objektspeicher mithilfe des Java Storage-SDKs v10 | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung verwenden Sie das Java Storage-SDK, um Container in einem Objektspeicher (Blob) zu erstellen, Dateien hochzuladen, Objekte aufzulisten und Dateien herunterzuladen.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: rogarana
ms.openlocfilehash: a789269e73e1817f6a45e1e5948dbfaa21efd283
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704469"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-java-sdk-v10-preview"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit dem Java-SDK v10 (Preview)

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem Java Storage-SDK Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten. Beim neuen Java-SDK wird das reaktive Programmierungsmodell mit RxJava zur Bereitstellung von asynchronen Vorgängen verwendet. Weitere Informationen zu RxJava finden Sie [hier](https://github.com/ReactiveX/RxJava). 

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* Installieren und konfigurieren Sie [Maven](http://maven.apache.org/download.cgi) zur Verwendung über die Befehlszeile oder eine beliebige Java-IDE Ihrer Wahl.
* Installieren und konfigurieren Sie ein [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) ist eine einfache Konsolenanwendung. 

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont.

Nachdem der Import durch das Projekt abgeschlossen ist, öffnen Sie **Quickstart.java** (im Verzeichnis **src/main/java/quickstart**).

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
Für diese Lösung müssen der Name und der Schlüssel Ihres Speicherkontos sicher in Umgebungsvariablen gespeichert sein, die sich auf dem Computer befinden, auf dem das Beispiel ausgeführt wird. Befolgen Sie je nach Betriebssystem die Schritte für eines der unten angegebenen Beispiele, um die Umgebungsvariablen zu erstellen.

### <a name="for-linux"></a>Für Linux

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="for-windows"></a>Für Windows

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Mit diesem Beispiel wird eine Testdatei in Ihrem Standardverzeichnis (AppData\Local\Temp, bei Windows-Benutzern) erstellt. Anschließend werden Sie aufgefordert, Befehle zum Hochladen der Testdatei in Blob Storage, zum Auflisten der Blobs im Container sowie zum Herunterladen der hochgeladenen Datei mit einem neuen Namen einzugeben, sodass Sie die alten und neuen Dateien vergleichen können. 

Wenn Sie das Beispiel mithilfe von Maven über die Befehlszeile ausführen möchten, öffnen Sie eine Shell, und navigieren Sie zu **storage-blobs-java-v10-quickstart** innerhalb Ihres geklonten Verzeichnisses. Geben Sie dann `mvn compile exec:java` ein.

Im Folgenden finden Sie ein Beispiel der Ausgabe, wenn Sie die Anwendung unter Windows ausführen.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Da Sie die Kontrolle über das Beispiel haben, geben Sie Befehle ein, damit der Code ausgeführt wird. Beachten Sie, dass bei den Eingaben die Groß-/Kleinschreibung berücksichtigt wird.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie **E** und dann die EINGABETASTE, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen mit dem Beispiel ausgeführt werden, öffnen Sie die Datei **Quickstart.java**, um den Code zu betrachten. 

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Als Nächstes gehen wir schrittweise durch den Beispielcode, damit Sie verstehen können, wie er funktioniert.

### <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte

Zunächst müssen die Verweise auf die Objekte erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf – jedes wird vom jeweils nächsten in der Liste verwendet.

* Erstellen Sie eine Instanz des „StorageURL“-Objekts, das auf das Speicherkonto verweist.

    Das [**StorageURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-preview)-Objekt ist eine Darstellung Ihres Speicherkontos, das Ihnen die Erzeugung einer neuen Pipeline ermöglicht. Bei einer Pipeline ([HTTP-Pipeline](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline)) handelt es sich um eine Reihe von Richtlinien, die zum Bearbeiten von Anforderungen und Antworten mit Autorisierungs-, Protokollierungs- und Wiederholmechanismen verwendet werden. Mit der Pipeline können Sie eine Instanz des [**ServiceURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-preview)-Objekts erstellen, das es Ihnen ermöglicht, eine Instanz des [**ContainerURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview)-Objekts zu erstellen, das wiederum zum Ausführen von Vorgängen bei Blob-Containern erforderlich ist.

* Erstellen Sie eine Instanz des „ContainerURL“-Objekts, das den Container darstellt, auf den Sie zugreifen. Container werden zum Organisieren der Blobs verwendet, so wie Sie auf Ihrem Computer Ordner zum Organisieren von Dateien verwenden.

    Das **ContainerURL**-Objekt stellt Ihnen einen Zugriffspunkt für den Containerdienst bereit. Mithilfe des **ContainerURL**-Objekts können Sie eine Instanz des [**BlobURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview)-Objekts erstellen, das zum Erstellen von Blobs erforderlich ist.

* Erstellen Sie eine Instanz des [BlobURL](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-preview)-Objekts, das auf das Blob verweist, das für Sie von Interesse ist.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Benennen von Containern, Blobs und Metadaten und Verweisen auf diese](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Erstellen eines Containers 

In diesem Abschnitt erstellen Sie eine Instanz des „ContainerURL“-Objekts sowie einen neuen Container. Der Name des Containers in diesem Beispiel lautet **quickstart**. 

In diesem Beispiel wird [containerURL.create](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-preview) verwendet, da bei jedem Ausführen des Beispiels ein neuer Container erstellt werden soll. Alternativ können Sie den Container im Voraus erstellen, damit er nicht im Code erstellt werden muss.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blob Storage unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten verwendet und auch in diesem Schnellstart verwendet. 

Um eine Datei in ein Blob hochzuladen, rufen Sie einen Verweis auf das Blob im Zielcontainer ab. Sobald Sie den Blobverweis erhalten haben, können Sie eine Datei in das Blob hochladen. Verwenden Sie hierzu entweder untergeordnete APIs wie [Upload](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-preview) (auch als „PutBlob“ bezeichnet, [StageBlock](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-preview#com_microsoft_azure_storage_blob__block_blob_u_r_l_stageBlock_String_Flowable_ByteBuffer__long_LeaseAccessConditions_) auch als „PutBLock“ bezeichnet) in der „BlockBlobURL“-Instanz oder übergeordnete APIs wie die [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-preview)-Methode, die in der [TransferManager](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-preview)-Klasse bereitgestellt werden. Wenn noch kein Blob vorhanden ist, wird durch diesen Vorgang eines erstellt. Andernfalls wird das vorhandene Blob überschrieben.

Der Beispielcode erstellt eine lokale Datei zur Verwendung für den Upload und Download. Die hochzuladende Datei wird als **sourceFile** und die URL des Blobs in **blob** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstart** hochgeladen.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {
    
    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());
            
    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Blockblobs können jede Art von Text oder Binärdateien darstellen. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs werden zum Anfügen von Daten an das Ende und häufig zum Protokollieren verwendet. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Mit [containerURL.listBlobsFlatSegment](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-preview) können Sie eine Liste mit Objekten in einem Container abrufen. Mit dieser Methode werden bis zu 5000 Objekte auf einmal sowie eine Fortsetzungsmarkierung (next-Markierung) zurückgegeben, falls weitere Objekte im Container enthalten sind. Daher erstellen wir eine Hilfsfunktion, die sich selbst wiederholt aufruft, solange in der vorherigen „listBlobsFlatSegment“-Antwort eine next-Markierung vorhanden ist.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs, 
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse -> 
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))    
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {                
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }
    
    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */
            
        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */
            
    return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie mithilfe von [blobURL.download](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-preview) Blobs auf Ihren lokalen Datenträger herunter.

Der folgende Code lädt das im vorherigen Abschnitt hochgeladene Blob herunter und fügt dem Blobnamen das Suffix „_DOWNLOADED“ hinzu, damit auf dem lokalen Datenträger beide Dateien angezeigt werden. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit [containerURL.delete](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-preview) den gesamten Container löschen. Mit dieser Methode werden auch die Dateien im Container gelöscht.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie mit Java Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. 

> [!div class="nextstepaction"]
> [Storage SDK v10 für Java-Quellcode](https://github.com/Azure/azure-storage-java/tree/New-Storage-SDK-V10-Preview)
> [API-Referenz](https://docs.microsoft.com/en-us/java/api/storage/client?view=azure-java-preview)
> [Weitere Informationen zu RxJava](https://github.com/ReactiveX/RxJava)