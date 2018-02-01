---
title: Verwenden des Blobspeichers (Objektspeicher) mit Ruby | Microsoft Docs
description: Speichern Sie nicht strukturierte Daten in der Cloud mit Azure Blob Storage (Objektspeicher).
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 01/18/2018
ms.author: tamram
ms.openlocfilehash: c4c6d47511acdae7afaf4a535c24c6fcc7e389b1
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-use-blob-storage-from-ruby"></a>Verwenden des Blob-Speichers mit Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht
Der Azure-BLOB-Speicher ist ein Dienst, bei dem unstrukturierte Daten in der Cloud als Objekte/Blobs gespeichert werden. In Blob Storage können alle Arten von Text- oder Binärdaten gespeichert werden, z. B. ein Dokument, eine Mediendatei oder ein Installer einer Anwendung. Der Blobspeicher wird auch als Objektspeicher bezeichnet.

In diesem Leitfaden wird die Durchführung gängiger Szenarien mit Blob Storage demonstriert. Die Beispiele wurden mit der Ruby-API erstellt. Die behandelten Szenarien umfassen das **Hochladen, Auflisten, Herunterladen** und **Löschen** von Blobs.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Erstellen einer Ruby-Anwendung
Erstellen Sie eine Ruby-Anwendung. Anweisungen finden Sie unter [Erstellen einer Ruby-App in App Service unter Linux](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby).


## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendung für den Zugriff auf Storage
Um Azure Storage zu verwenden, müssen Sie das Ruby-Azure-Paket herunterladen und verwenden. Dieses Paket enthält eine Reihe von Bibliotheken, die mit den REST-Speicherdiensten kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>Verwenden von RubyGems zum Abrufen des Pakets
1. Verwenden Sie eine Befehlszeilenschnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).
2. Geben Sie im Befehlsfenster „gem install azure-storage-blob“ ein, um das Gem und Abhängigkeiten zu installieren.

### <a name="import-the-package"></a>Importieren des Pakets
Fügen Sie mit Ihrem bevorzugten Texteditor Folgendes oben in die Ruby-Datei an der Stelle ein, an der Sie den Speicher verwenden möchten:

```ruby
require "azure/storage/blob"
```

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Azure Storage-Verbindung
Das Azure-Modul liest die Umgebungsvariablen **AZURE\_STORAGE\_ACCOUNT** und **AZURE\_STORAGE\_ACCESS_KEY** nach Informationen aus, die erforderlich sind, um eine Verbindung mit dem Azure-Speicherkonto herzustellen. Wenn diese Umgebungsvariablen nicht festgelegt werden, müssen Sie mithilfe von **Azure::Blob::BlobService::create** die Kontoinformationen mit dem folgenden Code angeben:

```ruby
blob_client = Azure::Storage::Blob::BlobService.create(
    storage_account_name: account_name,
    storage_access_key: account_key
    )
```

So rufen Sie diese Werte aus einem klassischen oder Resource Manager-Speicherkonto im Azure-Portal ab

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zum Speicherkonto, das Sie verwenden möchten.
3. Klicken Sie auf dem Blatt „Einstellungen“ auf der rechten Seite auf **Zugriffsschlüssel**.
4. Auf dem angezeigten Blatt „Zugriffsschlüssel“ sehen Sie Zugriffsschlüssel 1 und Zugriffsschlüssel 2. Sie können beide verwenden.
5. Klicken Sie auf das Symbol „Kopieren“, um den Schlüssel in die Zwischenablage zu kopieren.

## <a name="create-a-container"></a>Erstellen eines Containers
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Mit dem **Azure::Storage::Blob::BlobService**-Objekt können Sie mit Containern und Blobs arbeiten. Verwenden Sie die **create\_container()**-Methode, um einen Container zu erstellen.

Im folgenden Codebeispiel wird ein Container erstellt oder ggf. ein Fehler ausgegeben.

```ruby
azure_blob_service = Azure::Storage::Blob::BlobService.create_from_env
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Wenn Sie die Dateien im Container öffentlich machen möchten, können Sie die Berechtigungen des Containers festlegen.

Sie können einfach den <strong>create\_container()</strong>-Aufruf ändern, um die **public\_access\_level**-Option zu übergeben:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Gültige Werte für die **public\_access\_level**-Option sind:

* **Blob:** Gibt öffentlichen Lesezugriff für Blobs an. Blob-Daten innerhalb dieses Containers können über anonyme Anforderungen gelesen werden, Containerdaten sind aber nicht verfügbar. Clients können keine Blobs innerhalb des Containers über anonyme Anforderungen aufzählen.
* **Container:** Gibt vollständigen öffentlichen Lesezugriff für Container- und Blobdaten an. Clients können Blobs innerhalb des Containers über eine anonyme Anforderung aufzählen, können aber keine Container innerhalb des Speicherkontos aufzählen.

Sie können aber auch die öffentliche Zugriffsstufe eines Container mithilfe der **set\_container\_acl()**-Methode ändern, um die öffentliche Zugriffsstufe anzugeben.

Im folgenden Codebeispiel wird die öffentliche Zugriffsstufe in **container**geändert:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container
Um Inhalte in einen Blob hochzuladen, verwenden Sie die **create\_block\_blob()**-Methode, um den Blob zu erstellen, und verwenden Sie eine Datei oder Zeichenfolge als Inhalt des Blobs.

Durch den folgenden Code wird die Datei **test.png** als neuer Blob namens "image-blob" in den Container hochgeladen.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container
Verwenden Sie die **list_containers()**-Methode, um die Container aufzulisten.
Verwenden Sie die **list\_blobs()**-Methode, um die Blobs innerhalb eines Containers aufzulisten. Um alle Blobs in einem Container aufzulisten, müssen Sie ein vom Dienst zurückgegebenes Fortsetzungstoken nachverfolgen und weiterhin „list_blobs“ mit diesem Token ausführen. Einzelheiten finden Sie unter [Auflisten der REST-API von Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs).

Der folgende Code gibt alle Blobs in einem Container aus.

```ruby
nextMarker = nil
loop do
    blobs = azure_blob_service.list_blobs(container_name, { marker: nextMarker })
    blobs.each do |blob|
        puts "\tBlob name #{blob.name}"
    end
    nextMarker = blobs.continuation_token
    break unless nextMarker && !nextMarker.empty?
end
```

## <a name="download-blobs"></a>Herunterladen von Blobs
Um Blobs herunterzuladen, verwenden Sie die **get\_blob()**-Methode zum Abrufen des Inhalts.

Das folgende Codebeispiel zeigt, wie Sie **get\_blob()** verwenden, um die Inhalte von „iage-blob“ erunterzuladen und in eine lokale Datei zu schreiben.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Löschen eines Blobs
Verwenden Sie schließlich die **delete\_blob()**-Methode, um einen Blob zu löschen. Das folgende Codebeispiel demonstriert das Löschen eines Blobs.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links erhalten Sie weitere Informationen zu komplexeren Speicheraufgaben:

* [Azure Storage-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
* Repository [Azure Storage SDK für Ruby](https://github.com/azure/azure-storage-ruby) in GitHub
* [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

