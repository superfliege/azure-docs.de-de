---
title: Azure Migrate – Häufig gestellte Fragen (FAQ) | Microsoft-Dokumentation
description: Beantwortet häufig gestellte Fragen zu Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: snehaa
ms.openlocfilehash: 3f035f38b1ad68e9e39d151ffad3fc650a0a1d80
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952748"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate – Häufig gestellte Fragen (FAQ)

Dieser Artikel enthält häufig gestellte Fragen zu Azure Migrate. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Allgemein

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>Unterstützt Azure Migrate nur die Bewertung von VMware-Workloads?

Ja, Azure Migrate unterstützt derzeit nur die Bewertung von VMware-Workloads. Eine Unterstützung von Hyper-V und physischen Servern wird zukünftig hinzugefügt.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Benötigt Azure Migrate vCenter Server zum Ermitteln einer VMware-Umgebung?

Ja, Azure Migrate benötigt vCenter Server zum Ermitteln einer VMware-Umgebung. Die Ermittlung von ESXi-Hosts, die nicht von einem vCenter Server verwaltet werden, wird nicht unterstützt.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Was unterscheidet Azure Migrate von Azure Site Recovery?

Azure Migrate ist ein Bewertungsdienst, mit dem Sie Ihre lokalen Workloads bestimmen und Ihre Migration zu Azure planen können. Mit [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), das zudem eine Notfallwiederherstellungslösung ist, können Sie lokale Workloads zu IaaS-VMs in Azure migrieren.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>Was ist der Unterschied zwischen der Verwendung von Azure Migrate für Bewertungen und dem MAP Toolkit?

[Azure Migrate](migrate-overview.md) bietet eine Migrationsbewertung speziell für die Unterstützung der Migrationsbereitschaft und die Auswertung lokaler Workloads in Azure. [Microsoft-Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) weist andere Funktionen auf. Beispiele sind die Migrationsplanung für neuere Versionen von Windows-Client- und -Server-Betriebssystemen, die Softwarenutzungs-Nachverfolgung usw. Verwenden Sie für diese Szenarien weiterhin das MAP Toolkit.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Was unterscheidet Azure Migrate von Azure Site Recovery-Bereitstellungsplaner?

Azure Migrate ist ein Tool für die Migrationsplanung und Azure Site Recovery-Bereitstellungsplaner ist ein Tool für die Notfallwiederherstellungsplanung.

**Migration von VMware zu Azure**: Wenn Sie beabsichtigen, Ihre lokalen Workloads zu Azure zu migrieren, verwenden Sie für die Migrationsplanung Azure Migrate. Azure Migrate bewertet lokale Workloads und bietet Anleitungen, Einblicke und Mechanismen, um Sie bei der Migration zu Azure zu unterstützen. Wenn Ihr Migrationsplan fertig ist, können Sie Dienste wie Azure Site Recovery und Azure Database Migration Service nutzen, um die Computer zu Azure zu migrieren.

**Migration von Hyper-V zu Azure**: Aktuell unterstützt Azure Migrate nur die Bewertung virtueller VMware-Computer für die Migration zu Azure. Die Unterstützung für Hyper-V ist Teil der Roadmap für Azure Migrate. In der Zwischenzeit können Sie den Site Recovery-Bereitstellungsplaner verwenden. Nach Aktivierung der Hyper-V-Unterstützung in Azure Migrate, können Sie Azure Migrate zum Planen der Migration von Hyper-V-Workloads verwenden.

**Notfallwiederherstellung von VMware/Hyper-V in Azure:** Wenn Sie beabsichtigen, eine Notfallwiederherstellung (DR) mit Azure Site Recovery zu Azure durchzuführen, verwenden Sie den Azure Site Recovery-Bereitstellungsplaner für die DR-Planung. Der Site Recovery-Bereitstellungsplaner führt eine umfassende, ASR-spezifische Bewertung Ihrer lokalen Umgebung durch. Er bietet Empfehlungen, die Site Recovery für erfolgreiche DR-Vorgänge benötigt, z.B. Replikation und Failover Ihrer virtuellen Computer.  

### <a name="which-azure-regions-are-supported-by-azure-migrate"></a>Welche Azure-Regionen werden von Azure Migrate unterstützt?

Azure Migrate unterstützt derzeit die Regionen „USA, Osten“ und „USA, Westen-Mitte“ als Migrationsprojektstandorte. Auch wenn Sie Migrationsprojekte nur in „USA, Osten“ und „USA, Westen-Mitte“ erstellen können, haben Sie dennoch die Möglichkeit, Ihre Computer für [mehrere Zielstandorte](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties) zu bewerten. Der Projektstandort wird nur verwendet, um die ermittelten Daten zu speichern.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Wie stellt der lokale Standort eine Verbindung mit Azure Migrate her?

Die Verbindung kann über das Internet erfolgen oder ExpressRoute mit öffentlichem Peering nutzen.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Kann ich das VM-Setup mit der .OVA-Vorlage absichern?

Zusätzliche Komponenten (z.B. Antivirensoftware) können in der .OVA-Vorlage hinzugefügt werden, solange die für die Funktion des Azure Migrate-Geräts erforderlichen Kommunikations- und Firewall-Regeln unverändert bleiben.   

## <a name="discovery-and-assessment"></a>Ermittlung und Bewertung

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
  - Datenträger-Schreibdurchsatz
  - Datenträgerlesevorgänge pro Sekunde
  - Datenträger-Schreibvorgänge pro Sekunde
- Für jeden Netzwerkadapter, der an den virtuellen Computer angefügt ist:
  - Eingehender Netzwerkverkehr
  - Ausgehender Netzwerkverkehr

Die Agent-basierte Ermittlung ist eine Option, die zusätzlicher zur appliancebasierten Ermittlung verfügbar ist, und Kunden beim [Visualisieren von Abhängigkeiten](how-to-create-group-machine-dependencies.md) der lokalen virtuellen Computer unterstützt. Die Dependency-Agents sammeln Details wie FQDN, BS, IP-Adresse, MAC-Adresse, Prozesse, die innerhalb des virtuellen Computers ausgeführt werden, und die eingehenden/ausgehenden TCP-Verbindungen vom virtuellen Computer. Die Agent-basierte Ermittlung ist optional, und Sie müssen die Agents nicht installieren, wenn Sie die Abhängigkeiten der VMs nicht visualisieren möchten.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Wo und wie lange werden die gesammelten Daten gespeichert?

Die von der Collectorappliance gesammelten Daten werden im Azure-Speicherort gespeichert, den Sie beim Erstellen des Migrationsprojekts angeben. Die Daten werden sicher in einem Microsoft-Abonnement gespeichert und gelöscht, wenn der Benutzer das Azure Migrate-Projekt löscht.

Wenn Sie für die Abhängigkeitsvisualisierung Agents auf den VMs installieren, werden die von den Dependency-Agents gesammelten Daten in den USA in einem OMS-Arbeitsbereich gespeichert, der im Abonnement des Benutzers erstellt wurde. Diese Daten werden gelöscht, wenn Sie den OMS-Arbeitsbereich in Ihrem Abonnement löschen. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Werden die Daten im Ruhezustand und bei der Übertragung verschlüsselt?

Ja, die gesammelten Daten werden sowohl im Ruhezustand als auch bei der Übertragung verschlüsselt. Die von der Appliance gesammelten Metadaten werden sicher über das Internet über https an den Azure Migrate-Dienst gesendet. Die gesammelte Metadaten werden in [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) und [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in einem Microsoft-Abonnement gespeichert und im Ruhezustand verschlüsselt.

Die von den Dependency-Agents gesammelten Daten werden auch bei der Übertragung verschlüsselt (sicherer https-Kanal) und im Log Analytics-Arbeitsbereich im Abonnement des Benutzers gespeichert. Außerdem erfolgt eine Verschlüsselung im Ruhezustand.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Wie kommuniziert der Collector mit dem vCenter Server und dem Azure Migrate-Dienst?

Die Collectorappliance stellt mithilfe der vom Benutzer in der Appliance angegebenen Anmeldeinformationen eine Verbindung mit dem vCenter Server (Port 443) her. Anschließend erfolgt eine Abfrage von vCenter Server mithilfe der VMware PowerCLI, um Metadaten über die von vCenter Server verwalteten Metadaten zu sammeln. Es werden sowohl Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NIC usw.) als auch der Leistungsverlauf für jeden virtuellen Computer für den letzten Monat von vCenter Server gesammelt. Die gesammelte Metadaten werden dann zur Bewertung an den Azure Migrate-Dienst (über das Internet über https) gesendet. [Weitere Informationen](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>Kann ich eine Collectorappliance mit mehreren vCenter-Servern verbinden?

Ja, eine einzelne Collectorappliance kann für die Ermittlung mehrerer vCenter-Server verwendet werden, jedoch nicht gleichzeitig. Sie müssen die Ermittlungen nacheinander durchführen.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Ist die von Site Recovery verwendete .OVA-Vorlage in die von Azure Migrate verwendete .OVA-Datei integriert?

Derzeit gibt es keine Integration. Die .OVA-Vorlage in Site Recovery dient zum Einrichten eines Site Recovery-Konfigurationsservers für die Replikation von VMware-VMs und physischen Servern. Die von Azure Migrate verwendete .OVA-Datei wird zur Ermittlung von VMware-VMs, die von einem vCenter-Server verwaltet werden, im Rahmen der Migrationsbewertung verwendet.

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>Ich habe meine Computergröße geändert. Kann ich die Bewertung erneut ausführen?

Wenn Sie die Einstellungen auf einer VM ändern, auf die Sie zugreifen möchten, lösen Sie erneut die Ermittlung mit der Collectorappliance aus. Verwenden Sie dazu in der Appliance die Option **Sammlung erneut starten**. Nachdem die Sammlung abgeschlossen ist, wählen Sie im Portal die Option **Neu berechnen** für die Bewertung aus, um die aktualisierten Bewertungsergebnisse zu erhalten.

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
