---
title: Includedatei
description: Includedatei
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 11/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 5a4495dd675b662273715b5c13a5594adc87fceb
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56333777"
---
<!-- F-series, Fs-series* -->

Compute-optimierte VM-Größen weisen ein großes Verhältnis von CPU zu Arbeitsspeicher auf und eignen sich gut für Webserver mit mittlerem Durchsatz, Netzwerkappliances, Batchprozesse und Anwendungsserver. Dieser Artikel enthält Informationen zur Anzahl von vCPUs, Datenträgern und NICs sowie zum Speicherdurchsatz und zur Netzwerkbandbreite der einzelnen Größen in dieser Gruppe.

Die Fsv2-Serie basiert auf dem Prozessor Intel® Xeon® Platinum 8168 mit einer durchgehenden Turbotaktfrequenz von 3,4 GHz und einer maximalen Einzelkern-Turbofrequenz von 3,7 GHz. Intel® AVX-512-Anweisungen, die auf Intel Scalable Processors neu sind, können bei Gleitkommaoperationen mit einfacher und doppelter Genauigkeit mit Vektorverarbeitungeworkloads die Leistung verdoppeln. D.h., sie bewältigen alle Rechenworkloads wirklich schnell. 

Die Fsv2-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Compute-Einheit (Azure Compute Unit, ACU) das beste Preis-Leistungs-Verhältnis pro vCPU im Azure-Portfolio. 

Die F-Serie basiert auf dem Intel Xeon® E5-2673 v3-Prozessor (Haswell) mit 2,4 GHz, der mit der Intel Turbo Boost Technology 2.0 Taktfrequenzen von 3,1 GHz erreichen kann. Dies ist die gleiche CPU-Leistung wie bei virtuellen Computern (VMs) der Dv2-Serie.  

Virtuelle Computer der F-Serie sind eine hervorragende Wahl für Workloads, die schnellere CPUs erfordern, aber nicht so viel Arbeitsspeicher oder temporären Speicher pro vCPU benötigen.  Bei Arbeitslasten wie Analysen, Gamingservern, Webservern und Batchverarbeitung kommen die Vorteile und der Nutzen der F-Serie besonders gut zum Tragen.

Die Fs-Serie verfügt zusätzlich zum Premium-Speicher über alle Vorteile der F-Serie.

## <a name="fsv2-series-sup1sup"></a>Fsv2-Serie <sup>1</sup>

ACU: 195–210

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Größe             | vCPUs | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000/31 (32)           | 3200/47                | 2/875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000/63 (64)           | 6400/95                | 2/1.750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000/127 (128)        | 12800/190              | 4/3.500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000/255 (256)        | 25600/380              | 4/7.000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000/512 (512)        | 51200/750              | 8/14.000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000/1024 (1024)     | 80000/1100             | 8/28.000              |
| Standard_F72s_v2<sup>2, 3</sup> | 72 | 144 | 576         | 32             | 144000/1152 (1520)     | 80000/1100             | 8 / 30.000              |


<sup>1</sup> Virtuelle Computer der Fsv2-Serie verfügen über Hyper-Threading-Technologie von Intel®

<sup>2</sup> Bei mehr als 64 vCPUs ist eines der folgenden Gastbetriebssysteme erforderlich: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 und Red Hat Enterprise Linux, CentOS 7.3 oder Oracle Linux 7.3 mit LIS 4.2.1

<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

## <a name="fs-series-sup1sup"></a>Fs-Serie <sup>1</sup>

ACU: 210–250

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4.000/32 (12) |3.200/48 |2/750 |
| Standard_F2s |2 |4 |8 |8 |8.000/64 (24) |6.400/96 |2/1500 |
| Standard_F4s |4 |8 |16 |16 |16.000/128 (48) |12.800/192 |4/3000 |
| Standard_F8s |8 |16 |32 |32 |32.000/256 (96) |25.600/384 |8/6000 |
| Standard_F16s |16 |32 |64 |64 |64.000/512 (192) |51.200/768 |8 / 12000 |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

<sup>1</sup> Der mit einer VM der Fs-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Details finden Sie unter [Entwerfen für hohe Leistung](../articles/virtual-machines/windows/premium-storage-performance.md).


<br>

## <a name="f-series"></a>F-Serie

ACU: 210–250

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Größe         | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000/46/23                                           | 4/4 x 500                         | 2/750                 |
| Standard_F2  | 2         | 4           | 32             | 6000/93/46                                           | 8/8 x 500                         | 2/1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000/187/93                                         | 16/16 x 500                         | 4/3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000/375/187                                        | 32/32 x 500                       | 8/6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000/750/375                                        | 64/64 x 500                       | 8 / 12000           |


<br>


