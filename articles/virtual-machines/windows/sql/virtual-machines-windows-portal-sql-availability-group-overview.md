---
title: SQL Server-Verfügbarkeitsgruppen – Azure Virtual Machines – Übersicht | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Einführung in SQL Server-Verfügbarkeitsgruppen auf virtuellen Azure-Computern.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: 5f8ae6d9138a7413b0cca4cca7bcc47c13212674
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358050"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Einführung in SQL Server Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern #

Dieser Artikel enthält eine Einführung in SQL Server-Verfügbarkeitsgruppen in Azure Virtual Machines. 

Always On-Verfügbarkeitsgruppen in Azure Virtual Machines sind vergleichbar mit lokalen Always On-Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Das Diagramm veranschaulicht die Komponenten einer vollständigen SQL Server-Verfügbarkeitsgruppe in Azure Virtual Machines.

![Verfügbarkeitsgruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Der wesentliche Unterschied bei einer Verfügbarkeitsgruppe in Azure Virtual Machines ist, dass die virtuellen Azure-Computer einen [Lastenausgleich](../../../load-balancer/load-balancer-overview.md) erfordern. Der Lastenausgleich speichert die IP-Adressen für den Verfügbarkeitsgruppenlistener. Falls Sie über mehrere Verfügbarkeitsgruppen verfügen, benötigt jede Gruppe einen Listener. Ein einzelner Lastenausgleich kann mehrere Listener unterstützen.

Außerdem empfiehlt es sich, in einem Azure IaaS-VM-Gast-Failovercluster eine einzelne Netzwerkkarte pro Server (Clusterknoten) und ein einzelnes Subnetz zu haben. Azure-Netzwerktechnologie hat physische Redundanz, die zusätzliche Netzwerkkarten und Subnetze in einem Azure IaaS-VM-Gastcluster überflüssig macht. Obwohl im Clustervalidierungsbericht eine Warnung ausgegeben wird, dass die Knoten nur in einem einzigen Netzwerk erreichbar sind, kann diese Warnung für Azure IaaS-VM-Gast-Failovercluster einfach ignoriert werden. 

Wenn Sie zur Erstellung einer SQL Server-Verfügbarkeitsgruppe in Azure Virtual Machines bereit sind, sehen Sie sich die folgenden Tutorials an:

## <a name="automatically-create-an-availability-group-from-a-template"></a>Automatisches Erstellen einer Verfügbarkeitsgruppe auf der Grundlage einer Vorlage

[Automatisches Konfigurieren der AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern – Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Manuelles Erstellen einer Verfügbarkeitsgruppe im Azure-Portal

Sie können die virtuellen Computer auch manuell ohne Vorlage erstellen. Schließen Sie zunächst die erforderlichen Vorbereitungen ab, und erstellen Sie anschließend die Verfügbarkeitsgruppe. Informationen hierzu finden Sie in den folgenden Themen: 

- [Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md) (Konfigurieren der erforderlichen Komponenten für SQL Server Always On-Verfügbarkeitsgruppen in Azure Virtual Machines)

- [Erstellen einer AlwaysOn-Verfügbarkeitsgruppe zur Verbesserung der Verfügbarkeit und Notfallwiederherstellung](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren einer SQL Server Always On-Verfügbarkeitsgruppe in Azure Virtual Machines in verschiedenen Regionen](virtual-machines-windows-portal-sql-availability-group-dr.md)
