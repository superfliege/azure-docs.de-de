---
title: Gruppieren einer SAP (A)SCS-Instanz im Windows-Failovercluster per Dateifreigabe in Azure | Microsoft-Dokumentation
description: Gruppieren einer SAP (A)SCS-Instanz im Windows-Failovercluster per Dateifreigabe
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 472c3f35e2ae32550be62826407689f93101041f
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2492395]:https://launchpad.support.sap.com/#/notes/2492395

[kb4025334]:https://support.microsoft.com/help/4025334/windows-10-update-kb4025334

[dv2-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dv2-series
[ds-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#ds-series

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-volumes-s2d-choosing-filesystem]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-filesystem
[choosing-the-size-of-volumes-s2d]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-size-of-volumes
[deploy-sofs-s2d-in-azure]:https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment
[s2d-in-win-2016]:https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[deep-dive-volumes-in-s2d]:https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

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


# <a name="clustering-sap-ascs-instance-on-windows-failover-cluster-using-file-share-on-azure"></a>Gruppieren einer SAP (A)SCS-Instanz im Windows-Failovercluster per Dateifreigabe in Azure

> ![Windows][Logo_Windows] Windows
>

Windows Server-Failoverclustering ist die Grundlage für eine SAP ASCS/SCS-Installation und ein DBMS in Windows mit hoher Verfügbarkeit.

Bei einem Failovercluster handelt es sich um eine Gruppe von 1+n unabhängigen Servern, die zur Steigerung der Verfügbarkeit von Anwendungen und Diensten zusammenarbeiten. Wenn ein Knotenfehler auftritt, berechnet das Windows Server-Failoverclustering die Anzahl von Fehlern, die auftreten können, während trotzdem ein fehlerfreier Cluster für die Bereitstellung der Anwendungen und Dienste aufrechterhalten wird. Sie können zwischen verschiedenen Quorummodi wählen, um das Failoverclustering zu erzielen.

## <a name="prerequisite"></a>Voraussetzung
Sehen Sie sich die folgenden Dokumente an, bevor Sie mit dem Durcharbeiten dieses Dokuments beginnen:

* [Azure Virtual Machines High Availability Architecture and Scenarios for SAP NetWeaver][sap-high-availability-architecture-scenarios] (Azure Virtual Machines-Architektur für Hochverfügbarkeit und Szenarien für SAP NetWeaver)

> [!IMPORTANT]
>Das Gruppieren von SAP (A)SCS-Instanzen per Dateifreigabe wird für **SAP NetWeaver 7.40-Produkte (und höher)** mit **SAP Kernel 7.49 (und höher)** unterstützt.
>


## <a name="windows-server-failover-clustering-in-azure"></a>Windows Server-Failoverclustering in Azure

Im Vergleich mit Bare-Metal- oder privaten Cloudbereitstellungen sind für virtuelle Azure-Computer zusätzliche Schritte erforderlich, um Windows Server-Failoverclustering zu konfigurieren. Wenn Sie einen Cluster erstellen, müssen Sie mehrere IP-Adressen und virtuelle Hostnamen für die SAP ASCS/SCS-Instanz festlegen.

### <a name="name-resolution-in-azure-and-cluster-virtual-host-name"></a>Namensauflösung in Azure und Name des virtuellen Hosts für den Cluster

Die Azure-Cloudplattform bietet keine Möglichkeit, virtuelle IP-Adressen, z.B. Floating IP-Adressen, zu konfigurieren. Sie benötigen eine alternative Lösung für die Einrichtung einer virtuellen IP-Adresse, um die Clusterressource in der Cloud zu erreichen. Azure stellt über den Azure Load Balancer-Dienst einen **internen Lastenausgleich** zur Verfügung. Mit dem internen Load Balancer erreichen Clients den Cluster über die virtuelle IP-Adresse des Clusters. Sie müssen den internen Load Balancer in der Ressourcengruppe bereitstellen, die die Clusterknoten enthält. Anschließend konfigurieren Sie alle erforderlichen Portweiterleitungsregeln mithilfe der Testports des internen Load Balancers. Die Clients können über den virtuellen Hostnamen eine Verbindung herstellen. Der DNS-Server löst die IP-Adresse des Clusters auf. Der interne Load Balancer übernimmt die Weiterleitung an den aktiven Knoten des Clusters.

![Abbildung 1: Windows Server-Failoverclusteringkonfiguration in Azure ohne freigegebenen Datenträger][sap-ha-guide-figure-1001]

_**Abbildung 1:** Windows Server-Failoverclusteringkonfiguration in Azure ohne freigegebenen Datenträger_

## <a name="sap-ascs-ha-with-file-share"></a>SAP (A)SCS HA mit Dateifreigabe

SAP hat neue Ansätze und Alternativen zum Gruppieren von freigegebenen Datenträgern entwickelt, um eine Gruppierung von SAP (A)SCS-Instanzen in einem Windows-Failovercluster zu ermöglichen.

Hierbei ist die Verwendung einer **SMB-Dateifreigabe** eine Option zum Bereitstellen von **SAP GLOBAL HOST-Dateien**.

> [!NOTE]
>Eine SMB-Dateifreigabe ist eine weitere Option zum Gruppieren von freigegebenen Datenträgern, um die Gruppierung von SAP (A)SCS-Instanzen zu ermöglichen.  
>

Für diese Architektur gilt Folgendes:

* **SAP Central Services (mit eigener Dateistruktur und Prozessen für Nachrichten und das Einreihen in die Warteschlange) sind von SAP GLOBAL HOST-Dateien getrennt.**
* **SAP Central Services werden unter einer SAP (A)SCS-Instanz ausgeführt.**
* Die SAP (A)SCS-Instanz ist gruppiert und über den Namen des virtuellen Hosts **<(A)SCSVirtualHostName>** zugänglich.
* SAP GLOBAL-Dateien sind auf der SMB-Dateifreigabe angeordnet, und es wird darauf zugegriffen, indem der folgende <SAPGLOBALHost>-Hostname verwendet wird: \\\\&lt;SAPGLOBALHost&gt;\sapmnt\\&lt;SID&gt;\SYS\...
* Die SAP (A)SCS-Instanz wird auf einem lokalen Datenträger auf beiden Clusterknoten installiert.
* Der Netzwerkname **<(A)SCSVirtualHostName>** unterscheidet sich von **<SAPGLOBALHost>**.

![Abbildung 2: Neue SAP (A)SCS HA-Architektur mit SMB-Dateifreigabe][sap-ha-guide-figure-8004]

_**Abbildung 2:** Neue SAP (A)SCS HA-Architektur mit SMB-Dateifreigabe_

Voraussetzungen für die SMB-Dateifreigabe:

* Protokoll SMB 3.0 (oder höher)
* Möglichkeit zum Festlegen von Active Directory-Zugriffssteuerungslisten (AD ACLs) für **AD-Benutzergruppen** und **Computerobjekt-Computer**
* Für die Dateifreigabe muss HA aktiviert sein:
    * Zum Speichern von Dateien verwendete Datenträger dürfen kein Single Point of Failure sein.
    * Sie müssen sicherstellen, dass Ausfälle von Servern bzw. VMs nicht zu einem Ausfall der Dateifreigabe führen.

Die **SAP &lt;SID&gt;**-Clusterrolle enthält jetzt keine freigegebenen Clusterdatenträger und keine Clusterressource der allgemeinen Dateifreigabe (Generic File Share).


![Abbildung 3: SAP <SID>-Clusterrollenressourcen bei Verwendung einer Dateifreigabe][sap-ha-guide-figure-8005]

_**Abbildung 3:** **SAP &lt;SID&gt;**-Clusterrollenressourcen bei Verwendung einer Dateifreigabe_


## <a name="scale-out-file-share-sofs-with-storage-spaces-direct-s2d-on-azure-as-sapmnt-file-share"></a>Dateifreigabe für horizontales Hochskalieren (Scale-Out File Share, SOFS) mit „Direkte Speicherplätze“ (Storage Spaces Direct, S2D) in Azure als SAPMNT-Dateifreigabe

Sie können SOFS nutzen, um SAP GLOBAL HOST-Dateien zu hosten und zu schützen und einen hoch verfügbaren SAPMNT-Dateifreigabendienst anzubieten.

![Abbildung 4: SOFS-Dateifreigabe zum Schützen von SAP GLOBAL HOST-Dateien][sap-ha-guide-figure-8006]

_**Abbildung 4:** SOFS-Dateifreigabe zum Schützen von SAP GLOBAL HOST-Dateien_

> [!IMPORTANT]
>SOFS-Dateifreigaben werden in der Microsoft Azure-Cloud und in lokalen Umgebungen vollständig unterstützt.
>

**SOFS** ermöglicht eine SAPMNT-Dateifreigabe, die hoch verfügbar ist und horizontal skaliert werden kann.

**Direkte Speicherplätze (Storage Spaces Direct, S2D)** wird als **freigegebener Datenträger** für SOFS verwendet und ermöglicht das Erstellen von hoch verfügbarem und skalierbarem Speicher, indem Server mit lokalem Speicher verwendet werden. Daher ist freigegebener Speicher, der für SOFS verwendet wird, z.B. für SAP GLOBAL HOST-Dateien, kein Single Point of Failure (SPOF).

> [!IMPORTANT]
>Wenn Sie die Einrichtung der Notfallwiederherstellung planen, ist SOFS die empfohlene Lösung für hoch verfügbare Dateifreigaben in Azure.
>

### <a name="sap-prerequisites-for-sofs-in-azure"></a>SAP-Voraussetzungen für SOFS in Azure

Für SOFS benötigen Sie Folgendes:

* Mindestens zwei Clusterknoten für SOFS.

* Jeder Knoten muss mindestens zwei lokale Datenträger aufweisen.

* Aus Leistungsgründen müssen Sie die **Spiegelungsresilienz** nutzen:
    * **2-Wege**-Spiegelung für SOFS mit zwei Clusterknoten
    * **3-Wege**-Spiegelung für SOFS mit drei (oder mehr) Clusterknoten


* **Es wird empfohlen, für SOFS mit 3-Wege-Spiegelung mindestens drei Clusterknoten zu nutzen**.
Diese Einrichtung bietet eine bessere Skalierbarkeit und Speicherresilienz als die SOFS-Einrichtung mit zwei Clusterknoten und 2-Wege-Spiegelung.

* Sie müssen einen **Azure-Premium-Datenträger** verwenden.

* Es wird **empfohlen**, **Azure Managed Disks** zu nutzen.

* Es wird **empfohlen**, Volumes mit dem neuen **Dateisystem ReFS (Resilient File System)** zu formatieren.
    * [SAP Note 1869038 - SAP support for ReFs filesystem][1869038] (SAP-Hinweis 1869038 – SAP-Unterstützung für ReFS-Dateisystem)
    * Siehe [Auswahl des Dateisystems][planning-volumes-s2d-choosing-filesystem] unter „Planen von Volumes in Direkte Speicherplätze“.
    * Achten Sie darauf, dieses [kumulative MS-Update **KB4025334**][kb4025334] zu installieren.


* Sie können als Azure-VM-Größen die **DS-Serie** oder die **DSv2-Serie** verwenden.

* Um eine gute VM-Netzwerkleistung zu erzielen, die für die Datenträgersynchronisierung von „Direkte Speicherplätze“ erforderlich ist, sollten Sie einen VM-Typ verwenden, der mindestens über eine **„hohe“ Netzwerkbandbreite** verfügt.
Weitere Informationen finden Sie unter den Spezifikationen zur [DSv2-Serie][dv2-series] und [DS-Serie][ds-series].

* Es wird **empfohlen**, **einen Teil der Kapazität im Speicherpool nicht zuzuordnen und zu reservieren**. So können Volumes nach dem Ausfall von Datenträgern „an Ort und Stelle“ Reparaturen durchführen, um die Datensicherheit und die Leistung zu verbessern.

 Weitere Informationen finden Sie unter [Auswählen der Volumegröße][choosing-the-size-of-volumes-s2d].


* SOFS-Azure-VMs müssen in einer **eigenen Azure-Verfügbarkeitsgruppe** bereitgestellt werden.

* Es ist nicht erforderlich, den internen Azure Load Balancer für den Netzwerknamen der SOFS-Dateifreigabe zu konfigurieren, z.B. <SAPGlobalHostName>. Dies wird für den <(A)SCSVirtualHostname> der SAP (A)SCS-Instanz oder für das DBMS durchgeführt. SOFS führt für die Last das horizontale Hochskalieren über alle Clusterknoten hinweg durch, sodass für <SAPGlobalHostName> die lokale IP aller Clusterknoten verwendet wird.


> [!IMPORTANT]
>Die SAPMNT-Dateifreigabe, die auf SAPGLOBALHOST zeigt, kann nicht geändert werden. SAP unterstützt keinen anderen Freigabenamen als „sapmnt“.
>[SAP Note 2492395 - Can the share name sapmnt be changed?][2492395] (SAP-Hinweis 2492395 – Kann der Freigabename „sapmnt“ geändert werden?)

### <a name="configuring-sap-ascs-instances-and-sofs-in-two-clusters"></a>Konfigurieren von SAP (A)SCS-Instanzen und SOFS in zwei Clustern

Sie können SAP (A)SCS-Instanzen auch in einem Cluster mit einer eigenen SAP-<SID>-Clusterrolle bereitstellen. Die SOFS-Dateifreigabe wird in einem anderen Cluster mit einer anderen Clusterrolle konfiguriert.

> [!IMPORTANT]
>In diesem Szenario wird die SAP (A)SCS-Instanz so konfiguriert, dass sie über den UNC-Pfad \\\\&lt;SAPGLOBALHost&gt;\sapmnt\\&lt;SID&gt;\SYS\. auf den SAP GLOBAL HOST zugreifen kann.
>

![Abbildung 5: Bereitstellung der SAP (A)SCS-Instanz und von SOFS in ZWEI Clustern][sap-ha-guide-figure-8007]

_**Abbildung 5:** Bereitstellung der SAP (A)SCS-Instanz und von SOFS in ZWEI Clustern_

> [!IMPORTANT]
>In der Azure-Cloud muss jeder Cluster, der für SAP und für SOFS-Dateifreigaben verwendet wird, in einer eigenen Azure-Verfügbarkeitsgruppe bereitgestellt werden. So wird sichergestellt, dass diese Cluster-VMs auf die gesamte zugrunde liegende Azure-Infrastruktur verteilt sind.
>

## <a name="generic-file-share-with-sios-as-cluster-shared-disks"></a>Allgemeine Dateifreigabe mit SIOS als freigegebene Clusterdatenträger


> [!IMPORTANT]
>SOFS ist die empfohlene Lösung für Dateifreigaben mit hoher Verfügbarkeit.
>
>Aber wenn Sie planen, für Ihre hoch verfügbare Dateifreigabe auch die **Notfallwiederherstellung** einzurichten, müssen Sie die allgemeine Dateifreigabe und SIOS als Technologie für freigegebene Clusterdatenträger verwenden.
>

Die allgemeine Dateifreigabe ist eine weitere Option, um eine hoch verfügbare Dateifreigabe zu erzielen.

Hier können Sie als freigegebenen Clusterdatenträger eine SIOS-Lösung eines Drittanbieters nutzen.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Infrastructure Preparation for SAP HA using Windows Failover Cluster and File Share for SAP (A)SCS Instance][sap-high-availability-infrastructure-wsfc-file-share] (Vorbereitung der Azure-Infrastruktur für SAP HA mithilfe des Windows-Failoverclusters und der Dateifreigabe für die SAP (A)SCS-Instanz)

* [SAP NetWeaver HA-Installation auf dem Windows-Failovercluster und die Dateifreigabe für die SAP (A)SCS-Instanz][sap-high-availability-installation-wsfc-shared-disk]

* [Deploy a two-node Storage Spaces Direct scale-out file server for UPD storage in Azure][deploy-sofs-s2d-in-azure] (Bereitstellen eines Dateiservers (Direkte Speicherplätze) mit horizontaler Skalierung und zwei Knoten für UPD-Speicherung in Azure)

* [Direkte Speicherplätze in Windows Server 2016][s2d-in-win-2016]

* [Deep Dive: Volumes in Storage Spaces Direct][deep-dive-volumes-in-s2d] (Ausführliche Betrachtung: Volumes in Direkte Speicherplätze)
