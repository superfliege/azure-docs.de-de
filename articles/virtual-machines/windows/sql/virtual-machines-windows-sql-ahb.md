---
title: Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure | Microsoft-Dokumentation
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
ms.openlocfilehash: ff1281a249abf456176cffe2b02ef3c63b718d5a
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767995"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure
In diesem Artikel wird beschrieben, wie Sie das Lizenzierungsmodell für einen virtuellen SQL Server-Computer in Azure ändern, indem Sie den neuen SQL-VM-Ressourcenanbieter **Microsoft.SqlVirtualMachine** verwenden. Es gibt zwei Lizenzierungsmodelle für einen virtuellen Computer (VM), der SQL Server hostet: nutzungsbasierte Bezahlung und Bring Your Own License (BYOL). Mit PowerShell oder der Azure-Befehlszeilenschnittstelle können Sie jetzt ändern, welches Lizenzierungsmodell von Ihrer SQL Server-VM genutzt wird. 

**Nutzungsbasierte Bezahlung** bedeutet, dass die sekundenbasierten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz enthalten.

Das Modell **Bring Your Own License** (BYOL) wird auch als [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) bezeichnet und ermöglicht Ihnen die Verwendung Ihrer eigenen SQL Server-Lizenz mit einer VM, auf der SQL Server ausgeführt wird. Weitere Informationen zu Preisen finden Sie unter [Preisinformationen für virtuelle Azure-Computer mit SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

Beim Umschalten zwischen den beiden Lizenzierungsmodellen fällt **keine Ausfallzeit** an, die VM wird nicht neu gestartet, es ergeben sich **keine weiteren Kosten** (sondern durch das Aktivieren des Azure-Hybridvorteils werden die Kosten *reduziert*), und das andere Modell ist **sofort wirksam**. 

  >[!NOTE]
  > - Die Möglichkeit, das Lizenzierungsmodell umzuwandeln, ist derzeit nur verfügbar, wenn Sie mit einem SQL Server-VM-Image mit nutzungsbasierter Bezahlung beginnen. Wenn Sie mit einem Bring-Your-Own-License-Image aus dem Portal beginnen, können Sie dieses Image nicht in den nutzungsbasierten Bezahlmodus umwandeln. 
  > - CSP-Kunden können den AHB-Vorteil nutzen, indem Sie zuerst eine VM mit nutzungsbasierter Bezahlung bereitstellen und diese dann in Bring-Your-Own-License umwandeln. 

## <a name="prerequisites"></a>Voraussetzungen
Für die Verwendung des SQL-VM-Ressourcenanbieters ist die SQL-IaaS-Erweiterung erforderlich. Um den SQL-VM-Ressourcenanbieter verwenden zu können, benötigen Sie daher Folgendes:
- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) mit installierter [SQL-IaaS-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension). 


## <a name="register-existing-sql-server-vm-with-sql-resource-provider"></a>Registrieren der vorhandenen SQL Server-VM beim SQL-Ressourcenanbieter
Die Möglichkeit zum Umschalten zwischen den Lizenzierungsmodellen ist ein Feature, das vom neuen SQL-VM-Ressourcenanbieter (Microsoft.SqlVirtualMachine) bereitgestellt wird. SQL Server-VMs, die nach Dezember 2018 bereitgestellt werden, werden automatisch beim neuen Ressourcenanbieter registriert. Vorhandene virtuelle Computer, die vor diesem Datum bereitgestellt wurden, müssen jedoch manuell beim Ressourcenanbieter registriert werden, bevor das Lizenzierungsmodell gewechselt werden kann. 

  > [!NOTE] 
  > Wenn Sie Ihre SQL-VM-Ressource verwerfen, gelangen Sie zurück zur hartcodierten Lizenzeinstellung des Images. 

### <a name="register-sql-resource-provider-with-your-subscription"></a>Registrieren des SQL-Ressourcenanbieters bei Ihrem Abonnement 

Um Ihre SQL Server-VM beim SQL-Ressourcenanbieter zu registrieren, müssen Sie den Ressourcenanbieter bei Ihrem Abonnement registrieren. Sie können dazu PowerShell oder das Azure-Portal verwenden. 

#### <a name="using-powershell"></a>Verwenden von PowerShell
Der folgende Codeausschnitt registriert den SQL-Ressourcenanbieter bei Ihrem Azure-Abonnement. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

#### <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Die folgenden Schritte registriert den SQL-Ressourcenanbieter bei Ihrem Azure-Abonnement mithilfe des Azure-Portals. 

1. Öffnen Sie das Azure-Portal, und navigieren Sie zu **Alle Dienste**. 
1. Navigieren Sie zu **Abonnements**, und wählen Sie das gewünschte Abonnement aus.  
1. Navigieren Sie auf dem Blatt **Abonnements** zu **Ressourcenanbieter**. 
1. Geben Sie im Filter `sql` ein, um die SQL-bezogenen Ressourcenanbieter aufzurufen. 
1. Wählen Sie je nach gewünschter Aktion für den Anbieter **Microsoft.SqlVirtualMachine** entweder *Registrieren*, *Erneut registrieren* oder *Registrierung aufheben*. 

  ![Ändern des Anbieters](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>Registrieren der SQL Server-VM beim SQL-Ressourcenanbieter
Sobald der Ressourcenanbieter bei Ihrem Abonnement registriert ist, können Sie die PowerShell verwenden, um Ihre SQL Server-VM beim SQL-Ressourcenanbieter zu registrieren. 


```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```


## <a name="use-powershell"></a>Verwenden von PowerShell 
Sie können Ihr Lizenzierungsmodell auch mit PowerShell ändern.  Vergewissern Sie sich, dass Ihre SQL Server-VM bereits beim neuen SQL-Ressourcenanbieter registriert wurde, bevor Sie das Lizenzierungsmodell wechseln. 

Mit dem folgenden Codeausschnitt wird Ihr Lizenzierungsmodell von der nutzungsbasierten Bezahlung in BYOL (bzw. die Verwendung des Azure-Hybridvorteils) geändert: 
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzureRmResource -Force 
``` 

Mit dem folgenden Codeausschnitt wird Ihr BYOL-Modell in die nutzungsbasierte Bezahlung geändert:
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzureRmResource -Force 
```

  >[!NOTE]
  > Sie müssen den neuen SQL-VM-Ressourcenanbieter verwenden, um zwischen Lizenzen umschalten zu können. Wenn Sie versuchen, diese Befehle auszuführen, bevor Sie die SQL Server-VM für den neuen Anbieter registrieren, kann dieser Fehler auftreten: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Wenn dieser Fehler angezeigt wird, [registrieren Sie Ihre SQL Server-VM bei dem neuen Ressourcenanbieter](#register-existing-sql-server-vm-with-sql-resource-provider). 
 

## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle
Sie können die Azure CLI (Azure-Befehlszeilenschnittstelle) nutzen, um das Lizenzierungsmodell zu wechseln.  Vergewissern Sie sich, dass Ihre SQL Server-VM bereits beim neuen SQL-Ressourcenanbieter registriert wurde, bevor Sie das Lizenzierungsmodell wechseln. 

Mit dem folgenden Codeausschnitt wird Ihr Lizenzierungsmodell von der nutzungsbasierten Bezahlung in BYOL (bzw. die Verwendung des Azure-Hybridvorteils) geändert:
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

Mit dem folgenden Codeausschnitt wird Ihr BYOL-Modell in die nutzungsbasierte Bezahlung geändert: 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >Sie müssen den neuen SQL-VM-Ressourcenanbieter verwenden, um zwischen Lizenzen umschalten zu können. Wenn Sie versuchen, diese Befehle auszuführen, bevor Sie die SQL Server-VM für den neuen Anbieter registrieren, kann dieser Fehler auftreten: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Wenn dieser Fehler angezeigt wird, [registrieren Sie Ihre SQL Server-VM bei dem neuen Ressourcenanbieter](#register-existing-sql-server-vm-with-sql-resource-provider). 

## <a name="view-current-licensing"></a>Anzeigen der aktuellen Lizenzierung 

Mit dem folgenden Codeausschnitt können Sie das aktuelle Lizenzierungsmodell für Ihre SQL Server-VM anzeigen. 

```PowerShell
# View current licensing model for your SQL Server VM
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="known-errors"></a>Bekannte Fehler

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>Auf dem virtuellen Computer ist die SQL-IaaS-Erweiterung nicht installiert.
Die SQL-IaaS-Erweiterung ist eine notwendige Voraussetzung für die Registrierung der SQL Server-VM beim SQL-VM-Ressourcenanbieter. Wenn Sie versuchen, die SQL Server-VM zu registrieren, bevor Sie die SQL-IaaS-Erweiterung installiert haben, tritt der folgende Fehler auf:

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

Um dieses Problem zu beheben, installieren Sie die SQL-IaaS-Erweiterung, bevor Sie die SQL Server-VM registrieren. 

  > [!NOTE]
  > Beim Installieren der SQL-IaaS-Erweiterung wird der SQL Server-Dienst neu gestartet, daher sollte dies nur während eines Wartungsfensters erfolgen. Weitere Informationen finden Sie in der Beschreibung der [Installation der SQL-IaaS-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Argument für „Sku“-Parameter kann nicht überprüft werden.
Dieser Fehler kann auftreten, wenn Sie versuchen, das Lizenzierungsmodell für Ihre SQL Server-VM bei Verwendung von Azure PowerShell in einer Version über 4.0 zu ändern:

`Set-AzureRmResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Um diesen Fehler zu beheben, heben Sie die Auskommentierung dieser Zeilen im zuvor genannten PowerShell-Codeausschnitt auf, wenn Sie das Lizenzierungsmodell wechseln: 
```PowerShell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Verwenden Sie den folgenden Code, um Ihre Azure PowerShell-Version zu überprüfen:

```PowerShell
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für Azure-VMs mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)


