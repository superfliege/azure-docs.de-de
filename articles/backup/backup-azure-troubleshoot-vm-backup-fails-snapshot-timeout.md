---
title: 'Problembehandlung bei Azure Backup-Fehlern: Status des Gast-Agents ist unbekannt | Microsoft-Dokumentation'
description: Erfahren Sie mehr über die Symptome, Ursachen und Lösungen von Azure Backup-Fehlern in Verbindung mit dem Agent, der Erweiterung und Datenträgern.
services: backup
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
keywords: Azure Backup; VM-Agent; Netzwerkkonnektivität;
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: 81678f6a8659ffb763ebfe418098e510c73f6ae0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Beheben von Azure Backup-Fehlern: Probleme mit dem Agent oder der Erweiterung

Dieser Artikel enthält Schritte für die Problembehandlung, mit denen Sie Azure Backup-Fehler bei der Kommunikation zwischen dem VM-Agent und der Erweiterung beheben können.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM-Agent kann nicht mit Azure Backup kommunizieren

Fehlermeldung: „VM Agent unable to communicate with Azure Backup“ (VM-Agent kann nicht mit Azure Backup kommunizieren)

Nachdem Sie einen virtuellen Computer für den Backup-Dienst registriert und geplant haben, initiiert Backup den Auftrag, indem der Dienst mit dem VM-Agent kommuniziert, um eine Zeitpunkt-Momentaufnahme zu erstellen. Jede der folgenden Bedingungen kann verhindern, dass die Momentaufnahme ausgelöst wird. Wenn eine Momentaufnahme nicht ausgelöst wird, kann dies zu einem Fehler bei der Sicherung führen. Führen Sie die folgenden Problembehandlungsschritte in der angegebenen Reihenfolge aus, und versuchen Sie dann erneut, den Vorgang auszuführen:

**Ursache 1: [Der virtuelle Computer verfügt nicht über Internetzugriff](#the-vm-has-no-internet-access)**  
**Ursache 2: [Der Agent ist auf dem virtuellen Computer installiert, reagiert aber nicht (bei virtuellen Windows-Computern)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Ursache 3: [Der auf dem virtuellen Computer installierte Agent ist veraltet (bei virtuellen Linux-Computern)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Ursache 4: [Der Momentaufnahmestatus kann nicht abgerufen werden, oder es kann keine Momentaufnahme erstellt werden](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**Ursache 5: [Die Sicherungserweiterung kann nicht aktualisiert oder geladen werden](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Der Momentaufnahmevorgang kann nicht ausgeführt werden, weil der virtuelle Computer nicht mit dem Netzwerk verbunden ist

Fehlermeldung: „Snapshot operation failed due to no network connectivity on the virtual machine“ (Fehler beim Momentaufnahmevorgang aufgrund fehlender Netzwerkkonnektivität auf dem virtuellen Computer)

Nachdem Sie eine VM für den Azure Backup-Dienst registriert und geplant haben, wird der Auftrag von Backup initiiert, indem die Kommunikation mit der VM-Sicherungserweiterung durchgeführt wird, um eine Zeitpunkt-Momentaufnahme zu erstellen. Jede der folgenden Bedingungen kann verhindern, dass die Momentaufnahme ausgelöst wird. Wenn die Momentaufnahme nicht ausgelöst wird, kann bei der Sicherung ein Fehler auftreten. Führen Sie die folgenden Problembehandlungsschritte in der angegebenen Reihenfolge aus, und versuchen Sie dann erneut, den Vorgang auszuführen:    
**Ursache 1: [Der virtuelle Computer verfügt nicht über Internetzugriff](#the-vm-has-no-internet-access)**  
**Ursache 2: [Der Momentaufnahmestatus kann nicht abgerufen werden, oder es kann keine Momentaufnahme erstellt werden](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Ursache 3: [Die Sicherungserweiterung kann nicht aktualisiert oder geladen werden](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>Fehler beim Vorgang der VMSnapshot-Erweiterung

Fehlermeldung: „VMSnapshot extension operation failed“ (Fehler beim Vorgang der VMSnapshot-Erweiterung)

Nachdem Sie eine VM für den Azure Backup-Dienst registriert und geplant haben, wird der Auftrag von Backup initiiert, indem die Kommunikation mit der VM-Sicherungserweiterung durchgeführt wird, um eine Zeitpunkt-Momentaufnahme zu erstellen. Jede der folgenden Bedingungen kann verhindern, dass die Momentaufnahme ausgelöst wird. Wenn die Momentaufnahme nicht ausgelöst wird, kann bei der Sicherung ein Fehler auftreten. Führen Sie die folgenden Problembehandlungsschritte in der angegebenen Reihenfolge aus, und versuchen Sie dann erneut, den Vorgang auszuführen:  
**Ursache 1: [Der Momentaufnahmestatus kann nicht abgerufen werden, oder es kann keine Momentaufnahme erstellt werden](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Ursache 2: [Die Sicherungserweiterung kann nicht aktualisiert oder geladen werden](#the-backup-extension-fails-to-update-or-load)**  
**Ursache 3: [Der Agent ist auf dem virtuellen Computer installiert, reagiert aber nicht (bei virtuellen Windows-Computern)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Ursache 4: [Der auf dem virtuellen Computer installierte Agent ist veraltet (bei virtuellen Linux-Computern)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Fehler bei der Sicherung, weil der VM-Agent nicht reagiert

Fehlermeldung: „Unable to perform the operation as the VM Agent is not responsive“ (Der Vorgang kann nicht ausgeführt werden, da der VM-Agent nicht reagiert)

Nachdem Sie eine VM für den Azure Backup-Dienst registriert und geplant haben, wird der Auftrag von Backup initiiert, indem die Kommunikation mit der VM-Sicherungserweiterung durchgeführt wird, um eine Zeitpunkt-Momentaufnahme zu erstellen. Jede der folgenden Bedingungen kann verhindern, dass die Momentaufnahme ausgelöst wird. Wenn die Momentaufnahme nicht ausgelöst wird, kann bei der Sicherung ein Fehler auftreten. Führen Sie die folgenden Problembehandlungsschritte in der angegebenen Reihenfolge aus, und versuchen Sie dann erneut, den Vorgang auszuführen:  
**Ursache 1: [Der Agent ist auf dem virtuellen Computer installiert, reagiert aber nicht (bei virtuellen Windows-Computern)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Ursache 2: [Der auf dem virtuellen Computer installierte Agent ist veraltet (bei virtuellen Linux-Computern)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Ursache 3: [Der virtuelle Computer verfügt nicht über Internetzugriff](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Interner Fehler bei der Sicherung

Fehlermeldung: „Backup failed with an internal error – Please retry the operation in a few minutes“ (Interner Fehler bei der Sicherung – versuchen Sie, den Vorgang in einigen Minuten zu wiederholen)

Nachdem Sie eine VM für den Azure Backup-Dienst registriert und geplant haben, wird der Auftrag von Backup initiiert, indem die Kommunikation mit der VM-Sicherungserweiterung durchgeführt wird, um eine Zeitpunkt-Momentaufnahme zu erstellen. Jede der folgenden Bedingungen kann verhindern, dass die Momentaufnahme ausgelöst wird. Wenn die Momentaufnahme nicht ausgelöst wird, kann bei der Sicherung ein Fehler auftreten. Führen Sie die folgenden Problembehandlungsschritte in der angegebenen Reihenfolge aus, und versuchen Sie dann erneut, den Vorgang auszuführen:  
**Ursache 1: [Der virtuelle Computer verfügt nicht über Internetzugriff](#the-vm-has-no-internet-access)**  
**Ursache 2: [Der Agent ist auf dem virtuellen Computer installiert, reagiert aber nicht (bei virtuellen Windows-Computern)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Ursache 3: [Der auf dem virtuellen Computer installierte Agent ist veraltet (bei virtuellen Linux-Computern)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Ursache 4: [Der Momentaufnahmestatus kann nicht abgerufen werden, oder es kann keine Momentaufnahme erstellt werden](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Ursache 5: [Die Sicherungserweiterung kann nicht aktualisiert oder geladen werden](#the-backup-extension-fails-to-update-or-load)**  
**Ursache 6: [Der Sicherungsdienst ist aufgrund einer Ressourcengruppensperre nicht berechtigt, die alten Wiederherstellungspunkte zu löschen](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="causes-and-solutions"></a>Ursachen und Lösungen

### <a name="the-vm-has-no-internet-access"></a>Der virtuelle Computer verfügt nicht über Internetzugriff
Gemäß Bereitstellungsanforderung verfügt der virtuelle Computer nicht über Internetzugriff. Möglicherweise gelten auch Einschränkungen für den Computer, die einen Zugriff auf die Azure-Infrastruktur verhindern.

Die Sicherungserweiterung muss eine Verbindung mit öffentlichen Azure-IP-Adressen herstellen können, damit sie richtig funktioniert. Die Erweiterung sendet Befehle an einen Azure Storage-Endpunkt (HTTP-URL), um die Momentaufnahmen des virtuellen Computers zu verwalten. Wenn die Erweiterung keinen Zugriff auf das öffentliche Internet hat, tritt bei der Sicherung letztendlich ein Fehler auf.

####  <a name="solution"></a>Lösung
Um dieses Problem zu lösen, probieren Sie eine der folgenden Methoden aus:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Zulassen des Zugriffs auf den der Region entsprechenden Azure-Speicher

Sie können [Diensttags](../virtual-network/security-overview.md#service-tags) verwenden, um Verbindungen zum Speicher in einer bestimmten Region zuzulassen. Stellen Sie sicher, dass die Regel, die Zugriff auf das Speicherkonto gewährt, eine höhere Priorität hat als die Regel, die den Internetzugriff blockiert. 

![Netzwerksicherheitsgruppe mit Speichertags für eine Region](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Diensttags für Speicher befinden sich in der Vorschau. Sie sind nur in bestimmten Regionen verfügbar. Eine Liste der Regionen finden Sie unter [Diensttags](../virtual-network/security-overview.md#service-tags).

##### <a name="create-a-path-for-http-traffic"></a>Erstellen eines Pfads für HTTP-Datenverkehr

1. Wenn Netzwerkeinschränkungen bestehen (beispielsweise in Form einer Netzwerksicherheitsgruppe), ist es ratsam, einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereitzustellen.
2. Um den Zugriff auf das Internet über den HTTP-Proxyserver zuzulassen, fügen Sie der Netzwerksicherheitsgruppe, falls vorhanden, Regeln hinzu.

Informationen dazu, wie Sie einen HTTP-Proxy für VM-Sicherungen einrichten, finden Sie unter [Vorbereiten der Umgebung für die Sicherung virtueller Azure-Computer](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Wenn Sie verwaltete Azure-Datenträger verwenden, müssen Sie in den Firewalls möglicherweise einen weiteren Port öffnen (Port 8443).

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Der Agent ist auf dem virtuellen Computer installiert, reagiert aber nicht (bei virtuellen Windows-Computern)

#### <a name="solution"></a>Lösung
Der VM-Agent wurde möglicherweise beschädigt, oder der Dienst wurde angehalten. Durch Neuinstallation des VM-Agents erhalten Sie die neueste Version. Dadurch wird auch die Kommunikation mit dem Dienst neu gestartet.

1. Ermitteln Sie, ob der Windows-Gast-Agent-Dienst in den VM-Diensten ausgeführt wird (services.msc). Starten Sie den Windows-Gast-Agent-Dienst neu, und initiieren Sie die Sicherung.    
2. Wenn der Windows-Gast-Agent-Dienst in den Diensten nicht angezeigt wird, wechseln Sie in der Systemsteuerung zu **Programme und Funktionen**, um zu ermitteln, ob der Windows-Gast-Agent-Dienst installiert ist.
4. Wenn der Windows-Gast-Agent unter **Programm und Funktionen** angezeigt wird, deinstallieren Sie ihn.
5. Laden Sie die [aktuelle Version der Agent-MSI-Datei](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) herunter, und installieren Sie sie. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen.
6. Überprüfen Sie, ob der Windows-Gast-Agent-Dienst in den Diensten angezeigt wird.
7. Führen Sie eine bedarfsgesteuerten Sicherung aus: 
    * Wählen Sie im Portal die Option **Jetzt sichern** aus.

Überprüfen Sie auch, ob [Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) auf dem virtuellen Computer installiert ist. .NET 4.5 ist für die Kommunikation des VM-Agents mit dem Dienst erforderlich.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Der auf dem virtuellen Computer installierte Agent ist veraltet (bei virtuellen Linux-Computern).

#### <a name="solution"></a>Lösung
Die meisten Fehler im Zusammenhang mit Agents oder Erweiterungen bei virtuellen Linux-Computern werden durch Probleme verursacht, die einen veralteten VM-Agent betreffen. Befolgen Sie diese allgemeinen Richtlinien, um dieses Problem zu beheben:

1. Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents ](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Wir *empfehlen dringend*, den Agent ausschließlich über ein Verteilungsrepository zu aktualisieren. Wir raten davon ab, den Agent-Code direkt von GitHub herunterzuladen und die Aktualisierung durchzuführen. Falls der aktuelle Agent für Ihre Distribution nicht verfügbar ist, können Sie sich an den zuständigen Support wenden, um Informationen zur Installation zu erhalten. Eine Prüfung auf den aktuellen Agent können Sie auf der Seite für den [Windows Azure-Linux-Agent](https://github.com/Azure/WALinuxAgent/releases) im GitHub-Repository durchführen.

2. Stellen Sie mit dem folgenden Befehl sicher, dass der Azure-Agent auf dem virtuellen Computer ausgeführt wird: `ps -e`

 Wenn der Prozess nicht ausgeführt wird, starten Sie ihn mit den folgenden Befehlen neu:

 * Für Ubuntu: `service walinuxagent start`
 * Für andere Distributionen: `service waagent start`

3. [Konfigurieren Sie den Agent für den automatischen Neustart](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Führen Sie eine neue Testsicherung aus. Beziehen Sie die folgenden Protokolle vom virtuellen Computer, falls der Fehler weiterhin auftritt:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Führen Sie die folgenden Schritte aus, falls die ausführliche Protokollierung für waagent erforderlich ist:

1. Suchen Sie in der Datei „/etc/waagent.conf“ nach der folgenden Zeile: **Enable verbose logging (y|n)**
2. Ändern Sie den Wert für **Logs.Verbose** von *n* in *y*.
3. Speichern Sie die Änderung, und starten Sie waagent neu, indem Sie die weiter oben in diesem Abschnitt beschriebenen Schritte ausführen.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Der Momentaufnahmestatus kann nicht abgerufen werden, oder es kann keine Momentaufnahme erstellt werden
Die VM-Sicherung basiert auf dem Ausführen eines Momentaufnahmenbefehls für das zugrunde liegende Speicherkonto. Bei der Sicherung können Fehler auftreten, weil kein Zugriff auf das Speicherkonto besteht oder weil sich der Momentaufnahmetask verzögert.

#### <a name="solution"></a>Lösung
Die folgenden Umstände können zu Fehlern bei Momentaufnahmetasks führen:

| Ursache | Lösung |
| --- | --- |
| Der VM-Status wird falsch gemeldet, weil der virtuelle Computer im Remotedesktopprotokoll (RDP) heruntergefahren ist. | Wenn Sie den virtuellen Computer im Remotedesktopprotokoll herunterfahren, überprüfen Sie im Portal, ob der VM-Status richtig angezeigt wird. Falls nicht, fahren Sie den virtuellen Computer im Portal mithilfe der Option **Herunterfahren** auf dem VM-Dashboard herunter. |
| Der virtuelle Computer kann die Host- oder Fabric-Adresse nicht aus DHCP abrufen. | Für die VM-Sicherung mithilfe von IaaS muss im Gastbetriebssystem die DHCP-Option aktiviert sein. Wenn der virtuelle Computer die Host- oder Fabric-Adresse nicht aus DHCP-Antwort 245 abrufen kann, können keine Erweiterungen heruntergeladen oder ausgeführt werden. Wenn Sie eine statische private IP-Adresse benötigen, konfigurieren Sie diese über die Plattform. Die DHCP-Option innerhalb des virtuellen Computers sollte aktiviert bleiben. Weitere Informationen finden Sie unter [Festlegen einer statischen internen privaten IP-Adresse](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Die Sicherungserweiterung wird nicht aktualisiert oder geladen.
Wenn Erweiterungen nicht geladen werden können, tritt bei der Sicherung ein Fehler auf, weil keine Momentaufnahme erstellt werden kann.

#### <a name="solution"></a>Lösung

Deinstallieren Sie die Erweiterung, um das erneute Laden der VMSnapshot-Erweiterung zu erzwingen. Beim nächsten Sicherungsversuch wird die Erweiterung neu geladen.

So deinstallieren Sie die Erweiterung:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/) zu dem virtuellen Computer, auf dem der Sicherungsfehler auftritt.
2. Wählen Sie **Settings**aus.
3. Wählen Sie **Erweiterungen**.
4. Wählen Sie **VMSnapshot-Erweiterung** aus.
5. Wählen Sie **Deinstallieren**.

Diese Schritte bewirken, dass die Erweiterung während der nächsten Sicherung neu installiert wird.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Der Sicherungsdienst ist aufgrund einer Ressourcengruppensperre nicht berechtigt, die alten Wiederherstellungspunkte zu löschen
Dieses Problem tritt nur bei verwalteten virtuellen Computern auf, auf denen Benutzer die Ressourcengruppe sperren können. In diesem Fall kann der Sicherungsdienst ältere Wiederherstellungspunkte nicht löschen. Da maximal 18 Wiederherstellungspunkte vorhanden sein können, treten bei neueren Sicherungen Fehler auf.

#### <a name="solution"></a>Lösung

Zum Beheben des Problems führen Sie die folgenden Schritte aus, um die Wiederherstellungspunktsammlung zu entfernen: <br>
 
1. Entfernen Sie die Sperre der Ressourcengruppe, in der sich der virtuelle Computer befindet. 
2. Installieren Sie ARMClient mithilfe von Chocolatey: <br>
   https://github.com/projectkudu/ARMClient
3. Melden Sie sich bei ARMClient an: <br>
    `.\armclient.exe login`
4. Rufen Sie die zum virtuellen Computer gehörige Wiederherstellungspunktsammlung ab: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Beispiel: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Löschen Sie die Wiederherstellungspunktsammlung: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. Die nächste geplante Sicherung generiert automatisch eine Wiederherstellungspunktsammlung und neue Wiederherstellungspunkte.

 
Das Problem wird erneut auftreten, wenn Sie die Ressourcengruppe wieder sperren. 

