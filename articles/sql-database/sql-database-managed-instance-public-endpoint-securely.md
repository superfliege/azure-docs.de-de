---
title: Schützen der öffentlichen Endpunkte verwalteter Instanzen – verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Sicheres Verwenden öffentlicher Endpunkte in Azure mit einer verwalteten Instanz
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 9d5a3d18e8a7d3c5a6cb08e16e74dd4fbda9ca31
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012928"
---
# <a name="using-azure-sql-database-managed-instance-securely-with-public-endpoint"></a>Sicheres Verwenden einer verwalteten Azure SQL-Datenbank-Instanz mit öffentlichem Endpunkt

Eine verwaltete Azure SQL-Datenbank-Instanz kann aktiviert werden, um Benutzerkonnektivität über einen [öffentlichen Endpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) zu ermöglichen. Dieser Artikel enthält Anleitungen dazu, wie Sie diese Konfiguration noch sicherer machen.

## <a name="scenarios"></a>Szenarien

Eine verwaltete Instanz stellt einen privaten Endpunkt für Konnektivität aus einem virtuellen Netzwerk bereit. Die Standardoption ist das Bereitstellen maximaler Isolation. Es gibt jedoch Szenarien, in denen eine Verbindung über einen öffentlichen Endpunkt erforderlich ist:

- Integration mit ausschließlich mehrinstanzenfähigen PaaS-Angeboten
- Höherer Durchsatz beim Datenaustausch gegenüber einem VPN
- Verbot von PaaS innerhalb der Unternehmensnetzwerke durch Unternehmensrichtlinien

## <a name="deploying-managed-instance-for-public-endpoint-access"></a>Bereitstellen einer verwalteten Instanz für den Zugriff über einen öffentlichen Endpunkt

Auch wenn dies nicht unbedingt erforderlich ist, werden verwaltete Instanzen mit Zugriff auf einen öffentlichen Endpunkt meist in einem dedizierten isolierten virtuellen Netzwerk erstellt. In dieser Konfiguration wird das virtuelle Netzwerk nur für die Isolation der virtuellen Cluster verwendet. Dabei spielt es keine Rolle, ob sich der IP-Adressraum der verwalteten Instanz mit einem IP-Adressraum des Unternehmensnetzwerks überschneidet.

## <a name="securing-data-in-motion"></a>Schützen von Daten während der Übertragung

Der Datenverkehr verwalteter Instanzen wird immer verschlüsselt, wenn der Clienttreiber Verschlüsselung unterstützt. Die Daten verlassen zwischen der verwalteten Instanz und anderen virtuellen Azure-Computern oder Azure-Diensten nie das Azure-Backbone. Bei einer Verbindung zwischen einer verwalteten Instanz mit einem lokalen Netzwerk wird empfohlen, ExpressRoute mit Microsoft-Peering zu verwenden. Mit ExpressRoute wird die Übertragung von Daten über das öffentliche Internet vermieden (für private Verbindungen verwalteter Instanzen kann nur privates Peering verwendet werden).

## <a name="locking-down-inbound-and-outbound-connectivity"></a>Sperren ein- und ausgehender Verbindungen

Das folgende Diagramm zeigt die empfohlenen Sicherheitskonfigurationen.

![managed-instance-vnet.png](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Eine verwaltete Instanz verfügt über eine [dedizierte öffentliche Endpunktadresse](sql-database-managed-instance-find-management-endpoint-ip-address.md). Diese IP-Adresse sollte clientseitig in den ausgehenden Firewallregeln und den Regeln der Netzwerksicherheitsgruppen festgelegt werden, um ausgehende Verbindungen zu beschränken.

Um sicherzustellen, dass Datenverkehr an die verwaltete Instanz von vertrauenswürdigen Quellen stammt, wird empfohlen, nur Verbindungen mit Datenquellen mit bekannten IP-Adressen herzustellen. Beschränken Sie den Zugriff auf den öffentlichen Endpunkt der verwalteten Instanz über Port 3342 mithilfe einer Netzwerksicherheitsgruppe.

Wenn Clients eine Verbindung aus einem lokalen Netzwerk herstellen müssen, sollte die ursprüngliche Adresse in einen bekannten Satz von IP-Adressen übersetzt werden. Wenn dies nicht möglich ist (ein typisches Beispiel sind mobile Mitarbeiter), wird empfohlen, [Point-to-Site-VPN-Verbindungen und einen privaten Endpunkt](sql-database-managed-instance-configure-p2s.md) zu verwenden.

Wenn die Verbindungen von Azure aus gestartet werden, sollte der Datenverkehr von bekannten zugewiesenen [virtuellen IPs](../virtual-network/virtual-networks-reserved-public-ip.md) (z. B. virtuelle Computer) stammen. Zur Vereinfachung der Verwaltung von virtuellen IP-Adressen können Kunden ein [Präfix für öffentliche IP-Adressen](../virtual-network/public-ip-address-prefix.md) verwenden.