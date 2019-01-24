---
ms.assetid: ''
title: 'Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken: Azure Key Vault'
description: Schrittweise Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.workload: identity
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: d95ede3b6e99d6791a2642c6059281dedca3fcf2
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423159"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken

In diesem Artikel erhalten Sie schrittweise Anleitungen zum Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken, um den Zugriff auf Ihren Schlüsseltresor einzuschränken. Die [Dienstendpunkte virtueller Netzwerke für Key Vault](key-vault-overview-vnet-service-endpoints.md) ermöglichen Ihnen, den Zugriff auf angegebene virtuelle Netzwerke und eine Gruppe von IPv4-Adressbereichen (Internet Protocol, Version 4) zu beschränken.

> [!IMPORTANT]
> Nachdem Firewallregeln in Kraft sind, können Benutzer nur Vorgänge auf Key Vault-[Datenebene](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) ausführen, wenn ihre Anforderungen aus zulässigen virtuellen Netzwerken oder IPv4-Adressbereichen stammen. Dies gilt auch für den Zugriff auf den Schlüsseltresor aus dem Azure-Portal. Obwohl Benutzer im Azure-Portal zu einem Schlüsseltresor navigieren können, können sie möglicherweise keine Schlüssel, Geheimnisse oder Zertifikate auflisten, wenn ihr Clientcomputer nicht in der Zulassungsliste enthalten ist. Dies wirkt sich auch auf die Key Vault-Auswahl anderer Azure-Dienste aus. Benutzer können möglicherweise eine Liste von Schlüsseltresoren einsehen, aber keine Schlüssel auflisten, wenn Firewallregeln ihren Clientcomputer blockieren.

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Im Folgenden finden Sie Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken mithilfe des Azure-Portals:

1. Navigieren Sie zum Schlüsseltresor, den Sie absichern möchten.
2. Wählen Sie **Firewalls und virtuelle Netzwerke** aus.
3. Wählen Sie unter **Zugriff erlauben von** den Eintrag **Ausgewählte Netzwerke** aus.
4. Wenn Sie vorhandene virtuelle Netzwerke zu Firewalls und Regeln für virtuelle Netzwerke hinzufügen möchten, klicken Sie auf **+ Vorhandene virtuelle Netzwerke hinzufügen**.
5. Wählen Sie auf dem angezeigten neuen Blatt das Abonnement, die virtuellen Netzwerke und Subnetze aus, denen Sie Zugriff auf diesen Schlüsseltresor gewähren möchten. Wenn für die von Ihnen ausgewählten virtuellen Netzwerke und Subnetze keine Dienstendpunkte aktiviert sind, bestätigen Sie, dass Sie Dienstendpunkte aktivieren möchten, und wählen Sie **Aktivieren** aus. Es kann bis zu 15 Minuten dauern, bis diese Änderung wirksam wird.
6. Fügen Sie unter **IP-Netzwerke** IPv4-Adressbereiche hinzu, indem Sie IPv4-Adressbereiche in [CIDR-Notation (Classless Inter-Domain Routing)](https://tools.ietf.org/html/rfc4632) oder einzelne IP-Adressen eingeben.
7. Wählen Sie **Speichern** aus.

Sie können auch neue virtuelle Netzwerke und Subnetze hinzufügen und dann Dienstendpunkte für die neu erstellten virtuellen Netzwerke und Subnetze aktivieren, indem Sie **+ Neues virtuelles Netzwerk hinzufügen** auswählen. Folgen Sie dann den Anweisungen.

## <a name="use-the-azure-cli-20"></a>Verwenden von Azure CLI 2.0

Im Folgenden finden Sie Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken mithilfe der Azure CLI 2.0:

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

5. Fügen Sie einen IP-Adressbereich hinzu, aus dem Datenverkehr zugelassen werden soll.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Wenn dieser Schlüsselspeicher für vertrauenswürdige Dienste zugänglich sein sollte, legen Sie `bypass` auf `AzureServices` fest.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Aktivieren Sie die Netzwerkregeln, indem Sie die Standardaktion auf `Deny` festlegen.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

Im Folgenden finden Sie Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken mithilfe der PowerShell:

1. Installieren Sie die neueste Version von [Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps), und [melden Sie sich an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

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

5. Fügen Sie einen IP-Adressbereich hinzu, aus dem Datenverkehr zugelassen werden soll.
   ```PowerShell
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Wenn dieser Schlüsselspeicher für vertrauenswürdige Dienste zugänglich sein sollte, legen Sie `bypass` auf `AzureServices` fest.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Aktivieren Sie die Netzwerkregeln, indem Sie die Standardaktion auf `Deny` festlegen.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Referenzen

* Azure CLI 2.0-Befehle: [az keyvault network-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell-Cmdlets: [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [Add-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [Update-AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Nächste Schritte

* [Dienstendpunkte virtueller Netzwerke für Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Schützen Ihrer Key Vault-Instanz](key-vault-secure-your-key-vault.md)