---
title: Azure-Funktion zur sofortigen Wiederherstellung
description: Azure-Funktion zur sofortigen Wiederherstellung und häufig gestellte Fragen für VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: sogup
ms.openlocfilehash: 1a25a9c3e0d099349286476f0ae3791efee1642f
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56452813"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Verbesserte Sicherungs- und Wiederherstellungsleistung mit der Azure Backup-Funktion zur sofortigen Wiederherstellung

> [!NOTE]
> Aufgrund des Feedbacks von Benutzern wird **VM-Sicherungsstapel V2** in **Sofortige Wiederherstellung** umbenannt, um Verwechslungen mit Azure Stack-Funktionen zu vermeiden.

Das neue Modell für die sofortige Wiederherstellung bietet die folgenden Featureverbesserungen:

* Die als Teil eines Sicherungsauftrags erstellte Momentaufnahme kann genutzt werden, um für die Wiederherstellung verfügbar zu sein, ohne das Ende der Datenübertragung an den Tresor abzuwarten. Die Wartezeit, bis Momentaufnahmen vor dem Auslösen einer Wiederherstellung in den Tresor kopiert werden, wird reduziert.
* Die Sicherungs- und Wiederherstellungszeiten werden reduziert, indem Momentaufnahmen standardmäßig zwei Tage lang lokal gespeichert werden. Dieser Standardtresor kann auf einen beliebigen Wert zwischen ein bis fünf Tagen konfiguriert werden.
* Unterstützt Datenträgergrößen bis zu 4 TB.
* Unterstützt Standard-SSD-Datenträger.
*   Die Möglichkeit zur Verwendung der ursprünglichen Speicherkonten (pro Datenträger) eines nicht verwalteten virtuellen Computers bei der Wiederherstellung. Diese Funktion ist auch dann vorhanden, wenn die VM Datenträger enthält, die an mehrere Speicherkonten verteilt werden. Sie beschleunigt Wiederherstellungsvorgänge für verschiedene VM-Konfigurationen.



## <a name="whats-new-in-this-feature"></a>Was ist neu in diesem Feature?

Der Sicherungsauftrag besteht derzeit aus zwei Phasen:

1.  Erstellen einer VM-Momentaufnahme.
2.  Übertragen der Momentaufnahme eines virtuellen Computers in den Azure Recovery Services-Tresor.

Ein Wiederherstellungspunkt gilt erst dann als erstellt, wenn die Phasen 1 und 2 abgeschlossen sind. Als Teil dieses Upgrades wird ein Wiederherstellungspunkt erstellt, sobald die Momentaufnahme abgeschlossen ist. Dieser Wiederherstellungspunkt vom Typ Momentaufnahme kann auch für eine Wiederherstellung mit demselben Wiederherstellungsablauf verwendet werden. Sie können diesen Wiederherstellungspunkt im Azure-Portal mit „Momentaufnahme“ als Wiederherstellungspunkttyp identifizieren. Nach Übertragung der Momentaufnahme in den Tresor ändert sich der Wiederherstellungspunkttyp in „Momentaufnahme und Tresor“.

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Speicher und Tresor](./media/backup-azure-vms/instant-rp-flow.png)

Momentaufnahmen werden sieben Tage lang aufbewahrt. Mit diesem Feature kann die Wiederherstellung über diese Momentaufnahmen mit reduzierten Wiederherstellungszeiten durchgeführt werden. In Szenarien mit nicht verwalteten Datenträgern wird die für das Transformieren und Kopieren der Daten aus dem Tresor zurück in das Speicherkonto des Kunden benötigte Zeit reduziert. Für Benutzer von verwalteten Datenträgern werden aus den Sicherungsdaten verwaltete Datenträger erstellt.

## <a name="feature-considerations"></a>Überlegungen zum Feature

* Momentaufnahmen werden zur Verbesserung der Erstellung eines Wiederherstellungspunkts und zur Beschleunigung von Wiederherstellungsvorgängen zusammen mit den Datenträgern gespeichert. Aus diesem Grund werden Ihnen Speicherkosten angezeigt, die den Momentaufnahmen aus diesem Zeitraum entsprechen.
* Inkrementelle Momentaufnahmen werden als Seitenblobs gespeichert. Für alle Benutzer, die nicht verwaltete Datenträger verwenden, werden die Momentaufnahmen berechnet, die in ihrem lokalen Speicherkonto gespeichert sind. Da die Wiederherstellungspunktsammlungen für Sicherungen von verwalteten virtuellen Computern Blobmomentaufnahmen auf der zugrunde liegenden Speicherebene verwenden, werden für verwaltete Datenträger Kosten angezeigt, die Preisen für Blobmomentaufnahmen entsprechen, und inkrementell sind.
* Bei Premium-Speicherkonten werden die für die Momentaufnahmen, die für die Punkte erstellt werden, die zur sofortigen Wiederherstellung dienen, zu den 10 TB des zugeordneten Speicherplatzes gezählt.
* Sie haben die Möglichkeit, die Aufbewahrung von Momentaufnahmen entsprechend den Wiederherstellungsanforderungen zu konfigurieren. Je nach Anforderungen können Sie die Aufbewahrung von Momentaufnahmen auf dem Blatt „Sicherungsrichtlinie“ auf ein Minimum von einem Tag festlegen, wie es weiter unten beschrieben ist. Dadurch können Sie Kosten für die Aufbewahrung von Momentaufnahmen sparen, wenn Wiederherstellungen nicht häufig ausgeführt werden.


>[!NOTE]
>Mit diesem Upgrade für die sofortige Wiederherstellung wird die Aufbewahrungsdauer für Momentaufnahmen für alle Kunden (**sowohl neue als auch bestehende**) auf einen Standardwert von zwei Tagen festgelegt. Sie können die Dauer aber auch nach Bedarf auf einen beliebigen Wert zwischen ein und fünf Tagen festlegen.


## <a name="cost-impact"></a>Kostenauswirkung

Die inkrementellen Momentaufnahmen werden im Speicherkonto des virtuellen Computers gespeichert, der für die sofortige Wiederherstellung verwendet wird. Inkrementelle Momentaufnahme bedeutet, dass der durch eine Momentaufnahme belegte Speicherplatz dem Platz entspricht, der von Seiten belegt wird, die nach der Erstellung der Momentaufnahme geschrieben werden. Die Abrechnung erfolgt weiterhin pro GB für den durch die Momentaufnahme belegten Speicherplatz, und der Preis pro GB entspricht dem Preis auf der [Preisseite](https://azure.microsoft.com/pricing/details/managed-disks/).


## <a name="upgrading-to-instant-restore"></a>Upgrade auf die sofortige Wiederherstellung

Wenn Sie das Azure-Portal verwenden, wird Ihnen eine Benachrichtigung im Tresordashboard angezeigt. Diese Benachrichtigung bezieht sich auf die Unterstützung für große Datenträger und Verbesserungen der Geschwindigkeit beim Sichern und Wiederherstellen.
Wählen Sie das Banner aus, um einen Bildschirm für das Upgrade auf die sofortige Wiederherstellung zu öffnen.

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Unterstützungsbenachrichtigung](./media/backup-azure-vms/instant-rp-banner.png)

Klicken Sie auf **Upgrade** wie im folgenden Screenshot gezeigt:

![Sicherungsauftrag im VM-Sicherungsstapel, Resource Manager-Bereitstellungsmodell – Upgrade](./media/backup-azure-vms/instant-rp.png)

Sie können auch zur Seite **Eigenschaften** des Tresors navigieren, um unter **VM-Sicherungsstapel** die Option **Upgrade** aufzurufen.

![Sicherungsauftrag in VM-Sicherungsstapel: Seite „Eigenschaften“](./media/backup-azure-vms/instant-restore-capability-properties.png)


## <a name="configure-snapshot-retention-using-azure-portal"></a>Konfigurieren der Aufbewahrung von Momentaufnahmen über das Azure-Portal
Diese Option ist derzeit in den Regionen „USA, Westen-Mitte“, „Indien, Süden“ und „Australien, Osten“ verfügbar.

Für die aktualisierten Benutzer wird im Azure-Portal ein zusätzliches Feld auf dem Blatt **VM-Sicherungsrichtlinie** unter dem Abschnitt **Sofortige Wiederherstellung** angezeigt. Sie können die Aufbewahrungsdauer für Momentaufnahmen über das Blatt **VM-Sicherungsrichtlinie** für alle virtuellen Computer ändern, die der jeweiligen Sicherungsrichtlinie zugeordnet sind.

![Funktion zur sofortigen Wiederherstellung](./media/backup-azure-vms/instant-restore-capability.png)

## <a name="upgrade-to-instant-restore-using-powershell"></a>Upgrade auf die sofortige Wiederherstellung mithilfe von PowerShell

Wenn Sie Self-Service verwenden und ein Upgrade auf die sofortige Wiederherstellung ausführen möchten, führen Sie die folgenden Cmdlets in einem PowerShell-Terminal mit erhöhten Rechten aus:

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

## <a name="upgrade-to-instant-restore-using-cli"></a>Upgrade auf die sofortige Wiederherstellung mithilfe der CLI

Führen Sie die folgenden Befehle in einer Shell aus:

1.  Melden Sie sich bei Ihrem Azure-Konto an:

    ```
    az login
    ```

2.  Wählen Sie das Abonnement aus, das Sie registrieren möchten:

    ```
    az account set --subscription "Subscription Name"
    ```

3.  Registrieren Sie das folgende Abonnement:

    ```
    az feature register --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
    ```

## <a name="verify-that-the-upgrade-is-successful"></a>Sicherstellen, dass das Upgrade erfolgreich war

### <a name="powershell"></a>PowerShell
Führen Sie das folgende Cmdlet in einem PowerShell-Terminal mit erhöhten Rechten aus:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" -ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Führen Sie den folgenden Befehle in einer Shell aus:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

Wenn „Registriert“ anzeigt wird, wurde Ihr Abonnement auf die sofortige Wiederherstellung aktualisiert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Welche Kosten sind mit der sofortigen Wiederherstellung verbunden?
Momentaufnahmen werden zur Beschleunigung der Erstellung eines Wiederherstellungspunkts sowie von Wiederherstellungsvorgängen zusammen mit den Datenträgern gespeichert. Daher werden Ihnen Speicherkosten angezeigt, die der Vermerkdauer für Momentaufnahmen entsprechen, die im Rahmen der VM-Sicherungsrichtlinie ausgewählt wurde.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Halten die Momentaufnahmen, die für Punkte zur sofortigen Wiederherstellung erstellt werden, in Storage Premium-Konten die 10 TB-Grenze für Momentaufnahmen ein?
Ja, Momentaufnahmen, die für die Punkte erstellt werden, die zur sofortigen Wiederherstellung dienen, halten in Premium-Speicherkonten die 10 TB-Grenze für Momentaufnahmen ein.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Wie funktioniert die Aufbewahrung von Momentaufnahmen im Zeitraum von fünf Tagen?
Jeden Tag wird eine neue Momentaufnahme erstellt, sodass dann fünf einzelne Momentaufnahmen vorhanden sind. Die Größe der Momentaufnahme hängt von der Datenänderungsrate ab, die in den meisten Fällen etwa 2–7% beträgt.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Handelt es sich bei einer Momentaufnahme für die sofortige Wiederherstellung um eine inkrementelle Momentaufnahme oder eine vollständige Momentaufnahme?
Momentaufnahmen, die im Rahmen der Funktion für die sofortige Wiederherstellung erstellt werden, sind inkrementelle Momentaufnahmen.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Wie kann ich den ungefähren Kostenanstieg durch die Funktion zur sofortigen Wiederherstellung berechnen?
Dies hängt von der Datenänderungsrate des virtuellen Computers ab. Bei einem stabilen Zustand können Sie von folgender Rechnung ausgehen: Kostenanstieg = Aufbewahrungszeitraum der Momentaufnahme * tägliche Änderungsrate pro virtuellem Computer * Speicherkosten pro GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Wenn der Wiederherstellungstyp für einen Wiederherstellungspunkt „Momentaufnahme und Tresor“ ist, und ich einen Wiederherstellungsvorgang durchführe, welcher Wiederherstellungstyp wird dann verwendet?
Wenn der Wiederherstellungstyp „Momentaufnahme und Tresor“ ist, erfolgt die Wiederherstellung automatisch aus der lokalen Momentaufnahme. Dieser Vorgang ist verglichen mit einer Wiederherstellung aus dem Tresor viel schneller.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>Was passiert, wenn ich für den Aufbewahrungszeitraum des Wiederherstellungspunkts (Tarif 2) einen geringeren Wert als für den Aufbewahrungszeitraum der Momentaufnahme (Tarif 1) auswähle?
Das neue Modell lässt das Löschen des Wiederherstellungspunkts (Tarif2) nur zu, wenn die Momentaufnahme (Tarif1) gelöscht wird. Es empfiehlt sich, den Aufbewahrungszeitraum für den Wiederherstellungspunkt (Tarif2) auf einen höheren Wert als den Aufbewahrungszeitraum der Momentaufnahme festzulegen.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Warum ist meine Momentaufnahme auch nach Ablauf des in der Sicherungsrichtlinie festgelegten Aufbewahrungszeitraum noch vorhanden?
Wenn der Wiederherstellungspunkt eine Momentaufnahme aufweist und diese der letzte verfügbare Wiederherstellungspunkt ist, wird sie bis zur nächsten erfolgreichen Sicherung aufbewahrt. Dies entspricht der aktuellen konzipierten GC-Richtlinie. Diese gibt vor, dass immer mindestens ein aktueller Wiederherstellungspunkt vorhanden ist, für den Fall, dass alle weiteren Sicherungen aufgrund eines Problems auf dem virtuellen Computer fehlschlagen. In normalen Szenarien werden Wiederherstellungspunkte innerhalb von 24 Stunden nach ihrem Ablauf bereinigt.
