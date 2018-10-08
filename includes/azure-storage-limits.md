---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a777151216cb70b696da088b8a0d34779ebc39
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396122"
---
In der folgenden Tabelle werden die für die Azure Storage geltenden Standardgrenzwerte beschrieben. Der Grenzwert für *Eingang* bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. Der Grenzwert für *Ausgang* bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.

| Ressource | Standardlimit |
| --- | --- |
| Anzahl von Speicherkonten pro Region und Abonnement, einschließlich Standard- und Premium-Konten. | 200 |
| Maximale Speicherkontokapazität<sup>1</sup> | 500 TiB |
| Maximale Anzahl an Blob-Containern, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Meldungen pro Speicherkonto | Keine Begrenzung |
| Maximale Anforderungsrate<sup>1</sup> pro Speicherkonto | 20.000 Anforderungen pro Sekunde |
| Max. Eingang<sup>1</sup> pro Speicherkonto (US-Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 20 GBit/s für LRS/ZRS<sup>2</sup> |
| Max. Ausgang<sup>1</sup> pro Speicherkonto (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS, 30 GBit/s für LRS/ZRS |
| Max. Eingang<sup>1</sup> pro Speicherkonto (US-fremde Regionen) | 5 GBit/s bei aktiviertem RA-GRS/GRS, 10 GBit/s für LRS/ZRS<sup>2</sup> |
| Max. Ausgang<sup>1</sup> pro Speicherkonto (US-fremde Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 15 GBit/s für LRS/ZRS |

<sup>1</sup> Azure-Speicherkonten unterstützen höhere Grenzwerte für Kapazität, Anforderungsrate, Ein- und Ausgang auf Anforderung. Weitere Informationen zu höheren Grenzwerten finden Sie unter [Ankündigung größerer Speicherkonten mit höherer Skalierung](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/). Wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/), um eine Erhöhung der Kontogrenzwerte anzufordern.

<sup>2</sup>[Für Azure Storage sind folgende Replikationsoptionen](https://docs.microsoft.com/azure/storage/common/storage-redundancy) verfügbar:
* **RA-GRS**: Georedundanter Speicher mit Lesezugriff (RA-GRS) Wenn RA-GRS aktiviert ist, sind die Ausgangsziele für den sekundären Standort mit denen für den primären Standort identisch.
* **GRS**: Georedundanter Speicher 
* **ZRS**: Zonenredundanter Speicher
* **LRS**: Lokal redundanter Speicher 

