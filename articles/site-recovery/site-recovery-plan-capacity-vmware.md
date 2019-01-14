---
title: Planen der Kapazität und Skalierung der VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: Verwenden Sie diesen Artikel zum Planen der Kapazität und Skalierung beim Einrichten der Notfallwiederherstellung von VMware-VMs in Azure mit Azure Site Recovery.
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 6f644416a9e56009aadd0f8e1b217402d625af84
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53788734"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>Planen der Kapazität und Skalierung der VMware-Notfallwiederherstellung für Azure

Verwenden Sie diesen Artikel, um zu ermitteln, wie die Kapazität und die Skalierung beim Replizieren von lokalen VMware-VMs und physischen Servern in Azure mit [Azure Site Recovery](site-recovery-overview.md) geplant wird.

## <a name="how-do-i-start-capacity-planning"></a>Wie beginne ich mit der Kapazitätsplanung?

Sammeln Sie mit dem [Azure Site Recovery-Bereitstellungsplaner](https://aka.ms/asr-deployment-planner-doc) für die VMware-Replikation Informationen zu Ihrer Replikationsumgebung, um die Anforderungen für die Azure Site Recovery-Infrastruktur zu ermitteln. [Hier](site-recovery-deployment-planner.md) finden Sie weitere Informationen zu diesem Tool. Dieses Tool stellt einen Bericht mit vollständigen Informationen zu kompatiblen und inkompatiblen VMs, zu den Datenträgern pro VM und zur Datenänderungsrate pro Datenträger bereit. Außerdem stellt das Tool eine Zusammenfassung der Netzwerkbandbreitenanforderungen zum Erreichen des RPO-Ziels und der erforderlichen Azure-Infrastruktur bereit, die für erfolgreiche Replikation und Testfailovers benötigt wird.

## <a name="capacity-considerations"></a>Überlegungen zur Kapazität

**Komponente** | **Details** |
--- | --- | ---
**Replikation** | **Maximale tägliche Änderungsrate:** Ein geschützter Computer kann nur einen einzigen Prozessserver verwenden, und ein einzelner Prozessserver kann eine tägliche Änderungsrate von bis zu 2 TB verarbeiten. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen geschützten Computer unterstützt wird, 2 TB.<br/><br/> **Maximaler Durchsatz:** Ein replizierter Computer kann zu einem einzigen Speicherkonto in Azure gehören. Ein Standardspeicherkonto kann maximal 20.000 Anforderungen pro Sekunde verarbeiten, und es wird empfohlen, dass Sie die Anzahl von Eingabe-/Ausgabevorgängen pro Sekunde (IOPS) über einen Quellcomputer hinweg bei 20.000 zu halten. Wenn Ihr Quellcomputer z.B. 5 Datenträger aufweist und jeder Datenträger 120 IOPS (mit einer Größe von 8 KB) für den Quellcomputer generiert, wird das Azure-Limit von 500 IOPS pro Datenträger eingehalten. (Die Anzahl der erforderlichen Speicherkonten entspricht dem IOPS-Gesamtwert der Quellcomputer, geteilt durch 20.000.)
**Konfigurationsserver** | Der Konfigurationsserver muss in der Lage sein, die tägliche Änderungsrate für alle auf geschützten Computern ausgeführten Workloads zu bewältigen, und er muss über genügend Bandbreite verfügen, um Daten kontinuierlich in den Azure-Speicher zu replizieren.<br/><br/> Als bewährte Methode sollten Sie den Konfigurationsserver im selben Netzwerk und LAN-Segment platzieren, in dem sich auch die zu schützenden Computer befinden. Er kann in einem anderen Netzwerk platziert werden, aber die zu schützenden Computer sollten für ihn über Layer 3-Netzwerksichtbarkeit verfügen.<br/><br/> Die empfohlenen Größen für den Konfigurationsserver werden in der Tabelle im folgenden Abschnitt zusammengefasst.
**Prozessserver** | Der erste Prozessserver wird standardmäßig auf dem Konfigurationsserver installiert. Sie können zusätzliche Prozessserver bereitstellen, um Ihre Umgebung zu skalieren. <br/><br/> Der Prozessserver empfängt Replikationsdaten von geschützten Computern und optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung. Anschließend sendet er die Daten an Azure. Der Prozessservercomputer muss über genügend Ressourcen zum Ausführen dieser Aufgaben verfügen.<br/><br/> Der Prozessserver verwendet einen datenträgerbasierten Cache. Verwenden Sie einen separaten Cachedatenträger mit einer Größe von mindestens 600 GB, um Datenänderungen bei einem Netzwerkengpass oder -ausfall zwischenspeichern und verarbeiten zu können.

## <a name="size-recommendations-for-the-configuration-serverin-built-process-server"></a>Empfohlene Größen für den Konfigurationsserver bzw. integrierte Prozessserver

Jeder Konfigurationsserver, der mit der [OVF-Vorlage](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) bereitgestellt wird, verfügt über einen integrierten Prozessserver. Ressourcen wie CPU, Arbeitsspeicher und freier Speicherplatz eines Konfigurationsservers werden zu einer anderen Rate ausgelastet, wenn integrierte Prozessserver zum Schutz der VMs verwendet werden. Daher variieren die Anforderungen, wenn ein integrierter Prozessserver verwendet wird.
Ein Konfigurationsserver, für den integrierte Prozessserver zum Schutz der Arbeitsauslastung verwendet werden, kann basierend auf den folgenden Konfigurationen bis zu 200 virtuelle Computer ausführen.

**CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Weniger als 100 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz) | 18 GB | 600 GB | 500 GB bis 1 TB | Zwischen 100 und 150 Computer replizieren.
16 vCPUs (2 Sockets * 8 Kerne \@ 2,5 GHz) | 32 GB | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 200 Computer replizieren.
Bereitstellen eines weiteren Konfigurationsservers mit der [OVF-Vorlage](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). | | | | Stellen Sie einen neuen Konfigurationsserver bereit, wenn Sie mehr als 200 Computer replizieren.
Bereitstellen eines weiteren [Prozessservers](vmware-azure-set-up-process-server-scale.md#download-installation-file) | | | >2 TB| Stellen Sie einen neuen Prozessserver für die horizontale Skalierung bereit, wenn die gesamte tägliche Datenänderungsrate 2 TB überschreitet.

Hinweis:

* Jeder Quellcomputer ist mit 3 Datenträgern von jeweils 100 GB konfiguriert.
* Bei den Benchmarkingmessungen für den Cachedatenträger wurde ein Speicher aus 8 SAS-Laufwerken mit 10.000 U/min und RAID 10 verwendet.

## <a name="size-recommendations-for-the-configuration-server"></a>Empfohlene Größen für den Konfigurationsserver

Wenn Sie nicht beabsichtigen, den Konfigurationsserver als Prozessserver zu verwenden, führen Sie die folgende Konfiguration durch, um bis zu 650 VMs verwenden zu können.

**CPU** | **RAM** | **Größe des Betriebssystemdatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
24 vCPUs (2 Sockets * 12 Kerne \@ 2,5 GHz)| 32 GB | 80 GB | Nicht zutreffend | Bis zu 650 VMs

Jeder Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

Da die Prozessserverfunktionalität nicht verwendet wird, ist die Datenänderungsrate irrelevant. Sie können Ihre Arbeitsauslastung von einem integrierten Prozessserver in einen anderen Prozessserver für die horizontale Skalierung verschieben, indem Sie die [hier](vmware-azure-manage-process-server.md#balance-the-load-on-process-server) beschriebenen Anweisungen ausführen.

## <a name="size-recommendations-for-the-process-server"></a>Empfohlene Größen für den Prozessserver

Der Prozessserver ist die Komponente, die den Datenreplikationsprozess in Azure Site Recovery verarbeitet. Wenn die tägliche Änderungsrate 2 TB überschreitet, müssen Sie einen Prozessserver für die horizontale Skalierung hinzufügen, um die Replikationslast zu verarbeiten. Zum horizontalen Hochskalieren haben Sie folgende Möglichkeiten:

* Erhöhen Sie die Anzahl der Konfigurationsserver, indem Sie sie mit der [OVF-Vorlage](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) bereitstellen. Beispielsweise können Sie mit zwei Konfigurationsservern bis zu 400 Computer schützen.
* Fügen Sie weitere [Prozessserver für die horizontale Skalierung](vmware-azure-set-up-process-server-scale.md#download-installation-file) hinzu, und verwenden Sie diese anstelle des Konfigurationsservers (oder als Ergänzung zu diesem) für die Verarbeitung des Replikationsdatenverkehrs.

Die folgende Tabelle beschreibt dieses Szenario:

* Sie haben einen Prozessserver für die horizontale Skalierung eingerichtet.
* Sie haben geschützte VMs für die Verwendung des Prozessservers für die horizontale Skalierung konfiguriert.
* Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

**Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | ---
4 vCPUs (2 Sockets * 2 Kerne \@ 2,5 GHz), 8 GB Arbeitsspeicher | 300 GB | 250 GB oder weniger | Maximal 85 Computer replizieren.
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz), 12 GB Arbeitsspeicher | 600 GB | 250 GB bis 1 TB | Zwischen 85 und 150 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz), 24 GB Arbeitsspeicher | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 225 Computer replizieren.

Wie Sie Ihre Server skalieren, hängt davon ab, ob Sie das zentrale Hochskalieren oder das horizontale Hochskalieren als Modell bevorzugen.  Beim zentralen Hochskalieren stellen Sie wenige besonders leistungsstarke Konfigurations- und Prozessserver bereit. Beim horizontalen Hochskalieren stellen Sie mehr Server mit geringeren Ressourcen bereit. Wenn Sie zum Beispiel 200 Computer mit einer täglichen gesamten Datenänderungsrate von 1,5 TB schützen müssen, können Sie sich für eine der beiden folgenden Optionen entscheiden:

* Richten Sie einen einzelnen Prozessserver mit 16 vCPUs und 24 GB RAM ein.
* Oder richten Sie zwei Prozessserver ein (jeweils 8 vCPUs und 12 GB RAM).

## <a name="control-network-bandwidth"></a>Steuern der Netzwerkbandbreite

Nachdem Sie mit dem [Deployment Planner-Tool](site-recovery-deployment-planner.md) die Bandbreite berechnet haben, die für die Replikation erforderlich ist (die erste Replikation und anschließende Deltas), können Sie die Menge der für die Replikation verwendeten Bandbreite mit einer Reihe von Optionen steuern:

* **Bandbreite drosseln**: VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Sie können die Bandbreite auf den Computern, die als Prozessserver ausgeführt werden, einschränken.
* **Bandbreite beeinflussen**: Die für die Replikation genutzte Bandbreite lässt sich mithilfe einiger Registrierungsschlüssel beeinflussen:
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht.
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung während eines Failbacks verwendet werden.

### <a name="throttle-bandwidth"></a>Bandbreite einschränken

1. Öffnen Sie das Azure Backup-MMC-Snap-In auf dem Computer, der als Prozessserver fungiert. Standardmäßig ist eine Verknüpfung für Backup auf dem Desktop oder im folgenden Ordner verfügbar: C:\Programme\Microsoft Azure Recovery Services Agent\bin.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.

    ![Screenshot der Azure Backup-MMC-Snap-In-Option zum Ändern von Eigenschaften](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Aktivieren Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**. Legen Sie die Grenzwerte für Arbeitsstunden und arbeitsfreie Stunden fest. Der gültige Bereich reicht von 512 KBit/s bis 1023 MBit/s.

    ![Screenshot des Dialogfeld „Eigenschaften von Azure Backup“](./media/site-recovery-vmware-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

### <a name="influence-network-bandwidth-for-a-vm"></a>Beeinflussen der Netzwerkbandbreite für eine VM

1. Navigieren Sie in der Registrierung der VM zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert von **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert von **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## <a name="setup-azure-site-recovery-infrastructure-to-protect-more-than-500-virtual-machines"></a>Einrichten der Azure Site Recovery-Infrastruktur zum Schützen von mehr als 500 VMs

Bevor Sie die Azure Site Recovery-Infrastruktur einrichten, müssen Sie auf die Umgebung zugreifen, um die folgenden Faktoren zu ermitteln: die kompatiblen VMs, die tägliche Datenänderungsrate, die für das RPO-Ziel erforderliche Netzwerkbandbreite, die Anzahl der erforderlichen Azure Site Recovery-Komponenten, die für die Erstreplikation erforderliche Zeit usw.

1. Stellen Sie sicher, dass Sie den Bereitstellungsplaner mithilfe der [hier](site-recovery-deployment-planner.md) aufgeführten Anweisungen ausführen, um diese Parameter zu ermitteln.
2. Stellen Sie einen Konfigurationsserver gemäß der [hier](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server) aufgeführten Anforderungen bereit. Stellen Sie einen weiteren Konfigurationsserver bereit, wenn Ihre Produktionsarbeitsauslastung 650 VMs überschreitet.
3. Stellen Sie einen [Prozessserver für die horizontale Skalierung](vmware-azure-set-up-process-server-scale.md#download-installation-file) basierend auf der ermittelten täglichen Datenänderungsrate mithilfe der [hier](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server) aufgeführten Anweisungen bereit.
4. Wenn Sie erwarten, dass die Datenänderungsrate für einen VM-Datenträger 2 MBps überschreitet, [richten Sie ein Premium-Speicherkonto ein](tutorial-prepare-azure.md#create-a-storage-account). Da der Bereitstellungsplaner für einen bestimmten Zeitraums ausgeführt wird, werden Spitzen der Datenänderungsrate während anderen Zeiträumen möglicherweise nicht im Bericht erfasst.
5. [Legen Sie die Netzwerkbandbreite](site-recovery-plan-capacity-vmware.md#control-network-bandwidth) gemäß des gewünschten RPO-Ziels fest.
6. Führen Sie die Anweisungen im [Abschnitt zur Vorgehensweise](vmware-azure-set-up-source.md) aus, um die Notfallwiederherstellung für Ihre Arbeitsauslastung zu aktivieren, nachdem Sie die Infrastruktur eingerichtet haben.

## <a name="deploy-additional-process-servers"></a>Bereitstellen zusätzlicher Prozessserver

Wenn Sie Ihre Bereitstellung über 200 Quellcomputer horizontal hochskalieren müssen oder Ihre gesamte tägliche Änderungsrate 2 TB übersteigt, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens. Befolgen Sie Anleitung in [diesem Artikel](vmware-azure-set-up-process-server-scale.md), um den Prozessserver einzurichten. Nachdem Sie den Prozessserver eingerichtet haben, können Sie Quellcomputer migrieren, damit diese ihn verwenden können.

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren der Computer für die Verwendung des neuen Prozessservers

1. Klicken Sie unter **Einstellungen** > **Site Recovery servers** auf den Konfigurationsserver, und erweitern Sie anschließend **Prozessserver**.

    ![Screenshot des Dialogfelds „Prozessserver“](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Klicken Sie mit der rechten Maustaste auf den aktuell verwendeten Prozessserver, und klicken Sie auf **Wechseln**.

    ![Screenshot des Dialogfelds „Konfigurationsserver“](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. Wählen Sie unter **Zielprozessserver auswählen** den neuen Prozessserver aus, den Sie verwenden möchten, und wählen Sie anschließend die virtuellen Computer aus, für die der Server zuständig sein wird. Klicken Sie auf das Informationssymbol, um Informationen über den Server zu erhalten. Der durchschnittliche zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigte Speicherplatz wird angezeigt, um Sie bei Lastenentscheidungen zu unterstützen. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen.

## <a name="deploy-additional-master-target-servers"></a>Bereitstellen von zusätzlichen Masterzielservern

Sie benötigen in den folgenden Szenarien zusätzliche Masterzielserver:

1. Wenn Sie versuchen, einen Linux-basierten virtuellen Computer zu schützen
2. Wenn der auf dem Konfigurationsserver verfügbare Masterzielserver keinen Zugriff auf den Datenspeicher der VM hat
3. Wenn die Gesamtzahl von Datenträgern auf dem Masterzielserver (Anzahl von lokalen Datenträgern auf dem Server plus zu schützende Datenträger) höher als 60 ist

[Klicken Sie hier](vmware-azure-install-linux-master-target.md), um einen neuen Masterzielserver für den **Linux-basierten virtuellen Computer** hinzuzufügen.

Befolgen Sie für **Windows-basierte virtuelle Computer** die unten angegebene Anleitung.

1. Navigieren Sie zu **Recovery Services-Tresor** > **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
2. Klicken Sie auf den erforderlichen Konfigurationsserver und dann auf **+Masterzielserver**.![add-master-target-server.png](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Laden Sie das einheitliche Setup herunter, und führen Sie es auf der VM aus, um den Masterzielserver einzurichten.
4. Wählen Sie **Install master target** > **Next** (Masterziel installieren > Weiter). ![choose-MT.PNG](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Wählen Sie den Standardspeicherort für die Installation, und klicken Sie auf **Installieren**. ![MT-installation](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Klicken Sie auf **Proceed to Configuration** (Mit der Konfiguration fortfahren), um das Masterziel beim Konfigurationsserver zu registrieren. ![MT-proceed-configuration.PNG](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Geben Sie die IP-Adresse des Konfigurationsservers und die Passphrase ein. [Klicken Sie hier](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase), um zu erfahren, wie Sie eine Passphrase generieren.![cs-ip-passphrase](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. Klicken Sie auf **Registrieren**, und klicken Sie nach der Registrierung auf **Fertig stellen**.
9. Nach der erfolgreichen Registrierung ist dieser Server im Portal unter **Recovery Services-Tresor** > **Site Recovery-Infrastruktur** > **Konfigurationsserver** als Masterzielserver des entsprechenden Konfigurationsservers aufgeführt.

 >[!NOTE]
 >Sie können auch [hier](https://aka.ms/latestmobsvc) die aktuelle Version des einheitlichen Setups für den Masterzielserver für Windows herunterladen.

## <a name="next-steps"></a>Nächste Schritte

Laden Sie den [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner) herunter, und installieren Sie ihn.
