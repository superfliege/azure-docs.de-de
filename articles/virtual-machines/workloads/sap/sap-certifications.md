---
title: "Microsoft Azure-Zertifizierungen für SAP | Microsoft-Dokumentation"
description: Aktualisierte Liste der aktuellen Konfigurationen und Zertifizierungen von SAP auf der Azure-Plattform.
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: rclaus
ms.custom: 
ms.openlocfilehash: e1d1d984aa821bf891d83b3c4a053ba82811ba5e
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP-Zertifizierungen und -Konfigurationen in Microsoft Azure

SAP und Microsoft können auf eine lange Geschichte enger Zusammenarbeit in einer starken Partnerschaft zurückblicken, die den Kunden beider Unternehmen Vorteile bringt. Microsoft führt kontinuierlich Aktualisierungen seiner Plattform durch und übermittelt stets neue Zertifizierungsinformationen an SAP, um sicherzustellen, dass Microsoft Azure die optimale Plattform zum Ausführen von SAP-Workloads ist. In den folgenden Tabellen werden die unterstützten Konfigurationen und die anwachsende Liste von Zertifizierungen beschrieben. 

## <a name="sap-hana-certifications"></a>SAP HANA-Zertifizierungen
Referenzen:

- [SAP-Hinweis 2316233 – SAP HANA in Microsoft Azure (große Instanzen)](https://launchpad.support.sap.com/#/notes/2316233) zu großen HANA-Instanzen in Bezug auf Unterstützung für SAP HANA
- [SAP HANA-zertifizierte IaaS-Plattformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Amazon%20Web%20Services%2CMicrosoft%20Azure) zur Unterstützung für SAP HANA für native Azure-VMs

| SAP-Produkt | Unterstütztes Betriebssystem | Azure-Angebote |
| --- | --- | --- |
| SAP HANA Developer Edition (einschließlich HANA-Clientsoftware, bestehend aus SQLODBC, ODBO [nur Windows], ODBC, JDBC-Treibern, HANA Studio und HANA Database) | Red Hat Enterprise Linux, SUSE Linux Enterprise | VMs der D-Serie |
| Business One unter HANA | SUSE Linux Enterprise | DS14_v2 |
| SAP S/4 HANA |Red Hat Enterprise Linux, SUSE Linux Enterprise | Kontrollierte Verfügbarkeit für GS5, SAP HANA in Azure (große Instanzen) |
| Suite on HANA, OLTP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 für Bereitstellungen mit einem einzelnen Knoten in Nicht-Produktions-Szenarien, SAP HANA in Azure (große Instanzen) |
| HANA Enterprise für BW, OLAP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 für Bereitstellungen mit einem einzelnen Knoten, SAP HANA in Azure (große Instanzen) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 für Bereitstellungen mit einem einzelnen Knoten, SAP HANA in Azure (große Instanzen) |

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver-Zertifizierungen
Microsoft Azure ist für die folgenden SAP-Produkte zertifiziert. Microsoft und SAP bieten vollständigen Support für die Plattform.
Referenzen:

- [1928533 – SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen](https://launchpad.support.sap.com/#/notes/1928533) für alle SAP NetWeaver-basierten Anwendungen, einschließlich SAP TREX, SAP LiveCache und SAP Content Server, sowie alle Datenbanken mit Ausnahme von SAP HANA


| SAP-Produkt | Gastbetriebssystem | RDBMS | VM-Typ (virtueller Computer) |
| --- | --- | --- | --- |
| SAP Business Suite Software |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (nur Windows und Oracle Linux), DB2, SAP ASE |A5 bis A11, D11 bis D14, DS11 bis DS14, DS11_v2 bis DS15_v2, GS1 bis GS5 |
| SAP Business All-in-One |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (nur Windows und Oracle Linux), DB2, SAP ASE |A5 bis A11, D11 bis D14, DS11 bis DS14, DS11_v2 bis DS15_v2, GS1 bis GS5 |
| SAP BusinessObjects BI |Windows |– |A5 bis A11, D11 bis D14, DS11 bis DS14, DS11_v2 bis DS15_v2, GS1 bis GS5 |
| SAP NetWeaver |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (nur Windows und Oracle Linux), DB2, SAP ASE |A5 bis A11, D11 bis D14, DS11 bis DS14, DS11_v2 bis DS15_v2, GS1 bis GS5 |

## <a name="other-sap-workload-supported-on-azure"></a>Andere in Azure unterstützte SAP-Workloads

| SAP-Produkt | Gastbetriebssystem | RDBMS | VM-Typ (virtueller Computer) |
| --- | --- | --- | --- |
| SAP Business One unter SQL Server | Windows  | SQL Server | Alle NetWeaver-zertifizierten VM-Typen |
| SAP BPC 10.01 MS SP08 | Windows | | Alle NetWeaver-zertifizierten VM-Typen<br /> SAP-Hinweis 2451795 |
| SAP Business Objects BI-Plattform | Windows | | SAP-Hinweis 2145537 |
| SAP Data Services 4.2 | | | SAP-Hinweis 2288344 |
