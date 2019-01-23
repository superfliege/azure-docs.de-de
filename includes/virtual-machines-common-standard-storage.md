---
title: Includedatei
description: Includedatei
services: storage
author: yuemlu
ms.service: storage
ms.topic: include
ms.date: 01/08/2019
ms.author: yuemlu
ms.custom: include file
ms.openlocfilehash: ad57d373422e0fc310e51ac31f2a2e76999abf22
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193381"
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Kostengünstiger Standardspeicher und verwaltete/nicht verwaltete Azure-VM-Datenträger

Azure-Standardspeicher bietet zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer mit latenzempfindlichen Workloads. Darüber hinaus unterstützt er Blobs, Tabellen, Warteschlangen und Dateien. Bei Verwendung von Standardspeicher werden die Daten auf Festplattenlaufwerken (Hard Disk Drives, HDDs) gespeichert. Wenn Sie mit virtuellen Computern arbeiten, können Sie Standard-SSD- und -HDD-Datenträger für Dev/Test-Szenarien sowie weniger kritische Workloads und Premium-SSD-Datenträger für unternehmenskritische Produktionsanwendungen verwenden. Standardspeicher ist in allen Azure-Regionen verfügbar. 

Dieser Artikel beschäftigt sich hauptsächlich mit der Verwendung von Standard-SSD- und -HDD-Datenträgern. Weitere Informationen zur Verwendung von Speicher mit Blobs, Tabellen, Warteschlangen und Dateien finden Sie in der [Einführung in Storage](../articles/storage/common/storage-introduction.md).

## <a name="disk-types"></a>Datenträgertypen

Standarddatenträger für virtuelle Azure-Computer können in zwei Varianten erstellt werden:

**Nicht verwaltete Datenträger:** Bei diesem Datenträgertyp verwalten Sie die Speicherkonten, die zum Speichern der VHD-Dateien für die VM-Datenträger verwendet werden. VHD-Dateien werden als Seitenblobs in Speicherkonten gespeichert. Nicht verwaltete Datenträger können an jede beliebige Azure-VM-Größe angefügt werden – auch an virtuelle Computer, die in erster Linie Storage Premium verwenden (wie etwa die DSv2- und die GS-Serie). Virtuelle Azure-Computer unterstützen das Anfügen mehrerer Standarddatenträger. Dadurch kann pro VM eine Speicherkapazität von bis zu 256 TiB erreicht werden. Wenn Sie die Datenträgergrößen der Vorschau verwenden, können Sie pro virtuellem Computer bis zu 2 PiB Speicher verwenden.

[**Azure Managed Disks**](../articles/virtual-machines/windows/managed-disks-overview.md): Dieses Feature nimmt Ihnen die Verwaltung der Speicherkonten ab, die für die VM-Datenträger verwendet werden. Sie geben die Art (Premium-SSD, Standard-SSD oder Standard-HDD) und die benötigte Größe des Datenträgers an, und Azure erstellt und verwaltet ihn für Sie. Sie müssen die Datenträger nicht selbst auf mehrere Speicherkonten verteilen, um sicherzustellen, dass Sie die Skalierbarkeitsgrenzwerte für die Speicherkonten einhalten. Dies erledigt Azure für Sie.

Grundsätzlich stehen Ihnen zwar beide Arten von Datenträgern zur Verfügung, aber wir empfehlen Ihnen die Verwendung von Managed Disks, da Sie bei dieser Variante von zahlreichen Features profitieren.

Informationen zu den ersten Schritten mit Azure-Standardspeicher finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/). 

Informationen zum Erstellen eines virtuellen Computers mit Managed Disks finden Sie in den folgenden Artikeln.

* [Erstellen eines virtuellen Computers mithilfe von Resource Manager und PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)
* [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](../articles/virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Features von Standardspeicher

In diesem Abschnitt werden einige der Features von Standardspeicher erläutert. Ausführlichere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](../articles/storage/common/storage-introduction.md).

**Storage Standard**: Azure Storage Standard unterstützt Azure-Datenträger, Azure-Blobs, Azure Files, Azure-Tabellen und Azure-Warteschlangen. Wenn Sie Standardspeicherdienste verwenden möchten, beginnen Sie mit der [Erstellung eines Azure Storage-Kontos](../articles/storage/common/storage-quickstart-create-account.md).

**SSD Standard-Datenträger**: SSD Standard-Datenträger bieten eine zuverlässigere Leistung als HDD Standard-Datenträger und sind derzeit verfügbar. Weitere Informationen zur Verfügbarkeit von SSD Standard-Datenträgern in verschiedenen Regionen finden Sie unter [Regionale Verfügbarkeit von SSD Standard-Datenträgern](../articles/virtual-machines/windows/faq-for-disks.md#standard-ssds-azure-regions).

**HDD Standard-Datenträger**: HDD Standard-Datenträger können an beliebige virtuelle Azure-Computer angefügt werden. Dies schließt auch virtuelle Computer aus Größenserien mit Storage Premium (beispielsweise die DSv2- und die GS-Serie) mit ein. Ein Standard-HDD-Datenträger kann nur an einen einzelnen virtuellen Computer angefügt werden. An einen virtuellen Computer können allerdings mehrere dieser Datenträger angefügt werden (bis zur maximalen, für die VM-Größe definierten Datenträgeranzahl). Die Spezifikationen werden im folgenden Abschnitt zu Skalierbarkeits- und Leistungszielen für Standardspeicher ausführlicher beschrieben.

**Standardseitenblob**: Standardseitenblobs dienen zum Speichern persistenter Datenträger für virtuelle Computer, und Sie können wie bei anderen Arten von Azure-Blobs direkt über REST auf sie zugreifen. Bei [Seitenblobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) handelt es sich um eine Sammlung von 512-Byte-Seiten, die für zufällige Lese- und Schreibvorgänge optimiert sind. 

**Speicherreplikation**: In den meisten Regionen können in Standardspeicherkonten gespeicherte Daten lokal oder georedundant in mehreren Rechenzentren repliziert werden. Zur Verfügung stehen vier Replikationsarten: lokal redundanter Speicher (LRS), zonenredundanter Speicher (ZRS), georedundanter Speicher (GRS) und georedundanter Speicher mit Lesezugriff (RA-GRS). Für Managed Disks in Standardspeicher wird derzeit nur lokal redundanter Speicher (LRS) unterstützt. Weitere Informationen finden Sie unter [Azure Storage-Replikation](../articles/storage/common/storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele

In diesem Abschnitt werden die Skalierbarkeits- und Leistungsziele beschrieben, die bei der Verwendung von Standardspeicher zu berücksichtigen sind.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Kontogrenzwerte (gelten nicht für verwaltete Datenträger)

| **Ressource** | **Standardlimit** |
|--------------|-------------------|
| TB pro Speicherkonto  | 500 TB |
| Max. Eingang<sup>1</sup> pro Speicherkonto (US-Regionen) | 10 GBit/s bei aktiviertem GRS/ZRS, 20 GBit/s für LRS |
| Max. Ausgang<sup>1</sup> pro Speicherkonto (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS/ZRS, 30 GBit/s für LRS |
| Max. Eingang<sup>1</sup> pro Speicherkonto (europäische und asiatische Regionen) | 5 GBit/s bei aktiviertem GRS/ZRS, 10 GBit/s für LRS |
| Max. Ausgang<sup>1</sup> pro Speicherkonto (europäische und asiatische Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS/ZRS, 15 GBit/s für LRS |
| Gesamtanfragerate (ausgehend von einer Objektgröße von 1 KB) pro Speicherkonto | Bis zu 20.000 IOPS, Entitäten pro Sekunde oder Nachrichten pro Sekunde |

<sup>1</sup> „Eingang“ bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. „Ausgang“ bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.

Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungszielen für Azure Storage](../articles/storage/common/storage-scalability-targets.md).

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, erstellen Sie die Anwendung so, dass mehrere Speicherkonten verwendet werden, und partitionieren Sie Ihre Daten in diesen Speicherkonten. Alternativ dazu können Sie Azure Managed Disks verwenden und die Verwaltung der Partitionierung und Platzierung von Daten Azure überlassen.

### <a name="standard-disks-limits"></a>Grenzwerte für Standarddatenträger

Im Gegensatz zu Premium-Datenträgern werden für Standarddatenträger keine E/A-Vorgänge pro Sekunde (Input/Output Operations Per Second, IOPS) und kein Durchsatz (Bandbreite) bereitgestellt. Die Leistung von Standarddatenträgern hängt von der Größe des virtuellen Computers ab, dem der jeweilige Datenträger angefügt ist, und der Größe des Datenträgers.

Wenn Ihre Workload Datenträgerunterstützung für hohe Leistung und geringe Latenz erfordert, empfiehlt sich unter Umständen die Verwendung von Storage Premium. Weitere Informationen zu den Vorteilen von Storage Premium finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md).

## <a name="snapshots-and-copy-blob"></a>Momentaufnahmen und Kopieren von Blobs

Für den Storage-Dienst ist die VHD-Datei ein Seitenblob. Sie können Momentaufnahmen von Seitenblobs erstellen und sie an einen anderen Ort kopieren (etwa in ein anderes Speicherkonto).

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

Für nicht verwaltete Standarddatenträger können [inkrementelle Momentaufnahmen](../articles/virtual-machines/windows/incremental-snapshots.md) erstellt werden. Die Vorgehensweise ist dabei die gleiche wie bei Momentaufnahmen für Standardspeicher. Falls sich Ihr Quelldatenträger in einem lokal redundanten Speicherkonto befindet, empfiehlt es sich, Momentaufnahmen zu erstellen und in ein georedundantes Standardspeicherkonto zu kopieren. Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](../articles/storage/common/storage-redundancy.md).

Falls ein Datenträger an einen virtuellen Computer angefügt ist, sind bestimmte API-Vorgänge für die Datenträger nicht zulässig. Sie können beispielsweise den Vorgang [Kopieren von Blob](/rest/api/storageservices/Copy-Blob) für das Blob nicht durchführen, solange der Datenträger an den virtuellen Computer angeschlossen ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme dieses Blobs, indem Sie die REST-API-Methode [Momentaufnahme-Blob](/rest/api/storageservices/Snapshot-Blob) verwenden und anschließend den Vorgang [Kopieren von Blob](/rest/api/storageservices/Copy-Blob) der Momentaufnahme durchführen, um den angeschlossenen Datenträger zu kopieren. Alternativ können Sie den Datenträger auch trennen und dann die erforderlichen Vorgänge ausführen.

Zur Aufbewahrung georedundanter Kopien Ihrer Momentaufnahmen können Sie Momentaufnahmen mithilfe von „AzCopy“ oder „Copy Blob“ aus einem lokal redundanten Speicherkonto in ein georedundantes Standardspeicherkonto kopieren. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../articles/storage/common/storage-use-azcopy.md) und [Copy Blob](/rest/api/storageservices/Copy-Blob).

Ausführliche Informationen zum Ausführen von REST-Vorgängen für Seitenblobs in Standardspeicherkonten finden Sie in der [Referenz zur REST-API für Azure Storage Services](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Verwaltete Datenträger

Bei einer Momentaufnahme für einen verwalteten Datenträger handelt es sich um eine schreibgeschützte Kopie des verwalteten Datenträgers, die als verwalteter Standarddatenträger gespeichert wird. Für Managed Disks werden noch keine inkrementellen Momentaufnahmen unterstützt, dies ist jedoch bereits geplant.

Falls ein verwalteter Datenträger an einen virtuellen Computer angefügt ist, sind bestimmte API-Vorgänge für die Datenträger nicht zulässig. So können Sie beispielsweise keine Shared Access Signature (SAS) generieren, um einen Kopiervorgang auszuführen, während der Datenträger an einen virtuellen Computer angefügt ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme des Datenträgers, und führen Sie dann den Kopiervorgang für die Momentaufnahme aus. Alternativ können Sie den Datenträger trennen und anschließend eine Shared Access Signature (SAS) generieren, um den Kopiervorgang auszuführen.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei Verwendung von Standardspeicher sind folgende Abrechnungsaspekte zu berücksichtigen:

* Nicht verwaltete Standardspeicher-Datenträger/Datengröße
* Verwaltete Standarddatenträger
* Standardspeicher-Momentaufnahmen
* Ausgehende Datenübertragungen
* Transaktionen

**Nicht verwaltete Speicherdaten und Datenträgergröße**: Bei nicht verwalteten Datenträgern und anderen Daten (Blobs, Tabellen, Warteschlangen und Dateien) fallen nur Kosten für den genutzten Speicherplatz an. Wenn Sie also beispielsweise über einen virtuellen Computer verfügen, bei dem das Seitenblob mit 127 GB bereitgestellt ist, aber nur 10 GB des Speicherplatzes tatsächlich genutzt werden, werden nur 10 GB in Rechnung gestellt. Standardspeicher bis zu 8191 GB und nicht verwaltete Standard-Datenträger bis zu 4.095 GB werden unterstützt. 

**Verwaltete Datenträger**: Die Abrechnung für verwaltete Standard-Datenträger richtet sich nach der bereitgestellten Datenträgergröße. Azure ordnet die bereitgestellte Größe (aufgerundet) der nächstgelegenen Managed Disks-Option zu. Dies ist in den Tabellen unten angegeben. Jeder verwaltete Datenträger wird einer der unterstützten bereitgestellten Größen zugeordnet und entsprechend abgerechnet. Wenn Sie beispielsweise einen verwalteten Standard-Datenträger erstellen und eine bereitgestellte Größe von 200 GiB angeben, erfolgt die Abrechnung gemäß den Preisen für den Datenträgertyp S15.

Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

| **Verwalteter Standard-HDD-<br>Datenträgertyp** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** | **S60*** | **S70*** | **S80*** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Datenträgergröße        | 32 GiB  | 64 GiB  | 128 GB | 256 GiB | 512 GB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 8192 GiB (8 TiB) | 16385 GiB (16 TiB) | 32767 GiB (32 TiB) |


**Momentaufnahmen**: Für Momentaufnahmen von Standarddatenträgern wird die zusätzliche Kapazität in Rechnung gestellt, die von den Momentaufnahmen beansprucht wird. Informationen zu Momentaufnahmen finden Sie unter [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Ausgehende Datenübertragungen**: [Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/) (Daten, die von den Azure-Rechenzentren ausgehen) verursachen Kosten für die Bandbreitennutzung.

**Transaktion**: Bei Azure-Standardspeicher fällt pro 100.000 Transaktionen eine Gebühr von 0,0036 USD an. Transaktionen umfassen Lese- und Schreibvorgänge im Speicher.

Ausführliche Informationen zu den Preisen für Standardspeicher, Virtual Machines und Managed Disks finden Sie unter den folgenden Links:

* [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Preisgestaltung für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Preise für Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Azure Backup-Dienst – Unterstützung

Virtuelle Azure-Computer mit nicht verwalteten Datenträgern können mithilfe von Azure Backup gesichert werden. [Weitere Informationen](../articles/backup/backup-azure-vms-first-look-arm.md)

Der Azure Backup-Dienst kann auch in Kombination mit Managed Disks verwendet werden, um einen Sicherungsauftrag mit zeitbasierten Sicherungen, unkomplizierter Wiederherstellung von virtuellen Computern und Aufbewahrungsrichtlinien für Sicherungen zu erstellen. Weitere Informationen hierzu finden Sie im Artikel zur [Verwendung des Azure Backup-Diensts für virtuelle Computer mit Managed Disks](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Storage](../articles/storage/common/storage-introduction.md)

* [Erstellen eines Speicherkontos](../articles/storage/common/storage-quickstart-create-account.md)

* [Managed Disks Overview](../articles/virtual-machines/linux/managed-disks-overview.md) (Übersicht über Azure Managed Disks)

* [Erstellen eines virtuellen Computers mithilfe von Resource Manager und PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)

* [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](../articles/virtual-machines/linux/quick-create-cli.md)
