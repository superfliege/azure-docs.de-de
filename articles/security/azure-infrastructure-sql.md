---
title: Sicherheitsfeatures für Azure SQL-Datenbank
description: Dieser Artikel enthält eine allgemeine Beschreibung darüber, wie Kundendaten in Azure durch Azure SQL-Datenbank geschützt werden.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: cca8febb004029b13b0df09a047da701c4528e8e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101643"
---
# <a name="microsoft-azure-sql-database-security-features"></a>Sicherheitsfeatures für Microsoft Azure SQL-Datenbank    
Microsoft Azure SQL-Datenbank stellt einen relationalen Datenbankdienst in Azure bereit. Zum Schutz von Kundendaten und zum Bereitstellen verlässlicher Sicherheitsfeatures, die Kunden von einem relationalen Datenbankdienst erwarten, verfügt SQL-Datenbank über eigene Sicherheitsfunktionen. Diese Funktionen bauen auf den Steuerelementen auf, die von Azure geerbt werden.

## <a name="security-capabilities"></a>Sicherheitsfunktionen

### <a name="usage-of-tabular-data-stream-tds-protocol"></a>Verwenden des TDS-Protokolls (Tabular Data Stream)
Microsoft Azure SQL-Datenbank unterstützt nur das TDS-Protokoll, wofür die Datenbank nur über den Standardport TCP/1433 verfügbar sein muss.

### <a name="microsoft-azure-sql-database-firewall"></a>Firewall von Microsoft Azure SQL-Datenbank
Microsoft Azure SQL-Datenbank enthält eine Firewallfunktionalität, die wie unten veranschaulicht standardmäßig jeglichen Zugriff auf den SQL-Datenbankserver verhindert, um Kundendaten zu schützen.

![Firewall von Azure SQL-Datenbank][1]

Die Gateway-Firewall stellt die Funktionalität zum Einschränken von Adressen bereit, wodurch Kunden präzise Kontrolle über die Reichweite zulässiger IP-Adressen gewährt wird. Die Firewall gewährt Zugriff basierend auf der Ursprungs-IP-Adresse der jeweiligen Anforderung.

Mithilfe eines Verwaltungsportals oder programmgesteuert mithilfe der Microsoft Azure SQL-Datenbank-Verwaltungs-REST-API kann die Firewall konfiguriert werden. Die Firewall des Microsoft Azure SQL-Datenbank-Gateways verweigert standardmäßig allen Kunden den TDS-Zugriff auf Microsoft Azure SQL-Datenbanken. Der Zugriff muss mit Zugriffssteuerungslisten konfiguriert werden, um Microsoft Azure SQL-Datenbankverbindungen über Quell- und Zieladressen, Protokolle und Portnummern zuzulassen.

### <a name="dosguard"></a>DoSGuard
DoS-Angriffe (Denial-of-Service) werden durch einen SQL-Datenbank-Gateway-Dienst namens DoSGuard reduziert. DoSGuard überwacht aktiv fehlgeschlagene Anmeldungen von IP-Adressen. Wenn mehrere fehlerhafte Anmeldungen von einer spezifischen IP-Adresse innerhalb eines bestimmten Zeitraums auftreten, wird der IP-Adresse der Zugriff auf alle Ressourcen im Dienst für einen vordefinierten Zeitraum blockiert.

Darüber hinaus führt Microsoft Azure SQL-Datenbank-Gateway auch Folgendes aus:

- Sichere Kanalfunktionsaushandlungen, um überprüfte verschlüsselte TDS FIPS 140-2-Verbindungen bei der Verbindung mit Datenbankservern zu implementieren.
- Überprüfung zustandsbehafteter TDS-Pakete, während Verbindungen von Clients akzeptiert werden. Das Gateway überprüft die Verbindungsinformationen und übergibt die TDS-Pakete an den entsprechenden physischen Server basierend auf dem Datenbanknamen, der in der Verbindungszeichenfolge angegeben wurde.

Das übergreifende Prinzip für die Netzwerksicherheit von Microsoft Azure SQL-Datenbank besteht darin, nur Verbindungen und Kommunikationen zu erlauben, die für den Dienst erforderlich sind. Alle anderen Ports, Protokolle und Verbindungen werden standardmäßig blockiert. VLANs und ACLs werden verwendet, um die Netzwerkkommunikation von Quell- und Zielnetzwerken, Protokollen und Portnummern einzuschränken.

Genehmigte Mechanismen zum Implementieren von netzwerkbasierten ACLs umfassen: ACLs auf Routern und Lastenausgleichmodule. Diese werden durch Regeln von Azure-Netzwerken, Gast-VM-Firewalls und Microsoft Azure SQL-Datenbank-Gateway-Firewalls verwaltet, die vom Kunden konfiguriert werden.

## <a name="data-segregation-and-customer-isolation"></a>Trennung von Daten und Isolierung von Kunden
Das Azure-Produktionsnetzwerk ist so strukturiert, dass öffentlich zugängliche Systemkomponenten von internen Ressourcen getrennt werden. Physische und logische Grenzen bestehen zwischen Webservern, die Zugriff auf das öffentliche Azure-Portal und die zugrunde liegende virtuelle Infrastruktur von Azure gewähren, in denen sich Anwendungsinstanzen von Kunden sowie Kundendaten befinden.

Alle öffentlichen zugänglichen Informationen werden innerhalb des Azure-Produktionsnetzwerks verwaltet. Das Produktionsnetzwerk unterliegt der zweistufigen Authentifizierung und Schutzmechanismen für Grenzen, verwendet die im vorherigen Abschnitt beschriebenen Firewall- und Sicherheitsfeatures sowie unten angegebene Funktionen zum Isolieren von Daten.

### <a name="unauthorized-systems-and-isolation-of-fc"></a>Nicht autorisierte Systeme und Isolation von FC
Da FC der zentrale Orchestrator von Microsoft Azure-Fabric ist, gibt es wichtige Steuerelemente zur Minderung von Bedrohungen, insbesondere durch potenziell gefährdete FAs in Kundenanwendungen. FC erkennt keine Hardware, dessen Geräteinformationen (z.B. die MAC-Adresse) nicht im FC vorgeladen werden. Die DHCP-Server auf dem FC über konfigurierte Listen von MAC-Adressen der Knoten verfügen, die sie starten möchten. Selbst wenn nicht autorisierte Systeme verbunden werden, werden Sie nicht in den Fabric-Bestand aufgenommen, weshalb sich nicht verbunden oder autorisiert sind, um mit einem anderen System im Fabric-Bestand zu kommunizieren. Damit wird das Risiko reduziert, dass nicht autorisierte Systeme mit dem FC kommunizieren und Zugriff auf das VLAN und auf Azure erhalten.

### <a name="vlan-isolation"></a>VLAN-Isolation
Das Azure-Produktionsnetzwerk ist in drei primäre VLAN-Netzwerke logisch getrennt:

- Haupt-VLAN: Dient zum Verbinden von nicht vertrauenswürdigen Kundenknoten.
- FC-VLAN: Enthält vertrauenswürdige FCs und unterstützende Systeme.
- Geräte-VLAN: Enthält vertrauenswürdige Netzwerkgeräte und andere Infrastrukturgeräte.

### <a name="packet-filtering"></a>Paketfilterung
Die IPFilter und Software-Firewalls, die im Stammbetriebssystem und im Gastbetriebssystem der Knoten implementiert sind, erzwingen Einschränkungen der Konnektivität und verhindern nicht autorisierten Datenverkehr zwischen virtuellen Computern.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hypervisor, Stammbetriebssystem und Gast-VMs
Die Isolation des Stammbetriebssystem von den Gast-VMs und der Gast-VMs voneinander wird durch Hypervisor und das Stammbetriebssystem verwaltet.

### <a name="types-of-rules-on-firewalls"></a>Regeltypen für Firewalls
Eine Regel wird wie folgt definiert:

{Security Response Center-IP (SRC), SRC-Port, Ziel-IP, Zielport, Zielprotokoll, Ein/Aus, Zustandsbehaftet/Zustandslos, zustandsbehaftetes Flow Timeout}.

SYN-Pakete können nur empfangen oder gesendet werden, wenn eine der Regeln dies zulässt. Microsoft Azure verwendet zustandslose Regeln für TCP, wobei das Prinzip darin besteht, dass nur alle nicht SYN-Pakete in einen oder aus einem virtuellen Computer zugelassen werden. Die Voraussetzung für dieses Sicherheitsmodell ist, dass ein beliebiger Host-Stack tolerant gegenüber der Missachtung eines SYN-Pakets kann, wenn zuvor kein SYN-Paket erkannt wurde. Das TCP-Protokoll selbst ist zustandsbehaftet und erreicht in Kombination mit der zustandslosen auf SYN basierenden Regel ein allgemeines Verhalten einer zustandsbehafteten Implementierung.

Microsoft Azure verwendet eine zustandsbehaftete Regel für das User Datagramm-Protokoll (UDP). Jedes Mal, wenn ein UDP-Paket mit einer Regel übereinstimmt, wird ein umgekehrter Flow in die andere Richtung erstellt. Dieser Flow enthält ein integriertes Zeitlimit.

Kunden sind für das Einrichten ihrer eigenen Firewalls zusätzlich zu dem verantwortlich, was Microsoft Azure bereitstellt. Hier können Kunden die Regeln für eingehenden und ausgehenden Datenverkehr definieren.

### <a name="production-configuration-management"></a>Konfigurationsverwaltung der Produktion
Sichere Standardkonfigurationen werden von den jeweiligen Betriebsteams in Azure und Microsoft Azure SQL-Datenbank verwaltet. Alle Änderungen an der Konfiguration der Produktionssysteme werden mithilfe eines zentralen Nachverfolgungssystems dokumentiert und überwacht. Änderungen an Software und Hardware werden über das zentrale Nachverfolgungssystem überwacht. Netzwerkänderungen im Zusammenhang mit ACL werden mithilfe der Zugriffssteuerungsliste nachverfolgt.

Alle Konfigurationsänderungen an Microsoft Azure werden in der Stagingumgebung entwickelt und getestet. Danach werden sie in der Produktionsumgebung bereitgestellt. Softwarebuilds werden im Rahmen der Tests überprüft. Überprüfungen der Sicherheit und des Datenschutzes werden als Teil einer Checkliste von Zugangskriterien überprüft. Änderungen werden vom jeweiligen Bereitstellungsteam in geplanten Abständen bereitgestellt. Releases werden vom Personal der Bereitstellungsteams überprüft und genehmigt, bevor sie für die Produktion bereitgestellt werden.

Änderungen werden für die erfolgreiche Durchführung überwacht. In einem fehlerhaften Szenario wird für die Änderung ein Rollback auf den vorherigen Zustand ausgeführt, oder ein Hotfix wird unter Genehmigung des zuständigen Personals für den Fehler bereitgestellt. Quelldepot, Git, TFS, MDS, Runner, Azure-Sicherheitsüberwachung, der FC und die WinFabric-Plattform werden verwendet, um die Konfigurationseinstellungen in der virtuellen Azure-Umgebung zentral zu verwalten, anzuwenden und zu überprüfen.

Auf ähnliche Weise verfügen Hardware- und Netzwerkänderungen über bewährte Validierungsschritte, um die Einhaltung der Buildanforderungen auszuwerten. Die Releases werden über ein koordiniertes CAB (Change Advisory Board) aus jeweiligen Gruppen überprüft und autorisiert.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Sicherung der Azure-Infrastruktur in den folgenden Artikeln:

- [Azure facilities, premises, and physical security (Azure-Einrichtungen, Gelände und physische Sicherheit)](azure-physical-security.md)
- [Availability of Azure infrastructure (Verfügbarkeit der Azure-Infrastruktur)](azure-infrastructure-availability.md)
- [Azure information system components and boundaries (Komponenten und Grenzen des Azure-Informationssystems)](azure-infrastructure-components.md)
- [Azure network architecture (Azure-Netzwerkarchitektur)](azure-infrastructure-network.md)
- [Azure production network (Azure-Produktionsnetzwerk)](azure-production-network.md)
- [Azure production operations and management (Produktionsvorgänge und Verwaltung von Azure)](azure-infrastructure-operations.md)
- [Monitoring of Azure infrastructure (Überwachen der Azure-Infrastruktur)](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
- [Protection of customer data in Azure (Schutz von Kundendaten in Azure)](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png
