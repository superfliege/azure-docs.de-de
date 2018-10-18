---
title: Includedatei
description: Includedatei
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 6308710a12e983ba35d9bd20d9926cba4ae27c5c
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400265"
---
Arbeitsspeicheroptimierte VM-Größen bieten ein hohes Arbeitsspeicher-zu-CPU-Verhältnis und eignen sich hervorragend für relationale Datenbankserver, mittelgroße bis große Caches und In-Memory-Analysen. Dieser Artikel enthält Informationen zur Anzahl von vCPUs, Datenträgern und NICs sowie zum Speicherdurchsatz und zur Netzwerkbandbreite der einzelnen Größen in dieser Gruppe. 

* Die M-Serie bietet die höchste vCPU-Anzahl (bis zu 128 vCPUs) und den größten Arbeitsspeicher (bis zu 3,8 TiB) für die virtuellen Computer in der Cloud.  Dies ist ideal für extrem große Datenbanken oder andere Anwendungen, für die eine hohe vCPU-Anzahl und große Mengen an Arbeitsspeicher benötigt werden.

* Die Serien Dv2, G und deren DSv2/GS-Entsprechungen eignen sich ideal für Clientanwendungen, die schnellere vCPUs und eine höhere Leistung des temporären Speichers erfordern oder einen höheren Arbeitsspeicherbedarf haben.  Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.


* Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) oder 2,3-GHz-E5-2673 v4-Prozessors (Broadwell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

* Die Ev3-Serie bietet den 2,3-GHz-E5-2673 v4-Prozessor (Broadwell) in einer Hyperthreadkonfiguration und somit ein besseres Preis-Leistungs-Verhältnis für die meisten universellen Workloads, und sie bringt den Ev3 mit den allgemeinen VMs der meisten anderen Clouds in Einklang.  Der Speicher wurde erweitert (von 7 GiB/vCPU auf 8 GiB/vCPU), während die Datenträger- und Netzwerkgrenzwerte pro Kern angepasst wurden, um sich für den Übergang zum Hyperthreading anzupassen.  Die Ev3-Serie ist der Nachfolger für die VMs mit großen Arbeitsspeichergrößen der D/Dv2-Familien.

* Azure Compute bietet VM-Größen, die für einen bestimmten Hardwaretyp isoliert und für einen einzelnen Kunden bestimmt sind.  Diese VM-Größen eignen sich am besten für Workloads, die ein hohes Maß an Isolation von anderen Kunden erfordern, wenn es um Workloads mit Elementen wie Konformität und gesetzlichen Anforderungen geht.  Kunden können auch die Ressourcen dieser isolierten virtuellen Computer weiter unterteilen, indem sie die [Azure-Unterstützung für geschachtelte virtuelle Computer](https://azure.microsoft.com/blog/nested-virtualization-in-azure/) verwenden.  In den folgenden Tabellen der VM-Familien finden Sie Ihre Optionen für isolierte VMs.

## <a name="esv3-series"></a>Esv3-Serie 

ACU: 160 - 190 <sup>1</sup>

Storage Premium: unterstützt

Storage Premium-Zwischenspeicherung: unterstützt

Die Instanzen der ESv3-Serie basieren auf dem Prozessor vom Typ 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) und können mit Intel Turbo Boost Technology 2.0 3,5 GHz erreichen und Premium-Speicher nutzen. Die Instanzen der ESv3-Serie eignen sich ideal für speicherintensive Unternehmensanwendungen.


| Größe             | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4.000/32 (50)                                                       | 3.200/48                                | 2 / 1.000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8.000/64 (100)                                                      | 6.400/96                                | 2 / 2.000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16.000/128 (200)                                                    | 12.800/192                              | 4 / 4.000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32.000/256 (400)                                                    | 25.600/384                              | 8 / 8.000                                       |
| Standard_E20s_v3&nbsp;<sup>2</sup> | 20     | 160         | 320            | 32             | 40.000/320 (400)                                                    | 32.000/480                              | 8/10.000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64.000/512 (800)                                                    | 51.200/768                              | 8 / 16.000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128.000/1.024 (1.600)                                                   | 80.000/1.200                             | 8 / 30.000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128.000/1.024 (1.600)                                                   | 80.000/1.200                             | 8 / 30.000                             |


<sup>1</sup> Virtuelle Computer der Esv3-Serie verfügen über Hyper-Threading-Technologie von Intel®.

<sup>2</sup> Eingeschränkte Kerngrößen verfügbar.

<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.


## <a name="ev3-series"></a>Ev3-Serie 

ACU: 160 - 190 <sup>1</sup>

Storage Premium: nicht unterstützt

Storage Premium-Zwischenspeicherung: nicht unterstützt

Die Instanzen der Ev3-Serie basieren auf dem Prozessor vom Typ 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) und können mit Intel Turbo Boost Technology 2.0 3,5 GHz erreichen. Die Instanzen der ESv3-Serie eignen sich ideal für speicherintensive Unternehmensanwendungen.

Datenträgerspeicher wird separat zu virtuellen Computern abgerechnet. Verwenden Sie die ESv3-Größen, um Datenträger mit Premium-Speicher zu nutzen. Die Preis- und Abrechnungskennzahlen für die ESv3-Größen entsprechen denen der Ev3-Serie. 


| Größe            | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Maximale Anzahl NICs/Netzwerkbandbreite |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1.000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2.000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4.000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8.000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8/10.000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16.000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30.000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30.000           |

<sup>1</sup> Virtuelle Computer der Ev3-Serie verfügen über Hyper-Threading-Technologie von Intel®.

<sup>2</sup> Eingeschränkte Kerngrößen verfügbar.

<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.


## <a name="m-series"></a>M-Serie 

ACU: 160 - 180 <sup>1</sup>

Storage Premium: unterstützt

Storage Premium-Zwischenspeicherung: unterstützt

Schreibbeschleunigung: [unterstützt](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Größe            | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218,75 | 256  | 8  | 10.000 / 100 (793)  | 5.000  / 125 | 4 / 2.000 |
| Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437,5  | 512  | 16 | 20.000 / 200 (1,587) | 10.000/250 | 8 / 4.000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40.000 / 400 (3.174) | 20.000/500 | 8 / 8.000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40.000 / 400 (3.174) | 20.000/500 | 8 / 8.000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40.000 / 400 (3.174) | 20.000/500 | 8 / 8.000 |
| Standard_M64s  | 64 | 1024   | 2.048 | 64 | 80.000 / 800 (6.348)| 40.000/1.000 | 8 / 16.000          |
| Standard_M64ls  | 64 | 512    | 2.048 | 64 | 80.000 / 800 (6.348) | 40.000/1.000 | 8 / 16.000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1.792 | 2.048 | 64 | 80.000 / 800 (6.348)| 40.000/1.000 | 8 / 16.000          |
| Standard_M128s&nbsp;<sup>2,&nbsp;3</sup> | 128  | 2.048        | 4.096  | 64 | 160,000 / 1,600 (12,696) | 80.000/2.000                            | 8 / 30.000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3.892  | 4.096 | 64 | 160,000 / 1,600 (12,696) | 80.000/2.000                            | 8 / 30.000          |
| Standard_M64   | 64  | 1024 | 7.168  | 64 | 80.000  / 800  (1.228) | 40.000/1.000 | 8 / 16.000 |
| Standard_M64m  | 64  | 1.792 | 7.168  | 64 | 80.000  / 800  (1.228) | 40.000/1.000 | 8 / 16.000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2.048 | 14.336 | 64 | 250.000 / 1.600 (2.456) | 80.000/2.000 | 8/32.000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3.892 | 14.336 | 64 | 250.000 / 1.600 (2.456) | 80.000/2.000 | 8/32.000 |



<sup>1</sup> Virtuelle Computer der M-Serie verfügen über Hyper-Threading-Technologie von Intel®

<sup>2</sup> Für mehr als 64 vCPUs ist eines der folgenden Gastbetriebssysteme erforderlich: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 und Red Hat Enterprise Linux, CentOS 7.3 oder Oracle Linux 7.3 mit LIS 4.2.1.

<sup>3</sup> Eingeschränkte Kerngrößen verfügbar.

<sup>4</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>

## <a name="gs-series"></a>GS-Serie 

ACU: 180 - 240 <sup>1</sup>

Storage Premium: unterstützt

Storage Premium-Zwischenspeicherung: unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10.000/100 (264) |5.000 / 125 |2/2000 |
| Standard_GS2 |4 |56 |112 |16 |20.000/200 (528) |10.000/250 |2/4000 |
| Standard_GS3 |8 |112 |224 |32 |40.000/400 (1.056) |20.000/500 |4/8000 |
| Standard_GS4&nbsp;<sup>3</sup> |16 |224 |448 |64 |80.000/800 (2.112) |40.000/1.000 |8/16000 |
| Standard_GS5&nbsp;<sup>2,&nbsp;3</sup> |32 |448 |896 |64 |160.000/1.600 (4.224) |80.000/2.000 |8/20000 |

<sup>1</sup> Der mit einer VM der GS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md). 

<sup>2</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

<sup>3</sup> Eingeschränkte Kerngrößen verfügbar.

<br>

## <a name="g-series"></a>G-Serie

ACU: 180 - 240

Storage Premium: nicht unterstützt

Storage Premium-Zwischenspeicherung: nicht unterstützt

| Größe         | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 8/8 x 500                       | 2/2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 16/16 x 500                       | 2/4000                     |
| Standard_G3  | 8         | 112         | 1.536          | 24000/375/187                                        | 32/32 x 500                     | 4/8000                |
| Standard_G4  | 16        | 224         | 3.072          | 48000/750/375                                        | 64/64 x 500                     | 8/16000          |
| Standard_G5&nbsp;<sup>1</sup> | 32        | 448         | 6.144          | 96000/1500/750                                       | 64/64 x 500                     | 8/20000           |

<sup>1</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>


## <a name="dsv2-series-11-15"></a>DSv2-Serie 11-15

ACU: 210 - 250 <sup>1</sup>

Storage Premium: unterstützt

Storage Premium-Zwischenspeicherung: unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8.000/64 (72) |6.400/96 |2/1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16.000/128 (144) |12.800/192 |4/3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32.000/256 (288) |25.600/384 |8/6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64.000/512 (576) |51.200/768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80.000/640 (720) |64.000/960 |8/25000&nbsp;<sup>4</sup>


<sup>1</sup> Der mit einer VM der DSv2-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md).

<sup>2</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

<sup>3</sup> Eingeschränkte Kerngrößen verfügbar.

<sup>4</sup> 25000 Mbps mit beschleunigtem Netzwerkbetrieb.

<br>

## <a name="dv2-series-11-15"></a>Dv2-Serie 11-15

ACU: 210 - 250

Storage Premium: nicht unterstützt

Storage Premium-Zwischenspeicherung: nicht unterstützt

| Größe              | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8 x 500                         | 2/1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16 x 500                         | 4/3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32 x 500                       | 8/6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/64 x 500                       | 8 / 12000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1.000          | 60000/937/468                                        | 64/64 x 500                       | 8/25000&nbsp;<sup>2</sup> |

<sup>1</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird. 

<sup>2</sup> 25000 Mbps mit beschleunigtem Netzwerkbetrieb.



<br>



