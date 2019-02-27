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
ms.openlocfilehash: 0d771f03f9f71151ef25140148d4dd4daf3d46ec
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443290"
---
**Ausgehende Datenübertragungen**: [Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/bandwidth/) (Daten, die von den Azure-Datencentern ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

**Transaktionen:** Ihnen wird die Anzahl von Transaktionen berechnet, die Sie für einen verwalteten Standarddatenträger durchführen. Bei Standard-Festplattenlaufwerken berechnet Azure pro 100.000 Transaktionen eine Gebühr von 0,0036 USD. Transaktionen umfassen Lese- und Schreibvorgänge im Speicher.

SSD Standard-Datenträger verwenden eine E/A-Einheitengröße von 256 KB. Wenn die Daten, die übertragen werden, kleiner als 256 KB sind, werden sie als eine E/A-Einheit angesehen. Höhere E/A-Größen werden als mehrere Ein- bzw. Ausgaben der Größe 256 KB gezählt. Beispielsweise wird eine EA von 1.100 KB als fünf E/A-Einheiten gezählt.

Für einen verwalteten Premium-Datenträger fallen keine Kosten für Transaktionen an.

Ausführliche Informationen zu Preisen für Managed Disks finden Sie unter [Managed Disks Preise](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-ssd-vm-reservation-fee"></a>Reservierungsgebühr für SSD Ultra-VM

Virtuelle Azure-Computer können angeben, ob sie mit Ultra SSD kompatibel sind. Ein mit Ultra-Datenträgern kompatibler virtueller Computer weist dedizierte Bandbreitenkapazität zwischen der Compute-VM-Instanz und der Blockspeicher-Skalierungseinheit zu, um die Leistung zu optimieren und die Wartezeiten zu verringern. Das Hinzufügen dieser Funktion auf dem virtuellen Computer führt dazu, dass die Reservierungsgebühr nur erhoben wird, wenn Sie die Ultra-Datenträgerfunktion auf dem virtuellen Computer aktiviert haben, ohne ihr einen Ultra-Datenträger anzufügen. Wenn ein Ultra-Datenträger an den mit Ultra-Datenträgern kompatiblen virtuellen Computer angefügt ist, fällt diese Gebühr nicht an. Diese Gebühr fällt pro auf der VM bereitgestellter vCPU an.

Preisdetails für Ultra Disk-Datenträger finden Sie auf der [Seite mit der Preisübersicht für Azure-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).