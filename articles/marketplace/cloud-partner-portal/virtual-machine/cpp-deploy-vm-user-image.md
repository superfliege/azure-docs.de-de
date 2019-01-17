---
title: Bereitstellen einer Azure-VM aus einer Benutzer-VHD | Microsoft-Dokumentation
description: Erläutert, wie ein Benutzer-VHD-Image bereitgestellt wird, um eine Azure-VM-Instanz zu erstellen.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 48be60a7ba5770f8c329cb6323a5caa8fcf7f961
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265055"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Bereitstellen einer Azure-VM aus einer Benutzer-VHD

In diesem Artikel wird erläutert, wie Sie ein generalisiertes VHD-Image bereitstellen, um eine neue Azure-VM-Ressource zu erstellen. Dazu verwenden Sie die mitgelieferte Azure Resource Manager-Vorlage und das mitgelieferte Azure PowerShell-Skript.


## <a name="vhd-deployment-template"></a>VHD-Bereitstellungsvorlage

Kopieren Sie die Azure Resource Manager-Vorlage für [VHD-Bereitstellung](cpp-deploy-json-template.md) in eine lokale Datei namens `VHDtoImage.json`.  Bearbeiten Sie diese Datei, um Werte für die folgenden Parameter anzugeben. 

|  **Parameter**             |   **Beschreibung**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Name einer vorhandenen Azure-Ressourcengruppe.  Üblicherweise sollte die Replikationsgruppe verwendet werden, die Ihrem Schlüsseltresor zugeordnet ist.  |
| TemplateFile               | Vollständiger Pfadname zur Datei `VHDtoImage.json`                                    |
| userStorageAccountName     | Name des Speicherkontos                                                    |
| sNameForPublicIP           | DNS-Name für die öffentliche IP-Adresse. Muss aus Kleinbuchstaben bestehen.                                  |
| subscriptionId             | Azure-Abonnement-ID                                                  |
| Standort                   | Standardmäßiger geografischer Azure-Standort der Ressourcengruppe                       |
| vmName                     | Name des virtuellen Computers                                                    |
| vaultName                  | Der Name des Schlüsseltresors                                                          |
| vaultResourceGroup         | Die Ressourcengruppe des Schlüsseltresors
| certificateUrl             | URL des Zertifikats (einschließlich Version), das im Schlüsseltresor gespeichert ist, zum Beispiel: `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | URL der virtuellen Festplatte                                                   |
| vmSize                     | Größe des Instanz des virtuellen Computers.                                           |
| publicIPAddressName        | Name der öffentlichen IP-Adresse                                                  |
| virtualNetworkName         | Name des virtuellen Netzwerks                                                    |
| nicName                    | Name der Netzwerkschnittstellenkarte für das virtuelle Netzwerk                     |
| adminUserName              | Benutzername des Administratorkontos                                          |
| adminPassword              | Administratorkennwort                                                          |
|  |  |


## <a name="powershell-script"></a>PowerShell-Skript

Kopieren Sie das folgende Skript, und bearbeiten Sie es, um Werte für die Variablen `$storageaccount` und `$vhdUrl` anzugeben.  Führen Sie das Skript aus, um eine Azure-VM-Ressource von Ihrer vorhandenen generalisierten VHD zu erstellen.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Ihr virtueller Computer bereitgestellt wurde, können Sie [Ihr VM-Image zertifizieren](./cpp-certify-vm.md).
