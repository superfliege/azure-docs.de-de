---
title: Upgrade auf Azure VM-Sicherungsstapel V2
description: Upgradeprozess und FAQs für VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 8/1/2018
ms.author: trinadhk
ms.openlocfilehash: 1021900620272cc5476d8972daf9d7e0a161797a
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398000"
---
# <a name="upgrade-to-azure-vm-backup-stack-v2"></a>Upgrade auf Azure VM-Sicherungsstapel V2

Das Resource Manager-Bereitstellungsmodell für das Upgrade auf den VM-Sicherungsstapel bietet folgende Featureverbesserungen:

* Die als Teil eines Sicherungsauftrags erstellte Momentaufnahme kann angezeigt werden, um für die Wiederherstellung verfügbar zu sein, ohne das Ende der Datenübertragung abzuwarten. Die Wartezeit, bis Momentaufnahmen vor dem Auslösen einer Wiederherstellung in den Tresor kopiert werden, wird reduziert. Darüber hinaus hebt diese Funktion die zusätzlichen Speicheranforderungen für das Sichern von Premium-VMs auf – die erste Sicherung ausgenommen.  

* Die Sicherungs- und Wiederherstellungszeiten werden reduziert, indem Momentaufnahmen sieben Tage lang lokal gespeichert werden.

* Unterstützung für Datenträgergrößen bis zu 4 TB.

* Die Möglichkeit zur Verwendung der ursprünglichen Speicherkonten einer nicht verwalteten VM bei der Wiederherstellung. Diese Funktion ist auch dann vorhanden, wenn die VM Datenträger enthält, die an mehrere Speicherkonten verteilt werden. Sie beschleunigt Wiederherstellungsvorgänge für verschiedene VM-Konfigurationen.
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

* Das Upgrade des VM-Sicherungsstapels ist unidirektional, d.h., alle Sicherungen wechseln in diesen Flow. Da die Änderung auf Abonnementebene vorgenommen wird, wechseln sämtliche VMs in diesen Flow. Alle neuen Featureergänzungen basieren auf demselben Stapel. Derzeit können Sie den Stapel nicht auf Richtlinienebene kontrollieren.

* Momentaufnahmen werden zur Verbesserung der Erstellung eines Wiederherstellungspunkts und zur Beschleunigung von Wiederherstellungsvorgängen lokal gespeichert. Aus diesem Grund werden Ihnen Speicherkosten angezeigt, die den Momentaufnahmen eines Zeitraums von sieben Tagen entsprechen.

* Inkrementelle Momentaufnahmen werden als Seitenblobs gespeichert. Für alle Kunden, die nicht verwaltete Datenträger verwenden, werden die 7-Tage-Momentaufnahmen berechnet, die im lokalen Speicherkonto des Kunden gespeichert sind. Da die Wiederherstellungspunktsammlungen, die bei Sicherungen von verwalteten VMs verwendet werden, Blobmomentaufnahmen auf der zugrunde liegenden Speicherebene verwenden, werden für verwaltete Datenträger Kosten angezeigt, die [Preisen für Blobmomentaufnahmen](https://docs.microsoft.com/rest/api/storageservices/understanding-how-snapshots-accrue-charges) entsprechen, und inkrementell sind. 

* Wenn Sie eine Wiederherstellung von einem Wiederherstellungspunkt für die Momentaufnahme für eine Premium-V durchführen, wird ein temporärer Speicherort verwendet, während die VM im Rahmen der Wiederherstellung erstellt wird.

* Bei Premium-Speicherkonten werden die für die Momentaufnahmen, die für die Punkte erstellt werden, die zur sofortigen Wiederherstellung dienen, zu den 10 TB des zugeordneten Speicherplatzes gezählt.

## <a name="upgrade"></a>Upgrade
### <a name="the-azure-portal"></a>Das Azure-Portal
Wenn Sie das Azure-Portal verwenden, wird Ihnen eine Benachrichtigung im Tresordashboard angezeigt. Diese Benachrichtigung bezieht sich auf die Unterstützung für große Datenträger und Verbesserungen der Geschwindigkeit beim Sichern und Wiederherstellen. Sie können auch zur Seite „Eigenschaften“ des Tresors navigieren, um die Upgradeoption abzurufen.

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Unterstützungsbenachrichtigung](./media/backup-azure-vms/instant-rp-banner.png) 

Um einen Bildschirm für das Upgrade auf den neuen Stapel zu öffnen, wählen Sie das Banner aus. 

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Upgrade](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Führen Sie die folgenden Cmdlets in einem PowerShell-Terminal mit erhöhten Rechten aus:
1.  Melden Sie sich bei Ihrem Azure-Konto an: 

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Wählen Sie das Abonnement aus, das Sie registrieren möchten:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registrieren Sie das folgende Abonnement:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Stellen Sie sicher, dass das Upgrade abgeschlossen ist
Führen Sie das folgende Cmdlet in einem PowerShell-Terminal mit erhöhten Rechten aus:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Wenn „Registriert“ anzeigt wird, wird Ihr Abonnement auf VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell, aktualisiert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Die folgenden Fragen und Antworten wurden Foren entnommen oder von Kunden eingereicht.

### <a name="will-upgrading-to-v2-impact-current-backups"></a>Hat das Upgrade auf V2 Einfluss auf aktuelle Sicherungen?

Wenn Sie ein Upgrade auf V2 durchführen, hat dies keinen Einfluss auf aktuelle Sicherungen, und Sie müssen Ihre Umgebung nicht neu konfigurieren. Wenn Sie das Upgrade durchführen, funktioniert Ihre Sicherungsumgebung wie gewohnt weiter.

### <a name="what-does-it-cost-to-upgrade-to-azure-vm-backup-stack-v2"></a>Was kostet ein Upgrade auf v2 des Azure-VM-Sicherungsstapels?

Für das Upgrade des Stapels auf v2 fallen keine Kosten an. Momentaufnahmen werden zur Beschleunigung des Erstellungsvorgangs von Wiederherstellungspunkten und Wiederherstellungsvorgängen lokal gespeichert. Aus diesem Grund werden Ihnen Speicherkosten angezeigt, die den Momentaufnahmen eines Zeitraums von sieben Tagen entsprechen.

### <a name="does-upgrading-to-stack-v2-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>Wird die Momentaufnahmengrenze von 10 TB für Premium-Speicherkonten durch das Upgrade auf V2 erhöht?

Nein, der Grenzwert für sämtliche Momentaufnahmen pro Speicherkonto beträgt weiterhin 10 TB. 

### <a name="in-premium-storage-accounts-do-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Halten Momentaufnahmen, die für die Punkte erstellt werden, die zur sofortigen Wiederherstellung dienen, in Storage Premium-Konten die 10 TB-Grenze für Momentaufnahmen ein?

Ja, Momentaufnahmen, die für die Punkte erstellt werden, die zur sofortigen Wiederherstellung dienen, halten in Premium-Speicherkonten die 10 TB-Grenze für Momentaufnahmen ein.

### <a name="how-does-the-snapshot-work-during-the-seven-day-period"></a>Wie funktioniert die Momentaufnahme in dem festgelegten Zeitraum von sieben Tagen? 

Es wird jeden Tag eine neue Momentaufnahme erstellt. Es gibt sieben unterschiedliche Momentaufnahmen. Der Dienst erstellt am ersten Tag **keine** Kopie und nimmt in den nächsten sechs Tagen keine Änderungen vor.

### <a name="is-a-v2-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Handelt es sich bei einer Momentaufnahme mit V2 um eine inkrementelle Momentaufnahme oder eine vollständige Momentaufnahme?

Inkrementelle Momentaufnahmen werden für nicht verwaltete Datenträger verwendet. Bei verwalteten Datenträgern werden bei der Wiederherstellungspunktsammlung, die von Azure Backup erstellt wird, Blobmomentaufnahmen verwendet. Diese sind daher inkrementell. 
