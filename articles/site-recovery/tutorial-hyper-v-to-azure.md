---
title: "Einrichten der Notfallwiederherstellung für lokale Hyper-V-VMs in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Notfallwiederherstellung für lokale Hyper-V-VMs in Azure mit dem Azure Site Recovery-Dienst einrichten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 45e9b4dc-20ca-4c14-bee3-cadb8d9b8b24
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 96e5027adfb443aba18895213e8d83894e3f060a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Einrichten der Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie zur Notfallwiederherstellung, indem Replikation, Failover und Failback von lokalen Computern und virtuellen Azure-Computern (Virtual Machines, VMs) verwaltet und orchestriert werden.

In diesem Tutorial erfahren Sie, wie Sie die Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure einrichten. Das Tutorial gilt für Hyper-V-VMs, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden, und für solche, die nicht dort verwaltet werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten von Azure und lokale Voraussetzungen
> * Erstellen eines Recovery Services-Tresors für Site Recovery 
> * Einrichten der Quell- und Zielumgebungen für die Replikation 
> * Einrichten der Netzwerkzuordnung (wenn Hyper-V von System Center VMM verwaltet wird)
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Stellen Sie sicher, dass Sie die [Architektur und die Komponenten des Szenarios](concepts-hyper-v-to-azure-architecture.md) verstehen.
- Überprüfen Sie die [Supportanforderungen](site-recovery-support-matrix-to-azure.md) für alle Komponenten.
- Überprüfen Sie, ob die virtuellen Computer, die Sie replizieren möchten, die [Azure-VM-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllen.
- So planen Sie die Kapazität
    - Verwenden Sie das Tool [Hyper-V Capacity Planner](http://aka.ms/asr-capacity-planner-excel), um die Änderungsrate zu ermitteln.
    - Verwenden Sie den [Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) zur Analyse Ihrer Quellumgebung, zum Schätzen der Bandbreite und zum Überprüfen der Zielanforderungen.
- Bereiten Sie Azure vor. Sie benötigen ein Azure-Abonnement, ein virtuelles Azure-Netzwerk und ein Speicherkonto.
- Bereiten Sie die lokalen Hyper-V-Hosts und ggf. die VMM-Server vor.





### <a name="set-up-an-azure-account"></a>Einrichten eines Azure-Kontos

Erstellen Sie ein [Microsoft Azure-Konto](http://azure.microsoft.com/).

- Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.
- Erfahren Sie mehr über [Site Recovery-Preise](site-recovery-faq.md#pricing), und erhalten Sie [Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/).
- Erfahren Sie, welche [Regionen](https://azure.microsoft.com/pricing/details/site-recovery/) für Site Recovery unterstützt werden.

### <a name="verify-azure-account-permissions"></a>Überprüfen der Azure-Kontoberechtigungen

Vergewissern Sie sich, dass Ihr Azure-Konto über die Berechtigungen zum Replizieren virtueller Computer verfügt.

- Überprüfen der [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) für das Replizieren von Computern in Azure
- Überprüfen Sie die Berechtigungen für den [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md), und passen Sie sie ggf. an. 


### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Richten Sie ein [Azure-Netzwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md) ein.

- Azure-VMs werden in dieses Netzwerk platziert, wenn sie nach dem Failover erstellt werden.
- Das Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.


### <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

Richten Sie ein [Azure Storage-Konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) ein.

- Site Recovery repliziert lokale Computer in den Azure-Speicher. Virtuelle Azure-Computer werden nach dem Failover aus dem Speicher erstellt.
- Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.
- Das Speicherkonto kann ein Standard- oder [Premium](../storage/common/storage-premium-storage.md)-Konto sein.
- Wenn Sie ein Premium-Konto eingerichtet haben, benötigen Sie ein zusätzliches Standardkonto für die Protokolldaten.

### <a name="prepare-hyper-v-hosts"></a>Vorbereiten der Hyper-V-Hosts

1. Überprüfen Sie, ob die Hyper-V-Hosts die [Supportanforderungen](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) erfüllen.
2. Stellen Sie sicher, dass die Hosts auf die folgenden URLs zugreifen können:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Bei Verwendung von auf IP-Adressen basierenden Firewallregeln muss die Kommunikation mit Azure möglich sein.
    - Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
    - Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

### <a name="prepare-vmm-servers"></a>Vorbereiten der VMM-Server

Wenn Hyper-V-Hosts von VMM verwaltet werden, müssen Sie den lokalen VMM-Server vorbereiten. 

- Überprüfen Sie, ob der VMM-Server die [Supportanforderungen](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) erfüllt.
- Stellen Sie sicher, dass der VMM-Server über mindestens eine Cloud mit mindestens einer Hostgruppe verfügt. Der Hyper-V-Host, auf dem virtuelle Computer ausgeführt werden, sollte sich in der Cloud befinden.
- Der VMM-Server benötigt Zugriff auf das Internet und die folgenden URLs:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Bei Verwendung von auf IP-Adressen basierenden Firewallregeln muss die Kommunikation mit Azure möglich sein.
    - Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
    - Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.
- Bereiten Sie den VMM-Server für die Netzwerkzuordnung vor.


#### <a name="prepare-vmm-for-network-mapping"></a>Vorbereiten von VMM für die Netzwerkzuordnung

Bei Verwendung von VMM gilt die [Netzwerkzuordnung](site-recovery-network-mapping.md) zwischen lokalen VMM-VM-Netzwerken und virtuellen Azure-Netzwerken. Die Zuordnung stellt sicher, dass Azure-VMs mit dem richtigen Netzwerk verbunden sind, wenn sie nach einem Failover erstellt werden.

Bereiten Sie VMM wie folgt für die Netzwerkzuordnung vor:

1. Bereiten Sie ein [logisches VMM-Netzwerk](https://docs.microsoft.com/system-center/vmm/network-logical) vor, das der Cloud zugeordnet ist, in der sich die Hyper-V-Hosts befinden.
2. Stellen Sie sicher, dass Sie über ein [VM-Netzwerk](https://docs.microsoft.com/system-center/vmm/network-virtual) verfügen, das mit dem logischen Netzwerk verknüpft ist.
3. Verbinden Sie die virtuellen Computer mit dem VM-Netzwerk.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie die zu replizierenden Elemente und das Replikationsziel aus.

1. Klicken Sie im Tresor auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
2. Wählen Sie unter **Schutzziel** die Option **To Azure**>  (Zu Azure) und dann **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. 
    - Wenn Hyper-V-Hosts nicht von VMM verwaltet werden, wählen Sie **Nein** aus, um zu bestätigen, dass Sie VMM nicht verwenden.
    - Wenn Hosts in VMM-Clouds verwaltet werden, wählen Sie **Ja** aus.

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Bei der Einrichtung der Quellumgebung installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent und registrieren die lokalen Server im Tresor. 

1. Klicken Sie unter **Bereiten Sie die Infrastruktur vor** auf **Quelle**. 
    - Wenn Sie VMM nicht verwenden, klicken Sie auf **+ Hyper-V-Site**, und geben Sie einen Standortnamen an. Klicken Sie dann auf **+ Hyper-V-Server**, und fügen Sie dem Standort einen Host oder Cluster hinzu.
    - Klicken Sie, wenn Sie VMM verwenden, unter **Quelle vorbereiten** auf **+ VMM**, um einen VMM-Server hinzuzufügen. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **System Center VMM-Server** angezeigt wird.
2. Laden Sie die Anbieter- und Agent-Komponenten für Ihre Umgebung herunter.
    - Laden Sie die Installationsdatei für den Anbieter herunter (nur bei Hyper-V). Sie führen die Datei auf jedem Hyper-V-Host aus, um den Anbieter und den Agent zu installieren.
        
        ![Anbieter ohne VMM](./media/tutorial-hyper-v-to-azure/download-no-vmm.png)
    
    - Laden Sie für Hyper-V mit VMM den Anbieter und den Agent separat herunter. Führen Sie die Anbieterinstallation auf dem VMM-Server aus. Führen Sie die Agent-Installation auf jedem Hyper-V-Host aus.
    
        ![Anbieter und Agent mit VMM](./media/tutorial-hyper-v-to-azure/download-vmm.png)
    
3. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des Anbietersetups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

### <a name="install-components"></a>Installieren von Komponenten

Installieren Sie die Komponenten gemäß den Angaben in der Tabelle. 

**Komponente** | **Details** | **Nur Hyper-V** | **Hyper-V mit VMM**
--- | --- | --- | ---
**Azure Site Recovery-Anbieter** | Orchestriert die Replikation in Azure | Installation auf jedem Hyper-V-Host | Installation auf dem VMM-Server
**Recovery Services-Agent** | Führt die Datenreplikation durch | Installation auf jedem Hyper-V-Host | Installation auf dem Hyper-V-Host


#### <a name="install-the-provider-on-hyper-v-without-vmm"></a>Installieren des Anbieters unter Hyper-V ohne VMM

Installieren Sie den Anbieter auf jedem Hyper-V-Host, den Sie dem Hyper-V-Standort hinzugefügt haben. Wenn Sie auf einem Hyper-V-Cluster installieren, führen Sie das Setup auf jedem Clusterknoten aus. Durch das Installieren und Registrieren jedes Hyper-V-Clusterknotens wird sichergestellt, dass virtuelle Computer geschützt sind, selbst wenn sie über Knoten hinweg migrieren.

1. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
2. Ändern Sie unter **Installation** den Speicherort für die Anbieterinstallation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.
4. Klicken in **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen, die Sie heruntergeladen haben. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen und den Hyper-V-Standort an, zu dem der Hyper-V-Server gehört.
5. Geben Sie unter **Proxyeinstellungen** an, wie der auf dem VMM-Server oder auf Hyper-V-Hosts ausgeführte Anbieter eine Internetverbindung mit Site Recovery herstellen soll.
    * Für eine direkte Verbindung wählen Sie **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)** aus.
    * Bei einem Proxy wählen Sie **Unter Verwendung eines Proxyservers mit Azure Site Recovery verbinden** aus. Geben Sie ggf. Adresse, Port und Anmeldeinformationen des Proxys ein.
6. Nachdem der Server im Tresor registriert wurde, klicken Sie auf **Fertig stellen**.

Azure Site Recovery ruft die Metadaten vom Hyper-V-Server ab, und der Server wird in **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt.


#### <a name="install-the-recovery-services-agent-on-hyper-v-host-without-vmm"></a>Installieren des Recovery Services-Agents auf Hyper-V-Hosts ohne VMM

Wenn Sie in Ihrer Bereitstellung VMM verwenden, führen Sie das Setup für den Recovery Services Agent auf jedem Hyper-V-Host aus.

1. Klicken Sie im Setup-Assistenten in der **Voraussetzungsprüfung** auf **Weiter**. Alle fehlenden Komponenten, die Voraussetzung sind, werden automatisch installiert.

    ![Voraussetzungen Recovery Services-Agent](./media/tutorial-hyper-v-to-azure/hyperv-agent-prerequisites.png)
3. Übernehmen oder ändern Sie in den **Installationseinstellungen** den Installationsspeicherort und den Cachespeicherort. Das Cachelaufwerk benötigt mindestens 5 GB Speicherplatz. Es wird empfohlen, ein Laufwerk mit mindestens 600 GB freiem Speicherplatz zu verwenden. Klicken Sie dann auf **Weiter**.
4. Klicken Sie nach Abschluss der Installation in **Installation** auf **Schließen**, um den Assistenten zu beenden.

##### <a name="set-up-internet-access-via-a-proxy"></a>Einrichten von Internetzugriff über einen Proxy

Für den Recovery Services-Agent auf dem Hyper-V-Host ist für die VM-Replikation Internetzugriff auf Azure erforderlich. Gehen Sie bei der Einrichtung wie folgt vor, wenn Sie über einen Proxy auf das Internet zugreifen:

1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hyper-V-Host. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.
3. Geben Sie auf der Registerkarte **Proxykonfiguration** Informationen zum Proxy an.

    ![MARS-Agent registrieren](./media/tutorial-hyper-v-to-azure/mars-proxy.png)
4. Überprüfen Sie, ob der Agent die [erforderlichen URLs](#prepare-hyper-v-hosts) erreichen kann.

#### <a name="install-the-provider-on-the-vmm-server-hyper-v-with-vmm"></a>Installieren des Anbieters auf dem VMM-Server (Hyper-V mit VMM)

1. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
2. Ändern Sie unter **Installation** den Speicherort für die Anbieterinstallation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**. 
3. Registrieren Sie nun den VMM-Server im Tresor. Klicken in **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen, die Sie heruntergeladen haben. Geben Sie das Azure Site Recovery-Abonnement und den Tresornamen an.

    ![Serverregistrierung](./media/tutorial-hyper-v-to-azure/provider-vault-settings.png)

4. Geben Sie unter **Proxyeinstellungen** an, wie der auf dem VMM-Server oder auf Hyper-V-Hosts ausgeführte Anbieter eine Internetverbindung mit Site Recovery herstellen soll.

    * Für eine direkte Verbindung wählen Sie **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)** aus.
    * Bei einem Proxy wählen Sie **Unter Verwendung eines Proxyservers mit Azure Site Recovery verbinden** aus. Geben Sie ggf. Adresse, Port und Anmeldeinformationen des Proxys ein.
    - Es wird automatisch ein ausführendes VMM-Konto (DRAProxyAccount) mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass dieses Konto erfolgreich authentifiziert werden kann. Die Einstellungen für ausführende Konten können über die VMM-Konsole unter **Einstellungen** > **Sicherheit** > **Ausführende Konten** geändert werden. Starten Sie den VMM-Dienst neu, um die Änderungen zu übernehmen.
5. Geben Sie in **Datenverschlüsselung** an, ob alle an Azure gesendeten Daten verschlüsselt werden sollen. Bei Auswahl dieser Option stellt Site Recovery ein Zertifikat aus. Sie benötigen dieses Zertifikat zu einem späteren Zeitpunkt zum Entschlüsseln von Daten.
6. Geben Sie unter **VMM-Server** einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an. Wählen Sie unter **Cloudmetadaten synchronisieren** aus, ob Sie Metadaten für alle Clouds auf dem VMM-Server mit dem Tresor synchronisieren möchten. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren.

Nach der Registrierung ruft Azure Site Recovery Metadaten vom Server ab, und der VMM-Server wird in **Site Recovery-Infrastruktur** angezeigt.






## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie. 

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**.
2. Wählen Sie das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die Azure-VMs nach dem Failover erstellen möchten. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll.

3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

## <a name="configure-network-mapping-with-vmm"></a>Konfigurieren der Netzwerkzuordnung (mit VMM)

Wenn Sie VMM verwenden, richten Sie die Netzwerkzuordnung ein.

1. Klicken Sie unter **Site Recovery-Infrastruktur** > **Netzwerkzuordnungen** > **Netzwerkzuordnung** auf das Symbol **+Netzwerkzuordnung**.
2. Wählen Sie unter **Netzwerkzuordnung hinzufügen** den VMM-Quellserver aus. Wählen Sie **Azure** als Ziel aus.
3. Überprüfen Sie das Abonnement und das Bereitstellungsmodell nach einem Failover.
4. Wählen Sie unter **Quellnetzwerk** das lokale VM-Quellnetzwerk aus.
5. Wählen Sie unter **Zielnetzwerk** das Azure-Netzwerk aus, in dem Replikate von Azure-VMs nach der Erstellung im Anschluss an ein Failover platziert werden. Klicken Sie dann auf **OK**.

    ![Netzwerkzuordnung](./media/tutorial-hyper-v-to-azure/network-mapping-vmm.png)

## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an.
3. Geben Sie unter **Kopierhäufigkeit**an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).

    > [!NOTE]
    >  Eine Häufigkeit von 30 Sekunden wird bei der Replikation nach Storage Premium nicht unterstützt. Die Einschränkung richtet sich nach der Anzahl von Momentaufnahmen pro Blob (100), die von Storage Premium unterstützt wird. [Weitere Informationen](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Arten von Momentaufnahmen:
    - **Standardmomentaufnahme:** Bietet eine inkrementelle Momentaufnahme des gesamten virtuellen Computers
    - **App-konsistente Momentaufnahme:** Erstellt eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers. VSS (Volume Shadow Copy Service, Volumeschattenkopie-Dienst) stellt sicher, dass Apps zum Zeitpunkt der Momentaufnahme konsistent sind. Das Aktivieren anwendungskonsistenter Momentaufnahmen wirkt sich auf Quell-VMs auf die App-Leistung aus. Legen Sie einen Wert fest, der geringer ist als die Anzahl der konfigurierten zusätzlichen Wiederherstellungspunkte.
6. Geben Sie unter **Startzeit der ersten Replikation**an, wann die erste Replikation starten soll. Da die Replikation über Ihre Internetbandbreite durchgeführt wird, ist es ratsam, sie außerhalb der Spitzenzeiten einzuplanen.
7. Geben Sie unter **In Azure gespeicherte Daten verschlüsseln**an, ob ruhende Daten im Azure-Speicher verschlüsselt werden sollen. Klicken Sie dann auf **OK**.

    ![Replikationsrichtlinie](./media/tutorial-hyper-v-to-azure/replication-policy.png)


Wenn Sie eine neue Richtlinie erstellen, wird sie automatisch der VMM-Cloud oder dem Hyper-V-Standort zugeordnet.

## <a name="enable-replication"></a>Replikation aktivieren


1. Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**. 
2. Wählen Sie in **Quelle** den Hyper-V-Standort, den VMM-Server oder die Cloud aus. Klicken Sie dann auf **OK**.
3. Überprüfen Sie in **Ziel**, ob Azure als Ziel festgelegt ist, überprüfen Sie das Tresorabonnement, und vergewissern Sie sich, dass das Modell ausgewählt wurde, das Sie nach dem Failover in Azure verwenden möchten.
4. Wählen Sie das Speicherkonto für die Replikationsdaten und das Azure-Netzwerk, in dem Azure-VMs nach dem Failover platziert werden, aus.
5. Wählen Sie unter **Virtuelle Computer** > **Auswählen** die VMs aus, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

 Sie können den Fortschritt der Aktion **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nach der Ausführung des Auftrags **Schutz abschließen** ist die erste Replikation abgeschlossen und der virtuelle Computer bereit zum Failover.

## <a name="next-steps"></a>Nächste Schritte
[Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)
