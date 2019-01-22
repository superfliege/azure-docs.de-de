---
title: SQL Server auf Azure-VMs – Versionshinweise | Microsoft-Dokumentation
description: Erfahren Sie mehr über die neuen Funktionen und Verbesserungen von SQL Server auf Azure-VMs.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2018
ms.author: mathoma
ms.openlocfilehash: e3f44181ca1a5ea64815aadf52aa7ea792a21416
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358574"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server auf Azure-VMs – Versionshinweise

Mit Azure können Sie einen virtuellen Computer (VM) mit einem integrierten SQL Server-Image bereitstellen. Dieser Artikel listet neue Funktionen und Verbesserungen der neuesten SQL Server-Version auf, die auf einer Azure-VM bereitgestellt wird. 

## <a name="december-2018"></a>Dezember 2018

| **Änderung** | Details |
| --- | --- |
| **Neuer Ressourcenanbieter für SQL-Clustergruppen** | Es gibt ein neuen Ressourcenanbieter (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroup), der die Metadaten über den Windows-Failovercluster definiert. Ein Einbinden einer SQL Server-VM in die *SqlVirtualMachineGroup* startet den Windows-Failovercluster-Dienst und verknüpft den virtuellen Computer mit dem Cluster.  |
|**Automatisieren des Einrichtens einer Verfügbarkeitsgruppenbereitstellung mit Azure-Schnellstartvorlagen** |Es ist jetzt möglich, mit zwei Azure-Schnellstartvorlagen den Windows-Failovercluster zu erstellen, SQL Server-VMs in diesen einzubinden, den Listener zu erstellen und den internen Load Balancer zu konfigurieren. Weitere Informationen finden Sie unter [Erstellen von WSFC und Listener sowie Konfigurieren von ILB für eine Always On-Verfügbarkeitsgruppe auf einer SQL Server-VM mit Azure-Schnellstartvorlage](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Automatische Registrierung beim SQL Server-Ressourcenanbieter** | SQL Server-VMs, die nach diesem Monat bereitgestellt werden, werden automatisch beim neuen SQL Server-Ressourcenanbieter registriert. SQL Server-VMs, die vor diesem Monat bereitgestellt wurden, müssen weiterhin manuell registriert werden. Weitere Informationen finden Sie unter [Registrieren vorhandener SQL-VMs beim neuen Ressourcenanbieter](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-new-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>November 2018

| **Änderung** | Details |
| --- | --- |
| **Neuer SQL-VM-Ressourcenanbieter** |  Es gibt einen neuen Ressourcenanbieter für SQL Server-VMs (Microsoft.SqlVirtualMachine), der eine bessere Verwaltung Ihrer SQL Server-VM ermöglicht. Weitere Informationen zum Registrieren Ihrer VM finden Sie unter [Registrieren vorhandener SQL-VMs beim neuen Ressourcenanbieter](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-new-resource-provider). |
|**Wechsel des Lizenzierungsmodells** |Sie können nun mit der Azure CLI oder mit PowerShell für Ihre SQL-VM zwischen dem nutzungsbasierten Modell und dem BYOL-Modell (Bring-Your-Own-License) wechseln. Weitere Informationen finden Sie unter [Ändern des Lizenzierungsmodells für einen virtuellen SQL-Computer](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Zusätzliche Ressourcen

**Virtuelle Windows-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrieren einer Datenbank zu SQL Server auf einer Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md)
* [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md)
* [Anwendungsmuster und Entwicklungsstrategien für SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuelle Linux-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Linux-Computer](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bereitstellen eines virtuellen Linux-Computers mit SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Häufig gestellte Fragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server unter Linux – Dokumentation](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
