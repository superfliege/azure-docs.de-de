---
title: "Azure SQL Data Warehouse – Leistungsstufen | Microsoft-Dokumentation"
description: "Einführung in die für Elastizität und Compute optimierten Leistungsstufen, die in Azure SQL Data Warehouse zur Verfügung stehen"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 11/10/2017
ms.author: jrj;barbkess
ms.openlocfilehash: c403a73d03fd5152e2c0617b3e3784926c28f5c3
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="azure-sql-data-warehouse-performance-tiers-preview"></a>Azure SQL Data Warehouse – Leistungsstufen (Vorschau)
SQL Data Warehouse bietet zwei Leistungsstufen, die für analytische Workloads optimiert sind. In diesem Artikel werden die Konzepte von Leistungsstufen beschrieben, die Ihnen bei der Auswahl der am besten geeigneten Leistungsstufe für Ihre Workload helfen sollen. 


## <a name="what-is-a-performance-tier"></a>Was ist eine Leistungsstufe?
Eine Leistungsstufe ist eine Option, die die Konfiguration Ihres Data Warehouse bestimmt. Diese Option ist die erste Auswahl, die Sie beim Erstellen eines Data Warehouse treffen.  

- Die **Leistungsstufe „Optimiert für Elastizität“** trennt die Compute- und die Speicherebene in der Architektur. Diese Option eignet sich besonders für Workloads, die durch häufige Skalierung optimal von der Trennung zwischen Compute und Speicher profitieren können, um vorübergehende Aktivitätsspitzen zu unterstützen. Diese Computestufe hat den niedrigsten Einstiegspreis und deckt dank Skalierung den größten Teil der Kundenworkloads ab.

- Die **Leistungsstufe „Optimiert für Compute“** nutzt die neueste Azure-Hardware, um einen neuen NVMe-SSD-Zwischenspeicher einzuführen, der dafür sorgt, dass besonders häufig verwendete Daten in der Nähe der CPUs bleiben – also genau dort, wo Sie sie haben möchten. Dank automatischem Tiering des Speichers eignet sich diese Leistungsstufe hervorragend für komplexe Abfragen, da sämtliche Eingaben/Ausgaben lokal auf der Computeebene abgewickelt werden. Darüber hinaus wird der Columnstore erweitert, um eine unbegrenzte Menge an Daten in Ihrem SQL Data Warehouse speichern zu können. Die Leistungsstufe „Optimiert für Compute“ bietet die größtmögliche Skalierbarkeit und ermöglicht eine Skalierung auf bis zu 30.000 cDWU (compute Data Warehouse Units). Diese Stufe empfiehlt sich für Workloads mit kontinuierlich hohem Leistungsbedarf.

## <a name="service-levels"></a>Servicelevel
Das Servicelevelziel (Service Level Objective, SLO) ist die Skalierbarkeitseinstellung, die die Kosten und Leistungsstufe Ihres Data Warehouse festlegt. Die Servicelevel für die Leistungsstufe „Optimiert für Compute“ werden in Compute-Data Warehouse Einheiten (cDWU) gemessen, z.B. DW2000c. Die Leistungsstufe „Optimiert für Elastizität“ werden in DWUs gemessen, z.B. DW2000. Weitere Informationen finden Sie unter [Was ist eine Data Warehouse-Einheit?](what-is-a-data-warehouse-unit-dwu-cdwu.md)

In T-SQL bestimmt die Einstellung SERVICE_OBJECTIVE den Servicelevel und die Leistungsstufe für das Data Warehouse.

```sql
--Optimized for Elasticity
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Optimized for Compute
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="memory-maximums"></a>Arbeitsspeicher-Höchstwerte
Die Leistungsstufen haben unterschiedliche Arbeitsspeicherprofile, die sich in unterschiedlichen Größen an Arbeitsspeicher pro Abfrage widerspiegeln. Die Leistungsstufe „Optimiert für Compute“ bietet 2,5-mal mehr Arbeitsspeicher pro Abfrage als die Leistungsstufe „Optimiert für Elastizität“. Durch diesen zusätzlichen Arbeitsspeicher kann die Leistungsstufe „Optimiert für Compute“ ihre extrem hohe Leistung bereitstellen. Zusätzlicher Arbeitsspeicher pro Abfrage erlaubt auch die gleichzeitige Ausführung von mehr Abfragen, da die Abfragen niedrigere [Ressourcenklassen](resource-classes-for-workload-management.md) verwenden können. 

### <a name="optimized-for-elasticity"></a>Optimiert für Elastizität

Die Servicelevel der Leistungsstufe „Optimiert für Elastizität“ liegen zwischen DW100 und DW6000. 

| Servicelevel | Max. Anzahl gleichzeitiger Abfragen | Serverknoten | Verteilungen pro Serverknoten | Maximaler Arbeitsspeicher pro Verteilung (MB) | Maximaler Arbeitsspeicher pro Data Warehouse (GB) |
|:-------------:|:----------------------:|:-------------:|:------------------------------:|:--------------------------------:|:----------------------------------:|
| DW100         | 4                      | 1             | 60                             | 400                              |  24                                |
| DW200         | 8                      | 2             | 30                             | 800                              |  48                                |
| DW300         | 12                     | 3             | 20                             | 1.200                            |  72                                |
| DW400         | 16                     | 4             | 15                             | 1.600                            |  96                                |
| DW500         | 20                     | 5             | 12                             | 2.000                            | 120                                |
| DW600         | 24                     | 6             | 10                             | 2.400                            | 144                                |
| DW1000        | 32                     | 10            | 6                              | 4.000                            | 240                                |
| DW1200        | 32                     | 12            | 5                              | 4.800                            | 288                                |
| DW1500        | 32                     | 15            | 4                              | 6.000                            | 360                                |
| DW2000        | 32                     | 20            | 3                              | 8.000                            | 480                                |
| DW3000        | 32                     | 30            | 2                              | 12.000                           | 720                                |
| DW6000        | 32                     | 60            | 1                              | 24.000                           | 1.440                               |

### <a name="optimized-for-compute"></a>Optimiert für Compute

Die Servicelevel der Leistungsstufe „Optimiert für Compute“ liegen zwischen DW1000c und DW30000c. 

| Servicelevel | Max. Anzahl gleichzeitiger Abfragen | Serverknoten | Verteilungen pro Serverknoten | Maximaler Arbeitsspeicher pro Verteilung (GB) | Maximaler Arbeitsspeicher pro Data Warehouse (GB) |
|:-------------:|:----------------------:|:-------------:|:------------------------------:|:--------------------------------:|:----------------------------------:|
| DW1000c       | 32                     | 2             | 30                             |  10                              |   600                              |
| DW1500c       | 32                     | 3             | 20                             |  15                              |   900                              |
| DW2000c       | 32                     | 4             | 15                             |  20                              |  1200                              |
| DW2500c       | 32                     | 5             | 12                             |  25                              |  1500                              |
| DW3000c       | 32                     | 6             | 10                             |  30                              |  1800                              |
| DW5000c       | 32                     | 10            | 6                              |  50                              |  3000                              |
| DW6000c       | 32                     | 12            | 5                              |  60                              |  3600                              |
| DW7500c       | 32                     | 15            | 4                              |  75                              |  4500                              |
| DW10000c      | 32                     | 20            | 3                              | 100                              |  6000                              |
| DW15000c      | 32                     | 30            | 2                              | 150                              |  9000                              |
| DW30000c      | 32                     | 60            | 1                              | 300                              | 18000                              |

Die maximale cDWU beträgt DW30000c. Sie bietet 60 Serverknoten und eine Verteilung pro Serverknoten. Beispielsweise verarbeitet ein 600-TB-Data Warehouse mit DW30000c etwa 10 TB pro Serverknoten.

## <a name="concurrency-maximums"></a>Parallelitätshöchstwerte
SQL Data Warehouse bietet branchenführende Parallelität in einem einzigen Data Warehouse. Um sicherzustellen, dass für jede Abfrage genügend Ressourcen zur effizienten Verarbeitung zur Verfügung stehen, verfolgt das System den Verbrauch von Computeressourcen, indem jeder Abfrage Parallelitätsslots zugewiesen werden. Das System stellt Abfragen in eine Warteschlange, in der sie verbleiben, bis genügend Parallelitätsslots verfügbar sind. 

Parallelitätsslots legen außerdem die CPU-Prioritäten fest. Weitere Informationen finden Sie unter [Analysieren Ihrer Workload](analyze-your-workload.md).

### <a name="concurrency-slots"></a>Parallelitätsslots
Parallelitätsslots stellen eine einfache Möglichkeit dar, die verfügbaren Ressourcen für die Ausführung von Abfragen zu verfolgen. Sie ähneln Tickets für Sitzplätze in einem Konzert, die Sie erwerben, da die Sitzplatzanzahl begrenzt ist. Entsprechend verfügt SQL Data Warehouse über eine endliche Anzahl von Computeressourcen. Abfragen reservieren Computeressourcen durch die Anforderung von Parallelitätsslots. Bevor eine Abfrage ausgeführt werden kann, muss sie genügend Parallelitätsslots reservieren. Wenn eine Abfrage abgeschlossen wurde, werden ihre Parallelitätsslots freigegeben. 

* Die Leistungsstufe „Optimiert für Elastizität“ bietet 240 Parallelitätsslots.
* Die Leistungsstufe „Optimiert für Compute“ bietet 1.200 Parallelitätsslots.

Jede Abfrage belegt keinen, einen oder mehrere Parallelitätsslots. Systemabfragen und einige einfache Abfragen verbrauchen keine Slots. Standardmäßig erfordern Abfragen, die von Ressourcenklassen verwaltet werden, einen Parallelitätsslot. Komplexere Abfragen können zusätzliche Parallelitätsslots erfordern.  

- Eine Abfrage mit 10 Parallelitätsslots kann auf 5-mal mehr Computeressourcen als eine Abfrage mit 2 Parallelitätsslots zugreifen.
- Wenn jede Abfrage 10 Parallelitätsslots erfordert und es 40 Parallelitätsslots gibt, können nur 4 Abfragen gleichzeitig ausgeführt werden.
 
Nur von Ressourcen verwaltete Abfragen verbrauchen Parallelitätsslots. Die genaue Anzahl der verbrauchten Parallelitätsslots richtet sich nach der [Ressourcenklasse](resource-classes-for-workload-management.md) der Abfrage.

### <a name="optimized-for-compute"></a>Optimiert für Compute
Die folgende Tabelle zeigt die maximale Anzahl gleichzeitiger Abfragen und die Parallelitätsslots für jede [dynamische Ressourcenklasse](resource-classes-for-workload-management.md).  Die Werte gelten für die Leistungsstufe „Optimiert für Compute“.

**Dynamische Ressourcenklassen**
| Service Level | Maximale Anzahl gleichzeitiger Abfragen | Verfügbare Parallelitätsslots | Durch „smallrc“ verwendete Slots | Durch „mediumrc“ verwendete Slots | Durch „largerc“ verwendete Slots | Durch „xlargerc“ verwendete Slots |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW1000c       | 32                         |   40                        | 1                     |  8                     |  16                   |  32                    |
| DW1500c       | 32                         |   60                        | 1                     |  8                     |  16                   |  32                    |
| DW2000c       | 32                         |   80                        | 1                     | 16                     |  32                   |  64                    |
| DW2500c       | 32                         |  100                        | 1                     | 16                     |  32                   |  64                    |
| DW3000c       | 32                         |  120                        | 1                     | 16                     |  32                   |  64                    |
| DW5000c       | 32                         |  200                        | 1                     | 32                     |  64                   | 128                    |
| DW6000c       | 32                         |  240                        | 1                     | 32                     |  64                   | 128                    |
| DW7500c       | 32                         |  300                        | 1                     | 64                     | 128                   | 128                    |
| DW10000c      | 32                         |  400                        | 1                     | 64                     | 128                   | 256                    |
| DW15000c      | 32                         |  600                        | 1                     | 64                     | 128                   | 256                    |
| DW30000c      | 32                         | 1200                        | 1                     | 64                     | 128                   | 256                    |

**Statische Ressourcenklassen**

Die folgende Tabelle zeigt die maximale Anzahl gleichzeitiger Abfragen und die Parallelitätsslots für jede [statische Ressourcenklasse](resource-classes-for-workload-management.md).  

| Service Level | Maximale Anzahl gleichzeitiger Abfragen | Verfügbare Parallelitätsslots |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2000c       | 32                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 32                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 32                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW5000c       | 32                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 32                         |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 32                         |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 32                         |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 32                         |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 32                         | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

### <a name="optimized-for-elasticity"></a>Optimiert für Elastizität
Die folgende Tabelle zeigt die maximale Anzahl gleichzeitiger Abfragen und die Parallelitätsslots für jede [dynamische Ressourcenklasse](resource-classes-for-workload-management.md).  Die Werte gelten für die Leistungsstufe „Optimiert für Elastizität“.

**Dynamische Ressourcenklassen**

| Servicelevel | Maximale Anzahl gleichzeitiger Abfragen | Verfügbare Parallelitätsslots | smallrc | mediumrc | largerc | xlargerc |
|:-------------:|:--------------------------:|:---------------------------:|:-------:|:--------:|:-------:|:--------:|
| DW100         |  4                         |   4                         | 1       |  1       |  2      |   4      |
| DW200         |  8                         |   8                         | 1       |  2       |  4      |   8      |
| DW300         | 12                         |  12                         | 1       |  2       |  4      |   8      |
| DW400         | 16                         |  16                         | 1       |  4       |  8      |  16      |
| DW500         | 20                         |  20                         | 1       |  4       |  8      |  16      |
| DW600         | 24                         |  24                         | 1       |  4       |  8      |  16      |
| DW1000        | 32                         |  32                         | 1       |  8       | 16      |  32      |
| DW1200        | 32                         |  32                         | 1       |  8       | 16      |  32      |
| DW1500        | 32                         |  32                         | 1       |  8       | 16      |  32      |
| DW2000        | 32                         |  48                         | 1       | 16       | 32      |  64      |
| DW3000        | 32                         |  64                         | 1       | 16       | 32      |  64      |
| DW6000        | 32                         | 128                         | 1       | 32       | 64      | 128      |

**Statische Ressourcenklassen:** Die folgende Tabelle zeigt die maximale Anzahl gleichzeitiger Abfragen und die Parallelitätsslots für jede [statische Ressourcenklasse](resource-classes-for-workload-management.md).  Die Werte gelten für die Leistungsstufe „Optimiert für Elastizität“.

| Servicelevel | Maximale Anzahl gleichzeitiger Abfragen | Maximale Anzahl von Parallelitätsslots |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:-------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100         | 4                          |   4                       | 1         | 2          | 4          | 4          |  4         |  4         |  4         |   4        |
| DW200         | 8                          |   8                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW300         | 12                         |  12                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400         | 16                         |  16                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500         | 20                         |  20                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW600         | 24                         |  24                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000        | 32                         |  40                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1200        | 32                         |  48                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500        | 32                         |  60                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000        | 32                         |  80                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000        | 32                         | 120                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW6000        | 32                         | 240                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

Wenn einer dieser Grenzwerte erreicht wird, werden neue Abfragen in die Warteschlange eingereiht und nach dem Prinzip „First In, First Out“ ausgeführt.  Wenn eine Abfrage abgeschlossen wird und die Anzahl von Abfragen und Slots unter die Grenzwerte sinkt, gibt SQL Data Warehouse Abfragen in der Warteschlange frei. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Ressourcenklassen zum weiteren Optimieren Ihrer Workload finden Sie in den folgenden Artikeln:
* [Ressourcenklassen für die Workloadverwaltung](resource-classes-for-workload-management.md)
* [Analysieren Ihrer Workload](analyze-your-workload.md)

