---
title: Verfügbarkeit von SAP HANA innerhalb einer Azure-Region | Microsoft-Dokumentation
description: Vorgänge von SAP HANA auf nativen Azure-VMs
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/5/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 28fe9bc7c8cb1d59df404b7dd7429def54648a18
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>Verfügbarkeit von SAP HANA innerhalb einer Azure-Region
In diesem Abschnitt werden verschiedene Szenarien erläutert, die die Verfügbarkeitsszenarien innerhalb einer Azure-Region darstellen. Azure verfügt über zahlreiche Regionen, die auf der ganzen Welt verteilt sind. Die Liste der Azure-Regionen finden Sie im Artikel [Azure-Regionen](https://azure.microsoft.com/regions/). Für die Bereitstellung von SAP HANA auf VMs in einer Azure-Region bietet Microsoft die Bereitstellung einer einzelnen VM mit einer HANA-Instanz. Zur Erhöhung der Verfügbarkeit können Sie zwei VMs mit zwei HANA-Instanzen innerhalb einer [Azure-Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) bereitstellen, die aus Verfügbarkeitsgründen die HANA-Systemreplikation verwenden. Azure bietet eine Public Preview für [Azure-Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview). Diese Verfügbarkeitszonen werden an dieser Stelle noch nicht im Detail erläutert. Es werden lediglich einige allgemeine Überlegungen zur Verwendung von Verfügbarkeitsgruppen im Vergleich zu Verfügbarkeitszonen erläutert.

Worin besteht der Unterschied zwischen Verfügbarkeitsgruppen und Verfügbarkeitszonen von Azure? In Azure-Regionen, in denen Verfügbarkeitszonen angeboten werden, umfassen die Regionen mehrere Rechenzentren, die von der Stromversorgung, der Kühlung und dem Netzwerk unabhängig sind. Es werden unterschiedliche Zonen in einer einzelnen Azure-Region angeboten, um Ihnen die Bereitstellung von Anwendungen über zwei oder drei Verfügbarkeitszonen hinweg zu ermöglichen. Unter der Annahme, dass sich Probleme an Stromversorgungsquellen und/oder Netzwerken nur auf die Infrastruktur einer Verfügbarkeitszone auswirkt, ist Ihre Anwendungsbereitstellung innerhalb einer Azure-Region weiterhin voll funktionsfähig. Dies kann schließlich zu einer reduzierten Kapazität führen, da einige VMs in einer Zone verloren gehen könnten. VMs in den anderen beiden Zonen werden jedoch weiterhin ausgeführt. 
 
Eine Azure-Verfügbarkeitsgruppe ist eine Funktion zur logischen Gruppierung, mit der Sie in Azure sicherstellen können, dass die darin enthaltenen VM-Ressourcen voneinander fehlerisoliert sind, wenn sie in einem Azure-Rechenzentrum bereitgestellt werden. Azure stellt sicher, dass die virtuellen Computer innerhalb einer Verfügbarkeitsgruppe auf mehrere physische Server, Compute-Racks, Speichereinheiten und Netzwerkswitches verteilt werden. Dies wird in einigen Azure-Dokumentationen alternativ als Platzierungen in verschiedene [Update- und Fehlerdomänen](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) bezeichnet. Diese Platzierungen erfolgen normalerweise in einem Azure-Rechenzentrum. Unter der Annahme, dass Probleme an Stromversorgungsquellen und/oder Netzwerken das Rechenzentrum beeinträchtigen, würde Ihre gesamte Kapazität in einer Azure-Region betroffen werden.

Bei der Platzierung von Rechenzentren, die Azure-Verfügbarkeitszonen darstellen, wird ein Mittelweg zwischen der Übermittlung von Netzwerklatenzen zwischen Diensten, die in anderen für die meisten Anwendungen zulässigen Zonen bereitgestellt werden, und einer bestimmten Entfernung zwischen den Rechenzentren verfolgt. Damit soll im Idealfall erreicht werden, dass das Stromversorgungsnetz und die Infrastruktur für alle Verfügbarkeitszonen in dieser Region vor Naturkatastrophen geschützt bleiben. Doch angesichts des gewaltigen Ausmaßes, das sich bei Naturkatastrophen abzeichnet, können Verfügbarkeitszonen nicht immer die gewünschte Verfügbarkeit innerhalb einer Region zur Verfügung stellen. Denken Sie an Hurrikan Maria, der am 20.08.2017 Puerto Rico durchzog und auf der Insel mit einer Länge von 150 km einen Totalausfall anrichtete.   
  


## <a name="single-vm-scenario"></a>Szenario mit einer einzelnen VM
In diesem Szenario haben Sie einen virtuellen Azure-Computer für die SAP HANA-Instanz erstellt. Sie haben Azure Storage Premium zum Hosten des Betriebssystemdatenträgers und aller Datenträger verwendet. Die SLA von Azure mit einer Betriebszeit von 99,9 % und die SLAs anderer Azure-Komponenten reichen für Sie aus, um Ihren Verfügbarkeits-SLAs für Ihre Kunden gerecht zu werden. In diesem Szenario besteht kein Bedarf dafür, eine Azure-Verfügbarkeitsgruppe für VMs, die die DBMS-Ebene ausführen, zu nutzen. In diesem Szenario kommen zwei verschiedene Features zum Einsatz:

- Automatischer Neustart von Azure-VMs (auch als „Azure-Dienstreparatur“ bezeichnet)
- Automatischer Neustart von SAP HANA

Die Funktion „Automatischer Neustart von Azure-VMs“ bzw. „Dienstreparatur“ funktioniert in Azure auf zwei Ebenen:

- Überprüfung der Integrität einer auf dem Serverhost gehosteten VM durch den Azure-Serverhost
- Überwachung der Integrität und Verfügbarkeit des Serverhosts durch den Azure Fabric Controller

Bei jeder VM, die von einem Azure-Serverhost gehostet wird, wird die Integrität der gehosteten VMs durch eine Funktion zur Integritätsprüfung überwacht. Falls VMs in einen fehlerhaften Zustand wechseln sollten, kann ein VM-Neustart durch den Azure-Host-Agent initiiert werden, der die Integrität der VM überprüft. Der Azure Fabric Controller überprüft die Integrität des Hosts, indem er eine Vielzahl verschiedener Parameter prüft, die auf Probleme mit der Hosthardware hinweisen. Er prüft jedoch auch den Zugriff auf den Host über das Netzwerk. Ein Hinweis auf Probleme mit dem Host kann z.B. zu folgenden Aktionen führen:

- Neustart des Hosts und der VMs, die auf dem Host ausgeführt wurden, wenn der Host einen fehlerhaften Zustand signalisiert
- Neustart des Hosts und der VMs, die ursprünglich auf einem fehlerfreien Host gehostet wurden, für den Fall, dass sich der Host nach dem Neustart nicht in einem fehlerfreien Zustand befindet. In diesem Fall wird der Host als fehlerhaft gekennzeichnet und steht erst zur Verwendung für weitere Bereitstellungen bereit, wenn der Fehler behoben wurde. Anderenfalls muss der Host ersetzt werden.
- Sofortiger Neustart der VMs auf einem fehlerfreien Host in Fällen, in denen beim fehlerhaften Host Probleme im Zusammenhang mit dem Neustart auftreten. 

Durch die von Azure bereitgestellte Host- und VM-Überwachung werden Azure-VMs, bei denen Probleme mit dem Host auftreten, automatisch auf einem fehlerfreien Azure-Host neu gestartet. 

Das zweite Feature, auf das Sie in einem solchen Szenario angewiesen sind, betrifft den Vorgang, bei dem der auf einer solchen neu gestarteten VM ausgeführte HANA-Dienst nach dem Neustart der VM automatisch gestartet wird. Das Feature [Automatischer Neustart des HANA-Diensts](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) kann über die Watchdogdienste der verschiedenen HANA-Dienste konfiguriert werden.

Dieses Szenario mit einer einzelnen VM könnte durch Hinzufügen eines inaktiven Failoverknotens zu einer SAP HANA-Konfiguration optimiert werden. Dies wird in der SAP HANA-Dokumentation auch als [automatisches Hostfailover](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html) bezeichnet. Diese Konfiguration kann bei lokalen Bereitstellungen sinnvoll sein, bei denen die Serverhardware begrenzt ist und Sie für eine Gruppe von Produktionshosts einen einzelnen Serverknoten als Knoten zum automatischen Hostfailover reservieren. In Szenarien wie bei Azure, bei dem die zugrunde liegende Infrastruktur von Azure einen fehlerfreien Zielserver für einen erfolgreichen Neustart einer VM bereitstellt, ist von dem SAP HANA-Szenario mit dem automatischen Hostfailover jedoch abzuraten. 

Aus diesem Grund verwalten wir keine Referenzarchitektur, in der das automatische Hostfailover von HANA für einen Standbyknoten vorgesehen ist. Dies gilt auch für SAP HANA-Konfigurationen mit horizontaler Skalierung.


## <a name="availability-scenarios-involving-two-different-vms"></a>Verfügbarkeitsszenarien mit zwei verschiedenen VMs
Durch die Verwendung von zwei Azure-VMs in Azure-Verfügbarkeitsgruppen können Sie die Betriebszeit zwischen diesen beiden VMs erhöhen, wenn diese VMs in eine Azure-Verfügbarkeitsgruppe innerhalb einer Azure-Region platziert werden. Das grundlegende Setup in Azure würde etwa wie in der folgenden Abbildung aussehen: ![Zwei VMs mit allen Ebenen](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Um die verschiedenen Verfügbarkeitsszenarien zu veranschaulichen, werden nur einige der oben genannten Ebenen dargestellt, und die Abbildung wird auf die Ebenen der VMs, Hosts, Verfügbarkeitsgruppen und Azure-Regionen beschränkt. Azure-VNETs, -Ressourcengruppen und -Abonnements spielen für die beschriebenen Szenarien keine Rolle.

### <a name="replicating-backups-to-second-virtual-machine"></a>Replizieren von Sicherungen auf einen zweiten virtuellen Computer
Eines der elementarsten Setups ist die Verwaltung von Sicherungen, insbesondere Sicherungen von Transaktionsprotokollen, die von einer VM auf einen anderen virtuellen Azure-Computer übertragen werden. Sie können einen beliebigen Azure Storage-Typ auswählen. Sie wären für die Skripterstellung der Kopie der geplanten Sicherungen verantwortlich, die für die erste und zweite VM durchgeführt werden. Falls die Instanzen der zweiten VM vorausgesetzt werden sollen, müssten Sie vollständige inkrementelle bzw. differenzielle Sicherungen von Transaktionsprotokollen am erforderlichen Zeitpunkt wiederherstellen. Die Architektur würde wie folgt aussehen: ![Zwei VMs mit Speicherreplikation](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Dieses Setup ist im Hinblick auf hervorragende RPO- und RTO-Zeiten nicht optimal. Insbesondere bei den RTO-Zeiten müssen Einbußen in Kauf genommen werden, da die gesamte Datenbank mit den kopierten Sicherungen vollständig wiederhergestellt werden muss. Dieses Setup ist jedoch nützlich, um nach einer unbeabsichtigten Datenlöschung in den Hauptinstanzen Wiederherstellungen durchzuführen. Mit einem solchen Setup können Sie jederzeit eine Wiederherstellung zu einem bestimmten Zeitpunkt durchführen, die Daten extrahieren und die gelöschten Daten in Ihre Hauptinstanz importieren. Daher kann es sinnvoll sein, eine solche Methode zur Sicherungskopie in Kombination mit anderen Hochverfügbarkeitsfunktionen zu verwenden. In Phasen, in denen lediglich die Sicherungen kopiert werden, kann es ausreichend sein, anstelle der VM, auf der die SAP HANA-Hauptinstanzen ausgeführt werden, eine kleinere VM zu verwenden. Beachten Sie jedoch, dass bei kleineren VMs eine geringere Anzahl von VHDs angefügt werden kann. Unter [Größen für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) finden Sie die Grenzwerte für die einzelnen VM-Typen.

### <a name="using-sap-hana-system-replication-without-automatic-failover"></a>Verwendung der SAP HANA-Systemreplikation ohne automatisches Failover
Sie verwenden die SAP HANA-Systemreplikation für die folgenden Szenarien. Die von SAP veröffentlichte Dokumentation finden Sie im Artikel [System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) (Systemreplikation). Zwischen zwei Azure-VMs in einer einzelnen Azure-Region stehen Ihnen zwei unterschiedliche Konfigurationen zur Verfügung, bei denen einige Unterschiede bezüglich der Recovery Time Objective bestehen. Im Allgemeinen sind Szenarien ohne automatisches Failover für Szenarien innerhalb einer Azure-Region möglicherweise nicht allzu relevant. Dies ist darauf zurückzuführen, dass in den meisten Ausfallszenarien in der Azure-Infrastruktur, die Azure-Dienstreparatur die primäre VM auf einem anderen Host neu startet. In nur wenigen Grenzfällen kann eine Konfiguration dieser Art bei Ausfallszenarien Abhilfe leisten. Alternativ kann dies auf einige Fälle zutreffen, die Sie als Kunde realisieren möchten – insbesondere im Hinblick auf Effizienz.

#### <a name="using-hana-system-replication-without-auto-failover-and-without-data-pre-load"></a>Verwendung der HANA-Systemreplikation ohne automatisches Failover und ohne Laden von Daten im Voraus 
In diesem Szenario verwenden Sie die SAP HANA-Systemreplikation, um Daten auf synchrone Weise zu verschieben und so ein Recovery Point Objective (RPO) von 0 zu erzielen. Andererseits verfügen Sie über eine ausreichend lange Recovery Time Objective (RTO), sodass Sie weder ein Failover durchführen noch im Voraus Daten in den HANA-Instanzcache laden müssen. In einem solchen Fall haben Sie die Möglichkeit, Ihre Konfiguration durch folgende Maßnahmen wirtschaftlicher zu gestalten:

- Sie können eine weitere SAP HANA-Instanz auf der zweiten VM ausführen, die den Großteil des VM-Speichers belegt. Normalerweise übernimmt dies eine Instanz, die bei einem Failover auf die zweite VM heruntergefahren werden könnte. So können die replizierten Daten in den Cache der als Ziel festgelegten HANA-Instanz auf der zweiten VM geladen werden.
- Sie können eine kleinere VM als zweite VM verwenden. Bei einem Failover müsste vor dem manuellen Failover ein Schritt durchgeführt werden, bei dem Sie die VM-Größe auf die Größe der Quell-VM anpassen würden. Das Szenario sieht folgendermaßen aus:

![Zwei VMs mit Speicherreplikation](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Auch ohne Daten im Voraus in das HANA-Systemreplikationsziel zu laden, benötigen Sie mindestens 64 GB Speicher und darüber hinaus ausreichend Speicher, um die Rowstore-Daten im Speicher der Zielinstanz beizubehalten.

#### <a name="using-hana-system-replication-without-auto-failover-and-with-data-pre-load"></a>Verwendung der HANA-Systemreplikation ohne automatisches Failover und mit Laden von Daten im Voraus
Der Unterschied zu dem zuvor vorgestellten Szenario besteht darin, dass die Daten, die auf die HANA-Instanz auf der zweiten VM repliziert werden, im Voraus geladen werden. Hierdurch würden zwei Vorteile wegfallen, von denen Sie beim Szenario ohne das Laden von Daten im Voraus profitieren könnten. In diesem Fall können Sie kein weiteres SAP HANA-System auf der zweiten VM ausführen. Des Weiteren können Sie keine kleinere VM-Größe verwenden. Daher wird dieses Szenario bei Kunden selten implementiert.


### <a name="using-sap-hana-system-replication-with-automatic-failover"></a>Verwendung der SAP HANA-Systemreplikation mit automatischem Failover

Bei der standardmäßigen Verfügbarkeitskonfiguration innerhalb einer Azure-Region, die die meisten Kunden mit SAP HANA implementieren, ist ein Failovercluster für zwei virtuelle Azure-Computer mit dem SUSE Linux Enterprise Server (SLES) definiert. Der Linux-Cluster vom SLES basiert auf dem [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker)-Framework in Verbindung mit einem [STONITH](http://linux-ha.org/wiki/STONITH)-Gerät. Aus SAP HANA-Sicht wird der verwendete Replikationsmodus synchronisiert, und ein automatisches Failover wird konfiguriert. Auf der zweiten VM fungiert die SAP HANA-Instanz als unmittelbar betriebsbereiter Standbyknoten, der einen synchronen Datenstrom von Änderungsdatensätzen von der primären SAP HANA-Instanz empfängt. Da Transaktionen von der Anwendung auf dem primären HANA-Knoten committed werden, wartet dieser mit der Bestätigung des Commits in der Anwendung, bis der sekundäre SAP HANA-Knoten den Commitdatensatz bestätigt hat. SAP HANA weist zwei verschiedene Modi für die synchrone Replikation auf. Einzelheiten und Informationen zu den Unterschieden zwischen diesen beiden Modi für die synchrone Replikation finden Sie im Artikel [Replication modes for SAP HANA System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html) (Replikationsmodi für die SAP HANA-Systemreplikation).

Die allgemeine Konfiguration sieht wie folgt aus:

![Zwei VMs mit Speicherreplikation und Failover](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Diese Lösung wird ausgewählt, da Sie hierüber eine RPO von 0 und äußerst kurze RTO-Zeiten erzielen. Konfigurieren Sie die SAP HANA-Clientkonnektivität so, dass die SAP HANA-Clients zum Herstellen einer Verbindung mit der HANA-Systemreplikationskonfiguration die virtuelle IP-Adresse verwenden. Hierdurch entfällt die Notwendigkeit, die Anwendung bei einem Failover auf den sekundären Knoten erneut zu konfigurieren. In dieser Lösung müssen die Azure-VM-SKUs für den primären oder sekundären Knoten identisch sein.  


## <a name="next-steps"></a>Nächste Schritte
Eine ausführliche Anleitung zur Einrichtung einer solchen Konfiguration in Azure finden Sie in folgenden Artikeln:

- [Einrichten der SAP HANA-Systemreplikation auf virtuellen Azure-Computern](sap-hana-high-availability.md)
- [Your SAP on Azure – Part 4 – High Availability for SAP HANA using System Replication (SAP für Azure – Teil 4 – Hochverfügbarkeit für SAP HANA durch die Systemreplikation)](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Weitere Informationen zur SAP HANA-Verfügbarkeit in Azure-Regionen finden Sie in folgenden Artikeln:

- [Verfügbarkeit von SAP HANA in verschiedenen Azure-Regionen](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

