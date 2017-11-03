---
title: "SAP NetWeaver HA-Installation auf dem Windows-Failovercluster und die Dateifreigabe für die SAP (A)SCS-Instanz unter Azure | Microsoft-Dokumentation"
description: "SAP NetWeaver HA-Installation auf dem Windows-Failovercluster und die Dateifreigabe für die SAP (A)SCS-Instanz"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 71296618-673b-4093-ab17-b7a80df6e9ac
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec7888cfb9b0d288b5b25f580c852ee32306684c
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2017
---
# <a name="sap-netweaver-ha-installation-on-windows-failover-cluster-and-file-share-for-sap-ascs-instance-on-azure"></a>SAP NetWeaver HA-Installation auf dem Windows-Failovercluster und die Dateifreigabe für die SAP (A)SCS-Instanz unter Azure

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1596496]:https://launchpad.support.sap.com/#/notes/1596496

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[sofs-overview]:https://technet.microsoft.com/library/hh831349(v=ws.11).aspx
[new-in-win-2016-storage]:https://docs.microsoft.com/windows-server/storage/whats-new-in-storage

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[sap-blog-new-sap-cluster-resource-dll]:https://blogs.sap.com/2017/08/28/new-sap-cluster-resource-dll-is-available/
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host]:sap-high-availability-installation-wsfc-shared-disk.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-high-availability-installation-wsfc-shared-disk-add-probe-port]:sap-high-availability-installation-wsfc-shared-disk.md#10822f4f-32e7-4871-b63a-9b86c76ce761

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-official-ha-file-share-document]:https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png


[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

Dieses Dokument beschreibt, wie ein hoch verfügbares SAP-System unter Azure mithilfe des **Windows Server-Failoverclusters (WSFC)** und **Scale Out File Share** (Scale Out-Dateifreigabe) als Option für die Gruppierung der SAP (A)SCS-Instanz installiert und konfiguriert wird.

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie diese Dokumente, bevor Sie mit der Installation starten:

* [Architecture Guide - Clustering SAP (A)SCS Instance on **Windows Failover Cluster** Using **File Share**][ (Architekturhandbuch: Gruppieren der SAP (A)SCS-Instanz auf dem Windows-Failovercluster mithilfe der Dateifreigabe)sap-high-availability-guide-wsfc-file-share]

* [Azure Infrastructure Preparation for SAP HA using **Windows Failover Cluster** and **File Shared** for SAP (A)SCS Instance][ (Vorbereitung der Azure-Infrastruktur für SAP-HA mithilfe des Windows-Failoverclusters und der Dateifreigabe für die SAP (A)SCS-Instanz)sap-high-availability-infrastructure-wsfc-file-share]


Sie benötigen die folgenden ausführbaren Dateien bzw. DLLS von SAP:
* Das Installationstool SAP **Software Provisioning Manager** (**SWPM**) mit Version **SPS21 (oder höher)**.
* Laden Sie das **neueste NTCLUST.SAR**-Archiv mit der neuen SAP-Clusterressourcen-DLL herunter. Die neuen SAP-Cluster-DLLS unterstützen die SAP (A)SCS-Hochverfügbarkeit mit Dateifreigabe auf dem Windows Server-Failovercluster.

  Weitere Informationen zur neuen SAP-Clusterressourcen-DLL erhalten Sie auf diesem Blog: [New SAP cluster resource DLL is available!][sap-blog-new-sap-cluster-resource-dll]

Das DBMS-Setup wird hier nicht beschrieben, da die Einrichtung vom verwendeten DBMS abhängt. Jedoch wird davon ausgegangen, dass auf hohe Verfügbarkeit bezogene Aspekte des DBMS mit den Funktionen verwaltet werden, die verschiedene DBMS-Hersteller für Azure unterstützen. Beispiele hierfür sind Always On oder Datenbankspiegelung für SQL Server und Oracle Data Guard für Oracle-Datenbanken. In dem Szenario in diesem Artikel haben wir keinen zusätzlichen Schutz für das DBMS hinzugefügt.

Es sind keine Besonderheiten zu berücksichtigen, wenn unterschiedliche DBMS-Dienste mit dieser Art von SAP ASCS/SCS-Clusterkonfiguration in Azure interagieren.

> [!NOTE]
> Der Installationsvorgang von SAP NetWeaver ABAP-, Java- und ABAP+Java-Systemen ist nahezu identisch. Der wichtigste Unterschied ist, dass ein SAP ABAP-System eine ASCS-Instanz hat. Das SAP Java-System hat eine SCS-Instanz. Das SAP ABAP+Java-System hat eine ASCS- und eine SCS-Instanz, die in der gleichen Microsoft-Failoverclustergruppe ausgeführt werden. Unterschiede bei der Installation der einzelnen SAP NetWeaver-Installationsstapel werden explizit angegeben. Sie können davon ausgehen, dass alle anderen Teile gleich sind.  
>
>

## <a name="install-ascs-instance-on-ascs-cluster"></a>Installieren einer (A)SCS-Instanz auf dem (A)SCS-Cluster

> [!IMPORTANT]
>
>Derzeit wird keine HA-Einstellung mit Dateifreigabekonfiguration durch den Software Provisioning Manager (SWPM) des SAP-Installationstools unterstützt. Deshalb sind einige Handgriffe nötig, um das SAP-System zu installieren, um z.B. die SAP (A)SCS-Instanz zu installieren und zu gruppieren und den einzelnen SAP GLOBALHOST zu konfigurieren.  
>
>Andere Installationsschritte zum Installieren (und Gruppieren) der DBMS-Instanz und SAP-Anwendungsserver sind nicht nötig.
>

### <a name="install-ascs-instance-on-local-drive"></a>Installieren der (A)SCS-Instanz auf dem lokalen Laufwerk

Installieren Sie die SAP (A)SCS-Instanz auf **BEIDEN** Knoten des (A)SCS-Clusters. Installieren Sie sie auf dem **lokalen** Laufwerk. In unserem Beispiel ist das lokale Laufwerk C:\\. Sie können auch ein anderen lokales Laufwerk auswählen.  

Gehen Sie für die Installation folgendermaßen im Installationstool SWPM für SAP vor:

&lt;Produkt&gt; -> &lt;DBMS&gt; -> Installation Anwendung -> Anwendungsserver ABAP (oder Java) -> verteiltes System -> (A)SCS-Instanz

> [!IMPORTANT]
>Aktuell wird das Dateifreigabeszenario noch nicht durch das Installationstool SWPM von SAP unterstützt, da Sie den Installationspfad **nicht nutzen** können:
>
>&lt;Produkt&gt; -> &lt;DBMS&gt; -> Installation -> Anwendung -> Anwendungsserver ABAP (oder Java) -> Hochverfügbarkeitssystem -> ...
>

### <a name="remove-sapmnt-and-create-saploc-file-share"></a>Entfernen von SAPMNT und Erstellen einer SAPLOC-Dateifreigabe

SWMP hat die lokale Dateifreigabe SAPMNT im Ordner „C:\\usr\\sap“ erstellt.

Entfernen Sie die SAPMNT Datenfreigabe auf **beiden** (A)SCS-Clusterknoten:

Führen Sie folgendes PowerShell-Skript aus:

```PowerShell
Remove-SmbShare sapmnt -ScopeName * -Force
 ```

Wenn die SAPLOC-Dateifreigabe nicht vorhanden ist, erstellen Sie eine auf BEIDEN ASCS-Clusterknoten.

Führen Sie folgendes PowerShell-Skript aus:

```PowerShell
#Create SAPLOC share and set security
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"
$HostName = $env:computername
$SAPLocalAdminGroupName = "$HostName\SAP_LocalAdmin"
$SAPDisk = "C:"
$SAPusrSapPath = "$SAPDisk\usr\sap"

New-SmbShare -Name saploc -Path c:\usr\sap -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName , $SAPLocalAdminGroupName  
 ```

## <a name="prepare-sap-global-host-on-sofs-cluster"></a>Vorbereiten des SAP GLOBAL HOST auf dem SOFS-Cluster

Erstellen Sie in diesem Schritt das folgenden Volume und die Dateifreigabe auf dem SOFS-Cluster:

* SAP GLOBALHOST-Dateistruktur C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS\ auf dem freigegebenen SOFS-Clustervolume (CSV)

* Erstellen der SAPMNT-Dateifreigabe

* Legen Sie die Sicherheit auf der SAPMNT-Dateifreigabe und dem Ordner mit voller Kontrolle für Folgendes fest:
    * Benutzergruppe **&lt;DOMAIN&gt;\SAP_&lt;SID&gt;_GlobalAdmin**
    * SAP-(A)SCS-Clusterknotencomputer-**Objekte &lt;DOMAIN&gt;\ClusterNode1$ und &lt;DOMAIN&gt;\ClusterNode2$**

Um ein CSV-Volume mit Spiegelresilienz zu erstellen, so wie im Kapitel **SAP Prerequisites for SOFS in Azure  - ADD LINK (SAP-Voraussetzungen für SOFS in Azure – Verknüpfung hinzufügen)** beschrieben, führen Sie folgendes PowerShell-Cmdlet auf einem der SOFS-Clusterknoten aus:


```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR1 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```
Um SAPMNT zu erstellen und die Ordner-und Dateifreigabesicherheit zu erstellen, führen Sie folgendes PowerShell-Skript auf einem der SOFS-Clusterknoten aus:

```PowerShell
# Create SAPMNT on file share
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP (A)SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP (A)SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Create SAPMNT file share and set share security
New-SmbShare -Name sapmnt -Path $UsrSAPFolder -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName, $ASCSClusterObjectNode1, $ASCSClusterObjectNode2 -ContinuouslyAvailable $false -CachingMode None -Verbose

# Get SAPMNT file share security settings
Get-SmbShareAccess sapmnt

# Set files & folder security
$Acl = Get-Acl $UsrSAPFolder

# Add file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
 ```
## <a name="stop-ascs-instances-and-sap-services"></a>Beenden der (A)SCS-Instanzen und SAP-Dienste

Führen Sie die folgenden Schritte aus:
* Beenden Sie SAP (A)SCS-Instanzen auf beiden (A)SCS-Clusterknoten.
* Beenden Sie SAP (A)SCS-Windows-Dienste **SAP&lt;SID&gt;_&lt;InstanceNumber&gt;** auf beiden Clusterknoten.

## <a name="move-sys-folder-to-sofs-cluster"></a>Verschieben des Ordners „\SYS\....“ auf den SOFS-Cluster

Führen Sie die folgenden Schritte aus:
* Kopieren Sie den SYS-Ordner (z.B. C:\usr\sap\\&lt;SID&gt;\SYS) aus einem Ihrer (A)SCS-Clusterknoten auf den SOFS-Cluster, z.B. zu C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS.
* Löschen Sie den Ordner C:\usr\sap\\&lt;SID&gt;\SYS von beiden (A)SCS-Clusterknoten.

## <a name="update-cluster-security-setting-on-sap-ascs-cluster"></a>Aktualisieren der Cluster-Sicherheitseinstellung auf dem SAP (A)SCS-Cluster

Führen Sie das folgende PowerShell-Skript auf einem der SAP (A)SCS-Clusterknoten aus:

```PowerShell
# Grant <DOMAIN>\SAP_<SID>_GlobalAdmin group access to cluster

$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# Set full access for <DOMAIn>\SAP_<SID>_GlobalAdmin group
Grant-ClusterAccess -User $SAPSIDGlobalAdminGroupName -Full

#Check security settings
Get-ClusterAccess
```

## <a name="create-a-virtual-host-name-for-the-clustered-sap-ascs-instance"></a>Erstellen eines virtuellen Hostnamens für die SAP (A)SCS-Clusterinstanz

Wie im Kapitel [Erstellen eines virtuellen Hostnamens für die SAP ASCS/SCS-Clusterinstanz][sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host] beschrieben, erstellen Sie einen SAP (A)SCS-Clusternetzwerknamen, z.B. **pr1-ascs [10.0.6.7]**.

## <a name="update-default-and-sap-ascs-instance-profile"></a>Aktualisieren des DEFAULT- und SAP (A)SCS-Instanzprofils

Sie müssen das DEFAULT- und SAP (A)SCS-Instanzprofil &lt;SID&gt;_(A)SCS<Nr>_<Host> aktualisieren, um Folgendes zu verwenden:

* Neuer virtueller SAP (A)SCS-Hostname

* Neuer SAP GLOBAL-Hostname


| Alte Werte |  |
| --- | --- |
| SAP (A) SCS-Hostname = globaler SAP-Host | ascs-1 |
| SAP (A) SCS-Instanzprofilname | PR1_ASCS00_ascs-1 |

| Neue Werte |  |
| --- | --- |
| SAP (A)SCS-Hostname | **pr1-ascs** |
| Globaler SAP-Host | **sapglobal** |
| SAP (A) SCS-Instanzprofilname | PR1\_ASCS00\_**pr1-ascs** |

### <a name="update-sap-default-profile"></a>Aktualisieren des SAP DEFAULT-Profils


| Parametername | Parameterwert |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| rdisp/mshost | **pr1-ascs** |
| enque/serverhost | **pr1-ascs** |

### <a name="update-sap-ascs-instance-profile"></a>Aktualisieren des SAP (A) SCS-Instanzprofils

| Parametername | Parameterwert |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| DIR_PROFILE | \\\\**sapglobal**\sapmnt\PR1\SYS\profile |
| _PF | $(DIR_PROFILE)\PR1\_ASCS00_ pr1-ascs |
| Restart_Program_02 = local$(_MS) pf=$(_PF) | **Start**_Program_02 = local$(_MS) pf=$(_PF) |
| SAPLOCALHOST | **pr1-ascs** |
| Restart_Program_03 = local$(_EN) pf=$(_PF) | **Start**_Program_03 = local$(_EN) pf=$(_PF) |
| gw/netstat_once | **0** |
| enque/encni/set_so_keepalive  | **true** |
| service/ha_check_node | **1** |

> [!IMPORTANT]
>Sie können das PowerShell-Cmdlet **Update-SAPASCSSCSProfile** verwenden, um das Profilupdate zu automatisieren.
>
>Das PowerShell-Cmdlet unterstützt jeweils die SAP ABAP ASCS- und SAP Java SCS-Instanz.
>

Kopieren Sie **SAPScripts.ps1** auf Ihr lokales Laufwerk C:\tmp, und führen Sie das folgende PowerShell-Cmdlet aus:

```PowerShell
Import-Module C:\tmp\SAPScripts.ps1

Update-SAPASCSSCSProfile -PathToAscsScsInstanceProfile \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_ascs-1 -NewASCSHostName pr1-ascs -NewSAPGlobalHostName sapglobal -Verbose  
```

![Abbildung 1: SAPScripts.ps1-Ausgabe][sap-ha-guide-figure-8012]

_**Abbildung 1:** SAPScripts.ps1-Ausgabe_

## <a name="update-ltsidgtadm-user-environment-variable"></a>Aktualisieren der Benutzerumgebungsvariable &lt;sid&gt;adm

Aktualisieren Sie den neuen GLOBAL HOST UNC-Pfad der &lt;sid&gt;adm-Benutzerumgebung auf BEIDEN (A)SCS-Clusterknoten.
Melden Sie sich als &lt;sid&gt;adm-Benutzer and, und starten Sie das Regedit.exe-Tool.
Wechseln Sie zur **HKEY_CURRENT_USER** -> **Umgebung**, und aktualisieren Sie die Variablen auf den neuen Wert:

| Variable | Wert |
| --- | --- |
| RSEC_SSFS_DATAPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\data |
| RSEC_SSFS_KEYPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\key |
| SAPEXE | \\\\**sapglobal**\sapmnt\PR1\SYS\exe\uc\NTAMD64 |
| SAPLOCALHOST  | **pr1-ascs** |


## <a name="install-new-saprcdll"></a>Installieren der neuen SAPRC. DLL

Sie müssen eine neue Version der SAP-Clusterressource installieren, die das Dateifreigabeszenario unterstützt.

Laden Sie das neueste **NTCLUST.SAR**-Paket aus dem SAP Service Market Place herunter.

Entpacken Sie NTCLUS.SAR auf einem der (A)SCS-Clusterknoten, und führen Sie den folgenden Befehl aus der Eingabeaufforderung aus, um eine neue „saprc.dll“ zu installieren:

```
.\NTCLUST\insaprct.exe -yes -install
```

Die neue „saprc.dll“ wird auf beiden (A)SCS-Clusterknoten installiert.

Weitere Informationen finden Sie unter [SAP Note 1596496 - How to update SAP Resource Type DLLs for Cluster Resource Monitor][ (SAP-Hinweis 1596496: Aktualisieren der SAP-Ressourcentyp-DLLs für die Clusterressourcenüberwachung)1596496].

## <a name="create-sap-sid-cluster-group-network-name-and-ip"></a>Erstellen des SAP <SID>-Clusternetzwerknamens, des Netzwerknamens und der IP-Adresse

Sie müssen Folgendes erstellen:

* Die SAP-&lt;SID&gt;-Clustergruppe
* <(A)SCSNetworkName>
* und die entsprechende IP-Adresse

Führen Sie das folgende PowerShell-Cmdlet aus:

```PowerShell
# Create SAP Cluster Group
$SAPSID = "PR1"
$SAPClusterGroupName = "SAP $SAPSID"
$SAPIPClusterResourceName = "SAP $SAPSID IP"
$SAPASCSNetworkName = "pr1-ascs"
$SAPASCSIPAddress = "10.0.6.7"
$SAPASCSSubnetMask = "255.255.255.0"

# Create SAP ASCS instance Virtual IP cluster resource
Add-ClusterGroup -Name $SAPClusterGroupName -Verbose

#Create SAP ASCS Virtual IP Address
$SAPIPClusterResource = Add-ClusterResource -Name $SAPIPClusterResourceName -ResourceType "IP Address" -Group $SAPClusterGroupName -Verbose

# Set static IP Address
$param1 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,Address,$SAPASCSIPAddress
$param2 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,SubnetMask,$SAPASCSSubnetMask
$params = $param1,$param2
$params | Set-ClusterParameter

# Create corresponding network name
$SAPNetworkNameClusterResourceName = $SAPASCSNetworkName
Add-ClusterResource -Name $SAPNetworkNameClusterResourceName -ResourceType "Network Name" -Group $SAPClusterGroupName -Verbose

# Set Network DNS Name
$SAPNetworkNameClusterResource = Get-ClusterResource $SAPNetworkNameClusterResourceName
$SAPNetworkNameClusterResource | Set-ClusterParameter -Name Name -Value $SAPASCSNetworkName

#Check the updated values
$SAPNetworkNameClusterResource | Get-ClusterParameter

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPNetworkNameClusterResourceName -Dependency "[$SAPIPClusterResourceName]" -Verbose

#Start SAP <SID> Cluster Group
Start-ClusterGroup -Name $SAPClusterGroupName -Verbose
```

## <a name="register-sap-start-service-on-both-nodes"></a>Registrieren des SAP START-Diensts auf BEIDEN Knoten

Sie müssen den SAP.(A)SCS-sapstart-Dienst erneut registrieren, um ihn an das neue Profil und den Profilpfad zu heften.

Sie müssen diesen auf BEIDEN (A)SCS-Clusterknoten ausführen.

Führen Sie von der Eingabeaufforderung mit erhöhten Rechten folgenden Befehl aus:

```
C:\usr\sap\PR1\ASCS00\exe\sapstartsrv.exe -r -p \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_pr1-ascs -s PR1 -n 00 -U SAPCLUSTER\SAPServicePR1 -P mypasswd12 -e SAPCLUSTER\pr1adm
```

![Abbildung 2: Neuinstallation des SAP-Diensts][sap-ha-guide-figure-8013]

_**Abbildung 2:** Neuinstallation des SAP-Diensts_

Stellt sicher, dass Parameter korrekt sind. Wählen Sie **Manuell** als Starttyp aus.

## <a name="stop-ascs-service"></a>Beenden des (A)SCS-Diensts

Beenden Sie den SAP (A)SCS-Dienst **SAP&lt;SID&gt;_&lt;InstanceNumber&gt;** auf beiden (A)SCS-Clusterknoten.

## <a name="create-new-sap-service-and-sap-instance-resources"></a>Erstellen eines neuen SAP-Diensts und neuer SAP-Instanzressourcen

Nun müssen Sie die Erstellung von Ressourcen der SAP-&lt;SID&gt;-Clustergruppe abschließen, z.B. müssen Sie Ressourcen erstellen:

* **SAP-&lt;SID&gt;-&lt;Instanznummer&gt;-Dienst** und
* **SAP-&lt;SID&gt;-&lt;Instanznummer&gt;-Instanz**

Führen Sie das folgende PowerShell-Cmdlet aus:

```PowerShell
$SAPSID = "PR1"
$SAPInstanceNumber = "00"
$SAPNetworkNameClusterResourceName = "pr1-ascs"

$SAPServiceName = "SAP$SAPSID"+ "_" + $SAPInstanceNumber

$SAPClusterGroupName = "SAP $SAPSID"
$SAPServiceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Service"

$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPServiceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Service" -SeparateMonitor -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name ServiceName -Value $SAPServiceName

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPNetworkNameClusterResourceName]" -Verbose

$SAPInstanceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Instance"

# Create SAP Instance cluster resource
$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPInstanceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Resource" -SeparateMonitor -Verbose

#Set SAP Instance cluster resource parameters
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystemName -Value $SAPSID -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystem -Value $SAPInstanceNumber -Verbose

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPServiceClusterResourceName]" -Verbose
```

## <a name="add-a-probe-port"></a>Hinzufügen eines Testports

In diesem Schritt konfigurieren Sie eine SAP-Clusterressource (SAP-SID-IP-Testport) mithilfe von PowerShell. Führen Sie diese Konfiguration auf einem der SAP ASCS/SCS-Clusterknoten, wie [hier][sap-high-availability-installation-wsfc-shared-disk-add-probe-port] beschrieben, aus.

## <a name="install-ers-instance-on-both-cluster-nodes"></a>Installieren der ERS-Instanz auf BEIDEN Clusterknoten

Im nächsten Schritt müssen Sie die ERS-Instanz (Enqueue Replication Server) auf beiden Knoten des (A)SCS-Clusters erstellen.
Die Installationsoption finden Sie im SWPM-Menü:

&lt;Produkt&gt; -> &lt;DBMS&gt; -> Installation -> zusätzliche SAP-Systeminstanzen -> **Einreihen der Replikationsserverinstanz in die Warteschlange**

## <a name="install-dbms-instance-and-sap-application-servers"></a>Installieren der DBMS-Instanz und von SAP-Anwendungsservern

Schließen sie die Installation Ihres SAP-Systems ab, indem Sie Folgendes installieren:
* DBMS-Instanz
* Primärer SAP-Anwendungsserver
* Zusätzlicher SAP-Anwendungsserver

## <a name="next-steps"></a>Nächste Schritte

* [Installation of an (A)SCS Instance on a Failover Cluster with no Shared Disks - Official SAP Guidelines for HA File Share (Installieren einer (A)SCS-Instanz auf einem Failovercluster ohne freigegebene Datenträger: Offizielle Richtlinien für die hoch verfügbare Dateifreigabe)][sap-official-ha-file-share-document]:

* [Direkte Speicherplätze in Windows Server 2016][s2d-in-win-2016]

* [Übersicht über Dateiserver mit horizontaler Skalierung für Anwendungsdaten][sofs-overview]

* [Neuerungen beim Speicher in Windows Server 2016][new-in-win-2016-storage]
