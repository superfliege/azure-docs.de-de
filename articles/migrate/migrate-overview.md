---
title: Informationen zu Azure Migrate | Microsoft-Dokumentation
description: "Enthält eine Übersicht über den Azure Migrate-Dienst."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7b313bb4-c8f4-43ad-883c-789824add3288
ms.service: migrate
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: raynew
ms.openlocfilehash: d3d5a3bcd3be55d1915ff7fdc6d82aebbb992fc7
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="about-azure-migrate"></a>Informationen zu Azure Migrate

Mit dem Azure Migrate-Dienst werden lokale Workloads für die Migration zu Azure bewertet. Der Dienst bewertet die Eignung für die Migration und die leistungsbasierte Größenauslegung und stellt Kostenschätzungen für die Ausführung Ihrer lokalen Computer in Azure bereit. Wenn Sie die Nutzung von Lift & Shift-Migrationen erwägen oder sich in einer frühen Bewertungsphase der Migration befinden, ist dieser Dienst gut für Sie geeignet. Nach der Bewertung können Sie Dienste wie Azure Site Recovery und Azure Database Migration nutzen, um die Computer zu Azure zu migrieren.

> [!NOTE]
> Azure Migrate befindet sich derzeit in der Vorschauphase und unterstützt Produktionsworkloads.

## <a name="why-use-azure-migrate"></a>Gründe für die Verwendung von Azure Migrate

Azure Migrate bietet Folgendes:

- **Bewerten der Bereitschaft für Azure**: Bewerten Sie, ob Ihre lokalen Computer für die Ausführung in Azure geeignet sind. 
- **Nutzen von Empfehlungen zur Größe**: Sie erhalten basierend auf dem Leistungsverlauf lokaler VMs Empfehlungen zur Größenauslegung für Azure-VMs nach der Migration. 
- **Schätzen von monatlichen Kosten**: Sie erhalten Kostenschätzungen zur Ausführung von lokalen Computern in Azure.
- **Zuverlässiges Migrieren**: Wenn Sie lokale Computer zu Bewertungszwecken gruppieren, können Sie die Zuverlässigkeit der Bewertung erhöhen, indem Sie Abhängigkeiten visualisieren. Sie können die genauen Abhängigkeiten für einen bestimmten Computer oder für alle Computer einer Gruppe anzeigen.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Derzeit können Sie lokale virtuelle VMware-Computer (VMs) für die Migration zu Azure-VMs bewerten.
> [!NOTE]
> Für die Unterstützung für Hyper-V, die in einigen Monaten verfügbar sein wird, wurde eine Roadmap erstellt. Wir empfehlen Ihnen, in der Zwischenzeit den Azure Site Recovery-Bereitstellungsplaner zu verwenden, um die Migration von Hyper-V-Workloads zu planen. 
- Sie können bis zu 1.000 VMs in einer einzelnen Bewertung und bis zu 1.500 Computer in einem einzelnen Azure Migrate-Projekt bewerten. Wenn Sie die Bewertung ausweiten müssen, können Sie die Anzahl von Projekten oder Bewertungen erhöhen. [Weitere Informationen](how-to-scale-assessment.md).
- VMs, die Sie bewerten möchten, müssen mit vCenter Server, Version 5.5, 6.0 oder 6.5, verwaltet werden.
- Sie können ein Azure Migrate-Projekt nur in der Region „USA, Westen-Mitte“ erstellen. Dies hat aber keinerlei Auswirkung auf die Möglichkeit, Ihre Migration für einen anderen Azure-Zielstandort zu planen. Der Standort des Migrationsprojekts wird nur zum Speichern der Metadaten verwendet, die in der lokalen Umgebung ermittelt wurden.
- Das Azure Migrate-Portal ist derzeit nur auf Englisch verfügbar. 
- Für Azure Migrate wird derzeit nur die Replikation vom Typ [Lokal redundanter Speicher (LRS)](../storage/common/storage-introduction.md#replication) unterstützt.

## <a name="what-do-i-need-to-pay-for"></a>Für was muss ich zahlen?

Azure Migrate ist ohne Aufpreis erhältlich. Während der öffentlichen Vorschauphase fallen aber für die Nutzung der Funktionen zur Abhängigkeitsvisualisierung zusätzliche Kosten an. Zur Unterstützung der [Abhängigkeitsvisualisierung](concepts-dependency-visualization.md) erstellt Azure Migrate standardmäßig einen Log Analytics-Arbeitsbereich. Wenn Sie die Abhängigkeitsvisualisierung nutzen oder den Arbeitsbereich außerhalb von Azure Migrate verwenden, werden Ihnen die Kosten für den Einsatz des Arbeitsbereichs berechnet. [Hier finden Sie weitere Informationen zu den Kosten](https://azure.microsoft.com/en-us/pricing/details/insight-analytics/). Wenn der Dienst allgemein verfügbar wird, fallen für die Nutzung der Funktionen zur Abhängigkeitsvisualisierung keine Kosten an.


## <a name="whats-in-an-assessment"></a>Was umfasst eine Bewertung?

Azure Migrate-Bewertungen basieren auf den Einstellungen, die in dieser Tabelle zusammengefasst sind.

**Einstellung** | **Details**
--- | ---
**Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll. Standardmäßig ist dies der Ort, an dem Sie das Azure Migrate-Projekt erstellen. Sie können diese Einstellung ändern.   
**Speicherredundanz** | Der Speichertyp, der von den Azure-VMs nach der Migration verwendet wird. LRS ist die Standardeinstellung.
**Tarife** | Bei der Bewertung wird berücksichtigt, ob Sie für Software Assurance registriert sind und in den Genuss des [Azure-Vorteils bei Hybridnutzung](https://azure.microsoft.com/pricing/hybrid-use-benefit/) kommen. Außerdem werden Azure-Angebote berücksichtigt, die angewendet werden sollten, und Sie können abonnementspezifische Rabatte (%) angeben, die Sie zusätzlich zum Angebot erhalten. 
**Tarif** | Sie können den [Tarif (Basic/Standard)](../virtual-machines/windows/sizes-general.md) für die Azure-VMs angeben. Auf diese Weise können Sie in Abhängigkeit davon, ob Sie sich in einer Produktionsumgebung befinden, zu einer geeigneten Azure-VM-Familie migrieren. Standardmäßig wird der Tarif [Standard](../virtual-machines/windows/sizes-general.md) verwendet.
**Leistungsverlauf** | Standardmäßig wertet Azure Migrate die Leistung von lokalen Computern aus, indem ein Monat des Verlaufs mit einem Perzentilwert von 95% verwendet wird. Sie können diese Einstellung ändern.
**Komfortfaktor** | Bei Azure Migrate wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für eine VM mit zehn Kernen und 20% Auslastung ergibt sich normalerweise eine VM mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen. Die Standardeinstellung für den Komfortfaktor lautet 1.3x.


## <a name="how-does-azure-migrate-work"></a>Wie funktioniert Azure Migrate?

1.  Sie erstellen ein Azure Migrate-Projekt.
2.  Für Azure Migrate wird eine lokale VM genutzt, die als „Collectorappliance“ bezeichnet wird, um Informationen zu Ihren lokalen Computern zu ermitteln. Für die Erstellung der Appliance laden Sie die Setupdatei im Format „Open Virtualization Appliance“ (.ova) herunter und importieren sie als VM auf Ihrem lokalen vCenter Server.
3.  Sie stellen eine Verbindung mit der VM her, indem Sie Lesezugriff-Anmeldeinformationen für den vCenter Server verwenden und den Collector ausführen.
4.  Der Collector erfasst VM-Metadaten mit VMware PowerCLI-Cmdlets. Die Ermittlung erfolgt ohne Agent, und auf VMware-Hosts oder VMs wird nichts installiert. Die gesammelten Metadaten enthalten VM-Informationen (Kerne, Arbeitsspeicher, Datenträger, Datenträgergrößen und Netzwerkadapter). Außerdem werden Leistungsdaten für VMs gesammelt, z.B. CPU- und Arbeitsspeicherauslastung, Datenträger-IOPS, Datenträgerdurchsatz (MBit/s) und Netzwerkausgabe (MBit/s).
5.  Die Metadaten werden per Pushvorgang in das Azure Migrate-Projekt übertragen. Sie können sie im Azure-Portal anzeigen.
6.  Zur Durchführung der Bewertung fassen Sie VMs in Gruppen zusammen. Beispielsweise können Sie VMs gruppieren, auf denen dieselbe App ausgeführt wird. Sie können VMs gruppieren, indem Sie in vCenter oder im vCenter-Portal das Tagging nutzen. Verwenden Sie die Visualisierung, um Abhängigkeiten für einen bestimmten Computer oder für alle Computer einer Gruppe zu überprüfen.
7.  Sie erstellen eine Bewertung für eine Gruppe.
8.  Nach Abschluss der Bewertung können Sie das Ergebnis im Portal anzeigen oder im Excel-Format herunterladen.



  ![Azure Planner-Architektur](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Wie lauten die Portanforderungen?

In der Tabelle sind die Ports zusammengefasst, die für die Azure Migrate-Kommunikation erforderlich sind.

|Komponente          |Für die Kommunikation mit     |Erforderlicher Port  |Grund   |
|-------------------|------------------------|---------------|---------|
|Collector          |Azure Migrate-Dienst   |TCP 443        |Der Collector stellt über den SSL-Port 443 eine Verbindung mit dem Dienst her.|
|Collector          |vCenter Server          |Standard 9443   | Standardmäßig stellt der Collector eine Verbindung mit dem vCenter Server über Port 9443 her. Wenn der Server über einen anderen Port lauscht, sollte er auf der Collector-VM als Port für die ausgehende Richtung konfiguriert werden. |
|Lokale VM     | OMS-Arbeitsbereich          |[TCP 443](../log-analytics/log-analytics-windows-agents.md#system-requirements-and-required-configuration) |Der MMA-Agent nutzt TCP 443, um eine Verbindung mit Log Analytics herzustellen. Sie benötigen diesen Port nur, wenn Sie das Feature zur Visualisierung von Abhängigkeiten verwenden und den MMA-Agent installieren. |


  
## <a name="what-happens-after-assessment"></a>Was passiert nach der Bewertung?

Nachdem Sie die lokalen Computer für die Migration mit dem Azure Migrate-Dienst bewertet haben, können Sie einige Tools nutzen, um die Migration durchzuführen:

- **Azure Site Recovery**: Sie können Azure Site Recovery wie folgt für die Migration zu Azure verwenden:
  - Vorbereiten von Azure-Ressourcen, einschließlich eines Azure-Abonnements, eines virtuellen Azure-Netzwerks und eines Speicherkontos.
  - Vorbereiten Ihrer lokalen VMware-Server für die Migration. Sie überprüfen VMware-Supportanforderungen für Site Recovery, richten VMware-Server für die Ermittlung ein und bereiten die Installation des Site Recovery-Mobilitätsdiensts auf VMs vor, die Sie migrieren möchten. 
  - Einrichten der Migration. Sie richten einen Recovery Services-Tresor ein, konfigurieren die Migrationseinstellungen für Quelle und Ziel, richten eine Replikationsrichtlinie ein und aktivieren die Replikation. Sie können einen Testlauf der Notfallwiederherstellung durchführen, um zu überprüfen, ob die Migration einer VM zu Azure richtig funktioniert.
  - Ausführen eines Failovers zum Migrieren von lokalen Computern zu Azure. 
  - [Weitere Informationen](../site-recovery/tutorial-migrate-on-premises-to-azure.md) finden Sie im Tutorial zur Site Recovery-Migration.

- **Azure Database Migration**: Wenn Ihre lokalen Computer eine Datenbank ausführen, z.B. SQL Server, MySQL oder Oracle, können Sie den Azure Database Migration Service für die Migration verwenden. [Weitere Informationen](https://azure.microsoft.com/campaigns/database-migration/).



## <a name="next-steps"></a>Nächste Schritte 
[Arbeiten Sie ein Tutorial durch](tutorial-assessment-vmware.md), in dem es um die Erstellung einer Bewertung für eine lokale VMware-VM geht.