---
title: Ermitteln eines Verwaltungsendpunkts für eine verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die öffentliche IP-Adresse eines Verwaltungsendpunkts für eine verwaltete Azure SQL-Datenbank-Instanz abrufen und den integrierten Firewallschutz überprüfen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 542db0200229b4fea4ac67ad19c468f2b9dc67d1
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57772778"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Ermitteln der IP-Adresse des Verwaltungsendpunkts

Der virtuelle Cluster der verwalteten Azure SQL-Datenbank-Instanz enthält einen Verwaltungsendpunkt, der von Microsoft für Verwaltungsvorgänge verwendet wird. Der Verwaltungsendpunkt ist durch eine integrierte Firewall auf Netzwerkebene und eine gegenseitige Zertifikatüberprüfung auf Anwendungsebene geschützt. Sie können die IP-Adresse des Verwaltungsendpunkts ermitteln, aber Sie können nicht auf diesen Endpunkt zugreifen.

## <a name="determine-ip-address"></a>Ermitteln der IP-Adresse

Angenommen, der Host der verwalteten Instanz lautet `mi-demo.xxxxxx.database.windows.net`. Führen Sie `nslookup` aus, indem Sie den Hostnamen verwenden.

![Auflösen des internen Hostnamens](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Führen Sie nun einen weiteren `nslookup`-Vorgang für den hervorgehobenen Namen durch, indem Sie das Segment `.vnet.` entfernen. Nach der Ausführung dieses Befehls erhalten Sie die öffentliche IP-Adresse.

![Auflösen der öffentlichen IP-Adresse](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu verwalteten Instanzen und zur Konnektivität finden Sie unter [Azure SQL Database Managed Instance Connectivity Architecture](sql-database-managed-instance-connectivity-architecture.md) (Konnektivitätsarchitektur der verwalteten Azure SQL-Datenbank-Instanz).
