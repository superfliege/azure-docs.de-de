---
title: 'Schnellstart: Erstellen Ihres ersten Azure Container Instances-Containers mit PowerShell'
description: Beginnen Sie mit der Nutzung von Azure Container Instances, indem Sie eine Windows-Containerinstanz mit PowerShell erstellen.
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: marsma
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 0cc6612a91532774a2645676e36f617ddc5de12c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="create-your-first-container-in-azure-container-instances"></a>Erstellen Ihres ersten Containers in Azure Container Instances

Azure Container Instances erleichtert die Erstellung und Verwaltung von Docker-Containern in Azure, ohne dass Sie virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen.

In dieser Schnellstartanleitung erstellen Sie einen Windows-Container in Azure und machen ihn mit einer öffentlichen IP-Adresse über das Internet verfügbar. Dieser Vorgang wird mit einem einzelnen Befehl durchgeführt. Nach wenigen Augenblicken wird in Ihrem Browser Folgendes angezeigt:

![Mit Azure Container Instances bereitgestellte App im Browser][qs-powershell-01]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diese Schnellstartanleitung ist das Azure PowerShell-Modul, Version 4.4 oder höher, erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Erstellen eines Containers

Zum Erstellen eines Containers müssen Sie einen Namen, ein Docker-Image und eine Azure-Ressourcengruppe angeben. Der Container kann außerdem mit einer öffentlichen IP-Adresse über das Internet verfügbar gemacht werden, dies ist jedoch optional. Wir verwenden in diesem Fall einen Windows Nano Server-Container mit Internetinformationsdienste (Internet Information Services, IIS).

```powershell
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Innerhalb weniger Sekunden erhalten Sie eine Antwort auf Ihre Anforderung. Der Container befindet sich zunächst im Status **Wird erstellt**, wird aber in ein bis zwei Minuten gestartet. Sie können den Status mit dem Cmdlet `Get-AzureRmContainerGroup` überprüfen:

```powershell
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Der Bereitstellungsstatus und die IP-Adresse des Containers werden in der Cmdlet-Ausgabe angezeigt:

```
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/12345678-1234-1234-1234-12345678abcd/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            :
IpAddress                : 40.71.248.73
Ports                    : {80}
OsType                   : Windows
Volumes                  :
```

Sobald **ProvisioningState** für den Container `Succeeded` lautet, ist er über den Browser unter der angegebenen IP-Adresse erreichbar.

![Mit Azure Container Instances bereitgestellte IIS-Instanz im Browser][qs-powershell-01]

## <a name="delete-the-container"></a>Löschen des Containers

Wenn Sie den Container nicht mehr benötigen, können Sie ihn mithilfe des Cmdlets `Remove-AzureRmContainerGroup` entfernen:

```powershell
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen vorgefertigten Windows-Container in Azure Container Instances gestartet. Wenn Sie selbst einen Container erstellen und über Azure Container Registry in Azure Container Instances bereitstellen möchten, fahren Sie mit dem Azure Container Instances-Tutorial fort.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png
