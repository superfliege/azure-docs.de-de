---
title: Senden von Gastbetriebssystemmetriken an den Metrikspeicher von Azure Monitor unter Verwendung einer Resource Manager-Vorlage für eine VM-Skalierungsgruppe von Windows
description: Senden von Gastbetriebssystemmetriken an den Metrikspeicher von Azure Monitor unter Verwendung einer Resource Manager-Vorlage für eine VM-Skalierungsgruppe von Windows
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: b9808233e08e545c31e171afe104173dccc6abed
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434924"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Senden von Gastbetriebssystemmetriken an den Metrikspeicher von Azure Monitor unter Verwendung einer Resource Manager-Vorlage für eine VM-Skalierungsgruppe von Windows

Die [Windows Azure-Diagnoseerweiterung](azure-diagnostics.md) (WAD) von Azure Monitor ermöglicht es Ihnen, Metriken und Protokolle vom Gastbetriebssystem zu erfassen, das als Teil eines virtuellen Computers, eines Clouddiensts oder eines Service Fabric-Clusters ausgeführt wird.  Die Erweiterung kann Telemetriedaten an viele verschiedene Standorte senden, die im zuvor verlinkten Artikel aufgeführt sind.  

In diesem Artikel wird der Prozess zum Senden von Leistungsmetriken des Gastbetriebssystems für eine VM-Skalierungsgruppe von Windows an den Azure Monitor-Datenspeicher beschrieben. Ab WAD Version 1.11 können Sie Metriken direkt in den Azure Monitor-Metrikspeicher schreiben, in dem bereits Metriken der Standardplattformen gesammelt werden. Wenn Sie sie an dieser Position speichern, können Sie auf die gleichen Aktionen zugreifen, die auch für Plattformmetriken verfügbar sind.  Zu den Maßnahmen gehören zeitnahe Benachrichtigung, Diagrammerstellung, Routing, Zugriff über die REST-API und vieles mehr.  In der Vergangenheit hat die WAD-Erweiterung in Azure Storage geschrieben, aber nicht der Azure Monitor-Datenspeicher.  

Wenn Sie noch nicht mit Resource Manager-Vorlagen vertraut sind, erfahren Sie mehr über [Vorlagenbereitstellungen](../azure-resource-manager/resource-group-overview.md) sowie deren Struktur und Syntax.  

## <a name="pre-requisites"></a>Voraussetzungen

- Ihr Abonnement muss mit [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) registriert werden. 

- Sie müssen entweder [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) installiert haben, oder Sie können [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) verwenden. 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Einrichten von Azure Monitor als Datensenke 
Die Azure-Diagnoseerweiterung verwendet eine Funktion namens „Datensenken“, um Metriken und Protokolle an verschiedene Standorte zu leiten.  Die folgenden Schritte zeigen, wie Sie eine Resource Manager-Vorlage und PowerShell verwenden, um eine VM mit der neuen Datensenke „Azure Monitor“ bereitzustellen. 

## <a name="author-resource-manager-template"></a>Erstellen von Resource Manager-Vorlagen 
Für dieses Beispiel können Sie eine öffentlich zugängliche Beispielvorlage verwenden. Die Startvorlagen befinden sich unter https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale.  

- **Azuredeploy.json** ist eine vorkonfigurierte Resource Manager-Vorlage für die Bereitstellung einer VM-Skalierungsgruppe.

- **Azuredeploy.parameters.json** ist eine Parameterdatei, die Informationen wie Benutzername und Kennwort speichert, die Sie für Ihre VM festlegen. Während der Bereitstellung verwendet die Resource Manager-Vorlage die in dieser Datei festgelegten Parameter. 

Laden Sie beide Dateien herunter und speichern Sie sie lokal. 

###  <a name="modify-azuredeployparametersjson"></a>Ändern von azuredeploy.parameters.json
Öffnen Sie die Datei *azuredeploy.parameters.json*. 

- Stellen Sie eine **vmSKU** bereit, die Sie implementieren möchten (Empfehlung: Standard_D2_v3). 
- Geben Sie eine **windowsOSVersion** an, die Sie für Ihre VM-Skalierungsgruppe verwenden möchten (Empfehlung: 2016-Datacenter). 
- Benennen Sie die bereitzustellende VM-Skalierungsgruppe unter Verwendung einer **vmssName**-Eigenschaft. Beispiel: *VMSS-WAD-TEST*.    
- Geben Sie die Anzahl der VMs an, die Sie für die VM-Skalierungsgruppe ausführen möchten, die über die Eigenschaft **instanceCount** festgelegt wurde.
- Geben Sie Werte für **adminUsername** und **adminPassword** für die VM-Skalierungsgruppe ein. Diese Parameter werden für den Remotezugriff auf die VMs in der Skalierungsgruppe verwendet. Diese Parameter werden für den Remotezugriff auf die VMs verwendet. Verwenden Sie NICHT die in dieser Vorlage enthaltenen Parameter, um zu vermeiden, dass Ihre VM das Opfer eines Hijacking-Vorgangs wird. Bots durchsuchen das Internet nach Benutzernamen und Kennwörtern in öffentlichen Github-Repositorys. Es ist wahrscheinlich, dass sie VMs mit diesen Standardwerten testen. 


###  <a name="modify-azuredeployjson"></a>Ändern von azuredeploy.json
Öffnen Sie die Datei *azuredeploy.json*. 

Fügen Sie eine Variable hinzu, um die Informationen zum Speicherkonto in der Resource Manager-Vorlage zu speichern. Sie müssen im Rahmen der Installation der Diagnoseerweiterung weiterhin ein Speicherkonto angeben. Alle Protokolle und/oder Leistungsindikatoren, die in der Diagnosekonfigurationsdatei angegeben sind, werden in das angegebene Speicherkonto geschrieben und zusätzlich an den Metrikspeicher von Azure Monitor gesendet. 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
 ```
 
Suchen Sie die Definition der VM-Skalierungsgruppe im Ressourcenbereich und fügen Sie der Konfiguration den Abschnitt „identity“ hinzu. Dadurch wird sichergestellt, dass Azure eine Systemidentität zuweist. Dieser Schritt stellt sicher, dass die VMs in der Skalierungsgruppe Gastmetriken über sich selbst an Azure Monitor senden können.  

```json
  { 
      "type": "Microsoft.Compute/virtualMachineScaleSets", 
      "name": "[variables('namingInfix')]", 
      "location": "[resourceGroup().location]", 
      "apiVersion": "2017-03-30", 
      //add these lines below
      "identity": { 
           "type": "systemAssigned" 
       }, 
       //end of lines to add
 ```

Suchen Sie in der VM-Skalierungsgruppenressource den Abschnitt **virtualMachineProfile**. Fügen Sie ein neues Profil namens **extensionsProfile** hinzu, um Erweiterungen zu verwalten.  


Fügen Sie in **extensionProfile** eine neue Erweiterung zur Vorlage hinzu, wie im Abschnitt zur **VMSS-WAD-Erweiterung** veranschaulicht.  Dieser Abschnitt ist die MSI-Erweiterung (verwaltete Dienstidentität), die sicherstellt, dass die ausgegebenen Metriken von Azure Monitor akzeptiert werden. Das Feld **Name** kann einen beliebigen Namen enthalten. 

Mit dem unten angegebenen Code aus der MSI-Erweiterung wird die Diagnoseerweiterung und -konfiguration der VM-Skalierungsgruppenressource auch als Erweiterungsressource hinzugefügt. Sie können Leistungsindikatoren nach Bedarf hinzufügen/entfernen. 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed service identity   
                { 
                 "name": "VMSS-WAD-extension", 
                 "properties": { 
                       "publisher": "Microsoft.ManagedIdentity", 
                       "type": "ManagedIdentityExtensionForWindows", 
                       "typeHandlerVersion": "1.0", 
                       "autoUpgradeMinorVersion": true, 
                       "settings": { 
                             "port": 50342 
                           }, 
                       "protectedSettings": {} 
                     } 
                                
            }, 
            // add diagnostic extension. (Remove this comment after pasting.)
            { 
              "name": "[concat('VMDiagnosticsVmExt','_vmNodeType0Name')]", 
              "properties": { 
                   "type": "IaaSDiagnostics", 
                   "autoUpgradeMinorVersion": true, 
                   "protectedSettings": { 
                        "storageAccountName": "[variables('storageAccountName')]", 
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')),'2015-05-01-preview').key1]", 
                        "storageAccountEndPoint": "https://core.windows.net/" 
                   }, 
                   "publisher": "Microsoft.Azure.Diagnostics", 
                   "settings": { 
                        "WadCfg": { 
                              "DiagnosticMonitorConfiguration": { 
                                   "overallQuotaInMB": "50000", 
                                   "PerformanceCounters": { 
                                       "scheduledTransferPeriod": "PT1M", 
                                       "sinks": "AzMonSink", 
                                       "PerformanceCounterConfiguration": [ 
                                          { 
              "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                                              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Available Bytes", 
              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Committed Bytes", 
              "sampleRate": "PT15S" 
           } 
                                       ] 
                                 }, 
                                 "EtwProviders": { 
                                       "EtwEventSourceProviderConfiguration": [ 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Actors", 
                                              "scheduledTransferKeywordFilter": "1", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                              "eventDestination": "ServiceFabricReliableActorEventTable" 
                                           } 
                                           }, 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Services", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricReliableServiceEventTable" 
                                              } 
                                           } 
                                     ], 
                                     "EtwManifestProviderConfiguration": [ 
                                           { 
                                              "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8", 
                                              "scheduledTransferLogLevelFilter": "Information", 
                                              "scheduledTransferKeywordFilter": "4611686018427387904", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricSystemEventTable" 
                                              } 
                                          } 
                                     ] 
                               } 
                               }, 
                               "SinksConfig": { 
                                     "Sink": [ 
                                          { 
                                              "name": "AzMonSink", 
                                              "AzureMonitor": {} 
                                          } 
                                      ] 
                               } 
                         }, 
                         "StorageAccount": "[variables('storageAccountName')]" 
                         }, 
                        "typeHandlerVersion": "1.11" 
                  } 
           } 
            ] 
          }
          }
      }
    },
    //end of added code plus a few brackets. Be sure that the number and type of brackets match properly when done. 
    {
      "type": "Microsoft.Insights/autoscaleSettings",
...
```


Fügen Sie ein „dependsOn“ für das Speicherkonto hinzu, um sicherzustellen, dass es in der richtigen Reihenfolge erstellt wird. 
```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
 ```

Erstellen Sie ein Speicherkonto, wenn dieses nicht bereits in der Vorlage erstellt wurde.  
```json
"resources": [
  // add this code    
  {
     "type": "Microsoft.Storage/storageAccounts",
     "name": "[variables('storageAccountName')]",
     "apiVersion": "2015-05-01-preview",
     "location": "[resourceGroup().location]",
     "properties": {
       "accountType": "Standard_LRS"
      }
  },
  // end added code
  { 
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
```

Speichern und schließen Sie beide Dateien. 

## <a name="deploy-the-resource-manager-template"></a>Bereitstellen der Resource Manager-Vorlage 

> [!NOTE]
> Sie müssen die Azure-Diagnoseerweiterung Version 1.5 oder höher ausführen UND für die Eigenschaft „autoUpgradeMinorVersion“ den Wert *true* in Ihrer Resource Manager-Vorlage festgelegt haben.  Azure lädt dann die richtige Erweiterung, wenn die VM gestartet wird. Wenn diese Einstellungen in Ihrer Vorlage nicht vorhanden sind, ändern Sie sie und stellen Sie die Vorlage erneut bereit. 


Für die Bereitstellung der Resource Manager-Vorlage wird hier Azure PowerShell verwendet.  

1. Starten von PowerShell 
1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an.
1. Rufen Sie Ihre Liste der Abonnements mit `Get-AzureRmSubscription` ab.
1. Legen Sie das Abonnement fest, in dem Sie den virtuellen Computer erstellen/aktualisieren möchten. 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Erstellen Sie eine neue Ressourcengruppe für die bereitzustellende VM und führen Sie den folgenden Befehl aus. 

   ```PowerShell
    New-AzureRmResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Denken Sie daran, eine Azure-Region zu verwenden, die für benutzerdefinierte Metriken aktiviert ist. Hier eine Liste mit Ressourcen: 
 
1. Führen Sie die folgenden Befehle aus, um die VM bereitzustellen.  
   > [!NOTE] 
   > Wenn Sie eine bestehende Skalierungsgruppe aktualisieren möchten, fügen Sie einfach *-Mode Incremental* am Ende des folgenden Befehls hinzu. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Nach dem erfolgreichen Bereitstellen Ihres virtuellen Computers sollten Sie in der Lage sein, die VM-Skalierungsgruppe im Azure-Portal zu finden, und sie sollte Metriken für Azure Monitor ausgeben. 

   > [!NOTE] 
   > Es kann vorkommen, dass in der Umgebung des ausgewählten „vmSkuSize“ Fehler auftreten. Wechseln Sie in diesem Fall zurück zu Ihrer Datei „azuredeploy.json“ und aktualisieren Sie den Standardwert des Parameters „vmSkuSize“. In diesem Fall wird empfohlen, es mit „Standard_DS1_v2“ zu versuchen. 


## <a name="chart-your-metrics"></a>Erfassen Ihrer Metriken 

1. Melden Sie sich auf dem Azure-Portal an. 

1. Klicken Sie im linken Menü auf **Überwachen**. 

1. Klicken Sie auf der Seite „Überwachen“ auf **Metriken**. 

   ![Seite „Metriken“](./media/metrics-store-custom-rest-api/metrics.png) 

1. Ändern Sie den Aggregationszeitraum in **Letzte 30 Minuten**.  

1. Wählen Sie in der Dropdownliste der Ressourcen die VM-Skalierungsgruppe aus, die Sie gerade erstellt haben.  

1. Wählen Sie in der Dropdownliste der Namespaces **azure.vm.windows.guest** aus. 

1. Wählen Sie in der Dropdownliste der Metriken **Arbeitsspeicher\%Verwendete zugesicherte Bytes** aus.  

Sie können dann auch die Dimensionen dieser Metrik verwenden, um diese Metrik für eine bestimmte VM in der Skalierungsgruppe zu erfassen oder um jede VM in der Skalierungsgruppe zu erfassen. 



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [benutzerdefinierte Metriken](metrics-custom-overview.md).

