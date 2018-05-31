---
title: Sichern von Azure-Stack-Dateien und -Anwendungen | Microsoft-Dokumentation
description: Verwenden Sie Azure Backup, um Azure Stack-Dateien und -Anwendungen in Ihrer Azure Stack-Umgebung zu sichern und wiederherzustellen.
services: backup
documentationcenter: ''
author: adiganmsft
manager: shivamg
editor: ''
keyword: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/11/2018
ms.author: adigan,markgal
ms.openlocfilehash: 19067b40e8e87c160515d13bb490e7c1604788b6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34196711"
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Sichern von Dateien und Anwendungen in Azure Stack
Sie können Azure Backup verwenden, um Dateien und Anwendungen in Azure Stack zu schützen (oder zu sichern). Installieren Sie Microsoft Azure Backup Server als virtuellen Computer, der unter Azure Stack ausgeführt wird, um Dateien und Anwendungen zu sichern. Sie können alle Anwendungen schützen, die auf einem beliebigen Azure Stack-Server in demselben Netzwerk ausgeführt werden. Nachdem Sie Azure Backup Server installiert haben, fügen Sie Azure-Datenträger hinzu, um den lokalen Speicher zu vergrößern, der für die kurzfristige Sicherung von Daten verfügbar ist. Azure Backup Server nutzt Azure-Speicher für die langfristige Aufbewahrung.

> [!NOTE]
> Azure Backup Server und System Center Data Protection Manager (DPM) sind zwar ähnlich, aber DPM wird für die Verwendung mit Azure Stack nicht unterstützt.
>


## <a name="azure-backup-server-protection-matrix"></a>Azure Backup Server-Schutzmatrix
Azure Backup Server schützt die folgenden Workloads für virtuelle Azure Stack-Computer.

| Geschützte Datenquelle | Schutz und Wiederherstellung |
| --------------------- | ----------------------- |
| Windows Server – Halbjährlicher Kanal – Datacenter/Enterprise/Standard | Volumes, Dateien, Ordner |
| Windows Server 2016 – Datacenter/Enterprise/Standard | Volumes, Dateien, Ordner |
| Windows Server 2012 R2 – Datacenter/Enterprise/Standard | Volumes, Dateien, Ordner |
| Windows Server 2012 – Datacenter/Enterprise/Standard | Volumes, Dateien, Ordner |
| Windows Server 2008 R2 – Datacenter/Enterprise/Standard | Volumes, Dateien, Ordner |
| SQL Server 2016 | Datenbank |
| SQL Server 2014 | Datenbank |
| SQL Server 2012 SP1 | Datenbank |
| SharePoint 2013 | Farm, Datenbank, Front-End, Webserver |
| SharePoint 2010 | Farm, Datenbank, Front-End, Webserver |


## <a name="install-azure-backup-server"></a>Installieren von Azure Backup Server
Informationen zur Installation von Azure Backup Server auf einem virtuellen Azure Stack-Computer finden Sie im Artikel [Vorbereiten der Sicherung von Workloads per Azure Backup Server](backup-azure-microsoft-azure-backup.md). Beachten Sie Folgendes, bevor Sie Azure Backup Server installieren und konfigurieren:

### <a name="determining-size-of-virtual-machine"></a>Bestimmen der Größe des virtuellen Computers
Verwenden Sie die Größe A2 oder höher, um Azure Backup Server auf einem virtuellen Azure Stack-Computer auszuführen. Laden Sie als Hilfe bei der Auswahl einer VM-Größe den [Rechner für die Azure Stack-VM-Größe](https://www.microsoft.com/download/details.aspx?id=56832) herunter.

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Virtuelle Netzwerke auf virtuellen Azure Stack-Computern
Alle virtuellen Computer, die in einer Azure Stack-Workload verwendet werden, müssen demselben virtuellen Azure-Netzwerk und Azure-Abonnement angehören.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Speichern von Sicherungsdaten auf einem lokalen Datenträger in Azure
Azure Backup Server speichert Sicherungsdaten auf Azure-Datenträgern, die an den virtuellen Computer angefügt sind, um die Wiederherstellung des Betriebs zu ermöglichen. Nachdem die Datenträger und der Speicherplatz an den virtuellen Computer angefügt wurden, wird der Speicher für Sie von Azure Backup Server verwaltet. Die Menge an Sicherungsdatenspeicher richtet sich nach der Anzahl und Größe von Datenträgern, die an die einzelnen [virtuellen Azure Stack-Computer](../azure-stack/user/azure-stack-storage-overview.md) angefügt sind. Jede Azure Stack-VM-Größe verfügt über eine maximale Anzahl von Datenträgern, die an den virtuellen Computer angefügt werden kann. Beispielsweise umfasst A2 vier Datenträger. A3 umfasst acht Datenträger. A4 umfasst 16 Datenträger. Auch hier bestimmt die Größe und Anzahl von Datenträgern die Gesamtgröße des Sicherungsspeicherpools.

> [!IMPORTANT]
> Sie sollten Sicherungsdaten für die Wiederherstellung des Betriebs **nicht** länger als fünf Tage auf Datenträgern aufbewahren, die an Azure Backup Server angefügt sind.
>

Durch das Speichern von Sicherungsdaten in Azure wird die Sicherungsinfrastruktur in Azure Stack reduziert. Wenn Daten mehr als fünf Tage alt sind, sollten sie in Azure gespeichert werden.

Erstellen bzw. verwenden Sie einen Recovery Services-Tresor, um Sicherungsdaten in Azure zu speichern. Beim Vorbereiten einer Sicherung der Azure Backup Server-Workload [konfigurieren Sie den Recovery Services-Tresor](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Nach der Konfiguration wird bei jeder Ausführung eines Sicherungsauftrags im Tresor ein Wiederherstellungspunkt erstellt. Jeder Recovery Services-Tresor enthält bis zu 9999 Wiederherstellungspunkte. Je nach Anzahl von erstellten Wiederherstellungspunkten und Dauer ihrer Aufbewahrung können Sie Sicherungsdaten viele Jahre lang vorhalten. Beispielsweise können Sie monatliche Wiederherstellungspunkte erstellen und fünf Jahre lang aufbewahren.
 
### <a name="using-sql-server"></a>Verwenden von SQL Server
Wenn Sie eine SQL Server-Remoteinstanz für die Azure Backup Server-Datenbank verwenden, sollten Sie nur eine Azure Stack-VM mit Ausführung von SQL Server auswählen.

### <a name="azure-backup-server-vm-performance"></a>Azure Backup Server-VM-Leistung
Bei einer gemeinsamen Nutzung mit anderen virtuellen Computern können sich die Speicherkontogröße und die IOPS-Grenzwerte auf die Leistung des virtuellen Azure Backup Server-Computers auswirken. Aus diesem Grund sollten Sie ein separates Speicherkonto für den virtuellen Azure Backup Server-Computer verwenden. Der Azure Backup-Agent, der auf dem Azure Backup Server ausgeführt wird, benötigt temporären Speicher für folgende Zwecke:
    - Eigene Nutzung (Cachespeicherort)
    - Aus der Cloud wiederhergestellte Daten (lokaler Stagingbereich)
  
### <a name="configuring-azure-backup-temporary-disk-storage"></a>Konfigurieren des temporären Azure Backup-Speichers
Jeder virtuelle Azure Stack-Computer verfügt über temporären Datenträgerspeicher, der für den Benutzer als Volume `D:\` verfügbar ist. Der von Azure Backup benötigte lokale Stagingbereich kann so konfiguriert werden, dass er sich auf `D:\` befindet, und der Cachespeicherort kann auf `C:\` angeordnet werden. Auf diese Weise muss kein Speicher der Datenträger für Daten in Anspruch genommen werden, die an den virtuellen Azure Backup Server-Computer angefügt sind.

### <a name="scaling-deployment"></a>Skalieren der Bereitstellung
Wenn Sie Ihre Bereitstellung skalieren möchten, haben Sie die folgenden Optionen:
  - Zentral hochskalieren: Erhöhen Sie die Größe des virtuellen Azure Backup Server-Computers von der A-Serie auf die D-Serie, und erhöhen Sie den lokalen Speicher [gemäß der Anleitung zum virtuellen Azure Stack-Computer](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Daten auslagern: Senden Sie ältere Daten an Azure Backup Server, und behalten Sie nur die neuesten Daten in dem Speicher bei, der an den Azure Backup Server angefügt ist.
  - Horizontal hochskalieren: Fügen Sie weitere Azure Backup Server-Instanzen hinzu, um die Workloads zu schützen.

## <a name="see-also"></a>Weitere Informationen
Informationen zur Verwendung von Azure Backup Server zum Schützen anderer Workloads finden Sie in einem der folgenden Artikel:
- [Sichern einer SharePoint-Farm](backup-azure-backup-sharepoint-mabs.md)
- [Sichern einer SQL Server-Instanz](backup-azure-sql-mabs.md)
