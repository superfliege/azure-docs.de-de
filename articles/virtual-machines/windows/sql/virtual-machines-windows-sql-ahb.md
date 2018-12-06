---
title: Ändern des Lizenzierungsmodells für eine SQL-VM in Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Lizenzierung für einen virtuellen SQL-Computer in Azure wechseln.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/14/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 3bcbfc876ed27d16180ca03801f2054ad804e148
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52577060"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-virtual-machine-in-azure"></a>Ändern des Lizenzierungsmodells für einen virtuellen SQL-Computer in Azure
In diesem Artikel wird beschrieben, wie Sie das Lizenzierungsmodell für einen virtuellen SQL Server-Computer in Azure ändern, indem Sie den neuen SQL-Ressourcenanbieter **Microsoft.SqlVirtualMachine** verwenden. Es gibt zwei Lizenzierungsmodelle für einen virtuellen Computer (VM), der SQL Server hostet: nutzungsbasierte Bezahlung und Bring Your Own License (BYOL). Und mit PowerShell oder der Azure CLI können Sie jetzt ändern, welches Lizenzierungsmodell von Ihrer SQL-VM genutzt wird. 

**Nutzungsbasierte Bezahlung** bedeutet, dass die sekundenbasierten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz enthalten.

Das Modell **Bring Your Own License** wird auch als [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) bezeichnet und ermöglicht Ihnen die Verwendung Ihrer eigenen SQL Server-Lizenz mit einer VM, auf der SQL Server ausgeführt wird. Weitere Informationen zu Preisen finden Sie unter [Preisinformationen für virtuelle Azure-Computer mit SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

Beim Umschalten zwischen den beiden Lizenzierungsmodellen fällt **keine Ausfallzeit** an, die VM wird nicht neu gestartet, es ergeben sich **keine weiteren Kosten** (sondern durch das Aktivieren des Azure-Hybridvorteils werden die Kosten reduziert) und das andere Modell ist **sofort wirksam**. 


## <a name="register-legacy-sql-vm-with-new-resource-provider"></a>Registrieren des virtuellen SQL-Legacycomputers beim neuen Ressourcenanbieter
Die Möglichkeit zum Umschalten zwischen den Lizenzierungsmodellen ist ein Feature, das vom neuen SQL-VM-Ressourcenanbieter (Microsoft.SqlVirtualMachine) bereitgestellt wird. Derzeit müssen Sie zum Umschalten Ihres Lizenzierungsmodells zuerst den neuen Anbieter für Ihr Abonnement registrieren und anschließend Ihren virtuellen Legacycomputer beim neuen SQL-VM-Ressourcenanbieter registrieren. 

  >[!IMPORTANT]
  > Wenn Sie Ihre SQL-VM-Ressource verwerfen, gelangen Sie zurück zur hartcodierten Lizenzeinstellung des Images. 

### <a name="powershell"></a>PowerShell

Mit dem folgenden Code wird zuerst der neue SQL-Ressourcenanbieter für Ihr Abonnement registriert, und anschließend wird Ihr virtueller SQL-Legacycomputer beim neuen Ressourcenanbieter registriert. 

```powershell
# Connect to Azure
Connect-AzureRmAccount
Account: <account_name>

# Verify your subscription ID
Get-AzureRmContext

# Set the correct Azure Subscription ID
Set-AzureRmContext -SubscriptionId <Subscription_ID>

# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine

# Register your legacy SQL VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

### <a name="portal"></a>Portal
Sie können den neuen SQL-VM-Ressourcenanbieter auch über das Portal registrieren. Führen Sie hierzu die folgenden Schritte aus:
1. Öffnen Sie das Azure-Portal, und navigieren Sie zu **Alle Dienste**. 
1. Navigieren Sie zu **Abonnements**, und wählen Sie das gewünschte Abonnement aus.  

  ![Wählen Sie Ihr Abonnement aus.](media/virtual-machines-windows-sql-ahb/open-subscriptions.png)

1. Navigieren Sie auf dem Blatt **Abonnements** zu **Ressourcenanbieter**. 
1. Geben Sie im Filter `sql` ein, um die SQL-bezogenen Ressourcenanbieter aufzurufen. 
1. Wählen Sie je nach gewünschter Aktion für den Anbieter **Microsoft.SqlVirtualMachine** entweder *Registrieren*, *Erneut registrieren* oder *Registrierung aufheben*. 

  ![Ändern des Anbieters](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


## <a name="use-powershell"></a>Verwenden von PowerShell 
Sie können PowerShell verwenden, um Ihr Lizenzierungsmodell zu ändern.  Vergewissern Sie sich, dass Ihre SQL-VM bereits beim neuen SQL-Ressourcenanbieter registriert wurde, bevor Sie das Lizenzierungsmodell wechseln. 

Mit diesem Codeausschnitt wird Ihr Lizenzierungsmodell für nutzungsbasierte Bezahlung in BYOL (bzw. die Verwendung des Azure-Hybridvorteils) geändert: 
```powershell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
$SqlVm | Set-AzureRmResource -Force 
``` 

Mit diesem Codeausschnitt wird Ihr BYOL-Modell in die nutzungsbasierte Bezahlung geändert:
```powershell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
$SqlVm | Set-AzureRmResource -Force 
```

  >[!NOTE]
  > Sie müssen den neuen SQL-VM-Ressourcenanbieter verwenden, um zwischen Lizenzen umschalten zu können. Wenn Sie versuchen, diese Befehle vor dem Registrieren Ihrer SQL-VM für den neuen Anbieter auszuführen, tritt ggf. dieser Fehler auf: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` [Registrieren Sie Ihre SQL-VM beim neuen Ressourcenanbieter](#register-legacy-SQL-vm-with-new-resource-provider), wenn dieser Fehler angezeigt wird. 
 

## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle
Sie können die Azure CLI (Azure-Befehlszeilenschnittstelle) nutzen, um das Lizenzierungsmodell zu wechseln.  Vergewissern Sie sich, dass Ihre SQL-VM bereits beim neuen SQL-Ressourcenanbieter registriert wurde, bevor Sie das Lizenzierungsmodell wechseln. 

Mit diesem Codeausschnitt wird Ihr Lizenzierungsmodell für nutzungsbasierte Bezahlung in BYOL (bzw. die Verwendung des Azure-Hybridvorteils) geändert:
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

Mit diesem Codeausschnitt wird Ihr BYOL-Modell in die nutzungsbasierte Bezahlung geändert: 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >Sie müssen den neuen SQL-VM-Ressourcenanbieter verwenden, um zwischen Lizenzen umschalten zu können. Wenn Sie versuchen, diese Befehle vor dem Registrieren Ihrer SQL-VM für den neuen Anbieter auszuführen, tritt ggf. dieser Fehler auf: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` [Registrieren Sie Ihre SQL-VM beim neuen Ressourcenanbieter](#register-legacy-SQL-vm-with-new-resource-provider), wenn dieser Fehler angezeigt wird. 

## <a name="view-current-licensing"></a>Anzeigen der aktuellen Lizenzierung 

Mit dem folgenden Codeausschnitt können Sie das aktuelle Lizenzierungsmodell für Ihre SQL-VM anzeigen. 

```powershell
# View current licensing model for your SQL VM
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für virtuelle Azure-Computer mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)


