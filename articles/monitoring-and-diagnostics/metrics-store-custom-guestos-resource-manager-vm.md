---
title: Senden von Gastbetriebssystemmetriken an den Metrikspeicher von Azure Monitor unter Verwendung einer Resource Manager-Vorlage für einen virtuellen Windows-Computer
description: Senden von Gastbetriebssystemmetriken an den Metrikspeicher von Azure Monitor unter Verwendung einer Resource Manager-Vorlage für einen virtuellen Windows-Computer
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 4ed911766a14dd35ea662326a5d50df11cf81698
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984069"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Senden von Gastbetriebssystemmetriken an den Metrikspeicher von Azure Monitor unter Verwendung einer Resource Manager-Vorlage für einen virtuellen Windows-Computer

Die [Windows Azure-Diagnoseerweiterung](azure-diagnostics.md) (WAD) von Azure Monitor ermöglicht es Ihnen, Metriken und Protokolle vom Gastbetriebssystem zu erfassen, das als Teil eines virtuellen Computers, eines Clouddiensts oder eines Service Fabric-Clusters ausgeführt wird.  Die Erweiterung kann Telemetriedaten an viele verschiedene Standorte senden, die im zuvor verlinkten Artikel aufgeführt sind.  

In diesem Artikel wird der Prozess zum Senden von Leistungsmetriken des Gastbetriebssystems für einen virtuellen Windows-Computer an den Azure Monitor-Datenspeicher beschrieben. Ab WAD Version 1.11 können Sie Metriken direkt in den Azure Monitor-Metrikspeicher schreiben, in dem bereits Metriken der Standardplattformen gesammelt werden. Wenn Sie sie an dieser Position speichern, können Sie auf die gleichen Aktionen zugreifen, die auch für Plattformmetriken verfügbar sind.  Aktionen sind nahezu in Echtzeit Warnungen, Diagramme, routing, REST-API und vieles mehr zugreifen.  In der Vergangenheit hat die WAD-Erweiterung in Azure Storage geschrieben, aber nicht der Azure Monitor-Datenspeicher.   

Wenn Sie noch nicht mit Resource Manager-Vorlagen vertraut sind, erfahren Sie mehr über [Vorlagenbereitstellungen](../azure-resource-manager/resource-group-overview.md) sowie deren Struktur und Syntax.  

## <a name="pre-requisites"></a>Voraussetzungen

- Ihr Abonnement muss mit [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) registriert werden. 

- Sie müssen entweder [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) installiert haben, oder Sie können [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) verwenden. 

 
## <a name="set-up-azure-monitor-as-a-data-sink"></a>Einrichten von Azure Monitor als Datensenke 
Die Azure-Diagnoseerweiterung verwendet eine Funktion namens „Datensenken“, um Metriken und Protokolle an verschiedene Standorte zu leiten.  Die folgenden Schritte zeigen, wie Sie eine Resource Manager-Vorlage und PowerShell verwenden, um eine VM mit der neuen Datensenke „Azure Monitor“ bereitzustellen. 

## <a name="author-resource-manager-template"></a>Erstellen von Resource Manager-Vorlagen 
Für dieses Beispiel können Sie eine öffentlich zugängliche Beispielvorlage verwenden. Die Startvorlagen befinden sich unter https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows. 

- **Azuredeploy.json** ist eine vorkonfigurierte Resource Manager-Vorlage für die Bereitstellung eines virtuellen Computers. 

- **Azuredeploy.parameters.json** ist eine Parameterdatei, die Informationen wie Benutzername und Kennwort speichert, die Sie für Ihre VM festlegen. Während der Bereitstellung verwendet die Resource Manager-Vorlage die in dieser Datei festgelegten Parameter. 

Laden Sie beide Dateien herunter und speichern Sie sie lokal. 

###  <a name="modify-azuredeployparametersjson"></a>Ändern von azuredeploy.parameters.json
Öffnen Sie die Datei *azuredeploy.parameters.json*. 

1. Geben Sie Werte für *adminUsername* und *adminPassword* für die VM ein. Diese Parameter werden für den Remotezugriff auf die VMs verwendet. Verwenden Sie NICHT die in dieser Vorlage enthaltenen Parameter, um zu vermeiden, dass Ihre VM das Opfer eines Hijacking-Vorgangs wird. Bots durchsuchen das Internet nach Benutzernamen und Kennwörtern in öffentlichen Github-Repositorys. Es ist wahrscheinlich, dass sie VMs mit diesen Standardwerten testen.  

1. Erstellen Sie einen eindeutigen DNS-Namen für die VM.  

### <a name="modify-azuredeployjson"></a>Ändern von azuredeploy.json

Öffnen Sie die Datei *azuredeploy.json*. 

Fügen Sie dem Abschnitt **Variablen** der Vorlage nach dem Eintrag für **storageAccountName** eine Speicherkonto-ID hinzu.  

```json
// Find these lines 
"variables": { 
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]", 

// Add this line directly below.  
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]", 
```

Fügen Sie diese MSI-Erweiterung (verwaltete Dienstidentität) der Vorlage oben im Abschnitt „resources“ hinzu.  Die Erweiterung stellt sicher, dass Azure Monitor die ausgegebenen Metriken akzeptiert.  

```json
//Find this code 
"resources": [
// Add this code directly below
    { 
        "type": "Microsoft.Compute/virtualMachines/extensions", 
        "name": "WADExtensionSetup", 
        "apiVersion": "2015-05-01-preview", 
        "location": "[resourceGroup().location]", 
        "dependsOn": [ 
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" ], 
        "properties": { 
            "publisher": "Microsoft.ManagedIdentity", 
            "type": "ManagedIdentityExtensionForWindows", 
            "typeHandlerVersion": "1.0", 
            "autoUpgradeMinorVersion": true, 
            "settings": { 
                "port": 50342 
            } 
        } 
    }, 
```

Fügen Sie der VM-Ressource die Konfiguration „identity“ hinzu, um sicherzustellen, dass Azure der MSI-Erweiterung eine Systemidentität zuweist. Dieser Schritt stellt sicher, dass der virtuelle Computer Gastmetriken über sich selbst an Azure Monitor senden kann. 

```json
// Find this section
                "subnet": {
            "id": "[variables('subnetRef')]"
            }
        }
        }
    ]
    }
},
{ 
    "apiVersion": "2017-03-30", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[variables('vmName')]", 
    "location": "[resourceGroup().location]", 
    // add these 3 lines below
    "identity": {  
    "type": "SystemAssigned" 
    }, 
    //end of added lines
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
    "hardwareProfile": {   
    ...
```

Fügen Sie die folgende Konfiguration hinzu, um die Diagnoseerweiterung auf einem virtuellen Windows-Computer zu aktivieren.  Bei einem einfachen, auf Ressourcen-Manager basierenden virtuellen Computer kann die Erweiterungskonfiguration dem Array „resources“ für den virtuellen Computer hinzugefügt werden. Die Zeile „sinks“: „AzMonSink“ und das später im Abschnitt folgende entsprechende „SinksConfig“ ermöglichen es der Erweiterung, Metriken direkt an Azure Monitor zu senden. Sie können Leistungsindikatoren nach Bedarf hinzufügen/entfernen.  


```json
        "networkProfile": {
            "networkInterfaces": [
            {
                "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
            ]
        },
"diagnosticsProfile": { 
    "bootDiagnostics": { 
    "enabled": true, 
    "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]" 
    } 
} 
}, 
//Start of section to add 
"resources": [        
{ 
            "type": "extensions", 
            "name": "Microsoft.Insights.VMDiagnosticsSettings", 
            "apiVersion": "2015-05-01-preview", 
            "location": "[resourceGroup().location]", 
            "dependsOn": [ 
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" 
            ], 
            "properties": { 
            "publisher": "Microsoft.Azure.Diagnostics", 
            "type": "IaaSDiagnostics", 
            "typeHandlerVersion": "1.4", 
            "autoUpgradeMinorVersion": true, 
            "settings": { 
                "WadCfg": { 
                "DiagnosticMonitorConfiguration": { 
    "overallQuotaInMB": 4096, 
    "DiagnosticInfrastructureLogs": { 
                    "scheduledTransferLogLevelFilter": "Error" 
        }, 
                    "Directories": { 
                    "scheduledTransferPeriod": "PT1M", 
    "IISLogs": { 
                        "containerName": "wad-iis-logfiles" 
                    }, 
                    "FailedRequestLogs": { 
                        "containerName": "wad-failedrequestlogs" 
                    } 
                    }, 
                    "PerformanceCounters": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "sinks": "AzMonSink", 
                    "PerformanceCounterConfiguration": [ 
                        { 
                        "counterSpecifier": "\\Memory\\Available Bytes", 
                        "sampleRate": "PT15S" 
                        }, 
                        { 
                        "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                        "sampleRate": "PT15S" 
                        }, 
                        { 
                        "counterSpecifier": "\\Memory\\Committed Bytes", 
                        "sampleRate": "PT15S" 
                        } 
                    ] 
                    }, 
                    "WindowsEventLog": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "DataSource": [ 
                        { 
                        "name": "Application!*" 
                        } 
                    ] 
                    }, 
                    "Logs": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "scheduledTransferLogLevelFilter": "Error" 
                    } 
                }, 
                "SinksConfig": { 
                    "Sink": [ 
                    { 
                        "name" : "AzMonSink", 
                        "AzureMonitor" : {} 
                    } 
                    ] 
                } 
                }, 
                "StorageAccount": "[variables('storageAccountName')]" 
            }, 
            "protectedSettings": { 
                "storageAccountName": "[variables('storageAccountName')]", 
                "storageAccountKey": "[listKeys(variables('accountid'),'2015-06-15').key1]", 
                "storageAccountEndPoint": "https://core.windows.net/" 
            } 
            } 
        } 
        ] 
//End of section to add 
```


Speichern und schließen Sie beide Dateien. 
 

## <a name="deploy-the-resource-manager-template"></a>Bereitstellen der Resource Manager-Vorlage 

> [!NOTE]
> Sie müssen die Azure-Diagnoseerweiterung Version 1.5 oder höher ausführen UND für die Eigenschaft „autoUpgradeMinorVersion“ den Wert „true“ in Ihrer Resource Manager-Vorlage festgelegt haben.  Azure lädt dann die richtige Erweiterung, wenn die VM gestartet wird. Wenn diese Einstellungen in Ihrer Vorlage nicht vorhanden sind, ändern Sie sie und stellen Sie die Vorlage erneut bereit. 


Für die Bereitstellung der Resource Manager-Vorlage wird hier Azure PowerShell verwendet.  

1. Starten von PowerShell 
1. Anmelden an Azure mit `Login-AzureRmAccount`
1. Rufen Sie Ihre Liste der Abonnements mit `Get-AzureRmSubscription` ab.
1. Legen Sie das Abonnement fest, in dem Sie den virtuellen Computer erstellen/aktualisieren möchten. 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Erstellen Sie eine neue Ressourcengruppe für die bereitzustellende VM und führen Sie den folgenden Befehl aus. 

   ```PowerShell
    New-AzureRmResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>" 
   ```
   > [!NOTE] 
   > Denken Sie daran, eine [Azure-Region zu verwenden, die für benutzerdefinierte Metriken aktiviert ist](metrics-custom-overview.md). 
 
1. Führen Sie die folgenden Befehle aus, um die VM bereitzustellen.  
   > [!NOTE] 
   > Wenn Sie einen bestehenden virtuellen Computer aktualisieren möchten, fügen Sie einfach *-Mode Incremental* am Ende des folgenden Befehls hinzu. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>" 
   ```
  
1. Nach dem erfolgreichen Bereitstellen Ihres virtuellen Computers sollten Sie in der Lage sein, den virtuellen Computer im Azure-Portal zu finden, und er sollte Metriken für Azure Monitor ausgeben. 

   > [!NOTE] 
   > Es kann vorkommen, dass in der Umgebung des ausgewählten „vmSkuSize“ Fehler auftreten. Wechseln Sie in diesem Fall zurück zu Ihrer Datei „azuredeploy.json“ und aktualisieren Sie den Standardwert des Parameters „vmSkuSize“. In diesem Fall wird empfohlen, es mit „Standard_DS1_v2“ zu versuchen. 

## <a name="chart-your-metrics"></a>Erfassen Ihrer Metriken 

1. Anmelden beim Azure-Portal 

1. Klicken Sie im linken Menü auf **Überwachen**. 

1. Klicken Sie auf der Seite „Überwachen“ auf **Metriken**. 

   ![Seite „Metriken“](./media/metrics-store-custom-rest-api/metrics.png) 

1. Ändern Sie den Aggregationszeitraum in **Letzte 30 Minuten**.  

1. Wählen Sie in der Dropdownliste der Ressourcen den soeben erstellten virtuellen Computer aus. Wenn Sie den Namen in der Vorlage nicht geändert haben, sollte er *SimpleWinVM2* lauten.  

1. Wählen Sie in der Dropdownliste der Namespaces die Option **azure.vm.windows.guest** aus. 

1. Wählen Sie in der Dropdownliste der Metriken **Arbeitsspeicher\%Verwendete zugesicherte Bytes** aus.  
 

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [benutzerdefinierte Metriken](metrics-custom-overview.md).