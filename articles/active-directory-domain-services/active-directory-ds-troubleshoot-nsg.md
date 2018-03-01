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
ms.date: 02/12/2018
ms.author: ergreenl
ms.openlocfilehash: cc9a61314de7e10afe370c3b1307d03544a379d5
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Beheben von Problemen bei einer ungültigen Netzwerkkonfiguration für verwaltete Domänen
Dieser Artikel hilft Ihnen bei der Problembehandlung und Lösung von Konfigurationsfehlern im Zusammenhang mit dem Netzwerk, die zu der folgenden Fehlermeldung führen:

## <a name="alert-aadds104-network-error"></a>Warnung AADDS104: Netzwerkfehler
**Warnmeldung:** *Microsoft kann nicht auf die Domänencontroller für diese verwaltete Domäne zugreifen. Dies kann geschehen, wenn eine für Ihr virtuelles Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) den Zugriff auf ihre verwaltete Domäne blockiert. Eine weitere mögliche Ursache ist eine benutzerdefinierte Route, die eingehenden Datenverkehr aus dem Internet blockiert.*

Die häufigste Ursache von Netzwerkfehlern für Azure AD Domain Services sind ungültige NSG-Konfigurationen. Die für das virtuelle Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) muss den Zugriff auf [bestimmte Ports](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) zulassen. Wenn diese Ports blockiert sind, kann Microsoft die verwaltete Domäne nicht überwachen oder aktualisieren. Darüber hinaus wird die Synchronisierung zwischen Ihrem Azure AD-Verzeichnis und der verwalteten Domäne beeinträchtigt. Halten Sie diese Ports während der Erstellung Ihrer NSG offen, um Dienstunterbrechungen zu vermeiden.


## <a name="sample-nsg"></a>NSG-Beispiel
Die folgende Tabelle zeigt ein NSG-Beispiel, das den Schutz Ihrer verwalteten Domäne sicherstellt und es Microsoft zugleich ermöglicht, Informationen zu überwachen, zu verwalten und zu aktualisieren.

![NSG-Beispiel](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Für Azure AD Domain Services ist uneingeschränkter ausgehender Zugriff vom virtuellen Netzwerk erforderlich. Es wird empfohlen, keine zusätzliche NSG-Regel zu erstellen, die den ausgehenden Zugriff für das virtuelle Netzwerk beschränkt.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Hinzufügen einer Regel zu einer Netzwerksicherheitsgruppe mithilfe des Azure-Portals
Wenn Sie PowerShell nicht verwenden möchten, können Sie den NSGs mithilfe des Azure-Portals manuell einzelne Regeln hinzufügen. Führen Sie die folgenden Schritte aus, um Regeln in Ihrer Netzwerksicherheitsgruppe zu erstellen:

1. Navigieren Sie zur Seite [Netzwerksicherheitsgruppen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) im Azure-Portal.
2. Wählen Sie in der Tabelle die NSG aus, die dem Subnetz zugeordnet ist, in dem Ihre verwaltete Domäne aktiviert ist.
3. Klicken Sie im linken Bereich unter **Einstellungen** entweder auf **Eingangssicherheitsregeln** oder auf **Ausgangssicherheitsregeln**.
4. Erstellen Sie die Regel, indem Sie auf **Hinzufügen** klicken und die erforderlichen Informationen einsetzen. Klicken Sie auf **OK**.
5. Überprüfen Sie, ob Ihre Regel erstellt wurde, indem Sie sie in der Tabelle der Regeln suchen.


## <a name="create-an-nsg-for-azure-ad-domain-services-using-powershell"></a>Erstellen einer NSG für Azure AD Domain Services mithilfe von PowerShell
Diese NSG wird so konfiguriert, dass sie eingehenden Datenverkehr an den für Azure AD Domain Services erforderlichen Ports zulässt und andere unerwünschte Zugriffe in eingehender Richtung verweigert.

**Voraussetzung: Installieren und Konfigurieren von Azure PowerShell:** Befolgen Sie die Anweisungen zum [Installieren des Azure PowerShell-Moduls und Herstellen einer Verbindung mit Ihrem Azure-Abonnement](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

>[!TIP]
> Es wird empfohlen, die neueste Version des Azure PowerShell-Moduls zu verwenden. Wenn bereits eine ältere Version des Azure PowerShell-Moduls installiert ist, aktualisieren Sie sie auf die aktuelle Version.
>

Führen Sie die folgenden Schritte aus, um mit PowerShell eine neue NSG zu erstellen: 
1. Melden Sie sich bei Ihrem Azure-Abonnement an.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

2. Erstellen Sie eine NSG mit drei Regeln. Das folgende Skript definiert drei Regeln für die NSG, die den Zugriff auf die Ports zulässt, die für die Ausführung von Azure AD Domain Services erforderlich sind. Anschließend erstellt das Skript eine neue NSG, die diese Regeln enthält. Verwenden Sie das gleiche Format zum Hinzufügen weiterer Regeln, die weiteren eingehenden Datenverkehr zulassen, falls dies für die im virtuellen Netzwerk bereitgestellten Workloads erforderlich ist.

  ```PowerShell
  # Allow inbound HTTPS traffic to enable synchronization to your managed domain.
  $SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD `
  -Description "Allow synchronization with Azure AD" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443

  # Allow management of your domain over port 5986 (PowerShell Remoting)
  $PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting `
  -Description "Allow management of domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, `
  13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, `
  52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, `
  52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 `
  -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986

  # Allow management of your domain over port 3389 (remote desktop).
  $RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD `
  -Description "Allow management of domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389

  # Create the NSG with the 3 rules above
  $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroup -Location $Location `
  -Name "AAD-DomainServices-NSG" -SecurityRules $SyncRule,$PSRemotingRule,$RemoteDesktopRule
  ```

3. Ordnen Sie schließlich die NSG dem gewünschten VNET und Subnetz zu.

  ```PowerShell
  # Find vnet and subnet
  $Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
  $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

  # Set the nsg to the subnet and save the changes
  $Subnet.NetworkSecurityGroup = $Nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
  ```

## <a name="full-script-to-create-and-apply-an-nsg-for-azure-ad-domain-services"></a>Vollständiges Skript zum Erstellen und Anwenden einer NSG für Azure AD Domain Services
>[!TIP]
> Es wird empfohlen, die neueste Version des Azure PowerShell-Moduls zu verwenden. Wenn bereits eine ältere Version des Azure PowerShell-Moduls installiert ist, aktualisieren Sie sie auf die aktuelle Version.
>

```PowerShell
# Change the following values to match your deployment
$ResourceGroup = "ResourceGroupName"
$Location = "westus"
$VnetName = "exampleVnet"
$SubnetName = "exampleSubnet"

# Log in to your Azure subscription.
Login-AzureRmAccount

# Allow inbound HTTPS traffic to enable synchronization to your managed domain.
$SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD `
-Description "Allow synchronization with Azure AD" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

# Allow management of your domain over port 5986 (PowerShell Remoting)
$PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting `
-Description "Allow management of domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, `
13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, `
52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, `
52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 `
-SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 5986

# Allow management of your domain over port 3389 (remote desktop).
$RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD `
-Description "Allow management of domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

# Create the NSG with the 3 rules above
$Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroup -Location $Location `
-Name "AAD-DomainServices-NSG" -SecurityRules $SyncRule,$PSRemotingRule,$RemoteDesktopRule

# Find vnet and subnet
$Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

# Set the nsg to the subnet and save the changes
$Subnet.NetworkSecurityGroup = $Nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
```

> [!NOTE]
> Diese Standard-NSG sperrt nicht den Zugriff auf den Port, der für Secure LDAP verwendet wird. Wenn Sie den Secure LDAP-Zugriff über das Internet sperren möchten, lesen Sie die Informationen [in diesem Artikel](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="need-help"></a>Sie brauchen Hilfe?
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).
