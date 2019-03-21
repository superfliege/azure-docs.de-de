---
title: PowerShell-Modul für Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: Sie können PowerShell verwenden, um Azure Machine Learning Studio-Arbeitsbereiche, -Experimente, -Webdienste und vieles mehr zu erstellen und zu verwalten.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 01/25/2019
ms.openlocfilehash: bd3a82f326cdf7f51e8842e45333ff2bd647c260
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58092751"
---
# <a name="powershell-modules-for-azure-machine-learning-studio"></a>PowerShell-Modul für Azure Machine Learning Studio

Mithilfe von PowerShell-Modulen können Sie Ihre Studio-Ressourcen und -Assets wie z.B. Arbeitsbereiche, Datasets und Webdienste programmgesteuert verwalten.

Sie können mithilfe von drei PowerShell-Modulen mit Studio-Ressourcen interagieren:

* [Azure PowerShell Az](#az-rm) (veröffentlicht 2018) umfasst die gesamte Funktionalität von AzureRM, allerdings mit anderen Cmdlet-Namen
* [AzureRM](#az-rm), veröffentlicht 2016
* [Azure Machine Learning PowerShell – klassisch](#classic), veröffentlicht 2016

Trotz einiger Ähnlichkeiten ist jedes dieser Module für bestimmte Szenarien konzipiert. In diesem Artikel werden die Unterschiede zwischen den PowerShell-Modulen beschrieben, und Sie erhalten Unterstützung bei der Modulauswahl.

## <a name="choosing-modules"></a> Auswählen von Modulen

Die Auswahl zwischen den verfügbaren PowerShell-Modulen hängt von der Art der Ressourcen ab, die Sie verwalten.

Überprüfen Sie die nachstehende [Unterstützungstabelle](#support-table), um zu sehen, welche Ressourcen von jedem Modul unterstützt werden. PowerShell (klassisch) kann parallel zu entweder Az oder AzureRM installiert werden. So können Sie zwei Module installieren und alle Ressourcentypen abdecken (klassisch mit Az oder klassisch mit AzureRM).

Eine gleichzeitige Installation von Az und AzureRM wird jedoch nicht empfohlen. Bei der Auswahl zwischen Az und AzureRM empfiehlt Microsoft, Az für alle zukünftigen Bereitstellungen zu verwenden. Verwenden Sie AzureRM nur dann, wenn besondere Umstände in Ihrer Umgebung dies erforderlich machen.

Weitere Informationen zu den Unterschieden zwischen Az und AzureRM sowie über den bereitgestellten Migrationspfad finden Sie unter [Einführung in Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

## <a name="az-rm"></a> Azure PowerShell Az und AzureRM

Az und AzureRM verwalten beide Lösungen, die mit dem **Azure Resource Manager**-Bereitstellungsmodell bereitgestellt werden. Zu diesen Ressourcen gehören Studio-Arbeitsbereiche und neue Studio-Webdienste. Um Ressourcen zu verwalten, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden, müssen Sie das klassische PowerShell-Modul verwenden. Weitere Informationen über die Bereitstellungsmodelle finden Sie im Artikel zum [Vergleich der Azure Resource Manager-Bereitstellung und der klassischen Bereitstellung](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Az ist nun das vorgesehene PowerShell-Modul für die Interaktion mit Azure und umfasst die gesamte Funktionalität von AzureRM. Für AzureRM werden weiterhin Bugfixes bereitgestellt, jedoch keine neuen Cmdlets oder Features. Es gibt einen Upgradepfad von AzureRM, aber falls bei der Arbeit mit Studio Probleme mit Az auftreten, melden Sie das Problem, und greifen Sie auf die Verwendung von AzureRM zurück.

Befolgen Sie zum Einstieg in Az die [Installationsanweisungen für Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="classic"></a> PowerShell – klassisch

Das [klassische PowerShell-Modul](https://aka.ms/amlps) von Studio ermöglicht Ihnen das Verwalten von Ressourcen, die mit dem **klassischen Bereitstellungsmodell** bereitgestellt wurden. Zu diesen Ressourcen zählen Studio-Benutzerassets, klassische Webdienste und klassische Webdienstendpunkte.

Microsoft empfiehlt jedoch, für alle neuen Ressourcen das Resource Manager-Bereitstellungsmodell zu verwenden, um die Bereitstellung und Verwaltung der Ressourcen zu vereinfachen. Weitere Informationen über die Bereitstellungsmodelle finden Sie im Artikel zum [Vergleich der Azure Resource Manager-Bereitstellung und der klassischen Bereitstellung](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Laden Sie zum Einstieg in PowerShell (klassisch) das [Versionspaket](https://github.com/hning86/azuremlps/releases) von GitHub herunter, und befolgen Sie die [Anweisungen für die Installation](https://github.com/hning86/azuremlps/blob/master/README.md). In den Anweisungen wird erklärt, wie Sie die Blockierung der heruntergeladenen/entzippten DLL aufheben und sie dann in Ihre PowerShell-Umgebung importieren.

## <a name="support-table"></a> PowerShell-Unterstützungstabelle

 **Studio-Arbeitsbereiche** | **Az** |  **AzureRM** | **PowerShell – klassisch** |
| --- | --- | --- | --- |
| Erstellen/Löschen von Arbeitsbereichen | [Resource Manager-Vorlagen](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) | [Resource Manager-Vorlagen](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Verwalten von Arbeitsbereichsbenutzern |  |  | [Add-AmlWorkspaceUsers](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Verwalten von Mindestabnahmeplänen | [New-AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | [New-AzureRmMlCommitmentPlan](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/new-azurermmlcommitmentplan) |
|||
| **Webdienste** | **Az** | **AzureRM** | **PowerShell – klassisch** |
| Verwalten von Webdiensten | [New-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br> (Neue Webdienste) | [New-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/new-azurermmlwebservice) <br> (Neue Webdienste) |[New-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br> (klassische Webdienste) |
| Verwalten von Endpunkten/Schlüsseln |  [Get-AzMlWebServiceKeys](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekeys) <br> (neue Webdienste) | [Get-AzureRmMlWebServiceKeys](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservicekeys) <br> (neue Webdienste) | [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint) <br> (klassische Webdienste) |
|||
| **Benutzerassets** | **Az** | **AzureRM** | **PowerShell – klassisch** |
| Verwalten von Datasets/trainierten Modellen |  |  | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Verwalten von Experimenten |  |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Verwalten von benutzerdefinierten Modulen |  |  | [New-AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Nächste Schritte
Folgen Sie den Links zur vollständige Dokumentation für die PowerShell-Module:
* [AzureRM](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/#machine_learning)
* [PowerShell – klassisch](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
