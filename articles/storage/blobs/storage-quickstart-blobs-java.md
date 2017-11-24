---
title: "Azure-Schnellstart – Übertragen von Objekten nach/aus Azure Blob Storage mit Java | Microsoft-Dokumentation"
description: "Hier lernen Sie schnell, wie Sie mit Java Objekte nach/aus Azure Blob Storage übertragen."
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: 2a825e59093b64ab56110f9a045b0325728b39b2
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Übertragen von Objekten nach/aus Azure Blob Storage mit Java

In diesem Schnellstart erfahren Sie, wie Sie mit Java Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten.

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* Installieren einer IDE mit Maven-Integration

* Alternativ können Sie Maven installieren und so konfigurieren, dass es über die Befehlszeile verwendet werden kann.

In diesem Tutorial wird [Eclipse](http://www.eclipse.org/downloads/) mit der Konfiguration „Eclipse-IDE für Java-Entwickler“ verwendet.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-java-quickstart) ist eine einfache Konsolenanwendung. 

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Starten Sie Eclipse, und schließen Sie die Willkommensseite, um das Projekt zu öffnen. Klicken Sie auf **Datei** und dann auf **Open Projects from File System** (Projekte aus Dateisystem öffnen...). Stellen Sie sicher, dass **Detect and configure project natures** (Projektarten erkennen und konfigurieren) aktiviert ist. Klicken Sie auf **Verzeichnis**, und navigieren Sie zum Speicherort des geklonten Repositorys. Wählen Sie dort den Ordner **javaBlobsQuickstart** aus. Stellen Sie sicher, dass das Projekt **javaBlobsQuickstart** als Eclipse-Projekt angezeigt wird, und klicken Sie dann auf **Fertig stellen**.

Nachdem das Importieren des Projekts abgeschlossen ist, öffnen Sie **AzureApp.java** (unter **blobQuickstart.blobAzureApp** in **src/main/java**), und ersetzen Sie `accountname` und `accountkey` in der `storageConnectionString`-Zeichenfolge. Führen Sie die Anwendung dann aus.
     

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
    
Sie müssen in der Anwendung die Verbindungszeichenfolge für das Speicherkonto angeben. Öffnen Sie die Datei **AzureApp.Java**. Suchen Sie die Variable `storageConnectionString`. Ersetzen Sie die Werte `AccountName` und `AccountKey` in der Verbindungszeichenfolge durch die Werte, die Sie im Azure-Portal gespeichert haben. Ihre `storageConnectionString` sollte etwa folgendermaßen aussehen:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Dieses Beispiel erstellt eine Testdatei in Ihrem Standardverzeichnis („Dokumente“ für Windows-Benutzer), lädt diese in Blob Storage hoch, listet die Blobs im Container auf und lädt die Datei dann mit einem neuen Namen herunter, sodass Sie die alten und neuen Dateien vergleichen können. 

Führen Sie das Beispiel aus, indem Sie **STRG+F11** in Eclipse drücken.

Wenn Sie das Beispiel mithilfe von Maven über die Befehlszeile ausführen möchten, öffnen Sie eine Shell, und navigieren Sie zu **blobAzureApp** innerhalb Ihres geklonten Verzeichnisses. Geben Sie dann `mvn compile exec:java` ein.

Im Folgenden finden Sie ein Beispiel der Ausgabe, wenn Sie die Anwendung unter Windows ausführen.

```
Location of file: C:\Users\<user>\Documents\results.txt
File has been written
URI of blob is: http://myexamplesacct.blob.core.windows.net/quickstartblobs/results.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

 Bevor Sie fortfahren, überprüfen Sie Ihr Standardverzeichnis („Dokumente“ für Windows-Benutzer) auf die beiden Dateien. Sie können diese öffnen und prüfen, ob sie identisch sind. Kopieren Sie die URL für das Blob aus dem Konsolenfenster, und fügen Sie sie in einem Browser ein, um den Inhalt der Datei in Blob Storage anzuzeigen. Wenn Sie die EINGABETASTE drücken, werden der Speichercontainer und die Dateien gelöscht.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie die EINGABETASTE, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen das Beispiel ausführt, öffnen Sie die Datei **AzureApp.java**, um den Code zu betrachten. 

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Als Nächstes gehen wir schrittweise durch den Beispielcode, damit Sie verstehen können, wie er funktioniert.

### <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte

Zunächst müssen die Verweise auf die Objekte erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf – jedes wird vom jeweils nächsten in der Liste verwendet.

* Erstellen Sie eine Instanz des **CloudStorageAccount**-Objekts, das auf das [Speicherkonto](/java/api/com.microsoft.azure.management.storage._storage_account) verweist.

    Das **CloudStorageAccount**-Objekt ist eine Darstellung Ihres Speicherobjekts, und es ermöglicht den programmgesteuerten Zugriff und das programmgesteuerte Festlegen von Eigenschaften des Speicherkontos. Mithilfe des **CloudStorageAccount**-Objekts können Sie eine Instanz von **CloudBlobClient** erstellen, was für den Zugriff auf den Blob-Dienst erforderlich ist.

* Erstellen Sie eine Instanz des **CloudBlobClient**-Objekts, das auf den [Blob-Dienst](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) im Speicherkonto verweist.

    Der **CloudBlobClient** stellt Ihnen einen Zugriffspunkt für den Blob-Dienst zur Verfügung, mit dem Sie programmgesteuert auf Blob-Speichereigenschaften zugreifen und sie festlegen können. Mithilfe von **CloudBlobClient** können Sie eine Instanz des **CloudBlobClient**-Objekts erstellen, was zum Erstellen von Containern erforderlich ist.

* Erstellen Sie eine Instanz des **CloudBlobContainer**-Objekts, das den [Container](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) darstellt, auf den Sie zugreifen. Container werden zum Organisieren der Blobs verwendet, so wie Sie auf Ihrem Computer Ordner zum Organisieren von Dateien verwenden.    

    Sobald Sie über den **CloudBlobContainer** verfügen, können Sie eine Instanz des **CloudBlockBlob**-Objekts erstellen, das auf den speziellen [Blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) verweist, der für Sie von Interesse ist, und einen Upload-, Download- oder Kopiervorgang sowie weitere Vorgänge ausführen.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Benennen von Containern, Blobs und Metadaten und Verweisen auf diese](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Erstellen eines Containers 

In diesem Abschnitt erstellen Sie eine Instanz der Objekte und einen neuen Container und legen dann Berechtigungen für den Container fest, damit die Blobs öffentlich sind und einfach mit einer URL auf sie zugegriffen werden kann. Der Name des Containers lautet **quickstartblobs**. 

In diesem Beispiel wird **CreateIfNotExists** verwendet, da bei jedem Ausführen des Beispiels ein neuer Container erstellt werden soll. In einer Produktionsumgebung, in der Sie in der gesamten Anwendung den gleichen Container verwenden, empfiehlt es sich hingegen, **CreateIfNotExists** nur einmal aufzurufen. Alternativ können Sie den Container im Voraus erstellen, damit er nicht im Code erstellt werden muss.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blob Storage unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten verwendet und auch in diesem Schnellstart verwendet. 

Um eine Datei in ein Blob hochzuladen, rufen Sie einen Verweis auf das Blob im Zielcontainer ab. Nachdem Sie den Blobverweis abgerufen haben, können Sie mithilfe von [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long) Daten in das Blob hochladen. Wenn noch kein Blob vorhanden ist, wird durch diesen Vorgang eines erstellt. Andernfalls wird das vorhandene Blob überschrieben.

Der Beispielcode erstellt eine lokale Datei zur Verwendung für den Upload und Download. Die hochzuladende Datei wird als **source** und der Name des Blobs in **blob** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartblobs** hochgeladen.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Es gibt eine Reihe von [Uploadmethoden](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob), die Sie in Kombination mit Blob Storage verwenden können. Beispielsweise können Sie für eine Zeichenfolge die UploadText-Methode anstelle der Upload-Methode verwenden. 

Blockblobs können jede Art von Text oder Binärdateien darstellen. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Mit [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs) können Sie eine Liste von Dateien im Container abrufen. Der folgende Code ruft die Liste der Blobs ab und durchläuft sie, um die URIs der gefundenen Blobs anzuzeigen. Sie können den URI aus dem Befehlsfenster kopieren und in einen Browser einfügen, um die Datei anzuzeigen.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie mit [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String) Blobs auf den lokalen Datenträger herunter.

Der folgende Code lädt das im vorherigen Abschnitt hochgeladene Blob herunter und fügt dem Blobnamen das Suffix „_DOWNLOADED“ hinzu, damit auf dem lokalen Datenträger beide Dateien angezeigt werden. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists) den gesamten Container löschen. Dadurch werden ebenfalls die Dateien im Container gelöscht.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie mit Java Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](storage-java-how-to-use-blob-storage.md)

Weitere Informationen zum Storage-Explorer und Blobs finden Sie unter [Verwalten von Azure Blob Storage-Ressourcen mit dem Speicher-Explorer (Vorschau)](../../vs-azure-tools-storage-explorer-blobs.md).
