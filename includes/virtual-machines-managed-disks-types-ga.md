---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/14/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 30df480eb314594cbc4d949302aff11e6d764b6f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147864"
---
## <a name="premium-ssd"></a>SSD Premium

Azure SSD Premium bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs) mit E/A-intensiven Workloads. Um die Geschwindigkeit und Leistung von Storage Premium-Datenträgern zu nutzen, können Sie vorhandene VM-Datenträger zu SSD Premium-Datenträgern migrieren. SSD Premium ist für unternehmenskritische Produktionsanwendungen geeignet. SSD Premium kann nur mit VM-Serien verwendet werden, die mit Storage Premium kompatibel sind.

Weitere Informationen zu den einzelnen VM-Typen und -Größen in Azure für Windows (einschließlich der Größen, die mit Storage Premium kompatibel sind) finden Sie unter [Windows-VM-Größen](../articles/virtual-machines/windows/sizes.md). Weitere Informationen zu den einzelnen VM-Typen und -Größen in Azure für Linux (einschließlich der Größen, die mit Storage Premium kompatibel sind) finden Sie unter [Linux-VM-Größen](../articles/virtual-machines/linux/sizes.md).

### <a name="disk-size"></a>Datenträgergröße
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Im Gegensatz zu einem Standard-Speicherdatenträger sind bei der Bereitstellung eines Storage Premium-Datenträgers die Kapazität, die IOPS und der Durchsatz dieses Datenträgers garantiert. Wenn Sie beispielsweise einen P50-Datenträger erstellen, stellt Azure eine Speicherkapazität von 4.095 GB, 7.500 IOPS und einen Durchsatz von 250 MB/s für diesen Datenträger bereit. Die Anwendung kann die Kapazität und Leistung ganz oder teilweise nutzen. SSD Premium-Datenträger sind darauf ausgelegt, 99,9 Prozent der Zeit die Zielleistung bereitzustellen.

### <a name="transactions"></a>Transaktionen

Für SSD Premium gilt jeder E/A-Vorgang kleiner oder gleich 256 KiB Durchsatz als einzelner E/A-Vorgang. E/A-Vorgänge mit einem Durchsatz größer als 256 KiB gelten als mehrere Ein-bzw. Ausgabevorgänge der Größe 256 KiB.

## <a name="standard-ssd"></a>SSD Standard

Azure SSD Standard-Datenträger stellen eine kostengünstige Speichermöglichkeit dar, die für Workloads mit konstanter Leistung bei niedrigen IOPS-Werten optimiert ist. Standard-SSDs eignen sich insbesondere als Einstiegslösung zur Verlagerung von Diensten in die Cloud. Dies gilt vor allem dann, wenn bei der Ausführung vieler Workloads auf lokalen HDD-Lösungen Probleme auftreten. Im Vergleich zu HDD Standard bieten SSD Standard-Datenträger eine bessere Verfügbarkeit, Konsistenz, Zuverlässigkeit und Latenz. SSD Standard-Datenträger eignen sich für Webserver, Anwendungsserver mit geringer IOPS-Rate, wenig genutzte Unternehmensanwendungen und Dev/Test-Workloads. Wie HDD Standard-Datenträger sind SSD Standard-Datenträger für alle Azure-VMs verfügbar.

### <a name="disk-size"></a>Datenträgergröße
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Standard-SSDs sind dafür ausgelegt, 99 Prozent der Zeit Wartezeiten im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge zu erzielen sowie IOPS und Durchsätze bis zu den in der vorherigen Tabelle genannten Grenzwerten bereitzustellen. Die Istwerte für IOPS und Durchsätze können in einigen Fällen je nach Datenverkehrsmuster variieren. Standard-SSDs stellen eine konstantere Leistung und geringere Latenzen als HDD-Datenträger bereit.

### <a name="transactions"></a>Transaktionen

Für SSD Standard gilt jeder E/A-Vorgang kleiner oder gleich 256 KiB Durchsatz als ein einzelner E/A-Vorgang. E/A-Vorgänge mit einem Durchsatz größer als 256 KiB gelten als mehrere Ein-bzw. Ausgabevorgänge der Größe 256 KiB. Diese Transaktionen besitzen Auswirkungen auf die Abrechnung.

## <a name="standard-hdd"></a>HDD Standard

Azure HDD Standard bietet zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer mit Workloads, bei denen Wartezeiten eine untergeordnete Rolle spielen. Darüber hinaus unterstützt er Blobs, Tabellen, Warteschlangen und Dateien. Bei Verwendung von Standardspeicher werden die Daten auf Festplattenlaufwerken (Hard Disk Drives, HDDs) gespeichert. Wenn Sie mit virtuellen Computern arbeiten, können Sie SSD Standard- und HDD Standard-Datenträger für Dev/Test-Szenarien sowie weniger kritische Workloads verwenden. HDD Standard-Datenträger sind in allen Azure-Regionen verfügbar und können mit allen Azure-VMs verwendet werden.

### <a name="disk-size"></a>Datenträgergröße
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>Transaktionen

Für HDD Standard wird jeder E/A-Vorgang als einzelne Transaktion betrachtet, und zwar unabhängig von der E/A-Größe. Diese Transaktionen besitzen Auswirkungen auf die Abrechnung.

## <a name="billing"></a>Abrechnung

Bei der Verwendung verwalteter Datenträger gelten die folgenden Abrechnungsaspekte:

- Datenträgertyp
- Größe des verwalteten Datenträgers
- Momentaufnahmen
- Ausgehende Datenübertragungen
- Anzahl von Transaktionen

**Größe des verwalteten Datenträgers**: Die Abrechnung für verwaltete Datenträger erfolgt nach bereitgestellter Größe. Azure ordnet die bereitgestellte Größe (aufgerundet) der nächsthöheren angebotenen Datenträgergröße zu. Details zu den angebotenen Datenträgergrößen finden Sie in den obigen Tabellen. Jeder Datenträger ist einem Angebot für eine unterstützte bereitgestellte Datenträgergröße zugeordnet und wird entsprechend berechnet. Wenn Sie z.B. einen SSD Standard-Datenträger mit 200 GiB bereitstellen, wird das Datenträgerangebot E15 (256 GiB) zugeordnet. Die Abrechnung für bereitgestellte Datenträger erfolgt anteilig auf Stundenbasis unter Verwendung des monatlichen Preises für das Storage Premium-Angebot. Wenn Sie z.B. einen Datenträger des Typs E10 bereitgestellt und diesen nach 20 Stunden gelöscht haben, erfolgt die Abrechnung für das E10-Angebot anteilig für 20 Stunden. Welche Datenmenge tatsächlich auf den Datenträger geschrieben wurde, spielt keine Rolle.

**Momentaufnahmen**: Momentaufnahmen werden auf Basis der verwendeten Größe in Rechnung gestellt. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Datenträgers mit einer bereitgestellten Kapazität von 64 GiB und einer tatsächlichen Datengröße von 10 GiB erstellen, wird die Momentaufnahme nur für die in Anspruch genommene Datengröße von 10 GiB in Rechnung gestellt.