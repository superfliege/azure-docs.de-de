---
title: "Azure-Schnellstart – Übertragen von Objekten nach/aus Azure Blob Storage mit Python | Microsoft-Dokumentation"
description: "Hier lernen Sie schnell, wie Sie mit Python Objekte nach/aus Azure Blob Storage übertragen."
services: storage
documentationcenter: storage
author: ruthogunnnaike
manager: cwatson
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 44ec416a814ff6a5fef79ef21e2f54ce4ce4da17
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Übertragen von Objekten nach/aus Azure Blob Storage mit Python
In diesem Schnellstart erfahren Sie, wie Sie mit Python Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten. 

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch: 
* [Installieren Sie Python.](https://www.python.org/downloads/)
* Laden Sie das [Azure Storage SDK für Python](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python) herunter, und installieren Sie es. 

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

Nachdem das Speicherkonto erstellt wurde, wird es an das Dashboard angeheftet. Klicken Sie darauf, um es zu öffnen. Klicken Sie unter **EINSTELLUNGEN** auf **Zugriffsschlüssel**. Wählen Sie einen Schlüssel aus, kopieren Sie den Speicherkontonamen in die Zwischenablage, und fügen Sie ihn für die spätere Verwendung in Editor ein.

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung
Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) ist eine einfache Python-Anwendung.  

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Um die Python-Anwendung zu öffnen, suchen Sie nach dem Ordner „storage-blobs-python-quickstart“ und nach der Datei „example.py“.  

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
In der Anwendung müssen Sie Ihren Speicherkontonamen sowie den Kontoschlüssel angeben, um das `BlockBlobService`-Objekt zu erstellen. Öffnen Sie im Lösungsmappen-Explorer in Ihrer IDE die Datei `example.py`. Ersetzen Sie die Werte **accountname** und **accountkey** mit Ihrem Kontonamen und dem Kontoschlüssel. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Ausführen des Beispiels
Dieses Beispiel erstellt eine Testdatei im Ordner „Dokumente“. Das Beispielprogramm lädt die Testdatei in den Blobspeicher hoch, listet die Blobs im Container auf und lädt die Datei mit einem neuen Namen herunter. 

Führen Sie das Beispiel aus. Die folgende Ausgabe ist ein Beispiel der Ausgabe, die zurückgegeben wird, wenn die Anwendung ausgeführt wird.
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Wenn Sie eine beliebige Taste drücken, um den Vorgang fortzusetzen, löscht das Beispielprogramm den Speichercontainer und die Dateien. Überprüfen Sie Ihren Ordner „Dokumente“ nach den zwei Dateien, bevor Sie fortfahren. Sie können diese öffnen und prüfen, ob sie identisch sind.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](http://storageexplorer.com), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie eine beliebige Taste, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen das Beispiel ausführt, öffnen Sie die Datei „example.py“, um den Code zu betrachten. 

## <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte
Zunächst müssen die Verweise auf die Objekte erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf und jedes wird vom jeweils nächsten in der Liste verwendet.

* Instanziieren Sie das **BlockBlobService**-Objekt, das auf den Blob-Dienst im Speicherkonto verweist. 

* Instanziieren Sie das **CloudBlobContainer**-Objekt, das den Container darstellt, auf den Sie zugreifen. Container werden zum Organisieren der Blobs verwendet, so wie Sie auf Ihrem Computer Ordner zum Organisieren von Dateien verwenden.

Sobald Sie über den Cloud-Blobcontainer verfügen, können Sie das **CloudBlockBlob**-Objekt instanziieren, das auf den speziellen Blob verweist, der für Sie von Interesse ist, und Vorgänge wie einen Upload-, Download- oder Kopiervorgang ausführen.

In diesem Abschnitt instanziieren Sie die Objekte, erstellen einen neuen Container und legen dann Berechtigungen für den Container fest, damit die Blobs öffentlich sind. Der Name des Containers lautet **quickstartblobs**. 


```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
## <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blob Storage unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten und auch in diesem Schnellstart verwendet.  

Um eine Datei in einen Blob hochzuladen, rufen Sie den vollständigen Pfad der Datei ab, indem Sie den Verzeichnisnamen und den Dateinamen auf Ihrem lokalen Laufwerk verknüpfen. Sie können anschließend die Datei auf den angegebenen Pfad mithilfe der Methode „**create\_blob\_from\_path**“ hochladen. 

Der Beispielcode erstellt eine lokale Datei für den Upload und Download. Dabei wird die hochzuladende Datei als **file\_path\_to\_file** und der Name des Blob in **local\_file\_name** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartblobs** hochgeladen.

```python
# Create a file in Documents to test the upload and download.
local_path=os.path.expanduser("~\Documents")
local_file_name ="QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file =os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name
block_blob_service.create_blob_from_path(container_name, local_file_name, full_path_to_file)
```

Sie können für Blob Storage mehrere Uploadmethoden verwenden. Wenn Sie z.B. über einen Speicherdatenstrom verfügen, können Sie statt „**create\_blob\_from\_path**“ die Methode „**create\_blob\_from\_stream**“ verwenden. 

Blockblobs können eine Größe von bis zu 4,7 TB haben und alle Arten von Dateien sein, von Excel-Arbeitsblättern bis zu großen Videodateien. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Rufen Sie eine Liste der Dateien im Container mithilfe der Methode „**list_blobs**“ ab. Diese Methode gibt einen Generator zurück. Der folgende Code ruft die Liste der Blobs ab und durchläuft sie, um die Namen der in einem Container gefundenen Blobs anzuzeigen.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

## <a name="download-the-blobs"></a>Herunterladen der Blobs

Laden Sie Blobs auf Ihren lokalen Datenträger mithilfe der Methode „**get\_blob\_to\_path**“ herunter. Durch den folgenden Code wird der in einem vorherigen Abschnitt hochgeladene Blob heruntergeladen. „_DOWNLOADED“ wird als Suffix an den Blobnamen hinzugefügt, damit Sie beide Dateien auf dem lokalen Datenträger sehen können. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit **delete\_container** den gesamten Container löschen. Wenn die erstellten Dateien nicht mehr benötigt werden, verwenden Sie die **delete\_blob**-Methode, um die Dateien zu löschen.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Nächste Schritte
 
In diesem Schnellstart haben Sie gelernt, wie Sie mit Python Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](./storage-python-how-to-use-blob-storage.md)
 

Weitere Informationen zum Storage-Explorer und Blobs finden Sie unter [Verwalten von Azure Blob Storage-Ressourcen mit dem Speicher-Explorer (Vorschau)](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
