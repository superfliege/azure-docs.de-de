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
ms.openlocfilehash: 88a4110d68dc8aa921d647f90de654d2ebb4e17d
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395712"
---
## <a name="premium-ssd"></a>SSD Premium

Azure SSD Premium bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs) mit E/A-intensiven Workloads. Um die Geschwindigkeit und Leistung von Storage Premium-Datenträgern zu nutzen, können Sie vorhandene VM-Datenträger zu SSD Premium-Datenträgern migrieren. SSD Premium ist für unternehmenskritische Produktionsanwendungen geeignet.

### <a name="disk-size"></a>Datenträgergröße
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="standard-ssd"></a>SSD Standard

Azure SSD Standard-Datenträger stellen eine kostengünstige Speichermöglichkeit dar, die für Workloads mit konstanter Leistung bei niedrigen IOPS-Werten optimiert ist. Standard-SSDs eignen sich insbesondere als Einstiegslösung zur Verlagerung von Diensten in die Cloud. Dies gilt vor allem dann, wenn bei der Ausführung vieler Workloads auf lokalen HDD-Lösungen Probleme auftreten. Im Vergleich zu Festplattenlaufwerken bieten SSD Standard-Datenträger eine bessere Verfügbarkeit, Konsistenz, Zuverlässigkeit und Latenz. SSD Standard-Datenträger eignen sich für Webserver, Anwendungsserver mit geringer IOPS-Rate, wenig genutzte Unternehmensanwendungen und Dev/Test-Workloads.

### <a name="disk-size"></a>Datenträgergröße
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

## <a name="standard-hdd"></a>HDD Standard

Azure HDD Standard bietet zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer mit Workloads, bei denen Wartezeiten eine untergeordnete Rolle spielen. Darüber hinaus unterstützt er Blobs, Tabellen, Warteschlangen und Dateien. Bei Verwendung von Standardspeicher werden die Daten auf Festplattenlaufwerken (Hard Disk Drives, HDDs) gespeichert. Wenn Sie mit virtuellen Computern arbeiten, können Sie SSD Standard- und HDD Standard-Datenträger für Dev/Test-Szenarien sowie weniger kritische Workloads verwenden. Standardspeicher ist in allen Azure-Regionen verfügbar.

### <a name="disk-size"></a>Datenträgergröße
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

## <a name="billing"></a>Abrechnung

Bei der Verwendung verwalteter Datenträger gelten die folgenden Abrechnungsaspekte:

- Datenträgertyp
- Größe des verwalteten Datenträgers
- Momentaufnahmen
- Ausgehende Datenübertragungen
- Anzahl von Transaktionen

**Größe des verwalteten Datenträgers**: Die Abrechnung für verwaltete Datenträger erfolgt nach bereitgestellter Größe. Azure ordnet die bereitgestellte Größe (aufgerundet) der nächsthöheren angebotenen Datenträgergröße zu. Details zu den angebotenen Datenträgergrößen finden Sie in den obigen Tabellen. Jeder Datenträger ist einem Angebot für eine unterstützte bereitgestellte Datenträgergröße zugeordnet und wird entsprechend berechnet. Wenn Sie z.B. einen SSD Standard-Datenträger mit 200 GiB bereitstellen, wird das Datenträgerangebot E15 (256 GiB) zugeordnet. Die Abrechnung für bereitgestellte Datenträger erfolgt anteilig auf Stundenbasis unter Verwendung des monatlichen Preises für das Storage Premium-Angebot. Wenn Sie z.B. einen Datenträger des Typs E10 bereitgestellt und diesen nach 20 Stunden gelöscht haben, erfolgt die Abrechnung für das E10-Angebot anteilig für 20 Stunden. Welche Datenmenge tatsächlich auf den Datenträger geschrieben wurde, spielt keine Rolle.

**Momentaufnahmen**: Momentaufnahmen werden auf Basis der verwendeten Größe in Rechnung gestellt. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Datenträgers mit einer bereitgestellten Kapazität von 64 GiB und einer tatsächlichen Datengröße von 10 GiB erstellen, wird die Momentaufnahme nur für die in Anspruch genommene Datengröße von 10 GiB in Rechnung gestellt.