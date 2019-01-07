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
ms.openlocfilehash: 52036d8f5d25fc4a4f2d1b602428e9cba4762b7f
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993137"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server auf Azure-VMs – Versionshinweise

Mit Azure können Sie einen virtuellen Computer (VM) mit einem integrierten SQL Server-Image bereitstellen. Dieser Artikel listet neue Funktionen und Verbesserungen der neuesten SQL Server-Version auf, die auf einer Azure-VM bereitgestellt wird. 


## <a name="november-2018"></a>November 2018
- **Neuer SQL-Ressourcenanbieter**: Es gibt einen neuen Ressourcenanbieter für SQL-VMs, der eine bessere Verwaltung Ihrer VM ermöglicht. Weitere Informationen zum Registrieren Ihrer VM finden Sie unter [Registrieren vorhandener SQL-VMs beim neuen Ressourcenanbieter](virtual-machines-windows-sql-ahb.md#register-existing-sql-vm-with-new-resource-provider).
- **Wechsel des Lizenzierungsmodells**: Sie können nun mit der Azure CLI oder PowerShell für Ihre SQL-VM zwischen dem nutzungsbasierten Modell und dem BYOL-Modell (Bring-Your-Own-License) wechseln. Weitere Informationen finden Sie unter [Ändern des Lizenzmodells für eine SQL-VM](virtual-machines-windows-sql-ahb.md).



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
