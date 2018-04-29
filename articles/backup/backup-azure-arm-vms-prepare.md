---
title: 'Azure Backup: Vorbereiten der Sicherung virtueller Computer | Microsoft-Dokumentation'
description: Vergewissern Sie sich, dass Ihre Umgebung für die Sicherung virtueller Computer in Azure vorbereitet ist.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: Sicherung; Sichern;
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/1/2018
ms.author: markgal;trinadhk;sogup;
ms.openlocfilehash: caf2c54c986f8c4dd951628fd6908d42e7ddd281
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Vorbereiten der Umgebung für die Sicherung von mit Resource Manager bereitgestellten virtuellen Computern

Dieser Artikel enthält die Schritte zum Vorbereiten Ihrer Umgebung zum Sichern eines mit Azure Resource Manager bereitgestellten virtuellen Computers (VM). In den im Verfahren dargestellten Schritten wird das Azure-Portal genutzt. Wenn Sie einen virtuellen Computer sichern, werden die Sicherungsdaten oder Wiederherstellungspunkte in einem Recovery Services-Tresor gespeichert. Recovery Services-Tresore speichern die Sicherungsdaten für klassische und vom Resource Manager bereitgestellte virtuelle Computer.

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md).

Bevor Sie einen mit dem Resource Manager bereitgestellten virtuellen Computer schützen (oder sichern) können, stellen Sie sicher, dass folgende Voraussetzungen erfüllt sind:

* Erstellen oder identifizieren Sie einen Recovery Services-Tresor *in der Region, in der sich auch Ihr virtueller Computer befindet*.
* Wählen Sie ein Szenario aus, definieren Sie die Sicherungsrichtlinie, und definieren Sie die zu schützenden Elemente.
* Überprüfen Sie, ob ein VM-Agent (Erweiterung) auf dem virtuellen Computer installiert ist.
* Überprüfen Sie die Netzwerkverbindung.
* Wenn Sie für virtuelle Linux-Computer die Sicherungsumgebung für anwendungskonsistente Sicherungen anpassen möchten, führen Sie die [Schritte zum Konfigurieren von Skripts vor und nach der Momentaufnahme](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent) aus.

Wenn diese Bedingungen in Ihrer Umgebung bereits erfüllt sind, fahren Sie mit dem Artikel [Sichern von virtuellen Azure-Computern](backup-azure-arm-vms.md)fort. Wenn Sie mindestens eine dieser Voraussetzungen einrichten oder überprüfen müssen, führt dieser Artikel Sie durch die jeweiligen Schritte.

## <a name="supported-operating-systems-for-backup"></a>Unterstützte Betriebssystems für die Sicherung
 * **Linux**: Azure Backup unterstützt [eine Liste mit von Azure empfohlenen Verteilungen](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mit Ausnahme von CoreOS Linux. 
 
    > [!NOTE] 
    > Andere Bring-Your-Own-Linux-Distributionen sollten funktionieren, sofern der VM-Agent auf dem virtuellen Computer verfügbar ist und Python unterstützt wird. Allerdings werden diese Distributionen nicht unterstützt.
 * **Windows Server**: Versionen, die älter als Windows Server 2008 R2 sind, werden nicht unterstützt.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Einschränkungen beim Sichern und Wiederherstellen eines virtuellen Computers
Machen Sie sich vor der Vorbereitung der Umgebung mit diesen Einschränkungen vertraut:

* Die Sicherung von virtuellen Computern mit mehr als 16 Datenträgern wird nicht unterstützt.
* Die Sicherung von virtuellen Computern mit einer reservierten IP-Adresse und ohne definierten Endpunkt wird nicht unterstützt.
* Das Sichern von virtuellen Linux-Computern, die mit Linux Unified Key Setup (LUKS) verschlüsselt sind, wird nicht unterstützt.
* Das Sichern von virtuellen Computern, die freigegebene Clustervolumes (Cluster Shared Volumes, CSV) oder eine Konfiguration für Dateiserver mit horizontaler Skalierung enthalten, wird nicht empfohlen. Wenn dies der Fall ist, wird ein Fehler bei CSV-Schreibern erwartet. Dafür müssen alle während einer Momentaufnahmenaufgabe in der Clusterkonfiguration enthaltenen virtuellen Computer berücksichtigt werden. Azure Backup unterstützt keine Multi-VM-Konsistenz. 
* Im Netzwerk bereitgestellte und an einen virtuellen Computer angefügte Laufwerke werden nicht in die Sicherungsdaten einbezogen.
* Das Ersetzen eines vorhandenen virtuellen Computers während der Wiederherstellung wird nicht unterstützt. Wenn Sie versuchen, die VM wiederherzustellen, obwohl die VM vorhanden ist, wird die Wiederherstellung nicht ausgeführt.
* Die regionsübergreifende Sicherung und Wiederherstellung wird nicht unterstützt.
* Die Sicherung und Wiederherstellung von virtuellen Computern mit nicht verwalteten Datenträgern in Speicherkonten mit angewendeten Netzwerkregeln wird für Kunden mit dem alten VM-Sicherungsstapel nicht unterstützt. 
* Stellen Sie beim Konfigurieren der Sicherung sicher, dass die Speicherkontoeinstellungen für **Firewalls und virtuelle Netzwerke** Zugriff von allen Netzwerken zulassen.
* Sie können virtuelle Computer in allen öffentlichen Regionen von Azure sichern. (Siehe [Checkliste](https://azure.microsoft.com/regions/#services) der unterstützten Regionen.) Wenn die gewünschte Region derzeit nicht unterstützt wird, wird sie bei der Erstellung des Tresors in der Dropdownliste nicht angezeigt.
* Das Wiederherstellen eines virtuellen Domänencontrollercomputers, der Teil einer Konfiguration mit mehreren Domänencontrollern ist, wird nur über PowerShell unterstützt. Weitere Informationen finden Sie unter [Wiederherstellen von Multi-DC-Domänencontrollern](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Das Wiederherstellen virtueller Computer mit den folgenden besonderen Netzwerkkonfigurationen wird nur über PowerShell unterstützt. Virtuelle Computer, die mit dem Wiederherstellungsworkflow der Benutzeroberfläche erstellt werden, weisen diese Netzwerkkonfigurationen nach dem Abschluss des Wiederherstellungsvorgangs nicht auf. Weitere Informationen finden Sie unter [Wiederherstellen von VMs mit speziellen Netzwerkkonfigurationen](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Virtuelle Computer unter Load Balancer-Konfiguration (intern und extern)
  * Virtuelle Computer mit mehreren reservierten IP-Adressen
  * Virtuelle Computer mit mehreren Netzwerkadaptern

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Erstellen eines Recovery Services-Tresors für einen virtuellen Computer
Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinien, die den geschützten virtuellen Computern zugeordnet sind.

So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im Menü **Hub** die Option **Durchsuchen**, und geben Sie dann **Recovery Services** ein. Sobald Sie mit der Eingabe beginnen, wird die Ressourcenliste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore**.

    ![Eingabe im Feld und Auswahl von „Recovery Services-Tresore“ in den Ergebnissen](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.
3. Wählen Sie im Menü **Recovery Services-Tresore** die Option **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Der Bereich **Recovery Services-Tresore** wird geöffnet. Sie werden aufgefordert, Informationen für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** anzugeben.

    ![Bereich „Recovery Services-Tresore“](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen mit 2 bis 50 Zeichen ein. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
5. Wählen Sie **Abonnement** aus, um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
6. Wählen Sie **Ressourcengruppe** aus, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen. Umfassende Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Wählen Sie **Standort** aus, um die geografische Region für den Tresor auszuwählen. Der Tresor *muss* sich in derselben Region wie die zu schützenden virtuellen Computer befinden.

   > [!IMPORTANT]
   > Wenn Sie sich nicht sicher sind, an welchem Standort sich Ihr virtueller Computer befindet, schließen Sie das Dialogfeld zur Tresorerstellung, und wechseln Sie zur Liste der virtuellen Computer im Portal. Wenn Sie über virtuelle Computer in mehreren Regionen verfügen, müssen Sie in jeder Region einen Recovery Services-Tresor erstellen. Erstellen Sie den Tresor am ersten Standort, bevor Sie mit dem nächsten Standort fortfahren. Das Angeben von Speicherkonten zum Speichern der Sicherungsdaten ist nicht erforderlich. Der Recovery Services-Tresor und der Azure Backup-Dienst nehmen dies automatisch vor.
   >
   >

8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

    ![Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Nachdem Sie Ihren Tresor erstellt haben, widmen wir uns nun dem Festlegen der Speicherreplikation.

## <a name="set-storage-replication"></a>Festlegen der Speicherreplikation
Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Behalten Sie die Option mit dem georedundanten Speicher für Ihre primäre Sicherung bei. Wenn Sie eine günstigere und weniger langfristige Option wünschen, wählen Sie lokal redundanten Speicher.

So bearbeiten Sie die Einstellung für die Speicherreplikation:

1. Wählen Sie im Bereich **Recovery Services-Tresore** Ihren Tresor aus.
    Wenn Sie Ihren Tresor auswählen, werden der Bereich **Einstellungen** (auf dem oben der Name des Tresors angegeben ist) sowie der Bereich mit den Tresordetails geöffnet.

   ![Auswählen des Tresors aus der Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Scrollen Sie im Bereich **Einstellungen** mithilfe des vertikalen Schiebereglers nach unten zum **Verwaltungsabschnitt**, und klicken Sie auf **Sicherungsinfrastruktur**. Wählen Sie im Abschnitt **Allgemein** die Option **Sicherungskonfiguration**. Wählen Sie im Bereich **Speicherkonfiguration** die Speicherreplikationsoption für Ihren Tresor aus. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher.

   ![Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin den georedundanten Speicher. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie „Lokal redundanter Speicher“ aus. Weitere Informationen zu Speicheroptionen finden Sie in der [Übersicht über die Azure Storage-Replikation](../storage/common/storage-redundancy.md).

3. Wenn Sie den Speicherreplikationstyp geändert haben, wählen Sie **Speichern**.
    
Nachdem Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Auswählen eines Sicherungsziels, Festlegen der Richtlinie und Definieren von zu schützenden Elementen
Bevor Sie einen virtuellen Computer mit einem Recovery Services-Tresor registrieren, führen Sie den Ermittlungsprozess aus, um alle neuen dem Abonnement hinzugefügte virtuellen Computer zu identifizieren. Während des Ermittlungsprozesses fragt Azure die Liste der virtuellen Computer im Abonnement ab. Wenn neue virtuelle Computer gefunden werden, werden im Portal der Clouddienstname und die zugeordnete Region angezeigt. Das *Szenario* bezeichnet im Kontext des Azure-Portals das, was Sie im Recovery Services-Tresor eingeben. Die *Richtlinie* gibt die Häufigkeit und den Zeitpunkt für die Erstellung von Wiederherstellungspunkten an. Darüber hinaus enthält die Richtlinie die Beibehaltungsdauer für die Wiederherstellungspunkte.

1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit Schritt 2 fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben, öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie im Menü **Hub** die Option **Weitere Dienste** aus.

   a. Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.

      ![Eingabe im Feld und Auswahl von „Recovery Services-Tresore“ in den Ergebnissen](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wenn in Ihrem Abonnement keine Tresore vorhanden sind, ist diese Liste leer.

      ![Ansicht der Liste mit den Recovery Services-Tresoren](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

      Der Bereich **Einstellungen** und das Tresordashboard für den ausgewählten Tresor werden geöffnet.

      ![Bereich „Einstellungen“ und Tresordashboard](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Wählen Sie im Menü des Tresordashboards die Option **Sicherung**.

   ![Schaltfläche „Sicherung“](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Die Bereiche **Sicherung** und **Sicherungsziel** werden geöffnet.

3. Legen Sie im Bereich **Sicherungsziel** die Option **Wo wird Ihre Workload ausgeführt?** auf **Azure** und die Option **Was möchten Sie sichern?** auf **Virtueller Computer** fest. Wählen Sie dann **OK**aus.

   ![Bereiche „Sicherung“ und „Sicherungsziel“](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   In diesem Schritt wird die VM-Erweiterung im Tresor registriert. Der Bereich **Sicherungsziel** wird geschlossen, und der Bereich **Sicherungsrichtlinie** wird geöffnet.

   ![Bereiche „Sicherung“ und „Sicherungsrichtlinie“](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Wählen Sie im Bereich **Sicherungsrichtlinie** die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten.

   ![Sicherungsrichtlinie auswählen](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   Die Details zur Standardrichtlinie werden unter dem Dropdownmenü aufgeführt. Wählen Sie im Dropdownmenü die Option **Neu erstellen** aus, wenn Sie eine neue Richtlinie erstellen möchten. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Wählen Sie **OK**, um die Sicherungsrichtlinie mit dem Tresor zu verknüpfen.

   Der Bereich **Sicherungsrichtlinie** wird geschlossen, und der Bereich **Virtuelle Computer auswählen** wird geöffnet.
5. Wählen Sie im Bereich **Virtuelle Computer auswählen** die virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und wählen Sie **OK**.

   ![Bereich „Virtuelle Computer auswählen“](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   Der ausgewählte virtuelle Computer wird überprüft. Falls die erwarteten virtuellen Computer nicht angezeigt werden, sollten Sie überprüfen, ob sie sich in der gleichen Azure-Region wie der Recovery Services-Tresor befinden. Wenn die virtuellen Computer dennoch nicht angezeigt werden, stellen Sie sicher, dass sie nicht bereits durch einen anderen Tresor geschützt sind. Die Region des Recovery Services-Tresors wird auf dem Tresordashboard angezeigt.

6. Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, wählen Sie im Bereich **Sicherung** die Option **Sicherung aktivieren**. Mit diesem Schritt wird die Richtlinie im Tresor und auf den virtuellen Computern bereitgestellt. Der erste Wiederherstellungspunkt für den virtuellen Computer wird in diesem Schritt nicht erstellt.

   ![Schaltfläche „Sicherung aktivieren“](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nachdem die Sicherung erfolgreich aktiviert wurde, wird die Sicherungsrichtlinie nach dem Zeitplan ausgeführt. Wenn Sie einen bedarfsgesteuerten Sicherungsauftrag generieren möchten, um die virtuellen Computer jetzt zu sichern, finden Sie weitere Informationen unter [Auslösen des Sicherungsauftrags](./backup-azure-vms-first-look-arm.md#initial-backup).

Wenn Sie Probleme beim Registrieren des virtuellen Computers haben, überprüfen Sie die folgenden Informationen zum Installieren des VM-Agents und zu Netzwerkverbindungen. Die folgenden Informationen sind wahrscheinlich nicht erforderlich, wenn Sie in Azure erstellte virtuelle Computer schützen möchten. Wenn Sie jedoch Ihre virtuellen Computer zu Azure migriert haben, vergewissern Sie sich, dass Sie den VM-Agent ordnungsgemäß installiert haben und dass Ihr virtueller Computer mit dem virtuellen Netzwerk kommunizieren kann.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Installieren des VM-Agents auf dem virtuellen Computer
Damit die Sicherungserweiterung funktioniert, muss der Azure-[VM-Agent](../virtual-machines/windows/agent-user-guide.md) auf dem virtuellen Azure-Computer installiert sein. Wenn Ihr virtueller Computer über Azure Marketplace erstellt wurde, ist der VM-Agent auf dem virtuellen Computer bereits vorhanden. 

Die folgenden Informationen werden für Situationen bereitgestellt, in denen Sie *keinen* in Azure Marketplace erstellten virtuellen Computer verwenden. Angenommen, Sie haben einen virtuellen Computer aus einem lokalen Datencenter migriert. In diesem Fall muss der VM-Agent installiert werden, um den virtuellen Computer zu schützen.

Falls beim Sichern des virtuellen Azure-Computers Probleme auftreten, vergewissern Sie sich mithilfe der folgenden Tabelle, dass der Azure-VM-Agent auf dem virtuellen Computer ordnungsgemäß installiert ist. Die Tabelle enthält weitere Informationen zum VM-Agent für virtuelle Windows- und Linux-Computer.

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agents |Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. |Installieren Sie den neuesten [Linux-Agent](../virtual-machines/linux/agent-user-guide.md). Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. Es wird empfohlen, den Agent aus dem Repository Ihrer Distribution zu installieren. Es wird *nicht empfohlen*, den Linux-VM-Agent direkt von GitHub zu installieren.  |
| Aktualisieren des VM-Agents |Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Es wird empfohlen, den Agent über das Repository Ihrer Distribution zu aktualisieren. Es wird *nicht empfohlen*, den Linux-VM-Agent direkt in GitHub zu aktualisieren.<br><br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |
| Überprüfen der VM-Agent-Installation |1. Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner „C:\WindowsAzure\Packages“. <br><br>2. Suchen Sie die Datei „WaAppAgent.exe“. <br><br>3. Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der **Produktversion** sollte 2.6.1198.718 oder eine höhere Version angegeben sein. |N/V |

### <a name="backup-extension"></a>Backup-Erweiterung
Nachdem der VM-Agent auf dem virtuellen Computer installiert wurde, installiert der Azure Backup-Dienst die Sicherungserweiterung für den VM-Agent. Der Sicherungsdienst installiert nahtlos Upgrades und Patches für die Sicherungserweiterung.

Die Sicherungserweiterung wird vom Sicherungsdienst unabhängig davon installiert, ob der virtuelle Computer ausgeführt wird oder nicht. Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt. Der Sicherungsdienst setzt die Sicherung des virtuellen Computers jedoch auch dann fort, wenn dieser ausgeschaltet wurde und die Erweiterung nicht installiert werden konnte. Dies wird als *Offline-VM* bezeichnet. In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent*.

## <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung
Zum Verwalten der VM-Momentaufnahmen benötigt die Sicherungserweiterung eine Verbindung mit den öffentlichen Azure-IP-Adressen. Ohne die richtige Internetverbindung tritt bei diesen HTTP-Anforderungen des virtuellen Computers ein Timeout auf, und der Sicherungsvorgang funktioniert nicht. Wenn in Ihrer Bereitstellung Zugriffseinschränkungen aktiv sind (z.B. über eine Netzwerksicherheitsgruppe [NSG]), wählen Sie eine der folgenden Optionen für die Bereitstellung eines freien Pfades für den Sicherungsdatenverkehr:

* [Setzen Sie die IP-Bereiche der Azure-Rechenzentren auf die Whitelist](http://www.microsoft.com/en-us/download/details.aspx?id=41653).
* Stellen Sie einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereit.

Bei der Entscheidung, welche Option Sie verwenden, müssen Sie die Kompromisse zwischen Verwaltbarkeit, differenzierter Kontrolle und Kosten abwägen.

| Option | Vorteile | Nachteile |
| --- | --- | --- |
| Whitelist der IP-Bereiche |Keine zusätzlichen Kosten.<br><br>Verwenden Sie zum Öffnen des Zugriffs in einer Netzwerksicherheitsgruppe das **Set-AzureNetworkSecurityRule**-Cmdlet. |Komplexe Verwaltung, da sich die betroffenen IP-Bereiche im Laufe der Zeit ändern.<br><br>Ermöglicht den Zugriff auf Azure insgesamt, nicht nur auf den Speicher. |
| Verwenden eines HTTP-Proxys |Feinsteuerung im Proxy über die Speicher-URLs ist zulässig.<br><br>Zentraler Punkt für Internetzugriff auf virtuelle Computer.<br><br>Unterliegt keinen Azure-IP-Adressänderungen. |Zusätzliche Kosten für die Ausführung eines virtuellen Computers mit Proxysoftware. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Whitelist der IP-Bereiche der Azure-Rechenzentren
Ausführliche Informationen und Anweisungen zur Aufnahme der IP-Bereiche der Azure-Rechenzentren in eine Whitelist finden Sie auf der [Azure-Website](http://www.microsoft.com/en-us/download/details.aspx?id=41653).

Mit [Diensttags](../virtual-network/security-overview.md#service-tags) können Sie Verbindungen zum Speichern bestimmter Regionen zulassen. Stellen Sie sicher, dass die Regel, die Zugriff auf das Speicherkonto zulässt, eine höhere Priorität hat, als die Regel, die den Internetzugriff blockiert. 

![NSG mit Speichertags für eine Region](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Das folgende Video führt Sie durch die Schrittanleitung zum Konfigurieren von Diensttags: 

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!WARNING]
> Speicherdiensttags sind nur in bestimmten Regionen und in der Vorschau verfügbar. Eine Liste der Regionen finden Sie unter [Diensttags](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>Verwenden eines HTTP-Proxys für die Sicherung von virtuellen Computern
Beim Sichern eines virtuellen Computers werden die Befehle für die Momentaufnahmenverwaltung von der Sicherungserweiterung auf der dem virtuellen Computer per HTTPS-API an Azure Storage gesendet. Leiten Sie den Datenverkehr der Sicherungserweiterung über den HTTP-Proxy, da dies die einzige Komponente ist, die für den Zugriff auf das öffentliche Internet konfiguriert ist.

> [!NOTE]
> Wir empfehlen keine bestimmte Proxysoftware zur Verwendung. Stellen Sie sicher, dass Sie einen Proxy wählen, der mit den folgenden Konfigurationsschritten kompatibel ist.
>
>

Die folgende Beispielabbildung zeigt die drei Konfigurationsschritte, die zur Verwendung einen HTTP-Proxys erforderlich sind:

* Der virtuelle App-Computer leitet den gesamten HTTP-Datenverkehr für das öffentliche Internet über den virtuellen Proxy-Computer.
* Der virtuelle Proxy-Computer lässt eingehenden Datenverkehr von virtuellen Computern in das virtuelle Netzwerk zu.
* Die Netzwerksicherheitsgruppe mit dem Namen „NSF-lockdown“ benötigt eine Sicherheitsregel, die ausgehenden Internetdatenverkehr vom virtuellen Proxy-Computer zulässt.

Führen Sie die folgenden Schritte aus, um einen HTTP-Proxy für die Kommunikation mit dem öffentlichen Internet zu verwenden.

> [!NOTE]
> In diesen Schritten werden spezifische Namen und Werte für dieses Beispiel verwendet. Verwenden Sie beim Eingeben bzw. Ausschneiden und Einfügen von Informationen in Ihren Code die Namen und Werte für Ihre Bereitstellung.

#### <a name="step-1-configure-outgoing-network-connections"></a>Schritt 1. Ausgehende Netzwerkverbindungen konfigurieren
###### <a name="for-windows-machines"></a>Für Windows-Computer
Mit diesem Verfahren wird die Proxyserverkonfiguration für das lokale Systemkonto eingerichtet.

1. Laden Sie [PsExec](https://technet.microsoft.com/sysinternals/bb897553) herunter.
2. Öffnen Sie Internet Explorer, indem Sie den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten ausführen:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

3. Rufen Sie in Internet Explorer **Extras** > **Internetoptionen** > **Verbindungen** > **LAN-Einstellungen** auf.
4. Überprüfen Sie die Proxyeinstellungen für das Systemkonto. Legen Sie Proxy-IP und Port fest.
5. Schließen Sie Internet Explorer.

Mit dem folgenden Skript wird eine computerweite Proxykonfiguration eingerichtet und für den gesamten ausgehenden HTTP/HTTPS-Datenverkehr verwendet. Wenn Sie einen Proxyserver auf einem aktuellen Benutzerkonto (keinem lokalen Systemkonto) eingerichtet haben, verwenden Sie dieses Skript, um die Einstellungen dem SYSTEMKONTO zuzuweisen.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Wenn das Proxyserverprotokoll den Eintrag „(407) Proxyauthentifizierung erforderlich“ enthält, überprüfen Sie, ob Ihre Authentifizierung ordnungsgemäß eingerichtet ist.
>
>

###### <a name="for-linux-machines"></a>Für Linux-Computer
Fügen Sie der Datei ```/etc/environment``` die folgende Zeile hinzu:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Fügen Sie der Datei ```/etc/waagent.conf``` die folgenden Zeilen hinzu:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Schritt 2: Zulassen von eingehenden Verbindungen auf dem Proxyserver
1. Öffnen Sie auf dem Proxyserver die Windows-Firewall. Die einfachste Möglichkeit zum Zugriff auf die Firewall besteht darin, die **Windows-Firewall mit erweiterter Sicherheit** zu suchen.
2. Klicken Sie im Dialogfeld **Windows-Firewall mit erweiterter Sicherheit** mit der rechten Maustaste auf **Regeln für eingehenden Datenverkehr**, und wählen Sie **Neue Regel**.
3. Wählen Sie im Assistenten für neue eingehende Regeln auf der Seite **Regeltyp** die Option **Benutzerdefiniert** aus, und wählen Sie **Weiter**.
4. Wählen Sie die auf der Seite **Programm** die Option **Alle Programme** aus, und wählen Sie **Weiter**.
5. Geben Sie auf der Seite **Protokoll und Ports** die folgenden Informationen ein, und wählen Sie **Weiter**:
   * Wählen Sie **TCP** als **Protokolltyp** aus.
   * Wählen Sie für **Lokaler Port** die Option **Bestimmte Ports** aus. Geben Sie im folgenden Feld die Anzahl der konfigurierten Proxyports an.
   * Wählen Sie bei **Remoteport** die Option **Alle Ports** aus.

Übernehmen Sie im weiteren Verlauf des Assistenten bis zum Ende die Standardeinstellungen. Geben Sie dann dieser Regel einen Namen. 

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Schritt 3: Hinzufügen einer Ausnahmeregel zur NSG
Der folgende Befehl fügt der NSG eine Ausnahme hinzu. Diese Ausnahme lässt TCP-Datenverkehr von allen Ports unter 10.0.0.5 an alle Internetadressen über Port 80 (HTTP) oder 443 (HTTPS) zu. Falls Sie einen bestimmten Port im öffentlichen Internet benötigen, müssen Sie ```-DestinationPortRange``` diesen Port hinzufügen.

Geben Sie an einer Azure PowerShell-Eingabeaufforderung den folgenden Befehl ein:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
Ihre Umgebung ist jetzt für das Sichern Ihres virtuellen Computers vorbereitet. Der nächste logische Schritt besteht darin, eine Sicherung zu erstellen. Der Planungsartikel enthält ausführlichere Informationen zum Sichern virtueller Computer.

* [Sichern virtueller Computer](backup-azure-arm-vms.md)
* [Planen der Sicherungsinfrastruktur für virtuelle Computer](backup-azure-vms-introduction.md)
* [Verwalten der Sicherung virtueller Computer](backup-azure-manage-vms.md)
