---
title: "Azure PowerShell-Skriptbeispiel – Berechnen der Größe des Blobcontainers | Microsoft-Dokumentation"
description: "Berechnen Sie die Größe eines Containers in Azure Blob Storage, indem Sie die Größe der einzelnen Blobs addieren."
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 10/23/2017
ms.author: fryu
ms.openlocfilehash: 46f3eac0129da41062caba2da090f9e532505d67
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Berechnen der Größe eines Blob Storage-Containers

Mit diesem Skript berechnen Sie die Größe eines Containers in Azure Blob Storage, indem Sie die Größe der Blobs im Container addieren.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="understand-the-size-of-blob-storage-container"></a>Grundlegendes zur Größe eines Blob Storage-Containers

Die Gesamtgröße des Blob Storage-Containers schließt die Größe des Containers selbst und die Größe aller Blobs unter dem Container ein.

Im Folgenden wird beschrieben, wie die Speicherkapazität für Blobcontainer und Blobs berechnet wird. Nachstehend steht Len(X) für die Anzahl der Zeichen in der Zeichenfolge.

### <a name="blob-containers"></a>Blobcontainer

Es folgt eine Schätzung des pro Blobcontainer belegten Speicherplatzes:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Die Aufteilung ist wie folgt:
* Die 48 Bytes für den Verarbeitungsaufwand für jeden Container setzen sich aus der Uhrzeit der letzten Änderung, Berechtigungen, öffentlichen Einstellungen und einigen Systemmetadaten zusammen.
* Der Containername wird als Unicode gespeichert, weshalb Sie die Anzahl der Zeichen mit 2 multiplizieren müssen.
* Für alle für den Blobcontainer gespeicherten Metadaten die Länge des (als ASCII gespeicherten) Namens plus die Länge des Zeichenfolgenwerts.
* Die 512 Bytes pro signiertem Bezeichner umfassen den Namen des signierten Bezeichners, die Startzeit und Ablaufzeit sowie Berechtigungen.

### <a name="blobs"></a>Blobs

Es folgt eine Schätzung des pro Blob belegten Speicherplatzes:

* Blockblob (Basisblob oder Momentaufnahme)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
SizeInBytes(data in unique committed data blocks stored) +
SizeInBytes(data in uncommitted data blocks)
`

* Seitenblob (Basisblob oder Momentaufnahme)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
number of nonconsecutive page ranges with data * 12 bytes +
SizeInBytes(data in unique pages stored)
`

Die Aufteilung ist wie folgt:

* Die 124 Bytes für den Verarbeitungsaufwand für jeden Blob setzen sich aus den Angaben für die Uhrzeit der letzten Änderung, Cachesteuerung, Inhaltstyp, Inhaltssprache, Inhaltscodierung, Content-MD5, Berechtigungen, Momentaufnahmeninformationen, Lease und einigen Systemmetadaten zusammen.
* Der Blobname wird als Unicode gespeichert, weshalb Sie die Anzahl der Zeichen mit 2 multiplizieren müssen.
* Dann für alle gespeicherten Metadaten die Länge des (als ASCII gespeicherten) Namens plus die Länge des Zeichenfolgenwerts.
* Dann für Blockblobs
    * 8 Bytes für die Blockliste
    * Anzahl der Blöcke mal Größe der Block-ID in Bytes
    * Plus die Größe der Daten in allen Blöcken mit und ohne Commit. Hinweis: Wenn Momentaufnahmen verwendet werden, enthält diese Größe nur die eindeutigen Daten für diesen Basis- oder Momentaufnahmenblob. Wenn der Blöcke ohne Commit nach einer Woche nicht verwendet werden, werden sie vom Garbage Collector erfasst. Ab diesem Zeitpunkt werden sie nicht mehr in Rechnung gestellt.
* Dann für Seitenblobs
    * Anzahl nicht aufeinanderfolgender Seitenbereiche mit Daten mal 12 Byte. Dies ist die Anzahl eindeutiger Seitenbereiche, die Sie beim Aufruf der „GetPageRanges“-API sehen.
    * Plus die Größe der Daten in Bytes aller gespeicherten Seiten. Hinweis: Wenn Momentaufnahmen verwendet werden, enthält diese Größe nur die eindeutigen Seiten für den gezählten Basis- oder Momentaufnahmenblob.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Abrechnung von Azure Storage finden Sie im Blogbeitrag [Understanding Azure Storage Billing - Bandwidth, Transactions, and Capacity](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/)(Grundlagen zur Abrechnung von Azure-Speicher – Bandbreite, Transaktionen und Kapazität, in englischer Sprache).

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für Speicher finden Sie in den [PowerShell-Beispielen für Azure Storage](../blobs/storage-samples-blobs-powershell.md).
