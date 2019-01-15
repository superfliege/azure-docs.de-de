---
title: Migrieren von lokalen Computern mit Azure Site Recovery zu Azure | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie lokale Computer mithilfe von Azure Site Recovery zu Azure migrieren.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 4ba25da1f356f6164137dead467bd8bc948ce3d7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037836"
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

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. Bereiten Sie lokale [VMware](vmware-azure-tutorial-prepare-on-premises.md)- oder [Hyper-V](hyper-v-prepare-on-premises-tutorial.md)-Server vor.

Bevor Sie beginnen, empfiehlt sich eine Überprüfung der [VMware](vmware-azure-architecture.md)- oder [Hyper-V](hyper-v-azure-architecture.md)-Architektur für die Notfallwiederherstellung.


## <a name="prerequisites"></a>Voraussetzungen

Von paravirtualisierten Treibern exportierte Geräte werden nicht unterstützt.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

1. Melden Sie sich unter [Azure-Portal](https://portal.azure.com) > **Recovery Services** an.
2. Klicken Sie auf **Ressource erstellen** > **Verwaltungstools** > **Backup & Site Recovery** aus.
3. Geben Sie in **Name** den Anzeigenamen **ContosoVMVault** ein. Wenn Sie mehrere Abonnements haben, wählen Sie das gewünschte aus.
4. Erstellen Sie die Ressourcengruppe **ContosoRG**.
5. Geben Sie eine Azure-Region an. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Um schnell über das Dashboard auf den Tresor zuzugreifen, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

   ![Neuer Tresor](./media/migrate-tutorial-on-premises-azure/onprem-to-azure-vault.png)

Der neue Tresor wird dem **Dashboard** unter **Alle Ressourcen** und der Hauptseite **Recovery Services-Tresore** hinzugefügt.



## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.
1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie in **Schutzziel** aus, was Sie migrieren möchten.
    - **VMware**: Wählen Sie **To Azure** (Zu Azure) > **Ja, mit VMware vSphere-Hypervisor**.
    - **Physischer Computer:** Wählen Sie **To Azure** (Zu Azure) > **Nicht virtualisiert/Andere** aus.
    - **Hyper-V:** Wählen Sie **To Azure** (Zu Azure) > **Ja, mit Hyper-V** aus. Wenn Hyper-V-VMs von VMM verwaltet werden, wählen Sie **Ja** aus.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

- [Einrichten](vmware-azure-tutorial.md#set-up-the-source-environment) der Quellumgebung für VMware-VMs.
- [Einrichten](physical-azure-disaster-recovery.md#set-up-the-source-environment) der Quellumgebung für physische Server.
- [Einrichten](hyper-v-azure-tutorial.md#set-up-the-source-environment) der Quellumgebung für Hyper-V-VMs.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie das Ressourcen-Manager-Bereitstellungsmodell an.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

- [Richten Sie eine Replikationsrichtlinie ](vmware-azure-tutorial.md#create-a-replication-policy) für VMware-VMs ein.
- [Richten Sie eine Replikationsrichtlinie](physical-azure-disaster-recovery.md#create-a-replication-policy) für physische Server ein.
- [Richten Sie eine Replikationsrichtlinie](hyper-v-azure-tutorial.md#set-up-a-replication-policy) für Hyper-V-VMs ein.


## <a name="enable-replication"></a>Aktivieren der Replikation

- [Aktivieren Sie die Replikation](vmware-azure-tutorial.md#enable-replication) für VMware-VMs.
- [Aktivieren Sie die Replikation](physical-azure-disaster-recovery.md#enable-replication) für physische Server.
- Aktivieren Sie die Replikation für Hyper-V-VMs [mit](hyper-v-vmm-azure-tutorial.md#enable-replication) oder [ohne VMM](hyper-v-azure-tutorial.md#enable-replication).


## <a name="run-a-test-migration"></a>Ausführen einer Testmigration

Führen Sie ein [Testfailover](tutorial-dr-drill-azure.md) in Azure aus, um sicherzustellen, dass alles wie erwartet funktioniert.


## <a name="migrate-to-azure"></a>Migrieren zu Azure

Führen Sie ein Failover für die zu migrierenden Computer aus.

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** auf den Computer > **Failover**.
2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Wählen Sie den letzten Wiederherstellungspunkt aus.
3. Die Einstellung für den Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
4. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, virtuelle Computer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
5. Überprüfen Sie, ob die Azure-VM wie erwartet in Azure angezeigt wird.
6. Klicken Sie in **Replizierte Elemente** mit der rechten Maustaste auf die VM > **Migration abschließen**. Die folgenden Schritte werden ausgeführt:

    - Der Migrationsvorgang wird abgeschlossen, die Replikation für den virtuellen AWS-Computer wird beendet, und die Site Recovery-Abrechnung für den virtuellen Computer wird eingestellt.
    - In diesem Schritt werden die Replikationsdaten entfernt. Die migrierten virtuellen Computer werden nicht gelöscht.

    ![Abschließen der Migration](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Brechen Sie ein aktuell ausgeführtes Failover nicht ab**: Die VM-Replikation wird beendet, bevor das Failover gestartet wird. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. Bei physischen Servern, VMware-Linux-Computern, VMware-VMs ohne aktivierten DHCP-Dienst und VMware-VMs mit den folgenden Starttreibern kann das Testfailover länger dauern: storvsc, vmbus, storflt, intelide, atapi.

## <a name="after-migration"></a>Nach der Migration

Nach der Migration von Computern zu Azure sind eine Reihe von Schritten durchzuführen.

Einige Schritte können im Rahmen des Migrationsvorgangs mithilfe der integrierten Automatisierungsskriptfunktion in [Wiederherstellungsplänen]( https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation) automatisiert werden.   


### <a name="post-migration-steps-in-azure"></a>Schritte nach der Migration in Azure

- Führen Sie App-Anpassungen nach der Migration durch, z.B. die Aktualisierung von Datenbankverbindungszeichenfolgen und Webserverkonfigurationen. 
- Führen Sie endgültige Anwendungs- und Migrationsakzeptanztests für die migrierte Anwendung durch, die nun in Azure ausgeführt wird.
- Der [Azure-VM-Agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) verwaltet VM-Interaktionen mit dem Azure Fabric Controller. Dieser ist für einige Azure-Dienste wie Azure Backup, Site Recovery und das Azure Security Center erforderlich.
    - Wenn Sie VMware-Computer und physische Server migrieren, installiert das Mobility Service-Installationsprogramm verfügbare Azure-VM-Agents auf Windows-Computern. Auf Linux-VMs empfehlen wir, den Agent nach einem Failover zu installieren. a
    - Wenn Sie Azure-VMs in eine sekundäre Region migrieren, muss der Azure-VM-Agent vor der Migration auf der VM bereitgestellt werden.
    - Wenn Sie Hyper-V-VMs zu Azure migrieren, installieren Sie nach der Migration den Azure-VM-Agent auf der Azure-VM.
- Entfernen Sie manuell alle Site Recovery-Anbieter/-Agents von der VM. Wenn Sie VMware-VMs oder physische Server migrieren [deinstallieren Sie den Mobility Service von der Azure-VM][vmware-azure-install-mobility-service.md#uninstall-mobility-service-on-a-windows-server-computer].
- Beachten Sie zur Steigerung der Resilienz Folgendes:
    - Schützen Sie Daten, indem Sie Azure-VMs mit dem Azure Backup-Dienst sichern. [Weitere Informationen]( https://docs.microsoft.com/azure/backup/quick-backup-vm-portal)
    - Sorgen Sie für die kontinuierliche Ausführung und Verfügbarkeit von Workloads, indem Sie Azure-VMs mithilfe von Site Recovery in eine sekundäre Region replizieren. [Weitere Informationen](azure-to-azure-quickstart.md)
- Beachten Sie zur Steigerung der Sicherheit Folgendes:
    - Sperren und beschränken Sie den Zugriff von eingehendem Datenverkehr mit der [Just-in-Time-Verwaltung]( https://docs.microsoft.com/azure/security-center/security-center-just-in-time) im Azure Security Center.
    - Beschränken Sie den Netzwerkdatenverkehr mithilfe von [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview) auf Verwaltungsendpunkte.
    - Stellen Sie [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) bereit, um Datenträger und Daten vor Diebstahl und unbefugtem Zugriff zu schützen.
    - Erfahren Sie mehr über das [Sichern von IaaS-Ressourcen]( https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/ ), und besuchen Sie die Website [Azure Security Center](https://azure.microsoft.com/services/security-center/ ).
- Beachten Sie zur Überwachung und Verwaltung Folgendes:
    - Ziehen Sie die Bereitstellung von [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) in Erwägung, um den Ressourceneinsatz und die Ausgaben zu überwachen.

### <a name="post-migration-steps-on-premises"></a>Schritte nach der Migration in der lokalen Umgebung

- Migrieren Sie den App-Datenverkehr zur App, die auf der migrierten Azure-VM-Instanz ausgeführt wird.
- Entfernen Sie die lokalen VMs aus Ihrem lokalen VM-Bestand.
- Entfernen Sie die lokalen VMs aus lokalen Sicherungen.
- Aktualisieren Sie die interne Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adresse der Azure-VMs.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie lokale VMs in Azure-VMs migriert. Nun können Sie die [Notfallwiederherstellung](azure-to-azure-replicate-after-migration.md) für die Azure-VMs in einer sekundären Azure-Region einrichten.

  
