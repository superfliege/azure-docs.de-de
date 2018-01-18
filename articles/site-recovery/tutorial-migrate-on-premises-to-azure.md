---
title: Migrieren von lokalen Computern mit Azure Site Recovery zu Azure | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie Sie lokale Computer mithilfe von Azure Site Recovery zu Azure migrieren."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ee9397406cbca21d8bd53019d9daac5a037f508c
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrieren von lokalen Computern zu Azure

Zusätzlich zur Verwendung des Diensts [Azure Site Recovery](site-recovery-overview.md) zur Verwaltung und Orchestrierung der Notfallwiederherstellung von lokalen Computern und Azure-VMs für die BCDR-Zwecke (Business Continuity/Disaster Recovery, Geschäftskontinuität und Notfallwiederherstellung) können Sie Site Recovery auch zur Verwaltung der Migration von lokalen Computern in Azure verwenden.


In diesem Tutorial wird veranschaulicht, wie lokale VMs und physische Server in Azure migriert werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Auswählen eines Replikationsziels
> * Einrichten der Quell- und Zielumgebung
> * Einrichten einer Replikationsrichtlinie
> * Aktivieren der Replikation
> * Führen Sie eine Testmigration aus, um sicherzustellen, dass alles wie erwartet funktioniert.
> * Ausführen eines einmaligen Failovers in Azure

Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:

1. [Prepare Azure](tutorial-prepare-azure.md) (Vorbereiten von Azure)
2. Bereiten Sie lokale [VMware](tutorial-prepare-on-premises-vmware.md)- oder Hyper-V-Server vor.

Bevor Sie beginnen, empfiehlt sich eine Überprüfung der [VMware](concepts-vmware-to-azure-architecture.md)- oder [Hyper-V](concepts-hyper-v-to-azure-architecture.md)-Architektur für die Notfallwiederherstellung.


## <a name="prerequisites"></a>Voraussetzungen

Von paravirtualisierten Treibern exportierte Geräte werden nicht unterstützt.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

1. Melden Sie sich unter [Azure-Portal](https://portal.azure.com) > **Recovery Services** an.
2. Klicken Sie auf **Neu** > **Überwachung und Verwaltung** > **Backup und Site Recovery**.
3. Geben Sie in **Name** den Anzeigenamen **ContosoVMVault** ein. Wenn Sie mehrere Abonnements haben, wählen Sie das gewünschte aus.
4. Erstellen Sie die Ressourcengruppe **ContosoRG**.
5. Geben Sie eine Azure-Region an. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Um schnell über das Dashboard auf den Tresor zuzugreifen, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

   ![Neuer Tresor](./media/tutorial-migrate-on-premises-to-azure/onprem-to-azure-vault.png)

Der neue Tresor wird dem **Dashboard** unter **Alle Ressourcen** und der Hauptseite **Recovery Services-Tresore** hinzugefügt.



## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.
1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie in **Schutzziel** aus, was Sie migrieren möchten.
    - **VMware**: Wählen Sie **To Azure** (Zu Azure) > **Yes, with VMWare vSphere Hypervisor** (Ja, mit VMware vSphere Hypervisor) aus.
    - **Physischer Computer**: Wählen Sie **To Azure** (Zu Azure) > **Nicht virtualisiert/Andere** aus.
    - **Hyper-V**: Wählen Sie **To Azure** (Zu Azure) > **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. Wenn Hyper-V-VMs von VMM verwaltet werden, wählen Sie **Ja** aus.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

- [Einrichten](tutorial-vmware-to-azure.md#set-up-the-source-environment) der Quellumgebung für VMware-VMs.
- [Einrichten](tutorial-physical-to-azure.md#set-up-the-source-environment) der Quellumgebung für physische Server.
- [Einrichten](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) der Quellumgebung für Hyper-V-VMs.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie das Ressourcen-Manager-Bereitstellungsmodell an.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

- [Richten Sie eine Replikationsrichtlinie ](tutorial-vmware-to-azure.md#create-a-replication-policy) für VMware-VMs ein.
- [Richten Sie eine Replikationsrichtlinie](tutorial-physical-to-azure.md#create-a-replication-policy) für physische Server ein.
- [Richten Sie eine Replikationsrichtlinie](tutorial-hyper-v-to-azure.md#set-up-a-replication-policy) für Hyper-V-VMs ein.


## <a name="enable-replication"></a>Aktivieren der Replikation

- [Aktivieren Sie die Replikation](tutorial-vmware-to-azure.md#enable-replication) für VMware-VMs.
- [Aktivieren Sie die Replikation](tutorial-physical-to-azure.md#enable-replication) für physische Server.
- [Aktivieren Sie die Replikation](tutorial-hyper-v-to-azure.md#enable-replication) für Hyper-V-VMs.


## <a name="run-a-test-migration"></a>Ausführen einer Testmigration

Führen Sie ein [Testfailover](tutorial-dr-drill-azure.md) in Azure aus, um sicherzustellen, dass alles wie erwartet funktioniert.


## <a name="migrate-to-azure"></a>Migrieren zu Azure

Führen Sie ein Failover für die zu migrierenden Computer aus.

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** auf den Computer > **Failover**.
2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Wählen Sie den letzten Wiederherstellungspunkt aus.
3. Die Einstellung für den Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
4. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
5. Überprüfen Sie, ob die Azure-VM wie erwartet in Azure angezeigt wird.
6. Klicken Sie in **Replizierte Elemente** mit der rechten Maustaste auf die VM > **Migration abschließen**. Dadurch wird der Migrationsvorgang abgeschlossen, die Replikation für die VM wird beendet, und die Site Recovery-Abrechnung für die VM wird eingestellt.

    ![Abschließen der Migration](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. Bei physischen Servern, VMware-Linux-Computern, VMware-VMs ohne aktivierten DHCP-Dienst und VMware-VMs mit den folgenden Starttreibern kann das Testfailover länger dauern: storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie lokale VMs in Azure-VMs migriert. Nun können Sie die Notfallwiederherstellung für die Azure-VMs konfigurieren.

> [!div class="nextstepaction"]
> [Richten Sie die Notfallwiederherstellung](site-recovery-azure-to-azure-after-migration.md) für Azure-VMs nach der Migration von einem lokalen Standort in Azure ein.
