---
title: Ermitteln einer Firewall, die in eine verwaltete Azure SQL-Datenbank-Instanz integriert ist | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den integrierten Firewallschutz in einer verwalteten Azure SQL-Datenbank-Instanz überprüfen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: d7b781a2cfcf17285b105a104588fe1569dffe4b
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345067"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Überprüfen der integrierten Firewall einer verwalteten Instanz

Die [obligatorischen Eingangssicherheitsregeln](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) der verwalteten Instanz schreiben vor, dass die Verwaltung der Ports 9000, 9003, 1438, 1440 und 1452 für **alle Quellen** der Netzwerksicherheitsgruppe (NSG) offen ist, die die verwaltete Instanz schützt. Zwar sind diese Ports auf NSG-Ebene offen, auf der Netzwerkebene sind sie jedoch durch die integrierte Firewall geschützt.

## <a name="verify-firewall"></a>Überprüfen der Firewall

Um diese Ports zu überprüfen, verwenden Sie ein beliebiges Überprüfungssicherheitstool zum Testen der Ports. Der folgende Screenshot zeigt die Verwendung eines solchen Tools.

![Überprüfen der integrierten Firewall](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu verwalteten Instanzen und zur Konnektivität finden Sie unter [Azure SQL Database Managed Instance Connectivity Architecture (Konnektivitätsarchitektur der verwalteten Azure SQL-Datenbank-Instanz)](sql-database-managed-instance-connectivity-architecture.md).