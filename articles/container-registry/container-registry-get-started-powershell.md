---
title: 'Schnellstartanleitung: Erstellen einer privaten Docker-Registrierung in Azure mit PowerShell'
description: Hier lernen Sie, wie Sie schnell eine private Docker-Containerregistrierung mit PowerShell erstellen.
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ae21fc7ab016071d075324bf813243cef58dcd04
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>Schnellstart: Erstellen einer Azure Container Registry-Instanz mit PowerShell

Azure Container Registry ist ein verwalteter Docker-Containerregistrierungsdienst zum Speichern privater Docker-Containerimages. Diese Anleitung enthält ausführliche Informationen zum Erstellen einer Azure Container Registry-Instanz mit PowerShell, Übertragen eines Containerimages per Pushvorgang in die Registrierung und Bereitstellen des Containers aus Ihrer Registrierung in Azure Container Instances (ACI).

Für diese Schnellstartanleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Darüber hinaus muss Docker lokal installiert sein. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac-][docker-mac], [Windows-][docker-windows] oder [Linux-][docker-linux]System konfiguriert werden kann.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Erstellen Sie mit dem Befehl [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry) eine ACR-Instanz.

Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Im folgenden Beispiel wird der Name *myContainerRegistry007* verwendet. Ersetzen Sie diesen Namen durch einen eindeutigen Wert.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Anmelden bei ACR

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der ACR-Instanz anmelden. Verwenden Sie zunächst den Befehl [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential), um die Administratoranmeldeinformationen für die ACR-Instanz zu erhalten.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Verwenden Sie anschließend den Befehl [docker login][docker-login], um sich bei der ACR-Instanz anzumelden.

```powershell
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Der Befehl gibt nach Abschluss des Vorgangs `Login Succeeded` zurück. Möglicherweise wird auch eine Sicherheitswarnung angezeigt, in der die Verwendung des `--password-stdin`-Parameters empfohlen wird. Obwohl in diesem Artikel nicht auf dessen Verwendung eingegangen werden kann, wird empfohlen, diese bewährte Methode anzuwenden. Weitere Informationen zum Befehl [docker login][docker-login] finden Sie in der entsprechenden Referenzdokumentation.

## <a name="push-image-to-acr"></a>Übertragen eines Images an ACR mithilfe von Push

Um ein Image mithilfe von Push an Ihre Azure Container Registry-Instanz übertragen zu können, benötigen Sie zunächst ein Image. Führen Sie bei Bedarf den folgenden Befehl aus, um ein vorab erstelltes Image von Docker Hub abzurufen:

```powershell
docker pull microsoft/aci-helloworld
```

Das Image muss mit dem ACR-Anmeldeservernamen gekennzeichnet sein. Verwenden Sie hierfür den Befehl [docker tag][docker-tag].

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
docker tag microsoft/aci-helloworld $image
```

Verwenden Sie abschließend [docker push][docker-push], um das Image per Pushvorgang an die ACR zu übertragen.

```powershell
docker push $image
```

## <a name="deploy-image-to-aci"></a>Bereitstellen eines Images für ACI
Konvertieren Sie die Anmeldeinformationen für die Registrierung zuerst in ein PSCredential-Objekt, um das Image als Containerinstanz in Azure Container Instances (ACI) bereitzustellen.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Führen Sie den folgenden Befehl aus, um das Containerimage aus der Containerregistrierung mit einem CPU-Kern und 1 GB Arbeitsspeicher bereitzustellen:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name mycontainer -Image $image -Cpu 1 -MemoryInGB 1 -IpAddressType public -Port 80 -RegistryCredential $pscred
```

Sie sollten von Azure Resource Manager eine erste Antwort mit Details zu Ihrem Container erhalten. Wiederholen Sie den Befehl [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup], um den Status Ihres Containers zu überwachen und seine Ausführung zu verfolgen. Dieser Vorgang dauert normalerweise weniger als eine Minute.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Beispielausgabe: `Succeeded`

## <a name="view-the-application"></a>Anzeigen der Anwendung
Nachdem die Bereitstellung für ACI erfolgreich abgeschlossen wurde, können Sie die öffentliche IP-Adresse des Containers mit dem Befehl [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] abrufen:

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).IpAddress
```

Beispielausgabe: `"13.72.74.222"`

Zum Anzeigen der ausgeführten Anwendung navigieren Sie in Ihrem bevorzugten Browser zur öffentlichen IP-Adresse. Das sollte in etwa so aussehen:

![„Hello World“-Anwendung im Browser][qs-portal-15]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe, die Azure Container Registry und alle Azure Container Instances nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure Container Registry mit der Azure CLI erstellt und eine Instanz davon in Azure Container Instances gestartet. Fahren Sie mit dem Azure Container Instances-Tutorial fort, um eingehendere Informationen zu ACI zu erhalten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png
