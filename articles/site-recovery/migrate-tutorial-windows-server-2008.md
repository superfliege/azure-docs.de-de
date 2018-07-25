---
title: Migrieren von lokalen Windows Server 2008-Servern zu Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie lokale Windows Server 2008-Computer mithilfe von Azure Site Recovery zu Azure migrieren.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2018
ms.author: bsiva
ms.openlocfilehash: 0d3f28f0a9f1e9862fabb6ce5e96597f1534abd8
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011398"
---
# <a name="migrate-servers-running-windows-server-2008-2008-r2-to-azure"></a>Migrieren von Servern mit Windows Server 2008 oder 2008 R2 zu Azure

In diesem Tutorial erfahren Sie, wie lokale Server, auf denen Windows Server 2008 oder 2008 R2 ausgeführt wird, mithilfe von Azure Site Recovery zu Azure migriert werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten Ihrer lokalen Umgebung für die Migration
> * Einrichten der Zielumgebung
> * Einrichten einer Replikationsrichtlinie
> * Aktivieren der Replikation
> * Führen Sie eine Testmigration aus, um sicherzustellen, dass alles wie erwartet funktioniert.
> * Ausführen eines Failovers zu Azure und Durchführen der Migration

Im Abschnitt zu den Einschränkungen und bekannten Problemen werden einige der Einschränkungen und Problemumgehungen für bekannte Probleme aufgeführt, die bei der Migration von Windows Server 2008-Computern zu Azure auftreten könnten. 


## <a name="supported-operating-systems-and-environments"></a>Unterstützte Betriebssysteme und Umgebungen


|Betriebssystem  | Lokale Umgebung  |
|---------|---------|
|Windows Server 2008 SP2 – 32-Bit und 64-Bit (IA-32 und x86-64)</br>– Standard</br>– Enterprise</br>– Datacenter   |     VMware-VMs, Hyper-V-VMs und physische Server    |
|Windows Server 2008 R2 SP1 – 64-Bit</br>– Standard</br>– Enterprise</br>– Datacenter     |     VMware-VMs, Hyper-V-VMs und physische Server|

> [!WARNING]
> - Die Migration von Servern, auf denen Server Core ausgeführt wird, wird nicht unterstützt.
> - Vergewissern Sie sich vor der Migration, dass das neueste Service Pack und die aktuellen Windows-Updates installiert sind.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, sollten Sie sich die Azure Site Recovery-Architektur für die [Migration von VMware und physischen Servern](vmware-azure-architecture.md) oder [Migration von Hyper-V-VMs](hyper-v-azure-architecture.md) ansehen. 

Um virtuelle Hyper-V-Computer zu migrieren, auf denen Windows Server 2008 oder Windows Server 2008 R2 ausgeführt wird, führen Sie die Schritte im Tutorial [Migrieren von lokalen Computern zu Azure](migrate-tutorial-on-premises-azure.md) durch.

Im übrigen Teil dieses Tutorials erfahren Sie, wie Sie lokale virtuelle VMware-Computer und physische Server migrieren können, auf denen Windows Server 2008 oder 2008 R2 ausgeführt wird.


## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

- Der Konfigurationsserver, zusätzliche Prozessserver und der Mobilitätsdienst, die zum Migrieren von Windows Server 2008 SP2-Servern verwendet werden, sollten Version 9.18.0.1 der Azure Site Recovery-Software aufweisen. Das einheitliche Setup für Version 9.18.0.1 des Konfigurations- und Prozessservers kann unter [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup) heruntergeladen werden.

- Ein vorhandener Konfigurations- oder Prozessserver kann nicht für die Migration von Servern verwendet werden, auf denen Windows Server 2008 SP2 ausgeführt wird. Ein neuer Konfigurationsserver sollte mit Version 9.18.0.1 der Azure Site Recovery-Software bereitgestellt werden. Dieser Konfigurationsserver sollte nur für die Migration von Windows-Servern zu Azure verwendet werden.

- Anwendungskonsistente Wiederherstellungspunkte und das Feature für Konsistenz mit mehreren virtuellen Computern werden nicht für die Replikation von Servern unterstützt, auf denen Windows Server 2008 SP2 ausgeführt wird. Windows Server 2008 SP2-Server sollten an einem absturzkonsistenten Wiederherstellungspunkt migriert werden. Absturzkonsistente Wiederherstellungspunkte werden standardmäßig alle 5 Minuten generiert. Die Verwendung einer Replikationsrichtlinie mit einer konfigurierten Häufigkeit der Momentaufnahmen für Anwendungskonsistenz führt dazu, dass die Replikationsintegrität aufgrund mangelnder anwendungskonsistenter Wiederherstellungspunkte kritisch wird. Um zu verhindern, dass falsche positive Ergebnisse ausgegeben werden, legen Sie die Häufigkeit der Momentaufnahmen für Anwendungskonsistenz in der Replikationsrichtlinie auf „Aus“ fest.

- Die Server, die migriert werden, sollten .NET Framework 3.5 Service Pack 1 aufweisen, damit der Mobilitätsdienst funktioniert.


- Verfügt Ihr Server über dynamische Datenträger, werden Sie bei bestimmten Konfigurationen möglicherweise feststellen, dass diese Datenträger auf dem Server, für den ein Failover ausgeführt wurde, als offline gekennzeichnet sind oder als fremde Datenträger angezeigt werden. Möglicherweise wird Ihnen außerdem auffallen, dass der Status gespiegelter Sätze für gespiegelte Volumes auf dynamischen Datenträgern mit „Fehlerhafte Redundanz“ markiert ist. Sie können dieses Problem über „diskmgmt.msc“ beheben, indem Sie diese Datenträger manuell importieren und wieder aktivieren.

- Die Server, der migriert werden, sollten den Treiber „vmstorfl.sys“ aufweisen. Beim Failover tritt möglicherweise ein Fehler auf, wenn der Treiber nicht auf dem migrierten Server vorhanden ist. 
  > [!TIP]
  >Überprüfen Sie, ob der Treiber unter „C:\Windows\system32\drivers\vmstorfl.sys“ vorhanden ist. Wenn der Treiber nicht gefunden wird, können Sie das Problem umgehen, indem Sie eine Dummydatei erstellen. 
  >
  > Öffnen Sie die Eingabeaufforderung („Cmd.exe“ ausführen), und führen Sie den folgenden Befehl aus: „copy nul c:\Windows\system32\drivers\vmstorfl.sys“

- Eventuell können Sie das RDP nicht sofort auf Windows Server 2008 SP2-Servern festlegen, auf denen das 32-Bit-Betriebssystem ausgeführt wird, nachdem ein Failover für diese durchgeführt oder ein Failover zu Azure getestet wurde. Starten Sie den virtuellen Computer, für den ein Failover ausgeführt wurde, über das Azure-Portal neu, und stellen Sie erneut eine Verbindung her. Wenn Sie trotzdem keine Verbindung herstellen können, überprüfen Sie, ob der Server für das Zulassen von Remotedesktopverbindungen konfiguriert ist, und stellen Sie sicher, dass keine Firewallregeln oder Netzwerksicherheitsgruppen die Verbindung blockieren. 
  > [!TIP]
  > Vor der Migration von Servern wird ausdrücklich ein Testfailover empfohlen. Stellen Sie sicher, dass auf jedem Server, der migriert werden soll, mindestens ein erfolgreiches Testfailover ausgeführt wurde. Stellen Sie im Rahmen des Testfailovers eine Verbindung mit dem Computer her, für den ein Failover getestet wurde, und stellen Sie sicher, dass alles erwartungsgemäß funktioniert.
  >
  >Der Vorgang des Testfailovers ist unterbrechungsfrei und unterstützt Sie beim Testen von Migrationen, da Sie virtuelle Computer in einem isolierten Netzwerk Ihrer Wahl erstellen können. Im Gegensatz zum Failovervorgang wird die Datenreplikation während des Testfailovers fortgesetzt. Sie können beliebig viele Testfailover ausführen, bevor Sie mit der Migration beginnen. 
  >
  >


## <a name="getting-started"></a>Erste Schritte

Führen Sie die folgenden Aufgaben zur Vorbereitung des Azure-Abonnements und der lokalen VMware- bzw. physischen Umgebung durch:

1. [Prepare Azure](tutorial-prepare-azure.md) (Vorbereiten von Azure)
2. Vorbereiten einer lokalen [VMware-VM](vmware-azure-tutorial-prepare-on-premises.md)


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

1. Melden Sie sich unter [Azure-Portal](https://portal.azure.com) > **Recovery Services** an.
2. Klicken Sie auf **Ressource erstellen** > **Überwachung und Verwaltung** > **Backup und Site Recovery**.
3. Geben Sie unter **Name** den Anzeigenamen **W2K8-migration** an. Wenn Sie mehrere Abonnements haben, wählen Sie das gewünschte aus.
4. Erstellen Sie eine Ressourcengruppe namens **w2k8migrate**.
5. Geben Sie eine Azure-Region an. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Um schnell über das Dashboard auf den Tresor zuzugreifen, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

   ![Neuer Tresor](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

Der neue Tresor wird dem **Dashboard** unter **Alle Ressourcen** und der Hauptseite **Recovery Services-Tresore** hinzugefügt.


## <a name="prepare-your-on-premises-environment-for-migration"></a>Vorbereiten Ihrer lokalen Umgebung für die Migration

- Herunterladen des Konfigurationsserver-Installationsprogramms (einheitliches Setup) von [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup)
- [Richten Sie die Quellumgebung ein](physical-azure-disaster-recovery.md#set-up-the-source-environment), indem Sie die im vorherigen Schritt heruntergeladene Installationsdatei zu Hilfe nehmen.

> [!IMPORTANT]
> Verwenden Sie die Setupdatei, die Sie im ersten Schritt weiter oben heruntergeladen haben, um den Konfigurationsserver zu installieren und zu registrieren. Laden Sie die Setupdatei nicht aus dem Azure-Portal herunter. Die Setupdatei unter [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup) ist die einzige Version, die Windows Server 2008-Migrationen unterstützt.
>
> Sie können nicht mit einem vorhandenen Konfigurationsserver Computer migrieren, auf denen Windows Server 2008 ausgeführt wird. Über den oben angegebenen Link müssen Sie einen neuen Konfigurationsserver einrichten.

 ![Herunterladen des Tresorregistrierungsschlüssels](media/migrate-tutorial-windows-server-2008/download-vault-credentials.png) 
 

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie das Ressourcen-Manager-Bereitstellungsmodell an.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.


## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
2. Geben Sie unter **Replikationsrichtlinie erstellen** einen Richtliniennamen an.
3. Geben Sie in **RPO-Schwellenwert** den RPO-Grenzwert (Recovery Point Objective) an. Wenn der RPO-Wert bei der Replikation überschritten wird, wird eine Warnung generiert.
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** die Größe des Aufbewahrungszeitfensters für die einzelnen Wiederherstellungspunkte in Stunden an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt (72 Stunden für Standardspeicher).
5. Geben Sie unter **Häufigkeit der Momentaufnahmen für App-Konsistenz** die Option **Aus** an. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet.

> [!WARNING]
> Stellen Sie sicher, dass Sie für die Häufigkeit der Momentaufnahmen für App-Konsistenz der Replikationsrichtlinie **AUS** angeben. Nur absturzkonsistente Wiederherstellungspunkte werden beim Replizieren von Servern unter Windows Server 2008 unterstützt. Wenn Sie einen anderen Wert für die Häufigkeit der Momentaufnahmen für App-Konsistenz angeben, werden falsche Warnungen generiert. Dies führt dazu, dass die Replikationsintegrität des Servers aufgrund mangelnder App-konsistenter Wiederherstellungspunkte kritisch wird.

   ![Erstellen einer Replikationsrichtlinie](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>Aktivieren der Replikation

[Aktivieren Sie die Replikation](physical-azure-disaster-recovery.md#enable-replication) für die Windows Server 2008 SP2- bzw. Windows Server 2008 R2 SP1-Server, die migriert werden sollen.
   
   ![Hinzufügen eines physischen Servers](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![Aktivieren der Replikation](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>Ausführen einer Testmigration

Nachdem die erste Replikation abgeschlossen ist und der Serverstatus zu **Geschützt** wechselt, können Sie ein Testfailover für die Server ausführen, die repliziert werden.

Führen Sie ein [Testfailover](tutorial-dr-drill-azure.md) in Azure aus, um sicherzustellen, dass alles wie erwartet funktioniert.

   ![Testfailover](media/migrate-tutorial-windows-server-2008/testfailover.png)


## <a name="migrate-to-azure"></a>Migrieren zu Azure

Führen Sie ein Failover für die zu migrierenden Computer aus.

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** auf den Computer > **Failover**.
2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Wählen Sie den letzten Wiederherstellungspunkt aus.
3. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery fährt den Server herunter, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
4. Überprüfen Sie, ob die Azure-VM wie erwartet in Azure angezeigt wird.
5. Klicken Sie in **Replizierte Elemente** mit der rechten Maustaste auf die VM > **Migration abschließen**. Dadurch wird der Migrationsvorgang abgeschlossen, die Replikation für die VM wird beendet, und die Site Recovery-Abrechnung für die VM wird eingestellt.

   ![Abschließen der Migration](media/migrate-tutorial-windows-server-2008/complete-migration.png)


> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.
