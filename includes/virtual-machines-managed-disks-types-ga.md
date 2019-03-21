---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0201776914610ddaca50a670fc500156a65cd734
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2019
ms.locfileid: "58124683"
---
## <a name="premium-ssd"></a>SSD Premium

Azure SSD Premium bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs) mit E/A-intensiven Workloads. Um die Geschwindigkeit und Leistung von Storage Premium-Datenträgern zu nutzen, können Sie vorhandene VM-Datenträger zu SSD Premium-Datenträgern migrieren. SSD Premium ist für unternehmenskritische Produktionsanwendungen geeignet.

### <a name="disk-size"></a>Datenträgergröße

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| SSD Premium-Größen | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60* | P70* | P80* |
|-------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Datenträgergröße in GiB | 32 | 64 | 128 | 256 | 512 | 1024 | 2.048 | 4.095 | 8.192 | 16.384 | 32.767 |
| IOPS pro Datenträger | Bis zu 120 | Bis zu 240 | Bis zu 500 | Bis zu 1.100 | Bis zu 2.300 | Bis zu 5.000 | Bis zu 7.500 | Bis zu 7.500 | Bis zu 12.500 | Bis zu 15.000 | Bis zu 20.000 |
| Durchsatz pro Datenträger | Bis zu 25 MiB/s | Bis zu 50 MiB/s | Bis zu 100 MiB/s | Bis zu 125 MiB/s | Bis zu 150 MiB/s | Bis zu 200 MiB/s | Bis zu 250 MiB/s | Bis zu 250 MiB/s| Bis zu 480 MiB/s | Bis zu 750 MiB/s | Bis zu 750 MiB/s |

## <a name="standard-ssd"></a>SSD Standard

Azure SSD Standard-Datenträger stellen eine kostengünstige Speichermöglichkeit dar, die für Workloads mit konstanter Leistung bei niedrigen IOPS-Werten optimiert ist. Standard-SSDs eignen sich insbesondere als Einstiegslösung zur Verlagerung von Diensten in die Cloud. Dies gilt vor allem dann, wenn bei der Ausführung vieler Workloads auf lokalen HDD-Lösungen Probleme auftreten. Im Vergleich zu Festplattenlaufwerken bieten SSD Standard-Datenträger eine bessere Verfügbarkeit, Konsistenz, Zuverlässigkeit und Latenz. SSD Standard-Datenträger eignen sich für Webserver, Anwendungsserver mit geringer IOPS-Rate, wenig genutzte Unternehmensanwendungen und Dev/Test-Workloads.

### <a name="disk-size"></a>Datenträgergröße

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| SSD Standard-Größen | E4 | E6 | E10 | E15 | E20 | E30 | E40 | E50 | E60* | E70* | E80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Datenträgergröße in GiB | 32 | 64 | 128 | 256 | 512 | 1024 | 2.048 | 4.095 | 8.192 | 16.384 | 32.767 |
| IOPS pro Datenträger | Bis zu 120 | Bis zu 240 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 1.300 | Bis zu 2.000 | Bis zu 2.000 |
| Durchsatz pro Datenträger |  Bis zu 25 MiB/s |  Bis zu 50 MiB/s  |  Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s| Bis zu 300 MiB/s |  Bis zu 500 MiB/s | Bis zu 500 MiB/s |

## <a name="standard-hdd"></a>HDD Standard

Azure HDD Standard bietet zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer mit Workloads, bei denen Wartezeiten eine untergeordnete Rolle spielen. Darüber hinaus unterstützt er Blobs, Tabellen, Warteschlangen und Dateien. Bei Verwendung von Standardspeicher werden die Daten auf Festplattenlaufwerken (Hard Disk Drives, HDDs) gespeichert. Wenn Sie mit virtuellen Computern arbeiten, können Sie SSD Standard- und HDD Standard-Datenträger für Dev/Test-Szenarien sowie weniger kritische Workloads verwenden. Standardspeicher ist in allen Azure-Regionen verfügbar.

### <a name="disk-size"></a>Datenträgergröße

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| Standard-Datenträgertyp | S4 | S6 | S10 | S15 | S20 | S30 | S40 | S50 | S60* | S70* | S80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Datenträgergröße in GiB | 32 | 64 | 128 | 256 | 512 | 1024 | 2.048 | 4.095 | 8.192 | 16.384 | 32.767 |
| IOPS pro Datenträger | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 500 | Bis zu 1.300 | Bis zu 2.000 | Bis zu 2.000 |
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