---
title: Konnektivitätsarchitektur für eine verwaltete Instanz in Azure SQL-Datenbank | Microsoft-Dokumentation
description: Informationen über die Kommunikations- und Konnektivitätsarchitektur für verwaltete Azure SQL-Datenbank-Instanzen sowie die Übertragung des Datenverkehrs an die verwalteten Instanzen mithilfe der Komponenten.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: 801294241f399097d363dd8dc2682f158c0bf2cc
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358288"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Konnektivitätsarchitektur für eine verwaltete Instanz in Azure SQL-Datenbank

In diesem Artikel wird die Kommunikation in einer verwalteten Azure SQL-Datenbank-Instanz erläutert. Darüber hinaus wird die Konnektivitätsarchitektur beschrieben, und wie die Komponenten Datenverkehr an die verwaltete Instanz leiten.  

Die verwaltete SQL-Datenbank-Instanz wird im virtuellen Azure-Netzwerk und dem für verwaltete Instanzen dedizierten Subnetz platziert. Diese Bereitstellung bietet:

- Eine sichere private IP-Adresse.
- Die Möglichkeit, eine Verbindung eines lokalen Netzwerks mit einer verwalteten Instanz herzustellen.
- Die Möglichkeit, eine Verbindung einer verwalteten Instanz mit einem Verbindungsserver oder einem anderen lokalen Datenspeicher herzustellen.
- Die Möglichkeit, eine Verbindung einer verwalteten Instanz mit Azure-Ressourcen herzustellen.

## <a name="communication-overview"></a>Kommunikationsübersicht

Das folgende Diagramm zeigt Entitäten, die eine Verbindung mit einer verwalteten Instanz herstellen. Es zeigt auch die Ressourcen, die mit der verwalteten Instanz kommunizieren müssen. Der Kommunikationsvorgang im unteren Diagrammbereich bezieht sich auf Kundenanwendungen und Tools, die eine Verbindung mit der verwalteten Instanz als Datenquelle herstellen.  

![Entitäten in der Konnektivitätsarchitektur](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Eine verwaltete Instanz ist ein „Platform-as-a-Service“-Angebot (PaaS). Microsoft verwendet automatisierte Agenten (Verwaltung, Bereitstellung und Wartung) zum Verwalten dieses Diensts basierend auf Telemetriedatenströmen. Da Microsoft für die Verwaltung zuständig ist, können Kunden nicht über Remote Desktop Protocol (RDP) auf die virtuellen Clustercomputer der verwalteten Instanz zugreifen.

Bei einigen SQL Server-Vorgängen, die von Endbenutzern oder Anwendungen gestartet werden, müssen verwaltete Instanzen möglicherweise mit der Plattform interagieren. Ein Fall ist die Erstellung einer Datenbank für die verwaltete Instanz. Diese Ressource wird über das Azure-Portal, PowerShell, Azure-Befehlszeilenschnittstelle und REST-API verfügbar gemacht.

Verwaltete Instanzen hängen von Azure-Diensten wie Azure Storage für Sicherungen, Azure Service Bus für Telemetriedaten, Azure Active Directory für Authentifizierung und Azure Key Vault für Transparent Data Encryption (TDE) ab. Die verwalteten Instanzen stellen Verbindungen mit diesen Diensten her.

Bei der gesamten Kommunikation verwenden Zertifikate für Verschlüsselung und Signatur verwendet. Um die Vertrauenswürdigkeit der Kommunikationspartner sicherzustellen, überprüfen verwaltete Instanzen diese Zertifikate regelmäßig durch Kontaktieren einer Zertifizierungsstelle. Wenn die Zertifikate widerrufen werden oder nicht überprüft werden können, schließt die verwaltete Instanz die Verbindungen zum Schutz der Daten.

## <a name="high-level-connectivity-architecture"></a>Verbindungsarchitektur auf Makroebene

Auf hoher Ebene ist eine verwaltete Instanz eine Gruppe von Dienstkomponenten. Diese Komponenten werden auf einer dedizierten Gruppe isolierter virtueller Computer gehostet, die im virtuellen Subnetz des Kunden ausgeführt werden. Diese Computer bilden einen virtuellen Cluster.

Ein virtueller Cluster kann mehrere verwaltete Instanzen hosten. Der Cluster wird ggf. automatisch vergrößert oder verkleinert, wenn der Kunde die Anzahl der bereitgestellten Instanzen im Subnetz ändert.

Kundenanwendungen können nur dann eine Verbindung mit verwalteten Instanzen herstellen und Datenbanken abfragen sowie aktualisieren, wenn die Anwendungen innerhalb des virtuellen Netzwerks, des mittels Peering verknüpften virtuellen Netzwerks oder des durch VPN oder Azure ExpressRoute verbundenen Netzwerks ausgeführt werden. Dieses Netzwerk muss einen Endpunkt und eine private IP-Adresse verwenden.  

![Diagramm zur Konnektivitätsarchitektur](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Die Verwaltungs- und Bereitstellungsdienste von Microsoft werden außerhalb des virtuellen Netzwerks ausgeführt. Eine verwaltete Instanz und Microsoft-Dienste sind über die Endpunkte verbunden, die öffentliche IP-Adressen haben. Wenn eine verwaltete Instanz eine ausgehende Verbindung herstellt, sieht die Verbindung auf der Empfängerseite aufgrund der Netzwerkadressenübersetzung (Network Address Translation, NAT) so aus, käme sie von dieser öffentlichen IP-Adresse.

Der Verwaltungsdatenverkehr wird über das virtuelle Kundennetzwerk übertragen. Das bedeutet, dass die Elemente der Infrastruktur des virtuellen Netzwerks den Verwaltungsdatenverkehr beeinträchtigen können, indem sie einen Fehler bei der Instanz verursachen, sodass sie nicht mehr verfügbar ist.

> [!IMPORTANT]
> Microsoft nutzt zur Verbesserung der Dienstverfügbarkeit und -qualität Netzwerkzielrichtlinien für Infrastrukturelemente virtueller Azure-Netzwerke. Die Richtlinie kann Auswirkungen auf die Funktionsweise der verwalteten Instanz haben. Dieser Plattformmechanismus kommuniziert transparent Netzwerkanforderungen für Benutzer. Das Hauptziel der Richtlinie ist, Fehlkonfigurationen des Netzwerks zu verhindern und den normalen Betrieb der verwalteten Instanz zu gewährleisten. Wenn Sie eine verwaltete Instanz löschen, wird die Netzwerkzielrichtlinie ebenfalls entfernt.

## <a name="virtual-cluster-connectivity-architecture"></a>Verbindungsarchitektur für virtuellen Cluster

Im Folgenden wird die Konnektivitätsarchitektur für verwaltete Instanzen noch genauer betrachtet. Im unten abgebildeten Diagramm wird das konzeptionelle Layout des virtuellen Clusters dargestellt.

![Konnektivitätsarchitektur des virtuellen Clusters](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients stellen mithilfe des Hostnamens, der in der Form `<mi_name>.<dns_zone>.database.windows.net` vorliegen muss, eine Verbindung mit der verwalteten Instanz her. Dieser Hostname wird in eine private IP-Adresse aufgelöst, obwohl er in einer öffentlichen DNS-Zone (Domain Name System) registriert ist und öffentlich aufgelöst werden kann. Die `zone-id` wird beim Erstellen des Clusters automatisch generiert. Wenn in einem neu erstellten Cluster eine sekundäre verwaltete Instanz gehostet wird, verwendet diese die Zonen-ID gemeinsam mit dem primären Cluster. Weitere Informationen finden Sie unter [Verwenden von Autofailover-Gruppen für ein transparentes und koordiniertes Failover mehrerer Datenbanken](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Diese private IP-Adresse gehört zum internen Lastenausgleich der verwalteten Instanz. Der Lastenausgleich leitet Datenverkehr an das Gateway der verwalteten Instanz weiter. Da mehrere verwaltete Instanzen in demselben Cluster ausgeführt werden können, nutzt das Gateway den Hostnamen der verwalteten Instanz, um den Datenverkehr an den richtigen SQL-Enginedienst weiterzuleiten.

Verwaltungs- und Bereitstellungsdienste stellen über einen [Verwaltungsendpunkt](#management-endpoint), der einem externen Lastenausgleich zugeordnet ist, eine Verbindung mit einer verwalteten Instanz her. Der Datenverkehr wird nur an die Knoten weitergeleitet, wenn er an einer vordefinierten Gruppe von Ports empfangen wird, die ausschließlich die Verwaltungskomponenten der verwalteten Instanz verwenden. Eine integrierte Firewall auf den Knoten ist so eingerichtet, dass nur Datenverkehr von Microsoft-IP-Adressbereichen zulässig ist. Zertifikate authentifizieren gegenseitig die gesamte Kommunikation zwischen den Verwaltungskomponenten und der Verwaltungsebene.

## <a name="management-endpoint"></a>Verwaltungsendpunkt

Microsoft verwaltet die verwaltete Instanz mit einem Endpunkt für die Verwaltung. Dieser Endpunkt befindet sich im virtuellen Cluster der Instanz. Der Endpunkt für die Verwaltung wird durch eine integrierte Firewall auf Netzwerkebene geschützt. Auf der Anwendungsebene wird er durch gegenseitige Zertifikatüberprüfung geschützt. Die IP-Adresse des Endpunkts finden Sie unter [Ermitteln der IP-Adresse des Verwaltungsendpunkts](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Wenn Verbindungen in der verwalteten Instanz starten (wie bei Sicherungen und Überwachungsprotokollen), scheint der Datenverkehr bei der öffentlichen IP-Adresse des Verwaltungsendpunkts zu beginnen. Sie können den Zugriff von einer verwalteten Instanz auf öffentliche Dienste einschränken, indem Sie Firewallregeln festlegen, die nur die IP-Adresse der verwalteten Instanz zulassen. Weitere Informationen finden Sie unter [Überprüfen der integrierten Firewall einer verwalteten Instanz](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Im Gegensatz zu der Firewall für Verbindungen, die in der verwalteten Instanz beginnen, haben die Azure-Dienste, die sich in der Region der verwalteten Instanz befinden, eine Firewall, die für den Datenverkehr zwischen diesen Diensten optimiert ist.

## <a name="network-requirements"></a>Netzwerkanforderungen

Stellen Sie eine verwaltete Instanz in einem dedizierten Subnetz im virtuellen Netzwerk bereit. Das Subnetz muss diese Merkmale aufweisen:

- **Dediziertes Subnetz**: Das Subnetz der verwalteten Instanz darf mit keinem anderen Clouddienst verknüpft und kein Gatewaysubnetz sein. Das Subnetz darf keine Ressourcen außer der verwalteten Instanz enthalten, und Sie können später keine Ressourcen im Subnetz hinzufügen.
- **Netzwerksicherheitsgruppe (NSG)**: Eine NSG, die mit dem virtuellen Netzwerk verknüpft ist, muss [Eingangssicherheitsregeln](#mandatory-inbound-security-rules) und [Ausgangssicherheitsregeln](#mandatory-outbound-security-rules) vor allen anderen Regeln definieren. Mit NSGs können Sie den Zugriff auf den Datenendpunkt der verwalteten Instanz steuern, indem Sie den Datenverkehr über Port 1433 filtern.
- **Benutzerdefinierte Routingtabelle (User Defined Route, UDR):** Eine UDR-Tabelle, die mit dem virtuellen Netzwerk verknüpft ist, muss bestimmte [Einträge](#user-defined-routes) enthalten.
- **Keine Dienstendpunkte**: Dem Subnetz der verwalteten Instanz sollte kein Dienstendpunkt zugeordnet werden. Wenn Sie das virtuelle Netzwerk erstellen, überprüfen Sie, ob die Option „Dienstendpunkte“ auf „Deaktiviert“ festgelegt ist.
- **Ausreichende IP-Adressen**: Das Subnetz der verwalteten Instanz muss mindestens 16 IP-Adressen haben. Der empfohlene Mindestwert sind 32 IP-Adressen. Weitere Informationen finden Sie unter [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](sql-database-managed-instance-determine-size-vnet-subnet.md). Sie können verwaltete Instanzen im [vorhandenen Netzwerk](sql-database-managed-instance-configure-vnet-subnet.md) bereitstellen, nachdem Sie dieses entsprechend den [Netzwerkanforderungen für verwaltete Instanzen](#network-requirements) konfiguriert haben. Erstellen Sie andernfalls ein [neues Netzwerk und Subnetz](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Sie können keine neue verwaltete Instanz bereitstellen, wenn das Zielsubnetz nicht über diese Merkmale verfügt. Wenn Sie eine verwaltete Instanz erstellen, wird eine Netzwerkzielrichtlinie auf das Subnetz angewendet, um nicht konforme Änderungen am Netzwerksetup zu verhindern. Nachdem die letzte Instanz aus dem Subnetz entfernt wurde, wird auch die Netzwerkzielrichtlinie entfernt.

### <a name="mandatory-inbound-security-rules"></a>Obligatorische Eingangssicherheitsregeln

| NAME       |Port                        |Protokoll|Quelle           |Ziel|Aktion|
|------------|----------------------------|--------|-----------------|-----------|------|
|management  |9000, 9003, 1438, 1440, 1452|TCP     |Beliebig              |Beliebig        |ZULASSEN |
|mi_subnet   |Beliebig                         |Beliebig     |MI-SUBNETZ        |Beliebig        |ZULASSEN |
|health_probe|Beliebig                         |Beliebig     |AzureLoadBalancer|Beliebig        |ZULASSEN |

### <a name="mandatory-outbound-security-rules"></a>Obligatorische Ausgangssicherheitsregeln

| NAME       |Port          |Protokoll|Quelle           |Ziel|Aktion|
|------------|--------------|--------|-----------------|-----------|------|
|management  |80, 443, 12000|TCP     |Beliebig              |AzureCloud  |ZULASSEN |
|mi_subnet   |Beliebig           |Beliebig     |Beliebig              |MI-SUBNETZ*  |ZULASSEN |

> [!IMPORTANT]
> Stellen Sie sicher, dass es nur eine Regel für eingehenden Datenverkehr für die Ports 9000, 9003, 1438, 1440, 1452 und eine Regel für ausgehenden Datenverkehr für die Ports 80, 443, 12000 gibt. Die Bereitstellung von verwalteten Instanzen über ARM-Bereitstellungen schlägt fehl, wenn Regeln für eingehenden und ausgehenden Datenverkehr für jeden Port separat konfiguriert werden. Wenn für diese Ports separate Regeln gelten, schlägt die Bereitstellung mit folgendem Fehlercode fehl: `VnetSubnetConflictWithIntendedPolicy`

\* MI-SUBNETZ bezieht sich auf den IP-Adressbereich für das Subnetz in der Form 10.x.x.x/y. Diese Informationen finden Sie im Azure-Portal in den Subnetzeigenschaften.

> [!IMPORTANT]
> Obwohl die erforderlichen Eingangssicherheitsregeln den Datenverkehr von _allen_ Quellen an den Ports 9000, 9003, 1438, 1440 und 1452 zulassen, sind diese Ports durch eine integrierte Firewall geschützt. Weitere Informationen finden Sie unter [Ermitteln der IP-Adresse des Verwaltungsendpunkts](sql-database-managed-instance-find-management-endpoint-ip-address.md).
> [!NOTE]
> Wenn Sie die Transaktionsreplikation in einer verwalteten Instanz verwenden und Sie eine Instanzdatenbank als Herausgeber oder Verteiler einsetzen, öffnen Sie Port 445 (TCP ausgehend) in den Sicherheitsregeln des Subnetzes. Dieser Port ermöglicht den Zugriff auf die Azure-Dateifreigabe.

### <a name="user-defined-routes"></a>Benutzerdefinierte Routen

|NAME|Adresspräfix|Nächster Hop|
|----|--------------|-------|
|subnet_to_vnetlocal|[mi_subnet]|Virtuelles Netzwerk|
|mi-0-5-next-hop-internet|0.0.0.0/5|Internet|
|mi-11-8-nexthop-internet|11.0.0.0/8|Internet|
|mi-12-6-nexthop-internet|12.0.0.0/6|Internet|
|mi-128-3-nexthop-internet|128.0.0.0/3|Internet|
|mi-16-4-nexthop-internet|16.0.0.0/4|Internet|
|mi-160-5-nexthop-internet|160.0.0.0/5|Internet|
|mi-168-6-nexthop-internet|168.0.0.0/6|Internet|
|mi-172-12-nexthop-internet|172.0.0.0/12|Internet|
|mi-172-128-9-nexthop-internet|172.128.0.0/9|Internet|
|mi-172-32-11-nexthop-internet|172.32.0.0/11|Internet|
|mi-172-64-10-nexthop-internet|172.64.0.0/10|Internet|
|mi-173-8-nexthop-internet|173.0.0.0/8|Internet|
|mi-174-7-nexthop-internet|174.0.0.0/7|Internet|
|mi-176-4-nexthop-internet|176.0.0.0/4|Internet|
|mi-192-128-11-nexthop-internet|192.128.0.0/11|Internet|
|mi-192-160-13-nexthop-internet|192.160.0.0/13|Internet|
|mi-192-169-16-nexthop-internet|192.169.0.0/16|Internet|
|mi-192-170-15-nexthop-internet|192.170.0.0/15|Internet|
|mi-192-172-14-nexthop-internet|192.172.0.0/14|Internet|
|mi-192-176-12-nexthop-internet|192.176.0.0/12|Internet|
|mi-192-192-10-nexthop-internet|192.192.0.0/10|Internet|
|mi-192-9-nexthop-internet|192.0.0.0/9|Internet|
|mi-193-8-nexthop-internet|193.0.0.0/8|Internet|
|mi-194-7-nexthop-internet|194.0.0.0/7|Internet|
|mi-196-6-nexthop-internet|196.0.0.0/6|Internet|
|mi-200-5-nexthop-internet|200.0.0.0/5|Internet|
|mi-208-4-nexthop-internet|208.0.0.0/4|Internet|
|mi-224-3-nexthop-internet|224.0.0.0/3|Internet|
|mi-32-3-nexthop-internet|32.0.0.0/3|Internet|
|mi-64-2-nexthop-internet|64.0.0.0/2|Internet|
|mi-8-7-nexthop-internet|8.0.0.0/7|Internet|
||||

Darüber hinaus können Sie der Routingtabelle Einträge hinzufügen, um Datenverkehr mit lokalen privaten IP-Bereichen als Ziel über ein virtuelles Netzwerkgateway oder ein virtuelles Netzwerkgerät (Network Appliance, NVA) zu leiten.

Wenn das virtuelle Netzwerk ein benutzerdefiniertes DNS enthält, fügen Sie einen Eintrag für die IP-Adresse des rekursiven Azure-Resolvers (z.B. 168.63.129.16) hinzu. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten DNS für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-custom-dns.md). Der benutzerdefinierte DNS-Server muss Hostnamen in diesen Domänen und deren Subdomänen auflösen können: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com* und *microsoftonline-p.com*.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter  [Verwenden der Advanced Data Security einer Azure SQL-Datenbank mit virtuellen Netzwerken und nahezu 100%iger Kompatibilität](sql-database-managed-instance.md).
- Erfahren Sie, wie Sie [ein neues virtuelles Azure-Network](sql-database-managed-instance-create-vnet-subnet.md) oder [vorhandenes virtuelles Azure-Network](sql-database-managed-instance-configure-vnet-subnet.md) einrichten, wo Sie verwaltete Instanzen bereitstellen können.
- [Berechnen Sie die Größe des Subnetzes](sql-database-managed-instance-determine-size-vnet-subnet.md), in dem Sie verwaltete Instanzen bereitstellen möchten.
- Erfahren Sie, wie Sie eine verwaltete Instanz erstellen:
  - Im [Azure-Portal](sql-database-managed-instance-get-started.md).
  - Mit [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md).
  - Mit [einer Azure Resource Manager-Vorlage](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).
  - Mit [einer Azure Resource Manager-Vorlage (mit JumpBox, in die SSMS integriert ist)](https://portal.azure.com/).
