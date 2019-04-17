---
title: Verschieben eines Recovery Services-Tresors zwischen Azure-Abonnements oder in eine andere Ressourcengruppe
description: Anweisungen zum Verschieben eines Recovery Services-Tresors zwischen Azure-Abonnements und Ressourcengruppen.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sogup
ms.openlocfilehash: f4ab983fbebe9c0219e70fa7bd5742cf1c3a0491
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361974"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups-limited-public-preview"></a>Verschieben eines Recovery Services-Tresors zwischen Azure-Abonnements und Ressourcengruppen (Eingeschränkte Public Preview)

In diesem Artikel wird das Verschieben eines für Azure Backup konfigurierten Recovery Services-Tresor zwischen Azure-Abonnements oder in eine andere Ressourcengruppe im selben Abonnement erläutert. Sie können Azure-Portal oder PowerShell zum Verschieben eines Recovery Services-Tresors verwenden.

> [!NOTE]
> Zum Verschieben eines Recovery Services-Tresors und der zugehörigen Ressourcen in eine andere Ressourcengruppe sollten Sie zuerst [das Quellabonnement registrieren](#register-the-source-subscription-to-move-your-recovery-services-vault).

## <a name="supported-geos"></a>Unterstützte geografische Räume

Das Verschieben von Ressourcen für den Recovery Services-Tresor wird derzeit unterstützt in „Australien, Osten“, „Australien, Südosten“, „Kanada, Mitte“, „Kanada, Osten“, „Asien, Südosten“, „Asien, Osten“, „USA, Mitte“, „USA, Norden-Mitte“, „USA, Osten“, „USA, Osten 2“, „USA, Süden-Mitte“, „USA, Westen-Mitte“, „USA, Westen-Mitte 2“, „USA, Westen“, „Indien, Mitte“, „Indien, Süden“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „Europa, Norden“, „Europa, Westen“, „Südafrika, Norden“, „Südafrika, Westen“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „VAE, Mitte“ und „VAE, Norden“.

## <a name="prerequisites-for-moving-a-vault"></a>Voraussetzungen für das Verschieben eines Tresors

- Beim Verschieben zwischen Ressourcengruppen werden die Quell- und Zielressourcengruppe für die Dauer des Vorgangs gesperrt. Schreib- und Löschvorgänge in den Ressourcengruppen werden bis zum Abschluss der Verschiebung blockiert.
- Nur der Abonnementadministrator verfügt über die Berechtigungen, einen Tresor zu verschieben.
- Beim Verschieben eines Tresors zwischen Abonnements muss das Zielabonnement in einem aktivierten Status vorhanden sein und sich in demselben Mandanten befinden wie das Quellabonnement.
- Sie benötigen die Berechtigung zum Ausführen der Schreibvorgänge in der Zielressourcengruppe.
- Sie können den Speicherort des Recovery Services-Tresors nicht ändern. Das Verschieben des Tresors ändert nur die Ressourcengruppe. Die neue Ressourcengruppe hat möglicherweise einen anderen Speicherort, das ändert jedoch nicht den Speicherort der Ressource.
- Derzeit können Sie jeweils einen Recovery Services-Tresor pro Region verschieben.
- Wenn ein virtueller Computer nicht mit dem Recovery Services-Tresor zwischen Abonnements oder in eine neue Ressourcengruppe verschoben wird, bleiben die aktuellen VM-Wiederherstellungspunkte im Tresor intakt, bis sie ablaufen.
- Unabhängig davon, ob die VM mit dem Tresor verschoben wird oder nicht, können Sie die VM immer anhand des Sicherungsverlaufs im Tresor wiederherstellen.
- Azure Disk Encryption setzt voraus, dass Ihr Schlüsseltresor und die VMs zur selben Azure-Region und zum selben Azure-Abonnement gehören.
- Informationen zum Verschieben eines virtuellen Computers mit verwalteten Datenträgern finden Sie in diesem [Artikel](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/).
- Die Optionen zum Verschieben von Ressourcen, die über das klassische Modell bereitgestellt wurden, unterscheiden sich abhängig davon, ob Sie die Ressourcen innerhalb eines Abonnements oder in ein neues Abonnement verschieben. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources#classic-deployment-limitations).
- Für den Tresor definierte Sicherungsrichtlinien werden beibehalten, nachdem der Tresor zwischen Abonnements oder in eine neue Ressourcengruppe verschoben worden ist.
- Derzeit können Sie Tresore, die Azure Files-, Azure File Sync- oder SQL-in-IaaS-VMs enthalten, nicht zwischen Abonnements und Ressourcengruppen verschieben.
- Wenn Sie einen Tresor mit VM-Sicherungsdaten abonnementübergreifend verschieben, müssen Sie Ihre VMs in das gleiche Abonnement verschieben und die gleiche Zielressourcengruppe verwenden, um weiterhin Sicherungen durchführen zu können.<br>

> [!NOTE]
>
> Für die Verwendung mit **Azure Site Recovery** konfigurierte Recovery Services-Tresore können noch nicht verschoben werden. Wenn Sie virtuelle (Azure IaaS, Hyper-V, VMware) oder physische Computer für die Notfallwiederherstellung mit **Azure Site Recovery** konfiguriert haben, wird der Verschiebevorgang blockiert. Die Ressourcenverschiebefunktion für den Site Recovery-Dienst ist noch nicht verfügbar.

## <a name="register-the-source-subscription-to-move-your-recovery-services-vault"></a>Registrieren des Quellabonnements zum Verschieben Ihres Recovery Services-Tresors

Um das Quellabonnement für das **Verschieben** Ihres Recovery Services-Tresors zu registrieren, führen Sie die folgenden Cmdlets im PowerShell-Terminal aus:

1. Anmelden bei Ihrem Azure-Konto

   ```
   Connect-AzureRmAccount
   ```

2. Auswählen des Abonnements, das Sie registrieren möchten

   ```
   Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
   ```
3. Registrieren dieses Abonnements

   ```
   Register-AzureRmProviderFeature -ProviderNamespace Microsoft.RecoveryServices -FeatureName RecoveryServicesResourceMove
   ```

4. Führen Sie den folgenden Befehl aus:

   ```
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
   ```

Warten Sie 30 Minuten, bis das Abonnement der Whitelist hinzugefügt worden ist, bevor Sie mit der Verschiebung mittels Azure-Portal oder PowerShell beginnen.

## <a name="use-azure-portal-to-move-a-recovery-services-vault-to-different-resource-group"></a>Verwenden des Azure-Portals zum Verschieben eines Recovery Services-Tresors in eine andere Ressourcengruppe

So verschieben Sie einen Recovery Services-Tresor und die zugehörigen Ressourcen in eine andere Ressourcengruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie in der Liste der **Recovery Services-Tresore** den Tresor aus, den Sie verschieben möchten. Wenn das Tresordashboard geöffnet wird, wird es wie in der folgenden Abbildung dargestellt angezeigt.

   ![Öffnen des Recovery Services-Tresors](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

   Wenn Sie die **Essentials**-Informationen für Ihren Tresor nicht sehen, klicken Sie auf das Dropdownsymbol. Jetzt sollten die Essentials-Informationen für Ihren Tresor angezeigt werden.

   ![Registerkarte „Essentials“-Informationen](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Klicken Sie im Übersichtsmenü des Tresors auf **Ändern** neben **Ressourcengruppe**, um das Blatt **Ressourcen verschieben** zu öffnen.

   ![Ressourcengruppe ändern](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. Auf dem Blatt **Ressourcen verschieben** für den ausgewählten Tresor sollten die optionalen verwandten Ressourcen durch Aktivieren des Kontrollkästchens verschoben werden, wie in der folgenden Abbildung dargestellt.

   ![Abonnement verschieben](./media/backup-azure-move-recovery-services/move-resource.png)

5. Um die Zielressourcengruppe hinzuzufügen, wählen Sie in der Dropdownliste **Ressourcengruppe** eine vorhandene Ressource aus, oder klicken Sie auf die Option **Neue Gruppe erstellen**.

   ![Resource erstellen](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. Bestätigen Sie nach dem Hinzufügen der Ressourcengruppe mit der Option **Mir ist bewusst, dass Tools und Skripts, die den verschobenen Ressourcen zugeordnet sind, erst funktionieren, wenn ich sie auf die Verwendung neuer Ressourcen-IDs und Namespacereferenzen aktualisiere**, und klicken Sie dann auf **OK**, um das Verschieben des Tresors abzuschließen.

   ![Bestätigungsmeldung](./media/backup-azure-move-recovery-services/confirmation-message.png)


## <a name="use-azure-portal-to-move-a-recovery-services-vault-to-a-different-subscription"></a>Verwenden des Azure-Portals zum Verschieben eines Recovery Services-Tresors in ein anderes Abonnement

Sie können einen Recovery Services-Tresor und die zugehörigen Ressourcen in ein anderes Abonnement verschieben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie in der Liste der Recovery Services-Tresore den Tresor aus, den Sie verschieben möchten. Wenn das Tresordashboard geöffnet wird, wird es wie in der folgenden Abbildung dargestellt angezeigt.

    ![Öffnen des Recovery Services-Tresors](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    Wenn Sie die **Essentials**-Informationen für Ihren Tresor nicht sehen, klicken Sie auf das Dropdownsymbol. Jetzt sollten die Essentials-Informationen für Ihren Tresor angezeigt werden.

    ![Registerkarte „Essentials“-Informationen](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Klicken Sie im Übersichtsmenü des Tresors auf **Ändern** neben **Abonnement**, um das Blatt **Ressourcen verschieben** zu öffnen.

   ![Abonnement ändern](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. Wählen Sie die zu verschiebenden Ressourcen aus. Hier sollten Sie die Option **Alles markieren** verwenden, um alle aufgeführten optionalen Ressourcen auszuwählen.

   ![Ressource verschieben](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. Wählen Sie das Zielabonnement, in das der Tresor verschoben werden soll, in der Dropdownliste **Abonnement** aus.
6. Um die Zielressourcengruppe hinzuzufügen, wählen Sie in der Dropdownliste **Ressourcengruppe** eine vorhandene Ressource aus, oder klicken Sie auf die Option **Neue Gruppe erstellen**.

   ![Abonnement hinzufügen](./media/backup-azure-move-recovery-services/add-subscription.png)

7. Klicken Sie zur Bestätigung auf **Mir ist bewusst, dass Tools und Skripts, die den verschobenen Ressourcen zugeordnet sind, erst funktionieren, wenn ich sie auf die Verwendung neuer Ressourcen-IDs** aktualisiere, und klicken Sie auf **OK**.

> [!NOTE]
> Abonnementübergreifende Sicherung (RS-Tresor und geschützte virtuelle Computer befinden sich in verschiedenen Abonnements) wird nicht unterstützt. Außerdem kann die Speicherredundanzoption von lokalem redundantem Speicher (LRS) zu globalem redundantem Speicher (GRS) und umgekehrt während der Verschiebung des Tresors nicht geändert werden.
>
>

## <a name="use-powershell-to-move-a-vault"></a>Verschieben eines Tresors mithilfe von PowerShell

Um einen Recovery Services-Tresor in eine andere Ressourcengruppe zu verschieben, verwenden Sie das `Move-AzureRMResource`-Cmdlet. `Move-AzureRMResource` benötigt den Ressourcennamen und den Typ der Ressource. Sie erhalten beides über das `Get-AzureRmRecoveryServicesVault`-Cmdlet.

```
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Um Ressourcen in ein anderes Abonnement zu verschieben, schließen Sie den `-DestinationSubscriptionId`-Parameter ein.

```
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Nach Ausführen der obigen Cmdlets werden Sie aufgefordert, zu bestätigen, dass Sie die angegebenen Ressourcen verschieben möchten. Geben Sie **J** zur Bestätigung ein. Nach einer erfolgreichen Validierung wird die Ressource verschoben.

## <a name="use-cli-to-move-a-vault"></a>Verwenden von CLI, um einen Tresor zu verschieben

Um einen Recovery Services-Tresor in eine andere Ressourcengruppe zu verschieben, verwenden Sie das folgende Cmdlet:

```
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

Um Ressourcen in ein neues Abonnement zu verschieben, geben Sie den Parameter`--destination-subscription-id` an.

## <a name="post-migration"></a>Nach der Migration

1. Sie müssen die Zugriffssteuerungen für die Ressourcengruppen festlegen/überprüfen.  
2. Die Funktion zur Berichterstellung über die Sicherung und deren Überwachung muss nach Abschluss der Verschiebung für den Tresor neu konfiguriert werden. Die vorherige Konfiguration geht während des Verschiebevorgangs verloren.



## <a name="next-steps"></a>Nächste Schritte

Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben.

Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
