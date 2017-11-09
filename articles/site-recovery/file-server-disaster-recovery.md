---
title: "Schützen eines Dateiservers per Azure Site Recovery"
description: "In diesem Artikel wird beschrieben, wie Sie einen Dateiserver mit Azure Site Recovery schützen."
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: a746ace47c4f1190b7a695014543670a1a9cf879
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="protect-a-file-server-using-azure-site-recovery"></a>Schützen eines Dateiservers per Azure Site Recovery 

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern und virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung verschiedener Workloads.

In diesem Artikel wird beschrieben, wie Sie einen Dateiserver per Azure Site Recovery schützen, und er enthält weitere Empfehlungen für verschiedene Umgebungen. 

- [Protect Azure IaaS file server machines](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines) (Schützen von Azure IaaS-Dateiservercomputern)
- [Protect on-premises file servers](#replicate-an-onpremises-file-server-using-azure-site-recovery) (Schützen von lokalen Dateiservern)


## <a name="file-server-architecture"></a>Dateiserverarchitektur
Das Ziel eines offenen verteilten Dateifreigabesystems ist die Bereitstellung einer Umgebung, in der eine Gruppe von geografisch verteilten Benutzern zusammenarbeiten kann, um Dateien auf effiziente Weise zu bearbeiten, und für die sichergestellt ist, dass die Anforderungen in Bezug auf die Integrität durchgesetzt werden. In einem typischen lokalen Dateiserver-Ökosystem, das eine hohe Zahl von gleichzeitigen Benutzern und Inhaltselementen unterstützt, wird für die Planung der Replikation und die Bandbreitendrosselung die DFS-Replikation (Distributed File System Replication) verwendet. Für DFSR wird ein Komprimierungsalgorithmus genutzt, der als Remote Differential Compression (RDC) bezeichnet wird. Er kann eingesetzt werden, um Dateien über ein Netzwerk mit begrenzter Bandbreite auf effiziente Weise zu aktualisieren. Das Einfügen, Entfernen und Neuanordnen von Daten in Dateien wird erkannt, sodass es mit DFSR möglich ist, beim Aktualisieren von Dateien nur die geänderten Dateiblöcke zu replizieren. Es gibt auch Dateiserverumgebungen ohne Implementierungen von DFSR, in denen tägliche Sicherungen außerhalb der Spitzenzeiten erstellt werden, um auf Notfälle vorbereitet zu sein.

In der folgenden Topologie ist die Dateiserverumgebung mit DFSR-Implementierung dargestellt.
                
![DFSR-Architektur](media/site-recovery-file-server/dfsr-architecture.JPG)

Oben werden mehrere Dateiserver als Member bezeichnet, die an der Replikation von Dateien in einer Replikationsgruppe aktiv beteiligt sind. Der Inhalt des replizierten Ordners ist für alle Clients verfügbar, die Anforderungen an einen der Member senden. Dies gilt auch für den Fall, dass einer der Member in den Offlinezustand wechselt.

## <a name="disaster-recovery-recommendation-for-file-servers"></a>Empfehlung zur Notfallwiederherstellung für Dateiserver:

1.  Replizieren eines Dateiservers mit Azure Site Recovery: Die Dateiserver können per Azure Site Recovery in Azure repliziert werden. Wenn einer oder mehrere lokale Dateiserver nicht zugänglich sind, können die Wiederherstellungs-VMs in Azure aufgerufen werden und dann Anforderungen von Clients lokal bereitstellen, vorausgesetzt, in Azure sind die Site-to-Site-VPN-Konnektivität und Active Directory konfiguriert. Dies ist für eine Umgebung mit DFSR-Konfiguration oder eine einfache Dateiserverumgebung ohne DFSR möglich. 

2.  Erweitern von DFSR auf eine Azure IaaS-VM: In einer gruppierten Dateiserverumgebung mit DFSR-Implementierung besteht ein vorgeschlagener Ansatz darin, die lokale DFSR-Implementierung auf Azure zu erweitern. Ein virtueller Azure-Computer kann dann die Dateiserverrolle übernehmen. 

    Nachdem die Abhängigkeiten der Site-to-Site-VPN-Konnektivität und von Active Directory geregelt wurden und DFSR vorhanden ist, können die Clients auch dann eine Verbindung mit der Azure-VM zur Bereitstellung der Anforderungen herstellen, wenn einer oder mehrere lokale Dateiserver nicht erreichbar sind.

    Dieser Ansatz wird für den Fall empfohlen, dass Ihre VMs über Konfigurationen verfügen, die von Azure Site Recovery nicht unterstützt werden. Beispiel: Freigegebene Clusterdatenträger, die häufiger in Dateiserverumgebungen verwendet werden.  DFSR funktioniert auch in Umgebungen mit geringer Bandbreite und mittlerer Änderungsrate. Die zusätzlichen Kosten der Nutzung einer Azure-VM, die immer ausgeführt wird, sollten hierbei ebenfalls berücksichtigt werden.  

3.  Verwenden Sie den Azure File Sync-Dienst, um Ihre Dateien zu replizieren: Wenn Sie sich auf die Umstellung auf die Cloud vorbereiten oder bereits eine Azure-VM verwenden, raten wir Ihnen zur Verwendung des Azure File Sync-Diensts. Er ermöglicht die Synchronisierung von vollständig verwalteten Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll [Server Message Block](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (SMB) zugegriffen werden kann. Azure-Dateifreigaben können dann gleichzeitig über Cloudbereitstellungen oder lokale Bereitstellungen von Windows, Linux und macOS eingebunden werden. 

Das Diagramm unten enthält eine grafische Darstellung, um Ihnen die Entscheidung zu erleichtern, welche Strategie für Ihre Dateiserverumgebung verwendet werden soll.

![Entscheidungsstruktur](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-while-making-disaster-recovery-decision"></a>Zu berücksichtigende Faktoren beim Treffen der Entscheidung zur Notfallwiederherstellung

|Umgebung  |Empfehlung  |Zu berücksichtigende Punkte |
|---------|---------|---------|
|Dateiserverumgebung mit/ohne DFSR|   [Verwenden von Azure Site Recovery für die Replikation](#replicate-an-onpremises-file-servers-using-azure-site-recovery)   |    Für Site Recovery werden Cluster mit freigegebenen Datenträgern (NAS) nicht unterstützt. Wenn Ihre Umgebung eine dieser Konfigurationen nutzt, können Sie einen der anderen Ansätze verwenden. <br> Azure Site Recovery weist keine Unterstützung für SMB 3.0 auf. Dies bedeutet Folgendes: Nur wenn die an den Dateien vorgenommenen Änderungen am ursprünglichen Speicherort einer Datei aktualisiert werden, werden die Änderungen auf der replizierten VM eingebunden.
|Dateiserverumgebung mit DFSR     |  [Erweitern von DFSR auf einen virtuellen Azure IaaS-Computer:](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |     DFSR ist gut für Umgebungen mit stark begrenzter Bandbreite geeignet. Für diesen Ansatz ist es aber erforderlich, dass ständig eine Azure-VM ausgeführt wird. Die Kosten für die VM müssen in Ihrer Planung berücksichtigt werden.         |
|Azure IaaS-VM     |     [Azure File Sync ](#use-azure-file-sync-service-to-replicate-your-files)   |     Wenn Sie in einem Szenario für die Notfallwiederherstellung Azure File Sync verwenden, müssen Sie während des Failovers manuelle Aktionen durchführen. So wird sichergestellt, dass die Dateifreigaben für den Clientcomputer transparent zugänglich sind. Für AFS muss Port 445 für den Clientcomputer geöffnet sein.     |


### <a name="site-recovery-support"></a>Site Recovery-Unterstützung
Da die Site Recovery-Replikation anwendungsunabhängig ist, gelten die hier aufgezeigten Empfehlungen voraussichtlich auch für die folgenden Szenarien:
| Quelle    |Sekundärer Standort    |Azure
|---------|---------|---------|
|Azure| -|Ja|
|Hyper-V|   Ja |Ja
|VMware |Ja|   Ja
|Physischer Server|   Ja |Ja
 

> [!IMPORTANT]
> Bevor Sie mit einem der drei Ansätze unten fortfahren, sollten Sie sicherstellen, dass die folgenden Abhängigkeiten geregelt sind:

**Site-to-Site-Konnektivität**: Es muss eine direkte Verbindung zwischen dem lokalen Standort und dem Azure-Netzwerk hergestellt werden, um die Kommunikation zwischen Servern zu ermöglichen.  Dies kann mit einer sicheren Site-to-Site-VPN-Verbindung mit dem Microsoft Azure Virtual Network sichergestellt werden, das als Standort für die Notfallwiederherstellung genutzt wird.  
Weitere Informationen: [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Active Directory**: DFSR ist von Active Directory abhängig.  Dies bedeutet, dass die Active Directory-Gesamtstruktur mit lokalen Domänencontrollern auf den Standort für die Notfallwiederherstellung in Azure erweitert wird. Diese Schritte müssen auch dann ausgeführt werden, wenn Sie DFSR nicht nutzen und Benutzern – wie in den meisten Unternehmen – der Zugriff gewährt bzw. eine Überprüfung durchgeführt werden muss.
Weitere Informationen: [Schützen von Active Directory und DNS mit Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory)

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Empfehlung zur Notfallwiederherstellung für virtuelle Azure IaaS-Computer

Wenn Sie die Notfallwiederherstellung von Dateiservern konfigurieren und verwalten, die auf Azure IaaS-VMs gehostet werden, können Sie zwischen zwei Optionen wählen – je nachdem, ob Sie die Umstellung auf [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) durchführen möchten.

1. [Verwenden von Azure File Sync](#use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine)
2. [Verwenden von Azure Site Recovery](#replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery)

## <a name="use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine"></a>Verwenden des Azure File Sync-Diensts zum Replizieren von Dateien, die auf virtuellen IaaS-Computern gehostet werden

**Azure Files** kann herkömmliche lokale Dateiserver oder NAS-Geräte vollständig ersetzen bzw. erweitern. Azure-Dateifreigaben können auch per Azure-Dateisynchronisierung auf Windows Server-Instanzen repliziert werden (entweder lokal oder in der Cloud), um für die Daten am Ort ihrer Verwendung eine leistungsstarke und verteilte Zwischenspeicherung zu erzielen. Die unten angegebenen Schritte veranschaulichen die Empfehlung zur Notfallwiederherstellung für Azure-VMs, die die gleichen Funktionen wie herkömmliche Dateiserver ausführen:
1.  Schützen von Computern mit Azure Site Recovery (mit den hier angegebenen Schritten)
2.  Verwenden Sie Azure File Sync zum Replizieren von Dateien von der VM, die als Dateiserver dient, in der Cloud.
3.  Verwenden Sie das Wiederherstellungsplan-Feature von Azure Site Recovery, um Skripts zum [Einbinden der Azure-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) und Zugreifen auf die Freigabe auf Ihrem virtuellen Computer hinzuzufügen.

Mit den unten angegebenen Schritten wird kurz beschrieben, wie Sie den Azure File Sync-Dienst verwenden:

1. Führen Sie das [Erstellen eines Speicherkontos in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) durch. Wenn Sie georedundanten Speicher mit Lesezugriff (RA-GRS) für Ihre Speicherkonten ausgewählt haben, verfügen Sie im Notfall über Lesezugriff auf Ihre Daten in der sekundären Region. Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Erstellen einer Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share)

3. [Stellen Sie Azure File Sync auf Ihrem Azure-Dateiserver bereit](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide).

4. Erstellen einer Synchronisierungsgruppe: Endpunkte innerhalb einer Synchronisierungsgruppe bleiben miteinander synchron. Eine Synchronisierungsgruppe muss mindestens einen Cloudendpunkt, der eine Azure-Dateifreigabe darstellt, und einen Serverendpunkt, der einen Pfad auf einem Windows-Server darstellt, enthalten.
5.  Ihre Dateien bleiben jetzt zwischen der Azure-Dateifreigabe und Ihrem lokalen Server synchron.
6.  Führen Sie bei einem Notfall in Ihrer lokalen Umgebung ein Failover durch, indem Sie einen Wiederherstellungsplan verwenden, und fügen Sie das Skript zum [Einbinden der Azure-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) und Zugreifen auf die Freigabe auf Ihrem virtuellen Computer hinzu.

### <a name="replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery"></a>Replizieren eines virtuellen IaaS-Dateiservercomputers mit Azure Site Recovery

Führen Sie die ersten beiden Schritte auch aus, wenn bei Ihnen lokale Clients auf den virtuellen IaaS-Dateiservercomputer zugreifen. Fahren Sie ansonsten mit Schritt 3 fort.

1. Erstellen Sie eine Site-to-Site-VPN-Verbindung zwischen dem lokalen Standort und dem Azure-Netzwerk.  
2. Erweitern Sie die lokale Active Directory-Instanz.
3. Führen Sie das [Einrichten der Notfallwiederherstellung](azure-to-azure-tutorial-enable-replication.md) für den IaaS-Dateiservercomputer in eine sekundäre Region durch.


Weitere Informationen zur Notfallwiederherstellung in einer sekundären Region finden Sie [hier](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-using-azure-site-recovery"></a>Replizieren eines lokalen Dateiservers per Azure Site Recovery
Die Schritte unten veranschaulichen die Replikation für eine VMware-VM. Die Schritte zum Replizieren einer Hyper-V-VM finden Sie hier.

1.  Führen Sie das [Vorbereiten von Azure-Ressourcen](tutorial-prepare-azure.md) für die Replikation von lokalen Computern durch.
2.  Erstellen Sie eine Site-to-Site-VPN-Verbindung zwischen dem lokalen Standort und dem Azure-Netzwerk.  
3. Erweitern Sie die lokale Active Directory-Instanz.
4.  [Bereiten Sie lokale VMware-Server vor](tutorial-prepare-on-premises-vmware.md).
5.  Führen Sie das [Einrichten der Notfallwiederherstellung](tutorial-vmware-to-azure.md) in Azure für lokale VMs durch.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Erweitern von DFSR auf einen virtuellen Azure IaaS-Computer:

1.  Erstellen Sie eine Site-to-Site-VPN-Verbindung zwischen dem lokalen Standort und dem Azure-Netzwerk. 
2.  Erweitern Sie die lokale Active Directory-Instanz.
3.  Führen Sie das [Erstellen und Bereitstellen einer Dateiserver-VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) im Microsoft Azure Virtual Network durch.

    Stellen Sie sicher, dass der virtuelle Computer demselben Microsoft Azure Virtual Network hinzugefügt wird, das über gegenseitige Konnektivität mit der lokalen Umgebung verfügt. 

4.  Führen Sie das Installieren und [Konfigurieren der DFS-Replikation](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) unter Windows Server durch.

5.  [Implementieren Sie einen DFS-Namespace](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6.  Wenn der DFS-Namespace implementiert ist, kann das Failover von freigegebenen Ordnern von Produktions- zu Notfallwiederherstellungsstandorten durchgeführt werden, indem die Ordnerziele des DFS-Namespace aktualisiert werden.  Nachdem diese Änderungen des DFS-Namespace per Active Directory repliziert wurden, werden Benutzer auf transparente Weise mit den entsprechenden Ordnerzielen verbunden.

## <a name="use-azure-file-sync-service-to-replicate-your-on-premises-files"></a>Verwenden des Azure File Sync-Diensts zum Replizieren Ihrer lokalen Dateien:
Mit dem Azure File Sync-Dienst können Sie die gewünschten Dateien in der Cloud replizieren. Bei einem Notfall und der Nichterreichbarkeit Ihrer lokalen Dateiserver können Sie die gewünschten Dateispeicherorte aus der Cloud einbinden und mit dem Verarbeiten von Anforderungen der Clientcomputer fortfahren.
Empfohlener Ansatz zur Integration von Azure File Sync in Azure Site Recovery:
1.  Schützen von Dateiservercomputern mit Azure Site Recovery (mit den [hier](tutorial-vmware-to-azure.md) angegebenen Schritten)
2.  Verwenden Sie Azure File Sync, um Dateien von dem Computer, der als Dateiserver dient, in der Cloud zu replizieren.
3.  Nutzen Sie das Wiederherstellungsplan-Feature von Azure Site Recovery zum Hinzufügen von Skripts, um die Azure-Dateifreigabe auf der FileServer-VM in Azure einzubinden, für die das Failover durchgeführt wurde.

Mit den unten angegebenen Schritten wird die Verwendung des Azure File Sync-Diensts beschrieben:

1. Führen Sie das [Erstellen eines Speicherkontos in Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) durch. Wenn Sie georedundanten Speicher mit Lesezugriff (RA-GRS) (empfohlen) für Ihre Speicherkonten ausgewählt haben, verfügen Sie im Notfall über Lesezugriff auf Ihre Daten in der sekundären Region. Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Erstellen einer Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share)

3. Führen Sie das [Bereitstellen von Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) auf Ihrem lokalen Dateiserver durch.

4. Erstellen einer Synchronisierungsgruppe: Endpunkte innerhalb einer Synchronisierungsgruppe bleiben miteinander synchron. Eine Synchronisierungsgruppe muss mindestens einen Cloudendpunkt, der eine Azure-Dateifreigabe darstellt, und einen Serverendpunkt, der einen Pfad auf der lokalen Windows Server-Instanz darstellt, enthalten.

1. Ihre Dateien bleiben jetzt zwischen der Azure-Dateifreigabe und Ihrem lokalen Server synchron.
6.  Führen Sie bei einem Notfall in Ihrer lokalen Umgebung ein Failover durch, indem Sie einen Wiederherstellungsplan verwenden, und fügen Sie das Skript zum Einbinden der Azure-Dateifreigabe und Zugreifen auf die Freigabe auf Ihrem virtuellen Computer hinzu.

> [!NOTE]
> Sicherstellen, dass Port 445 geöffnet ist: Azure Files verwendet das SMB-Protokoll. SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass der TCP-Port 445 des Clientcomputers nicht durch die Firewall blockiert wird.


## <a name="doing-a-test-failover"></a>Durchführen eines Testfailovers

1.  Wechseln Sie zum Azure-Portal, und wählen Sie Ihren Recovery Services-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan, der für die FileServer-Umgebung erstellt wurde.
3.  Klicken Sie auf „Testfailover“.
4.  Wählen Sie den Wiederherstellungspunkt und das virtuelle Azure-Netzwerk aus, um den Prozess für das Testfailover zu starten.
5.  Wenn die sekundäre Umgebung aktiv ist, können Sie Ihre Überprüfungen durchführen.
6.  Klicken Sie nach Abschluss der Überprüfungen auf „Cleanup test failover“ (Testfailover bereinigen). Die Testfailover-Umgebung ist nun bereinigt.

Weitere Informationen zum Durchführen des Testfailovers finden Sie [hier](site-recovery-test-failover-to-azure.md).

Eine Anleitung zum Ausführen des Testfailovers für AD und DNS finden Sie unter [Test failover considerations for AD and DNS](site-recovery-active-directory.md) (Überlegungen zum Testfailover für AD und DNS).

## <a name="doing-a-failover"></a>Durchführen eines Failovers

1.  Wechseln Sie zum Azure-Portal, und wählen Sie Ihren Recovery Services-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan, der für die FileServer-Umgebung erstellt wurde.
3.  Klicken Sie auf „Failover“.
4.  Wählen Sie einen Wiederherstellungspunkt aus, um den Failoverprozess zu starten.

Weitere Informationen zum Durchführen des Testfailovers finden Sie [hier](site-recovery-failover.md).
