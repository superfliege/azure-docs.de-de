---
title: Automatische Betriebssystemupgrades mit Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Betriebssystem automatisch auf VM-Instanzen in einer Skalierungsgruppe aktualisieren.
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: guybo
ms.openlocfilehash: 32358b23bb0a0a878e986150dd992513579d61c4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Automatische Betriebssystemupgrades für Azure-VM-Skalierungsgruppen

Das automatische Upgrade von Betriebssystemimages ist ein Feature der Vorschauversion von Azure VM-Skalierungsgruppen, mit der alle virtuellen Computer automatisch auf das neueste Betriebssystemimage aktualisiert werden.

Das automatische Betriebssystemupgrade weist folgende Merkmale auf:

- Nach der Konfiguration wird das neueste von den Imageherausgebern veröffentlichte Betriebssystemimage automatisch ohne Benutzereingriff auf die Skalierungsgruppe angewendet.
- Aktualisiert Batches von Instanzen immer parallel, wenn ein neues Plattformimage vom Herausgeber veröffentlicht wird.
- Ist in Anwendungsintegritätstests integriert (optional, jedoch aus Sicherheitsgründen dringend empfohlen).
- Funktioniert für alle VM-Größen.
- Funktioniert für Windows- und Linux-Plattformimages.
- Sie können automatische Upgrades jederzeit abwählen (Betriebssystemupgrades können auch manuell initiiert werden).
- Der Betriebssystemdatenträger eines virtuellen Computers wird durch den neuen, mit der neuesten Imageversion erstellten Betriebssystemdatenträger ersetzt. Konfigurierte Erweiterungen und benutzerdefinierte Datenskripts werden ausgeführt, wobei persistente Datenträger weiterhin aufbewahrt werden.


## <a name="preview-notes"></a>Hinweise zur Vorschauversion 
In der Vorschau gelten die folgenden Begrenzungen und Einschränkungen:

- Automatische Betriebssystemupgrades unterstützen nur [drei Betriebssystem-SKUs](#supported-os-images). Es gibt keine SLA oder Garantien. Es wird empfohlen, automatische Upgrades in der Vorschau nicht auf kritische Produktionsworkloads anzuwenden.
- Unterstützung für Skalierungsgruppen im Service Fabric-Cluster wird bald verfügbar sein.
- Azure-Datenträgerverschlüsselung (zurzeit in der Vorschau) wird derzeit **nicht** mit automatischen Betriebssystemupgrades für VM-Skalierungsgruppen unterstützt.
- Die Portalumgebung wird in Kürze unterstützt.


## <a name="register-to-use-automatic-os-upgrade"></a>Registrieren für die Verwendung automatischer Betriebssystemupgrades
Um das Feature für automatische Betriebssystemupgrades zu verwenden, registrieren Sie den Vorschauanbieter wie folgt mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
```

Es dauert ungefähr 10 Minuten, bis der Status als *Registriert* gemeldet wird. Sie können den aktuellen Registrierungsstatus mit [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen. Stellen Sie nach der Registrierung wie folgt mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) sicher, dass der Anbieter *Microsoft.Compute* registriert wurde:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

Es wird empfohlen, dass Ihre Anwendungen Integritätsüberprüfungen verwenden. Verwenden Sie zum Registrieren des Anbieterfeatures für Integritätstests [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) wie folgt:

```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
```

Es dauert wieder ungefähr 10 Minuten, bis der Status als *Registriert* gemeldet wird. Sie können den aktuellen Registrierungsstatus mit [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen. Stellen Sie nach der Registrierung wie folgt mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) sicher, dass der Anbieter *Microsoft.Network* registriert wurde:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```


## <a name="supported-os-images"></a>Unterstützte Betriebssystemimages
Derzeit werden nur bestimmte Betriebssystemplattform-Images unterstützt. Sie können derzeit keine benutzerdefinierten Images verwenden, die Sie selbst erstellt haben. Die *version*-Eigenschaft des Plattformimages muss auf *latest* festgelegt werden.

Derzeit werden die folgenden SKUs unterstützt (weitere werden später hinzugefügt):
    
| Herausgeber               | Angebot         |  Sku               | Version  |
|-------------------------|---------------|--------------------|----------|
| MicrosoftWindowsServer  | Windows Server | 2012-R2-Datacenter | neueste   |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter    | neueste   |
| Canonical               | UbuntuServer  | 16.04-LTS          | neueste   |


## <a name="application-health"></a>Anwendungsintegrität
Während eines Betriebssystemupgrades werden VM-Instanzen in einer Skalierungsgruppe batchweise nacheinander aktualisiert. Das Upgrade sollte nur fortgesetzt werden, wenn die Kundenanwendung auf den aktualisierten VM-Instanzen fehlerfrei ist. Es wird empfohlen, dass die Anwendung dem Upgrademodul für das Skalierungsgruppen-Betriebssystem Integritätssignale zur Verfügung stellt. Standardmäßig wertet die Plattform während Betriebssystemupgrades den VM-Energiezustand und den Status der Erweiterungsbereitstellung aus, um festzustellen, ob eine VM-Instanz nach einem Upgrade fehlerfrei ist. Während des Betriebssystemupgrades einer VM-Instanz wird ihr Betriebssystemdatenträger durch einen neuen Datenträger ersetzt, der auf der neuesten Imageversion basiert. Nachdem das Betriebssystemupgrade abgeschlossen wurde, werden die konfigurierten Erweiterungen auf diesen VMs ausgeführt. Erst wenn alle Erweiterungen auf einem virtuellen Computer erfolgreich bereitgestellt wurden, wird die Anwendung als fehlerfrei angesehen. 

Eine Skalierungsgruppe kann optional mit Anwendungsintegritätstests konfiguriert werden, um der Plattform genaue Informationen zum fortlaufenden Status der Anwendung bereitzustellen. Anwendungsintegritätstests sind benutzerdefinierte Lastenausgleichs-Prüfpunkte, die als Integritätssignal verwendet werden. Die auf einer Skalierungsgruppen-VM-Instanz ausgeführte Anwendung kann auf externe HTTP- oder TCP-Anforderungen reagieren und dadurch anzeigen, dass sie fehlerfrei ist. Weitere Informationen zur Funktionsweise von benutzerdefinierten Lastenausgleichstests finden Sie unter [Grundlegendes zu Lastenausgleichstests](../load-balancer/load-balancer-custom-probe-overview.md). Ein Anwendungsintegritätstest ist für automatische Betriebssystemupgrades nicht erforderlich, wird jedoch dringend empfohlen.

Wenn die Skalierungsgruppe für die Verwendung mehrerer Platzierungsgruppen konfiguriert ist, müssen Tests mit einem [Standardlastenausgleich](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) verwendet werden.


### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Konfigurieren eines benutzerdefinierten Lastenausgleichstests als Anwendungsintegritätstest für eine Skalierungsgruppe
Erstellen Sie als bewährte Methode einen Lastenausgleichstest explizit für die Skalierungsgruppenintegrität. Es kann derselbe Endpunkt für einen vorhandenen HTTP-Test oder einen TCP-Test verwendet werden, für einen Integritätstest ist jedoch möglicherweise ein anderes Verhalten als bei einem herkömmlichen Lastenausgleichstest erforderlich. Beispielsweise kann ein herkömmliche Lastenausgleichstest den Status „Fehlerhaft“ zurückgeben, wenn die Last der Instanz zu hoch ist, obwohl dies zum Ermitteln der Instanzintegrität bei einem automatischen Betriebssystemupgrade möglicherweise nicht angemessen ist. Konfigurieren Sie den Test so, dass eine hohe Stichprobenrate von unter zwei Minuten gilt.

Der Lastenausgleichstest kann im *networkProfile* der Skalierungsgruppe referenziert werden und wie folgt einem internen oder einem öffentlich zugänglichen Lastenausgleich zugeordnet werden:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Erzwingen einer Betriebssystemimage-Upgraderichtlinie in Ihrem Abonnement
Für sichere Upgrades wird dringend empfohlen, eine Upgraderichtlinie zu erzwingen. Diese Richtlinie kann Anwendungsintegritätstests in Ihrem Abonnement erfordern. Die folgende Azure Resource Manager-Richtlinie weist Bereitstellungen zurück, in denen keine Einstellungen für automatische Betriebssystemimage-Upgrades konfiguriert wurden:

1. Rufen Sie die integrierte Richtliniendefinition für Azure Resource Manager wie folgt mit [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) ab:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Weisen Sie die Richtlinie wie folgt mit [New-AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) einem Abonnement zu:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```


## <a name="configure-auto-updates"></a>Konfigurieren von automatischen Updates
Um automatische Upgrades zu konfigurieren, stellen Sie sicher, dass die *automaticOSUpgrade*-Eigenschaften in der Definition des Skalierungsgruppenmodells auf *TRUE* festgelegt ist. Sie können diese Eigenschaft mit Azure PowerShell oder Azure CLI 2.0 konfigurieren.

Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit Azure PowerShell (4.4.1 oder höher) konfiguriert:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```


Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der Azure-Befehlszeilenschnittstelle (2.0.20 oder höher) konfiguriert:

```azure-cli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Überprüfen des Status eines automatischen Betriebssystemupgrades
Sie können den Status des letzten für Ihre Skalierungsgruppe durchgeführten Betriebssystemupgrades mit Azure PowerShell, Azure CLI 2.0 oder der REST-API überprüfen.

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit Azure PowerShell (4.4.1 oder höher) überprüft:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der Azure-Befehlszeilenschnittstelle (2.0.20 oder höher) überprüft:

```azure-cli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST-API
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der REST-API überprüft:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

Der GET-Aufruf gibt Eigenschaften ähnlich der folgenden Beispielausgabe zurück:

```json
{
  "properties": {
    "policy": {
      "maxBatchInstancePercent": 20,
      "maxUnhealthyInstancePercent": 5,
      "maxUnhealthyUpgradedInstancePercent": 5,
      "pauseTimeBetweenBatches": "PT0S"
    },
    "runningStatus": {
      "code": "Completed",
      "startTime": "2017-06-16T03:40:14.0924763+00:00",
      "lastAction": "Start",
      "lastActionTime": "2017-06-22T08:45:43.1838042+00:00"
    },
    "progress": {
      "successfulInstanceCount": 3,
      "failedInstanceCount": 0,
      "inprogressInstanceCount": 0,
      "pendingInstanceCount": 0
    }
  },
  "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
  "location": "southcentralus"
}
```


## <a name="automatic-os-upgrade-execution"></a>Ausführung automatischer Betriebssystemupgrades
Für eine erweiterte Nutzung von Anwendungsintegritätstests führen Skalierungssatz-Betriebssystemupgrades die folgenden Schritte aus:

1. Wenn mehr als 20 % der Instanzen fehlerhaft sind, wird das Upgrade beendet, andernfalls wird fortgefahren.
2. Der nächste Batch zu aktualisierender VM-Instanzen wird identifiziert, wobei ein Batch maximal 20 % der gesamten Instanzenzahl aufweist.
3. Die Betriebssysteme des nächsten Batches von VM-Instanzen werden aktualisiert.
4. Wenn mehr als 20 % der aktualisierten Instanzen fehlerhaft sind, wird das Upgrade beendet, andernfalls wird fortgefahren.
5. Wenn der Kunde Anwendungsintegritätstests konfiguriert hat, wartet das Upgrade bis zu 5 Minuten darauf, dass die Tests fehlerfrei werden, dann wird unmittelbar mit dem nächsten Batch fortgefahren. Andernfalls wartet es 30 Minuten, bevor mit den nächsten Batch fortgefahren wird.
6. Wenn weitere zu aktualisierende Instanzen verbleiben, wird für den nächsten Batch mit Schritt 1 fortgefahren, andernfalls ist das Upgrade beendet.

Das Upgrademodul für das Skalierungsgruppen-Betriebssystem überprüft die allgemeine Integrität der VM-Instanzen, bevor die jeweiligen Batches aktualisiert werden. Beim Aktualisieren eines Batches finden eventuell andere für die gleiche Zeit geplante oder nicht geplante Wartungen in Azure-Rechenzentren statt, die die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können. Es ist daher möglich, dass vorübergehend mehr als 20 % der Instanzen nicht verfügbar sind. In solchen Fällen wird das Upgrade der Skalierungsgruppe am Ende des aktuellen Batches beendet.


## <a name="deploy-with-a-template"></a>Bereitstellen mit einer Vorlage

Sie können die folgende Vorlage zum Bereitstellen einer Skalierungsgruppe verwenden, die automatische Upgrades verwendet: <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>Automatische parallele Upgrades – Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Nächste Schritte
Weitere Beispiele für die Verwendung automatischer Betriebssystemupgrades mit Skalierungsgruppen finden Sie im [GitHub-Repository für Vorschaufeatures](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
