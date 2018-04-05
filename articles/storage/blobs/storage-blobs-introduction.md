---
title: 'Einführung in Blob Storage: Objektspeicher in Azure | Microsoft-Dokumentation'
description: Azure Blob Storage dient zum Speichern großer Mengen unstrukturierter Objektdaten wie etwa Text- und Binärdaten. Ihre Anwendungen können über PowerShell oder über die Azure-Befehlszeilenschnittstelle, über Code mittels Azure Storage-Clientbibliotheken oder über REST auf Objekte in Blob Storage zugreifen.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: overview
ms.date: 03/27/2018
ms.author: tamram
ms.openlocfilehash: 0fff0032ec2452413bcd1df3175634b14a64208f
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="introduction-to-blob-storage"></a>Einführung in Blob Storage

Azure Blob Storage ist Microsofts Cloudspeicherlösung für Datenobjekte. In Blob Storage können große Mengen unstrukturierter Objektdaten wie etwa Text- und Binärdaten gespeichert werden. Über HTTP oder HTTPS kann von überall auf der Welt auf die Daten in Blob Storage zugegriffen werden. Sie können Blob Storage verwenden, um Daten öffentlich auf der ganzen Welt zur Verfügung zu stellen oder um Anwendungsdaten privat zu speichern.

BLOB-Speicherungen werden hauptsächlich für folgende Zwecke verwendet:

* Speichern von Bildern oder Dokumenten direkt auf einem Browser
* Speichern von Dateien für verteilten Zugriff
* Video- und Audio-Streaming
* Speicherung für Sicherung und Wiederherstellung, Notfallwiederherstellung und Archivierung
* Speicherung von Daten für Analysen durch einen lokalen oder von Azure gehosteten Dienst
* Speicherung virtueller Festplatten für die Verwendung mit Azure Virtual Machines

## <a name="blob-service-concepts"></a>Konzepte des Blob-Diensts

Der BLOB-Dienst umfasst die folgenden Komponenten:

![Blob-Architektur](./media/storage-blobs-introduction/blob1.png)

* **Speicherkonto:** Alle Zugriffe auf den Azure-Speicher erfolgen über ein Speicherkonto. Dieses Speicherkonto kann ein **allgemeines Speicherkonto (v1 oder v2)** oder ein **Blob Storage-Konto** sein. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Container:** Ein Container dient zur Gruppierung eines Satzes von Blobs. Alle BLOBs müssen sich in Containern befinden. Ein Konto kann eine beliebige Anzahl von Containern enthalten. In einem Container kann eine beliebige Anzahl von BLOBs gespeichert sein. Beachten Sie, dass der Containername ausschließlich Kleinbuchstaben enthalten darf.

* **Blob:** Eine Datei von beliebiger Art und Größe. Azure Storage bietet drei Arten von Blobs: Blockblobs, [Seitenblobs](storage-blob-pageblob-overview.md) und Anfügeblobs.
  
    *Blockblobs* eignen sich ideal zum Speichern von Text- oder Binärdateien, z.B. Dokumente und Mediendateien. *Anfügeblobs* ähneln Blockblobs dahingehend, dass sie aus Blöcken bestehen. Allerdings sind sie für Anfügevorgänge optimiert, sodass sie für Protokollierungsszenarien nützlich sind. Ein einzelnes Blockblob kann bis zu 50.000 Blöcke mit jeweils bis zu 100 MB enthalten; dies ergibt eine Gesamtgröße von etwas mehr als 4,75 TB (100 MB X 50.000). Ein einzelnes Anfügeblob kann bis zu 50.000 Blöcke mit jeweils bis zu 4 MB enthalten; dies ergibt eine Gesamtgröße von etwas mehr als 195 GB (4 MB X 50.000).
  
    *Seitenblobs* können bis zu 8 TB groß sein und sind besonders für häufige Lese-und Schreibvorgänge effizient. Virtuelle Azure-Computer verwenden Seitenblobs als Betriebssystem und Datenträger.
  
    Ausführliche Informationen zum Benennen von Containern und Blobs finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)(Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein Speicherkonto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md)
* [Azure Storage-Beispiele mit .NET](../common/storage-samples-dotnet.md)
* [Azure Storage-Beispiele mit Java](../common/storage-samples-java.md)
