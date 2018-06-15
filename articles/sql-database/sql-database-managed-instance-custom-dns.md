---
title: Benutzerdefiniertes DNS für verwaltete Azure SQL-Datenbank-Instanzen | Microsoft-Dokumentation
description: In diesem Thema werden die Konfigurationsoptionen für ein benutzerdefiniertes DNS mit einer verwalteten Azure SQL-Datenbank-Instanz beschrieben.
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 05a7b600ae8672447126b79cda10ca94c6d0fb48
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649328"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Konfigurieren eines benutzerdefinierten DNS für eine verwaltete Azure SQL-Datenbank-Instanz

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion) bereitgestellt werden. Es gibt ein paar Szenarien (z.B. Verbindungsserver für andere SQL-Instanzen in Ihrer Cloud- oder Hybridumgebung), bei denen private Hostnamen von der verwalteten Instanz aufgelöst werden müssen. In diesem Fall müssen Sie einen benutzerdefinierten DNS in Azure konfigurieren. Da die verwaltete Instanz den gleichen DNS für deren interne Funktionsweise verwendet, muss die DNS-Konfiguration des virtuellen Netzwerks mit der verwalteten Instanz kompatibel sein. 

Um die Konfiguration einer benutzerdefinierten DNS zu erstellen, die mit der verwalteten Instanz kompatibel ist, müssen Sie folgende Schritte ausführen: 
- Konfigurieren des benutzerdefinierten DNS zum Weiterleiten von Anforderungen an Azure DNS 
- Einrichten des benutzerdefinierten DNS als primär und Azure DNS als sekundär für das VNET 
- Registrieren des benutzerdefinierten DNS als primär und Azure DNS als sekundär

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Konfigurieren des benutzerdefinierten DNS zum Weiterleiten von Anforderungen an Azure DNS 

Gehen Sie folgendermaßen vor, um die DNS-Weiterleitung unter Windows Server 2016 zu konfigurieren: 

1. Klicken Sie im **Server-Manager** auf **Tools** und dann auf **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Doppelklicken Sie auf **Weiterleitungen**.

   ![Weiterleitungen](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. Klicken Sie auf **Edit**. 

   ![Liste der Weiterleitungen](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Geben Sie die IP-Adresse eines rekursiven Azure-Konfliktlösers ein, z.B. 168.63.129.16.

   ![IP-Adresse des rekursiven Konfliktlösers](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Einrichten des benutzerdefinierten DNS als primär und Azure DNS als sekundär 
 
In die DNS-Konfiguration für ein Azure-VNET müssen IP-Adressen eingegeben werden. Konfigurieren Sie daher anhand der folgenden Schritte die Azure-VM, die den DNS-Server mit einer statischen IP-Adresse hostet: 

1. Öffnen Sie im Azure-Portal die VM-Netzwerkschnittstelle des benutzerdefinierten DNS.

   ![Netzwerkschnittstelle](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. Wählen Sie im Abschnitt „IP-Konfigurationen“ die IP-Konfiguration aus. 

   ![IP-Konfiguration](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Legen Sie für private IP-Adressen „Statisch“ fest. Notieren Sie sich die IP-Adresse (10.0.1.5 bei diesem Screenshot). 

   ![Statisch](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Registrieren des benutzerdefinierten DNS als primär und Azure DNS als sekundär 

1. Im Azure-Portal finden Sie die Option „Benutzerdefiniertes DNS“ für Ihr VNET.

   ![Option „Benutzerdefiniertes DNS“](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Wechseln Sie zu „Benutzerdefiniert“, und geben Sie die Server IP-Adresse Ihres benutzerdefinierten DNS sowie die IP-Adresse des rekursiven Azure-Konfliktlösers ein, z.B. 168.63.129.16. 

   ![Option „Benutzerdefiniertes DNS“](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Wenn Sie nicht den rekursiven Azure-Konfliktlöser im DNS festlegen, wird die verwaltete Instanz in einen fehlerhaften Zustand versetzt. Um diesen Zustand zu verlassen, müssen Sie eventuell eine neue Instanz in einem VNET mit den entsprechenden Netzwerkrichtlinien erstellen, Daten auf Instanzebene neu erstellen und Ihre Datenbanken wiederherstellen. Weitere Informationen finden Sie unter [VNET-Konfiguration](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial, in dem die Erstellung einer neuen verwalteten Instanz gezeigt wird, finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-create-tutorial-portal.md).
- Informationen zum Konfigurieren eines VNet für eine verwaltete Instanz finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-vnet-configuration.md).
