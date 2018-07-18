---
title: 'Schnellstart: Erstellen eines Azure Analysis Services-Servers mithilfe von PowerShell | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mithilfe von PowerShell einen Azure Analysis Services-Server erstellen.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9149f15d0503b9a39ac67d9c3f6fc1ddde7e03bd
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952765"
---
# <a name="quickstart-create-a-server---powershell"></a>Schnellstart: Erstellen eines Servers mit PowerShell

In diesem Schnellstart erfahren Sie, wie Sie mithilfe von PowerShell über die Befehlszeile einen Azure Analysis Services-Server in Ihrem Azure-Abonnement erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Besuchen Sie die Webseite [Kostenlose Azure-Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/), und erstellen Sie ein Konto.
- **Azure Active Directory:** Ihr Abonnement muss einem Azure Active Directory-Mandanten zugeordnet sein, und Sie müssen über ein Konto in diesem Verzeichnis verfügen. Weitere Informationen finden Sie unter [Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md).
- **Azure PowerShell-Modul, Version 4.0 oder höher**. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Informationen zum Installieren oder Upgraden finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="import-azurermanalysisservices-module"></a>Importieren des Moduls „AzureRm.AnalysisServices“

Für die Servererstellung in Ihrem Abonnement verwenden Sie das Komponentenmodul [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Laden Sie das Modul „AzureRm.AnalysisServices“ in Ihre PowerShell-Sitzung.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mithilfe des Befehls [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) bei Ihrem Azure-Abonnement an. Befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Wenn Sie Ihren Server erstellen, müssen Sie eine Ressourcengruppe in Ihrem Abonnement angeben. Falls Sie noch nicht über eine Ressourcengruppe verfügen, können Sie mithilfe des Befehls [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine neue Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` in der Region „USA, Westen“ erstellt.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Erstellen eines Servers

Erstellen Sie mithilfe des Befehls [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) einen neuen Server. Im folgenden Beispiel wird ein Server namens „myServer“ in „myResourceGroup“ in der Region „USA, Westen“ zum Tarif „D1“ (kostenlos) erstellt und philipc@adventureworks.com als Serveradministrator angegeben.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Mithilfe des Befehls [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) können Sie den Server aus Ihrem Abonnement entfernen. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials aus dieser Sammlung fortfahren möchten, entfernen Sie den Server nicht. Im folgenden Beispiel wird der im vorherigen Schritt erstellte Server entfernt.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie einen Server in Ihrem Azure-Abonnement mithilfe von PowerShell erstellen. Da Sie nun über einen Server verfügen, können Sie durch Konfigurieren einer (optionalen) Serverfirewall zur Sicherheit beitragen. Außerdem können Sie Ihrem Server direkt über das Portal ein einfaches Beispieldatenmodell hinzufügen. Ein Beispielmodell ist hilfreich, wenn Sie sich mit dem Konfigurieren von Modelldatenbankrollen und dem Testen von Clientverbindungen vertraut machen. Fahren Sie mit dem Tutorial zum Hinzufügen eines Beispielmodells fort, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Schnellstart: Konfigurieren einer Serverfirewall – Azure-Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Tutorial: Tutorial: Hinzufügen eines Beispielmodells](analysis-services-create-sample-model.md)