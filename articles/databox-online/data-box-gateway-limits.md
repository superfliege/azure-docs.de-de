---
title: Für Azure Data Box Gateway geltende Einschränkungen | Microsoft-Dokumentation
description: Beschreibt für das System geltende Einschränkungen und empfohlene Größen für Microsoft Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/14/2019
ms.author: alkohli
ms.openlocfilehash: 34aac9a589516ace080906095ef3c14b34469bbd
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311187"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Für Azure Data Box Gateway geltende Einschränkungen (Vorschauversion)


Berücksichtigen Sie die folgenden Einschränkungen beim Bereitstellen und Betreiben Ihrer Microsoft Azure Data Box Gateway-Lösung. 

> [!IMPORTANT] 
> Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Vertragsbedingungen für die Nutzung der Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen. 


## <a name="data-box-gateway-service-limits"></a>Für den Data Box Gateway-Dienst geltende Einschränkungen

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-gateway-device-limits"></a>Für das Data Box Gateway-Gerät geltende Einschränkungen

In der folgenden Tabelle werden die für das Data Box Gateway-Gerät geltenden Einschränkungen beschrieben.

| BESCHREIBUNG | Wert |
|---|---|
| Nein. der Dateien pro Gerät |100 Mio. <br> Der Grenzwert liegt bei ca. 25 Mio. Dateien pro 2 TB Datenträgerkapazität mit einer Obergrenze von 100 Mio. |
| Nein. der Freigaben pro Gerät |24 |
| Nein. von Freigaben pro Azure-Speichercontainer |1 |
|Maximale in eine Freigabe geschriebene Dateigröße|Für ein virtuelles Gerät mit 2 TB beträgt die maximale Dateigröße 500 GB. <br> Die maximale Dateigröße steigt mit der Größe des Datenträgers im vorhergehenden Verhältnis an, bis ein Maximum von 5 TB erreicht wird. |

## <a name="azure-storage-limits"></a>Speichergrenzwerte für Azure

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto und Objekte

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]

## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
