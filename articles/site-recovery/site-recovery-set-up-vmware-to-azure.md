---
title: Einrichten der Quellumgebung (VMware in Azure) | Microsoft Docs
description: Dieser Artikel beschreibt, wie Sie Ihre lokale Umgebung einrichten, um mit dem Replizieren von virtuellen VMware-Computern in Azure zu beginnen.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: anoopkv
ms.openlocfilehash: 2b6b0e5cc95f28b5596e7e898f5f035e3647d9c5
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Einrichten der Quellumgebung (VMware in Azure)
> [!div class="op_single_selector"]
> * [VMware zu Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Physisch nach Azure](./site-recovery-set-up-physical-to-azure.md)

In diesem Artikel wird beschrieben, wie Sie Ihre lokale Quellumgebung einrichten, um virtuelle Computer zu replizieren, die unter „VMware zu Azure“ ausgeführt werden. Er enthält die Schritte zum Auswählen Ihres Replikationsszenarios, Einrichten eines lokalen Computers als Site Recovery-Konfigurationsserver und automatischen Ermitteln von lokalen VMs. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie Folgendes bereits durchgeführt haben:
- [Einrichten von Ressourcen](tutorial-prepare-azure.md) im [Azure-Portal](http://portal.azure.com)
- [Vorbereiten lokaler VMware-Server](tutorial-prepare-on-premises-vmware.md) mit Angabe eines dedizierten Kontos für die automatische Ermittlung



## <a name="choose-your-protection-goals"></a>Auswählen der Schutzziele

1. Navigieren Sie im Azure-Portal zum Blatt **Recovery Services-Tresor**, und wählen Sie den Tresor aus.
2. Navigieren Sie im Ressourcenmenü des Tresors zu **Erste Schritte** > **Site Recovery** > **Schritt 1: Vorbereiten der Infrastruktur** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with VMware vSphere Hypervisor** (Ja, mit VMware vSphere-Hypervisor) aus. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Einrichten des Konfigurationsservers

Sie richten den Konfigurationsserver als lokale VMware-VM ein und verwenden eine OVF-Vorlage (Open Virtualization Format). [Erfahren Sie mehr](concepts-vmware-to-azure-architecture.md) zu den Komponenten, die auf der VMware-VM installiert werden. 

1. Informieren Sie sich über die [Voraussetzungen](how-to-deploy-configuration-server.md#prerequisites) für die Bereitstellung des Konfigurationsservers. [Überprüfen Sie die Kapazität](how-to-deploy-configuration-server.md#capacity-planning) für die Bereitstellung.
2. Führen Sie den [Download](how-to-deploy-configuration-server.md#download-the-template) und [Import](how-to-deploy-configuration-server.md#import-the-template-in-vmware) der OVF-Vorlage durch (how-to-deploy-configuration-server.md), um eine lokale VMware-VM einzurichten, auf der der Konfigurationsserver ausgeführt wird.
3. Aktivieren Sie die VMware-VM, und [registrieren](how-to-deploy-configuration-server.md#register-the-configuration-server) Sie sie im Recovery Services-Tresor.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Fügen Sie das VMware-Konto hinzu, das für die automatische Ermittlung verwendet wird.

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Herstellen einer Verbindung mit dem VMware-Server

Damit Azure Site Recovery virtuelle Computern in Ihrer lokalen Umgebung ermitteln kann, müssen Sie den VMware vCenter Server- oder vSphere ESXi-Host mit Site Recovery verbinden.

Klicken Sie auf **+vCenter**, um eine Verbindung mit einem VMware vCenter Server- oder VMware vSphere ESXi-Host herzustellen.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Häufige Probleme
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Nächste Schritte
[Einrichten Ihrer Zielumgebung](./site-recovery-prepare-target-vmware-to-azure.md) in Azure
