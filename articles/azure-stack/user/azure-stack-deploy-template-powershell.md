---
title: Bereitstellen von Vorlagen mithilfe von PowerShell in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Computer mit einer Resource Manager-Vorlage und PowerShell bereitstellen.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: d271b155d65a7dd95a92262da338cf3a272d140b
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Bereitstellen von Vorlagen in Azure Stack mithilfe von PowerShell

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Verwenden Sie PowerShell zum Bereitstellen von Azure Resource Manager-Vorlagen im Azure Stack Development Kit.  Resource Manager-Vorlagen stellen alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereit.

## <a name="run-azurerm-powershell-cmdlets"></a>Ausführen von AzureRM PowerShell-Cmdlets
In diesem Beispiel führen Sie ein Skript aus, um einen virtuellen Computer mithilfe einer Resource Manager-Vorlage im Azure Stack Development Kit bereitzustellen.  Bevor Sie fortfahren, stellen Sie sicher, dass [PowerShell konfiguriert](azure-stack-powershell-configure-user.md) wurde.  

Die in dieser Beispielvorlage verwendete VHD-Datei ist „WindowsServer-2012-R2-Datacenter“.

1. Wechseln Sie zu <http://aka.ms/AzureStackGitHub>, suchen Sie nach der Vorlage **101-simple-windows-vm**, und speichern Sie diese hier: C:\\templates\\azuredeploy-101-simple-windows-vm.json.
2. Führen Sie das folgende Bereitstellungsskript in PowerShell aus. Ersetzen Sie *username* und *password* durch Ihren Benutzernamen und Ihr Kennwort. Erhöhen Sie in nachfolgenden Verwendungen den Wert für den *$myNum*-Parameter, um zu verhindern, dass Ihre Bereitstellung überschrieben wird.
   
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
3. Öffnen Sie das Azure Stack-Portal, klicken Sie auf **Durchsuchen**, klicken Sie auf **virtuelle Computer**, und suchen Sie nach dem neuen virtuellen Computer (*myDeployment001*).


## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von Vorlagen mithilfe von Visual Studio](azure-stack-deploy-template-visual-studio.md)

