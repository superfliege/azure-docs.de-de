---
title: Einrichten der Quellumgebung für die VMware-Replikation in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Einrichtung Ihrer lokalen Umgebung für das Replizieren von virtuellen VMware-Computern in Azure mithilfe von Azure Site Recovery.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: b2c564e8d49e39d9cdc09d3fe168388d579de70e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Einrichten der Quellumgebung für die Replikation von VMware in Azure

Dieser Artikel beschreibt die Einrichtung Ihrer lokalen Quellumgebung für das Replizieren von virtuellen VMware-Computern in Azure. Er enthält die Schritte zum Auswählen Ihres Replikationsszenarios, Einrichten eines lokalen Computers als Site Recovery-Konfigurationsserver und automatischen Ermitteln von lokalen VMs. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie Folgendes bereits durchgeführt haben:
- [Einrichten von Ressourcen](tutorial-prepare-azure.md) im [Azure-Portal](http://portal.azure.com)
- [Vorbereiten lokaler VMware-Server](vmware-azure-tutorial-prepare-on-premises.md) mit Angabe eines dedizierten Kontos für die automatische Ermittlung



## <a name="choose-your-protection-goals"></a>Auswählen der Schutzziele

1. Navigieren Sie im Azure-Portal zum Blatt **Recovery Services-Tresor**, und wählen Sie den Tresor aus.
2. Navigieren Sie im Ressourcenmenü des Tresors zu **Erste Schritte** > **Site Recovery** > **Schritt 1: Vorbereiten der Infrastruktur** > **Schutzziel**.

    ![Ziele wählen](./media/vmware-azure-set-up-source/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with VMware vSphere Hypervisor** (Ja, mit VMware vSphere-Hypervisor) aus. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Einrichten des Konfigurationsservers

Sie richten den Konfigurationsserver als lokale VMware-VM ein und verwenden eine OVF-Vorlage (Open Virtualization Format). [Erfahren Sie mehr](concepts-vmware-to-azure-architecture.md) zu den Komponenten, die auf der VMware-VM installiert werden. 

1. Informieren Sie sich über die [Voraussetzungen](vmware-azure-deploy-configuration-server.md#prerequisites) für die Bereitstellung des Konfigurationsservers.
2. [Überprüfen Sie die Kapazität](vmware-azure-deploy-configuration-server.md#capacity-planning) für die Bereitstellung.
3. Führen Sie den [Download](vmware-azure-deploy-configuration-server.md#download-the-template) und [Import](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) der OVF-Vorlage durch (how-to-deploy-configuration-server.md), um eine lokale VMware-VM einzurichten, auf der der Konfigurationsserver ausgeführt wird.
4. Aktivieren Sie die VMware-VM, und [registrieren](vmware-azure-deploy-configuration-server.md#register-the-configuration-server) Sie sie im Recovery Services-Tresor.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Fügen Sie das VMware-Konto hinzu, das für die automatische Ermittlung verwendet wird.

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Herstellen einer Verbindung mit dem VMware-Server

Damit Azure Site Recovery virtuelle Computern in Ihrer lokalen Umgebung ermitteln kann, müssen Sie den VMware vCenter Server- oder vSphere ESXi-Host mit Site Recovery verbinden.

Klicken Sie auf **+vCenter**, um eine Verbindung mit einem VMware vCenter Server- oder VMware vSphere ESXi-Host herzustellen.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Häufige Probleme
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Nächste Schritte
[Einrichten Ihrer Zielumgebung](./vmware-azure-set-up-target.md) in Azure
