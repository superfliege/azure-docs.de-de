---
title: Azure Migrate – Häufig gestellte Fragen (FAQ) | Microsoft-Dokumentation
description: Beantwortet häufig gestellte Fragen zu Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 06/06/2018
ms.author: snehaa
ms.openlocfilehash: b18d2cecfd7556ad3f05d0f63435d16bc29ebab1
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34826133"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate – Häufig gestellte Fragen (FAQ)

Dieser Artikel enthält häufig gestellte Fragen zu Azure Migrate. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Allgemein

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Was unterscheidet Azure Migrate von Azure Site Recovery?

Azure Migrate ist ein Bewertungsdienst, mit dem Sie Ihre lokalen Workloads bestimmen und Ihre Migration zu Azure planen können. Mit [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), das zudem eine Notfallwiederherstellungslösung ist, können Sie lokale Workloads zu IaaS-VMs in Azure migrieren. 

### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Was unterscheidet Azure Migrate von Azure Site Recovery-Bereitstellungsplaner?

Azure Migrate ist ein Tool für die Migrationsplanung und Azure Site Recovery-Bereitstellungsplaner ist ein Tool für die Notfallwiederherstellungsplanung.

**Migration von VMware zu Azure**: Wenn Sie beabsichtigen, Ihre lokalen Workloads zu Azure zu migrieren, verwenden Sie für die Migrationsplanung Azure Migrate. Azure Migrate bewertet lokale Workloads und bietet Anleitungen, Einblicke und Mechanismen, um Sie bei der Migration zu Azure zu unterstützen. Wenn Ihr Migrationsplan fertig ist, können Sie Dienste wie Azure Site Recovery und Azure Database Migration Service nutzen, um die Computer zu Azure zu migrieren.

**Migration von Hyper-V zu Azure**: Aktuell unterstützt Azure Migrate nur die Bewertung virtueller VMware-Computer für die Migration zu Azure. Die Unterstützung für Hyper-V ist Teil der Roadmap für Azure Migrate. In der Zwischenzeit können Sie den ASR-Bereitstellungsplaner verwenden. Nach Aktivierung der Hyper-V-Unterstützung in Azure Migrate, können Sie Azure Migrate zum Planen der Migration von Hyper-V-Workloads verwenden.

**Notfallwiederherstellung von VMware/Hyper-V in Azure**: Wenn Sie beabsichtigen, eine Notfallwiederherstellung (DR) mit Azure Site Recovery (ASR) zu Azure durchzuführen, verwenden Sie den ASR-Bereitstellungsplaner für die DR-Planung. Der ASR-Bereitstellungsplaner führt eine umfassende, ASR-spezifischen Bewertung Ihrer lokalen Umgebung durch. Er bietet Empfehlungen, die ASR für erfolgreiche DR-Vorgänge benötigt, z.B. Replikation und Failover Ihrer virtuellen Computer.  

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Benötigt Azure Migrate vCenter Server zum Ermitteln einer VMware-Umgebung?

Ja, Azure Migrate benötigt vCenter Server zum Ermitteln einer VMware-Umgebung. Die Ermittlung von ESXi-Hosts, die nicht von einem vCenter Server verwaltet werden, wird nicht unterstützt.

## <a name="discovery"></a>Ermittlung

### <a name="what-data-is-collected-by-azure-migrate"></a>Welche Daten werden von Azure Migrate gesammelt?

Azure Migrate unterstützt zwei Arten von der Ermittlung – appliancebasierte Ermittlung und Agent-basierte Ermittlung.
Die appliancebasierte Ermittlung sammelt Metadaten über die lokalen VMs. Die vollständige Liste der von der Appliance gesammelten Metadaten wird unten aufgeführt:

**Konfigurationsdaten des virtuellen Computers**
- Anzeigename des virtuellen Computers (in vCenter)
- Inventurpfad des virtuellen Computers (Host/Cluster/Ordner in vCenter)
- IP-Adresse
- MAC-Adresse
- Betriebssystem
- Anzahl der Kerne, Datenträger, NICs
- Arbeitsspeichergröße, Datenträgergrößen

**Leistungsdaten des virtuellen Computers**
- CPU-Auslastung
- Speicherauslastung
- Für jeden an den virtuellen Computer angefügten Datenträger:
  - Datenträgerlesedurchsatz
  - Datenträgerschreibdurchsatz
  - Datenträgerlesevorgänge pro Sekunde
  - Datenträgerschreibvorgänge pro Sekunde
- Für jeden Netzwerkadapter, der an den virtuellen Computer angefügt ist:
  - Eingehender Netzwerkverkehr
  - Ausgehender Netzwerkverkehr

Die Agent-basierte Ermittlung ist eine Option, die zusätzlicher zur appliancebasierten Ermittlung verfügbar ist, und Kunden beim [Visualisieren von Abhängigkeiten](how-to-create-group-machine-dependencies.md) der lokalen virtuellen Computer unterstützt. Die Dependency-Agents sammeln Details wie FQDN, BS, IP-Adresse, MAC-Adresse, Prozesse, die innerhalb des virtuellen Computers ausgeführt werden, und die eingehenden/ausgehenden TCP-Verbindungen vom virtuellen Computer. Die Agent-basierte Ermittlung ist optional, und Sie müssen die Agents nicht installieren, wenn Sie die Abhängigkeiten der VMs nicht visualisieren möchten.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Wo und wie lange werden die gesammelten Daten gespeichert?

Die von der Collectorappliance gesammelten Daten werden im Azure-Speicherort gespeichert, den Sie beim Erstellen des Migrationsprojekts angeben. Die Daten werden sicher in einem Microsoft-Abonnement gespeichert und gelöscht, wenn der Benutzer das Azure Migrate-Projekt löscht.

Wenn Sie für die Abhängigkeitsvisualisierung Agents auf den VMs installieren, werden die von den Dependency-Agents gesammelten Daten in den USA in einem OMS-Arbeitsbereich gespeichert, der im Abonnement des Benutzers erstellt wurde. Diese Daten werden gelöscht, sobald der Benutzer den OMS-Arbeitsbereich in ihrem oder seinem Abonnement löscht. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Wie kommuniziert der Collector mit dem vCenter Server und dem Azure Migrate-Dienst?

Die Collectorappliance stellt mithilfe der vom Benutzer in der Appliance angegebenen Anmeldeinformationen eine Verbindung mit dem vCenter Server (Port 443) her. Anschließend erfolgt eine Abfrage von vCenter Server mithilfe der VMware PowerCLI, um Metadaten über die von vCenter Server verwalteten Metadaten zu sammeln. Es werden sowohl Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NIC usw.) als auch der Leistungsverlauf für jeden virtuellen Computer für den letzten Monat von vCenter Server gesammelt. Die gesammelte Metadaten werden dann zur Bewertung an den Azure Migrate-Dienst (über das Internet über https) gesendet. [Weitere Informationen](concepts-collector.md)

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Werden die Daten im Ruhezustand und bei der Übertragung verschlüsselt?

Ja, die gesammelten Daten werden sowohl im Ruhezustand als auch bei der Übertragung verschlüsselt. Die von der Appliance gesammelten Metadaten werden sicher über das Internet über https an den Azure Migrate-Dienst gesendet. Die gesammelte Metadaten werden in [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) und [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in einem Microsoft-Abonnement gespeichert und im Ruhezustand verschlüsselt.

Die von den Dependency-Agents gesammelten Daten werden auch bei der Übertragung verschlüsselt (sicherer https-Kanal) und im Log Analytics-Arbeitsbereich im Abonnement des Benutzers gespeichert. Außerdem erfolgt eine Verschlüsselung im Ruhezustand.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Wie kann ich eine Umgebung mit mehreren Mandanten in Azure Migrate ermitteln?

Wenn Sie bei einer von mehreren Mandanten gemeinsam genutzten Umgebung keine VMs eines Mandanten im Abonnement eines anderen ermitteln möchten, können Sie das Feld „Bereich“ in der Collectorappliance nutzen, um den Bereich der Ermittlung festzulegen. Sollten Mandanten Hosts gemeinsam nutzen, erstellen Sie Anmeldeinformationen, die nur auf diejenigen VMs einen schreibgeschützten Zugriff ermöglichen, die dem bestimmten Mandanten gehören. Verwenden Sie dann diese Anmeldeinformationen in der Collectorappliance und geben Sie den Bereich als Host für die Ermittlung an. Alternativ können Sie auch Ordner in der vCenter Server-Instanz erstellen – z.B. Ordner1 für Mandant1 und Ordner2 für Mandant2 – und unter dem gemeinsam genutzten Host die VMs für Mandant1 in Ordner1 und für Mandant2 in Ordner2 verschieben. Legen Sie anschließend im Collector den Bereich für die Ermittlungen fest, indem Sie den entsprechenden Ordner angeben.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Wie viele virtuelle Computer können in einem einzelnen Migrationsprojekt ermittelt werden?

In einem einzelnen Migrationsprojekt können 1500 virtuelle Computer ermittelt werden. Wenn Ihre lokale Umgebung mehr Computer enthält, finden Sie hier [weitere Informationen](how-to-scale-assessment.md) zur Ermittlung einer großen Umgebung in Azure Migrate.

## <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Ist die Nutzung des Features zur Visualisierung von Abhängigkeiten kostenpflichtig?

Azure Migrate ist ohne Aufpreis erhältlich. Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kann ich für die Visualisierung von Abhängigkeiten einen vorhandenen Arbeitsbereich verwenden?

Azure Migrate unterstützt nicht die Verwendung eines vorhandenen Arbeitsbereichs für die Visualisierung von Abhängigkeiten, jedoch unterstützt der Microsoft Monitoring Agent (MMA) Multi-Homing und ermöglicht es Ihnen, Daten an mehrere Arbeitsbereiche zu senden. Wenn Sie die Agents also bereits in einem Arbeitsbereich bereitgestellt und konfiguriert haben, können Sie Multi-Homing im MMA-Agent nutzen und ihn im Azure Migrate-Arbeitsbereich konfigurieren (zusätzlich zum bestehenden Arbeitsbereich) und ihn zum Laufen bringen. [Hier](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) ist ein Blog, in dem die Aktivierung von Multi-Homing in einem MMA-Agent erläutert wird.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht zu Azure Migrate](migrate-overview.md).
- Erfahren Sie, wie Sie eine VMware-Umgebung [ermitteln und bewerten](tutorial-assessment-vmware.md) können.
