---
title: Erste Schritte mit SAP in Azure-VMs | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über SAP-Lösungen auf virtuellen Windows-Computern in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40ed06bef45948068e3845e728d9c1d63ed62e71
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762802"
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Verwenden von Azure zum Hosten und Ausführen von SAP-Workloadszenarien

Mit Microsoft Azure können Sie Ihre geschäftskritischen SAP-Workloads und -Szenarien zuverlässig auf einer skalierbaren, konformen und in Unternehmen bewährten Plattform ausführen.  Profitieren Sie von der Skalierbarkeit, Flexibilität und den Einsparungsmöglichkeiten von Azure. Dank der erweiterten Partnerschaft zwischen Microsoft und SAP können Sie SAP-Anwendungen in Dev/Test- und Produktionsszenarien in Azure ausführen – und erhalten dabei vollständigen Support. Wir können Sie in allen Belangen unterstützen, von SAP NetWeaver bis SAP S/4HANA, SAP BI, von Linux bis Windows, von SAP HANA bis SQL.

Neben dem Hosten von SAP NetWeaver-Szenarien mit verschiedenen DBMS-Lösungen in Azure können Sie auch andere SAP-Workloadszenarien hosten, z.B. SAP BI unter Azure. 

Die Einzigartigkeit von Azure für SAP HANA ist ein Angebot, mit dem sich Azure von seinen Wettbewerbern absetzt. Um das Hosten von SAP-Szenarien mit SAP HANA zu ermöglichen, für die mehr Arbeitsspeicher- und CPU-Ressourcen benötigt werden, wird in Azure die Nutzung von dedizierter Bare-Metal-Hardware für Kunden angeboten. Sie kann zum Ausführen von SAP HANA-Bereitstellungen verwendet werden, für die bis zu 24TB Arbeitsspeicher (120TB horizontale Skalierung) für S/4HANA oder andere SAP HANA-Workloads erforderlich sind. Mit dieser einzigartigen Azure-Lösung von SAP HANA in Azure (große Instanzen) können Sie SAP HANA auf der dedizierten Bare-Metal-Hardware mit der SAP-Anwendungsschicht oder Workload-Middleware-Ebene ausführen, die auf nativen Azure Virtual Machines gehostet wird. Diese Lösung wird in mehreren Dokumenten im Abschnitt „SAP HANA in Azure (große Instanzen)“ beschrieben.   

Durch das Hosten von SAP-Workloadszenarien in Azure können auch Anforderungen in Bezug auf die Identitätsintegration und das einmalige Anmelden mit Azure Activity Directory für verschiedene SAP-Komponenten und SAP-SaaS- oder -PaaS-Angebote entstehen. Eine Liste mit Integrations- und SSO-Szenarien per Azure Active Directory (AAD) und SAP-Entitäten mit den entsprechenden Beschreibungen finden Sie im Abschnitt „AAD-SAP-Identitätsintegration und einmaliges Anmelden“.

## <a name="latest-changes"></a>Aktuelle Änderungen

Veröffentlichung von [Steuerung von HANA in Azure (große Instanzen) über das Azure-Portal](hana-li-portal.md)

Veröffentlichung von [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs unter SUSE Linux Enterprise Server mit Azure NetApp Files für SAP-Anwendungen](high-availability-guide-suse-netapp-files.md)

Klarstellung zu den Einstellungen für den **Linux OS-Parameter net.ipv4.tcp_timestamps** in Kombination mit dem Azure Load Balancer

Freigabe von [SAP-Workloadkonfigurationen mit Azure-Verfügbarkeitszonen](sap-ha-availability-zones.md)

Freigabe der [Prüfliste für SAP-Workloadplanung und -bereitstellung](sap-deployment-checklist.md)




## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA in Azure (große Instanzen)

Eine Dokumentationsreihe führt Sie durch SAP HANA in Azure (große Instanzen), Kurzform: HANA für große Instanzen. In den Dokumenten werden die aufgeführten Bereiche von HANA für große Instanzen behandelt:

- [Übersicht über SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [Installieren von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Problembehandlung und Überwachung von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Nächste Schritte:

- Lesen Sie [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA in Azure Virtual Machines
In diesem Abschnitt der Dokumentation werden verschiedene Aspekte von SAP HANA behandelt. Dabei wird vorausgesetzt, dass Sie mit den wichtigsten Diensten von Azure vertraut sind, die elementare Dienste von Azure IaaS bereitstellen, also hauptsächlich über Kenntnisse über Azure Compute, Speicher und Netzwerke verfügen. Viele dieser Themen werden im SAP NetWeaver-bezogenen [Azure-Planungsleitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) behandelt. 

Die Dokumentation, die sich speziell auf HANA in Azure bezieht, umfasst folgende Artikel und deren Unterartikel:

- [Schnellstart: Manuelle Installation von SAP HANA (Einzelinstanz) auf Azure-VMs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Bereitstellen von SAP S/4HANA oder BW/4HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [SAP HANA-Infrastrukturkonfigurationen und -Vorgänge in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Hochverfügbarkeit von SAP HANA für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Verfügbarkeit von SAP HANA innerhalb einer Azure-Region](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [SAP HANA-Verfügbarkeit in den Azure-Regionen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA-Sicherung mit Azure Backup auf Dateiebene](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA-Sicherung auf der Grundlage von Speichermomentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Bereitstellung von SAP NetWeaver in Azure Virtual Machines
In diesem Abschnitt finden Sie die Dokumentation zur Planung und Bereitstellung von SAP NetWeaver und Business One in Azure. Die Dokumentation in diesem Kapitel konzentriert sich hauptsächlich auf die Grundlagen und die Verwendung von nicht-HANA-Datenbanken mit SAP-Workloads in Azure. Die Dokumente und Artikel über Hochverfügbarkeit bilden auch die Grundlage für die Hochverfügbarkeit von HANA in Azure. Die Liste umfasst beispielsweise die folgenden Artikel:

- [SAP Business One in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Bereitstellen von SAP IDES EHP7 SP3 für SAP ERP 6.0 in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [Ausführen von SAP NetWeaver auf Microsoft Azure-SUSE Linux-VMs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Azure Virtual Machines – Planung und Implementierung für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure Virtual Machines – Bereitstellung für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Schützen einer SAP NetWeaver-Anwendungsbereitstellung mit mehreren Ebenen mit Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [SAP LaMa-Connector für Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

In Bezug auf nicht-HANA-Datenbanken mit SAP-Workloads in Azure umfasst die Liste beispielsweise die folgenden Dokumente:

- [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [Azure Virtual Machines – SQL Server-DBMS-Bereitstellung für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [Azure Virtual Machines – Oracle-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [Azure Virtual Machines – IBM DB2-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [Azure Virtual Machines – SAP ASE-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Bereitstellung von SAP MaxDB, SAP liveCache und SAP Content Server auf Azure-VMs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Sehen Sie sich in Bezug auf SAP HANA-Datenbanken in Azure den Abschnitt „SAP HANA in Azure Virtual Machines“ an.

Das Einstiegsdokument für Hochverfügbarkeit von SAP-Workloads in Azure finden Sie hier:

- [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Das Einstiegsdokument verweist auf verschiedene andere Dokumente, die sich mit der entsprechenden Architektur und Szenarien befassen. In den nachfolgenden Szenariendokumenten werden Links zu ausführlichen technischen Dokumenten bereitgestellt, in denen die Bereitstellung und Konfiguration der verschiedenen Hochverfügbarkeitsmethoden erläutert werden. In den anderen Dokumenten über das Einrichten und Konfigurieren von Hochverfügbarkeit für SAP NetWeaver-Workloads werden Linux- und Windows-Betriebssysteme behandelt.


In Bezug auf die Integration von Azure Active Directory (AAD) und SAP-Diensten und Einmaliges Anmelden umfasst die Liste beispielsweise die folgenden Dokumente:

- [Tutorial: Azure Active Directory-Integration mit SAP Cloud for Customer](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform Identity Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Ihre S/4HANA-Umgebung – SAP Fiori Launchpad: SAML-SSO mit Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

In Bezug auf die Integration von Azure-Diensten in SAP-Komponenten umfasst die Liste beispielsweise die folgenden Dokumente:

- [Verwenden von SAP HANA in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery und SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Verwenden des SAP BW-Connectors in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [SAP HANA- und Business Warehouse-Datenintegration mit Azure Data Factory](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




