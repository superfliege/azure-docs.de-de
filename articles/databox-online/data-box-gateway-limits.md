---
title: Für Azure Data Box Gateway geltende Einschränkungen | Microsoft-Dokumentation
description: Beschreibt für das System geltende Einschränkungen und empfohlene Größen für Microsoft Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/15/2019
ms.author: alkohli
ms.openlocfilehash: f01fb88bf0ea726b421111a262bdfdd68cd3d38c
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388424"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Für Azure Data Box Gateway geltende Einschränkungen (Vorschauversion)


Berücksichtigen Sie die folgenden Einschränkungen beim Bereitstellen und Betreiben Ihrer Microsoft Azure Data Box Gateway-Lösung. 

> [!IMPORTANT] 
> Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Vertragsbedingungen für die Nutzung der Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen. 


## <a name="data-box-gateway-service-limits"></a>Für den Data Box Gateway-Dienst geltende Einschränkungen

- In dieser Version ist der Dienst nur in bestimmten Regionen in den USA, Europa und Asien-Pazifik verfügbar. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](#data-box-gateway-overview#region-availability). Das Speicherkonto sollte sich physisch in der Nähe der Region befinden, in der das Gerät bereitgestellt wird (diese kann sich von der Region des Diensts unterscheiden).
- Das Verschieben einer Data Box Gateway-Ressource in ein anderes Abonnement oder eine andere Ressourcengruppe wird nicht unterstützt. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="data-box-gateway-device-limits"></a>Für das Data Box Gateway-Gerät geltende Einschränkungen

In der folgenden Tabelle werden die für das Data Box Gateway-Gerät geltenden Einschränkungen beschrieben.

| BESCHREIBUNG | Wert |
|---|---|
| Nein. der Dateien pro Gerät |100 Mio. <br> Der Grenzwert liegt bei ca. 25 Mio. Dateien pro 2 TB Datenträgerkapazität mit einer Obergrenze von 100 Mio. |
| Nein. der Freigaben pro Gerät |24 |
|Maximale in eine Freigabe geschriebene Dateigröße|Für ein virtuelles Gerät mit 2 TB beträgt die maximale Dateigröße 500 GB. <br> Die maximale Dateigröße steigt mit der Größe des Datenträgers im vorhergehenden Verhältnis an, bis ein Maximum von 5 TB erreicht wird. |

## <a name="azure-storage-limits"></a>Speichergrenzwerte für Azure

In diesem Abschnitt werden die geltenden Einschränkungen für den Azure Storage-Dienst und die Benennungskonventionen für Azure Files, Azure-Blockblobs und Azure-Seitenblobs beschrieben, sofern für den Data Box Gateway-/Data Box Edge-Dienst zutreffend. Machen Sie sich sorgfältig mit den Speichergrenzwerten vertraut, und beachten Sie alle Empfehlungen.

Aktuelle Informationen zu Grenzwerten für den Azure Storage-Dienst und bewährte Vorgehensweisen für die Benennung von Freigaben, Containern und Dateien finden Sie hier:

- [Benennen und Referenzieren von Containern, Blobs und Metadaten](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Verstehen von Blockblobs, Anfügeblobs und Seitenblobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Wenn Dateien oder Verzeichnisse die Grenzwerte des Azure Storage-Diensts überschreiten oder nicht den Benennungskonventionen für Azure Files/-Blobs entsprechen, werden diese Dateien oder Verzeichnisse nicht über den Data Box Gateway-/Data Box Edge-Dienst in Azure Storage erfasst.

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

Die folgenden Einschränkungen gelten für Daten, die in Azure verschoben werden.

- Es wird empfohlen, dass nicht mehr als ein Gerät in den gleichen Container schreibt.
- Wenn Sie bereits über ein Azure-Objekt (z.B. Blob oder Datei) in der Cloud verfügen, das den gleichen Namen hat wie das Objekt, das kopiert wird, überschreibt das Gerät die Datei in der Cloud.
- Eine unter Freigabeordnern erstellte leere Verzeichnishierarchie (ohne jegliche Dateien) wird nicht in die Blobcontainer hochgeladen.
- Wenn Sie Dateien kopieren, die größer als das Gerät sind, sollten Sie *Robocopy* oder *rsync* verwenden, um sicherzustellen, dass keine Fehler enthalten sind.

## <a name="azure-storage-account-size-and-object-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto und Objekte

Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Die neuesten Informationen zu diesen Einschränkungen finden Sie unter [Skalierbarkeitsziele für Azure-Blobspeicher](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) und [Skalierbarkeitsziele für Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | 500 TB pro Speicherkonto|


## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

Hier sind die Größen der Azure-Objekte, die geschrieben werden können. Stellen Sie sicher, dass alle Dateien, die hochgeladen werden, diesen Einschränkungen entsprechen.

| Azure-Objekttyp | Standardlimit                                             |
|-------------------|-----------------------------------------------------------|
| Blockblob        | ~ 8TB                                                 |
| Seitenblob         | 1 TB <br> Jede Datei, die im Seitenblobformat hochgeladen wird, muss einem ganzzahligen Vielfachen von 512 Bytes entsprechen. Andernfalls tritt beim Hochladen ein Fehler auf. <br> VHD- und VHDX-Dateien entsprechen einem ganzzahligen Vielfachen von 512 Bytes. |


## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
