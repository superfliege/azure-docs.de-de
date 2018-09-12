---
ms.assetid: ''
title: Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken
description: Schrittweise Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/31/2018
ms.author: ambapat
ms.openlocfilehash: c58fc56742c0a11771bdd84e4195e6f476622a3b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346669"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken

In dieser Anleitung wird schrittweise erklärt, wie Sie Key Vault-Firewalls und virtuelle Netzwerke konfigurieren, um den Zugriff auf Ihren Schlüsseltresor einzuschränken. Die [Virtual Network-Dienstendpunkte für Key Vault](key-vault-overview-vnet-service-endpoints.md) ermöglichen Ihnen, den Zugriff auf den Schlüsseltresor auf angegebene virtuelle Netzwerk und/oder eine Gruppe von IPv4-Adressbereichen (Internet Protocol, Version 4) zu beschränken.

> [!IMPORTANT]
> Sobald Firewalls und Regeln für virtuelle Netzwerke in Kraft sind, können alle Vorgänge auf Key Vault-[Datenebene](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) NUR ausgeführt werden, wenn Aufruferanforderungen aus zulässigen virtuellen Netzwerken oder IPV4-Adressbereichen stammen. Dies gilt auch für den Zugriff auf den Schlüsseltresor im Azure-Portal. Während ein Benutzer im Azure-Portal zu einem Schlüsseltresor navigieren kann, kann er möglicherweise keine Schlüssel/Geheimnisse/Zertifikate auflisten, wenn sein Clientcomputer nicht in der Zulassungsliste enthalten ist. Dies wirkt sich auch die „Key Vault-Auswahl“ anderer Azure-Dienste aus. Benutzer können möglicherweise eine Liste von Schlüsseltresoren einsehen, aber keine Schlüssel auflisten, wenn Firewallregeln ihren Clientcomputer blockieren.

## <a name="azure-portal"></a>Azure-Portal

1. Navigieren Sie zum Schlüsseltresor, den Sie absichern möchten.
2. Klicken Sie auf **Firewalls und virtuelle Netzwerke**.
3. Klicken Sie unter **Zugriff erlauben von** auf **Ausgewählte Netzwerke**.
4. Wenn Sie vorhandene virtuelle Netzwerke zu Firewalls und Regeln für virtuelle Netzwerke hinzufügen möchten, klicken Sie auf **+ Vorhandene virtuelle Netzwerke hinzufügen**.
5. Wählen Sie auf dem eingeblendeten neuen Blatt das Abonnement, die virtuellen Netzwerke und Subnetze aus, denen Sie Zugriff auf diesen Schlüsseltresor gewähren möchten. Wenn bei den von Ihnen ausgewählten virtuellen Netzwerken und Subnetzen keine Dienstendpunkte aktiviert sind, wird die folgende Meldung angezeigt: „Bei den folgenden Netzwerken sind die Dienstendpunkte nicht aktiviert...“. Klicken Sie auf **Aktivieren**, nachdem Sie bestätigt haben, dass Sie Dienstendpunkte für die aufgeführten virtuellen Netzwerke und Subnetze aktivieren möchten. Dieser Vorgang kann bis zu 15 Minuten dauern.
6. Sie können auch neue virtuelle Netzwerke und Subnetze hinzufügen und dann Dienstendpunkte für die neu erstellten virtuellen Netzwerke und Subnetze aktivieren, indem Sie auf **+ Neues virtuelles Netzwerk hinzufügen** klicken und die Anweisungen befolgen.
7. Unter **IP-Netzwerke** können Sie IPv4-Adressbereiche hinzufügen, indem Sie IPv4-Adressbereiche in [CIDR-Notation (Classless Inter-Domain Routing)](https://tools.ietf.org/html/rfc4632) oder einzelne IP-Adressen eingeben.
8. Klicken Sie auf **Speichern**.

## <a name="azure-cli-20"></a>Azure CLI 2.0

1. [Installieren Sie Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli), und [melden Sie sich an](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Listen Sie die verfügbaren Regeln für virtuelle Netzwerke auf. Wenn Sie keine Regeln für diesen Schlüsselspeicher festgelegt haben, ist die Liste leer.
```azurecli
az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
```

3. Aktivieren Sie den Dienstendpunkt für Key Vault in einem vorhandenen virtuellen Netzwerk und Subnetz.
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
```

4. Fügen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz hinzu.
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
```

5. Fügen Sie einen IP-Adressbereich hinzu, aus dem Datenverkehr zugelassen ist.
```azurecli
az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
```

6. Wenn dieser Schlüsselspeicher für vertrauenswürdige Dienste zugänglich sein soll, legen Sie „bypass“ auf „AzureServices“ fest.
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
```

7. Nun ein letzter wichtiger Schritt: Aktivieren Sie die Netzwerkregeln, indem Sie die Standardaktion auf „deny“ festlegen.
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
```

## <a name="azure-powershell"></a>Azure PowerShell

1. Installieren Sie die neueste Version von [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps), und [melden Sie sich an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Listen Sie die verfügbaren Regeln für virtuelle Netzwerke auf. Wenn Sie keine Regeln für diesen Schlüsselspeicher festgelegt haben, ist die Liste leer.
```PowerShell
(Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
```

3. Aktivieren Sie den Dienstendpunkt für Key Vault in einem vorhandenen virtuellen Netzwerk und Subnetz.
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
```

4. Fügen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz hinzu.
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
```

5. Fügen Sie einen IP-Adressbereich hinzu, aus dem Datenverkehr zugelassen ist.
```PowerShell
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
```

6. Wenn dieser Schlüsselspeicher für vertrauenswürdige Dienste zugänglich sein soll, legen Sie „bypass“ auf „AzureServices“ fest.
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
```

7. Nun ein letzter wichtiger Schritt: Aktivieren Sie die Netzwerkregeln, indem Sie die Standardaktion auf „deny“ festlegen.
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
```

## <a name="references"></a>Referenzen

* Azure CLI 2.0-Befehle: [az keyvault network-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell-Cmdlets: [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [Add-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [Update-AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Nächste Schritte

* [Dienstendpunkte virtueller Netzwerke für Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Schützen Ihrer Key Vault-Instanz](key-vault-secure-your-key-vault.md)