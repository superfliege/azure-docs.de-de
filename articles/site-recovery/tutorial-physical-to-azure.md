---
title: "Einrichten der Notfallwiederherstellung in Azure für physische lokale Server mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Notfallwiederherstellung in Azure für lokale Windows- und Linux-Server mit dem Azure Site Recovery-Dienst einrichten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 805f6946-c6da-491f-980e-bf724bebdf0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 2f9a5092d4dacce8aa1630830499635b3b4eb151
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Einrichten der Notfallwiederherstellung in Azure für physische lokale Server

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie zur Notfallwiederherstellung, indem Replikation, Failover und Failback von lokalen Computern und virtuellen Azure-Computern (Virtual Machines, VMs) verwaltet und orchestriert werden.

In diesem Tutorial erfahren Sie, wie Sie die Notfallwiederherstellung von physischen lokalen Windows- und Linux-Servern in Azure einrichten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten von Azure und lokale Voraussetzungen
> * Erstellen eines Recovery Services-Tresors für Site Recovery 
> * Einrichten der Quell- und Zielumgebungen für die Replikation
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für einen Server

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Stellen Sie sicher, dass Sie die [Architektur und die Komponenten des Szenarios](concepts-physical-to-azure-architecture.md) verstehen.
- Überprüfen Sie die [Supportanforderungen](site-recovery-support-matrix-to-azure.md) für alle Komponenten.
- Überprüfen Sie, ob die Server, die Sie replizieren möchten, die [Azure-VM-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllen.
- Bereiten Sie Azure vor. Sie benötigen ein Azure-Abonnement, ein virtuelles Azure-Netzwerk und ein Speicherkonto.
- Vorbereiten eines Kontos für die automatische Installation des Mobility Service auf jedem Server, den Sie replizieren möchten.



### <a name="set-up-an-azure-account"></a>Einrichten eines Azure-Kontos

Erstellen Sie ein [Microsoft Azure-Konto](http://azure.microsoft.com/).

- Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.
- Erfahren Sie mehr über [Site Recovery-Preise](site-recovery-faq.md#pricing), und erhalten Sie [Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/).
- Erfahren Sie, welche [Regionen](https://azure.microsoft.com/pricing/details/site-recovery/) für Site Recovery unterstützt werden.

### <a name="verify-azure-account-permissions"></a>Überprüfen der Azure-Kontoberechtigungen

Vergewissern Sie sich, dass Ihr Azure-Konto über die Berechtigungen für die Replikation von virtuellen Computern in Azure verfügt.

- Überprüfen Sie die [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines), die Sie für das Replizieren von Computern in Azure benötigen.
- Überprüfen Sie die Berechtigungen für den [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md), und passen Sie sie ggf. an. 



### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Richten Sie ein [Azure-Netzwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md) ein.

- Azure-VMs werden in dieses Netzwerk platziert, wenn sie nach dem Failover erstellt werden.
- Das Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.


## <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

Richten Sie ein [Azure Storage-Konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) ein.

- Site Recovery repliziert lokale Computer in den Azure-Speicher. Virtuelle Azure-Computer werden nach dem Failover aus dem Speicher erstellt.
- Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.
- Das Speicherkonto kann ein Standard- oder [Premium](../storage/common/storage-premium-storage.md)-Konto sein.
- Wenn Sie ein Premium-Konto eingerichtet haben, benötigen Sie auch ein zusätzliches Standard-Konto für die Protokolldaten.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobilitätsdienst muss auf jedem Computer installiert sein, den Sie replizieren möchten. Site Recovery installiert diesen Dienst automatisch, wenn Sie die Replikation für den Server aktivieren. Für die automatische Installation müssen Sie ein Konto vorbereiten, mit dem Site Recovery auf den Server zugreift.

- Sie können ein Domänenkonto oder ein lokales Konto verwenden
- Wenn Sie kein Domänenkonto verwenden, deaktivieren Sie für Windows-VMs die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer. Zu diesem Zweck fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
- Um den Registrierungseintrag zum Deaktivieren der Einstellung über eine Befehlszeilenschnittstelle hinzuzufügen, geben Sie Folgendes ein: ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Bei Linux muss auf dem Linux-Quellserver das root-Konto verwendet werden.


## <a name="create-a-vault"></a>Erstellen eines Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie die zu replizierenden Elemente und das Replikationsziel aus.

1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie unter **Schutzziel** die Option **In Azure** > **Nicht virtualisiert/Andere** aus.

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie den Konfigurationsserver ein, registrieren Sie ihn im Tresor, und ermitteln Sie die VMs.

1. Klicken Sie auf **Site Recovery** > **Infrastruktur vorbereiten** > **Quelle**.
2. Wenn Sie keinen Konfigurationsserver verwenden, klicken Sie auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/tutorial-physical-to-azure/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Registrieren des Konfigurationsservers im Tresor

Führen Sie zunächst folgende Schritte aus: 

- Stellen Sie auf dem Konfigurationsservercomputer sicher, dass die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) synchronisiert ist. Die Zeiten sollten übereinstimmen. Falls der Unterschied 15 Minuten beträgt, ist das Setup unter Umständen nicht erfolgreich.
- Stellen Sie sicher, dass der Computer auf die folgenden URLs zugreifen kann: [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- Bei Verwendung von auf IP-Adressen basierenden Firewallregeln muss die Kommunikation mit Azure möglich sein.
- Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
- Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

Führen Sie das einheitliche Setup als lokaler Administrator auf dem Konfigurationsserver aus. Standardmäßig werden auch der Prozessserver und der Masterzielserver auf dem Konfigurationscomputer installiert.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Nach Abschluss der Registrierung wird der Konfigurationsserver auf der Seite **Einstellungen** > **Server** im Tresor angezeigt.


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie das Ziel-Bereitstellungsmodell an.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/tutorial-physical-to-azure/network-storage.png)


## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
2. Geben Sie unter **Replikationsrichtlinie erstellen** einen Richtliniennamen an.
3. Geben Sie in **RPO-Schwellenwert** den RPO-Grenzwert (Recovery Point Objective) an. Mit diesem Wert wird angegeben, wie oft Datenwiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** die Größe des Aufbewahrungszeitfensters für die einzelnen Wiederherstellungspunkte in Stunden an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt (72 Stunden für Standardspeicher).
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

    ![Replikationsrichtlinie](./media/tutorial-physical-to-azure/replication-policy.png)


Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. Standardmäßig wird für das Failback automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** wird z.B. die Failbackrichtlinie **rep-policy-failback** erstellt. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.

## <a name="enable-replication"></a>Replikation aktivieren

Aktivieren Sie die Replikation für jeden Server.

- Site Recovery installiert den Mobility Service, wenn die Replikation aktiviert wird.
- Wenn Sie die Replikation für einen Server aktivieren, kann es länger als 15 Minuten dauern, bis die Änderungen wirksam sind und im Portal angezeigt werden.

1. Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Physische Computer** aus.
4. Wählen Sie den Prozessserver (Konfigurationsserver) aus. Klicken Sie dann auf **OK**.
5. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Azure-Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure verwendet werden soll (klassisch oder Resource Manager).
6. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten. 
7. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen.
8. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. 
9. Klicken Sie unter **Physische Computer** auf **+ Physischer Computer**. Geben Sie den Namen und die IP-Adresse an. Wählen Sie das Betriebssystem des Computers, den Sie replizieren möchten, aus. Es dauert einige Minuten, bis die Server ermittelt und aufgelistet werden. 
10. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll.
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. 
12. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.


Zum Überwachen der hinzugefügten Server können Sie den letzten Zeitpunkt der Ermittlung unter **Konfigurationsserver** > **Letzter Kontakt um** überprüfen. Wenn Sie Computer hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken) und klicken auf **Aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

[Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

