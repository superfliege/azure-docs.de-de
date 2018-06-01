---
title: Vorgehensweise zum Upgraden/Migrieren der SKU für den primären Knotentyp auf höhere SKUs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die SKU für den primären Knotentyp mithilfe von PowerShell- und CLI-Befehlen auf höhere SKUs upgraden/migrieren.
services: service-fabric
documentationcenter: .net
author: v-rachiw
manager: navya
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2018
ms.author: v-rachiw
ms.openlocfilehash: 8d5b560068a9e0bc0169bfdb98c5e939e34a3b8c
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34274025"
---
# <a name="upgrademigrate-the-sku-for-primary-node-type-to-higher-sku"></a>Upgraden/Migrieren der SKU für den primären Knotentyp auf eine höhere SKU

In diesem Artikel wird beschrieben, wie Sie die SKU des primären Knotentyps von Service Fabric-Clustern mithilfe von Azure PowerShell auf eine höhere SKU upgraden/migrieren

## <a name="add-a-new-virtual-machine-scale-set"></a>Hinzufügen neuer VM-Skalierungsgruppen 

Stellen Sie eine neue VM-Skalierungsgruppe und einen Lastenausgleich bereit. Die Konfiguration der Service Fabric-Erweiterung (insbesondere der Knotentyp) der neuen VM-Skalierungsgruppe sollte mit der der alten Skalierungsgruppe, die Sie upgraden möchten, identisch sein. Überprüfen Sie im SF-Explorer, ob die neuen Knoten verfügbar sind. 

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird Azure PowerShell verwendet, um die aktualisierte Resource Manager-Vorlage *template.json* mithilfe der Ressourcengruppe *myResourceGroup* bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName myResourceGroup -TemplateFile \template\template.json 
```

Ändern Sie anhand des folgenden Beispiels die JSON-Vorlage, um die neue Ressource der VM-Skalierungsgruppe mit dem primären Knotentyp im vorhandenen Cluster hinzuzufügen

```json
        {
            "apiVersion": "[variables('vmssApiVersion')]",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[parameters('vmNodeType2Name')]",
            "location": "[parameters('computeLocation')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType2Name')))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
            ],
            "properties": {
                "overprovision": "[parameters('overProvision')]",
                "upgradePolicy": {
                    "mode": "Automatic"
                },
                "virtualMachineProfile": {
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "[concat(parameters('vmNodeType2Name'),'_ServiceFabricNode')]",
                                "properties": {
                                    "type": "ServiceFabricNode",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                                    },
                                    "publisher": "Microsoft.Azure.ServiceFabric",
                                    "settings": {
                                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                                        "dataPath": "D:\\\\SvcFab",
                                        "durabilityLevel": "Bronze",
                                        "enableParallelJobs": true,
                                        "nicPrefixOverride": "[parameters('subnet0Prefix')]",
                                        "certificate": {
                                            "thumbprint": "[parameters('certificateThumbprint')]",
                                            "x509StoreName": "[parameters('certificateStoreValue')]"
                                        }
                                    },
                                    "typeHandlerVersion": "1.0"
                                }
                            },
                            {
                                "name": "[concat('VMDiagnosticsVmExt','_vmNodeType2Name')]",
                                "properties": {
                                    "type": "IaaSDiagnostics",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                        "storageAccountEndPoint": "https://core.windows.net/"
                                    },
                                    "publisher": "Microsoft.Azure.Diagnostics",
                                    "settings": {
                                        "WadCfg": {
                                            "DiagnosticMonitorConfiguration": {
                                                "overallQuotaInMB": "50000",
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
                                            }
                                        },
                                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                                    },
                                    "typeHandlerVersion": "1.5"
                                }
                            }
                        ]
                    },
                    "networkProfile": {
                        "networkInterfaceConfigurations": [
                            {
                                "name": "[concat(parameters('nicName'), '-2')]",
                                "properties": {
                                    "ipConfigurations": [
                                        {
                                            "name": "[concat(parameters('nicName'),'-',2)]",
                                            "properties": {
                                                "loadBalancerBackendAddressPools": [
                                                    {
                                                        "id": "[variables('lbPoolID2')]"
                                                    }
                                                ],
                                                "loadBalancerInboundNatPools": [
                                                    {
                                                        "id": "[variables('lbNatPoolID2')]"
                                                    }
                                                ],
                                                "subnet": {
                                                    "id": "[variables('subnet0Ref')]"
                                                }
                                            }
                                        }
                                    ],
                                    "primary": true
                                }
                            }
                        ]
                    },
                    "osProfile": {
                        "adminPassword": "[parameters('adminPassword')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "computernamePrefix": "[parameters('vmNodeType2Name')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[parameters('sourceVaultValue')]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateStore": "[parameters('certificateStoreValue')]",
                                        "certificateUrl": "[parameters('certificateUrlValue')]"
                                    }
                                ]
                            }
                        ]
                    },
                    "storageProfile": {
                        "imageReference": {
                            "publisher": "[parameters('vmImagePublisher')]",
                            "offer": "[parameters('vmImageOffer')]",
                            "sku": "[parameters('vmImageSku')]",
                            "version": "[parameters('vmImageVersion')]"
                        },
                        "osDisk": {
                            "caching": "ReadOnly",
                            "createOption": "FromImage",
                            "managedDisk": {
                                "storageAccountType": "[parameters('storageAccountType')]"
                            }
                        }
                    }
                }
            },
            "sku": {
                "name": "[parameters('vmNodeType2Size')]",
                "capacity": "[parameters('nt2InstanceCount')]",
                "tier": "Standard"
            },
            "tags": {
                "resourceType": "Service Fabric",
                "clusterName": "[parameters('clusterName')]"
            }
        },
```


## <a name="remove-old-virtual-machine-scale-set"></a>Entfernen alter VM-Skalierungsgruppen

Deaktivieren Sie die VM-Instanzen der alten VM-Skalierungsgruppe, damit Sie den Knoten entfernen können. Dieser Vorgang dauert möglicherweise etwas länger. Sie können alle gleichzeitig deaktivieren. Daraufhin werden sie in eine Warteschlange eingereiht. Warten Sie, bis alle Knoten deaktiviert sind. 

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird die Azure Service Fabric-PowerShell verwendet, um die Knoteninstanz *NTvm1_0* in der alten VM-Skalierungsgruppe *NTvm1* zu deaktivieren:

```powershell
Disable-ServiceFabricNode -NodeName NTvm1_0 -Intent RemoveNode 
```

Entfernen Sie die vollständige Skalierungsgruppe. Warten Sie, bis der Bereitstellungsstatus der Skalierungsgruppe erfolgreich geändert wurde. 

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird Azure PowerShell verwendet, um die vollständige Skalierungsgruppe *NTvm1* aus der Ressourcengruppe *myResourceGroup* zu entfernen:

```powershell
Remove-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName NTvm1
```

## <a name="remove-load-balancer-related-to-old-scale-set"></a>Entfernen des Lastenausgleichs der alten Skalierungsgruppe

Entfernen Sie den Lastenausgleich der alten Skalierungsgruppe. Dieser Schritt führt zur einer kurzen Ausfallzeit für den Cluster

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird Azure PowerShell zum Entfernen des Lastenausgleichs *LB-myCluster-NTvm1* für die alte Skalierungsgruppe aus der Ressourcengruppe *myResourceGroup* verwendet:

```powershell
Remove-AzureRmLoadBalancer -Name LB-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

## <a name="remove-public-ip-related-to-old-scale-set"></a>Entfernen der öffentlichen IP-Adresse für die alte Skalierungsgruppe

Speichern der DNS-Einstellungen der öffentlichen IP-Adresse für die alte Skalierungsgruppe in einer Variable und anschließendes Entfernen der öffentlichen IP-Adresse

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird Azure PowerShell verwendet, um die DNS-Einstellungen der öffentlichen IP-Adresse *LBIP-myCluster-NTvm1* in einer Variable zu speichern und die IP-Adresse zu entfernen:

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn
Remove-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

## <a name="update-public-ip-address-related-to-new-scale-set"></a>Aktualisieren der öffentlichen IP-Adresse für die neue Skalierungsgruppe

Aktualisieren der DNS-Einstellungen der öffentlichen IP-Adresse für die neue Skalierungsgruppe mit DNS-Einstellungen der öffentlichen IP-Adresse für die alte Skalierungsgruppe

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird Azure PowerShell verwendet, um die DNS-Einstellungen der öffentlichen IP-Adresse *LBIP-myCluster-NTvm3* mit den zuvor in einer Variable gespeicherten DNS-Einstellungen zu aktualisieren:

```powershell
$PublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP 
```

## <a name="remove-knowledge-of-service-fabric-node-from-fm"></a>Entfernen von Informationen zum Service Fabric-Knoten aus dem Failover-Manager

Benachrichtigen Sie Service Fabric, dass die ausgefallenen Knoten aus dem Cluster entfernt wurden. (Führen Sie diesen Befehl für alle VM-Instanzen der alten VM-Skalierungsgruppe aus.) (Wenn die Dauerhaftigkeit der alten VM-Skalierungsgruppe Silver oder Gold war, ist dieser Schritt eventuell nicht erforderlich. Dieser Schritt wird dann vom System automatisch durchgeführt.)

### <a name="azure-powershell"></a>Azure PowerShell
Im folgenden Beispiel wird Azure Service Fabric PowerShell verwendet, um Service Fabric über die Entfernung des Knotens *NTvm1_0* zu benachrichtigen:

```powershell
Remove-ServiceFabricNodeState -NodeName NTvm1_0
```