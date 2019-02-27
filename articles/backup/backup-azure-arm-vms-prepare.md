---
title: Sichern virtueller Azure-Computer in einem Recovery Services-Tresor mit Azure Backup
description: Beschreibt die Sicherung virtueller Azure-Computer in einem Recovery Services-Tresor mit Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: e782afb971f95a654119d9817edeef02642bee9e
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447564"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Sichern virtueller Azure-Computer in einem Recovery Services-Tresor

Dieser Artikel beschreibt die Sicherung von Azure-VMs mit [Azure Backup](backup-overview.md) durch Bereitstellung und Aktivierung der Sicherung in einem Recovery Services-Tresor. 

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Überprüfen der unterstützten Szenarien und Voraussetzungen
> * Vorbereiten von Azure-VMs Installieren des Azure-VM-Agents (falls nötig) und Überprüfen des ausgehenden Zugriffs für virtuelle Computer
> * Erstellen eines Tresors
> * Einrichten von Speicher für den Tresor
> * Ermitteln von VMs und Konfigurieren von Sicherungseinstellungen und Richtlinien
> * Aktivieren der Sicherung für Azure-VMs


> [!NOTE]
   > In diesem Artikel wird beschrieben, wie Sie Azure-VMs sichern, indem Sie einen Tresor einrichten und die zu sichernden VMs auswählen. Dies ist hilfreich, wenn Sie mehrere VMs sichern möchten. Sie können eine Azure-VM auch direkt über die zugehörigen [VM-Einstellungen sichern](backup-azure-vms-first-look-arm.md).

## <a name="before-you-start"></a>Vorbereitung

Azure Backup sichert Azure-VMs durch die Installation einer Erweiterung für den Azure-VM-Agent auf dem Computer.

1. [Überprüfen Sie](backup-architecture.md#architecture-direct-backup-of-azure-vms) die Architektur für die Azure-VM-Sicherung.
[Erfahren Sie mehr](backup-azure-vms-introduction.md) über die Azure-VM-Sicherung und die Sicherungserweiterung.
2. Sehen Sie sich die [Unterstützungsmatrix](backup-support-matrix-iaas.md) für die Sicherung virtueller Azure-Computer an.
3. Vorbereiten von Azure-VMs Installieren Sie den VM-Agent, falls er noch nicht installiert ist, und überprüfen Sie den ausgehenden Zugriff für VMs, die Sie sichern möchten.


## <a name="prepare-azure-vms"></a>Vorbereiten von Azure-VMs

Installieren Sie den VM-Agent (falls nötig) und überprüfen Sie den von VMs ausgehenden Zugriff.

### <a name="install-the-vm-agent"></a>Installieren des VM-Agents 
Installieren Sie den Agent bei Bedarf wie folgt:

**VM** | **Details**
--- | ---
**Virtuelle Windows-Computer** | [Laden Sie die Agent-MSI-Datei herunter, und installieren Sie sie](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Für die Installation benötigen Sie Administratorberechtigungen auf dem Computer.<br/><br/> Unter *C:\WindowsAzure\Packages* können Sie sich vergewissern, dass die Installation erfolgreich abgeschlossen wurde, indem Sie mit der rechten Maustaste auf „WaAppAgent.exe“ klicken und dann auf **Eigenschaften** > Registerkarte **Details** klicken. Für **Produktversion** sollte 2.6.1198.718 oder eine höhere Version angegeben sein.<br/><br/> Wenn Sie den Agent aktualisieren, stellen Sie sicher, dass keine Sicherungsvorgänge ausgeführt werden, und [installieren Sie den Agent neu](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Virtuelle Linux-Computer** | Für die Installation und Upgrades des Azure Linux-Agents sollte nach Möglichkeit ein RPM- oder DEB-Paket aus dem Paketrepository Ihrer Verteilung installiert werden. Das Azure Linux-Agent-Paket wird von allen [unterstützten Distributionsanbietern](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) in ihre jeweiligen Images und Repositorys integriert. Der Agent steht auf [GitHub](https://github.com/Azure/WALinuxAgent) zur Verfügung, jedoch wird die Installation über GitHub nicht empfohlen.<br/><br/> Wenn Sie den Agent aktualisieren, stellen Sie sicher, dass keine Sicherungsvorgänge ausgeführt werden, und aktualisieren Sie die Binärdateien. 


### <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung

Die Sicherungserweiterung, die auf der VM ausgeführt wird, muss über ausgehenden Zugriff auf öffentliche Azure-IP-Adressen verfügen.

- Für die Kommunikation des virtuellen Azure-Computers mit dem Azure Backup-Dienst ist kein expliziter ausgehender Netzwerkzugriff erforderlich.
- Allerdings können beim Herstellen einer Verbindung auf bestimmten älteren virtuellen Computern aufgrund des Fehlers **ExtensionSnapshotFailedNoNetwork** Probleme auftreten. Verwenden Sie in diesem Fall eine der folgenden Optionen, damit die Sicherungserweiterung für den Sicherungsdatenverkehr mit öffentlichen Azure-IP-Adressen kommunizieren kann.

   **Option** | **Aktion** ** | **Vorteile** | **Nachteile**
   --- | --- | --- | ---
   **NSG-Regeln einrichten** | Lassen Sie die [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) zu.<br/><br/>  Sie können eine Regel hinzufügen, die den Zugriff auf den Azure Backup-Dienst mithilfe eines [Diensttags](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure) zulässt, anstatt jeden Adressbereich individuell zuzulassen und zu verwalten. [Erfahren Sie mehr](../virtual-network/security-overview.md#service-tags) über Diensttags. | Keine zusätzlichen Kosten. Einfache Verwaltung mit Diensttags.
   **Proxy bereitstellen** | Stellen Sie einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereit. | Ermöglicht den Zugriff auf Azure insgesamt, nicht nur auf den Speicher. Die Feinsteuerung über die Speicher-URLs ist möglich.<br/><br/> Zentraler Punkt für Internetzugriff auf virtuelle Computer.<br/><br/> Zusätzliche Kosten für den Proxy.<br/><br/> 
   **Azure Firewall einrichten** | Lassen Sie den Datenverkehr mit der VM durch Azure Firewall mithilfe eines FQDN-Tags (vollqualifizierter Domänenname) für den Azure Backup-Dienst zu.|  Einfache Verwendung, wenn Azure Firewall in einem VNet-Subnetz eingerichtet ist. | Es können keine eigenen FQDN-Tags erstellt werden, und die FQDNs in einem Tag können nicht geändert werden.<br/><br/> Wenn Sie verwaltete Azure-Datenträger verwenden, müssen Sie in den Firewalls möglicherweise einen weiteren Port öffnen (Port 8443).

#### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Einrichten einer NSG-Regel (Netzwerksicherheitsgruppe) zum Zulassen des ausgehenden Zugriffs auf Azure

Wenn der Zugriff Ihrer Azure-VM durch eine NSG verwaltet wird, lassen Sie den ausgehenden Zugriff auf die erforderlichen Bereiche und Ports für den Sicherungsspeicher zu.

1. Klicken Sie in der VM unter **Netzwerk** auf **Regel für ausgehenden Port hinzufügen**.
2. Klicken Sie unter **Ausgangssicherheitsregel hinzufügen** auf **Erweitert**.
3. Wählen Sie im Feld **Quelle** den Eintrag **VirtualNetwork** aus.
4. Geben Sie für **Quellportbereiche** ein Sternchen (*) ein, um ausgehenden Zugriff über alle Ports zuzulassen.
5. Wählen Sie **Diensttag** als **Ziel** aus. Wählen Sie **Storage.region** aus der Liste aus. Wählen dabei die Region aus, in der sich der Tresor und die VMs befinden, die gesichert werden sollen.
6. Wählen Sie unter **Zielportbereiche** den entsprechenden Port aus.
    - Für nicht verwaltete VMs mit nicht verschlüsseltem Speicherkonto: 80
    - Für nicht verwaltete VMs mit verschlüsseltem Speicherkonto: 443 (Standardeinstellung)
    - Für verwaltete VMs: 8443
7. Wählen Sie für **Protokoll** die Option **TCP** aus.
8. Geben Sie unter **Priorität** einen Prioritätswert an, der geringer als alle höheren Ablehnungsregeln ist. Wenn Sie über eine Regel verfügen, die den Zugriff verhindert, muss die neue Zulassungsregel höhergestellt werden. Wenn Sie beispielsweise über eine **Deny_All**-Regel mit der Priorität 1.000 verfügen, muss für Ihre neue Regel ein Wert unter 1.000 festgelegt werden.
9. Geben Sie einen Namen und eine Beschreibung für die Regel ein, und klicken Sie dann auf **OK**.

Sie können die NSG-Regel auf mehrere VMs anwenden, um ausgehenden Zugriff zu erlauben.

Dieses Video führt Sie durch diesen Prozess.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


#### <a name="route-backup-traffic-through-a-proxy"></a>Routen des Sicherungsdatenverkehrs über einen Proxy

Sie können den Sicherungsdatenverkehr über ein Proxy routen und dem Proxy dann den Zugriff auf die erforderlichen Azure-Bereiche gewähren.
Sie sollten Ihre Proxy-VM wie folgt konfigurieren:

- Die Azure-VM sollte jeglichen HTTP-Datenverkehr an das öffentliche Internet über den Proxy routen.
- Der Proxy sollte jeglichen eingehenden Datenverkehr von VMs im entsprechenden virtuellen Netzwerk (VNet) zulassen.
- Die NSG **NSF-lockdown** benötigt eine Regel, die ausgehenden Internetdatenverkehr vom virtuellen Proxy-Computer zulässt.

##### <a name="set-up-the-proxy"></a>Einrichten des Proxys
Wenn Sie über keinen Proxy für das Systemkonto verfügen, richten Sie wie folgt einen ein:

1. Laden Sie [PsExec](https://technet.microsoft.com/sysinternals/bb897553) herunter.

2. Führen Sie den Befehl **PsExec.exe -i -s cmd.exe** aus, um die Eingabeaufforderung mit einem Systemkonto auszuführen.
3. Führen Sie den Browser im Systemkontext aus. Beispiel: **%PROGRAMFILES%\Internet Explorer\iexplore.exe** für Internet Explorer.  
4. Definieren Sie die Proxyeinstellungen.
    - Unter Linux:
        - Fügen Sie die folgende Zeile in die Datei **/etc/environment** ein:
            - **http_proxy=http://proxy IP address:proxy port**
        - Fügen Sie die folgenden Zeilen in die Datei **/etc/waagent.conf** ein:
            - **HttpProxy.Host=proxy IP address**
            - **HttpProxy.Port=proxy port**
    - Legen Sie unter Windows in den Browsereinstellungen fest, dass ein Proxy verwendet werden soll. Wenn Sie derzeit einen Proxy für ein Benutzerkonto verwenden, können Sie mithilfe des folgenden Skripts die Einstellung auf Systemkontoebene anwenden.
        ```powershell
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

##### <a name="allow-incoming-connections-on-the-proxy"></a>Zulassen von eingehenden Verbindungen über den Proxy

Lassen Sie eingehende Verbindungen in den Proxyeinstellungen zu.

- Öffnen Sie beispielsweise **Windows Firewall mit erweiterter Sicherheit**.
    - Klicken Sie mit der rechten Maustaste auf **Eingangsregeln** > **Neue Regel**.
    - Klicken Sie unter **Regeltyp** auf **Benutzerdefiniert** > **Weiter**.
    - Klicken Sie unter **Programm** auf **Alle Programme** > **Weiter**.
    - Legen Sie unter **Protokolle und Ports** den Typ **TCP**, für **Lokale Ports** die Option **Bestimmte Ports** und für **Remoteport** die Option **Alle Ports** fest.
    - Beenden Sie den Assistenten, und legen Sie einen Namen für die Regel fest.

##### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Hinzufügen einer Ausnahmeregel zur NSG für den Proxy

Lassen Sie für die NSG **NSF-lockdown** den Datenverkehr über alle Ports unter 10.0.0.5 an alle Internetadressen über Port 80 (HTTP) oder 443 (HTTPS) zu.

- Das folgende PowerShell-Skript enthält ein Beispiel zum Zulassen des Datenverkehrs.
- Anstatt den ausgehenden Datenverkehr an alle öffentlichen Internetadressen zuzulassen, können Sie einen IP-Adressbereich (-DestinationPortRange) festlegen oder den Diensttag „storage.region“ verwenden.   

    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```

### <a name="allow-firewall-access-with-fqdn-tag"></a>Zulassen des Firewallzugriffs mithilfe des FQDN-Tags

Sie können Azure Firewall so konfigurieren, dass der ausgehende Zugriff für Netzwerkdatenverkehr an Azure Backup zugelassen wird.

- [Weitere Informationen](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) zum Bereitstellen von Azure Firewall.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/firewall/fqdn-tags) über FQDN-Tags.

## <a name="create-a-vault"></a>Erstellen eines Tresors

Ein Tresor speichert Sicherungen und Wiederherstellungspunkte, die im Laufe der Zeit erstellt wurden, sowie Sicherungsrichtlinien, die zu gesicherten VMs gehören. Erstellen Sie wie folgt einen Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im Menü **Hub** die Option **Durchsuchen** aus, und geben Sie dann **Recovery Services** ein. Wählen Sie **Recovery Services-Tresore**.

    ![Eingabe im Feld und Auswahl von „Recovery Services-Tresore“ in den Ergebnissen](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

3. Wählen Sie im Menü **Recovery Services-Tresore** die Option **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    ![Bereich „Recovery Services-Tresore“](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Geben Sie unter **Recovery Services-Tresore** >  **Name** einen Anzeigenamen ein, über den der Tresor identifiziert wird.
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


## <a name="configure-a-backup-policy"></a>Konfigurieren einer Sicherungsrichtlinie

Ermitteln Sie die VMs im Abonnement, und konfigurieren Sie die Sicherung.

1. Klicken Sie im Tresor auf **Übersicht**, und klicken Sie dann auf **+ Sicherung**.

   ![Schaltfläche „Sicherung“](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Die Bereiche **Sicherung** und **Sicherungsziel** werden geöffnet.

2. Wählen Sie **Azure** unter **Sicherungsziel**> **Wo wird Ihre Workload ausgeführt?** aus. Klicken Sie unter **Was möchten Sie sichern?** auf **Virtueller Computer** >  **OK**. Dadurch wird die VM-Erweiterung im Tresor registriert.

   ![Bereiche „Sicherung“ und „Sicherungsziel“](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   In diesem Schritt wird die VM-Erweiterung im Tresor registriert. Der Bereich **Sicherungsziel** wird geschlossen, und der Bereich **Sicherungsrichtlinie** wird geöffnet.

3. Wählen Sie unter **Sicherungsrichtlinie** die Richtlinie aus, die dem Tresor zugeordnet werden soll. Klicken Sie dann auf **OK**.
    - Die Details zur Standardrichtlinie werden unter dem Dropdownmenü aufgeführt.
    - Klicken Sie auf **Neu erstellen**, um eine Richtlinie zu erstellen. Erfahren Sie mehr über das [Definieren einer Richtlinie](backup-azure-arm-vms-prepare.md#configure-a-backup-policy).
    

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

- Eine erste Sicherung wird gemäß Ihrem Sicherungszeitplan ausgeführt.
- Die Sicherungserweiterung wird vom Sicherungsdienst unabhängig davon installiert, ob der virtuelle Computer ausgeführt wird oder nicht.
    - Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt.
    -  Der virtuelle Computer wird jedoch auch gesichert, wenn er ausgeschaltet ist und die Erweiterung nicht installiert werden kann. Dies wird als *Offline-VM* bezeichnet. In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent*.
    Beachten Sie, dass Azure Backup für die Sicherungen von Azure-VMs keine automatische Uhrzeitanpassung an die Sommerzeit unterstützt. Sicherungsrichtlinien können bei Bedarf angepasst werden.
  
 ## <a name="run-the-initial-backup"></a>Ausführen der ersten Sicherung

Die erste Sicherung wird gemäß dem Zeitplan ausgeführt, sofern Sie sie nicht sofort manuell ausführen. Gehen Sie zur manuellen Ausführung folgendermaßen vor:

1. Klicken Sie im Tresormenü auf **Sicherungselemente**.
2. Klicken Sie unter **Sicherungselemente** auf **Virtueller Azure-Computer**.
3. Klicken Sie in der Liste **Sicherungselemente** auf das Auslassungszeichen **...**.
4. Klicken Sie auf **Jetzt sichern**.
5. Verwenden Sie unter **Jetzt sichern** den Kalender, um den letzten Tag zur Beibehaltung des Wiederherstellungspunkts auszuwählen. Klicken Sie dann auf **OK**.
6. Überwachen Sie die Portalbenachrichtigungen. Sie können den Auftragsstatus im Dashboard des Tresors unter **Sicherungsaufträge** > **In Bearbeitung** überwachen. Je nach Größe Ihrer VM kann das Erstellen der ersten Sicherung einige Zeit dauern.



## <a name="next-steps"></a>Nächste Schritte

- Beheben von Problemen, die bei [Azure-VM-Agents](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) oder [Azure-VM-Sicherungen](backup-azure-vms-troubleshoot.md) auftreten können.
- [Sichern virtueller Azure-Computer](backup-azure-vms-first-look-arm.md)
