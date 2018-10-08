---
title: Benutzerdefiniertes DNS für verwaltete Azure SQL-Datenbank-Instanzen | Microsoft-Dokumentation
description: In diesem Thema werden die Konfigurationsoptionen für ein benutzerdefiniertes DNS mit einer verwalteten Azure SQL-Datenbank-Instanz beschrieben.
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
ms.date: 09/23/2018
ms.openlocfilehash: f26ea763d48d03fe7e981b7abbbe64e573ec0b3a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224272"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Konfigurieren eines benutzerdefinierten DNS für eine verwaltete Azure SQL-Datenbank-Instanz

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz bereitgestellt werden. Es gibt einige Szenarien (z.B. Verbindungsserver für andere SQL-Instanzen in Ihrer Cloud- oder Hybridumgebung), bei denen private Hostnamen von der verwalteten Instanz aufgelöst werden müssen. In diesem Fall müssen Sie einen benutzerdefinierten DNS in Azure konfigurieren. Da die verwaltete Instanz den gleichen DNS für deren interne Funktionsweise verwendet, muss die DNS-Konfiguration des virtuellen Netzwerks mit der verwalteten Instanz kompatibel sein. 

Um dafür zu sorgen, dass eine benutzerdefinierte DNS-Konfiguration mit der verwalteten Instanz kompatibel ist, müssen Sie folgende Schritte ausführen: 
- Konfigurieren Sie einen benutzerdefinierten DNS-Server für die Auflösung von öffentlichen Domänennamen. 
- Setzen Sie die DNS-IP-Adresse 168.63.129.16 des rekursiven Azure-Konfliktlösers an das Ende der DNS-Liste des virtuellen Netzwerks. 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Einrichten benutzerdefinierter DNS-Serverkonfigurationen

1. Im Azure-Portal finden Sie die Option „Benutzerdefiniertes DNS“ für Ihr VNET.

   ![Option „Benutzerdefiniertes DNS“](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Wechseln Sie zu „Benutzerdefiniert“, und geben Sie die IP-Adresse Ihres benutzerdefinierten DNS-Servers sowie die IP-Adresse des rekursiven Azure-Konfliktlösers ein: 168.63.129.16. 

   ![Option „Benutzerdefiniertes DNS“](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Wenn Sie den rekursiven Azure-Resolver in der DNS-Liste nicht festlegen, kann es passieren, dass die verwaltete Instanz in einen fehlerhaften Zustand überführt wird, wenn die benutzerdefinierten Server aus irgendeinem Grund nicht verfügbar sind. Um diesen Zustand zu verlassen, müssen Sie eventuell eine neue Instanz in einem VNET mit den entsprechenden Netzwerkrichtlinien erstellen, Daten auf Instanzebene neu erstellen und Ihre Datenbanken wiederherstellen. Durch das Festlegen des rekursiven Azure-Resolvers als letzten Eintrag in der DNS-Liste können öffentliche Namen immer noch aufgelöst werden, auch wenn bei allen benutzerdefinierten Servern ein Fehler auftritt. Weitere Informationen finden Sie unter [VNET-Konfiguration](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial, in dem die Erstellung einer neuen verwalteten Instanz gezeigt wird, finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md).
- Informationen zum Konfigurieren eines VNet für eine verwaltete Instanz finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-vnet-configuration.md).
