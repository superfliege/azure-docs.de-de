---
title: "Problembehandlung für Azure backup-Fehler im klassischen Portal | Microsoft Docs"
description: Problembehandlung bei Azure-Sicherung und Wiederherstellung der virtuellen Azure-Computern im klassischen Portal.
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: 284a1b64fbb15d0aa800182c6671d447e191b76a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problembehandlung bei Azure Virtual machines backup
> [!div class="op_single_selector"]
> * [Recovery Services-Tresor](backup-azure-vms-troubleshoot.md)
> * [Backup-Tresor](backup-azure-vms-troubleshoot-classic.md)
>
>

Sie können Fehler beim Verwenden von Azure Backup mit Informationen in der folgenden Tabelle aufgeführten Probleme beheben.

## <a name="discovery"></a>-Ermittlung
| Sicherungsvorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| -Ermittlung |Fehler beim Ermitteln neuer Elemente – Microsoft Azure Backup gefunden "und" Interner Fehler. Warten Sie einige Minuten, und wiederholen Sie dann den Vorgang. |Wiederholen Sie den Ermittlungsprozess nach 15 Minuten. |
| -Ermittlung |Fehler beim Ermitteln neuer Elemente – wird durch eine erneute Ermittlung Vorgang bereits ausgeführt. Warten Sie, bis der aktuelle Ermittlungsvorgang abgeschlossen wurde. |Keine |

## <a name="register"></a>Registrieren
| Sicherungsvorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Registrieren |Anzahl der an den virtuellen Computer angefügten Datenträger überstieg die unterstützte Begrenzung – bitte einige Datenträger auf diesem virtuellen Computer trennen, und wiederholen Sie den Vorgang. Azure Backup unterstützt bis zu 16 Datenträger zugeordnet, die auf virtuellen Azure-Computer für die Sicherung |Keine |
| Registrieren |Microsoft Azure Backup ist einen interner Fehler - warten Sie einige Minuten, und wiederholen Sie dann den Vorgang aufgetreten. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. |Sie können diesen Fehler aufgrund einer der folgenden nicht unterstützten Konfiguration des virtuellen Computers auf Premium-LRS abrufen. <br> Storage Premium VMs kann mithilfe der Recovery Services-Tresor gesichert werden. [Weitere Informationen](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Registrieren |Fehler bei der Registrierung mit vorgangstimeout Agent installieren |Überprüfen Sie, ob die Version des Betriebssystems des virtuellen Computers unterstützt wird. |
| Registrieren |Befehl fehlgeschlagen - Ausführung wird ein anderer Vorgang ausgeführt für dieses Element. Warten Sie, bis der vorherige Vorgang abgeschlossen ist |Keine |
| Registrieren |Virtuelle Computer mit virtuellen Festplatten auf Premium-Speicher gespeichert werden für die Sicherung nicht unterstützt. |Keine |
| Registrieren |VM-Agent ist nicht vorhanden, auf dem virtuellen Computer – installieren Sie die erforderliche Voraussetzung, VM-Agent, und wiederholen Sie den Vorgang. |[Erfahren Sie mehr](#vm-agent) zu VM-Agent-Installation, und überprüfen Sie die VM-Agent-Installation. |

## <a name="backup"></a>Sicherung
| Sicherungsvorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Sicherung |Konnte nicht mit der VM-Agent für den momentaufnahmestatus des Anwendungsbereichs kommunizieren. Timeout bei der Momentaufnahme-Teilvorgang für VM. -Finden Sie im Handbuch zum Beheben dieses Problems. |Dieser Fehler wird ausgelöst, wenn ein mit der VM-Agent Problem oder des Netzwerkzugriffs auf Azure-Infrastruktur in irgendeiner Form blockiert wird. Erfahren Sie mehr über [Debuggen von VM-snapshot-Probleme](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Wenn der VM-Agent keine Probleme verursacht werden, klicken Sie dann den virtuellen Computer neu. Manchmal ein falscher Status der virtuellen Maschine kann verursachen und Neustart der VM setzt diese "ungültigen Zustand" |
| Sicherung |Interner Fehler bei der Sicherung – Bitte wiederholen Sie den Vorgang in einigen Minuten erneut. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support |Überprüfen Sie, ob es ein vorübergehendes Problem beim Zugriff auf die VM-Speicher ist. Überprüfen Sie [Azure Status](https://azure.microsoft.com/en-us/status/) so festzustellen, ob laufende Probleme im Zusammenhang mit Compute / / Speichernetzwerk in der Region. Bitte wiederholen Sie die backup-Post-Problem verringert wird. |
| Sicherung |Der Vorgang konnte nicht ausgeführt werden, wie die virtuellen Computer nicht mehr vorhanden ist. |Sicherung kann nicht ausgeführt werden, da der virtuelle Computer für die Sicherung konfigurierte gelöscht wurde. Beenden Sie weitere Sicherungen, indem Sie in der Ansicht für geschützte Elemente zu wechseln, wählen Sie geschütztes Element, und klicken Sie auf Schutz beenden. Sie können Daten beibehalten, durch Auswahl der Option "Daten" Sicherung beibehalten. Sie können später den Schutz fortsetzen für diese virtuelle Maschine, indem Sie auf Konfigurieren auf Schutz gegen registrierte Elemente anzeigen |
| Sicherung |Fehler beim Installieren des Azure Recovery Services-Erweiterung für das ausgewählte Element - VM-Agent ist eine Voraussetzung für Azure Recovery Services-Erweiterung. Installieren Sie den Azure-VM-Agent, und wiederholen Sie den Registrierungsvorgang |<ol> <li>Überprüfen Sie, ob der VM-Agent ordnungsgemäß installiert wurde. <li>Stellen Sie sicher, dass das Flag für die VM-Konfiguration ordnungsgemäß festgelegt ist.</ol> [Erfahren Sie mehr](#validating-vm-agent-installation) zu VM-Agent-Installation, und überprüfen Sie die VM-Agent-Installation. |
| Sicherung |Befehl fehlgeschlagen - ist ein anderer Vorgang derzeit für dieses Element ausgeführt. Warten Sie, bis der vorherige Vorgang abgeschlossen ist, und wiederholen Sie dann |Eine vorhandene sicherungs- oder Wiederherstellungsauftrag für den virtuellen Computer ausgeführt wird, und ein neuer Auftrag kann nicht gestartet werden, während der Auftrag ausgeführt wird. |
| Sicherung |Fehler bei der Erweiterungsinstallation mit dem Fehler "COM+ konnte keine Kommunikation mit dem Microsoft Distributed Transaction Coordinator |Dies bedeutet normalerweise, dass der COM+-Dienst nicht ausgeführt wird. Wenden Sie sich an Microsoft Support, um Hilfe zum Beheben dieses Problems. |
| Sicherung |Momentaufnahme-Vorgang konnte nicht mit der VSS-Vorgangsfehler "dieses Laufwerk durch BitLocker-Laufwerkverschlüsselung gesperrt ist. Sie müssen das Laufwerk in der Systemsteuerung entsperren. |Deaktivieren von BitLocker für alle Laufwerke auf dem virtuellen Computer, und beobachten, ob die VSS-Problem gelöst werden kann |
| Sicherung |Virtuelle Computer mit virtuellen Festplatten auf Premium-Speicher gespeichert werden für die Sicherung nicht unterstützt. |Keine |
| Sicherung |Virtuellen Azure-Computer nicht gefunden. |Dies geschieht, wenn der primäre virtuelle Computer gelöscht wurden, jedoch weiterhin die Sicherungsrichtlinie für einen virtuellen Computer zum Ausführen der Sicherung zu suchen. Um diesen Fehler zu beheben: <ol><li>Erstellen Sie die virtuelle Maschine mit dem gleichen Namen und die gleichen Ressourcengruppennamen [Cloud-Service-Name], <br>(OR) <li> Deaktivieren Sie den Schutz für diesen virtuellen Computer, damit die nachfolgende Sicherungen werden nicht ausgelöst. </ol> |
| Sicherung |VM-Agent ist nicht vorhanden, auf dem virtuellen Computer – installieren Sie die erforderliche Voraussetzung, VM-Agent, und wiederholen Sie den Vorgang. |[Erfahren Sie mehr](#vm-agent) zu VM-Agent-Installation, und überprüfen Sie die VM-Agent-Installation. |

## <a name="jobs"></a>Jobs
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Auftrag abbrechen |Abbruch wird für diesen Auftragstyp nicht unterstützt: Warten Sie, bis der Auftrag abgeschlossen wurde. |Keine |
| Auftrag abbrechen |Der Auftrag befindet sich nicht in einer abbrechbaren Status - Bitte warten Sie, bis der Auftrag abgeschlossen wurde. <br>ODER<br> Der ausgewählte Auftrag ist nicht in einer abbrechbaren Zustand: Warten Sie, bis des Auftrags abgeschlossen. |Der Auftrag ist aller Wahrscheinlichkeit fast abgeschlossen; Warten Sie, bis der Auftrag abgeschlossen ist. |
| Auftrag abbrechen |Der Auftrag kann nicht abgebrochen werden, da er nicht ausgeführt wird – Abbruch wird nur unterstützt, für Aufträge, die ausgeführt werden. Bitte Versuch Abbrechen in Bearbeitung befindlichen Auftrag. |Dies geschieht aufgrund einer vorübergehenden Zustand. Warten Sie eine Minute, und wiederholen Sie den Vorgang "Abbrechen" |
| Auftrag abbrechen |Fehler beim Abbrechen des Auftrags - Bitte warten Sie, bis der Auftrag abgeschlossen ist. |Keine |

## <a name="restore"></a>Wiederherstellung
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Wiederherstellung |Fehler bei der Wiederherstellung mit Cloud interner Fehler |<ol><li>Cloud-Dienst auf die Sie wiederherstellen möchten, ist mit DNS-Einstellungen konfiguriert. Sie können überprüfen <br>$deployment = Get-AzureDeployment - ServiceName "ServiceName"-Slot "Produktion" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Wenn Adresse konfiguriert, bedeutet dies, dass DNS-Einstellungen konfiguriert sind.<br> <li>Cloud-Dienst an, in die Sie wiederherstellen möchten mit ReservedIP konfiguriert ist, und vorhandene virtuelle Computer im Cloud-Dienst im Status "beendet" sind.<br>Sehen Sie sich, dass für ein Cloud-Dienst IP-Adresse reserviert wurde, indem Sie mithilfe des folgenden Powershell-Cmdlets:<br>$deployment = Get-AzureDeployment - ServiceName "Servicename"-Slot "Produktion" $DEP ReservedIPName <br><li>Sie möchten eine virtuelle Maschine mit der folgenden speziellen Netzwerkkonfigurationen in demselben Cloud-Dienst wiederherstellen. <br>– Virtuelle Computer unter Lastenausgleichsmodul-Konfiguration (intern und extern)<br>– Virtuelle Computer mit mehreren reservierte IP-Adressen<br>– Virtuelle Computer mit mehreren NICs<br>Wählen Sie einen neuen Clouddienst in der Benutzeroberfläche oder finden Sie unter [wiederherstellungsaspekte](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) für virtuelle Computer mit speziellen Netzwerkkonfigurationen</ol> |
| Wiederherstellung |Der ausgewählte DNS-Name ist bereits vergeben. - Geben Sie einen anderen DNS-Namen, und versuchen Sie es erneut. |Der DNS-Namen hier bezieht sich auf den Namen des Cloud-Dienst (in der Regel Endung. cloudapp.net). Dies muss eindeutig sein. Wenn dieser Fehler auftritt, müssen Sie einen anderen Namen für den virtuellen Computer während der Wiederherstellung auswählen. <br><br> Dieser Fehler wird nur für Benutzer von Azure-Portal angezeigt. Der Restore-Vorgang mithilfe von PowerShell ist erfolgreich, da es nur die Datenträger wiederhergestellt, und nicht den virtuellen Computer erstellen. Der Fehler wird Datenwachstums werden, wenn der virtuelle Computer von Ihnen explizit erstellt wird, nach dem Wiederherstellungsvorgang für des Datenträgers. |
| Wiederherstellung |Der angegebene virtuelle Netzwerk ist nicht richtig konfiguriert – Geben Sie eine andere virtuelle Netzwerkkonfiguration, und versuchen Sie es erneut. |Keine |
| Wiederherstellung |Der angegebenen Cloud-Dienst verwendet eine reservierte IP-Adresse, die nicht übereinstimmen, mit der Konfiguration des virtuellen Computers an, die wiederhergestellt wird – Geben Sie einen anderen Clouddienst, der keine reservierte IP-Adresse verwendet werden, oder wählen Sie einen anderen Wiederherstellungspunkt aus wiederherstellen. |Keine |
| Wiederherstellung |Cloud-Dienst hat die maximale Anzahl von eingabeendpunkte erreicht: Wiederholen Sie den Vorgang, durch einen anderen Clouddienst angeben oder einen vorhandenen Endpunkt verwenden. |Keine |
| Wiederherstellung |Backup-Tresor und Ziel Speicherkonto befinden sich in zwei verschiedenen Regionen – stellen Sie sicher, dass das im Wiederherstellungsvorgang angegebene Speicherkonto in derselben Azure-Region wie der sicherungstresor befindet. |Keine |
| Wiederherstellung |Speicherkonto für die angegebene der Restore-Vorgang nicht unterstützt – nur Basic-/Standard-Speicherkonten mit lokal redundanten oder georedundanten replikationseinstellungen unterstützt werden. Wählen Sie ein unterstütztes Speicherkonto |Keine |
| Wiederherstellung |Typ für den Wiederherstellungsvorgang angegebene speicherkontotyp ist nicht online - stellen Sie sicher, dass das im Wiederherstellungsvorgang angegebene Speicherkonto online ist. |Dies kann aufgrund eines vorübergehenden Fehlers im Azure-Speicher oder aufgrund eines Ausfalls auftreten. Wählen Sie ein anderes Speicherkonto aus. |
| Wiederherstellung |Ressourcengruppenkontingent wurde erreicht: Bitte löschen Sie einige Ressourcengruppen aus Azure-Portal, oder wenden Sie sich an Azure-Support, um die Grenzwerte zu erhöhen. |Keine |
| Wiederherstellung |Ausgewählte Subnetz ist nicht vorhanden.-Wählen Sie ein Subnetz die vorhanden ist |Keine |

## <a name="policy"></a>Richtlinie
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Richtlinie erstellen |Fehler beim Erstellen der Richtlinie – Bitte verringern Sie die Auswahl der Aufbewahrungsoptionen, um die Konfiguration fortzusetzen. |Keine |

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-the-vm-agent"></a>Einrichten von VM-Agent
In der Regel ist der VM-Agent bereits vorhanden in virtuellen Computern, die aus dem Azure-Katalog erstellt werden. Virtuelle Computer, die aus einer lokalen Datencentern migriert werden müssten jedoch nicht der VM-Agent installiert. Für solche virtuelle Computer muss der VM-Agent explizit installiert werden. Erfahren Sie mehr über [installieren den VM-Agent auf einem vorhandenen virtuellen Computer](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Für virtuelle Windows-Computer:

* Herunterladen und Installieren der [-Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Sie benötigen Administratorrechte, um die Installation abzuschließen.
* [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) um anzugeben, dass der Agent installiert ist.

Für virtuelle Linux-Computer:

* Installieren der neuesten [Linux-Agent](https://github.com/Azure/WALinuxAgent) von Github.
* [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) um anzugeben, dass der Agent installiert ist.

### <a name="updating-the-vm-agent"></a>Aktualisieren des VM-Agents
Für virtuelle Windows-Computer:

* Aktualisieren der VM-Agent ist so einfach wie die Neuinstallation der [VM-Agent-Binärdateien](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Allerdings müssen Sie sicherstellen, dass keine Sicherungsvorgang ausgeführt wird, während die VM-Agent aktualisiert wird.

Für virtuelle Linux-Computer:

* Befolgen Sie die Anweisungen auf [Linux VM-Agent aktualisieren](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>VM-Agent-Installation wird überprüft.
So überprüfen Sie für die VM-Agent-Version auf virtuellen Windows-Computer:

1. Melden Sie sich beim virtuellen Azure-Computer, und navigieren Sie zu dem Ordner *C:\WindowsAzure\Packages*. Finden Sie die Datei WaAppAgent.exe vorhanden ist.
2. Fahren Sie mit der rechten Maustaste **Eigenschaften**, und wählen Sie dann die **Details** Registerkarte. Das Feld Product Version sollte 2.6.1198.718 oder höher
