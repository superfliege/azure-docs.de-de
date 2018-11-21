---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/23/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 84333b26ac70db4b400f7236d4255f4b57a6ca7d
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572185"
---
In der folgenden Tabelle werden die für die Azure Storage geltenden Standardgrenzwerte beschrieben. Der Grenzwert für *Eingang* bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. Der Grenzwert für *Ausgang* bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.

| Ressource | Standardlimit |
| --- | --- |
| Anzahl von Speicherkonten pro Region und Abonnement, einschließlich Standard- und Premium-Konten. | 250 |
| Maximale Speicherkontokapazität | 2 PB für USA und Europa, 500 TB für alle anderen Regionen, einschließlich Vereinigtes Königreich |
| Maximale Anzahl an Blob-Containern, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Meldungen pro Speicherkonto | Keine Begrenzung |
| Maximale Anforderungsrate<sup>1</sup> pro Speicherkonto | 20.000 Anforderungen pro Sekunde |
| Max. Eingang<sup>1</sup> pro Speicherkonto (US-Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 20 GBit/s für LRS/ZRS<sup>2</sup> |
| Max. Eingang<sup>1</sup> pro Speicherkonto (US-fremde Regionen) | 5 GBit/s bei aktiviertem RA-GRS/GRS, 10 GBit/s für LRS/ZRS<sup>2</sup> |
| Max. Ausgang für universelle v2-Speicherkonten und Blobspeicherkonten (alle Regionen) | 50 GBit/s |
| Max. Ausgang für universelle v1-Speicherkonten (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS, 30 GBit/s für LRS/ZRS <sup>2</sup> |
| Max. Ausgang für universelle v1-Speicherkonten (Nicht-US-Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 15 GBit/s für LRS/ZRS <sup>2</sup> |

<sup>1</sup> Azure-Speicherkonten unterstützen höhere Grenzwerte für Eingang auf Anforderung. Wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/), um eine Erhöhung der Kontogrenzwerte für Eingang anzufordern.

<sup>2</sup>[Für Azure Storage sind folgende Replikationsoptionen](https://docs.microsoft.com/azure/storage/common/storage-redundancy) verfügbar:
* **RA-GRS**: Georedundanter Speicher mit Lesezugriff (RA-GRS) Wenn RA-GRS aktiviert ist, sind die Ausgangsziele für den sekundären Standort mit denen für den primären Standort identisch.
* **GRS**: Georedundanter Speicher 
* **ZRS**: Zonenredundanter Speicher
* **LRS**: Lokal redundanter Speicher 

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, können Sie die Anwendung so erstellen, dass mehrere Speicherkonten verwendet werden. Sie können Ihre Datenobjekte dann basierend auf diesen Speicherkonten partitionieren. Informationen zu Volumenpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

Alle Speicherkonten werden in einer flachen Netzwerktopologie ausgeführt und unterstützen die in diesem Artikel beschriebenen Skalierbarkeits- und Leistungsziele, unabhängig vom Zeitpunkt ihrer Erstellung. Weitere Informationen zur flachen Netzwerkarchitektur von Azure Storage sowie zur Skalierbarkeit finden Sie unter [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)(in englischer Sprache).

