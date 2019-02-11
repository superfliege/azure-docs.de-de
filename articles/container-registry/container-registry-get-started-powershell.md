---
title: 'Schnellstart: Erstellen einer privaten Docker-Registrierung in Azure – PowerShell'
description: Hier lernen Sie, wie Sie schnell eine private Docker-Containerregistrierung in Azure mit PowerShell erstellen können.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b8ff8e671d51a148177e66b30225dd7536a48028
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299742"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Schnellstart: Erstellen einer privaten Containerregistrierung mit Azure PowerShell

Azure Container Registry ist ein verwalteter privater Docker-Containerregistrierungsdienst zum Erstellen, Speichern und Bereitstellen von Docker-Containerimages. In diesem Schnellstart erfahren Sie, wie eine Azure Container Registry mit PowerShell erstellt wird. Übertragen Sie anschließend mithilfe von Docker-Befehlen ein Containerimage per Push in die Registrierung. Rufen Sie abschließend das Image per Pull aus der Registrierung ab, und führen Sie es aus.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart ist das Azure PowerShell-Modul Version 5.7.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um Ihre installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Darüber hinaus muss Docker lokal installiert sein. Docker stellt Pakete für [macOS][docker-mac], [Windows][docker-windows] und [Linux][docker-linux] Systeme bereit.

Da Azure Cloud Shell nicht alle erforderlichen Docker-Komponenten (z.B. den `dockerd`-Daemon) enthält, können Sie Cloud Shell für diesem Schnellstart nicht verwenden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl [Connect-AzureRmAccount][Connect-AzureRmAccount] bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Sobald die Authentifizierung mit Azure erfolgt ist, erstellen Sie mit [New-AzureRmResourceGroup][New-AzureRmResourceGroup] eine Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Sie Azure-Ressourcen bereitstellen und verwalten.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Erstellen einer Containerregistrierung

Als nächstes erstellen Sie mit dem Befehl [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry] eine Containerregistrierung in Ihrer neuen Ressourcengruppe.

Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Das folgende Beispiel erstellt eine Registrierung mit dem Namen „myContainerRegistry007“. Ersetzen Sie *myContainerRegistry007* im folgenden Befehl, und führen Sie diesen dann zum Erstellen der Registrierung aus:

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

In dieser Schnellstartanleitung erstellen Sie eine Registrierung vom Typ *Basic*. Dabei handelt es sich um eine kostenoptimierte Option für Entwickler, die sich mit Azure Container Registry vertraut machen. Ausführliche Informationen zu verfügbaren Diensttarifen finden Sie unter [Azure Container Registry-SKUs][container-registry-skus].

## <a name="log-in-to-registry"></a>Anmelden bei der Registrierung

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der Registrierung anmelden. In Produktionsszenarien sollten Sie eine einzelne Identität oder einen Dienstprinzipal für den Zugriff auf die Containerregistrierung verwenden. Aktivieren Sie der Einfachheit halber in dieser Schnellstartanleitung den Administratorbenutzer in Ihrer Registrierung aber mit dem Befehl [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential]:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Führen Sie dann [docker login][docker-login] aus, um sich anzumelden:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Der Befehl gibt nach Abschluss des Vorgangs `Login Succeeded` zurück.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Arbeit mit den in diesem Schnellstart erstellten Ressourcen beendet haben, verwenden Sie den Befehl [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup], um die Ressourcengruppe, die Containerregistrierung und die darin gespeicherten Containerimages zu entfernen:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mit Azure PowerShell eine Azure Container Registry-Instanz erstellt, ein Containerimage per Push übertragen und das Image per Pull aus der Registrierung abgerufen und ausgeführt. Fahren Sie mit den Azure Container Registry-Tutorials fort, um eingehendere Informationen zu ACR zu erhalten.

> [!div class="nextstepaction"]
> [Tutorials zu Azure Container Registry][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md