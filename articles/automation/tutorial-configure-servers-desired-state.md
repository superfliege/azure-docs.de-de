---
title: "Konfigurieren von Servern mit einem gewünschten Status und Verwalten der Abweichung mit Azure Automation | Microsoft-Dokumentation"
description: 'Tutorial: Verwalten von Serverkonfigurationen mit Azure Automation DSC'
services: automation
documentationcenter: automation
author: eslesar
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: eslesar
ms.custom: 
ms.openlocfilehash: 63a83e35ce29541de578cb264464448fb6ee3e1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Konfigurieren von Servern mit einem gewünschten Status und Verwalten der Abweichung mit Azure Automation

Mit der Funktion „Azure Automation-Konfiguration für den gewünschten Zustand“ (Desired State Configuration, DSC) können Sie Konfigurationen für Ihre Server festlegen und sicherstellen, dass sich diese Server im Verlauf der Zeit im angegebenen Zustand befinden.



> [!div class="checklist"]
> * Integrieren eines virtuellen Computers in die Azure Automation DSC-Verwaltung
> * Hochladen einer Konfiguration in Azure Automation
> * Kompilieren einer Konfiguration in eine Knotenkonfiguration
> * Zuweisen einer Knotenkonfiguration zu einem verwalteten Knoten
> * Überprüfen des Konformitätsstatus eines verwalteten Knotens

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Lernprogramm abzuschließen, benötigen Sie:

* Ein Azure Automation-Konto. Informationen zum Erstellen eines ausführenden Azure Automation-Kontos finden Sie unter [Azure Run As Account](automation-sec-configure-azure-runas-account.md)(Ausführendes Azure-Konto).
* Eine Azure Resource Manager-VM (nicht klassisch) unter Windows Server 2008 R2 oder höher. Eine Anleitung zum Erstellen einer VM finden Sie unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
* Azure PowerShell-Modul, Version 3.6 oder höher. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.
* Vertrautheit mit DSC. Weitere Informationen zu DSC finden Sie unter [Windows PowerShell DSC – Übersicht](https://docs.microsoft.com/powershell/dsc/overview).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Erstellen und Hochladen einer Konfiguration in Azure Automation

Für dieses Tutorial verwenden wir eine einfache DSC-Konfiguration, die sicherstellt, dass IIS auf der VM installiert ist.

Weitere Informationen zu DSC-Konfigurationen finden Sie unter [DSC-Konfigurationen](https://docs.microsoft.com/powershell/dsc/configurations).

Geben Sie in einem Texteditor Folgendes ein, und speichern Sie die Datei lokal als `TestConfig.ps1`.

```powershell
configuration TestConfig {

   Node WebServer {

      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

Rufen Sie das Cmdlet `Import-AzureRmAutomationDscConfiguration` auf, um die Konfiguration in Ihr Automation-Konto hochzuladen:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Kompilieren einer Konfiguration in eine Knotenkonfiguration

Eine DSC-Konfiguration muss in eine Knotenkonfiguration kompiliert werden, bevor sie einem Knoten zugewiesen werden kann.

Informationen zum Kompilieren von Konfigurationen finden Sie unter [DSC-Konfigurationen](https://docs.microsoft.com/powershell/dsc/configurations).

Rufen Sie das Cmdlet `Start-AzureRmAutomationDscCompilationJob` auf, um die `TestConfig`-Konfiguration in eine Knotenkonfiguration zu kompilieren:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Dies erstellt eine Konfiguration mit dem Namen `TestConfig.WebServer` in Ihrem Automation-Konto.

## <a name="register-a-vm-to-be-managed-by-dsc"></a>Registrieren einer VM für die Verwaltung durch DSC

Mit Azure Automation DSC können Sie Azure-VMs (mit dem klassischen oder Resource Manager-Modell), lokale VMs, Linux-Computer, AWS-VMs und lokale physische Computer verwalten. In diesem Thema wird das Registrieren von ausschließlich Azure Resource Manager-VMs behandelt.
Informationen zum Integrieren anderer Computertypen finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).

Rufen Sie das Cmdlet `Register-AzureRmAutomationDscNode` auf, um Ihre VM bei Azure Automation DSC zu registrieren.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName "MyResourceGroup" -AutomationAccountName "myAutomationAccount" -AzureVMName "DscVm"
```

Dadurch wird die angegebene VM als DSC-Knoten in Ihrem Azure Automation-Konto registriert.

### <a name="specify-configuration-mode-settings"></a>Angeben der Konfigurationsmoduseinstellungen

Wenn Sie eine VM als verwalteten Knoten registrieren, können Sie auch Eigenschaften der Konfiguration angeben.
Sie können z.B. festlegen, dass der Zustand der VM nur einmalig angewendet werden soll (DSC versucht nicht, die Konfiguration nach der ersten Prüfung zu übernehmen), indem Sie `ApplyOnly` als Wert der **ConfigurationMode**-Eigenschaft angeben:

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationMode 'ApplyOnly'
```

Sie können auch angeben, wie oft DSC den Konfigurationszustand überprüft, indem Sie die **ConfigurationModeFrequencyMins**-Eigenschaft verwenden:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationModeFrequencyMins 60
```

Weitere Informationen zum Festlegen von Konfigurationseigenschaften für einen verwalteten Knoten finden Sie unter [Register-AzureRmAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationdscnode?view=azurermps-4.3.1&viewFallbackFrom=azurermps-4.2.0).

Weitere Informationen zu DSC-Konfigurationseinstellungen finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](https://docs.microsoft.com/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Zuweisen einer Knotenkonfiguration zu einem verwalteten Knoten

Nun können wir die kompilierte Knotenkonfiguration der zu konfigurierenden VM zuordnen.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -Id $node.Id
```

Damit wird dem registrierten DSC-Knoten `DscVm` die benannte Knotenkonfiguration `TestConfig.WebServer` zugeordnet.
Standardmäßig wird der DSC-Knoten alle 30 Minuten auf Konformität mit der Knotenkonfiguration geprüft.
Informationen zum Ändern des Intervalls für die Konformitätsprüfung finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](https://docs.microsoft.com/PowerShell/DSC/metaConfig)

## <a name="check-the-compliance-status-of-a-managed-node"></a>Überprüfen des Konformitätsstatus eines verwalteten Knotens

Sie können Berichte zum Konformitätsstatus eines DSC-Knotens abrufen, indem Sie das Cmdlet `Get-AzureRmAutomationDscNodeReport` aufrufen:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports Get-Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Id $node.Id

# Display the most recent report
$report[0]
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Anleitung zum Integrieren von Knoten, die mit Azure Automation DSC verwaltet werden sollen, finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
* Weitere Informationen zur Verwendung von Azure Automation DSC über das Azure-Portal finden Sie unter [Erste Schritte mit Azure Automation DSC](automation-dsc-getting-started.md).
* Informationen zum Kompilieren von DSC-Konfigurationen und der anschließenden Zuweisung an Zielknoten finden Sie unter [Kompilieren von Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
* Informationen zur PowerShell-Cmdlet-Referenz für Azure Automation DSC finden Sie unter [Azure Automation DSC-Cmdlets](/powershell/module/azurerm.automation/#automation).
* Preisinformationen finden Sie unter [Azure Automation DSC – Preise](https://azure.microsoft.com/pricing/details/automation/).
* Ein Beispiel zur Verwendung von Azure Automation DSC in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment für IaaS-VMs mit Azure Automation DSC und Chocolatey](automation-dsc-cd-chocolatey.md).