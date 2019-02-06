---
title: Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: In diesem Artikel wird eine Kommunikationsübersicht für die verwaltete Azure SQL-Datenbank-Instanz bereitgestellt. Außerdem werden die Verbindungsarchitektur und die Übertragung des Datenverkehrs an die verwaltete Instanz mithilfe der unterschiedlichen Komponenten erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 12/10/2018
ms.openlocfilehash: b709bbacce23a89b8c60b77a524018b50ca1ca5e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245666"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz

In diesem Artikel wird eine Kommunikationsübersicht für die verwaltete Azure SQL-Datenbank-Instanz bereitgestellt. Außerdem werden die Verbindungsarchitektur und die Übertragung des Datenverkehrs an die verwaltete Instanz mithilfe der unterschiedlichen Komponenten erläutert.  

Die verwaltete Azure SQL-Datenbank-Instanz wird in Azure-VNET und dem dedizierten Subnetze für verwaltete Instanzen platziert. Diese Bereitstellung ermöglicht die folgenden Szenarien: 
- Sichern der privaten IP-Adresse
- Direktes Herstellen einer Verbindung mit einer verwalteten Instanz von einem lokalen Netzwerk
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und einem Verbindungsserver oder einem anderen lokalen Datenspeicher
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und Azure-Ressourcen

## <a name="communication-overview"></a>Kommunikationsübersicht

Im folgenden Diagramm sind Entitäten abgebildet, die eine Verbindung mit der verwalteten Instanz herstellen. Außerdem sind Ressourcen zu sehen, mit denen die verwaltete Instanz aus Funktionalitätsgründen kommunizieren muss.

![Entitäten der Verbindungsarchitektur](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Der Kommunikationsvorgang im unteren Diagrammbereich bezieht sich auf Kundenanwendungen und Tools, die eine Verbindung mit der verwalteten Instanz als Datenquelle herstellen.  

Die verwaltete Instanz ist ein PaaS-Angebot (Platform-as-a-Service). Dies bedeutet, dass Microsoft diesen Dienst mithilfe automatisierter Agents (zur Verwaltung, Bereitstellung und Wartung) auf der Grundlage von Telemetriedatenströmen verwaltet. Da die Verwaltung der verwalteten Instanz ausschließlich der Verantwortung von Microsoft unterliegt, können Kunden nicht auf virtuelle Clustercomputer der verwalteten Instanz über RDP zugreifen.

Bei einigen SQL Server-Vorgängen, die von Endbenutzern oder Anwendungen gestartet werden, muss die verwaltete Instanz möglicherweise mit der Plattform interagieren. Ein Beispiel hierfür ist die Erstellung einer Datenbank der verwalteten Instanz. Diese Ressource wird über das Portal, PowerShell und die Azure CLI verfügbar gemacht.

Die verwaltete Instanz ist nur im Zusammenspiel mit anderen Azure-Diensten (beispielsweise Azure Storage für Sicherungen, Azure Service Bus für Telemetriedaten, Azure AD für die Authentifizierung und Azure Key Vault für TDE) voll funktionsfähig und stellt Verbindungen mit diesen her.

Alle der oben genannten Übertragungen werden verschlüsselt und mit Zertifikaten signiert. Die verwaltete Instanz kontaktiert regelmäßig die Zertifizierungsstelle, um die Zertifikate zu überprüfen. Dadurch wird sichergestellt, dass die Kommunikationspartner als vertrauenswürdig eingestuft werden. Wenn die Zertifikate widerrufen werden oder die verwaltete Instanz sie nicht überprüfen kann, werden die Verbindungen zum Schutz der Daten unterbrochen.

## <a name="high-level-connectivity-architecture"></a>Verbindungsarchitektur auf Makroebene

Auf der Makroebene setzt sich die verwaltete Instanz aus Dienstkomponenten zusammen, die auf dedizierten isolierten VMs gehostet werden. Letztere werden innerhalb des Subnetzes des virtuellen Kundennetzwerks ausgeführt und bilden einen virtuellen Cluster.

In einem einzelnen virtuellen Cluster können mehrere verwaltete Instanzen gehostet werden. Der Cluster wird ggf. automatisch vergrößert oder verkleinert, wenn der Kunde die Anzahl der bereitgestellten Instanzen im Subnetz ändert.

Kundenanwendungen können nur dann eine Verbindung mit einer verwalteten Instanz herstellen und Datenbanken abfragen sowie aktualisieren, wenn die Anwendungen innerhalb des virtuellen Netzwerks, des mittels Peering verknüpften virtuellen Netzwerks oder des mittels VPN/ExpressRoute verbundenen Netzwerks ausgeführt werden und ein Endpunkt mit einer privaten IP-Adresse verwendet wird.  

![Diagramm zur Verbindungsarchitektur](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Verwaltungs- und Bereitstellungsdienste von Microsoft werden außerhalb des virtuellen Netzwerks ausgeführt. Die Verbindung zwischen einer verwalteten Instanz und Microsoft-Diensten wird daher über Endpunkte mit öffentlichen IP-Adressen hergestellt. Wenn die verwaltete Instanz eine ausgehende Verbindung aufbaut, wird auf der Empfängerseite aufgrund der Netzwerkadressenübersetzung (NAT) die öffentliche IP-Adresse angezeigt.

Der Verwaltungsdatenverkehr wird über das virtuelle Kundennetzwerk übertragen. Dies bedeutet, dass die Infrastrukturelemente des virtuellen Netzwerks den Verwaltungsdatenverkehr negativ beeinflussen können, wodurch die Instanz möglicherweise in einen fehlerhaften Zustand überführt wird und damit nicht mehr verfügbar ist.

> [!IMPORTANT]
> Microsoft nutzt zur Verbesserung der Dienstverfügbarkeit und -qualität Netzwerkzielrichtlinien für Infrastrukturelemente virtueller Azure-Netzwerke, die sich möglicherweise negativ auf die Funktionalität der verwalteten Instanz auswirken. Mit diesem Plattformmechanismus werden den Endbenutzern Netzwerkanforderungen transparent mitgeteilt. Hierbei besteht das Hauptziel darin, einer fehlerhaften Netzwerkkonfiguration vorzubeugen und den Normalbetrieb der verwalteten Instanz zu gewährleisten. Durch das Löschen der verwalteten Instanz wird auch die Netzwerkzielrichtlinie entfernt.

## <a name="virtual-cluster-connectivity-architecture"></a>Verbindungsarchitektur für virtuellen Cluster

Im Folgenden wird die Verbindungsarchitektur der verwalteten Instanz noch genauer betrachtet. Im unten abgebildeten Diagramm wird das konzeptionelle Layout des virtuellen Clusters dargestellt.

![Diagramm für Verbindungsarchitektur des virtuellen Clusters](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients stellen mithilfe des Hostnamens, der in der Form `<mi_name>.<dns_zone>.database.windows.net` vorliegen muss, eine Verbindung mit der verwalteten Instanz her. Dieser Hostname wird in die private IP-Adresse aufgelöst, obwohl er in der öffentlichen DNS-Zone registriert ist und öffentlich aufgelöst werden kann. Die `zone-id` wird beim Erstellen des Clusters automatisch generiert. Wenn in einem neu erstellten Cluster eine sekundäre verwaltete Instanz gehostet wird, verwendet diese die Zonen-ID gemeinsam mit dem primären Cluster. Weitere Informationen finden Sie unter [Autofailover-Gruppen](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Diese private IP-Adresse gehört zum internen Load Balancer der verwalteten Instanz, der den Datenverkehr an das Gateway (GW) der verwalteten Instanz weiterleitet. Da mehrere verwaltete Instanzen theoretisch in demselben Cluster ausgeführt werden könnten, nutzt das GW den Hostnamen der verwalteten Instanz, um den Datenverkehr an den richtigen SQL-Enginedienst weiterzuleiten.

Verwaltungs- und Bereitstellungsdienste stellen über [Verwaltungsendpunkte](#management-endpoint), die dem externen Lastenausgleich zugeordnet sind, eine Verbindung mit der verwalteten Instanz her. Der Datenverkehr wird nur an die Knoten weitergeleitet, wenn er an einer vordefinierten Gruppe von Ports empfangen wird, die ausschließlich von Verwaltungskomponenten der verwalteten Instanz verwendet werden. Die integrierte Firewall auf den Knoten ist so konfiguriert, dass nur Datenverkehr von Microsoft-spezifischen IP-Adressbereichen zulässig ist. Alle Übertragungen zwischen den Verwaltungskomponenten und der Verwaltungsebene werden gegenseitig mithilfe von Zertifikaten authentifiziert.

## <a name="management-endpoint"></a>Verwaltungsendpunkt

Der virtuelle Cluster der verwalteten Azure SQL-Datenbank-Instanz enthält einen Verwaltungsendpunkt, der von Microsoft zur Verwaltung der verwalteten Instanz verwendet wird. Der Verwaltungsendpunkt ist durch eine integrierte Firewall auf Netzwerkebene und eine gegenseitige Zertifikatüberprüfung auf Anwendungsebene geschützt. Sie können die [IP-Adresse des Verwaltungsendpunkts ermitteln](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Wenn Verbindungen aus der verwalteten Instanz heraus initiiert werden (Sicherung, Überwachungsprotokoll), stammt der Datenverkehr anscheinend von der öffentlichen IP-Adresse des Verwaltungsendpunkts. Sie können den Zugriff von der verwalteten Instanz auf öffentliche Dienste einschränken, indem Sie Firewallregeln festlegen, die nur die IP-Adresse der verwalteten Instanz zulassen. Erfahren Sie mehr über die Methode, mit der [die integrierte Firewall der verwalteten Instanz überprüft werden kann](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Dies gilt nicht für das Festlegen von Firewallregeln für Azure-Dienste, die sich in derselben Region wie die verwaltete Instanz befinden, da die Azure-Plattform über eine Optimierung für den Datenverkehr verfügt, der zwischen den verbundenen Diensten erfolgt.

## <a name="network-requirements"></a>Netzwerkanforderungen

Sie können die verwaltete Instanz in einem dedizierten Subnetz (dem Subnetz der verwalteten Instanz) innerhalb des virtuellen Netzwerks bereitstellen, das den folgenden Anforderungen entspricht:
- **Dediziertes Subnetz**: Das Subnetz der verwalteten Instanz darf mit keinem anderen Clouddienst verknüpft und kein Gatewaysubnetz sein. Sie können weder eine verwaltete Instanz in einem Subnetz erstellen, das andere Ressourcen als die verwaltete Instanz enthält, noch zu einem späteren Zeitpunkt Ressourcen im Subnetz hinzufügen.
- **Kompatible Netzwerksicherheitsgruppe (NSG)**: Eine NSG, die mit einem Subnetz einer verwalteten Instanz verbunden ist, muss Regeln in den folgenden Tabellen („Obligatorische Eingangssicherheitsregeln“ und „Obligatorische Ausgangssicherheitsregeln“) den Vorrang vor anderen Regeln geben. Mit NSGs können Sie den Zugriff auf den Datenendpunkt der verwalteten Instanz vollständig steuern, indem Sie den Datenverkehr über Port 1433 filtern. 
- **Kompatible benutzerdefinierte Routingtabelle**: Das Subnetz der verwalteten Instanz benötigt eine benutzerdefinierte Routingtabelle, der **0.0.0.0.0.0/0 mit dem nächsten Hop zum Internet** als obligatorische benutzerdefinierte Route zugewiesen ist. Darüber hinaus können Sie eine benutzerdefinierte Route hinzufügen, die Datenverkehr mit lokalen privaten IP-Bereichen als Ziel über ein virtuelles Netzwerkgateway oder ein virtuelles Netzwerkgerät leitet. 
- **Optionales benutzerdefiniertes DNS**: Wenn ein benutzerdefiniertes DNS im virtuellen Netzwerk angegeben ist, muss die IP-Adresse des rekursiven Azure-Resolvers (z.B. 168.63.129.16) der Liste hinzugefügt werden. Weitere Informationen finden Sie unter [Konfigurieren des benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md). Der benutzerdefinierte DNS-Server muss Hostnamen in den folgenden Domänen und deren Subdomänen auflösen können: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com* und *microsoftonline-p.com*. 
- **Keine Dienstendpunkte**: Dem Subnetz der verwalteten Instanz darf kein Dienstendpunkt zugeordnet sein. Überprüfen Sie, ob die Option „Dienstendpunkte“ beim Erstellen des virtuellen Netzwerks auf „Deaktiviert“ festgelegt ist.
- **Ausreichende IP-Adressen**: Das Subnetz der verwalteten Instanz muss mindestens 16 IP-Adressen aufweisen. Die empfohlene Mindestanzahl sind 32 IP-Adressen. Weitere Informationen finden Sie unter [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](sql-database-managed-instance-determine-size-vnet-subnet.md). Sie können verwaltete Instanzen im [vorhandenen Netzwerk](sql-database-managed-instance-configure-vnet-subnet.md) bereitstellen, nachdem Sie dieses entsprechend den [Netzwerkanforderungen für verwaltete Instanzen](#network-requirements) konfiguriert haben, oder ein [neues Netzwerk und Subnetz](sql-database-managed-instance-create-vnet-subnet.md) erstellen.

> [!IMPORTANT]
> Wenn das Zielsubnetz nicht alle genannten Anforderungen erfüllt, können Sie keine neue verwaltete Instanz bereitstellen. Wenn eine verwaltete Instanz erstellt wird, wird eine *Netzwerkabsichtsrichtlinie* auf das Subnetz angewendet, um nicht konforme Änderungen an der Netzwerkkonfiguration zu verhindern. Nachdem die letzte Instanz aus dem Subnetz entfernt wurde, wird auch die *Netzwerkabsichtsrichtlinie* entfernt.

### <a name="mandatory-inbound-security-rules"></a>Obligatorische Eingangssicherheitsregeln 

| NAME       |Port                        |Protokoll|Quelle           |Ziel|Aktion|
|------------|----------------------------|--------|-----------------|-----------|------|
|management  |9000, 9003, 1438, 1440, 1452|TCP     |Beliebig              |Beliebig        |ZULASSEN |
|mi_subnet   |Beliebig                         |Beliebig     |MI-SUBNETZ        |Beliebig        |ZULASSEN |
|health_probe|Beliebig                         |Beliebig     |AzureLoadBalancer|Beliebig        |ZULASSEN |

### <a name="mandatory-outbound-security-rules"></a>Obligatorische Ausgangssicherheitsregeln 

| NAME       |Port          |Protokoll|Quelle           |Ziel|Aktion|
|------------|--------------|--------|-----------------|-----------|------|
|management  |80, 443, 12000|TCP     |Beliebig              |Internet   |ZULASSEN |
|mi_subnet   |Beliebig           |Beliebig     |Beliebig              |MI-SUBNETZ  |ZULASSEN |

  > [!Note]
  > MI-SUBNETZ bezieht sich auf den IP-Adressbereich für das Subnetz in der Form 10.x.x.x/y. Diese Informationen finden Sie im Azure-Portal (über die Eigenschaften für das Subnetz).
  
  > [!Note]
  > Obwohl obligatorische Eingangssicherheitsregeln den Datenverkehr von _allen_ Quellen an den Ports 9000 9003, 1438, 1440 und 1452 zulassen, sind diese Ports durch eine integrierte Firewall geschützt. In [diesem Artikel](sql-database-managed-instance-find-management-endpoint-ip-address.md) wird gezeigt, wie Sie die IP-Adresse des Verwaltungsendpunkts ermitteln und Firewallregeln überprüfen können. 
  
  > [!Note]
  > Wenn Sie Transaktionsreplikation in der verwalteten Instanz verwenden und jede Datenbank in der verwalteten Instanz als Verleger oder Verteiler verwendet wird, muss Port 445 (TCP ausgehend) auch in den Sicherheitsregeln des Subnetzes geöffnet sein, um auf die Azure-Dateifreigabe zugreifen zu können.
  
## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter  [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md)
- Erfahren Sie, wie Sie [ein neues VNET konfigurieren](sql-database-managed-instance-create-vnet-subnet.md) oder [ein vorhandenes VNET konfigurieren](sql-database-managed-instance-configure-vnet-subnet.md), in dem Sie verwaltete Instanzen bereitstellen können.
- [Berechnen Sie die Größe des Subnetzes](sql-database-managed-instance-determine-size-vnet-subnet.md), in dem verwaltete Instanzen bereitgestellt werden sollen. 
- Informationen zum Erstellen einer verwaltete Instanz per Schnellstart finden Sie in den folgenden Artikeln. Zur Erstellung können
  - Im [Azure-Portal](sql-database-managed-instance-get-started.md)
  - Mit [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - [Azure Resource Manager-Vorlagen](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - oder [Azure Resource Manager-Vorlagen im Zusammenspiel mit einer Jumpbox, in die SSMS integriert ist](https://portal.azure.com/), verwendet werden.
