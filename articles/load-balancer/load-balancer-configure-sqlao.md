---
title: Konfigurieren des Lastenausgleichs für SQL Server-Always On | Microsoft-Dokumentation
description: Konfigurieren von Lastenausgleich für SQL Server-Always On und Verwenden von PowerShell zum Erstellen des Lastenausgleichs für die SQL-Implementierung
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: a0c2345b47b9103ac6a7ae998f13a12332e3907e
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="configure-a-load-balancer-for-sql-server-always-on"></a>Konfigurieren des Lastenausgleichs für SQL Server-Always On



SQL Server Always On-Verfügbarkeitsgruppen können jetzt mit einem internen Lastenausgleich ausgeführt werden. Verfügbarkeitsgruppen sind eine führende SQL Server-Lösung für Hochverfügbarkeit und Notfallwiederherstellung. Der Verfügbarkeitsgruppenlistener ermöglicht Clientanwendungen eine nahtlose Verbindung mit dem primären Replikat unabhängig von der Anzahl der Replikate in der Konfiguration.

Der Name des Listeners (DNS) wird einer IP-Adresse mit Lastenausgleich zugeordnet. Azure Load Balancer leitet den eingehenden Datenverkehr nur an den primären Server in der Replikatgruppe weiter.

Sie können die Unterstützung des internen Lastenausgleichs für SQL Server Always On-Endpunkte (Listener) verwenden. Sie haben nun die Kontrolle über die Barrierefreiheit des Listeners. Sie können die IP-Adresse mit Lastenausgleich aus einem bestimmten Subnetz in Ihrem virtuellen Netzwerk auswählen.

Mithilfe eines internen Lastenausgleichs auf dem Listener kann auf den SQL Server-Endpunkt (z. B. Server=tcp:ListenerName,1433;Database=DatabaseName) nur über Folgendes zugegriffen werden:

* Dienste und virtuelle Computer im gleichen virtuellen Netzwerk
* Dienste und virtuelle Computer aus dem lokalen Netzwerk
* Dienste und virtuelle Computer aus verbundenen virtuellen Netzwerken

![Interner Lastenausgleich für SQL Server Always On](./media/load-balancer-configure-sqlao/sqlao1.png)

## <a name="add-an-internal-load-balancer-to-the-service"></a>Hinzufügen eines internen Lastenausgleichs zum Dienst

1. Im folgenden Beispiel konfigurieren Sie ein virtuelles Netzwerk, das ein Subnetz namens „Subnet-1“ enthält:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Fügen Sie Endpunkte mit Lastenausgleich für einen internen Lastenausgleich auf jedem virtuellen Computer hinzu.

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    Im vorherigen Beispiel werden zwei virtuelle Computer namens „sqlsvc1“ und „sqlsvc2“ im Clouddienst „Sqlsvc“ ausgeführt. Nachdem Sie den internen Lastenausgleich mit dem Switch `DirectServerReturn` erstellt haben, fügen Sie dem internen Lastenausgleich Endpunkte mit Lastenausgleich hinzu. Die Endpunkte mit Lastenausgleich ermöglichen es SQL Server, die Listener für die Verfügbarkeitsgruppen zu konfigurieren.

Weitere Informationen zu SQL Server Always On finden Sie unter [Konfigurieren eines internen Load Balancers für eine AlwaysOn-Verfügbarkeitsgruppe in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte zum Konfigurieren eines öffentlichen Lastenausgleichs](load-balancer-get-started-internet-arm-ps.md)
* [Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)
* [Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)
* [Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)
