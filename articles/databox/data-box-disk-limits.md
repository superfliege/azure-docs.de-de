---
title: Begrenzungen für Azure Data Box-Datenträger | Microsoft-Dokumentation
description: Beschreibt Systembegrenzungen und empfohlene Größen für den Microsoft Azure Data Box-Datenträger.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 01/09/2019
ms.author: alkohli
ms.openlocfilehash: 412727d79c194172f2855d014d1eaf18f44167f6
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159348"
---
# <a name="azure-data-box-disk-limits"></a>Begrenzungen für Azure Data Box Disk


Berücksichtigen Sie die folgenden Einschränkungen beim Bereitstellen und Betreiben Ihrer Microsoft Azure Data Box-Datenträgerlösung. 

## <a name="data-box-service-limits"></a>Data Box-Diensteinschränkungen

 - Der Data Box-Dienst ist nur in den USA, Europa, Kanada und Australien in allen Azure-Regionen für die öffentliche Azure-Cloud verfügbar.
 - Ein einzelnes Speicherkonto wird mit dem Data Box-Datenträger unterstützt.

## <a name="data-box-disk-performance"></a>Leistung des Data Box-Datenträgers

Beim Testen mit Datenträgern, die über USB 3.0 verbunden waren, betrug die Datenträgerleistung bis zu 430MB/s. Die tatsächlichen Zahlen variieren abhängig von der Größe der verwendeten Datei. Bei kleineren Dateien stellen Sie möglicherweise eine geringere Leistung fest.

## <a name="azure-storage-limits"></a>Speichergrenzwerte für Azure

In diesem Abschnitt werden die Grenzwerte für den Azure Storage-Dienst und die erforderlichen Benennungskonventionen für Azure Files, Azure-Blockblobs und Azure-Seitenblobs beschrieben, sofern mit dem Data Box-Dienst verfügbar. Machen Sie sich sorgfältig mit den Speichergrenzwerten vertraut, und beachten Sie alle Empfehlungen.

Aktuelle Informationen zu Grenzwerten für den Azure Storage-Dienst und bewährte Vorgehensweisen für die Benennung von Freigaben, Containern und Dateien finden Sie hier:

- [Benennen und Referenzieren von Containern, Blobs und Metadaten](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Verstehen von Blockblobs, Anfügeblobs und Seitenblobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Wenn Dateien oder Verzeichnisse die Grenzwerte des Azure Storage-Diensts überschreiten oder nicht den Benennungskonventionen für Azure Files/-Blobs entsprechen, werden diese Dateien oder Verzeichnisse nicht über den Data Box-Dienst in Azure Storage erfasst.

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

- Kopieren Sie Daten nicht direkt auf die Datenträger. Kopieren Sie Daten in die zuvor erstellten Ordner *BlockBlob* und *PageBlob*.
- Ein Ordner unter *BlockBlob* und *PageBlob* ist ein Container. Container werden beispielsweise als *BlockBlob/container* und *PageBlob/container* erstellt.
- Wenn Sie bereits über ein Azure-Objekt (z.B. ein Blob) in der Cloud verfügen, das den gleichen Namen hat wie das Objekt, das kopiert wird, überschreibt der Data Box-Datenträger die Datei in der Cloud.
- Jede Datei, die in die Freigaben *BlockBlob* und *PageBlob* geschrieben wird, wird als Blockblob bzw. Seitenblob hochgeladen.
- Eine unter den Ordnern *BlockBlob* und *PageBlob* erstellte leere Verzeichnishierarchie (ohne jegliche Dateien) wird nicht hochgeladen.
- Wenn beim Hochladen von Daten in Azure Fehler auftreten, wird im Zielspeicherkonto ein Fehlerprotokoll erstellt. Der Pfad zu diesem Fehlerprotokoll ist im Portal verfügbar, wenn der Upload abgeschlossen ist, und Sie können das Protokoll überprüfen, um Korrekturmaßnahmen zu ergreifen. Löschen Sie keine Daten aus der Quelle, ohne die hochgeladenen Daten zu überprüfen.

## <a name="azure-storage-account-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto

Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Die neuesten Informationen zu diesen Einschränkungen finden Sie unter [Skalierbarkeitsziele für Azure-Blobspeicher](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) und [Skalierbarkeitsziele für Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | 500TB pro Speicherkonto. <br> Dies schließt Daten aus allen Quellen einschließlich Data Box-Datenträger ein.|


## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

Hier sind die Größen der Azure-Objekte, die geschrieben werden können. Stellen Sie sicher, dass alle Dateien, die hochgeladen werden, diesen Einschränkungen entsprechen.

| Azure-Objekttyp | Standardlimit                                             |
|-------------------|-----------------------------------------------------------|
| Blockblob        | ~ 8TB                                                 |
| Seitenblob         | 1 TB <br> (Jede Datei, die im Seitenblobformat hochgeladen wird, muss einem ganzzahligen Vielfachen von 512Bytes entsprechen, andernfalls tritt beim Hochladen ein Fehler auf. <br> VHD- und VHDX-Dateien entsprechen einem ganzzahligen Vielfachen von 512Bytes.) |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Benennungskonventionen für Azure-Blockblobs und -Seitenblobs

| Entität                                       | Konventionen                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Containernamen für Blockblob und Seitenblob | Es muss sich um einen gültigen DNS-Namen mit einer Länge von 3 bis 63 Zeichen handeln. <br>  Sie muss mit einem Buchstaben oder einer Zahl beginnen. <br> Nur Kleinbuchstaben, Zahlen und Bindestriche (-) sind zulässig. <br> Vor und nach jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer stehen. <br> Aufeinanderfolgende Bindestriche sind in Namen nicht zulässig. |
| Blobnamen für Blockblobs und Seitenblobs      | Für Blobnamen wird die Groß-/Kleinschreibung beachtet, und sie können eine beliebige Kombination von Zeichen enthalten. <br> Ein Blobname muss zwischen 1 und 1.024 Zeichen lang sein. <br> Reservierte URL-Zeichen müssen korrekt mit Escapezeichen versehen sein. <br>Die Anzahl von Pfadsegmenten, aus denen der Blobname besteht, darf 254 nicht überschreiten. Ein Pfadsegment ist die Zeichenfolge zwischen aufeinanderfolgenden Trennzeichen (z.B. der Schrägstrich (/)), die für den Namen eines virtuellen Verzeichnisses steht. |


## <a name="next-steps"></a>Nächste Schritte
* Überprüfen der [Systemanforderungen für Data Box](data-box-system-requirements.md)
