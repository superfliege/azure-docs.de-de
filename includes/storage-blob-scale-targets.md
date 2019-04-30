---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/20/2019
ms.author: tamram
ms.openlocfilehash: d96f400332b7953b34a157b3b52cf00bb20db76e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012521"
---
| Ressource | Ziel        |
|----------|---------------|
| Maximale Größe eines einzelnen Blobcontainers | Identisch mit maximaler Speicherkontokapazität |
| Maximale Anzahl von Blöcken in einem Blockblob oder einem Anfügeblob | 50.000 Blöcke |
| Maximale Größe eines Blocks in einem Blockblob | 100 MiB |
| Maximale Größe eines Blockblobs | 50.000 × 100MiB (ca. 4,75TiB) |
| Maximale Größe eines Blocks in einem Anfügeblob | 4 MiB |
| Maximale Größe eines Anfügeblobs | 50.000 × 4MiB (ca. 195GiB) |
| Max. Größe eines Seitenblobs | 8 TiB |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Blobcontainer | 5 |
|Zieldurchsatz bei Einzel-Blob |Bis zur Eingangs-/Ausgangsbegrenzung des Speicherkontos<sup>1</sup> |

<sup>1</sup> Einzelobjektdurchsatz ist abhängig von mehreren Faktoren einschließlich, aber nicht beschränkt auf: Parallelität, Anforderungsgröße, Leistungsstufe, Geschwindigkeit der Quelle bei Uploads und Downloadziel. Um den Vorteil von Leistungsverbesserungen durch [Blockblobs mit hohem Durchsatz](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) zu nutzen, verwenden Sie eine Put Blob- oder Put Block-Anforderungsgröße von > 4MiB (> 256MiB für Premium-Leistungs-Blockblobspeicher oder Data Lake Storage Gen2).