---
title: Includedatei
description: Includedatei
services: storage
author: rogara
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f64645db782b055e1c544f257149411f29fc99d7
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34806314"
---
## <a name="about-vhds"></a>Informationen zu VHDs
Die in Azure verwendeten VHDs sind VHD-Dateien, die als Seiten-Blobs in einem Standard- oder Premium-Speicherkonto in Azure gespeichert sind. Informationen zu Seitenblobs finden Sie unter [Grundlegendes zu Blockblobs und Seitenblobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Ausführliche Informationen zu Storage Premium finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md).

Azure unterstützt das VHD-Format mit eingebauten („festen“) Datenträgern. Das feste Format legt den logischen Datenträger in der Datei linear aus, daher wird Datenträger-Offset X bei Blob-Offset X gespeichert. Eine kleinere Fußzeile am Ende des Blobs beschreibt die Eigenschaften der VHD. Oftmals verschwendet das feste Format Speicherplatz, da die meisten Datenträger über große ungenutzte Bereiche davon verfügen. Azure speichert VHD-Dateien jedoch in einem platzsparenden Format. Daher profitieren Sie gleichzeitig von den Vorteilen fester und dynamischer Datenträger. Nähere Informationen finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle für die Erstellung von Datenträgern oder Images verwenden möchten, sind schreibgeschützt. Eine Ausnahme bilden die vom Benutzer hochgeladenen oder in Azure Storage kopierten VHD-Dateien (die entweder Lese-/Schreibzugriff bieten oder schreibgeschützt sein können). Beim Erstellen eines Datenträgers oder Images werden Kopien der VHD-Quelldateien von Azure erstellt. Diese Kopien sind je nach Verwendung der VHD entweder schreibgeschützt oder ermöglichen Lese- und Schreibvorgänge.

Beim Erstellen eines virtuellen Computers mithilfe eines Images, erstellt Azure einen Datenträger für den virtuellen Computer, bei dem es sich um eine Kopie der VHD-Quelldatei handelt. Um ein versehentliches Löschen zu vermeiden, setzt Azure eine Lease für alle VHD-Quelldateien, die zum Erstellen eines Images, Betriebssystem-Datenträgers oder Datenträgers für Daten verwendet wird.

Um eine VHD-Quelldatei löschen zu können, müssen Sie die Lease entfernen, indem Sie den Datenträger oder das Image löschen. Um eine VHD-Datei zu löschen, die von einem virtuellen Computer als Betriebssystem-Datenträger verwendet wird, können Sie den virtuellen Computer, den Betriebssystem-Datenträger und die VHD-Quelldatei auf einmal entfernen, indem Sie den virtuellen Computer und alle verbundenen Datenträger löschen. Zum Entfernen einer VHD-Datei, die als Quelle für einen Datenträger verwendet wird, sind jedoch mehrere Schritte in einer bestimmten Reihenfolge erforderlich. Trennen Sie den Datenträger zunächst vom virtuellen Computer, und löschen Sie den Datenträger und dann die VHD-Datei.

> [!WARNING]
> Wenn Sie eine VHD-Quelldatei aus dem Speicher löschen oder das Speicherkonto löschen, kann Microsoft diese Daten nicht wiederherstellen.

## <a name="types-of-disks"></a>Datenträgertypen 
Azure-Datenträger sind auf eine Verfügbarkeit von 99,999% ausgelegt. Azure-Datenträger stellen durchgängig eine Dauerhaftigkeit auf Unternehmensniveau bereit, mit einer branchenweit führenden auf das Jahr umgerechneten Fehlerrate von NULL%.

Es gibt drei Leistungsstufen, zwischen denen Sie beim Erstellen Ihrer Datenträger wählen können: Premium SSD-Datenträger, Standard SSD (Vorschau) und Standard-HDD-Speicher. Außerdem gibt es zwei Arten von Datenträgern: nicht verwaltet und verwaltet.

### <a name="standard-hdd-disks"></a>Standard-HDD-Datenträger
Standard-HDD-Datenträger basieren auf HDDs (Festplattenlaufwerken) und ermöglichen eine kosteneffiziente Speicherung. Standard-HDD-Speicher kann lokal in einem einzelnen Datencenter repliziert oder georedundant mit primärem und sekundärem Rechenzentrum verwendet werden. Weitere Informationen zur Speicherreplikation finden Sie unter [Azure Storage-Replikation](../articles/storage/common/storage-redundancy.md). 

Weitere Informationen zur Verwendung von Standard-HDD-Datenträgern finden Sie unter [Standard-Speicher und -Datenträger](../articles/virtual-machines/windows/standard-storage.md).

### <a name="standard-ssd-disks-preview"></a>Standard-SSD-Datenträger (Vorschau)
Standard-SSD-Datenträger sind für die gleichen Workloads wie Standard-HDD-Datenträger ausgelegt, aber sie bieten eine konsistentere Leistung und Zuverlässigkeit als HDDs. In Standard-SSD-Datenträgern sind Elemente von Premium-SSD-Datenträgern und Standard-HDD-Datenträgern kombiniert. Hieraus ergibt sich eine kosteneffiziente Lösung, die am besten für Anwendungen wie Webserver geeignet ist, für die in Bezug auf Datenträger kein hoher IOPS-Wert benötigt wird. Falls verfügbar, sind Standard-SSD-Datenträger die empfohlene Bereitstellungsoption für die meisten Workloads. Standard-SSD-Datenträger sind nur als verwaltete Datenträger und während der Vorschauphase nur in [ausgewählten Regionen](../articles/virtual-machines/windows/faq-for-disks.md) und mit dem Resilienztyp „Lokal redundanter Speicher“ (LRS) verfügbar.

### <a name="premium-ssd-disks"></a>Premium-SSD-Datenträger 
Premium-SSD-Datenträger basieren auf SSDs und verfügen über eine hohe Leistung und geringe Latenz für virtuelle Computer mit E/A-intensiven Workloads. In der Regel können Sie Premium-SSD-Datenträger mit Größen verwenden, deren Serienname ein „s“ enthält. Es gibt beispielsweise die Dv3-Serie und die Dsv3-Serie. Letztere kann mit Premium-SSD-Datenträgern verwendet werden.  Weitere Informationen finden Sie unter [Storage Premium](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger
Bei nicht verwalteten Datenträgern handelt es sich um den herkömmlichen Datenträgertyp für virtuelle Computer. Bei diesen Datenträgern erstellen Sie Ihr eigenes Speicherkonto und geben es beim Erstellen des Datenträgers an. Achten Sie darauf, dass Sie nicht zu viele Datenträger unter dem gleichen Speicherkonto erstellen, da Sie ansonsten unter Umständen die [Skalierbarkeitsziele](../articles/storage/common/storage-scalability-targets.md) des Speicherkontos (beispielsweise 20.000 IOPS) überschreiten, was eine Drosselung der virtuellen Computer zur Folge hat. Bei nicht verwalteten Datenträgern müssen Sie herausfinden, wie Sie einzelne oder mehrere Speicherkonten optimal nutzen, um die bestmögliche Leistung für Ihre virtuellen Computer zu erzielen.

### <a name="managed-disks"></a>Verwaltete Datenträger 
Managed Disks nimmt Ihnen die Speicherkontoerstellung und -verwaltung ab und sorgt dafür, dass Sie sich keine Gedanken mehr über die Skalierbarkeitsgrenzwerte des Speicherkontos machen müssen. Sie geben einfach die Datenträgergröße und die Leistungsstufe (Standard/Premium) an und überlassen Azure die Erstellung und Verwaltung der Datenträger. So müssen Sie sich beim Hinzufügen von Datenträgern oder beim Skalieren virtueller Computer keine Gedanken mehr über den verwendeten Speicher machen. 

Darüber hinaus können Sie Ihre benutzerdefinierten Images in einem einzelnen Speicherkonto pro Azure-Region verwalten und mit diesen Hunderte von virtuellen Computern im gleichen Abonnement erstellen. Weitere Informationen zu Managed Disk finden Sie in der [Übersicht über Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md).

Wir empfehlen, Azure Managed Disks für neue virtuelle Computer zu verwenden und bislang nicht verwaltete Datenträger in verwaltete Datenträger umzuwandeln, um von den zahlreichen Features zu profitieren, die in Managed Disks zur Verfügung stehen.

### <a name="disk-comparison"></a>Datenträgervergleich
Die folgende Tabelle enthält eine Gegenüberstellung von Standard-HDD, Standard-SSD und Premium-SSD (sowohl für nicht verwaltete als auch für verwaltete Datenträger), um Ihnen die Entscheidung zu erleichtern.

|    | Azure-Premium-Datenträger |Azure-Standard-SSD-Datenträger (Vorschau)| Azure-Standard-HDD-Datenträger 
|--- | ------------------ | ------------------------------- | ----------------------- 
| Datenträgertyp | Solid-State-Laufwerke (SSD) | Solid-State-Laufwerke (SSD) | Festplattenlaufwerke (HDD)  
| Übersicht  | SSD-basierte Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer mit E/A-intensiven Workloads oder zum Hosten unternehmenskritischer Produktionsumgebungen |Konsistentere und höhere Zuverlässigkeit als HDD. Für Workloads mit niedrigem IOPS-Wert optimiert| HDD-basierter kosteneffizienter Datenträger für sporadischen Zugriff
| Szenario  | Produktionsworkloads und leistungsabhängige Workloads |Webserver, wenig genutzte Unternehmensanwendungen und Dev/Test| Sicherung, nicht kritisch, sporadischer Zugriff 
| Datenträgergröße | P4: 32 GiB (nur verwaltete Datenträger)<br>P6: 64 GiB (nur verwaltete Datenträger)<br>P10: 128 GiB<br>P15: 256 GiB (nur verwaltete Datenträger)<br>P20: 512 GiB<br>P30: 1024 GiB<br>P40: 2048 GiB<br>P50: 4095 GiB |Nur verwaltete Datenträger:<br>E10: 128 GiB<br>E15: 256 GiB<br>E20: 512 GiB<br>E30: 1024 GiB<br>E40: 2048 GiB<br>E50: 4095 GiB | Nicht verwaltete Datenträger: 1 GiB - 4 TiB (4095 GiB) <br><br>Verwaltete Datenträger:<br> S4: 32 GiB <br>S6: 64 GiB <br>S10: 128 GiB <br>S15: 256 GiB <br>S20: 512 GiB <br>S30: 1024 GiB <br>S40: 2048 GiB<br>S50: 4095 GiB
| Max. Durchsatz pro Datenträger | 250 MiB/s | Bis zu 60 MiB/s | Bis zu 60 MiB/s 
| Max. IOPS pro Datenträger | 7500 IOPS | Bis zu 500 IOPS | Bis zu 500 IOPS 