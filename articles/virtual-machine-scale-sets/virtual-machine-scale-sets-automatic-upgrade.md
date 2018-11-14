---
title: Automatische Betriebssystemimageupgrades mit Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Betriebssystemimage auf VM-Instanzen in einer Skalierungsgruppe automatisch aktualisieren.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: rajraj
ms.openlocfilehash: c8ba9ac3150b5a84b2902afaaefcf78c76764fed
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036189"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Automatische Betriebssystemimageupgrades mit Azure-VM-Skalierungsgruppen

Das automatische Upgrade von Betriebssystemimages ist ein Feature von Azure-VM-Skalierungsgruppen, mit dem alle virtuellen Computer automatisch auf das neueste Betriebssystemimage aktualisiert werden.

Das automatische Betriebssystemupgrade weist folgende Merkmale auf:

- Nach der Konfiguration wird das neueste von den Imageherausgebern veröffentlichte Betriebssystemimage automatisch ohne Benutzereingriff auf die Skalierungsgruppe angewendet.
- Aktualisiert Batches von Instanzen immer parallel, wenn ein neues Plattformimage vom Herausgeber veröffentlicht wird.
- Integriert Anwendungsintegritätstests.
- Unterstützt alle VM-Größen und sowohl Windows- als auch Linux-Plattformimages.
- Sie können automatische Upgrades jederzeit abwählen (Betriebssystemupgrades können auch manuell initiiert werden).
- Der Betriebssystemdatenträger eines virtuellen Computers wird durch den neuen, mit der neuesten Imageversion erstellten Betriebssystemdatenträger ersetzt. Konfigurierte Erweiterungen und benutzerdefinierte Datenskripts werden ausgeführt, wobei persistente Datenträger weiterhin aufbewahrt werden.
- Azure Disk Encryption (in der Vorschauversion) wird derzeit nicht unterstützt.  

## <a name="how-does-automatic-os-image-upgrade-work"></a>Wie funktioniert das automatische Upgrade von Betriebssystemimages?

Bei einem Upgrade wird der Betriebssystemdatenträger eines virtuellen Computers durch einen neuen ersetzt, der mit der neuesten Imageversion erstellt wird. Alle konfigurierten Erweiterungen und benutzerdefinierten Datenskripts werden ausgeführt, wobei persistente Datenträger weiterhin aufbewahrt werden. Um die Anwendungsausfallzeiten zu minimieren, erfolgen Upgrades in Batches von Computern, wobei jeweils nicht mehr als 20 % der Skalierungsgruppe aktualisiert wird. Sie haben auch die Möglichkeit, einen Azure Load Balancer-Anwendungsintegritätstest zu integrieren. Es wird dringend empfohlen, einen Anwendungstakt zu integrieren und den Erfolg des Upgrades jedes Batches im Upgradeprozess zu überprüfen. Die Ausführungsschritte sehen wie folgt aus: 

1. Bevor mit dem Upgradeprozess begonnen wird, stellt der Orchestrator sicher, dass nicht mehr als 20 % der Instanzen fehlerhaft sind. 
2. Der Batch zu aktualisierender VM-Instanzen wird identifiziert, wobei ein Batch maximal 20 % der gesamten Instanzenzahl aufweist.
3. Das Betriebssystemimage dieses Batches von VM-Instanzen wird aktualisiert.
4. Wenn der Kunde Anwendungsintegritätstests konfiguriert hat, wartet das Upgrade bis zu 5 Minuten darauf, dass die Tests fehlerfrei werden, bevor mit der Aktualisierung des nächsten Batches fortgefahren wird. 
5. Wenn weitere zu aktualisierende Instanzen verbleiben, wird für den nächsten Batch mit Schritt 1 fortgefahren, andernfalls ist das Upgrade beendet.

Der Upgradeorchestrator für das Skalierungsgruppen-Betriebssystem überprüft die allgemeine Integrität der VM-Instanzen, bevor die einzelnen Batches aktualisiert werden. Beim Aktualisieren eines Batches finden eventuell andere für die gleiche Zeit geplante oder nicht geplante Wartungen in Azure-Rechenzentren statt, die die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können. Es ist daher möglich, dass vorübergehend mehr als 20 % der Instanzen nicht verfügbar sind. In solchen Fällen wird das Upgrade der Skalierungsgruppe am Ende des aktuellen Batches beendet.

## <a name="supported-os-images"></a>Unterstützte Betriebssystemimages
Derzeit werden nur bestimmte Betriebssystemplattform-Images unterstützt. Sie können derzeit keine benutzerdefinierten Images verwenden, die Sie selbst erstellt haben. 

Derzeit werden die folgenden SKUs unterstützt (weitere werden künftig hinzugefügt):
    
| Herausgeber               | Betriebssystemangebot      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS *        | 
| Rogue Wave (OpenLogic)  | CentOS        | 7.5 *              | 
| CoreOS                  | CoreOS        | Stable             | 
| Microsoft Corporation   | Windows Server | 2012-R2-Datacenter | 
| Microsoft Corporation   | Windows Server | 2016-Datacenter    | 
| Microsoft Corporation   | Windows Server | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | Windows Server | 2016-Datacenter-with-Containers |

* Unterstützung für diese Images wird derzeit eingeführt und steht in Kürze in allen Azure-Regionen zur Verfügung. 

## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Anforderungen für das Konfigurieren des automatischen Upgrades von Betriebssystemimages

- Die *version*-Eigenschaft des Plattformimages muss auf *latest* festgelegt werden.
- Verwenden Sie Anwendungsintegritätstests für Nicht-Service Fabric-Skalierungsgruppen.
- Stellen Sie sicher, dass die Ressourcen, auf die das Skalierungsgruppenmodell verweist, verfügbar und auf dem neuesten Stand sind. 
  Exa.SAS-URI für die Bootstrap-Nutzlast in VM-Erweiterungseigenschaften, Nutzlast im Speicherkonto, Verweis auf Geheimnisse im Modell. 

## <a name="configure-automatic-os-image-upgrade"></a>Konfigurieren des automatischen Upgrades von Betriebssystemimages
Stellen Sie zum Konfigurieren des automatischen Upgrades von Betriebssystemimages sicher, dass die Eigenschaft *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* in der Definition des Skalierungsgruppenmodells auf *true* festgelegt ist. 

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2018-10-01`
```

```json
{ 
  "properties": { 
    "upgradePolicy": { 
      "automaticOSUpgradePolicy": { 
        "enableAutomaticOSUpgrade":  true 
      } 
    } 
  } 
} 
```

Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der Azure-Befehlszeilenschnittstelle (Azure CLI 2.0.47 oder höher) konfiguriert:

```azurecli
az vmss update --name myVMSS --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```
Die Unterstützung für das Konfigurieren dieser Eigenschaft mithilfe von Azure PowerShell wird in Kürze eingeführt.

## <a name="using-application-health-probes"></a>Verwenden von Anwendungsintegritätstests 

Während eines Betriebssystemupgrades werden VM-Instanzen in einer Skalierungsgruppe batchweise nacheinander aktualisiert. Das Upgrade sollte nur fortgesetzt werden, wenn die Kundenanwendung auf den aktualisierten VM-Instanzen fehlerfrei ist. Es wird empfohlen, dass die Anwendung der Upgrade-Engine für das Skalierungsgruppen-Betriebssystem Integritätssignale zur Verfügung stellt. Standardmäßig wertet die Plattform während Betriebssystemupgrades den VM-Energiezustand und den Status der Erweiterungsbereitstellung aus, um festzustellen, ob eine VM-Instanz nach einem Upgrade fehlerfrei ist. Während des Betriebssystemupgrades einer VM-Instanz wird ihr Betriebssystemdatenträger durch einen neuen Datenträger ersetzt, der auf der neuesten Imageversion basiert. Nachdem das Betriebssystemupgrade abgeschlossen wurde, werden die konfigurierten Erweiterungen auf diesen VMs ausgeführt. Erst wenn alle Erweiterungen auf einem virtuellen Computer erfolgreich bereitgestellt wurden, wird die Anwendung als fehlerfrei angesehen. 

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
> [!NOTE]
> Wenn Sie automatische Betriebssystemupgrades mit Service Fabric verwenden, wird das neue Betriebssystemimage nacheinander in einzelnen Updatedomänen bereitgestellt. So wird gewährleistet, dass die in Service Fabric ausgeführten Dienste hochverfügbar bleiben. Um automatische Betriebssystemupgrades in Service Fabric nutzen zu können, muss Ihr Cluster zur Verwendung der Dauerhaftigkeitsstufe „Silber“ oder höher konfiguriert sein. Weitere Informationen zu den Dauerhaftigkeitsmerkmalen von Service Fabric-Clustern finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Die Anmeldeinformationen müssen aktuell sein
Wenn Ihre Skalierungsgruppe Anmeldeinformationen zum Zugreifen auf externe Ressourcen verwendet – wenn z. B. eine VM-Erweiterung konfiguriert wurde, die ein SAS-Token für das Speicherkonto verwendet –, müssen Sie sicherstellen, dass die Anmeldeinformationen aktuell sind. Wenn die verwendeten Anmeldeinformationen (Zertifikate und Token eingeschlossen) abgelaufen sind, kommt es beim Upgrade zu einem Fehler, und der erste VM-Batch wechselt in einen fehlerhaften Zustand.

Um nach einem Fehler bei der Ressourcenauthentifizierung die VMs wiederherzustellen und automatische Betriebssystemupgrades erneut zu aktivieren, führen Sie die folgenden Schritte aus:

* Generieren Sie das Token (oder andere Anmeldeinformationen) neu, die an Ihre Erweiterungen übergeben werden.
* Stellen Sie sicher, dass auf dem virtuellen Computer verwendete Anmeldeinformationen für die Kommunikation mit externen Entitäten aktuell sind.
* Aktualisieren Sie Erweiterungen im Skalierungsgruppenmodell mit den neuen Token.
* Stellen Sie die aktualisierte Skalierungsgruppe bereit, um alle VM-Instanzen – einschließlich der fehlerhaften – zu aktualisieren. 

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Abrufen des Verlaufs automatischer Betriebssystemimageupgrades 
Sie können den Verlauf des letzten für Ihre Skalierungsgruppe durchgeführten Betriebssystemupgrades mit Azure PowerShell, Azure CLI 2.0 oder der REST-API überprüfen. Sie können den Verlauf für die letzten fünf Betriebssystemupgradeversuche innerhalb der vergangenen zwei Monate abrufen.

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit Azure PowerShell überprüft:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der Azure-Befehlszeilenschnittstelle (Azure CLI 2.0.47 oder höher) überprüft:

```azurecli
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST-API
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myVMSS* in der Ressourcengruppe *myResourceGroup* mit der REST-API überprüft:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```
Die Dokumentation für diese API finden Sie unter https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getosupgradehistory.

Der GET-Aufruf gibt Eigenschaften ähnlich der folgenden Beispielausgabe zurück:

```json
{
  "value": [
    {
      "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
} 
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Wie erhalte ich die neueste Version eines Plattformimages? 

Sie können die Imageversionen für automatische Betriebssystemupgrades von unterstützten SKUs mithilfe der folgenden Beispiele erhalten: 

```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2018-10-01`
```

```powershell
Get-AzureRmVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

```azurecli
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="deploy-with-a-template"></a>Bereitstellen mit einer Vorlage

Sie können die folgende Vorlage zum Bereitstellen einer Skalierungsgruppe verwenden, die automatische Upgrades verwendet: <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>Automatische parallele Upgrades – Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Nächste Schritte
Weitere Beispiele für die Verwendung automatischer Betriebssystemupgrades mit Skalierungsgruppen finden Sie im [GitHub-Repository für Vorschaufeatures](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
