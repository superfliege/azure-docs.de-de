---
title: "Azure Active Directory Domain Services: Problembehandlung für die Konfiguration von Netzwerksicherheitsgruppen | Microsoft-Dokumentation"
description: "Problembehandlung für die NSG-Konfiguration für Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ergreenl
ms.openlocfilehash: 447f9119ea379e278be77d8699c7dcb751ea3085
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-network-security-group-configuration"></a>Azure AD Domain Services: Problembehandlung für die Konfiguration von Netzwerksicherheitsgruppen



## <a name="aadds104-network-error"></a>AADDS104: Netzwerkfehler

**Warnmeldung:** *Microsoft kann nicht auf die Domänencontroller für diese verwaltete Domäne zugreifen. Dies kann geschehen, wenn eine für Ihr virtuelles Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) den Zugriff auf ihre verwaltete Domäne blockiert. Eine weitere mögliche Ursache besteht in einer benutzerdefinierten Route, die eingehenden Datenverkehr aus dem Internet blockiert.*

Die häufigste Ursache von Netzwerkfehlern für Azure AD Domain Services kann fehlerhaften NSG-Konfigurationen zugeschrieben werden. Um sicherzustellen, dass Microsoft Ihre verwaltete Domäne warten und verwalten kann, müssen Sie eine Netzwerksicherheitsgruppe (NSG) verwenden, um den Zugriff auf [bestimmte Ports](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) innerhalb Ihres Subnetzes zu ermöglichen. Wenn diese Ports blockiert sind, kann Microsoft nicht auf die benötigten Ressourcen zugreifen, was Ihrem Dienst abträglich sein kann. Halten Sie diese Ports während der Erstellung Ihrer NSG offen, um sicherzustellen, dass keine Dienstunterbrechung eintritt.

## <a name="sample-nsg"></a>NSG-Beispiel
Die folgende Tabelle zeigt ein NSG-Beispiel, das den Schutz Ihrer verwalteten Domäne sicherstellt und es Microsoft zugleich ermöglicht, Informationen zu überwachen, zu verwalten und zu aktualisieren.

![NSG-Beispiel](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Für Azure AD Domain Services ist uneingeschränkter ausgehender Zugriff erforderlich. Es wird empfohlen, keine zusätzlichen Ausgangsregeln für Ihre NSGs zu erstellen.

## <a name="adding-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Hinzufügen einer Regel zu einer Netzwerksicherheitsgruppe mithilfe des Azure-Portals
Wenn Sie PowerShell nicht verwenden möchten, können Sie den NSGs mithilfe des Azure-Portals manuell einzelne Regeln hinzufügen. Befolgen Sie diese Schritte, um Regeln in Ihrer Netzwerksicherheitsgruppe zu erstellen.

1. Navigieren Sie zur Seite [Netzwerksicherheitsgruppen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) im Azure-Portal.
2. Wählen Sie in der Tabelle die NSG aus, die Ihrer Domäne zugeordnet ist.
3. Klicken Sie unter „Einstellungen“ im linken Navigationsbereich entweder auf **Eingangssicherheitsregeln** oder auf **Ausgangssicherheitsregeln**.
4. Erstellen Sie die Regel, indem Sie auf **Hinzufügen** klicken und die erforderlichen Informationen einsetzen. Klicken Sie auf **OK**.
5. Überprüfen Sie, ob Ihre Regel erstellt wurde, indem Sie sie in der Tabelle der Regeln suchen.


## <a name="create-a-default-nsg-using-powershell"></a>Erstellen einer Standard-NSG mit PowerShell

Vorstehend finden Sie Schritte, die Sie zum Erstellen einer neuen NSG mithilfe von PowerShell verwenden können, die alle für die Ausführung von Azure AD Domain Services erforderlichen Ports offen lässt und jeglichen anderen unerwünschten Zugriff verweigert.


Diese Lösung erfordert die Installation und Ausführung von [Azure AD Powershell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?toc=%2Fazure%2Factive-directory-domain-services%2Ftoc.json&view=azureadps-2.0).

1. Stellen Sie eine Verbindung mit Ihrem Azure AD-Verzeichnis her.

  ```PowerShell
  # Connect to your Azure AD directory.
  Connect-AzureAD
  ```
2. Melden Sie sich bei Ihrem Azure-Abonnement an.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

3. Erstellen Sie eine NSG mit drei Regeln. Das folgende Skript definiert drei Regeln für die NSG, die den Zugriff auf die Ports zulässt, die für die Ausführung von Azure AD Domain Services erforderlich sind. Anschließend erstellt das Skript eine neue NSG, die diese Regeln enthält. Sie können bei Bedarf gerne weitere Regeln im gleichen Format hinzufügen.

  ```PowerShell
  # Create the rules needed
  $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443
  $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389
  $rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986
  # Create the NSG with the 3 rules above
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDS-NSG" -SecurityRules $rule1,$rule2,$rule3
  ```

4. Schließlich ordnet das Skript die NSG dem gewählten vnet und Subnetz zu.

  ```PowerShell
  # Find vnet and subnet
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
  # Set the nsg to the subnet and save the changes
  $subnet.NetworkSecurityGroup = $nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

### <a name="full-script"></a>Vollständiges Skript

```PowerShell
#Change the following values to match your deployment
$resourceGroup = "ResourceGroupName"
$vnetName = "exampleVnet"
$subnetName = "exampleSubnet"

$serviceIPs = "52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161"

# Create the rules needed
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389


# Connect to your Azure AD directory.
Connect-AzureAD

# Log in to your Azure subscription.
Login-AzureRmAccount

# Create the NSG with the 3 rules above
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "NSG-Default" -SecurityRules $rule1,$rule2,$rule3

# Find vnet and subnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName

# Set the nsg to the subnet and save the changes
$subnet.NetworkSecurityGroup = $nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

> [!NOTE]
>Diese Standard-NSG sperrt nicht den Zugriff auf den Port, der für Secure LDAP verwendet wird. Informationen zum Erstellen einer Regel für diesen Port finden Sie in [diesem Artikel](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="contact-us"></a>Kontakt
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).
