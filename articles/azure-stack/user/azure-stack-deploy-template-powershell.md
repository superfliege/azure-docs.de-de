---
title: Bereitstellen von Vorlagen mithilfe von PowerShell in Azure Stack | Microsoft-Dokumentation
description: Stellen Sie eine Vorlage in Azure Stack mithilfe von PowerShell bereit.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 4af82deef029120aa2699e7c69c501ae61a1e8bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359814"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Bereitstellen einer Vorlage in Azure Stack mithilfe von PowerShell

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können PowerShell zum Bereitstellen von Azure Resource Manager-Vorlagen (ARM) für Azure Stack verwenden. In diesem Artikel wird das Verwenden von PowerShell zum Bereitstellen einer Vorlage veranschaulicht.

## <a name="run-azurerm-powershell-cmdlets"></a>Ausführen von AzureRM PowerShell-Cmdlets

In diesem Beispiel werden AzureRM-PowerShell-Cmdlets und eine auf GitHub gespeicherte Vorlage verwendet. Die Vorlage erstellt einen virtuelle Windows Server 2012 R2 Datacenter-Computer.

>[!NOTE]
>Bevor Sie dieses Beispiel ausprobieren, stellen Sie sicher, dass Sie [PowerShell für einen Azure Stack-Benutzer konfiguriert](azure-stack-powershell-configure-user.md) haben.

1. Wechseln Sie zu „<http://aka.ms/AzureStackGitHub>“, und suchen Sie die Vorlage **101-simple-windows-vm**. Speichern Sie die Vorlage hier: C:\\templates\\azuredeploy-101-simple-windows-vm.json.
2. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.
3. Ersetzen Sie *username* und *password* im folgenden Skript durch Ihren Benutzernamen und Ihr Kennwort, und führen Sie das Skript aus.

   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```

   >[!IMPORTANT]
   >Erhöhen Sie bei jeder Skriptausführung den Wert für den „$myNum“-Parameter, um zu verhindern, dass Ihre Bereitstellung überschrieben wird.

4. Öffnen Sie das Azure Stack-Portal, wählen Sie **Durchsuchen**, und wählen Sie dann **Virtuelle Computer**, um nach dem neuen virtuellen Computer (*myDeployment001*) zu suchen.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mithilfe von Visual Studio](azure-stack-deploy-template-visual-studio.md)
