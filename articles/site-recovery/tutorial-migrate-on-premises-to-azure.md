---
title: Migrieren von lokalen Computern mit Azure Site Recovery zu Azure | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie Sie lokale Computer mithilfe von Azure Site Recovery zu Azure migrieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: cfd44f7f06faa7d1d00efa9427dbf5d1d0a89ef1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrieren von lokalen Computern zu Azure

Der [Azure Site Recovery](site-recovery-overview.md)-Dienst verwaltet und koordiniert Replikation, Failover und Failback von lokalen Computern sowie virtuellen Azure-Computern (VMs).

In diesem Tutorial wird veranschaulicht, wie lokale VMs und physische Server mit Site Recovery zu Azure migriert werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten der Voraussetzungen für die Bereitstellung
> * Erstellen eines Recovery Services-Tresors für Site Recovery
> * Bereitstellen von lokalen Verwaltungsservern
> * Einrichten einer Replikationsrichtlinie und Aktivieren der Replikation
> * Durchführen eines Notfallwiederherstellungsverfahrens, um die ordnungsgemäße Funktion sicherzustellen
> * Ausführen eines einmaligen Failovers in Azure

## <a name="overview"></a>Übersicht

Sie migrieren einen Computer, indem Sie die Replikation für den Computer aktivieren und ein Failover in Azure ausführen.


## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Schritte müssen für dieses Tutorial ausgeführt werden:

- [Vorbereiten](tutorial-prepare-azure.md) von Azure-Ressourcen, einschließlich eines Azure-Abonnements, eines virtuellen Azure-Netzwerks und eines Speicherkontos.
- [Vorbereiten](tutorial-prepare-on-premises-vmware.md) von lokalen VMware-Servern und -VMs.
- Beachten Sie, das von paravirtualisierten Treibern exportierte Geräte nicht unterstützt werden.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.
1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie in **Schutzziel** Folgendes aus:
    - **VMware**: Wählen Sie **To Azure** (Zu Azure) > **Yes, with VMWare vSphere Hypervisor** (Ja, mit VMware vSphere Hypervisor) aus.
    - **Physischer Computer**: Wählen Sie **To Azure** (Zu Azure) > **Nicht virtualisiert/Andere** aus.
    - **Hyper-V**: Wählen Sie **To Azure** (Zu Azure) > **Yes, with Hyper-V** (Ja, mit Hyper-V) aus.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

- [Einrichten](tutorial-vmware-to-azure.md#set-up-the-source-environment) der Quellumgebung für VMware-VMs.
- [Einrichten](tutorial-physical-to-azure.md#set-up-the-source-environment) der Quellumgebung für physische Server.
- [Einrichten](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) der Quellumgebung für Hyper-V-VMs.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie das Ziel-Bereitstellungsmodell an.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

- [Richten Sie eine Replikationsrichtlinie ](tutorial-vmware-to-azure.md#create-a-replication-policy) für VMware-VMs ein.


## <a name="enable-replication"></a>Replikation aktivieren

- [Aktivieren Sie die Replikation](tutorial-vmware-to-azure.md#enable-replication) für VMware-VMs.


## <a name="run-a-test-migration"></a>Ausführen einer Testmigration

Führen Sie ein [Testfailover](tutorial-dr-drill-azure.md) in Azure aus, um sicherzustellen, dass alles wie erwartet funktioniert.


## <a name="migrate-to-azure"></a>Migrieren zu Azure

Führen Sie ein Failover für die zu migrierenden Computer aus.

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** auf den Computer > **Failover**.
2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Wählen Sie den letzten Wiederherstellungspunkt aus.
3. Die Einstellung für den Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
4. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
5. Überprüfen Sie, ob die Azure-VM wie erwartet in Azure angezeigt wird.
6. Klicken Sie in **Replizierte Elemente** mit der rechten Maustaste auf die VM > **Migration abschließen**. Dadurch wird der Migrationsvorgang abgeschlossen, die Replikation für die VM wird beendet, und die Site Recovery-Abrechnung für die VM wird eingestellt.

    ![Abschließen der Migration](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. Bei physischen Servern, VMware-Linux-Computern, VMware-VMs ohne aktivierten DHCP-Dienst und VMware-VMs mit den folgenden Starttreibern kann das Testfailover länger dauern: storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Replizieren von Azure-VMs nach dem Migrieren in Azure in einer anderen Region](site-recovery-azure-to-azure-after-migration.md)
