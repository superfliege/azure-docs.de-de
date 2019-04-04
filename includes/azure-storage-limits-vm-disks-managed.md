---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 12/12/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3ad66d235e00d9866794ca90be9934e174bf4102
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553687"
---
**Verwaltete HDD Standard-Datenträger**

| Standard-Datenträgertyp  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60*             | S70*             | S80*             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB          | 32             | 64             | 128            | 256  | 512            | 1024    | 2.048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOPS pro Datenträger       | Bis zu 500              | Bis zu 500              | Bis zu 500              | Bis zu 500 | Bis zu 500              | Bis zu 500              | Bis zu 500             | Bis zu 500              | Bis zu 1.300              | Bis zu 2.000              | Bis zu 2.000              |
| Durchsatz pro Datenträger | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s| Bis zu 300 MiB/s | Bis zu 500 MiB/s | Bis zu 500 MiB/s |

**Verwaltete SSD Standard-Datenträger**

| Standard-SSD-Datenträgertyp | E4                | E6                | E10               | E15               | E20             | E30              | E40              | E50              | E60*             | E70*             | E80*             |
|------------------------|-------------------|-------------------|-------------------|-------------------|-----------------|------------------|------------------|------------------|-------------------|-------------------|-------------------|
| Datenträgergröße in GiB       | 32                | 64                | 128               | 256               | 512             | 1024            | 2.048            | 4.095            | 8.192             | 16.384            | 32.767            |
| IOPS pro Datenträger          | Bis zu 120         | Bis zu 240         | Bis zu 500         | Bis zu 500         | Bis zu 500       | Bis zu 500        | Bis zu 500        | Bis zu 500        | Bis zu 1.300       | Bis zu 2.000       | Bis zu 2.000       |
| Durchsatz pro Datenträger    | Bis zu 25 MB/s   | Bis zu 50 MB/s   | Bis zu 60 MB/s   | Bis zu 60 MB/s   | Bis zu 60 MB/s | Bis zu 60 MB/s  | Bis zu 60 MB/s  | Bis zu 60 MB/s  | Bis zu 300 MiB/s | Bis zu 500 MiB/s | Bis zu 500 MiB/s |

**Verwaltete SSD Premium-Datenträger: Grenzwerte pro Datenträger**

| Premium-Datenträgertyp  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60*             | P70*             | P80*             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB           | 32             | 64             | 128            | 256  | 512            | 1024    | 2.048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOPS pro Datenträger       | Bis zu 120 | Bis zu 240              | Bis zu 500              | Bis zu 1.100 | Bis zu 2.300              | Bis zu 5.000              | Bis zu 7.500             | Bis zu 7.500              | Bis zu 12.500              | Bis zu 15.000              | Bis zu 20.000              |
| Durchsatz pro Datenträger | Bis zu 25 MiB/s | Bis zu 50 MiB/s | Bis zu 100 MiB/s | Bis zu 125 MiB/s | Bis zu 150 MiB/s | Bis zu 200 MiB/s | Bis zu 250 MiB/s | Bis zu 250 MiB/s| Bis zu 480 MiB/s | Bis zu 750 MiB/s | Bis zu 750 MiB/s |

**Verwaltete SSD Premium-Datenträger: Grenzwerte pro VM**

| Ressource | Standardlimit |
| --- | --- |
| Maximale Anzahl IOPS pro VM |80.000 IOPS mit GS5-VM |
| Maximaler Durchsatz pro VM |2.000 MB/s mit GS5-VM |
