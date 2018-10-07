---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: bb9a2a884439b00f52adfa9b7c1010a4610a77f7
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47401656"
---
# <a name="ultra-ssd-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Verwaltete SSD Ultra-Datenträger (Preview) für Workloads virtueller Azure-Computer

Azure SSD Ultra (Preview) bietet hohen Durchsatz, hohe IOPS und konsistenten Datenträgerspeicher mit niedrigen Wartezeiten für Azure IaaS-VMs. Dieses neue Angebot bietet Spitzenleistung auf den gleichen Verfügbarkeitsebenen wie unsere vorhandenen Datenträgerangebote. Zu den weiteren Vorteilen von SSD Ultra gehört die Möglichkeit, die Datenträgerleistung dynamisch in Abstimmung mit Ihren Workloads ändern zu können, ohne Ihre virtuellen Computer neu starten zu müssen. SSD Ultra eignet sich für datenintensive Workloads wie SAP HANA, führende Datenbanken und Workloads mit vielen Transaktionen.

## <a name="ultra-ssd-features"></a>SSD Ultra-Features

**Verwaltete Datenträger:** SSD Ultra-Datenträger sind nur als verwaltete Datenträger verfügbar. SSD Ultra-Datenträger können nicht als nicht verwaltete Datenträger oder als Seitenblobs bereitgestellt werden. Beim Erstellen eines verwalteten Datenträgers geben Sie den SKU-Typ des Datenträgers als „UltraSSD_LRS“ an sowie die IOPS und den benötigten Durchsatz. Azure erstellt und verwaltet daraufhin den Datenträger für Sie.  

**Virtuell Computer**: SSD Ultra-Datenträger sind darauf ausgelegt, mit allen SSD Premium-aktivierten Azure Virtual Machine-SKUs zu funktionieren. Zum Zeitpunkt der Preview sind die VM-Größen allerdings noch auf ES/DS v3 VM-Instanzen beschränkt.

**Dynamische Leistungskonfiguration**: SSD Ultra-Datenträger erlauben es Ihnen, die Datenträgerleistung (IOPS und Durchsatz) dynamisch in Abstimmung mit Ihren Workloadanforderungen ändern zu können, ohne Ihre virtuellen Computer neu starten zu müssen.

## <a name="scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele

Wenn Sie eine SSD Ultra bereitstellen, haben Sie die Option, die Kapazität und die Leistung des Datenträgers unabhängig voneinander zu konfigurieren. SSD Ultra-Datenträger sind in mehreren festen Größen von 4 GiB bis 64 TiB erhältlich und bieten ein flexibles Leistungskonfigurationsmodell, durch das Sie IOPS und Durchsatz unabhängig konfigurieren können. SSD Ultra-Datenträger können nur als Datenträger für Daten genutzt werden. Wir empfehlen, SSD Premium-Datenträger als Betriebssystemdatenträger zu verwenden.

Einige Hauptfunktionen von SSD Ultra-Datenträgern sind:

- Datenträgerkapazität: SSD Ultra bietet Ihnen verschiedene Datenträgergrößen zwischen 4 GiB und 64 TiB.
- Datenträger-IOPS: SSD Ultra-Datenträger unterstützen IOPS-Limits von 300 IOPS/GiB bis hin zu maximal 160.000 IOPS pro Datenträger. Um die bereitgestellten IOPS-Werte zu erreichen, stellen Sie sicher, dass der IOPS-Wert für den ausgewählten Datenträger unter dem für den virtuellen Computer liegt. Der Mindestwert für Datenträger-IOPS beträgt 100 IOPS.
- Datenträgerdurchsatz: Mit SSD Ultra-Datenträgern beträgt das Durchsatzlimit für einen einzelnen Datenträger 256 KiB/s für jeden bereitgestellten IOPS-Wert bis zu maximal 2000 MB/s pro Datenträger (dabei ist 1 MB/s = 10^6 Byte pro Sekunde). Der Mindest-Datenträgerdurchsatz beträgt 1 MiB.

Die folgende Tabelle fasst die unterstützten Konfigurationen für die unterschiedlichen Datenträgergrößen zusammen:  

### <a name="ultra-ssd-managed-disk-offerings"></a>Verwaltete SSD Ultra-Datenträger – Angebote

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

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei Verwendung von SSD Ultra-Datenträgern sind folgende Abrechnungsaspekte zu berücksichtigen:

- Größe der verwalteten Datenträger
- Bereitgestellte IOPS der verwalteten Datenträger
- Bereitgestellter Durchsatz der verwalteten Datenträger
- Reservierungsgebühr für SSD Ultra-VM

### <a name="managed-disk-size"></a>Größe der verwalteten Datenträger

Verwaltete Datenträger werden nach bereitgestellter Größe abgerechnet. Azure ordnet die bereitgestellte Größe (aufgerundet) dem Angebot für die nächsthöhere Datenträgergröße zu. Details zu den angebotenen Datenträgergrößen finden Sie in der Tabelle im obigen Abschnitt zu Skalierbarkeits- und Leistungszielen. Jeder Datenträger wird der bereitgestellten Datenträgergröße zugeordnet und entsprechend auf Stundenbasis abgerechnet. Wenn Sie z.B. einen SSD Ultra-Datenträger mit 200 GiB bereitstellen und ihn nach 20 Stunden löschen, wird der Datenträger dem Datenträgerangebot mit 256 GiB zugeordnet, und Ihnen werden über einen Zeitraum von 20 Stunden 256 GiB in Rechnung gestellt. Welche Datenmenge tatsächlich auf den Datenträger geschrieben wurde, spielt keine Rolle.

### <a name="managed-disk-provisioned-iops"></a>Bereitgestellte IOPS der verwalteten Datenträger

IOPS ist die Anzahl von Anforderungen, die Ihre Anwendung pro Sekunde an die Datenträger sendet. Ein E/A-Vorgang kann sequenziell oder zufällig, ein Lese- oder ein Schreibvorgang sein. Ebenso wie bei der Datenträgergröße erfolgt die Abrechnung der bereitgestellten IOP auf Stundenbasis. Details zu den angebotenen Datenträger-IOPS finden Sie in der Tabelle im obigen Abschnitt zu Skalierbarkeits- und Leistungszielen.

### <a name="managed-disk-provisioned-throughput"></a>Bereitgestellter Durchsatz der verwalteten Datenträger

Durchsatz ist die Menge der Daten, die Ihre Anwendung in einem angegebenen Intervall an die Datenträger überträgt, gemessen in Bytes/Sekunde. Wenn Ihre Anwendung umfangreiche E/A-Vorgänge mit durchführt, benötigt sie hohen Durchsatz.  

Zwischen Durchsatz und IOPS gibt es, wie in der folgenden Formel dargestellt, eine Beziehung: IOPS x E/A-Größe = Durchsatz

Aus diesem Grund ist es wichtig, die optimalen Durchsatz- und IOPS-Werte zu bestimmen, die Ihre Anwendung benötigt. Beim Versuch, einen der Faktoren zu optimieren, ist der andere ebenfalls betroffen. Wir empfehlen, mit einem Durchsatz zu beginnen, der einer E/A-Größe von 16 KiB entspricht, und diesen dann anzupassen, wenn ein höherer Durchsatz benötigt wird.

Details zum unterstützten Durchsatz bei SSD Ultra finden Sie in der Tabelle im obigen Abschnitt zu Skalierbarkeits- und Leistungszielen. Ebenso wie bei der Datenträgergröße und IOPS wird der bereitgestellte Durchsatz auf stündlicher Basis pro bereitgestellten MBit/s abgerechnet.

### <a name="ultra-ssd-vm-reservation-fee"></a>Reservierungsgebühr für SSD Ultra-VM

Wir führen eine Funktion auf dem virtuellen Computer ein, die angibt, dass Ihr virtueller Computer SSD Ultra-kompatibel ist. Eine SSD Ultra-kompatible VM weist dedizierte Bandbreitenkapazität zwischen der Compute-VM-Instanz und der Blockspeicher-Skalierungseinheit zu, um die Leistung zu optimieren und die Wartezeiten zu verringern. Das Hinzufügen dieser Funktion auf der VM führt dazu, dass die Reservierungsgebühr nur erhoben wird, wenn Sie die SSD Ultra-Funktion auf der VM aktiviert haben, ohne ihr einen SSD Ultra-Datenträger anzufügen. Wenn der SSD Ultra-kompatiblen VM ein SSD Ultra-Datenträger angefügt ist bzw. wird, würde diese Gebühr nicht anfallen. Diese Gebühr fällt pro auf der VM bereitgestellter vCPU an.

Die neuen Preisdetails zu den in der begrenzten Preview verfügbaren SSD Ultra-Datenträger finden Sie auf der [Seite mit der Preisübersicht für Azure-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="ultra-ssd-preview-scope-and-limitations"></a>Umfang und Einschränkungen der SSD Ultra Preview

In der Preview gilt für SSD Ultra-Datenträger:

- Anfängliche Unterstützung „USA, Osten 2“ in einer einzelnen Verfügbarkeitszone.  
- Können nur mit Verfügbarkeitszonen verwendet werden (Verfügbarkeitsgruppen und einzelnen VM-Bereitstellungen außerhalb der Zonen können keine SSD Ultra-Datenträger anfügen).
- Werden nur auf ES/DS v3 VMs unterstützt.
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4K.  
- Können nur als leere Datenträger erstellt werden.  
- Können aktuell nur mit Resource Manager-Vorlagen, CLI und Python SDK bereitgestellt werden.
- Unterstützen (noch) keine Momentaufnahmen von Datenträgern, VM-Images, Verfügbarkeitsgruppen, Virtual Machine Scale Sets und Azure Disk Encryption.
- Unterstützen (noch) keine Integration mit Azure Backup oder Azure Site Recovery.
- Wie bei den  [meisten Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) sollte diese Funktion bis zum Erreichen der allgemeinen Verfügbarkeit nicht für Produktionsworkloads verwendet werden.