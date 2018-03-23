---
title: Includedatei
description: Includedatei
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 30f6feb920d78c9c325c5556f5530ac4c8d9459b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
<!-- Not used for Ls-series -->

## <a name="size-table-definitions"></a>Definitionen der Größentabelle

- Speicherkapazität wird in GiB-Einheiten oder 1.024^3 Bytes angezeigt. Beachten Sie beim Vergleich von in GB (1.000^3 Bytes) gemessenen Datenträgern mit in GiB (1.024^3) gemessenen Datenträgern, dass die in GiB angegebenen Kapazitätszahlen kleiner erscheinen können. Beispiel: 1.023 GiB = 1.098,4 GB.
- Der Datenträgerdurchsatz wird in E/A-Vorgängen pro Sekunde (Input/Output Operations Per Second, IOPS) und MB/s gemessen, wobei MB/s = 10^6 Bytes/Sekunde beträgt.
- Datenträger können mit oder ohne Cache betrieben werden. Beim Datenträgerbetrieb mit Cache ist der Hostcachemodus auf **ReadOnly** oder **ReadWrite** festgelegt.  Beim Datenträgerbetrieb ohne Cache ist der Hostcachemodus auf **None**festgelegt.
-   Die beste Leistung für Ihre virtuellen Computer erhalten Sie, wenn Sie die Anzahl von Datenträgern auf zwei Datenträger pro vCPU beschränken.
- **Erwartete Netzwerkbandbreite** ist die maximale aggregierte [Bandbreite pro VM-Typ](../articles/virtual-network/virtual-machine-network-throughput.md), die NIC-übergreifend für alle Ziele zugeordnet ist. Obergrenzen werden zwar nicht garantiert, können aber bei der Wahl des passenden VM-Typs für die geplante Anwendung als Richtwert herangezogen werden. Die tatsächliche Netzwerkleistung hängt von unterschiedlichen Faktoren ab. Hierzu zählen beispielsweise Netzwerküberlastung, Anwendungslasten und die Netzwerkeinstellungen. Informationen zum Optimieren des Netzwerkdurchsatzes finden Sie unter [Optimieren des Netzwerkdurchsatzes für virtuelle Azure-Computer](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Unter Umständen muss eine bestimmte Version gewählt oder der virtuelle Computer optimiert werden, um die erwartete Netzwerkbandbreite unter Linux oder Windows zu erzielen. Weitere Informationen finden Sie unter [Testen der Bandbreite/des Durchsatzes (NTTTCP)](../articles/virtual-network/virtual-network-bandwidth-testing.md).



