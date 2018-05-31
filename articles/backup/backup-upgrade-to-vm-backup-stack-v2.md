---
title: Upgrade auf das Azure Resource Manager-Bereitstellungsmodell für Azure VM-Sicherungsstapel | Microsoft-Dokumentation
description: Upgradeprozess und FAQs für VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell
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
ms.openlocfilehash: 1e5515486afac5a6d84a35bca33f55ae98e287d3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764858"
---
# <a name="upgrade-to-the-azure-resource-manager-deployment-model-for-azure-vm-backup-stack"></a>Upgrade auf das Azure Resource Manager-Bereitstellungsmodell für Azure VM-Sicherungsstapel
Das Resource Manager-Bereitstellungsmodell für das Upgrade auf den VM-Sicherungsstapel bietet folgende Featureverbesserungen:
* Die als Teil eines Sicherungsauftrags erstellte Momentaufnahme kann angezeigt werden, um für die Wiederherstellung verfügbar zu sein, ohne das Ende der Datenübertragung abzuwarten. Die Wartezeit, bis Momentaufnahmen vor dem Auslösen einer Wiederherstellung in den Tresor kopiert werden, wird reduziert. Darüber hinaus hebt diese Funktion die zusätzlichen Speicheranforderungen für das Sichern von Premium-VMs auf – die erste Sicherung ausgenommen.  

* Die Sicherungs- und Wiederherstellungszeiten werden reduziert, weil Momentaufnahmen sieben Tage lang lokal beibehalten werden.

* Unterstützung für Datenträgergrößen bis zu 4 TB.

* Die Möglichkeit zur Verwendung der ursprünglichen Speicherkonten einer nicht verwalteten VM bei der Wiederherstellung. Diese Möglichkeit ist auch dann vorhanden, wenn die VM Datenträger enthält, die über Speicherkonten verteilt werden. Sie beschleunigt die Wiederherstellung bei einer Vielzahl von VM-Konfigurationen.
    > [!NOTE] 
    > Diese Funktion entspricht nicht dem Überschreiben der ursprünglichen VM. 
    >

## <a name="whats-changing-in-the-new-stack"></a>Was ändert sich im neuen Stapel?
Der Sicherungsauftrag besteht derzeit aus zwei Phasen:
1.  Erstellen einer VM-Momentaufnahme. 
2.  Übertragen einer VM-Momentaufnahme in den Azure Backup-Tresor. 

Ein Wiederherstellungspunkt gilt erst dann als erstellt, wenn die Phasen 1 und 2 abgeschlossen sind. Als Teil des neuen Stapels wird ein Wiederherstellungspunkt erstellt, sobald die Momentaufnahme abgeschlossen ist. Sie können die Wiederherstellung auch von diesem Wiederherstellungspunkt aus mit demselben Wiederherstellungsablauf durchführen. Sie können diesen Wiederherstellungspunkt im Azure-Portal mit „Momentaufnahme“ als Wiederherstellungspunkttyp identifizieren. Nach Übertragung der Momentaufnahme in den Tresor ändert sich der Wiederherstellungspunkttyp in „Momentaufnahme und Tresor“. 

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Speicher und Tresor](./media/backup-azure-vms/instant-rp-flow.jpg) 

Momentaufnahmen werden standardmäßig sieben Tage lang aufbewahrt. Mit diesem Feature kann die Wiederherstellung über diese Momentaufnahmen schneller erfolgen. Es verringert die Zeit, die für das Kopieren der Daten aus dem Tresor in das Speicherkonto des Kunden benötigt wird. 

## <a name="considerations-before-upgrade"></a>Überlegungen vor dem Upgrade
* Das Upgrade des VM-Sicherungsstapels ist unidirektional. Folglich wechseln alle Sicherungen in diesen Fluss. Da eine Aktivierung auf Abonnementebene vorliegt, wechseln sämtliche VMs in diesen Fluss. Alle neuen Featureergänzungen basieren auf demselben Stapel. In zukünftigen Releases wird es möglich sein, dies auf Richtlinienebene zu steuern.

* Momentaufnahmen werden zur Verbesserung der Erstellung eines Wiederherstellungspunkts und zur Beschleunigung der Wiederherstellung lokal gespeichert. Daher werden Ihnen Speicherkosten angezeigt, die den Momentaufnahmen in einem Zeitraum von sieben Tagen entsprechen.

* Inkrementelle Momentaufnahmen werden als Seitenblobs gespeichert. Für alle Kunden, die nicht verwaltete Datenträger verwenden, werden die 7-Tage-Momentaufnahmen berechnet, die im lokalen Speicherkonto des Kunden gespeichert sind. Gemäß dem aktuellen Preismodell entstehen Kunden keine Kosten für verwaltete Datenträger.

* Wenn Sie eine Wiederherstellung von einem Momentaufnahmen-Wiederherstellungspunkt aus für eine Premium-VM durchführen, sehen Sie, dass ein temporärer Speicherort verwendet wird, während die VM im Rahmen der Wiederherstellung erstellt wird.

* Bei Premium-Speicherkonten belegen die Momentaufnahmen, die für eine sofortige Wiederherstellung erfasst werden, 10 TB des zugeordneten Speicherplatzes.

## <a name="upgrade"></a>Upgrade
### <a name="the-azure-portal"></a>Das Azure-Portal
Wenn Sie das Azure-Portal verwenden, wird Ihnen eine Benachrichtigung im Tresordashboard angezeigt. Diese Benachrichtigung bezieht sich auf die Unterstützung für große Datenträger und Verbesserungen der Geschwindigkeit beim Sichern und Wiederherstellen.

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Unterstützungsbenachrichtigung](./media/backup-azure-vms/instant-rp-banner.png) 

Um einen Bildschirm für das Upgrade auf den neuen Stapel zu öffnen, wählen Sie das Banner aus. 

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Upgrade](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Führen Sie die folgenden Cmdlets in einem PowerShell-Terminal mit erhöhten Rechten aus:
1.  Melden Sie sich bei Ihrem Azure-Konto an: 

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Wählen Sie das Abonnement aus, das Sie für die Vorschau registrieren möchten:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registrieren Sie dieses Abonnement für die private Vorschau:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Stellen Sie sicher, dass das Upgrade abgeschlossen ist
Führen Sie das folgende Cmdlet in einem PowerShell-Terminal mit erhöhten Rechten aus:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Wenn „Registriert“ anzeigt wird, wird Ihr Abonnement auf VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell, aktualisiert.
