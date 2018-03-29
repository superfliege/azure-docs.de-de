---
title: Upgrade auf Azure VM-Sicherungsstapel V2 | Microsoft-Dokumentation
description: Upgradeprozess und häufig gestellte Fragen zu VM-Sicherungsstapel V2
services: backup, virtual-machines
documentationcenter: ''
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: ''
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/08/2018
ms.author: trinadhk, sogup
ms.openlocfilehash: 6d214072bccb8b2b42828ee003dcf349985b4f43
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="upgrade-to-vm-backup-stack-v2"></a>Upgrade auf VM-Sicherungsstapel V2
Das Upgrade auf VM-Sicherungsstapel V2 bietet die folgenden Featureverbesserungen:
* Die als Teil eines Sicherungsauftrags erstellte Momentaufnahme kann angezeigt werden, um für die Wiederherstellung verfügbar zu sein, ohne das Ende der Datenübertragung abzuwarten.
Dies reduziert die Wartezeit, bis die Momentaufnahme in den Tresor kopiert ist, bevor die Wiederherstellung ausgelöst wird. Außerdem hebt dies die zusätzlichen Speicheranforderungen für das Sichern von Premium-VMs auf – die erste Sicherung ausgenommen.  

* Die Sicherungs- und Wiederherstellungszeiten werden reduziert, weil Momentaufnahmen 7 Tage lang lokal beibehalten werden. 

* Unterstützung für Datenträgergrößen bis zu 4 TB.  

* Ursprüngliche Speicherkonten können bei einer Wiederherstellung eines nicht verwalteten virtuellen Computers verwendet werden (auch wenn die VM Datenträger aufweist, die über mehrere Speicherkonten verteilt sind). Dies beschleunigt die Wiederherstellung für eine Vielzahl von VM-Konfigurationen. 
    > [!NOTE] 
    > Dies ist nicht identisch mit dem Überschreiben des ursprünglichen virtuellen Computers. 
    > 
    >

## <a name="what-is-changing-in-the-new-stack"></a>Was ändert sich im neuen Stapel?
Der Sicherungsauftrag besteht heute aus zwei Phasen:
1.  Erstellen der VM-Momentaufnahme. 
2.  Übertragen der VM-Momentaufnahme in den Azure Backup-Tresor. 

Ein Wiederherstellungspunkt gilt erst dann als erstellt, wenn die Phasen 1 und 2 abgeschlossen sind. Als Teil des neuen Stapels wird ein Wiederherstellungspunkt erstellt, sobald die Momentaufnahme abgeschlossen ist. Sie können auch von diesem Wiederherstellungspunkt aus mit demselben Wiederherstellungsablauf wiederherstellen. Sie können diesen Wiederherstellungspunkt im Azure-Portal mit „Momentaufnahme“ als Wiederherstellungspunkttyp identifizieren. Nach Übertragung der Momentaufnahme in den Tresor ändert sich der Wiederherstellungspunkttyp in „Momentaufnahme und Tresor“. 

![Sicherungsauftrag in VM-Sicherungsstapel V2](./media/backup-azure-vms/instant-rp-flow.jpg) 

Standardmäßig werden Momentaufnahmen sieben Tage lang beibehalten. So kann die Wiederherstellung über diese Momentaufnahmen schneller ausgeführt werden, da der Zeitaufwand für das Zurückkopieren der Daten aus dem Tresor auf das Kundenspeicherkonto reduziert wird. 

## <a name="considerations-before-upgrade"></a>Überlegungen vor dem Upgrade
* Dies ist ein unidirektionales Upgrade des VM-Sicherungsstapels. Deshalb werden alle zukünftige Sicherungen diesem Ablauf folgen. Aufgrund der **Aktivierung auf Abonnementebene gehen alle virtuellen Computer in diesen Ablauf ein**. Alle neuen Featureergänzungen basieren auf demselben Stapel. In zukünftigen Releases wird es möglich sein, dies auf Richtlinienebene zu steuern. 
* Stellen Sie bei virtuellen Computern mit Premium-Datenträgern während der ersten Sicherung sicher, dass der Größe des virtuellen Computers entsprechender Speicherplatz im Speicherkonto verfügbar ist, bis die erste Sicherung abgeschlossen ist. 
* Da Momentaufnahmen lokal gespeichert werden, um das Erstellen eines Wiederherstellungspunkts zu unterstützen und auch die Wiederherstellung zu beschleunigen, werden Ihnen während des Zeitraums von sieben Tagen die Speicherkosten für die Momentaufnahmen angezeigt.
* Wenn Sie eine Wiederherstellung von einem Momentaufnahmen-Wiederherstellungspunkt aus für eine Premium-VM durchführen, sehen Sie, das ein temporärer Speicherort verwendet wird, während der virtuelle Computer im Rahmen der Wiederherstellung erstellt wird. 

## <a name="how-to-upgrade"></a>Wie werden Upgrades durchgeführt?
### <a name="the-azure-portal"></a>Das Azure-Portal
Wenn Sie das Azure-Portal verwenden, wird eine Benachrichtigung auf dem Dashboard des Tresors angezeigt, die sich auf die Unterstützung großer Datenträger und Geschwindigkeitsverbesserungen bei Sichern und Wiederherstellen bezieht.

![Sicherungsauftrag in VM-Sicherungsstapel V2](./media/backup-azure-vms/instant-rp-banner.png) 

Um einen Bildschirm für das Upgrade auf den neuen Stapel zu öffnen, wählen Sie das Banner aus. 

![Sicherungsauftrag in VM-Sicherungsstapel V2](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Führen Sie die folgenden Cmdlets in einem PowerShell-Terminal mit erhöhten Rechten aus:
1.  Melden Sie sich beim Azure-Konto an. 

```
PS C:> Login-AzureRmAccount
```

2.  Wählen Sie das Abonnement aus, das Sie für die Vorschau registrieren möchten:

```
PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
```

3.  Registrieren Sie dieses Abonnement für die private Vorschau:

```
PS C:>  Register-AzureRmProviderFeature -FeatureName “InstantBackupandRecovery” –ProviderNamespace Microsoft.RecoveryServices
```

## <a name="verify-whether-the-upgrade-is-complete"></a>Überprüfen Sie, ob das Upgrade abgeschlossen ist.
Führen Sie das folgende Cmdlet in einem PowerShell-Terminal mit erhöhten Rechten aus:

```
Get-AzureRmProviderFeature -FeatureName “InstantBackupandRecovery” –ProviderNamespace Microsoft.RecoveryServices
```

Wenn „Registriert“ anzeigt wird, wird Ihr Abonnement auf VM-Sicherungsstapel V2 aktualisiert. 



