---
title: SAP-Workloadkonfigurationen mit Azure-Verfügbarkeitszonen | Microsoft-Dokumentation
description: Architektur und Szenarien für die Hochverfügbarkeit für SAP NetWeaver unter Verwendung von Azure-Verfügbarkeitszonen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/03/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 409a304296d3fdff897a203177e2c150162755c6
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746216"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>SAP-Workloadkonfigurationen mit Azure-Verfügbarkeitszonen

Eine der Hochverfügbarkeitsfunktionen, die Azure zur Verbesserung der allgemeinen Verfügbarkeit von SAP-Workloads in Azure bietet, sind [Azure-Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview). Verfügbarkeitszonen stehen bereits in verschiedenen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/) bereit. Weitere Regionen werden folgen. 

Die grundlegende Architektur der SAP-Hochverfügbarkeit wird in dieser Abbildung dargestellt:

![Standard-Hochverfügbarkeitskonfiguration](./media/sap-ha-availability-zones/standard-ha-config.png)

Die SAP-Anwendungsschicht wird in einer [Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) von Azure bereitgestellt. Für die Hochverfügbarkeit von SAP Central Services stellen Sie zwei VMs in einer separaten Verfügbarkeitsgruppe bereit und verwenden Windows-Failoverclusterdienste oder Pacemaker (Linux) zum Bereitstellen eines Hochverfügbarkeitsframeworks, das bei einem Infrastruktur- oder Softwareproblem ein automatisches Failover ermöglicht. In folgenden Dokumentationen werden solche Bereitstellungen ausführlich behandelt:

- [Gruppieren einer SAP ASCS/SCS-Instanz in einem Windows-Failovercluster mithilfe freigegebener Clusterdatenträger in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Gruppieren einer SAP ASCS/SCS-Instanz in einem Windows-Failovercluster per Dateifreigabe in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs unter SUSE Linux Enterprise Server für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver unter Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Eine ähnliche Architektur wird für die DBMS-Schicht von SAP NetWeaver, S/4HANA oder Hybris-Systemen angewandt. Sie stellen die DBMS-Schicht in einem Aktiv/Passiv-Modus mit einer Failoverclusterlösung bereit, die entweder DBMS-spezifische Failoverframeworks, Windows-Failoverclusterdienste oder Pacemaker verwendet, um die Failoveraktivitäten im Falle von Infrastruktur- oder Softwarefehlern zu initiieren. 

Zum Bereitstellen derselben Architektur mit Azure-Verfügbarkeitszonen müssen einige Änderungen der beschriebenen Architektur vorgenommen werden. Diese Änderungen werden in den verschiedenen Teilen dieses Dokuments beschrieben.

## <a name="considerations-deploying-across-availability-zones"></a>Überlegungen zur Verfügbarkeitszonen übergreifenden Bereitstellung

Für Verfügbarkeitszonen müssen einige Punkte beachtet werden. Darunter befinden sich Überlegungen wie die folgenden:

- Azure-Verfügbarkeitszonen bieten keine Garantie für eine bestimmte Entfernung zwischen den verschiedenen Zonen innerhalb einer Azure-Region.
- Azure-Verfügbarkeitszonen sind keine ideale Lösung zur Notfallwiederherstellung. In Zeiten, in denen große Naturkatastrophen in einigen Regionen der Welt umfangreiche Schäden verursachen, einschließlich starker Schäden an der Energieversorgungsinfrastruktur, könnte der Abstand zwischen verschiedenen Zonen für eine hinreichende Lösung zur Notfallwiederherstellung nicht groß genug sein.
- Die Netzwerklatenz zwischen verschiedenen Azure-Verfügbarkeitszonen in den verschiedenen Azure-Regionen ist von Azure-Region zu Azure-Region unterschiedlich. In einigen Fällen können Sie die in verschiedenen Zonen bereitgestellte SAP-Anwendungsschicht ausführen, da die Netzwerklatenz von einer Zone zur aktiven DBMS-VM bezüglich der Beeinträchtigung von Geschäftsprozessen noch akzeptabel ist. Es gibt jedoch in manchen Azure-Regionen Szenarien, in denen die Latenz zwischen der aktiven DBMS-VM in einer Zone und einer SAP-Anwendungsinstanz in einer anderen Zone zu starke Beeinträchtigungen verursacht und für die SAP-Geschäftsprozesse nicht akzeptabel ist. Daher müssen sich die Bereitstellungsarchitekturen für eine Aktiv/Aktiv-Architektur für die Anwendung und eine Aktiv/Passiv-Architektur unterscheiden, wo die zonenübergreifende Latenz zu hoch ist.
- Entscheiden Sie, in welcher Konfiguration Sie Azure-Verfügbarkeitszonen verwenden könnten. Basierend auf der Netzwerklatenz, die Sie zwischen den verschiedenen Zonen messen. Die Netzwerklatenz spielt in zwei verschiedenen Bereichen eine wichtige Rolle:
    - Bei der Latenz zwischen den beiden DBMS-Instanzen, für die eine synchrone Replikation eingerichtet werden muss. Je höher die Netzwerklatenz, desto höher die Möglichkeit der Auswirkung auf die Skalierbarkeit Ihrer Workload.
    - Bei dem Unterschied in der Netzwerklatenz zwischen einem virtuellen Computer, der eine SAP-Dialoginstanz in derselben Zone wie die aktive DBMS-Instanz ausführt, und einer ähnlichen VM in einer anderen Zone. Je höher dieser Unterschied ist, desto höher die Auswirkungen auf die Laufzeit von Geschäftsprozessen und Batchaufträgen, abhängig davon, ob sie in der gleichen Zone mit dem DBMS oder in einer anderen Zone ausgeführt werden.


Für die Nutzung von Azure-Features gelten einige Funktionalitätseinschränkungen, die bei der zonenübergreifenden Bereitstellung von Azure-VMs und der unterschiedliche Verfügbarkeitszonen innerhalb derselben Azure-Region übergreifenden Einrichtung von Failoverlösungen angewandt werden können. Diese Einschränkungen sind:

- Die Verwendung von [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) ist für die Bereitstellung in Azure-Verfügbarkeitszonen zwingend erforderlich. 
- Die Zuordnung der Zonenaufzählungen zu den physischen Zonen liegt auf einer Azure-Abonnementbasis fest. Wenn Sie verschiedene Abonnements verwenden, um Ihre SAP-Systeme bereitzustellen, müssen Sie die idealen Zonen für jedes Abonnement einzeln definieren.
- Sie können Azure-Verfügbarkeitsgruppen nicht in einer Azure-Verfügbarkeitszone bereitstellen. Wählen Sie entweder eine Azure-Verfügbarkeitszone oder eine Azure-Verfügbarkeitsgruppe als Bereitstellungsframework für virtuelle Computer aus.
- Sie können den [Azure Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) nicht zum Erstellen von Failoverclusterlösungen auf der Grundlage von Windows-Failoverclusterdiensten oder Linux Pacemaker verwenden. Stattdessen müssen Sie die [Standard-SKU von Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) verwenden.



## <a name="ideal-zone-combination"></a>Kombination der idealen Zonen
Um zu entscheiden, wie Sie Verfügbarkeitszonen nutzen können, müssen Sie eine Untersuchung durchzuführen, die Ihnen folgende Ergebnisse liefert:

- Die Netzwerklatenz zwischen den drei verschiedenen Zonen einer Azure-Region. Auf dieser Basis müssten Sie die Zonen auswählen können, bei denen im zonenübergreifenden Netzwerkdatenverkehr die geringste Netzwerklatenz auftritt.
- Den Unterschied zwischen der VM-zu-VM-Latenz innerhalb einer der von Ihnen ausgewählten Zonen und der Netzwerklatenz zwischen den zwei Zonen, die Sie ausgewählt haben.
- Eine Aussage darüber, ob die VM-Typen, die die ausgewählten Verfügbarkeitszonen übergreifend bereitgestellt werden müssen, in den beiden Zonen Ihrer Wahl verfügbar sind. Vor allem bei VMs der M-Serie werden Sie Situationen erleben, in denen die verschiedenen VM-SKUs der M-Serie nur in zwei der drei Zonen zur Verfügung stehen.

### <a name="network-latency-between-zones-and-within-zone"></a>Netzwerklatenz zwischen Zonen und innerhalb der Zone
Ermitteln Sie wie folgt die Latenz zwischen den verschiedenen Zonen:

- Stellen Sie die VM-SKU bereit, die Sie für Ihre DBMS-Instanz in allen drei Zonen verwenden möchten. Stellen Sie sicher, dass [Beschleunigter Azure-Netzwerkbetrieb](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) bei der Durchführung dieser Messung aktiviert ist.
- Wenn Sie die beiden Zonen mit der geringsten Netzwerklatenz ermittelt haben, stellen Sie weitere drei virtuelle Computer der VM-SKU bereit, die Sie als Anwendungsschicht-VM die drei Verfügbarkeitszonen übergreifend verwenden möchten. Messen Sie die Netzwerklatenz für die zwei „DBMS-VMs“ in den beiden anderen „DBMS“-Zonen Ihrer Wahl. 
- Verwenden Sie zum Messen das Tool **niping**. Dies ist ein Tool von SAP, das wie in den SAP-Supporthinweisen [500235](https://launchpad.support.sap.com/#/notes/500235) und [1100926](https://launchpad.support.sap.com/#/notes/1100926/E) beschrieben funktioniert. Konzentrieren Sie sich auf die Befehle, die SAP für Latenzmessungen dokumentiert hat. Das Tool **ping** wird hier nicht empfohlen, weil **ping** nicht auf den Codepfaden des beschleunigten Azure-Netzwerkbetriebs funktioniert.

Basierend auf den Messungen und der Verfügbarkeit Ihrer VM-SKUs in den verschiedenen Zonen müssen Sie die Entscheidungen treffen:

- Definieren der idealen Zonen für die DBMS-Ebene
- Beantworten Sie die Frage, ob Sie basierend auf den Unterschieden der Netzwerklatenz in einer Zone oder zonenübergreifend Ihre aktive SAP-Anwendungsschicht auf eine, zwei oder alle drei verschiedenen Zonen verteilen können.
- Beantworten Sie die Frage, ob Sie aus der Sicht einer Anwendung eine Aktiv/Passiv- oder Aktiv/Aktiv-Konfiguration bereitstellen möchten (siehe weiter unten).

Berücksichtigen Sie bei diesen Entscheidungen auch die SAP-Empfehlungen zur Netzwerklatenz im SAP-Hinweis [1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Unbedingt beachten

> [!IMPORTANT]
> Ihre Messungen und Entscheidungen gelten für das Azure-Abonnement, mit dem Sie diese Messungen vorgenommen haben. Wenn Sie ein anderes Azure-Abonnement verwenden, müssen Sie die Messungen wiederholen, da die Zuordnung Ihrer vom Abonnement abhängigen Zonenenumeration sich mit einem anderen Abonnement ändern kann.


> [!IMPORTANT]
> Es ist anzunehmen, dass die Ergebnisse der oben durchgeführten Messungen in jeder Azure-Region, die [Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) unterstützt, unterschiedlich sind. Auch wenn Ihre Anforderungen an die Netzwerklatenz sich nicht ändern, müssen Sie vielleicht verschiedene Bereitstellungsstrategien in verschiedenen Azure-Regionen anpassen, da die Netzwerklatenz zwischen Zonen unterschiedlich sein kann. Es ist anzunehmen, dass in einigen Azure-Regionen die Netzwerklatenz zwischen den drei verschiedenen Zonen völlig unterschiedlich sein kann. In anderen Regionen ist die Netzwerklatenz zwischen den drei verschiedenen Zonen hingegen einheitlicher. Die Annahme, dass zwischen Zonen **immer** eine Netzwerklatenz von 1 bis 2 Millisekunden besteht, ist **falsch**. Die Verfügbarkeitszonen übergreifende Netzwerklatenz in Azure-Regionen kann nicht verallgemeinert werden.


## <a name="activeactive-deployment"></a>Aktiv/Aktiv-Bereitstellung
Diese Bereitstellungsarchitektur wird als Aktiv/Aktiv bezeichnet, da Sie Ihre aktiven SAP-Dialoginstanzen zwei oder drei Zonen übergreifend bereitstellen. Die SAP Central Services, die die Replikation mit Einreihen in die Warteschlange verwenden, werden zwei Zonen übergreifend bereitgestellt. Dies gilt auch für die DBMS-Ebene, die die gleichen Zonen übergreifend wie die SAP Central Services bereitgestellt wird.

Um über eine Konfiguration dieser Art nachzudenken, müssen Sie die zwei Verfügbarkeitszonen in Ihrer Region ermitteln, die zonenübergreifende Netzwerklatenz bieten, die für Ihre Workload und Ihre synchrone DBMS-Replikation akzeptabel ist. Darüber hinaus soll das Delta zwischen der Netzwerklatenz in den Zonen, die Sie ausgewählt haben, und der zonenübergreifenden Netzwerklatenz nicht zu groß sein. Der Grund für Letztgenanntes ist, dass die Laufzeitvariationen Ihrer Geschäftsprozesse oder Batchprozesse in Abhängigkeit davon, ob ein Auftrag in der Zone mit dem DBMS-Server oder zonenübergreifend ausgeführt wird, nicht zu groß sein sollen. Einige Variationen sind zulässig, aber nicht Unterschiedsfaktoren.

Ein vereinfachtes Schema einer zwei Zonen übergreifenden Aktiv/Aktiv-Bereitstellung könnte wie folgt aussehen:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Die folgenden Überlegungen gelten für diese Konfiguration:

- Sie behandeln die Azure-Verfügbarkeitszonen als Fehler- und Updatedomänen für alle virtuellen Computer, da Verfügbarkeitsgruppen nicht in Azure-Verfügbarkeitszonen bereitgestellt werden können.
- Die Azure-Load Balancer, die Sie für die Failovercluster der SAP Central Services sowie die DBMS-Ebene verwenden, müssen [Standard SKU Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) sein. Der Basic Load Balancer funktioniert nicht zonenübergreifend.
- Das virtuelle Azure-Netzwerk und dessen Subnetze, die Sie zum Hosten des SAP-Systems bereitgestellt haben, werden über Zonen gestreckt. Sie **benötigen keine** separaten virtuellen Netzwerke für jede Zone.
- Verwenden Sie für alle VMs, die Sie bereitstellen, [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Nicht verwaltete Datenträger werden für zonale Bereitstellungen nicht unterstützt.
- Azure Storage Premium oder [SSD Ultra-Speicher](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) unterstützen keinen Speicherreplikationstyp zonenübergreifend. Daher ist die Anwendung (DBMS-oder SAP Central Services) für das Replizieren wichtiger Daten verantwortlich.
- Dasselbe gilt auch für das freigegebene sapmnt-Verzeichnis, das ein freigegebener Datenträger (Windows), eine CIFS-Freigabe (Windows) oder NFS-Freigabe (Linux) ist. Sie müssen eine Technologie verwenden, die einen solchen freigegebenen Datenträger oder eine Dateifreigabe zwischen den Zonen repliziert. Im Moment werden die folgenden Technologien unterstützt:
    - Für Windows wird eine Clusterlösung, die SIOS Datakeeper wie in [Gruppieren eine SAP ASCS/SCS-Instanz auf einem Windows-Failovercluster mithilfe freigegebener Clusterdatenträger in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) dokumentiert verwendet, zonenübergreifend unterstützt.
    - Für SUSE Linux wird eine NFS-Freigabe, die wie in [Hochverfügbarkeit für NFS auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) dokumentiert erstellt wird, unterstützt.
    - Zu diesem Zeitpunkt wird die Lösung, die Windows-SOFS (Scale-Out File Services) wie in [Vorbereiten der Azure-Infrastruktur für SAP-Hochverfügbarkeit mit einem Windows-Failovercluster und einer Dateifreigabe für SAP ASCS-/SCS-Instanzen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) dokumentiert verwendet, **nicht zonenübergreifend unterstützt**.
- Die dritte Zone wird verwendet, um das SBD-Gerät für den Fall zu hosten, dass Sie einen [SUSE Linux Pacemaker-Cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) oder zusätzliche Anwendungsinstanzen erstellen.
- Um Laufzeitkonsistenz für einige wichtige geschäftliche Transaktionen und/oder Aufträge zu erzielen. Sie können versuchen, bestimmte Batchaufträge und Benutzer mit der aktiven DBMS-Instanz mithilfe von SAP Batch Server-Gruppen, Anmeldegruppen oder RFC-Gruppen direkt zu bestimmten Anwendungsinstanzen in der Zone zu leiten. Allerdings müssen Sie diese Gruppen bei einem Failover einer Zone manuell zu Dialoginstanzen verschieben, die sich in den verbleibenden Zonen befinden. 
- Entscheiden Sie, ob Sie einige ruhende Dialoginstanzen in jeder Zone bereitstellen möchten, um in der Lage zu sein, sofort die frühere Ressourcenkapazität zu erzielen, falls eine Zone, in der Sie einen Teil Ihrer Anwendungsinstanzen bereitgestellt haben, außer Betrieb ist.


## <a name="activepassive-deployment"></a>Aktiv/Passiv-Bereitstellung
Falls Sie kein akzeptables Delta zwischen der Netzwerklatenz in einer Zone und der zonenübergreifenden Netzwerklatenz feststellen, hat die Architektur, die Sie bereitstellen können, aus Sicht der SAP-Anwendungsschicht einen Aktiv/Passiv-Charakter. Sie definieren eine „aktive“ Zone – die Zone, in der Sie die vollständige Anwendungsschicht bereitstellen, und wo Sie versuchen, die aktive DBMS-Instanz und die aktive SAP Central Services-Instanz auszuführen. Mit einer solchen Konfiguration stellen Sie sicher, dass kein extremer Laufzeitunterschied zwischen Geschäftstransaktionen und Batchaufträgen auftritt, abhängig davon, ob ein Auftrag in der gleichen Zone mit der aktiven DBMS-Instanz ausgeführt wird oder nicht.

Das grundlegende Layout einer solchen Architektur sieht folgendermaßen aus:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Die folgenden Überlegungen gelten für diese Konfiguration:

- Verfügbarkeitsgruppen können nicht in Azure-Verfügbarkeitszonen bereitgestellt werden. Daher bleibt es in diesem Fall bei einer einzigen Update- und Fehlerdomäne für Ihre Anwendungsschicht. Grund dafür ist, dass sie nur in einer einzigen Zone bereitgestellt wird. Diese Konfiguration ist ein leichter Rückschlag im Vergleich zu der Referenzarchitektur, die voraussieht, dass Sie die Anwendungsschicht in einer Azure-Verfügbarkeitsgruppe bereitstellen.
- Der Einsatz einer solchen Architektur erfordert eine genaue Überwachung, und Sie müssen versuchen, die aktiven DBMS- und SAP Central Services-Instanzen in derselben Zone zu halten wie Ihre bereitgestellte Anwendungsschicht. Im Falle eines Failovers der SAP Central Services- oder DBMS-Instanz müssen Sie sicherstellen, dass Sie so früh wie möglich ein manuelles Failback in die Zone mit der SAP-Anwendungsschicht durchführen können.
- Die Azure-Load Balancer, die Sie für die Failovercluster der SAP Central Services sowie die DBMS-Ebene verwenden, müssen [Standard SKU Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) sein. Der Basic Load Balancer funktioniert nicht zonenübergreifend.
- Das virtuelle Azure-Netzwerk und dessen Subnetze, die Sie zum Hosten des SAP-Systems bereitgestellt haben, werden über Zonen gestreckt. Sie **benötigen keine** separaten virtuellen Netzwerke für jede Zone.
- Für alle VMs, die Sie bereitstellen, müssen Sie [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) verwenden. Nicht verwaltete Datenträger werden für zonale Bereitstellungen nicht unterstützt.
- Azure Storage Premium oder [SSD Ultra-Speicher](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) unterstützen keinen Speicherreplikationstyp zonenübergreifend. Daher ist die Anwendung (DBMS-oder SAP Central Services) für das Replizieren wichtiger Daten verantwortlich.
- Dasselbe gilt auch für das freigegebene sapmnt-Verzeichnis, das ein freigegebener Datenträger (Windows), eine CIFS-Freigabe (Windows) oder NFS-Freigabe (Linux) ist. Sie müssen eine Technologie verwenden, die einen solchen freigegebenen Datenträger oder eine Dateifreigabe zwischen den Zonen repliziert. Im Moment werden die folgenden Technologien unterstützt:
    - Für Windows wird eine Clusterlösung, die SIOS Datakeeper wie in [Gruppieren eine SAP ASCS/SCS-Instanz auf einem Windows-Failovercluster mithilfe freigegebener Clusterdatenträger in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) dokumentiert verwendet, zonenübergreifend unterstützt.
    - Für SUSE Linux wird eine NFS-Freigabe, die wie in [Hochverfügbarkeit für NFS auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) dokumentiert erstellt wird, unterstützt.
    - Zu diesem Zeitpunkt wird die Lösung, die Windows-SOFS (Scale-Out File Services) wie in [Vorbereiten der Azure-Infrastruktur für SAP-Hochverfügbarkeit mit einem Windows-Failovercluster und einer Dateifreigabe für SAP ASCS-/SCS-Instanzen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) dokumentiert verwendet, **nicht zonenübergreifend unterstützt**.
- Die dritte Zone wird verwendet, um das SBD-Gerät für den Fall zu hosten, dass Sie einen [SUSE Linux Pacemaker-Cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) oder zusätzliche Anwendungsinstanzen erstellen.
- Stellen Sie ruhende virtuelle Computer in der (aus DBMS-Perspektive) passiven Zone bereit, um die Anwendungsressourcen nach einem Zonenausfall starten zu können.
    - Sie können [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) nicht verwenden, um aktive virtuelle Computer nach ruhenden virtuellen Computern zwischen Zonen zu replizieren. Zu diesem Zeitpunkt kann Azure Site Recovery eine solche Funktion nicht erfüllen.
- Investieren Sie in Automatisierung, die Ihnen beim Ausfall einer Zone den automatischen Start der SAP-Anwendungsschicht in der zweiten Zone ermöglicht.

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Kombinierte Konfiguration von Hochverfügbarkeit und Notfallwiederherstellung
Obwohl Microsoft keine Informationen zur geografischen Entfernung zwischen den Einrichtungen liefert, die verschiedene Azure-Verfügbarkeitszonen in einer bestimmten Azure-Region hosten, nutzen einige Kunden Zonen für eine kombinierte HA- und DR-Konfiguration, die eine **R**ecovery **P**oint **O**bjective (RPO) von 0 (null) verspricht. Dies bedeutet, dass Sie auch bei der Notfallwiederherstellung keine committeten Datenbanktransaktionen verlieren sollten. 

> [!NOTE]
> Eine solche Konfiguration wird nur für bestimmte Umstände empfohlen. Dies ist z.B. der Fall, wenn Daten die Azure-Region aus Sicherheits- und Compliancegründen nicht verlassen dürfen. 

Ein Beispiel einer solchen Konfiguration veranschaulicht diese Grafik:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

Die folgenden Überlegungen gelten für diese Konfiguration:

- Entweder nehmen Sie an, dass eine signifikante Distanz zwischen den Einrichtungen liegt, die eine Verfügbarkeitszone hosten. Oder Sie sind gezwungen, in einer bestimmten Azure-Region zu bleiben. Verfügbarkeitsgruppen können nicht in Azure-Verfügbarkeitszonen bereitgestellt werden. Daher bleibt es in diesem Fall bei einer einzigen Update- und Fehlerdomäne für Ihre Anwendungsschicht. Grund dafür ist, dass sie nur in einer einzigen Zone bereitgestellt wird. Dies ist ein leichter Rückschlag im Vergleich zu der Referenzarchitektur, die voraussieht, dass Sie die Anwendungsschicht in einer Azure-Verfügbarkeitsgruppe bereitstellen.
- Der Einsatz einer solchen Architektur erfordert eine genaue Überwachung, und Sie müssen versuchen, die aktiven DBMS- und SAP Central Services-Instanzen in derselben Zone zu halten wie Ihre bereitgestellte Anwendungsschicht. Im Falle eines Failovers der SAP Central Services- oder DBMS-Instanz müssen Sie sicherstellen, dass Sie so früh wie möglich ein manuelles Failback in die Zone mit der SAP-Anwendungsschicht durchführen können.
- Sie haben die Produktionsanwendungsinstanzen auf den virtuellen Computern vorinstalliert, auf denen die aktiven QA-Anwendungsinstanzen ausgeführt werden.
- Bei einem Ausfall einer Zone müssen Sie die QA-Anwendungsinstanzen herunterfahren und stattdessen die Produktionsinstanzen starten. Denken Sie daran, dass Sie mit virtuellen Namen für die Anwendungsinstanzen arbeiten müssen, damit dies funktioniert.
- Die Azure-Load Balancer, die Sie für die Failovercluster der SAP Central Services sowie die DBMS-Ebene verwenden, müssen [Standard SKU Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) sein. Der Basic Load Balancer funktioniert nicht zonenübergreifend.
- Das virtuelle Azure-Netzwerk und dessen Subnetze, die Sie zum Hosten des SAP-Systems bereitgestellt haben, werden über Zonen gestreckt. Sie **benötigen keine** separaten virtuellen Netzwerke für jede Zone.
- Für alle VMs, die Sie bereitstellen, müssen Sie [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) verwenden. Nicht verwaltete Datenträger werden für zonale Bereitstellungen nicht unterstützt.
- Azure Storage Premium oder [SSD Ultra-Speicher](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) unterstützen keinen Speicherreplikationstyp zonenübergreifend. Daher ist die Anwendung (DBMS-oder SAP Central Services) für das Replizieren wichtiger Daten verantwortlich.
- Dasselbe gilt auch für das freigegebene sapmnt-Verzeichnis, das ein freigegebener Datenträger (Windows), eine CIFS-Freigabe (Windows) oder NFS-Freigabe (Linux) ist. Sie müssen eine Technologie verwenden, die einen solchen freigegebenen Datenträger oder eine Dateifreigabe zwischen den Zonen repliziert. Im Moment werden die folgenden Technologien unterstützt:
    - Für Windows wird eine Clusterlösung, die SIOS Datakeeper wie in [Gruppieren eine SAP ASCS/SCS-Instanz auf einem Windows-Failovercluster mithilfe freigegebener Clusterdatenträger in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) dokumentiert verwendet, zonenübergreifend unterstützt.
    - Für SUSE Linux wird eine NFS-Freigabe, die wie in [Hochverfügbarkeit für NFS auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) dokumentiert erstellt wird, unterstützt.
    - Zu diesem Zeitpunkt wird die Lösung, die Windows-SOFS (Scale-Out File Services) wie in [Vorbereiten der Azure-Infrastruktur für SAP-Hochverfügbarkeit mit einem Windows-Failovercluster und einer Dateifreigabe für SAP ASCS-/SCS-Instanzen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) dokumentiert verwendet, **nicht zonenübergreifend unterstützt**.
- Die dritte Zone wird verwendet, um das SBD-Gerät für den Fall zu hosten, dass Sie einen [SUSE Linux Pacemaker-Cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) oder zusätzliche Anwendungsinstanzen erstellen.





## <a name="next-steps"></a>Nächste Schritte
In den nächsten Schritten erhalten Sie Informationen zum Azure-Verfügbarkeitszonen übergreifenden Bereitstellen:

- [Gruppieren einer SAP ASCS/SCS-Instanz in einem Windows-Failovercluster mithilfe freigegebener Clusterdatenträger in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Vorbereiten der Azure-Infrastruktur für SAP-Hochverfügbarkeit mit einem Windows-Failovercluster und einer Dateifreigabe für SAP ASCS-/SCS-Instanzen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






