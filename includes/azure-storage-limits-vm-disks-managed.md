---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: dcd94cbd7963c13bd739b1b004fe5ac506d3ea9d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006487"
---
**Verwaltete Standard-VM-Datenträger (HDDs)**

| Standard-Datenträgertyp  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60              | S70              | S80              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB          | 32             | 64             | 128            | 256  | 512            | 1024    | 2.048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOPS pro Datenträger       | Bis zu 500              | Bis zu 500              | Bis zu 500              | Bis zu 500 | Bis zu 500              | Bis zu 500              | Bis zu 500             | Bis zu 500              | Bis zu 1.300              | Bis zu 2000              | Bis zu 2.000              |
| Durchsatz pro Datenträger | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s| Bis zu 300 MiB/s | Bis zu 500 MiB/s | Bis zu 500 MiB/s |

**Verwaltete Standard-VM-SSDs**

| Standard-SSD-Datenträgertyp  | E10               | E15               | E20             | E30 | E40              | E50              | E60              | E70              | E80              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB           | 128             | 256             | 512            | 1024  | 2.048            | 4.095     | 8.192     | 16.384     | 32.767    |
| IOPS pro Datenträger       | Bis zu 500              | Bis zu 500              | Bis zu 500              | Bis zu 500 | Bis zu 500              | Bis zu 500              | Bis zu 500             | Bis zu 500              | Bis zu 1.300              | Bis zu 2.000              | Bis zu 2.000              |
| Durchsatz pro Datenträger | Bis zu 60 MB/s | Bis zu 60 MB/s | Bis zu 60 MB/s | Bis zu 60 MB/s | Bis zu 60 MB/s | Bis zu 60 MB/s | Bis zu 60 MB/s | Bis zu 60 MB/s| Bis zu 300 MiB/s |  Bis zu 500 MiB/s | Bis zu 500 MiB/s |

**Verwalteter Premium-VM-Datenträger: Grenzwerte pro Datenträger**

| Premium-Datenträgertyp  | P4               | P6               | P10             | P15 | P20              | S30              | P40              | P50              | P60              | P70              | P80              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB           | 32             | 64             | 128            | 256  | 512            | 1024    | 2.048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOPS pro Datenträger       | Bis zu 120 | Bis zu 240              | Bis zu 500              | Bis zu 1.100 | Bis zu 2.300              | Bis zu 5.000              | Bis zu 7.500             | Bis zu 7.500              | Bis zu 12.500              | Bis zu 15.000              | Bis zu 20.000              |
| Durchsatz pro Datenträger | Bis zu 25 MiB/s | Bis zu 50 MiB/s | Bis zu 100 MiB/s | Bis zu 125 MiB/s | Bis zu 150 MiB/s | Bis zu 200 MiB/s | Bis zu 250 MiB/s | Bis zu 250 MiB/s| Bis zu 480 MiB/s | Bis zu 750 MiB/s | Bis zu 750 MiB/s |

**Verwalteter Premium-VM-Datenträger: Grenzwerte pro VM**

| Ressource | Standardlimit |
| --- | --- |
| Max. IOPS pro virtuellem Computer |80.000 IOPS mit GS5-VM |
| Max. Durchsatz pro virtuellem Computer |2.000 MB/s mit GS5-VM |
