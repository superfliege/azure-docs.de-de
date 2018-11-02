---
title: Für Azure Data Box geltende Einschränkungen | Microsoft-Dokumentation
description: Beschreibt für das System geltende Einschränkungen und empfohlene Größen für Microsoft Azure Data Box-Komponenten und -Verbindungen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 10/10/2018
ms.author: alkohli
ms.openlocfilehash: de47cae219aa457343df292bb91b6af06c4b1186
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091928"
---
# <a name="azure-data-box-limits"></a>Für Azure Data Box geltende Einschränkungen

Berücksichtigen Sie die folgenden Einschränkungen beim Bereitstellen und Betreiben Ihrer Microsoft Azure Data Box. In der folgenden Tabelle werden die für die Data Box geltenden Einschränkungen beschrieben.


## <a name="data-box-service-limits"></a>Data Box-Diensteinschränkungen

 - Der Data Box-Dienst ist nur in den USA in allen [Azure-Regionen für die öffentliche Azure-Cloud](https://azure.microsoft.com/regions/) verfügbar.
 - Wenn Sie mehrere Speicherkonten mit dem Data Box-Dienst nutzen, müssen alle Speicherkonten zur gleichen Azure-Region gehören.
 - Es wird empfohlen, nicht mehr als drei Speicherkonten zu verwenden. Weitere Speicherkonten können möglicherweise die Leistung beeinträchtigen.

## <a name="data-box-limits"></a>Für Data Box geltende Einschränkungen

- In Data Box können maximal 500 Millionen Dateien gespeichert werden.

## <a name="azure-storage-limits"></a>Speichergrenzwerte für Azure

In diesem Abschnitt werden die Grenzwerte für den Azure Storage-Dienst und die erforderlichen Benennungskonventionen für Azure Files, Azure-Blockblobs und Azure-Seitenblobs beschrieben, sofern mit dem Data Box-Dienst verfügbar. Machen Sie sich sorgfältig mit den Speichergrenzwerten vertraut, und beachten Sie alle Empfehlungen.

Aktuelle Informationen zu Grenzwerten für den Azure Storage-Dienst und bewährte Vorgehensweisen für die Benennung von Freigaben, Containern und Dateien finden Sie hier:

- [Benennen und Referenzieren von Containern, Blobs und Metadaten](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Verstehen von Blockblobs, Anfügeblobs und Seitenblobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Wenn Dateien oder Verzeichnisse die Grenzwerte des Azure Storage-Diensts überschreiten oder nicht den Benennungskonventionen für Azure Files/-Blobs entsprechen, werden diese Dateien oder Verzeichnisse nicht über den Data Box-Dienst in Azure Storage erfasst.

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

- Kopieren Sie keine Daten direkt unter die zuvor erstellten Freigaben. Sie müssen zunächst einen Ordner unter der Freigabe erstellen und dann Daten in diesen Ordner kopieren.
- Ein Ordner unter *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* ist ein Container. Container werden beispielsweise als *StorageAccount_BlockBlob/container* und *StorageAccount_PageBlob/container* erstellt.
- Jeder Ordner, der direkt unter *StorageAccount_AzureFiles* erstellt wird, wird in eine Azure-Dateifreigabe umgewandelt.
- Wenn Sie bereits über ein Azure-Objekt (z.B. Blob oder Datei) in der Cloud verfügen, das den gleichen Namen hat wie das Objekt, das kopiert wird, überschreibt Data Box die Datei in der Cloud.
- Jede Datei, die in die Freigaben *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* geschrieben wird, wird als Blockblob bzw. Seitenblob hochgeladen.
- Azure Blob Storage unterstützt keine Verzeichnisse. Wenn Sie einen Ordner unter dem Ordner *StorageAccount_BlockBlob* erstellen, werden im Blobnamen virtuelle Ordner erstellt. Für Azure Files wird die tatsächliche Verzeichnisstruktur beibehalten.
- Eine unter den Ordnern *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* erstellte leere Verzeichnishierarchie (ohne jegliche Dateien) wird nicht hochgeladen.
- Wenn beim Hochladen von Daten in Azure Fehler auftreten, wird im Zielspeicherkonto ein Fehlerprotokoll erstellt. Der Pfad zu diesem Fehlerprotokoll ist verfügbar, sobald das Hochladen abgeschlossen ist, und Sie können das Protokoll überprüfen, um Korrekturmaßnahmen zu ergreifen. Löschen Sie keine Daten aus der Quelle, ohne die hochgeladenen Daten zu überprüfen.

## <a name="azure-storage-account-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto

Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Die neuesten Informationen zu diesen Einschränkungen finden Sie unter [Skalierbarkeitsziele für Azure-Blobspeicher](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) und [Skalierbarkeitsziele für Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | 500 TiB pro Speicherkonto. <br> Dies schließt Daten aus allen Quellen einschließlich Data Box ein.|
| Azure File                                                          | 5 TiB pro Freigabe.<br> Alle Ordner unter *StorageAccount_AzureFiles* müssen diese Beschränkung einhalten.       |

## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

Hier sind die Größen der Azure-Objekte, die geschrieben werden können. Stellen Sie sicher, dass alle Dateien, die hochgeladen werden, diesen Einschränkungen entsprechen.

| Azure-Objekttyp | Standardlimit                                             |
|-------------------|-----------------------------------------------------------|
| Blockblob        | Ca. 4,75 TiB                                                 |
| Seitenblob         | 8 TiB <br> Jede Datei, die im Seitenblobformat hochgeladen wird, muss einem ganzzahligen Vielfachen von 512 Bytes entsprechen. Andernfalls tritt beim Hochladen ein Fehler auf. <br> VHD- und VHDX-Dateien entsprechen einem ganzzahligen Vielfachen von 512 Bytes. |
| Azure File        | 1 TiB                                                      |

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Benennungskonventionen für Azure-Blockblobs, -Seitenblobs und -Dateien

| Entität                                       | Konventionen                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Containernamen für Blockblob und Seitenblob | Es muss sich um einen gültigen DNS-Namen mit einer Länge von 3 bis 63 Zeichen handeln. <br>  Sie muss mit einem Buchstaben oder einer Zahl beginnen. <br> Nur Kleinbuchstaben, Zahlen und Bindestriche (-) sind zulässig. <br> Vor und nach jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer stehen. <br> Aufeinanderfolgende Bindestriche sind in Namen nicht zulässig. |
| Freigabenamen für Azure-Dateien                  | Wie oben                                                                                                                                                                                                                                                                                                             |
| Verzeichnis- und Dateinamen für Azure-Dateien     |<li> Groß-/Kleinschreibung wird beibehalten, aber nicht berücksichtigt, maximal 255 Zeichen. </li><li> Schrägstrich (/) als letztes Zeichen nicht erlaubt. </li><li>Falls eingegeben, erfolgt eine automatische Entfernung. </li><li> Folgende Zeichen sind nicht zulässig: `" `" \ / : | < > * ?`</li><li> Reservierte URL-Zeichen müssen korrekt mit Escapezeichen versehen sein. </li><li> Ungültige Zeichen im URL-Pfad sind unzulässig. Codepunkte wie „\uE000“ sind keine gültigen Unicode-Zeichen. Einige ASCII- und Unicode-Zeichen, z.B. Steuerzeichen (0x00 bis 0x1F, \u0081 usw.), sind ebenfalls unzulässig. Regeln für Unicode-Zeichenfolgen in HTTP/1.1 finden Sie unter „RFC 2616, Section 2.2: Basic Rules“ und „RFC 3987“. </li><li> Die folgenden Dateinamen sind unzulässig: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, Punkt (.) und zwei Punkte (..).</li>|
| Blobnamen für Blockblobs und Seitenblobs      | </li><li>Für Blobnamen wird die Groß-/Kleinschreibung beachtet, und sie können eine beliebige Kombination von Zeichen enthalten. </li><li>Ein Blobname muss zwischen 1 und 1.024 Zeichen lang sein. </li><li>Reservierte URL-Zeichen müssen korrekt mit Escapezeichen versehen sein. </li><li>Die Anzahl von Pfadsegmenten, aus denen der Blobname besteht, darf 254 nicht überschreiten. Ein Pfadsegment ist die Zeichenfolge zwischen aufeinanderfolgenden Trennzeichen (z.B. der Schrägstrich (/)), die für den Namen eines virtuellen Verzeichnisses steht.</li> |
