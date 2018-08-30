---
title: Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: In diesem Artikel wird eine Kommunikationsübersicht für die verwaltete Azure SQL-Datenbank-Instanz bereitgestellt. Außerdem werden die Verbindungsarchitektur und die Übertragung des Datenverkehrs an die verwaltete Instanz mithilfe der unterschiedlichen Komponenten erläutert.
keywords: ''
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.subservice: managed instance
ms.custom: ''
ms.date: 08/16/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.topic: conceptual
ms.openlocfilehash: 54917c6548c7f0bfacad6408732c5619e6346683
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "40177505"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz 

In diesem Artikel wird eine Kommunikationsübersicht für die verwaltete Azure SQL-Datenbank-Instanz bereitgestellt. Außerdem werden die Verbindungsarchitektur und die Übertragung des Datenverkehrs an die verwaltete Instanz mithilfe der unterschiedlichen Komponenten erläutert.  

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

Clients stellen mithilfe des Hostnamens, der in der Form <mi_name>.<clusterid>.database.windows.net. vorliegen muss, eine Verbindung mit der verwalteten Instanz her. Dieser Hostname wird in die private IP-Adresse aufgelöst, obwohl er in der öffentlichen DNS-Zone registriert ist und öffentlich aufgelöst werden kann. 

Diese private IP-Adresse gehört zum internen Load Balancer der verwalteten Instanz, der den Datenverkehr an das Gateway (GW) der verwalteten Instanz weiterleitet. Da mehrere verwaltete Instanzen theoretisch in demselben Cluster ausgeführt werden könnten, nutzt das GW den Hostnamen der verwalteten Instanz, um den Datenverkehr an den richtigen SQL-Enginedienst weiterzuleiten. 

Verwaltungs- und Bereitstellungsdienste stellen über öffentliche Endpunkte, die dem externen Lastenausgleich zugeordnet sind, eine Verbindung mit der verwalteten Instanz her. Der Datenverkehr wird nur an die Knoten weitergeleitet, wenn er an Ports empfangen wird, die ausschließlich von Verwaltungskomponenten der verwalteten Instanz verwendet werden. Alle Übertragungen zwischen den Verwaltungskomponenten und der Verwaltungsebene werden gegenseitig mithilfe von Zertifikaten authentifiziert. 

## <a name="next-steps"></a>Nächste Schritte 

- Eine Übersicht finden Sie unter  [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md) 
- Weitere Informationen zur VNet-Konfiguration finden Sie unter  [Konfigurieren eines VNet für eine verwaltete Instanz](sql-database-managed-instance-vnet-configuration.md). 
- Informationen zum Erstellen einer verwaltete Instanz per Schnellstart finden Sie in den folgenden Artikeln. Zur Erstellung können 
  - das [Azure-Portal](sql-database-managed-instance-create-tutorial-portal.md), 
  - [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/), 
  - [Azure Resource Manager-Vorlagen](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/) 
  - oder [Azure Resource Manager-Vorlagen im Zusammenspiel mit einer Jumpbox, in die SSMS integriert ist](https://portal.azure.com/), verwendet werden. 

