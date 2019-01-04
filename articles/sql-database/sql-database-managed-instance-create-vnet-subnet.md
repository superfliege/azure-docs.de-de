---
title: 'Verwaltete Azure SQL-Datenbank-Instanz: Erstellen eines VNET | Microsoft-Dokumentation'
description: In diesem Thema erfahren Sie, wie Sie ein virtuelles Netzwerk (VNET) erstellen, in dem Sie eine verwaltete Azure SQL-Datenbank-Instanz bereitstellen können.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: ebdfc80d3802ad8eb7da6fb7f152efdaee8d777d
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345075"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz

In diesem Thema erfahren Sie, wie Sie ein gültiges VNET und ein entsprechendes Subnetz erstellen, in dem Sie verwaltete Azure SQL-Datenbank-Instanzen bereitstellen können.

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz bereitgestellt werden. Diese Bereitstellung ermöglicht die folgenden Szenarien: 
- Schützen der privaten IP-Adresse
- Direktes Herstellen einer Verbindung mit einer verwalteten Instanz von einem lokalen Netzwerk 
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und einem Verbindungsserver oder einem anderen lokalen Datenspeicher 
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und Azure-Ressourcen  

  > [!Note]
  > Vor dem Bereitstellen Ihrer ersten Instanz sollten Sie [die Größe des Subnetzes für die verwaltete Instanz ermitteln](sql-database-managed-instance-determine-size-vnet-subnet.md), da die Größe des Subnetzes nach dem Hinzufügen der Ressourcen nicht mehr geändert werden kann.
  > Wenn Sie die Verwendung eines vorhandenen virtuellen Netzwerks beabsichtigen, müssen Sie diese Netzwerkkonfiguration ändern, um Ihre verwaltete Instanz einzubeziehen. Weitere Informationen finden Sie unter [Ändern eines vorhandenen virtuellen Netzwerks für die verwaltete Instanz](sql-database-managed-instance-configure-vnet-subnet.md). 

## <a name="create-a-new-virtual-network"></a>Erstellen eines neuen virtuellen Netzwerks

Die einfachste Möglichkeit zum Erstellen und Konfigurieren eines virtuellen Netzwerks ist die Verwendung der Azure Resource Manager-Bereitstellungsvorlage.

1. Melden Sie sich beim Azure-Portal an.

2. Verwenden Sie die Schaltfläche **In Azure bereitstellen**, um das virtuelle Netzwerk in Azure-Cloud bereitzustellen:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Diese Schaltfläche öffnet ein Formular zum Konfigurieren der Netzwerkumgebung, in der Sie die verwaltete Instanz bereitstellen können.

  > [!Note]
  > Diese Azure Resource Manager-Vorlage stellt das virtuelle Netzwerk mit zwei Subnetzen bereit. Ein Subnetz namens **ManagedInstances** ist für verwaltete Instanzen reserviert und verfügt über eine vorkonfigurierte Routingtabelle, während das andere Subnetz namens **Standard** für andere Ressourcen verwendet wird, die auf die verwaltete Instanz zugreifen sollen (z. B. Azure Virtual Machines). Sie können das Subnetz **Standard** entfernen, wenn Sie es nicht benötigen.

3. Konfigurieren Sie die Netzwerkumgebung. Im folgenden Formular können Sie Parameter für Ihre Netzwerkumgebung konfigurieren:

![Azure-Netzwerk konfigurieren](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

Sie können die Namen des VNETs und der Subnetze ändern und die Ihren Netzwerkressourcen zugeordneten IP-Adressbereiche anpassen. Sobald Sie auf die Schaltfläche „Kaufen“ klicken, erstellt und konfiguriert dieses Formular Ihre Umgebung. Wenn Sie keine zwei Subnetze benötigen, können Sie das Standardsubnetz löschen. 

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Machen Sie sich mit der [Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md) vertraut.
- Informieren Sie sich unter [Configure an existing VNet for Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vnet-subnet.md) (Konfigurieren eines vorhandenen VNET für die verwaltete Azure SQL-Datenbank-Instanz) über die Vorgehensweise zum Ändern eines vorhandenen virtuellen Netzwerks für die verwaltete Instanz.
- Ein Tutorial zum Erstellen eines VNET, Erstellen einer verwalteten Instanz und Wiederherstellen einer Datenbank anhand einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Bei Problemen im Zusammenhang mit dem DNS lesen Sie [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
