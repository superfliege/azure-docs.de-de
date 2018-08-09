---
title: Automatische Betriebssystemupgrades mit Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Betriebssystem automatisch auf VM-Instanzen in einer Skalierungsgruppe aktualisieren.
services: virtual-machine-scale-sets
documentationcenter: ''
author: yeki
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: yeki
ms.openlocfilehash: 6b20ef98e008d9c5d984ba29eed894b1c5ec8c09
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263247"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Automatische Betriebssystemupgrades für Azure-VM-Skalierungsgruppen

Das automatische Upgrade von Betriebssystemimages ist ein Feature der Vorschauversion von Azure VM-Skalierungsgruppen, mit der alle virtuellen Computer automatisch auf das neueste Betriebssystemimage aktualisiert werden.

Das automatische Betriebssystemupgrade weist folgende Merkmale auf:

- Nach der Konfiguration wird das neueste von den Imageherausgebern veröffentlichte Betriebssystemimage automatisch ohne Benutzereingriff auf die Skalierungsgruppe angewendet.
- Aktualisiert Batches von Instanzen immer parallel, wenn ein neues Plattformimage vom Herausgeber veröffentlicht wird.
- Ist in Anwendungsintegritätstests integriert (optional, jedoch aus Sicherheitsgründen dringend empfohlen).
- Funktioniert für alle VM-Größen.
- Funktioniert für Windows- und Linux-Plattformimages.
- Sie können automatische Updates jederzeit abwählen (Betriebssystemupdates können auch manuell gestartet werden).
- Der Betriebssystemdatenträger eines virtuellen Computers wird durch den neuen, mit der neuesten Imageversion erstellten Betriebssystemdatenträger ersetzt. Konfigurierte Erweiterungen und benutzerdefinierte Datenskripts werden ausgeführt, wobei persistente Datenträger weiterhin aufbewahrt werden.


## <a name="preview-notes"></a>Hinweise zur Vorschauversion 
In der Vorschau gelten die folgenden Begrenzungen und Einschränkungen:

- Automatische Betriebssystemupgrades unterstützen nur [vier Betriebssystem-SKUs](#supported-os-images). Es gibt keine SLA oder Garantien. Wenden Sie automatische Upgrades in der Vorschau möglichst nicht auf kritische Produktionsworkloads an.
- Die Azure-Datenträgerverschlüsselung wird derzeit **nicht** mit automatischen Betriebssystemupgrades für VM-Skalierungsgruppen unterstützt.


## <a name="register-to-use-automatic-os-upgrade"></a>Registrieren für die Verwendung automatischer Betriebssystemupgrades
Um das Feature für automatische Betriebssystemupgrades zu verwenden, registrieren Sie den Vorschauanbieter mit Azure Powershell oder Azure CLI 2.0.

### <a name="powershell"></a>PowerShell

1. Registrieren mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

     ```powershell
     Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
     ```

2. Es dauert ungefähr 10 Minuten, bis der Status als *Registriert* gemeldet wird. Sie können den aktuellen Registrierungsstatus mit [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen. 

3. Überprüfen Sie nach der Registrierung, ob der *Microsoft.Compute*-Anbieter registriert ist. Im folgenden Beispiel wird Azure Powershell mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) verwendet:

     ```powershell
     Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
     ```


### <a name="cli-20"></a>CLI 2.0

1. Registrieren mit [az feature register](/cli/azure/feature#az-feature-register):

     ```azurecli
     az feature register --name AutoOSUpgradePreview --namespace Microsoft.Compute
     ```

2. Es dauert ungefähr 10 Minuten, bis der Status als *Registriert* gemeldet wird. Sie können den aktuellen Registrierungsstatus mit [az feature-show](/cli/azure/feature#az-feature-show) überprüfen. 
 
3. Stellen Sie nach der Registrierung sicher, dass der Anbieter *Microsoft.Compute* registriert ist. Im folgenden Beispiel wird Azure CLI (2.0.20 oder höher) mit [az provider register](/cli/azure/provider#az-provider-register) verwendet:

     ```azurecli
     az provider register --namespace Microsoft.Compute
     ```

> [!NOTE]
> In Service Fabric-Clustern gilt ein eigenes Konzept der Anwendungsintegrität, aber Skalierungsgruppen ohne Service Fabric verwenden zum Überwachen der Anwendungsintegrität den Lastenausgleichs-Integritätstest. 
>
> ### <a name="azure-powershell"></a>Azure Powershell
>
> 1. Registrieren des Anbieterfeatures für Integritätstests mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):
>
>      ```powershell
>      Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
>      ```
>
> 2. Es dauert wieder ungefähr 10 Minuten, bis der Status als *Registriert* gemeldet wird. Sie können den aktuellen Registrierungsstatus mit [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen.
>
> 3. Stellen Sie nach der Registrierung mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) sicher, dass der Anbieter *Microsoft.Network* registriert wurde:
>
>      ```powershell
>      Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
>      ```
>
>
> ### <a name="cli-20"></a>CLI 2.0
>
> 1. Registrieren des Anbieterfeatures für Integritätstests mit [az feature register](/cli/azure/feature#az-feature-register):
>
>      ```azurecli
>      az feature register --name AllowVmssHealthProbe --namespace Microsoft.Network
>      ```
>
> 2. Es dauert wieder ungefähr 10 Minuten, bis der Status als *Registriert* gemeldet wird. Sie können den aktuellen Registrierungsstatus mit [az feature-show](/cli/azure/feature#az-feature-show) überprüfen. 
>
> 3. Stellen Sie nach der Registrierung sicher, dass der Anbieter *Microsoft.Network* wie folgt mit [az provider register](/cli/azure/provider#az-provider-register) registriert wurde:
>
>      ```azurecli
>      az provider register --namespace Microsoft.Network
>      ```

## <a name="portal-experience"></a>Portalfunktion
Nachdem Sie die oben genannten Registrierungsschritte ausgeführt haben, können Sie zum [Azure-Portal](https://aka.ms/managed-compute) navigieren, um automatische Betriebssystemupgrades für Ihre Skalierungsgruppen zu aktivieren und den Upgradefortschritt anzuzeigen:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Unterstützte Betriebssystemimages
Derzeit werden nur bestimmte Betriebssystemplattform-Images unterstützt. Sie können derzeit keine benutzerdefinierten Images verwenden, die Sie selbst erstellt haben. Die *version*-Eigenschaft des Plattformimages muss auf *latest* festgelegt werden.

Derzeit werden die folgenden SKUs unterstützt (weitere werden später hinzugefügt):
    
| Herausgeber               | Angebot         |  Sku               | Version  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04-LTS          | neueste   |
| MicrosoftWindowsServer  | Windows Server | 2012-R2-Datacenter | neueste   |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter    | neueste   |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter-Smalldisk | neueste   |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter-with-Containers | neueste   |



## <a name="application-health-without-service-fabric"></a>Anwendungsintegrität ohne Service Fabric
> [!NOTE]
> Dieser Abschnitt gilt nur für Skalierungsgruppen ohne Service Fabric. In Service Fabric gilt ein eigenes Konzept der Anwendungsintegrität. Wenn Sie automatische Betriebssystemupgrades mit Service Fabric verwenden, wird das neue Betriebssystemimage nacheinander in einzelnen Updatedomänen bereitgestellt. So wird gewährleistet, dass die in Service Fabric ausgeführten Dienste hochverfügbar bleiben. Weitere Informationen zu den Dauerhaftigkeitsmerkmalen von Service Fabric-Clustern finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

Während eines Betriebssystemupgrades werden VM-Instanzen in einer Skalierungsgruppe batchweise nacheinander aktualisiert. Das Upgrade sollte nur fortgesetzt werden, wenn die Kundenanwendung auf den aktualisierten VM-Instanzen fehlerfrei ist. Aus diesem Grund muss die Anwendung der Upgrade-Engine für das Skalierungsgruppen-Betriebssystem Integritätssignale zur Verfügung stellen. Während Betriebssystemupgrades wertet die Plattform den VM-Energiezustand und den Status der Erweiterungsbereitstellung aus, um festzustellen, ob eine VM-Instanz nach einem Upgrade fehlerfrei ist. Während des Betriebssystemupgrades einer VM-Instanz wird ihr Betriebssystemdatenträger durch einen neuen Datenträger ersetzt, der auf der neuesten Imageversion basiert. Nachdem das Betriebssystemupgrade abgeschlossen wurde, werden die konfigurierten Erweiterungen auf diesen VMs ausgeführt. Erst wenn alle Erweiterungen auf einem virtuellen Computer erfolgreich bereitgestellt wurden, wird die Anwendung als fehlerfrei angesehen. 

Darüber hinaus *muss* die Skalierungsgruppe mit Anwendungsintegritätstests konfiguriert werden, um der Plattform genaue Informationen zum fortlaufenden Status der Anwendung bereitzustellen. Anwendungsintegritätstests sind benutzerdefinierte Lastenausgleichs-Prüfpunkte, die als Integritätssignal verwendet werden. Die auf einer Skalierungsgruppen-VM-Instanz ausgeführte Anwendung kann auf externe HTTP- oder TCP-Anforderungen reagieren und dadurch anzeigen, dass sie fehlerfrei ist. Weitere Informationen zur Funktionsweise von benutzerdefinierten Lastenausgleichstests finden Sie unter [Grundlegendes zu Lastenausgleichstests](../load-balancer/load-balancer-custom-probe-overview.md).

Wenn die Skalierungsgruppe für die Verwendung mehrerer Platzierungsgruppen konfiguriert ist, müssen Tests mit einem [Standardlastenausgleich](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) verwendet werden.

### <a name="important-keep-credentials-up-to-date"></a>Wichtig: Ihre Anmeldeinformationen müssen aktuell sein
Wenn Ihre Skalierungsgruppe Anmeldeinformationen zum Zugreifen auf externe Ressourcen verwendet, müssen Sie sicherstellen, dass die Anmeldeinformationen immer auf dem neuesten Stand sind. Beispielsweise kann eine VM-Erweiterung konfiguriert werden, um ein SAS-Token für das Speicherkonto zu verwenden. Wenn die verwendeten Anmeldeinformationen (Zertifikate und Token eingeschlossen) abgelaufen sind, kommt es beim Upgrade zu einem Fehler, und der erste VM-Batch wechselt in einen fehlerhaften Zustand.

Führen Sie die folgenden Schritte aus, um nach einem Fehler bei der Ressourcenauthentifizierung die VMs wiederherzustellen und automatische Betriebssystemupgrades erneut zu aktivieren:

* Generieren Sie das Token (oder andere Anmeldeinformationen) neu, die an Ihre Erweiterungen übergeben werden.
* Stellen Sie sicher, dass in der VM verwendete Anmeldeinformationen zur Kommunikation mit externen Entitäten aktuell sind.
* Aktualisieren Sie Erweiterungen im Skalierungsgruppenmodell mit den neuen Token.
* Stellen Sie die aktualisierte Skalierungsgruppe bereit, um alle VM-Instanzen – einschließlich der fehlerhaften – zu aktualisieren. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Konfigurieren eines benutzerdefinierten Lastenausgleichstests als Anwendungsintegritätstest für eine Skalierungsgruppe
Sie *müssen* einen Lastenausgleichstest explizit für die Skalierungsgruppenintegrität erstellen. Sie können denselben Endpunkt für einen vorhandenen HTTP-Test oder TCP-Test verwenden. Für einen Integritätstest ist jedoch möglicherweise ein anderes Verhalten als bei einem herkömmlichen Lastenausgleichstest erforderlich. Beispielsweise kann ein herkömmlicher Lastenausgleichstest den Status „Fehlerhaft“ zurückgeben, wenn die Last der Instanz zu hoch ist. Zum Ermitteln der Instanzintegrität bei einem automatischen Betriebssystemupgrade ist dies hingegen möglicherweise nicht angemessen. Konfigurieren Sie den Test so, dass eine hohe Stichprobenrate von unter zwei Minuten gilt.

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

### <a name="powershell"></a>PowerShell
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

### <a name="cli-20"></a>CLI 2.0
Zuweisen von Richtlinien zu einem Abonnement mit integrierter Azure Resource Manager-Richtlinie:

```azurecli
az policy assignment create --display-name "Enforce automatic OS upgrades with app health checks" --name "Enforce automatic OS upgrades" --policy 465f0161-0087-490a-9ad9-ad6217f4f43a --scope "/subscriptions/<SubscriptionId>"
```

## <a name="configure-auto-updates"></a>Konfigurieren von automatischen Updates
Um automatische Upgrades zu konfigurieren, stellen Sie sicher, dass die *automaticOSUpgrade*-Eigenschaften in der Definition des Skalierungsgruppenmodells auf *TRUE* festgelegt ist. Sie können diese Eigenschaft mit Azure PowerShell oder Azure CLI 2.0 konfigurieren.

### <a name="powershell"></a>PowerShell
Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit Azure PowerShell (4.4.1 oder höher) konfiguriert:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```

### <a name="cli-20"></a>CLI 2.0
Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der Azure-Befehlszeilenschnittstelle (2.0.20 oder höher) konfiguriert:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Überprüfen des Status eines automatischen Betriebssystemupgrades
Sie können den Status des letzten für Ihre Skalierungsgruppe durchgeführten Betriebssystemupgrades mit Azure PowerShell, Azure CLI 2.0 oder der REST-API überprüfen.

### <a name="powershell"></a>PowerShell
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit Azure PowerShell (4.4.1 oder höher) überprüft:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="cli-20"></a>CLI 2.0
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der Azure-Befehlszeilenschnittstelle (2.0.20 oder höher) überprüft:

```azurecli
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

1. Beenden Sie das Upgrade, wenn mehr als 20 % der Instanzen fehlerhaft sind. Fahren Sie andernfalls fort.
2. Der nächste Batch zu aktualisierender VM-Instanzen wird identifiziert, wobei ein Batch maximal 20 % der gesamten Instanzenzahl aufweist.
3. Die Betriebssysteme des nächsten Batches von VM-Instanzen werden aktualisiert.
4. Beenden Sie das Upgrade, wenn mehr als 20 % der aktualisierten Instanzen fehlerhaft sind. Fahren Sie andernfalls fort.
5. Bei Skalierungsgruppen, die nicht zu einem Service Fabric-Cluster gehören, wartet das Upgrade bis zu 5 Minuten darauf, dass Tests fehlerfrei werden und fährt dann sofort mit dem nächsten Batch fort. Bei Skalierungsgruppen, die Bestandteil eines Service Fabric-Clusters sind, wartet die Skalierungsgruppe 30 Minuten lang, bevor sie mit dem nächsten Batch fortfährt.
6. Wenn weitere zu aktualisierende Instanzen verbleiben, wird für den nächsten Batch mit Schritt 1 fortgefahren, andernfalls ist das Upgrade beendet.

Die Upgrade-Engine für das Skalierungsgruppen-Betriebssystem überprüft die allgemeine Integrität der VM-Instanzen, bevor die jeweiligen Batches aktualisiert werden. Beim Aktualisieren eines Batches finden eventuell andere für die gleiche Zeit geplante oder nicht geplante Wartungen in Azure-Rechenzentren statt, die die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können. Daher ist es möglich, dass mehr als 20 % der Instanzen nicht verfügbar sind. In solchen Fällen wird das Upgrade der Skalierungsgruppe am Ende des aktuellen Batches beendet.


## <a name="deploy-with-a-template"></a>Bereitstellen mit einer Vorlage

Sie können die folgende Vorlage zum Bereitstellen einer Skalierungsgruppe verwenden, die automatische Upgrades verwendet: <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>Automatische parallele Upgrades – Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Nächste Schritte
Weitere Beispiele für die Verwendung automatischer Betriebssystemupgrades mit Skalierungsgruppen finden Sie im [GitHub-Repository für Vorschaufeatures](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
