---
title: Schützen einer SAP NetWeaver-Anwendungsbereitstellung mit mehreren Schichten mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie SAP NetWeaver-Anwendungsbereitstellungen mit Azure Site Recovery schützen.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: b6ab734186f23d51d60e51bd0946329d5209097b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-by-using-site-recovery"></a>Schützen einer SAP NetWeaver-Anwendungsbereitstellung mit mehreren Ebenen mit Azure Site Recovery

Die meisten großen und mittleren SAP-Bereitstellungen weisen eine Form von Notfallwiederherstellungslösung auf. Die Bedeutung robuster und testfähiger Notfallwiederherstellungslösungen ist weiter gewachsen, da immer mehr Kerngeschäftsprozesse in Anwendungen wie SAP verschoben werden. Azure Site Recovery wurde mit SAP-Anwendungen getestet und integriert. Azure Site Recovery übersteigt die Möglichkeiten der meisten lokalen Notfallwiederherstellungslösungen, und dies zu geringeren Gesamtkosten (TCO) als Lösungen von Mitbewerbern.

Mit Azure Site Recovery haben Sie folgende Möglichkeiten:
* Ermöglichen Sie den **Schutz von lokal ausgeführten SAP NetWeaver-Produktionsanwendungen und Anwendungen, die nicht von NetWeaver stammen**, indem Sie die Komponenten in Azure replizieren.
* Ermöglichen Sie den **Schutz von in Azure ausgeführten SAP NetWeaver-Produktionsanwendungen und Anwendungen, die nicht von NetWeaver stammen**, indem Sie die Komponenten in einem anderen Azure-Datencenter replizieren.
* **Vereinfachen Sie die Cloudmigration** mithilfe von Azure Site Recovery zum Migrieren Ihrer SAP-Bereitstellung nach Azure.
* **Vereinfachen Sie SAP-Projektupgrades, Testläufe und Prototyperstellung**, indem Sie bei Bedarf einen Produktionsklon zum Testen von SAP-Anwendungen erstellen.

In diesem Artikel wird beschrieben, wie Sie SAP NetWeaver-Anwendungsbereitstellungen mit [Azure Site Recovery](site-recovery-overview.md) schützen. Der Artikel umfasst bewährte Methoden für den Schutz einer SAP NetWeaver-Bereitstellung mit drei Ebenen in Azure durch die Replikation mit Azure Site Recovery in ein anderes Azure-Rechenzentrum. Beschrieben werden die unterstützten Szenarien und Konfigurationen sowie das Ausführen von Testfailovern (Übungen zur Notfallwiederherstellung) als auch von tatsächlichen Failovern.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, stellen Sie sicher, dass Sie wissen, wie Sie die folgenden Aufgaben ausführen:

* [Replikation eines virtuellen Computers in Azure](azure-to-azure-walkthrough-enable-replication.md)
* [Entwerfen eines Netzwerks für die Wiederherstellung](site-recovery-azure-to-azure-networking-guidance.md)
* [Ausführen eines Testfailovers nach Azure](azure-to-azure-walkthrough-test-failover.md)
* [Ausführen eines Failovers nach Azure](site-recovery-failover.md)
* [Replizieren eines Domänencontrollers](site-recovery-active-directory.md)
* [Replizieren von SQL Server](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Mit Azure Site Recovery können Sie eine Notfallwiederherstellungslösung in folgenden Szenarien implementieren:
* SAP-Systeme, die in einem Azure-Rechenzentrum ausgeführt werden, das in ein anderes Azure-Rechenzentrum repliziert wird (Azure-zu-Azure-Notfallwiederherstellung). Weitere Informationen finden Sie unter [Architektur der Azure-zu-Azure-Replikation](https://aka.ms/asr-a2a-architecture).
* Lokal auf VMware-Servern (oder physischen Servern) ausgeführte SAP-Systeme, die in einen Standort für die Notfallwiederherstellung in einem Azure-Rechenzentrum repliziert werden (VMware-zu-Azure-Notfallwiederherstellung). Dieses Szenario erfordert einige zusätzliche Komponenten. Weitere Informationen finden Sie unter [VMware in der Architektur für die Azure-Replikation](https://aka.ms/asr-v2a-architecture).
* Lokal unter Hyper-V ausgeführte SAP-Systeme, die in einen Standort für die Notfallwiederherstellung in einem Azure-Rechenzentrum repliziert werden (Hyper-V-zu-Azure-Notfallwiederherstellung). Dieses Szenario erfordert einige zusätzliche Komponenten. Weitere Informationen finden Sie unter [Architektur der Replikation von Hyper-V in Azure](https://aka.ms/asr-h2a-architecture).

In diesem Artikel wird das Szenario der Azure-zu-Azure-Notfallwiederherstellung verwendet, um die SAP-Notfallwiederherstellungsfunktionen von Azure Site Recovery zu veranschaulichen. Da die Replikation mit Site Recovery nicht anwendungsspezifisch ist, gilt der beschriebene Prozess vermutlich auch für andere Szenarien.

### <a name="required-foundation-services"></a>Erforderliche Grunddienste
In dem in diesem Artikel besprochenen Szenario werden die folgenden Foundation-Dienste bereitgestellt:
* Azure ExpressRoute oder Azure VPN Gateway
* Mindestens ein Active Directory-Domänencontroller und ein DNS-Server unter Azure

Es wird empfohlen, diese Infrastruktur einzurichten, bevor Sie Site Recovery bereitstellen.

## <a name="typical-sap-application-deployment"></a>Typische SAP-Anwendungsbereitstellung
Große SAP-Kunden stellen in der Regel zwischen 6 und 20 einzelne SAP-Anwendungen bereit. Die meisten dieser Anwendungen basieren auf SAP NetWeaver ABAP- oder Java-Engines. Diese NetWeaver-Kernanwendungen werden durch viele kleinere, spezifische und eigenständige SAP-Engines (nicht NetWeaver) sowie in der Regel durch einige SAP-fremde Anwendungen unterstützt.  

Es ist wichtig, alle SAP-Anwendungen zu inventarisieren, die in Ihrer Umgebung ausgeführt werden. Bestimmen Sie dann den Bereitstellungsmodus (zwei oder drei Ebenen), Versionen, Patches, Größen, Fehlerraten und die Anforderungen an die Persistenz der Datenträger.

![Diagramm eines typischen SAP-Bereitstellungsmusters](./media/site-recovery-sap/sap-typical-deployment.png)

Schützen Sie die Persistenzebene der SAP-Datenbank über die nativen DBMS-Tools wie SQL Server AlwaysOn, Oracle DataGuard oder SAP HANA System Replication. Wie die SAP-Datenbankebene wird auch die Clientebene nicht von Azure Site Recovery geschützt. Es ist wichtig, Faktoren zu berücksichtigen, die diese Schicht betreffen. Zu diesen Faktoren zählen Verzögerung der DNS-Verteilung, Sicherheit und Remotezugriff auf das Rechenzentrum für die Notfallwiederherstellung.

Azure Site Recovery ist die empfohlene Lösung für die Anwendungsebene, auch für SAP SCS und ASCS. Andere Anwendungen wie Nicht-NetWeaver-SAP-Anwendungen und SAP-fremde Anwendungen bilden einen Teil der allgemeinen SAP-Bereitstellungsumgebung. Sie sollten mit Azure Site Recovery geschützt werden.

## <a name="replicate-virtual-machines"></a>Replizieren von virtuellen Computern
Zu Beginn des Replizieren aller virtuellen Computer für SAP-Anwendungen in ein Azure-Notfallwiederherstellungsrechenzentrum folgen Sie der Anleitung unter [Replikation eines virtuellen Computers in Azure](azure-to-azure-walkthrough-enable-replication.md).

Wenn Sie eine statische IP-Adresse verwenden, können Sie die IP-Adresse angeben, die Sie dem virtuellen Computer zuordnen möchten. Um die IP-Adresse festzulegen, wechseln Sie zu **Einstellungen für Compute und Netzwerk** > **Netzwerkschnittstellenkarte**.

![Screenshot, der zeigt, wie eine private IP-Adresse im Bereich „Site Recovery-Netzwerkschnittstellenkarte“ festgelegt wird](./media/site-recovery-sap/sap-static-ip.png)

## <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans
Ein Wiederherstellungsplan unterstützt die Sequenzierung der verschiedenen Ebenen in einer Anwendung mit mehreren Ebenen während eines Failovers. Die Sequenzierung trägt zur Wahrung der Anwendungskonsistenz bei. Beim Erstellen eines Wiederherstellungsplans für eine Webanwendung mit mehreren Ebenen führen Sie die unter [Erstellen eines Wiederherstellungsplans mit Site Recovery](site-recovery-create-recovery-plans.md) beschriebenen Schritte aus.

### <a name="add-scripts-to-the-recovery-plan"></a>Hinzufügen von Skripts zum Wiederherstellungsplan
Für die korrekte Funktionsweise Ihrer Anwendungen kann es erforderlich sein, nach dem Failover bzw. während eines Testfailovers einige Vorgänge auf den virtuellen Azure-Computern durchzuführen. Sie können einige Vorgänge nach einem Failover automatisieren. Durch Hinzufügen entsprechender Skripts zum Wiederherstellungsplan können Sie beispielsweise den DNS-Eintrag aktualisieren sowie Bindungen und Verbindungen ändern.

### <a name="dns-update"></a>DNS-Update
Wenn das DNS für das dynamische DNS-Update konfiguriert ist, führen virtuelle Computer nach dem Starten normalerweise ein Update des DNS mit der neuen IP-Adresse durch. Falls Sie einen expliziten Schritt für das Update des DNS mit den neuen IP-Adressen der virtuellen Computer hinzufügen möchten, fügen Sie ein [Skript zum Aktualisieren der IP-Adresse im DNS](https://aka.ms/asr-dns-update) als Aktion nach dem Failover den Wiederherstellungsplangruppen hinzu.  

## <a name="example-azure-to-azure-deployment"></a>Beispiel für eine Azure-zu-Azure-Bereitstellung
Das folgende Diagramm zeigt das Szenario der Azure-zu-Azure-Notfallwiederherstellung mit Azure Site Recovery:

![Diagramm eines Azure-zu-Azure-Replikationsszenarios](./media/site-recovery-sap/sap-replication-scenario.png)

* Das primäre Rechenzentrum befindet sich in Singapur (Azure Asien, Südosten). Das Rechenzentrum für die Notfallwiederherstellung befindet sich in Hongkong (Azure Asien, Osten). In diesem Szenario wird lokale Hochverfügbarkeit durch zwei virtuelle Computer mit SQL Server AlwaysOn im synchronen Modus in Singapur bereitgestellt.
* Die Dateifreigabe SAP ASCS stellt Hochverfügbarkeit für die einzelnen SAP-Fehlerquellen bereit. Die Dateifreigabe ASCS erfordert keinen freigegebenen Clusterdatenträger. Anwendungen wie SIOS sind nicht erforderlich.
* Notfallwiederherstellungsschutz für die DBMS-Ebene wird mithilfe der asynchronen Replikation erreicht.
* Dieses Szenario zeigt eine „symmetrische Notfallwiederherstellung“. Dieser Begriff beschreibt eine Lösung für die Notfallwiederherstellung, die ein exaktes Replikat der Produktion ist. Die Notfallwiederherstellung der SQL Server-Lösung verfügt über lokale Hochverfügbarkeit. Die symmetrische Notfallwiederherstellung ist nicht obligatorisch für die Datenbankebene. Viele Kunden nutzen die Flexibilität von Cloudbereitstellungen, um nach einem Notfallwiederherstellungsereignis schnell einen lokalen Hochverfügbarkeitsknoten zu erstellen.
* Das Diagramm zeigt die SAP NetWeaver-ASCS- und Anwendungsserver-Ebene, die durch Azure Site Recovery repliziert wird.

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

1.  Wählen Sie im Azure-Portal Ihren Recovery Services-Tresor aus.
2.  Wählen Sie den Wiederherstellungsplan aus, den Sie für die SAP-Anwendungen erstellt haben.
3.  Wählen Sie **Testfailover** aus.
4.  Wählen Sie den Wiederherstellungspunkt und das virtuelle Azure-Netzwerk aus, um den Prozess für das Testfailover zu starten.
5.  Wenn die sekundäre Umgebung aktiv ist, können Sie Überprüfungen durchführen.
6.  Klicken Sie nach Abschluss der Überprüfungen auf **Testfailover bereinigen**, um die Failoverumgebung zu bereinigen.

Weitere Informationen finden Sie unter [Testfailover nach Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Ausführen eines Failovers

1.  Wählen Sie im Azure-Portal Ihren Recovery Services-Tresor aus.
2.  Wählen Sie den Wiederherstellungsplan aus, den Sie für die SAP-Anwendungen erstellt haben.
3.  Wählen Sie **Failover** aus.
4.  Wählen Sie den Wiederherstellungspunkt aus, um den Failoverprozess zu starten.

Weitere Informationen finden Sie unter [Failover in Site Recovery](site-recovery-failover.md).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen einer Lösung für die Notfallwiederherstellung für SAP NetWeaver-Bereitstellungen mithilfe von Site Recovery finden Sie unter den herunterladbaren Whitepapers [SAP NetWeaver: Erstellen einer Notfallwiederherstellungslösung mit Azure Site Recovery](http://aka.ms/asr-sap). Das Whitepaper behandelt auch Empfehlungen für unterschiedliche SAP-Architekturen, listet unterstützte Anwendungen und VM-Typen für SAP in Azure auf und beschreibt Testplanoptionen für Ihre Notfallwiederherstellungslösung.
* Erfahren Sie mehr über das [Replizieren anderer Workloads](site-recovery-workload.md) mit Site Recovery.
