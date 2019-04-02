---
title: Bereitstellen einer VM über den Azure Marketplace | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie einen virtuellen Computer über einen vorkonfigurierten virtuellen Computer vom Azure Marketplace bereitstellen.
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
ms.openlocfilehash: d800d2a9c4eced2fa347658ecbb5b7a97031d997
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838696"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Bereitstellen eines virtuellen Computers über den Azure Marketplace

In diesem Artikel wird beschrieben, wie Sie einen vorkonfigurierten virtuellen Computer (VM) über einen Azure Marketplace bereitstellen, indem Sie das beigefügte Azure PowerShell-Skript verwenden.  Mit diesem Skript werden auch die WinRM-HTTP- und HTTPS-Endpunkte auf der VM verfügbar gemacht.  Für dieses Skript ist es erforderlich, dass Sie bereits ein Zertifikat in Azure Key Vault hochgeladen haben, wie unter [Erstellen von Zertifikaten für Azure Key Vault](./cpp-create-key-vault-cert.md) beschrieben. 


## <a name="vm-deployment-template"></a>VM-Bereitstellungsvorlage

Die Azure-VM-Bereitstellungsvorlage für den Schnellstart ist als Onlinedatei unter [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json) verfügbar.  Sie enthält die folgenden Parameter:

|  **Parameter**        |   **Beschreibung**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName | Name des Speicherkontos                       |
| dnsNameForPublicIP    | DNS-Name für die öffentliche IP-Adresse. Muss aus Kleinbuchstaben bestehen.    |
| adminUserName         | Benutzername des Administrators                          |
| adminPassword         | Kennwort des Administrators                          |
| imagePublisher        | Imageherausgeber                                   |
| imageOffer            | Imageangebot                                       |
| imageSKU              | Image-SKU                                         |
| vmSize                | Größe des virtuellen Computers                                    |
| vmName                | Name des virtuellen Computers                                    |
| vaultName             | Der Name des Schlüsseltresors                             |
| vaultResourceGroup    | Die Ressourcengruppe des Schlüsseltresors                   |
| certificateUrl        | URL für das Zertifikat, einschließlich der Version in KeyVault, z.B. `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>Bereitstellungsskript

Bearbeiten Sie das folgende Azure PowerShell-Skript, und führen Sie es aus, um die angegebene Azure Marketplace-VM bereitzustellen.

```powershell

New-AzureRmResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine vorkonfigurierte VM bereitgestellt haben, können Sie die darauf enthaltenen Lösungen und Dienste konfigurieren und darauf zugreifen oder die VM für weitere Entwicklungsschritte nutzen. 
