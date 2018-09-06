---
title: Benutzerdefiniertes DNS für verwaltete Azure SQL-Datenbank-Instanzen | Microsoft-Dokumentation
description: In diesem Thema werden die Konfigurationsoptionen für ein benutzerdefiniertes DNS mit einer verwalteten Azure SQL-Datenbank-Instanz beschrieben.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: af9afcbf97df5f3d7fa82f6ea0163c714fa4f582
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051740"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Konfigurieren eines benutzerdefinierten DNS für eine verwaltete Azure SQL-Datenbank-Instanz

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion) bereitgestellt werden. Es gibt einige Szenarien (z.B. Verbindungsserver für andere SQL-Instanzen in Ihrer Cloud- oder Hybridumgebung), bei denen private Hostnamen von der verwalteten Instanz aufgelöst werden müssen. In diesem Fall müssen Sie einen benutzerdefinierten DNS in Azure konfigurieren. Da die verwaltete Instanz den gleichen DNS für deren interne Funktionsweise verwendet, muss die DNS-Konfiguration des virtuellen Netzwerks mit der verwalteten Instanz kompatibel sein. 

Um dafür zu sorgen, dass eine benutzerdefinierte DNS-Konfiguration mit der verwalteten Instanz kompatibel ist, müssen Sie folgende Schritte ausführen: 
- Konfigurieren Sie einen benutzerdefinierten DNS-Server für die Auflösung von öffentlichen Domänennamen. 
- Setzen Sie die DNS-IP-Adresse 168.63.129.16 des rekursiven Azure-Konfliktlösers an das Ende der DNS-Liste des virtuellen Netzwerks. 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Einrichten benutzerdefinierter DNS-Serverkonfigurationen

1. Im Azure-Portal finden Sie die Option „Benutzerdefiniertes DNS“ für Ihr VNET.

   ![Option „Benutzerdefiniertes DNS“](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Wechseln Sie zu „Benutzerdefiniert“, und geben Sie die IP-Adresse Ihres benutzerdefinierten DNS-Servers sowie die IP-Adresse des rekursiven Azure-Konfliktlösers ein: 168.63.129.16. 

   ![Option „Benutzerdefiniertes DNS“](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Wenn Sie nicht den rekursiven Azure-Konfliktlöser im DNS festlegen, wird die verwaltete Instanz in einen fehlerhaften Zustand versetzt. Um diesen Zustand zu verlassen, müssen Sie eventuell eine neue Instanz in einem VNET mit den entsprechenden Netzwerkrichtlinien erstellen, Daten auf Instanzebene neu erstellen und Ihre Datenbanken wiederherstellen. Weitere Informationen finden Sie unter [VNET-Konfiguration](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial, in dem die Erstellung einer neuen verwalteten Instanz gezeigt wird, finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-create-tutorial-portal.md).
- Informationen zum Konfigurieren eines VNet für eine verwaltete Instanz finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-vnet-configuration.md).
