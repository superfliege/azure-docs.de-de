---
title: Key Vault-VM-Erweiterung für Windows | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie einen Agent zur automatischen Aktualisierung von Key Vault-Geheimnissen auf virtuellen Computern per VM-Erweiterung bereitstellen.
services: virtual-machines-windows
documentationcenter: ''
author: dragav
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: dragosav
ms.openlocfilehash: 19e177f086ea9fa817a36e67ace77aed39ee89b5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33944752"
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Key Vault-VM-Erweiterung für Windows

## <a name="overview"></a>Übersicht

Die Key Vault-VM-Erweiterung ermöglicht die automatische Aktualisierung von Geheimnissen, die in einem Azure-Schlüsseltresor gespeichert sind. Hierbei überwacht die Erweiterung eine Liste mit beobachteten Zertifikaten, die in Schlüsseltresoren gespeichert sind, und ruft bei Erkennung einer Änderung die entsprechenden Zertifikate ab und installiert sie. Die Key Vault-VM-Erweiterung wird veröffentlicht und von Microsoft unterstützt (derzeit auf Windows-VMs). Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die Key Vault-VM-Erweiterung für Windows. 

## <a name="prerequisites"></a>Voraussetzungen

Die Key Vault-VM-Erweiterung ist von der VM-Erweiterung „Verwaltete Dienstidentität“ abhängig, um die Authentifizierung gegenüber dem Key Vault-Dienst zu ermöglichen. Ausführlichere Informationen finden Sie in der Dokumentation zur MSI-VM-Erweiterung.

### <a name="operating-system"></a>Betriebssystem

Die Key Vault-VM-Erweiterung unterstützt derzeit alle Windows-Versionen.

| Distribution | Version |
|---|---|
| Windows | Core |

### <a name="internet-connectivity"></a>Internetkonnektivität

Um die Key Vault-VM-Erweiterung für Windows verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-schema"></a>Erweiterungsschema

Im folgenden JSON-Code ist das Schema für die Key Vault-VM-Erweiterung dargestellt. Für die Erweiterung sind keine geschützten Einstellungen erforderlich. Alle Einstellungen werden als Informationen ohne Sicherheitsauswirkungen angesehen. Für die Erweiterung werden eine Liste mit überwachten Geheimnissen, die Abrufhäufigkeit und ein Zielzertifikatspeicher benötigt. Dies gilt insbesondere in folgenden Fällen:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel | Datentyp |
| ---- | ---- | ---- |
| apiVersion | 2016-10-01 | date |
| Herausgeber | Microsoft.Azure.KeyVault.Edp | Zeichenfolge |
| type | KeyVaultForWindows | Zeichenfolge |
| typeHandlerVersion | 0.0 | int |
| pollingIntervalInS | 3600 | int |
| certificateStoreName | MY | Zeichenfolge |
| certificateStoreLocation  | LOCAL_MACHINE | Zeichenfolge |
| observedCertificates  | ["https://myvault.vault.azure.net:443/secrets/mycertificate"] | Zeichenfolgenarray


## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Vorlagen sind ideal, wenn Sie virtuelle Computer bereitstellen, für die nach der Bereitstellung eine Aktualisierung der Zertifikate erforderlich ist. Die Erweiterung kann auf einzelnen virtuellen Computern oder in VM-Skalierungsgruppen bereitgestellt werden. Das Schema und die Konfiguration sind für beide Vorlagentypen gleich. 

Die JSON-Konfiguration für eine VM-Erweiterung muss im VM-Ressourcenfragment der Vorlage geschachtelt sein (im `"resources": []`-Objekt des virtuellen Computers).

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

## <a name="azure-powershell-deployment"></a>Azure PowerShell-Bereitstellung

Azure PowerShell kann verwendet werden, um die Key Vault-VM-Erweiterung auf einem vorhandenen virtuellen Computer oder in einer VM-Skalierungsgruppe bereitzustellen. 

* Stellen Sie die Erweiterung wie folgt auf einer VM bereit:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMName> + '/KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Set-AzureRmVmExtension -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Gehen Sie wie folgt vor, um die Erweiterung in einer VM-Skalierungsgruppe bereitzustellen, z.B. als Teil eines Service Fabric-Clusters:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMSSName> + '_KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Add-AzureRmVmssExtension -VirtualMachineScaleSet <VMSS> -Name $extName -Publisher $extPublisher -Type $extType -Setting $settings
    
    ```

Beachten Sie hierbei die folgenden Einschränkungen bzw. Anforderungen:
- Die Bereitstellung muss in der Region „USA, Süden-Mitte“ erfolgen.
- Key Vault-Einschränkungen:
    - Muss zum Zeitpunkt der Bereitstellung vorhanden sein. 
    - Muss sich in derselben Region und Ressourcengruppe wie die Bereitstellung befinden.
    - Muss für die Bereitstellung und Vorlagenbereitstellung aktiviert sein.

## <a name="azure-cli-deployment"></a>Bereitstellung für die Azure-Befehlszeilenschnittstelle

Beispiele für die Bereitstellung der Key Vault-VM-Erweiterung in Azure CLI sind in Kürze verfügbar. 

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe von Azure PowerShell abgerufen werden. Führen Sie über Azure PowerShell den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für eine bestimmte VM anzuzeigen.

```powershell
Get-AzureRMVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.Edp.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).
