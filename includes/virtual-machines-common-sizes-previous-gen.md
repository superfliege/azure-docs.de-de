---
title: Includedatei
description: Includedatei
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 07/06/2018
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 937ac1d892cdea849d7e5a89c8e69c6b069f2efd
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56334043"
---
Dieser Artikel enthält Informationen zu den vorherigen Generationen von VM-Größen. Diese Größen können weiterhin verwendet werden, es sind jedoch neuere Generationen verfügbar.


## <a name="ds-series"></a>DS-Serie

ACU: 160

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |4 |4.000/32 (43) |3.200/32 |2/500 |
| Standard_DS2 |2 |7 |14 |8 |8.000/64 (86) |6.400/64 |2/1000 |
| Standard_DS3 |4 |14 |28 |16 |16.000/128 (172) |12.800/128 |4/2000 |
| Standard_DS4 |8 |28 |56 |32 |32.000/256 (344) |25.600/256 |8/4000 |

<br>

## <a name="ds-series---memory-optimized"></a>DS-Serie – arbeitsspeicheroptimiert

ACU: 160 <sup>1</sup>

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8.000/64 (72) |6.400/64 |2/1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000/128 (144) |12.800/128 |4/2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000/256 (288) |25.600/256 |8/4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000/512 (576) |51.200/512 |8 / 8000 |

<sup>1</sup> Der mit einer VM der DS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Details finden Sie unter [Entwerfen für hohe Leistung](../articles/virtual-machines/windows/premium-storage-performance.md).



## <a name="d-series"></a>D-Serie 

ACU: 160

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Größe         | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000/46/23                                           | 4/4 x 500                         | 2/500                 |
| Standard_D2  | 2         | 7           | 100            | 6000/93/46                                           | 8/8 x 500                         | 2/1000                     |
| Standard_D3  | 4         | 14          | 200            | 12000/187/93                                         | 16/16 x 500                         | 4/2000                     |
| Standard_D4  | 8         | 28          | 400            | 24000/375/187                                        | 32/32 x 500                       | 8/4000                     |

<br>

## <a name="d-series---memory-optimized"></a>D-Serie – arbeitsspeicheroptimiert

ACU: 160

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Größe         | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 8/8 x 500                         | 2/1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 16/16 x 500                         | 4/2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 32/32 x 500                       | 8/4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 64/64 x 500                       | 8 / 8000                |

<br>

## <a name="a-series---compute-intensive-instances"></a>A-Serie: Rechenintensive Instanzen

ACU: 225

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Die Größen A8 bis A11 und die Größen der H-Reihe werden auch als *rechenintensive Instanzen* bezeichnet. Die Hardware, auf der diese Größen ausgeführt werden, wurde für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Clusteranwendungen (High Performance Computing), Modellierung und Simulationen. Die Reihen A8 bis A11 nutzen Intel Xeon E5-2670 mit 2,6 GHZ und die H-Reihe Intel Xeon E5-2667 v3 mit 3,2 GHz.  Dieser Artikel enthält Informationen zur Anzahl von vCPUs, Datenträgern und NICs sowie zum Speicherdurchsatz und zur Netzwerkbandbreite der einzelnen Größen in dieser Gruppe. 

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (HDD): GiB | Max. Anzahl Datenträger | Max. Datenträgerdurchsatz: IOPS | Maximale Anzahl NICs|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32 x 500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64 x 500 |4 |
| Standard_A10 |8 |56 |382 |32 |32 x 500 |2  |
| Standard_A11 |16 |112 |382 |64 |64 x 500 |4 |

<sup>1</sup> Für MPI-Anwendungen ist ein dediziertes RDMA-Back-End-Netzwerk durch ein FDR InfiniBand-Netzwerk aktiviert, das eine äußerst geringe Latenz und eine hohe Bandbreite ermöglicht.

<br>

## <a name="a-series"></a>A-Serie

ACU: 50–100

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (HDD): GiB | Max. Anzahl Datenträger | Max. Datenträgerdurchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 <sup>1</sup> |1 |0,768 |20 |1 |1 x 500 |2/100 |
| Standard_A1 |1 |1,75 |70 |2 |2 x 500 |2/500  |
| Standard_A2 |2 |3,5 |135 |4 |4 x 500 |2/500 |
| Standard_A3 |4 |7 |285 |8 |8 x 500 |2/1000 |
| Standard_A4 |8 |14 |605 |16 |16 x 500 |4/2000 |
| Standard_A5 |2 |14 |135 |4 |4 x 500 |2/500 |
| Standard_A6 |4 |28 |285 |8 |8 x 500 |2/1000 |
| Standard_A7 |8 |56 |605 |16 |16 x 500 |4/2000 |
<br>

<sup>1</sup> Die Größe „A0“ ist auf der physischen Hardware „überzeichnet“. Nur für diese spezielle Größe kann es dazu kommen, dass sich andere Kundenbereitstellungen negativ auf die Leistung Ihrer aktiven Workload auswirken. Die relative Leistung ist unten als erwartete Baseline beschrieben und unterliegt einer ungefähren Variabilität von 15 Prozent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard_A0 – A4 mit Befehlszeilenschnittstelle (CLI) und PowerShell

Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größennamen in der Befehlszeilenschnittstelle und in PowerShell:

* Standard_A0 ist „Sehr klein“ 
* Standard_A1 ist „Klein“
* Standard_A2 ist „Mittel“
* Standard_A3 ist „Groß“
* Standard_A4 ist „Extragroß“

## <a name="basic-a"></a>Basic A

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Die Größen des Basic-Tarifs sind hauptsächlich für Entwicklungsworkloads und andere Anwendungen bestimmt, für die kein Lastenausgleich, keine automatische Skalierung und keine speicherintensiven virtuellen Computer erforderlich sind.

|Größe – Größe\Name | vCPU |Arbeitsspeicher|Netzwerkkarten (max.)|Max. Größe der temporären Datenträger |Maximal Datenträger (jeweils 1023 GB)|Maximal IOPS (300 pro Datenträger)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1 x 300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2 x 300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4 x 300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|4 x 300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16 x 300|
 


