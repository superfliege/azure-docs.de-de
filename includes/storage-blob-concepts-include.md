---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/17/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7889fbc9373cbdfdfab891bf8b1cd610523c7032
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088090"
---
Azure Blob Storage ist die Objektspeicherlösung von Microsoft für die Cloud. Blobspeicher ist für die Speicherung großer Mengen von unstrukturierten Daten, z.B. Text oder Binärdaten, optimiert.

Blobspeicher ist für folgende Zwecke ideal geeignet:

* Speichern von Bildern oder Dokumenten direkt für einen Browser
* Speichern von Dateien für verteilten Zugriff
* Video- und Audio-Streaming
* Schreiben in Protokolldateien
* Speichern von Daten für Sicherung und Wiederherstellung, Notfallwiederherstellung und Archivierung
* Speichern von Daten für Analysen durch einen lokalen oder von Azure gehosteten Dienst

Benutzer- oder Clientanwendungen können auf Blobspeicherobjekte per HTTP oder HTTPS (von jedem Ort der Welt) über URLs, die [Azure Storage-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), die [Azure CLI](https://docs.microsoft.com/cli/azure/storage) oder eine Azure Storage-Clientbibliothek zugreifen. Die Speicherclientbibliotheken sind für verschiedene Sprachen verfügbar, z.B. [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [PHP](http://azure.github.io/azure-storage-php/) und [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Konzepte des Blob-Diensts

Blobspeicher macht drei Ressourcen verfügbar: Ihr Speicherkonto, die Container im Konto und die Blobs in einem Container. Im folgenden Diagramm ist die Beziehung zwischen diesen Ressourcen dargestellt.

![Diagramm der Blobspeicherarchitektur (bzw. Objektspeicher)](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Speicherkonto

Der gesamte Zugriff auf Datenobjekte erfolgt in Azure Storage über ein Speicherkonto. Weitere Informationen finden Sie unter [Azure-Speicherkonto – Übersicht](../articles/storage/common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Container

Ein Container dient zum Organisieren einer Gruppe von Blobs (ähnlich wie ein Ordner in einem Dateisystem). Alle Blobs befinden sich innerhalb eines Containers. Ein Speicherkonto kann eine unbegrenzte Anzahl von Containern enthalten, und in einem Container kann eine unbegrenzte Anzahl von Blobs gespeichert werden. 

  > [!NOTE]
  > Der Containername darf ausschließlich Kleinbuchstaben enthalten.

### <a name="blob"></a>Blob
 
Azure Storage verfügt über drei Arten von Blobs: Blockblobs, Anfügeblobs und [Seitenblobs](../articles/storage/blobs/storage-blob-pageblob-overview.md) (für VHD-Dateien).

* In Blockblobs werden Text- und Binärdaten bis zu einer Größe von ca. 4,7 TB gespeichert. Blockblobs bestehen aus Datenblöcken, die einzeln verwaltet werden können.
* Anfügeblobs bestehen wie Blockblobs auch aus Blöcken, aber sie sind für Anfügevorgänge optimiert. Anfügeblobs sind beispielsweise ideal für Szenarien, bei denen es um das Protokollieren von Daten virtueller Computer geht.
* In Seitenblobs werden Random-Access-Dateien mit einer Größe von bis zu 8 TB gespeichert. Die VHD-Dateien, die als Grundlage für VMs dienen, werden in Seitenblobs gespeichert.

Alle Blobs befinden sich innerhalb eines Containers. Ein Container ähnelt einem Ordner in einem Dateisystem. Sie können Blobs außerdem in virtuellen Verzeichnissen anordnen und wie in einem Dateisystem darin navigieren. 

Es kann sein, dass das Hochladen von Daten in Blobspeicher per Übertragung aufgrund von großen Datasets und Netzwerkeinschränkungen unrealistisch ist. Sie können [Azure Data Box Disk](../articles/databox/data-box-disk-overview.md) nutzen, um SSDs (Solid-State Disks) von Microsoft anzufordern. Anschließend können Sie Ihre Daten auf diese Datenträger kopieren und zurück an Microsoft senden, damit sie in den Blobspeicher hochgeladen werden können.

Falls Sie große Datenmengen aus Ihrem Speicherkonto exportieren müssen, helfen Ihnen die Informationen unter [Was ist der Azure Import/Export-Dienst?](../articles/storage/common/storage-import-export-service.md) weiter.
  
Ausführliche Informationen zum Benennen von Containern und Blobs finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)(Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).
