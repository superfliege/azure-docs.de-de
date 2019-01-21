---
title: Vorbereiten der Sicherung virtueller Azure-Computer mit Azure Backup
description: In diesem Artikel wird beschrieben, wie virtuelle Azure-Computer auf die Sicherung mit dem Azure Backup-Dienst vorbereitet werden.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: 65e4c6d66e410e8cd761128028b7a47e21db86eb
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354500"
---
# <a name="prepare-to-back-up-azure-vms"></a>Vorbereiten der Sicherung virtueller Azure-Computer

In diesem Artikel wird beschrieben, wie Sie die Sicherung eines virtuellen Azure-Computers (VM) mit einem Recovery Services-Tresor von [Azure Backup](backup-introduction-to-azure-backup.md) vorbereiten. Die Vorbereitung der Sicherung umfasst Folgendes:


> [!div class="checklist"]
> * Vorbereitung: Überprüfen der unterstützten Szenarios und Einschränkungen
> * Voraussetzungen: Überprüfen der Anforderungen von Azure-VMs und der Netzwerkkonnektivität
> * Erstellen eines Tresors.
> * Einrichten der Speicherreplikation
> * Ermitteln von VMs und Konfigurieren von Sicherungseinstellungen und Richtlinien
> * Aktivieren der Sicherung für ausgewählte VMs


> [!NOTE]
   > In diesem Artikel wird beschrieben, wie Sie Azure-VMs sichern, indem Sie einen Tresor einrichten und die zu sichernden VMs auswählen. Dies ist hilfreich, wenn Sie mehrere VMs sichern möchten. Sie können eine Azure-VM auch direkt über die zugehörigen VM-Einstellungen sichern. [Weitere Informationen](backup-azure-vms-first-look-arm.md)

## <a name="before-you-start"></a>Vorbereitung

1. [Verschaffen Sie sich einen Überblick](backup-azure-vms-introduction.md) über Azure Backup für Azure-VMs.
2. Überprüfen Sie die folgenden Einschränkungen und Informationen zur Unterstützung.

   **Unterstützung/Einschränkung** | **Details**
   --- | ---
   **Windows-Betriebssystem** | Windows Server 2008 R2 (64 Bit) oder höher.<br/><br/> Windows Client 7 (64-Bit) oder höher.
   **Linux-Betriebssystem** | Mit Ausnahme von CoreOS Linux können Sie alle 64-Bit-Linux-Verteilungen sichern, die [von Azure unterstützt](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) werden.<br/><br/> Überprüfen Sie, welche Linux-Betriebssysteme die [Dateiwiederherstellung unterstützen](backup-azure-restore-files-from-vm.md#for-linux-os).<br/><br/> Auch andere Linux-Distributionen können funktionieren, sofern der VM-Agent auf dem virtuellen Computer verfügbar ist und Python unterstützt wird. Diese Verteilungen werden jedoch nicht unterstützt.
   **Region** | Sie können Azure-VMs in allen [unterstützten Regionen](https://azure.microsoft.com/regions/#services) sichern. Wenn eine Region nicht unterstützt wird, können Sie diese beim Erstellen des Tresors nicht auswählen.<br/><br/> Sie können nicht über mehrere Azure-Regionen hinweg sichern und wiederherstellen. Dies funktioniert nur innerhalb einer Region.
   **Datenträgerlimit** | Sie können keine VMs mit mehr als 16 Datenträgern sichern.
   **Freigegebener Speicher** | Das Sichern von VMs mit CSV oder Dateiservern mit horizontaler Skalierung wird nicht empfohlen. CSV-Schreiber schlagen voraussichtlich fehl.
   **Linux-Verschlüsselung** | Das Sichern von virtuellen Linux-Computern, die mit Linux Unified Key Setup (LUKS) verschlüsselt sind, wird nicht unterstützt.
   **VM-Konsistenz:** | Azure Backup unterstützt keine Multi-VM-Konsistenz.
   **Netzwerk** | Im Netzwerk bereitgestellte und an einen virtuellen Computer angefügte Laufwerke werden nicht in die gesicherten Daten einbezogen.<br/><br/>
   **Momentaufnahmen** | Das Erstellen von Momentaufnahmen auf Datenträgern mit aktivierter Schreibbeschleunigung wird nicht unterstützt. Dies hindert Azure Backup am Erstellen einer App-konsistenten Momentaufnahme aller VM-Datenträger.
   **PowerShell** | Es gibt einige Aktionen, die nur mit PowerShell verfügbar sind:<br/><br/> – das Wiederherstellen von VMs, die von internen bzw. externen Lastenausgleichsmodulen oder mit mehreren reservierten IP-Adressen oder Adaptern verwaltet werden [Weitere Informationen](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations)<br/><br/> – das Wiederherstellen einer Domänencontroller-VM in einer Konfiguration mit mehreren Domänencontrollern [Weitere Informationen](backup-azure-arm-restore-vms.md#restore-domain-controller-vms)
   **Systemzeit** | Azure Backup unterstützt keine automatische Uhrzeitanpassung an die Sommerzeit für die Sicherungen von Azure-VMs. Sicherungsrichtlinien können bei Bedarf angepasst werden.
   **Speicherkonten** | Wenn Sie ein Speicherkonto mit Netzwerkeinschränkung verwenden, stellen Sie sicher, dass Sie die Option **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben** aktivieren, damit der Azure Backup-Dienst auf das Konto zugreifen kann. Die Wiederherstellung auf Elementebene wird für Speicherkonten mit Netzwerkeinschränkung nicht unterstützt.<br/><br/> Stellen Sie sicher, dass die Einstellung **Firewalls und virtuelle Netzwerke** eines Speicherkontos den Zugriff über **alle Netzwerke** zulässt.


## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen den Tresor in der gleichen Region wie die Azure-VMs erstellen, die gesichert werden sollen.
- Überprüfen Sie die Regionen der Azure-VMs, bevor Sie beginnen.
    - Wenn Sie über VMs in verschiedenen Regionen verfügen, sollten Sie in jeder dieser Regionen einen Tresor erstellen.
    - Zum Speichern der Sicherungsdaten müssen Sie keine Speicherkonten angeben. Der Tresor und der Azure Backup-Dienst nehmen dies automatisch vor.
- Stellen Sie sicher, dass der VM-Agent auf den Azure-VMs installiert ist, die gesichert werden sollen.

### <a name="install-the-vm-agent"></a>Installieren des VM-Agents

Azure Backup installiert eine Sicherungserweiterung (VM Snapshot oder VM Snapshot Linux) auf dem VM-Agent, der auf der Azure-VM ausgeführt wird.
    -  Der Azure-VM-Agent wird standardmäßig auf jeder Windows-VM installiert, die auf der Grundlage eines Azure Marketplace-Image bereitgestellt wird. Wenn Sie ein Azure Marketplace-Image über das Portal, mithilfe von PowerShell, über die CLI (Befehlszeilenschnittstelle) oder unter Verwendung einer Azure Resource Manager-Vorlage bereitstellen, wird der Azure-VM-Agent ebenfalls installiert.
    - Wenn Sie eine VM von einem lokalen Speicher in Azure migriert haben, ist der Agent noch nicht installiert, und Sie müssen die Installation durchführen, bevor Sie die Sicherung für die VM aktivieren können.

Installieren Sie den Agent bei Bedarf wie folgt:

**VM** | **Details**
--- | ---
**Virtuelle Windows-Computer** | [Laden Sie den Agent herunter](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409), und installieren Sie ihn mit Administratorberechtigungen auf der VM.<br/><br/> Unter *C:\WindowsAzure\Packages* können Sie sich vergewissern, dass die Installation erfolgreich abgeschlossen wurde, indem Sie mit der rechten Maustaste auf „WaAppAgent.exe“ klicken und dann auf **Eigenschaften** > Registerkarte **Details** klicken. Für **Produktversion** sollte 2.6.1198.718 oder eine höhere Version angegeben sein.
**Virtuelle Linux-Computer** | Für die Installation und Upgrades des Azure Linux-Agents sollte nach Möglichkeit ein RPM- oder DEB-Paket aus dem Paketrepository Ihrer Verteilung installiert werden. Das Azure Linux-Agent-Paket wird von allen [unterstützten Distributionsanbietern](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) in ihre jeweiligen Images und Repositorys integriert. Der Agent steht auf [GitHub](https://github.com/Azure/WALinuxAgent) zur Verfügung, jedoch wird die Installation über GitHub nicht empfohlen.
Falls beim Sichern des virtuellen Azure-Computers Probleme auftreten, vergewissern Sie sich mithilfe der folgenden Tabelle, dass der Azure-VM-Agent auf dem virtuellen Computer ordnungsgemäß installiert ist. Die Tabelle enthält weitere Informationen zum VM-Agent für virtuelle Windows- und Linux-Computer.

### <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung

Die Sicherungserweiterung, die auf der VM ausgeführt wird, muss über ausgehenden Zugriff auf öffentliche Azure-IP-Adressen verfügen.

> [!NOTE]
> Für die Kommunikation des virtuellen Azure-Computers mit dem Azure Backup-Dienst ist kein expliziter ausgehender Netzwerkzugriff erforderlich. Auf bestimmten älteren virtuellen Computern können jedoch Probleme und der Fehler **ExtensionSnapshotFailedNoNetwork** auftreten. Zur Umgehung dieses Fehlers wählen Sie eine der folgenden Optionen aus, damit die Sicherungserweiterung mit öffentlichen Azure-IP-Adressen kommunizieren kann, um einen freien Pfad für den Sicherungsdatenverkehr bereitzustellen.

- **NSG-Regeln:** Lassen Sie die [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) zu. Sie können eine Regel hinzufügen, die den Zugriff auf den Azure Backup-Dienst mithilfe eines [Diensttags](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure) zulässt, anstatt jeden Adressbereich individuell zuzulassen und diese mit der Zeit zu verwalten. Weitere Informationen zum Diensttag finden Sie in diesem [Artikel](../virtual-network/security-overview.md#service-tags).
- **Proxy:** Stellen Sie einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereit.
- **Azure Firewall:** Lassen Sie auf dem virtuellen Computer den Datenverkehr über Azure Firewall mithilfe eines FQDN-Tags für den Azure Backup-Dienst zu.

Berücksichtigen Sie die Vor- und Nachteile bei Ihrer Entscheidung.

**Option** | **Vorteile** | **Nachteile**
--- | --- | ---
**NSG** | Keine zusätzlichen Kosten. Einfache Verwaltung mit Diensttags. | Ermöglicht den Zugriff auf Azure insgesamt, nicht nur auf den Speicher. |
**HTTP-Proxy** | Die Feinsteuerung über die Speicher-URLs ist möglich.<br/><br/> Zentraler Punkt für Internetzugriff auf virtuelle Computer.<br/><br/> Zusätzliche Kosten für den Proxy.
**FQDN-Tags** | Einfache Verwendung, wenn Azure Firewall in einem VNet-Subnetz eingerichtet ist. | Es können keine eigenen FQDN-Tags erstellt werden, und die FQDNs in einem Tag können nicht geändert werden.

Wenn Sie verwaltete Azure-Datenträger verwenden, müssen Sie in den Firewalls möglicherweise einen weiteren Port öffnen (Port 8443).

### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Einrichten einer NSG-Regel (Netzwerksicherheitsgruppe) zum Zulassen des ausgehenden Zugriffs auf Azure

Wenn der Zugriff Ihrer Azure-VM durch eine NSG verwaltet wird, lassen Sie den ausgehenden Zugriff auf die erforderlichen Bereiche und Ports für den Sicherungsspeicher zu.

1. Klicken Sie in der VM unter **Netzwerk** auf **Regel für ausgehenden Port hinzufügen**.

  - Wenn Sie über eine Regel verfügen, die den Zugriff verhindert, muss die neue Zulassungsregel höhergestellt werden. Wenn Sie beispielsweise über eine **Deny_All**-Regel mit der Priorität 1.000 verfügen, muss für Ihre neue Regel ein Wert unter 1.000 festgelegt werden.
2. Klicken Sie unter **Ausgangssicherheitsregel hinzufügen** auf **Erweitert**.
3. Wählen Sie im Feld **Quelle** den Eintrag **VirtualNetwork** aus.
4. Geben Sie für **Quellportbereiche** ein Sternchen (*) ein, um ausgehenden Zugriff über alle Ports zuzulassen.
5. Wählen Sie **Diensttag** als **Ziel** aus. Wählen Sie „Storage.<region>“ aus der Liste aus. Wählen dabei die Region aus, in der sich der Tresor und die VMs befinden, die gesichert werden sollen.
6. Wählen Sie unter **Zielportbereiche** den entsprechenden Port aus.

    - Für VMs mit nicht verwalteten Datenträgern und nicht verschlüsseltem Speicherkonto: 80
    - Für VMs mit nicht verwalteten Datenträgern und verschlüsseltem Speicherkonto: 443 (Standardeinstellung)
    - Für verwaltete VMs: 8443
7. Wählen Sie für **Protokoll** die Option **TCP** aus.
8. Geben Sie unter **Priorität** einen Prioritätswert ein, der geringer als alle höheren Ablehnungsregeln ist.
9. Geben Sie einen Namen und eine Beschreibung für die Regel ein, und klicken Sie dann auf **OK**.

Sie können die NSG-Regel auf mehrere VMs anwenden, um Azure Backup den ausgehenden Zugriff auf Azure zu genehmigen.

Dieses Video führt Sie durch diesen Prozess.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!WARNING]
> Diensttags für Speicher befinden sich in der Vorschau. Sie sind nur in bestimmten Regionen verfügbar. Eine Liste der Regionen finden Sie unter [Diensttags](../virtual-network/security-overview.md#service-tags).

### <a name="route-backup-traffic-through-a-proxy"></a>Routen des Sicherungsdatenverkehrs über einen Proxy

Sie können den Sicherungsdatenverkehr über ein Proxy routen und dem Proxy dann den Zugriff auf die erforderlichen Azure-Bereiche gewähren.
Sie sollten Ihre Proxy-VM wie folgt konfigurieren:

- Die Azure-VM sollte jeglichen HTTP-Datenverkehr an das öffentliche Internet über den Proxy routen.
- Der Proxy sollte jeglichen eingehenden Datenverkehr von VMs im entsprechenden virtuellen Netzwerk (VNet) zulassen.
- Die NSG **NSF-lockdown** benötigt eine Regel, die ausgehenden Internetdatenverkehr vom virtuellen Proxy-Computer zulässt.

Im Folgenden wird das Einrichten des Proxys beschrieben. Dabei werden Beispielwerte verwendet. Fügen Sie stattdessen Ihre eigenen Werte ein.

#### <a name="set-up-a-system-account-proxy"></a>Einrichten eines Proxys für das Systemkonto
Wenn Sie über keinen Proxy für das Systemkonto verfügen, richten Sie wie folgt einen ein:

1. Laden Sie [PsExec](https://technet.microsoft.com/sysinternals/bb897553) herunter.
2. Führen Sie den Befehl **PsExec.exe -i -s cmd.exe** aus, um die Eingabeaufforderung mit einem Systemkonto auszuführen.
3. Führen Sie den Browser im Systemkontext aus. Beispiel:  **PROGRAMFILES%\Internet Explorer\iexplore.exe** für Internet Explorer.  
4. Definieren Sie die Proxyeinstellungen.
    - Unter Linux:
        - Fügen Sie die folgende Zeile in die Datei **/etc/environment** ein:
            - **http_proxy=http://proxy IP address:proxy port**
        - Fügen Sie die folgenden Zeilen in die Datei **/etc/waagent.conf** ein:
            - **HttpProxy.Host=proxy IP address**
            - **HttpProxy.Port=proxy port**
    - Legen Sie unter Windows in den Browsereinstellungen fest, dass ein Proxy verwendet werden soll. Wenn Sie derzeit einen Proxy für ein Benutzerkonto verwenden, können Sie mithilfe des folgenden Skripts die Einstellung auf Systemkontoebene anwenden.
        ```
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

#### <a name="allow-incoming-connections-on-the-proxy"></a>Zulassen von eingehenden Verbindungen über den Proxy

Lassen Sie eingehende Verbindungen in den Proxyeinstellungen zu.

- Öffnen Sie beispielsweise **Windows Firewall mit erweiterter Sicherheit**.
    - Klicken Sie mit der rechten Maustaste auf **Eingangsregeln** > **Neue Regel**.
    - Klicken Sie unter **Regeltyp** auf **Benutzerdefiniert** > **Weiter**.
    - Klicken Sie unter **Programm** auf **Alle Programme** > **Weiter**.
    - Legen Sie unter **Protokolle und Ports** den Typ **TCP**, für **Lokale Ports** die Option **Bestimmte Ports** und für **Remoteport** die Option **Alle Ports** fest.
    - Beenden Sie den Assistenten, und legen Sie einen Namen für die Regel fest.

#### <a name="add-an-exception-rule-to-the-nsg"></a>Hinzufügen einer Ausnahmeregel zur NSG

Lassen Sie für die NSG **NSF-lockdown** den Datenverkehr über alle Ports unter 10.0.0.5 an alle Internetadressen über Port 80 (HTTP) oder 443 (HTTPS) zu.

- Das folgende PowerShell-Skript enthält ein Beispiel zum Zulassen des Datenverkehrs.
- Anstatt den ausgehenden Datenverkehr an alle öffentlichen Internetadressen zuzulassen, können Sie einen IP-Adressbereich (-DestinationPortRange) festlegen oder den Diensttag „storage.region“ verwenden.   

    ```
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```

### <a name="allow-firewall-access-with-fqdn-tag"></a>Zulassen des Firewallzugriffs mithilfe des FQDN-Tags

Sie können Azure Firewall so konfigurieren, dass der ausgehende Zugriff für Netzwerkdatenverkehr an Azure Backup zugelassen wird.

- [Weitere Informationen](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) zum Bereitstellen von Azure Firewall.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/firewall/fqdn-tags) über FQDN-Tags.

## <a name="create-a-vault"></a>Erstellen eines Tresors

Ein Recovery Services-Tresor für Azure Backup speichert Sicherungen und Wiederherstellungspunkte, die im Laufe der Zeit erstellt wurden, sowie Sicherungsrichtlinien, die zu gesicherten VMs gehören. Erstellen Sie wie folgt einen Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im Menü **Hub** die Option **Durchsuchen** aus, und geben Sie dann **Recovery Services** ein. Sobald Sie mit der Eingabe beginnen, wird die Ressourcenliste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore**.

    ![Eingabe im Feld und Auswahl von „Recovery Services-Tresore“ in den Ergebnissen](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.
3. Wählen Sie im Menü **Recovery Services-Tresore** die Option **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Der Bereich **Recovery Services-Tresore** wird geöffnet. Sie werden aufgefordert, Informationen für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** anzugeben.

    ![Bereich „Recovery Services-Tresore“](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird.
    - Der Name muss für das Azure-Abonnement eindeutig sein.
    - Er kann zwischen 2 bis 50 Zeichen enthalten.
    - Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
5. Wählen Sie **Abonnement** aus, um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
6. Wählen Sie **Ressourcengruppe** aus, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen. Weitere Informationen zu [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).
7. Wählen Sie **Standort** aus, um die geografische Region für den Tresor auszuwählen. Der Tresor *muss* sich in derselben Region wie die zu sichernden VMs befinden.
8. Klicken Sie auf **Erstellen**.
    - Es kann eine Weile dauern, bis der Tresor fertiggestellt wird.
    - Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal.
    ![Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

## <a name="set-up-storage-replication"></a>Einrichten der Speicherreplikation

Standardmäßig verfügt Ihr Tresor über einen [georedundanten Speicher (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). GRS wird als primäre Sicherung empfohlen, dennoch können Sie [lokal redundanten Speicher](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) als günstigere Alternative nutzen.

Passen Sie die Speicherreplikation wie folgt an:

1. Klicken Sie im Tresor auf **Sicherungsinfrastruktur**, und klicken Sie dann auf **Sicherungskonfiguration**.

   ![Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/full-blade.png)

2. Passen Sie die Speicherredundanzmethode in der **Sicherungskonfiguration** nach belieben an, und klicken Sie dann auf **Speichern**.


## <a name="configure-backup"></a>Konfigurieren der Sicherung

Ermitteln Sie die VMs im Abonnement, und konfigurieren Sie die Sicherung.

1. Klicken Sie im Tresor auf **Übersicht**, und klicken Sie dann auf **+ Sicherung**.

   ![Schaltfläche „Sicherung“](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Die Bereiche **Sicherung** und **Sicherungsziel** werden geöffnet.

2. Wählen Sie **Azure** unter **Sicherungsziel**> **Wo wird Ihre Workload ausgeführt?** aus. Klicken Sie unter **Was möchten Sie sichern?** auf **Virtueller Computer** >  **OK**. Dadurch wird die VM-Erweiterung im Tresor registriert.

   ![Bereiche „Sicherung“ und „Sicherungsziel“](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   In diesem Schritt wird die VM-Erweiterung im Tresor registriert. Der Bereich **Sicherungsziel** wird geschlossen, und der Bereich **Sicherungsrichtlinie** wird geöffnet.

3. Wählen Sie unter **Sicherungsrichtlinie** die Richtlinie aus, die dem Tresor zugeordnet werden soll. Klicken Sie dann auf **OK**.
    - Die Details zur Standardrichtlinie werden unter dem Dropdownmenü aufgeführt.
    - Klicken Sie auf **Neu erstellen**, um eine Richtlinie zu erstellen. Erfahren Sie mehr über das [Definieren einer Richtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).

    ![Bereiche „Sicherung“ und „Sicherungsrichtlinie“](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Wählen Sie im Bereich **Virtuelle Computer auswählen** die VMs aus, für die die festgelegte Sicherungsrichtlinie verwendet werden soll, und klicken Sie dann auf **OK**.

    - Die ausgewählte VM wird überprüft.
    - Sie können nur VMs auswählen, die sich in der gleichen Region wie der Tresor befinden. VMs können nur in einem Tresor gesichert werden.

   ![Bereich „Virtuelle Computer auswählen“](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. Klicken Sie unter **Sicherung** auf **Sicherung aktivieren**.

   - Damit wird die Richtlinie für den Tresor und die VMs bereitgestellt, und die Sicherungserweiterung wird auf dem VM-Agent auf der Azure-VM installiert.
   - Mit diesem Schritt wird der erste Wiederherstellungspunkt für die VM nicht erstellt.

   ![Schaltfläche „Sicherung aktivieren“](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nach dem Aktivieren der Sicherung:

- Die Sicherungsrichtlinie wird gemäß Ihres Sicherungszeitplans ausgeführt.
- Die Sicherungserweiterung wird vom Sicherungsdienst unabhängig davon installiert, ob der virtuelle Computer ausgeführt wird oder nicht.
    - Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt.
    -  Der virtuelle Computer wird jedoch auch gesichert, wenn er ausgeschaltet ist und die Erweiterung nicht installiert werden kann. Dies wird als *Offline-VM* bezeichnet. In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent*.
- Wenn Sie für die VM sofort eine bedarfsgesteuerte Sicherung generieren möchten, klicken Sie unter **Sicherungselemente** auf die Auslassungspunkte (...) neben der VM, und klicken Sie dann auf **Jetzt sichern**.


## <a name="next-steps"></a>Nächste Schritte

- Beheben von Problemen, die bei [Azure-VM-Agents](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) oder [Azure-VM-Sicherungen](backup-azure-vms-troubleshoot.md) auftreten können.
- [Sichern virtueller Azure-Computer](backup-azure-vms-first-look-arm.md)
