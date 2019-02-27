---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8a067474fb172d4ff7a7fdf7eb6d24536bd2d017
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443411"
---
# <a name="what-disk-types-are-available-in-azure"></a>Welche Datenträgertypen stehen in Azure zur Verfügung?

Azure Managed Disks bietet derzeit vier Datenträgertypen, von denen drei allgemein verfügbar sind und eine sich in der Vorschauphase befindet. Für jeden dieser vier Datenträgertypen gelten eigene Kundenszenarien.

## <a name="disk-comparison"></a>Datenträgervergleich

Die folgende Tabelle enthält eine Gegenüberstellung von SSD Ultra- (Vorschau), SSD Premium-, SSD Standard- und HDD Standard-Laufwerken für verwaltete Datenträger, um Ihnen die Entscheidung zu erleichtern. (SSD steht für ein Solid State Drive, HDD für ein Festplattenlaufwerk.)

|   | SSD Ultra (Vorschau)   | SSD Premium   | SSD Standard   | HDD Standard   |
|---------|---------|---------|---------|---------|
|Datenträgertyp   |SSD   |SSD   |SSD   |Festplattenlaufwerk   |
|Szenario   |E/A-intensive Workloads wie SAP HANA, führende Datenbanken (z.B. SQL, Oracle) und andere Workloads mit vielen Transaktionen.   |Produktionsworkloads und leistungsabhängige Workloads   |Webserver, wenig genutzte Unternehmensanwendungen und Dev/Test   |Sicherung, nicht kritisch, sporadischer Zugriff   |
|Datenträgergröße   |65.536 Gibibyte (GiB) (Vorschau)   |4.095 GiB (allgemein verfügbar), 32.767 GiB (Vorschau)    |4.095 GiB (allgemein verfügbar), 32.767 GiB (Vorschau)   |4.095 GiB (allgemein verfügbar), 32.767 GiB (Vorschau)   |
|Max. Durchsatz   |2.000 MiB/s (Vorschau)   |250 MiB/s (allgemein verfügbar), 750 MiB/s (Vorschau)   |60 MiB/s (allgemein verfügbar), 500 MiB/s (Vorschau)   |60 MiB/s (allgemein verfügbar), 500 MiB/s (Vorschau)   |
|Max. IOPS   |160.000 (Vorschau)   |7.500 (allgemein verfügbar), 20.000 (Vorschau)   |500 (allgemein verfügbar), 2.000 (Vorschau)   |500 (allgemein verfügbar), 2.000 (Vorschau)   |

## <a name="ultra-ssd-preview"></a>SSD Ultra (Vorschau)

Azure SSD Ultra (Vorschau) bietet hohen Durchsatz, hohe IOPS und konsistenten Datenträgerspeicher mit niedrigen Wartezeiten für Azure-IaaS-VMs. Zu den weiteren Vorteilen von SSD Ultra gehört die Möglichkeit, die Datenträgerleistung dynamisch in Abstimmung mit Ihren Workloads ändern zu können, ohne Ihre virtuellen Computer neu starten zu müssen. SSD Ultra eignet sich für datenintensive Workloads wie SAP HANA, führende Datenbanken und Workloads mit vielen Transaktionen. SSD Ultra kann nur als Datenträger für Daten verwendet werden. Als Betriebssystemdatenträger empfehlen wir, SSD Premium-Datenträger zu verwenden.

### <a name="performance"></a>Leistung

Wenn Sie einen Ultra-Datenträger bereitstellen, können Sie die Kapazität und die Leistung des Datenträgers unabhängig voneinander konfigurieren. SSD Ultra-Datenträger sind in mehreren festen Größen von 4 GiB bis 64 TiB erhältlich und bieten ein flexibles Leistungskonfigurationsmodell, durch das Sie IOPS und Durchsatz unabhängig konfigurieren können.

Einige Hauptfunktionen von SSD Ultra-Datenträgern sind:

- Datenträgerkapazität: Die Kapazität von SSD Ultra reicht von 4 GiB bis 64 TiB.
- Datenträger-IOPS: SSD Ultra-Datenträger unterstützen IOPS-Limits von 300 IOPS/GiB bis hin zu maximal 160.000 IOPS pro Datenträger. Um die bereitgestellten IOPS-Werte zu erreichen, stellen Sie sicher, dass der IOPS-Wert für den ausgewählten Datenträger unter dem für den virtuellen Computer liegt. Der Mindestwert für Datenträger-IOPS beträgt 100 IOPS.
- Datenträgerdurchsatz: Mit SSD Ultra beträgt das Durchsatzlimit für einen einzelnen Datenträger 256 KiB/s für jeden bereitgestellten IOPS-Wert bis zu maximal 2000 MB/s pro Datenträger (dabei ist 1 MB/s = 10^6 Byte pro Sekunde). Der Mindest-Datenträgerdurchsatz beträgt 1 MiB.

### <a name="disk-size"></a>Datenträgergröße

|Datenträgergröße (GiB)  |IOPS-Obergrenzen  |Durchsatzlimit (MB/s)  |
|---------|---------|---------|
|4     |1.200         |300         |
|8     |2.400         |600         |
|16     |4.800         |1.200         |
|32     |9.600         |2.000         |
|64     |19.200         |2.000         |
|128     |38.400         |2.000         |
|256     |76.800         |2.000         |
|512     |80.000         |2.000         |
|1.024–65.536 (Größen in diesem Bereich erhöhen sich in Schritten von 1 TiB)     |160.000         |2.000         |

### <a name="preview-scope-and-limitations"></a>Umfang und Einschränkungen der Vorschauversion

In der Vorschauversion gilt für SSD Ultra-Datenträger Folgendes:

- Werden in „USA, Osten 2“ in einer einzelnen Verfügbarkeitszone unterstützt.  
- Können nur mit Verfügbarkeitszonen verwendet werden. (Verfügbarkeitsgruppen und einzelne VM-Bereitstellungen außerhalb der Zonen können keine Ultra-Datenträger anfügen.)
- Werden nur auf ES/DS v3 VMs unterstützt.
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4K.  
- Können nur als leere Datenträger erstellt werden.  
- Können aktuell nur mit Azure Resource Manager-Vorlagen, der CLI und dem Python SDK bereitgestellt werden.
- Unterstützen noch keine Datenträger-Momentaufnahmen, VM-Images, Verfügbarkeitsgruppen, VM-Skalierungsgruppen und Azure Disk Encryption.
- Unterstützen (noch) keine Integration in Azure Backup oder Azure Site Recovery.
- Wie bei den  [meisten Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) sollte dieses Feature bis zum Erreichen der allgemeinen Verfügbarkeit nicht für Produktionsworkloads verwendet werden.

## <a name="premium-ssd"></a>SSD Premium

Azure SSD Premium bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs) mit E/A-intensiven Workloads. Um die Geschwindigkeit und Leistung von Storage Premium-Datenträgern zu nutzen, können Sie vorhandene VM-Datenträger zu SSD Premium-Datenträgern migrieren. SSD Premium ist für unternehmenskritische Produktionsanwendungen geeignet.

### <a name="disk-size"></a>Datenträgergröße

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| SSD Premium-Größen  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60*              | P70*              | P80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB           | 32             | 64             | 128            | 256  | 512            | 1024    | 2.048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOPS pro Datenträger       | Bis zu 120 | Bis zu 240              | Bis zu 500              | Bis zu 1.100 | Bis zu 2.300              | Bis zu 5.000              | Bis zu 7.500             | Bis zu 7.500              | Bis zu 12.500              | Bis zu 15.000              | Bis zu 20.000              |
| Durchsatz pro Datenträger | Bis zu 25 MiB/s | Bis zu 50 MiB/s | Bis zu 100 MiB/s | Bis zu 125 MiB/s | Bis zu 150 MiB/s | Bis zu 200 MiB/s | Bis zu 250 MiB/s | Bis zu 250 MiB/s| Bis zu 480 MiB/s | Bis zu 750 MiB/s | Bis zu 750 MiB/s |

## <a name="standard-ssd"></a>SSD Standard

Azure SSD Standard-Datenträger stellen eine kostengünstige Speichermöglichkeit dar, die für Workloads mit konstanter Leistung bei niedrigen IOPS-Werten optimiert ist. Standard-SSDs eignen sich insbesondere als Einstiegslösung zur Verlagerung von Diensten in die Cloud. Dies gilt vor allem dann, wenn bei der Ausführung vieler Workloads auf lokalen HDD-Lösungen Probleme auftreten. Im Vergleich zu Festplattenlaufwerken bieten SSD Standard-Datenträger eine bessere Verfügbarkeit, Konsistenz, Zuverlässigkeit und Latenz. SSD Standard-Datenträger eignen sich für Webserver, Anwendungsserver mit geringer IOPS-Rate, wenig genutzte Unternehmensanwendungen und Dev/Test-Workloads.

### <a name="disk-size"></a>Datenträgergröße

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| SSD Standard-Größen  | E10               | E15               | E20             | E30 | E40              | E50              | E60*              | E70*              | E80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB           | 128             | 256             | 512            | 1024  | 2.048            | 4.095     | 8.192     | 16.384     | 32.767    |
| IOPS pro Datenträger       | Bis zu 500              | Bis zu 500              | Bis zu 500              | Bis zu 500 | Bis zu 500              | Bis zu 500              | Bis zu 500             | Bis zu 500              | Bis zu 1.300              | Bis zu 2.000              | Bis zu 2.000              |
| Durchsatz pro Datenträger | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s| Bis zu 300 MiB/s |  Bis zu 500 MiB/s | Bis zu 500 MiB/s |

## <a name="standard-hdd"></a>HDD Standard

Azure HDD Standard bietet zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer mit Workloads, bei denen Wartezeiten eine untergeordnete Rolle spielen. Darüber hinaus unterstützt er Blobs, Tabellen, Warteschlangen und Dateien. Bei Verwendung von Standardspeicher werden die Daten auf Festplattenlaufwerken (Hard Disk Drives, HDDs) gespeichert. Wenn Sie mit virtuellen Computern arbeiten, können Sie SSD Standard- und HDD Standard-Datenträger für Dev/Test-Szenarien sowie weniger kritische Workloads verwenden. Standardspeicher ist in allen Azure-Regionen verfügbar.

### <a name="disk-size"></a>Datenträgergröße

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| Standard-Datenträgertyp  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60*              | S70*              | S80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße in GiB          | 32             | 64             | 128            | 256  | 512            | 1024    | 2.048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOPS pro Datenträger       | Bis zu 500              | Bis zu 500              | Bis zu 500              | Bis zu 500 | Bis zu 500              | Bis zu 500              | Bis zu 500             | Bis zu 500              | Bis zu 1.300              | Bis zu 2.000              | Bis zu 2.000              |
| Durchsatz pro Datenträger | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s| Bis zu 300 MiB/s | Bis zu 500 MiB/s | Bis zu 500 MiB/s |

## <a name="billing"></a>Abrechnung

Bei der Verwendung verwalteter Datenträger gelten die folgenden Abrechnungsaspekte:

- Datenträgertyp
- Größe des verwalteten Datenträgers
- Momentaufnahmen
- Ausgehende Datenübertragungen
- Anzahl von Transaktionen

**Größe des verwalteten Datenträgers**: Die Abrechnung für verwaltete Datenträger erfolgt nach bereitgestellter Größe. Azure ordnet die bereitgestellte Größe (aufgerundet) der nächsthöheren angebotenen Datenträgergröße zu. Details zu den angebotenen Datenträgergrößen finden Sie in den obigen Tabellen. Jeder Datenträger ist einem Angebot für eine unterstützte bereitgestellte Datenträgergröße zugeordnet und wird entsprechend berechnet. Wenn Sie z.B. einen SSD Standard-Datenträger mit 200 GiB bereitstellen, wird das Datenträgerangebot E15 (256 GiB) zugeordnet. Die Abrechnung für bereitgestellte Datenträger erfolgt anteilig auf Stundenbasis unter Verwendung des monatlichen Preises für das Storage Premium-Angebot. Wenn Sie z.B. einen Datenträger des Typs E10 bereitgestellt und diesen nach 20 Stunden gelöscht haben, erfolgt die Abrechnung für das E10-Angebot anteilig für 20 Stunden. Welche Datenmenge tatsächlich auf den Datenträger geschrieben wurde, spielt keine Rolle.

**Momentaufnahmen**: Momentaufnahmen werden auf Basis der verwendeten Größe in Rechnung gestellt. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Datenträgers mit einer bereitgestellten Kapazität von 64 GiB und einer tatsächlichen Datengröße von 10 GiB erstellen, wird die Momentaufnahme nur für die in Anspruch genommene Datengröße von 10 GiB in Rechnung gestellt.
