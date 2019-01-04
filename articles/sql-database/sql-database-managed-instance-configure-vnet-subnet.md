---
title: Verwenden einer verwalteten Azure SQL-Datenbank-Instanz zum Konfigurieren eines vorhandenen VNET/Subnetzes | Microsoft-Dokumentation
description: In diesem Thema wird beschrieben, wie Sie ein vorhandenes virtuelles Netzwerk (VNET) und Subnetz konfigurieren, in dem Sie eine verwaltete Azure SQL-Datenbank-Instanz bereitstellen können.
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
ms.openlocfilehash: 53aba5192ddf57598965fcfe0db5f2b18423c7e9
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345598"
---
# <a name="configure-an-existing-vnet-for-azure-sql-database-managed-instance"></a>Konfigurieren eines vorhandenen VNET für eine verwaltete Azure SQL-Datenbank-Instanz

Eine verwaltete Azure SQL-Datenbank-Instanz darf nur in einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) und in dem Subnetz bereitgestellt werden, das speziell für verwaltete Instanzen bestimmt ist. Sie können das vorhandene VNET und Subnetz verwenden, wenn diese jeweils gemäß den [VNET-Anforderungen für verwaltete Instanzen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) konfiguriert sind. 

Falls Sie über ein neues Subnetz verfügen, das immer noch nicht konfiguriert ist, und nicht sicher sind, ob das Subnetz die [Anforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) erfüllt, oder falls Sie nach dem Vornehmen von Änderungen die Konformität des Subnetzes mit den [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) überprüfen möchten, können Sie wie folgt vorgehen: Überprüfen und ändern Sie Ihr Netzwerk mit dem Skript, das in diesem Abschnitt beschrieben wird. 

  > [!Note]
  > Sie können nur eine verwaltete Instanz in virtuellen Resource Manager-Netzwerken erstellen. Azure-VNETs, die mit dem klassischen Bereitstellungsmodell bereitgestellt werden, werden nicht unterstützt. Berechnen Sie die Subnetzgröße, indem Sie die Richtlinien im Abschnitt [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances) befolgen. Der Grund ist, dass die Größe des Subnetzes nicht mehr geändert werden kann, nachdem Sie die Ressourcen darin bereitgestellt haben.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Überprüfen und Ändern eines vorhandenen virtuellen Netzwerks 

Wenn Sie eine verwaltete Instanz in einem vorhandenen Subnetz erstellen möchten, empfehlen wir das folgende PowerShell-Skript zum Vorbereiten des Subnetzes:
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Die Subnetzvorbereitung erfolgt in drei einfachen Schritten:

1. Überprüfen: Das ausgewählte virtuelle Netzwerk und das Subnetz werden im Hinblick auf die Netzwerkanforderungen der verwalteten Instanz überprüft.
2. Bestätigen: Dem Benutzer wird eine Reihe von Änderungen angezeigt, mit denen das Subnetz für die Bereitstellung der verwalteten Instanz vorbereitet werden soll und die er bestätigen muss.
3. Vorbereiten: Das virtuelle Netzwerk und das Subnetz werden richtig konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial zum Erstellen eines VNET, Erstellen einer verwalteten Instanz und Wiederherstellen einer Datenbank anhand einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Lesen Sie bei Problemen im Zusammenhang mit dem DNS den Artikel [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
