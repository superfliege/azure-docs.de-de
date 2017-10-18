---
title: "Übersicht über SQL Server auf virtuellen Azure-Computern unter Linux | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie vollwertige SQL Server-Editionen auf virtuellen Azure-Computern unter Linux ausführen. Außerdem finden Sie hier direkte Links zu allen Linux-basierten SQL Server-VM-Images sowie zu verwandten Inhalten."
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.workload: iaas-sql-server
ms.date: 10/02/2017
ms.author: jroth
ms.openlocfilehash: 787e696edd2a446bec280b763fe5ac591782c8ae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Übersicht über SQL Server auf virtuellen Azure-Computern (Linux)

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](sql-server-linux-virtual-machines-overview.md)

Dieses Thema beschreibt die Optionen im Zusammenhang mit der Ausführung von SQL Server auf virtuellen Azure-Computern (VMs) unter Linux und enthält [Links zu Portal-Images](#option-1-create-a-sql-vm-with-per-minute-licensing).

> [!NOTE]
> Wenn Sie bereits mit SQL Server vertraut sind und nur Informationen zum Bereitstellen eines virtuellen Linux-Computers mit SQL Server benötigen, helfen Ihnen die Informationen unter [Provision a Linux SQL Server virtual machine in the Azure portal](provision-sql-server-linux-virtual-machine.md) (Bereitstellen eines virtuellen Linux-Computers mit SQL Server über das Azure-Portal) weiter. Informationen zur Erstellung eines virtuellen Windows-Computers mit SQL Server finden Sie dagegen unter [Bereitstellen eines virtuellen Computers mit SQL Server über das Azure-Portal](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md).

Wenn Sie ein Datenbankadministrator oder Entwickler sind, stellen Azure-VMs eine Möglichkeit zum Verschieben Ihrer lokalen SQL Server-Workloads und -Anwendungen in die Cloud dar.

## <a name="scenarios"></a>Szenarien

Es gibt viele Gründe, die für das Hosten von Daten in Azure sprechen. Wenn Sie Ihre Anwendung in Azure entwickeln oder zu Azure migrieren, ist es aus Leistungsgründen sinnvoll, auch die Back-End-Daten in Azure zu platzieren. Sie haben automatisch Zugriff auf mehrere Rechenzentren, um weltweite Präsenz und die Notfallwiederherstellung zu ermöglichen. Außerdem sind die Daten dauerhaft stark geschützt.

Die Ausführung von SQL Server auf Azure-VMs ist eine Möglichkeit zum Speichern Ihrer relationalen Daten in Azure. Sie können auch den Azure SQL-Datenbank-Dienst verwenden. Eine Gegenüberstellung von SQL Server auf virtuellen Computern und Azure SQL-Datenbank finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

## <a name="create-a-new-sql-vm"></a>Erstellen einer neuen SQL-VM

Eine ausführliche Anleitung zum Erstellen eines neuen virtuellen SQL Server-Computers finden Sie im Tutorial [Provision a Linux SQL Server virtual machine in the Azure portal](provision-sql-server-linux-virtual-machine.md) (Bereitstellen eines virtuellen Linux-Computers mit SQL Server über das Azure-Portal).

Die folgende Tabelle enthält eine Matrix der aktuellen SQL Server-Images im Katalog der virtuellen Computer. Klicken Sie auf einen beliebigen Link, um mit dem Erstellen einer neuen SQL-VM mit Ihren Angaben zu Version, Edition und Betriebssystem zu beginnen.

> [!TIP]
> Informationen zu den VM- und SQL-Preisen für diese Images finden Sie auf der [Seite mit den Preisinformationen für virtuelle Linux-Computer mit SQL Server](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

| Version | Betriebssystem | Edition |
| --- | --- | --- |
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> Informationen zu den verfügbaren Images für virtuelle Windows-Computer mit SQL Server finden Sie in der [Übersicht über SQL Server auf virtuellen Azure-Computern](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).

## <a id="packages"></a> Installierte Pakete

Wenn Sie SQL Server unter Linux konfigurieren, installieren Sie das Datenbankmodulpaket und je nach Bedarf mehrere optionale Pakete. Die meisten Pakete werden von den Linux-VM-Images für SQL Server automatisch installiert. Die folgende Tabelle zeigt, welche Pakete für die einzelnen Distributionen installiert werden:

| Distribution | [Datenbankmodul](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [Tools](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [SQL Server-Agent](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [Volltextsuche](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [HA-Add-On](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![no](./media/sql-server-linux-virtual-machines-overview/no.png) |
| SLES | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![no](./media/sql-server-linux-virtual-machines-overview/no.png) | ![no](./media/sql-server-linux-virtual-machines-overview/no.png) |
| Ubuntu | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![Ja](./media/sql-server-linux-virtual-machines-overview/yes.png) |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konfigurieren und Verwenden von SQL Server unter Linux finden Sie in der [Übersicht über SQL Server unter Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview).
