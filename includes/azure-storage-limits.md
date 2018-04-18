---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/03/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6381f8f0e68853183fc3e17e76b4ab93b152b48b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
| Ressource | Standardlimit |
| --- | --- |
| Anzahl von Speicherkonten pro Region | 200<sup>1</sup> |
| Maximale Speicherkontokapazität | 500 TiB<sup>2</sup> |
| Maximale Anzahl an Blob-Containern, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Meldungen pro Speicherkonto | Keine Begrenzung |
| Maximale Anforderungsrate pro Speicherkonto | 20.000 Anforderungen pro Sekunde<sup>2</sup> |
| Max. Eingang<sup>3</sup> pro Speicherkonto (US-Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 20 GBit/s für LRS/ZRS<sup>4</sup> |
| Max. Ausgang<sup>3</sup> pro Speicherkonto (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS, 30 GBit/s für LRS/ZRS<sup>4</sup> |
| Max. Eingang<sup>3</sup> pro Speicherkonto (US-fremde Regionen) | 5 GBit/s bei aktiviertem RA-GRS/GRS, 10 GBit/s für LRS/ZRS<sup>4</sup> |
| Max. Ausgang<sup>3</sup> pro Speicherkonto (US-fremde Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 15 GBit/s für LRS/ZRS<sup>4</sup> |

<sup>1</sup> Dies umfasst sowohl Standard- als auch Premium-Speicherkonten. Wenn Sie mehr als 200 Speicherkonten benötigen, stellen Sie eine Anfrage an den [Azure-Support](https://azure.microsoft.com/support/faq/). Das Azure Storage-Team überprüft Ihr Geschäftsszenario und kann bis zu 250 Speicherkonten genehmigen. 

<sup>2</sup> Wenn Sie erweiterte Grenzwerte für Ihr Speicherkonto benötigen, wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/). Das Azure Storage-Team prüft Ihre Anforderung und entscheidet von Fall zu Fall, ob eine Erhöhung gewährt wird. Sowohl Universal- als auch Blobspeicherkonten unterstützen eine erhöhte Kapazität, eingehende/ausgehende Daten und eine Anforderungsrate auf Anfrage. Die neuen Höchstwerte für Blobspeicherkonten finden Sie im Blog zur [Ankündigung größerer Speicherkonten mit größerer Skalierung](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> Die einzigen Beschränkungen sind die Grenzwerte für Kontoeingang und -ausgang. *Eingang* bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. *Ausgang* bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.  

<sup>4</sup> Redundanzoptionen für Azure Storage sind:
* **RA-GRS**: Georedundanter Speicher mit Lesezugriff (RA-GRS) Wenn RA-GRS aktiviert ist, sind die Ausgangsziele für den sekundären Standort mit denen für den primären Standort identisch.
* **GRS**: Georedundanter Speicher 
* **ZRS**: Zonenredundanter Speicher
* **LRS**: Lokal redundanter Speicher 
