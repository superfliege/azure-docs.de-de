---
title: "Ausführen eines Failovers und Failbacks für VMware-VMs und physische Server, die mit Site Recovery nach Azure repliziert werden | Microsoft-Dokumentation"
description: "Informationen zum Ausführen eines Failovers für VMware-VMs und physische Server auf Azure und eines Failbacks auf den lokalen Standort mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 28a14a9b28dfe9c2014add9b9f691bce6ba91a4c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Ausführen eines Failovers und Failbacks für VMware-VMs und physische Server, die nach Azure repliziert werden.

In diesem Tutorial wird das Ausführen eines Failovers einer VMware-VM auf Azure beschrieben. Nach dem Failover erfolgt ein Failback zum lokalen Standort, wenn er verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der VMware-VM-Eigenschaften, um die Konformität mit Azure-Anforderungen festzustellen
> * Ausführen eines Failovers auf Azure
> * Erstellen eines Prozessservers und Masterzielservers für das Failback
> * Erneutes Schützen von virtuellen Azure-Computern zum lokalen Standort
> * Failover von Azure auf den lokalen Standort
> * Erneutes Schützen von lokalen-VMs, um das erneute Replizieren nach Azure zu starten

Dies ist das fünfte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben.

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Lokales Vorbereiten von VMware](tutorial-prepare-on-premises-vmware.md)
3. [Einrichten der Notfallwiederherstellung](tutorial-vmware-to-azure.md)
4. [Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Vorbereiten für Failover und Failback

Stellen Sie sicher, dass auf dem virtuellen Computer keine Momentaufnahmen vorhanden sind. Der lokale virtuelle Computer wird während des erneuten Schützens deaktiviert.
Dies hilft, die Konsistenz der Daten während der Replikation sicherzustellen. Schalten Sie den virtuellen Computer nach Abschluss des erneuten Schützens nicht ein. Verwenden Sie zum erneuten Schützen einer Replikationsgruppe denselben Masterzielserver. Wenn Sie für das erneute Schützen einer Replikationsgruppe einen anderen Masterzielserver verwenden, kann der Server keinen gemeinsamen Zeitpunkt angeben.

Failover und Failback weisen vier Phasen auf:

1. **Failover auf Azure**: Failover von Computern vom lokalen Standort auf Azure.
2. **Erneutes Schützen von virtuellen Azure-Computern**: Erneutes Schützen der Azure-VMs, sodass sie wieder zurück nach lokalen VMware-VMs repliziert werden.
3. **Failover auf lokalen Standort**: Ausführen eines Failovers, um ein Failback aus Azure durchzuführen.
4. **Erneutes Schützen von lokalen VMs**: Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer, auf die Sie das Failback durchgeführt haben, erneut, damit diese mit der Replikation nach Azure beginnen.

## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Überprüfen Sie die VM-Eigenschaften, und stellen Sie sicher, dass der virtuelle Computer die [Azure-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > VM.

2. Im Bereich **Repliziertes Element** finden Sie eine Zusammenfassung der Informationen zu virtuellen Computern, den Integritätsstatus sowie die neuesten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.

3. In **Compute und Netzwerk** können Sie den Azure-Namen, die Ressourcengruppe, Zielgröße, [Verfügbarkeitsgruppe](../virtual-machines/windows/tutorial-availability-sets.md) und [Einstellungen verwalteter Datenträger](#managed-disk-considerations) ändern.

4. Sie können Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, anzeigen und ändern.

5. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="run-a-failover-to-azure"></a>Ausführen eines Failovers auf Azure

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Failover**.

2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Sie können eine der folgenden Optionen auswählen:
   - **Neueste** (Standard): Mit dieser Option werden zuerst alle an Site Recovery gesendeten Daten verarbeitet. Sie bietet die niedrigste RPO (Recovery Point Objective), da die nach dem Failover erstellte Azure-VM über alle Daten verfügt, die bei Auslösung des Failovers zu Site Recovery repliziert wurden.
   - **Letzte Verarbeitung:** Mit dieser Option wird ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde. Diese Option bietet eine niedrige Recovery Time Objective (RTO), da keine Zeit für die Verarbeitung unverarbeiteter Daten aufgewendet wird.
   - **Letzte App-Konsistenz:** Mit dieser Option wird ein Failover des virtuellen Computers auf den letzten App-konsistenten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde.
   - **Benutzerdefiniert**: Geben Sie einen Wiederherstellungspunkt an.

3. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, um zu versuchen, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

4. Wenn Sie das Herstellen einer Verbindung mit der Azure-VM vorbereitet haben, stellen Sie eine Verbindung her, um sie nach dem Failover zu überprüfen.

5. **Committen** Sie das Failover nach der Überprüfung. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht.

> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet.
> Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. Bei physischen Servern, VMware-Linux-Computern, VMware-VMs ohne aktivierten DHCP-Dienst und VMware-VMs mit den folgenden Starttreibern kann das Testfailover länger dauern: storvsc, vmbus, storflt, intelide, atapi.

## <a name="create-a-process-server-in-azure"></a>Erstellen eines Prozessservers in Azure

Der Prozessserver empfängt Daten von der Azure-VM und sendet Daten an den lokalen Standort. Zwischen dem Prozessserver und dem geschützten virtuellen Computer ist ein Netzwerk mit geringer Wartezeit erforderlich.

- Wenn Sie über eine Azure ExpressRoute-Verbindung verfügen, können Sie zu Testzwecken den lokalen Prozessserver verwenden, der automatisch auf dem Konfigurationsserver installiert wird.
- Wenn Sie über eine VPN-Verbindung verfügen, oder Sie das Failback in einer Produktionsumgebung ausführen, müssen Sie eine Azure-VM als Azure-basierten Prozessserver für Failback einrichten.
- Um einen Prozessserver in Azure einzurichten, befolgen Sie die Anweisungen in [diesem Artikel](site-recovery-vmware-setup-azure-ps-resource-manager.md).

## <a name="configure-the-master-target-server"></a>Konfigurieren des Masterzielservers

Standardmäßig wird der Masterzielserver auf dem lokalen Konfigurationsserver ausgeführt. Für dieses Tutorial wird der Standardmaster verwendet. Der Masterzielserver empfängt Failbackdaten.

Wenn der virtuelle Computer sich auf einem ESXi-Host befindet, der von einem vCenter-Server verwaltet wird, benötigt der Masterzielserver Zugriff auf den VM-Datenspeicher (VMDK), um replizierte Daten auf die VM-Datenträger zu schreiben. Stellen Sie sicher, dass der VM-Datenspeicher auf dem Host des Masterziels mit Lese-/Schreibzugriff eingebunden wird.

Wenn der virtuelle Computer sich auf einem ESXi-Host befindet, der nicht von einem vCenter-Server verwaltet wird, erstellt der Site Recovery-Dienst während des erneuten Schutzes einen neuen virtuellen Computer. Dieser virtuelle Computer wird auf dem ESX-Host erstellt, auf dem Sie das Masterziel erstellen.
Die Festplatte des virtuellen Computers muss sich in einen Datenspeicher befinden, der für den Host zugänglich ist, auf dem der Masterzielserver ausgeführt wird.

Wenn der virtuelle Computer vCenter nicht verwendet, sollten Sie die Ermittlung des Hosts abschließen, auf dem der Masterzielserver ausgeführt wird, bevor Sie den Computer erneut schützen können. Dasselbe gilt auch bei einem Failback für physische Server. Als weitere Option können Sie den lokalen virtuellen Computer (sofern er noch vorhanden ist) vor dem Ausführen des Failbacks löschen. Beim Failback wird dann ein neuer virtueller Computer auf dem gleichen Host erstellt, der als Masterziel-ESX-Host fungiert. Wenn Sie das Failback zu einem anderen Speicherort durchführen, werden die Daten in dem gleichen Datenspeicher und in dem gleichen ESX-Host wiederhergestellt, der vom lokalen Masterzielserver verwendet wird.

Es ist nicht möglich, Storage vMotion auf dem Masterzielserver zu verwenden. Wenn Sie dies tun, ist kein Failback möglich, da die Datenträger nicht verfügbar sind. Schließen Sie die Masterzielserver aus der vMotion-Liste aus.

## <a name="reprotect-azure-vms"></a>Erneutes Schützen der virtuellen Azure-Computer

Dieses Verfahren setzt voraus, dass der lokale virtuelle Computer nicht verfügbar ist, und Sie für das erneute Schützen einen anderen Speicherort verwenden.

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover ausgeführt wurde > **Erneut schützen**.
2. Überprüfen Sie in **Erneut schützen**, ob **Azure auf lokal** ausgewählt ist.
3. Geben Sie den lokalen Masterzielserver und den Prozessserver an.

4. Wählen Sie in **Datenspeicher** den Masterziel-Datenspeicher aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Verwenden Sie diese Option, wenn der lokale virtuelle Computer gelöscht wurde und neue Datenträger erstellt werden müssen. Diese Einstellungen werden ignoriert, wenn die Datenträger bereits vorhanden sind. Sie müssen aber trotzdem einen Wert angeben.
5. Wählen Sie das Masterziel-Aufbewahrungslaufwerk aus. Die Failbackrichtlinie wird automatisch ausgewählt.
6. Klicken Sie auf **OK**, um das erneute Schützen zu starten. Ein Auftrag beginnt mit der Replikation des virtuellen Computers von Azure am lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.

> [!NOTE]
> Wenn Sie den virtuellen Azure-Computer unter einem vorhandenen lokalen virtuellen Computer wiederherstellen möchten, binden Sie den Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff auf dem ESXi-Host des Masterzielservers ein.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Ausführen eines Failovers von Azure auf den lokalen Standort

Um zurück nach dem lokalen Standort zu replizieren, wird eine Failbackrichtlinie verwendet. Diese Richtlinie wird automatisch erstellt, wenn Sie eine Replikationsrichtlinie für die Replikation zu Azure erstellen:

- Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet.
- Die Richtlinie kann nicht geändert werden.
- Die Werte der Richtlinie sind:
    - RPO-Schwellenwert = 15 Minuten
    - Aufbewahrungszeitraum des Wiederherstellungspunkts = 24 Stunden
    - Häufigkeit von Momentaufnahmen für App-Konsistenz = 60 Minuten

Führen Sie das Failover wie folgt aus:

1. Klicken Sie auf der Seite **Replizierte Elemente** mit der rechten Maustaste auf den Computer > **Ungeplantes Failover**.
2. Überprüfen Sie in **Failover bestätigen**, dass das Failover von Azure aus erfolgt.

3. Wählen Sie den Wiederherstellungspunkt aus, den Sie für das Failover verwenden möchten. Ein anwendungskonsistenter Punkt liegt vor dem aktuellsten Zeitpunkt und verursacht einigen Datenverlust. Wenn das Failover ausgeführt wird, fährt Site Recovery die Azure-VMs herunter und startet den lokalen virtuellen Computer. Da dies mit einer gewissen Ausfallzeit verbunden ist, wählen Sie einen geeigneten Zeitpunkt.
4. Klicken Sie mit der rechten Maustaste auf den Computer, und klicken Sie auf **Committen**. Dadurch wird ein Auftrag ausgelöst, der die Azure-VMs entfernt.
5. Stellen Sie sicher, dass die Azure-VMs wie erwartet heruntergefahren wurden.


## <a name="reprotect-on-premises-machines-to-azure"></a>Erneutes Schützen lokaler Computer in Azure

Die Daten sollten nun wieder auf Ihrem lokalen Standort sein, aber sie werden nicht nach Azure repliziert. Sie können die Replikation nach Azure folgendermaßen erneut starten:

1. Wählen Sie im Tresor > **Einstellungen** >**Replizierte Elemente** die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie auf **Erneut schützen**.
2. Wählen Sie den Prozessserver, mit dem die replizierten Daten nach Azure gesendet werden, und klicken Sie auf **OK**.

Nachdem das erneute Schützen abgeschlossen ist, wird der virtuelle Computer zurück nach Azure repliziert, und Sie können ggf. ein Failover ausführen.
