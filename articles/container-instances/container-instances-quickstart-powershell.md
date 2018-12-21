---
title: 'Schnellstart: Ausführen einer Anwendung in Azure Container Instances – PowerShell'
description: In dieser Schnellstartanleitung stellen Sie mit Azure PowerShell eine Docker-Containeranwendung in Azure Container Instances bereit.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b17cca7f0c00aba260b97b29345ff33156a50138
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183946"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-azure-powershell"></a>Schnellstart: Ausführen einer Containeranwendung in Azure Container Instances mit Azure PowerShell

Führen Sie mithilfe von Azure Container Instances Docker-Container schnell und einfach in Azure aus. Sie müssen keine virtuellen Computer bereitstellen und keine vollständige Containerorchestrierungsplattform wie Kubernetes verwenden. In dieser Schnellstartanleitung erstellen Sie mithilfe des Azure-Portals einen Windows-Container in Azure und machen seine Anwendung mit einem vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) verfügbar. Einige Sekunden nach dem Ausführen eines einzigen Bereitstellungsbefehls können Sie zur ausgeführten Anwendung navigieren:

![In Azure Container Instances bereitgestellte App im Browser][qs-powershell-01]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.5 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Azure Container Instances muss wie alle Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.

Erstellen Sie zunächst mit dem folgenden [New-AzureRmResourceGroup][New-AzureRmResourceGroup]-Befehl eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*:

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Erstellen eines Containers

Sie besitzen eine Ressourcengruppe und können nun einen Container in Azure ausführen. Geben Sie zum Erstellen einer Containerinstanz mit Azure PowerShell einen Ressourcengruppennamen, einen Containerinstanznamen und ein Docker-Containerimage im Cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup] an. Sie können Ihre Container über das Internet verfügbar machen, indem Sie mindestens einen zu öffnenden Port und/oder eine DNS-Namensbezeichnung angeben. In dieser Schnellstartanleitung stellen Sie einen Container mit einer DNS-Namensbezeichnung bereit, der eine per Nano Server ausgeführte Instanz von Internetinformationsdienste (IIS) hostet.

Führen Sie den folgenden Befehl aus, um eine Containerinstanz zu starten. Der Wert `-DnsNameLabel` muss innerhalb der Azure-Region, in der Sie die Instanz erstellen, eindeutig sein. Falls die Fehlermeldung „DNS-Namensbezeichnung ist nicht verfügbar.“ angezeigt wird, sollten Sie eine andere DNS-Namensbezeichnung verwenden.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Innerhalb weniger Sekunden sollten Sie eine Antwort von Azure erhalten. Als `ProvisioningState` des Containers wird anfänglich **Erstellung** angezeigt, aber dies sollte sich nach ein oder zwei Minuten in **Erfolgreich** ändern. Überprüfen Sie den Bereitstellungsstatus mit dem Cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Der Bereitstellungsstatus, der vollqualifizierte Domänenname (Fully Qualified Domain Name, FQDN) und die IP-Adresse des Containers werden in der Cmdlet-Ausgabe angezeigt:

```console
PS Azure:\> Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Wenn `ProvisioningState` für den Container **Erfolgreich** lautet, können Sie im Browser zu `Fqdn` navigieren. Der Vorgang war erfolgreich, wenn in etwa die folgende Webseite angezeigt wird. Sie haben eine in einem Docker-Container ausgeführte Anwendung in Azure bereitgestellt.

![Mit Azure Container Instances bereitgestellte IIS-Instanz im Browser][qs-powershell-01]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Container nicht mehr benötigen, entfernen Sie ihn mithilfe des Cmdlets [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure-Containerinstanz auf der Grundlage eines Images in der öffentlichen Docker Hub-Registrierung erstellt. Fahren Sie mit dem Azure Container Instances-Tutorial fort, wenn Sie ein Containerimage erstellen und über eine private Azure-Containerregistrierung bereitstellen möchten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
