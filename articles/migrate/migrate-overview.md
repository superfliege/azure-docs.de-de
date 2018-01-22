---
title: Informationen zu Azure Migrate | Microsoft-Dokumentation
description: "Enthält eine Übersicht über den Azure Migrate-Dienst."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 0bd3d7a9961e7a095684262ae1031f5a3ac0c3fb
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2018
---
# <a name="about-azure-migrate"></a>Informationen zu Azure Migrate

Mit dem Azure Migrate-Dienst werden lokale Workloads für die Migration zu Azure bewertet. Der Dienst bewertet die Eignung für die Migration und die leistungsbasierte Größenauslegung und stellt Kostenschätzungen für die Ausführung Ihrer lokalen Computer in Azure bereit. Wenn Sie die Nutzung von Lift & Shift-Migrationen erwägen oder sich in einer frühen Bewertungsphase der Migration befinden, ist dieser Dienst gut für Sie geeignet. Nach der Bewertung können Sie Dienste wie Azure Site Recovery und Azure Database Migration nutzen, um die Computer zu Azure zu migrieren.

> [!NOTE]
> Azure Migrate befindet sich derzeit in der Vorschauphase und unterstützt Produktionsworkloads.

## <a name="why-use-azure-migrate"></a>Gründe für die Verwendung von Azure Migrate

Azure Migrate bietet Folgendes:

- **Bewerten der Bereitschaft für Azure**: Bewerten Sie, ob Ihre lokalen Computer für die Ausführung in Azure geeignet sind. 
- **Größenempfehlungen**: Nutzen Sie Größenempfehlungen für virtuelle Azure-Computer, die auf dem Leistungsverlauf lokaler virtueller Computer basieren. 
- **Voraussichtliche monatliche Kosten**: Erhalten Sie Kostenschätzungen zur Ausführung lokaler Computer in Azure.  
- **Migration mit hoher Zuverlässigkeit**: Visualisieren Sie Abhängigkeiten von lokalen Computern, um Gruppen von Computern zu erstellen, die Sie gemeinsam bewerten und migrieren möchten. Sie können die genauen Abhängigkeiten für einen bestimmten Computer oder für alle Computer einer Gruppe anzeigen.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Derzeit können Sie lokale virtuelle VMware-Computer (VMs) für die Migration zu Azure-VMs bewerten.

> [!NOTE]
> Unterstützung für Hyper-V ist in der Roadmap enthalten und wird in Kürze verfügbar gemacht. Wir empfehlen Ihnen, in der Zwischenzeit den [Azure Site Recovery-Bereitstellungsplaner](http://aka.ms/asr-dp-hyperv-doc) zu verwenden, um die Migration von Hyper-V-Workloads zu planen. 

- Sie können in einer einzigen Ermittlung bis zu 1.000 und in einem einzelnen Projekt bis zu 1.500 virtuelle Computer ermitteln. Außerdem können Sie in einem einzelnen Assessment bis zu 400 virtuelle Computer bewerten. Wenn Sie die Ermittlung oder Bewertung ausweiten müssen, können Sie die Anzahl von Ermittlungen oder Assessments erhöhen. [Weitere Informationen](how-to-scale-assessment.md)
- VMs, die Sie bewerten möchten, müssen mit vCenter Server, Version 5.5, 6.0 oder 6.5, verwaltet werden.
- Sie können ein Azure Migrate-Projekt nur in der Region „USA, Westen-Mitte“ erstellen. Dies hat aber keinerlei Auswirkung auf die Möglichkeit, Ihre Migration für einen anderen Azure-Zielstandort zu planen. Der Standort des Migrationsprojekts wird nur zum Speichern der Metadaten verwendet, die in der lokalen Umgebung ermittelt wurden.
- Azure Migrate unterstützt nur verwaltete Datenträger für die Migrationsbewertung.

## <a name="what-do-i-need-to-pay-for"></a>Für was muss ich zahlen?

Azure Migrate ist ohne Aufpreis erhältlich. Während der öffentlichen Vorschauphase fallen aber für die Nutzung der Funktionen zur Abhängigkeitsvisualisierung zusätzliche Kosten an. Zur Unterstützung der [Abhängigkeitsvisualisierung](concepts-dependency-visualization.md) erstellt Azure Migrate standardmäßig einen Log Analytics-Arbeitsbereich. Wenn Sie die Abhängigkeitsvisualisierung nutzen oder den Arbeitsbereich außerhalb von Azure Migrate verwenden, werden Ihnen die Kosten für den Einsatz des Arbeitsbereichs berechnet. [Hier finden Sie weitere Informationen zu den Kosten](https://azure.microsoft.com/en-us/pricing/details/insight-analytics/). Wenn der Dienst allgemein verfügbar wird, fallen für die Nutzung der Funktionen zur Abhängigkeitsvisualisierung keine Kosten an.


## <a name="whats-in-an-assessment"></a>Was umfasst eine Bewertung?

Mit einer Bewertung können Sie die Azure-Eignung lokaler virtueller Computer ermitteln sowie geeignete Größenempfehlungen und Kostenschätzungen für die Ausführung der virtuellen Computer in Azure erhalten. Die Eigenschaften, auf denen die Bewertungen basieren, sind in der folgenden Tabelle zusammengefasst. Diese Eigenschaften können über das Azure Migrate-Portal geändert werden. 

**Eigenschaft** | **Details**
--- | ---
**Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll. Der Zielstandort ist standardmäßig auf „USA, Westen 2“ festgelegt. 
**Speicherredundanz** | Der Speichertyp, der von den Azure-VMs nach der Migration verwendet wird. LRS ist die Standardeinstellung.
**Tarife** | Bei der Bewertung wird berücksichtigt, ob Sie für Software Assurance registriert sind und in den Genuss des [Azure-Vorteils bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/) kommen. Außerdem werden Azure-Angebote berücksichtigt, die angewendet werden sollten, und Sie können abonnementspezifische Rabatte (%) angeben, die Sie zusätzlich zum Angebot erhalten. 
**Preisstufe** | Sie können den [Tarif (Basic/Standard)](../virtual-machines/windows/sizes-general.md) für die Azure-VMs angeben. Auf diese Weise können Sie in Abhängigkeit davon, ob Sie sich in einer Produktionsumgebung befinden, zu einer geeigneten Azure-VM-Familie migrieren. Standardmäßig wird der Tarif [Standard](../virtual-machines/windows/sizes-general.md) verwendet.
**Leistungsverlauf** | Standardmäßig wertet Azure Migrate die Leistung von lokalen Computern aus, indem ein Monat des Verlaufs mit einem Perzentilwert von 95% verwendet wird. Sie können diese Einstellung ändern.
**Komfortfaktor** | Bei Azure Migrate wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für eine VM mit zehn Kernen und 20% Auslastung ergibt sich normalerweise eine VM mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen. Die Standardeinstellung für den Komfortfaktor lautet 1.3x.


## <a name="how-does-azure-migrate-work"></a>Wie funktioniert Azure Migrate?

1.  Sie erstellen ein Azure Migrate-Projekt.
2.  Für Azure Migrate wird eine lokale VM genutzt, die als „Collectorappliance“ bezeichnet wird, um Informationen zu Ihren lokalen Computern zu ermitteln. Für die Erstellung der Appliance laden Sie die Setupdatei im OVA-Format (Open Virtualization Appliance) herunter und importieren sie als virtuellen Computer auf Ihrem lokalen vCenter Server-Computer.
3.  Sie stellen über die Konsolenverbindung in vCenter Server eine Verbindung mit dem virtuellen Computer her, geben im Zuge der Verbindungsherstellung ein neues Kennwort für den virtuellen Computer an und führen dann auf dem virtuellen Computer die Collectoranwendung aus, um die Ermittlung zu initiieren.
4.  Der Collector erfasst VM-Metadaten mit VMware PowerCLI-Cmdlets. Die Ermittlung erfolgt ohne Agent, und auf VMware-Hosts oder VMs wird nichts installiert. Die gesammelten Metadaten enthalten VM-Informationen (Kerne, Arbeitsspeicher, Datenträger, Datenträgergrößen und Netzwerkadapter). Außerdem werden Leistungsdaten für VMs gesammelt, z.B. CPU- und Arbeitsspeicherauslastung, Datenträger-IOPS, Datenträgerdurchsatz (MBit/s) und Netzwerkausgabe (MBit/s).
5.  Die Metadaten werden per Pushvorgang in das Azure Migrate-Projekt übertragen. Sie können sie im Azure-Portal anzeigen.
6.  Zur Durchführung der Bewertung fassen Sie die ermittelten virtuellen Computer in Gruppen zusammen. Beispielsweise können Sie virtuelle Computer gruppieren, auf denen die gleiche Anwendung ausgeführt wird. Für eine präzisere Gruppierung können Sie mithilfe der Abhängigkeitsvisualisierung Abhängigkeiten eines bestimmten Computers anzeigen oder aber Abhängigkeiten für alle Computer in einer Gruppe anzeigen und die Gruppe weiter eingrenzen.
7.  Anschließend können Sie eine Bewertung für die gebildete Gruppe erstellen. 
8.  Nach Abschluss der Bewertung können Sie das Ergebnis im Portal anzeigen oder im Excel-Format herunterladen.



  ![Azure Planner-Architektur](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Wie lauten die Portanforderungen?

In der Tabelle sind die Ports zusammengefasst, die für die Azure Migrate-Kommunikation erforderlich sind.

|Komponente          |Für die Kommunikation mit     |Erforderlicher Port  |Grund   |
|-------------------|------------------------|---------------|---------|
|Collector          |Azure Migrate-Dienst   |TCP 443        |Der Collector stellt über den SSL-Port 443 eine Verbindung mit dem Dienst her.|
|Collector          |vCenter Server          |Standard 9443   | Standardmäßig stellt der Collector eine Verbindung mit dem vCenter Server über Port 9443 her. Wenn der Server an einem anderen Port lauscht, muss er auf dem virtuellen Collectorcomputer als ausgehender Port konfiguriert werden. |
|Lokale VM     | OMS-Arbeitsbereich (Operations Management Suite)          |[TCP 443](../log-analytics/log-analytics-windows-agent.md) |Der MMA-Agent nutzt TCP 443, um eine Verbindung mit Log Analytics herzustellen. Sie benötigen diesen Port nur, wenn Sie das Feature zur Visualisierung von Abhängigkeiten verwenden und den Microsoft Monitoring Agent (MMA) installieren. |


  
## <a name="what-happens-after-assessment"></a>Was passiert nach der Bewertung?

Nachdem Sie die lokalen Computer für die Migration mit dem Azure Migrate-Dienst bewertet haben, können Sie einige Tools nutzen, um die Migration durchzuführen:

- **Azure Site Recovery**: Sie können Azure Site Recovery wie folgt für die Migration zu Azure verwenden:
  - Vorbereiten von Azure-Ressourcen, einschließlich eines Azure-Abonnements, eines virtuellen Azure-Netzwerks und eines Speicherkontos.
  - Vorbereiten Ihrer lokalen VMware-Server für die Migration. Sie überprüfen VMware-Supportanforderungen für Site Recovery, richten VMware-Server für die Ermittlung ein und bereiten die Installation des Site Recovery-Mobilitätsdiensts auf VMs vor, die Sie migrieren möchten. 
  - Einrichten der Migration. Sie richten einen Recovery Services-Tresor ein, konfigurieren die Migrationseinstellungen für Quelle und Ziel, richten eine Replikationsrichtlinie ein und aktivieren die Replikation. Sie können einen Testlauf der Notfallwiederherstellung durchführen, um zu überprüfen, ob die Migration einer VM zu Azure richtig funktioniert.
  - Ausführen eines Failovers zum Migrieren von lokalen Computern zu Azure. 
  - [Weitere Informationen](../site-recovery/tutorial-migrate-on-premises-to-azure.md) finden Sie im Tutorial zur Site Recovery-Migration.

- **Azure Database Migration**: Wenn Ihre lokalen Computer eine Datenbank ausführen, z.B. SQL Server, MySQL oder Oracle, können Sie den Azure Database Migration Service für die Migration verwenden. [Weitere Informationen](https://azure.microsoft.com/campaigns/database-migration/)



## <a name="next-steps"></a>Nächste Schritte 
[Arbeiten Sie ein Tutorial durch](tutorial-assessment-vmware.md), in dem es um die Erstellung einer Bewertung für eine lokale VMware-VM geht.