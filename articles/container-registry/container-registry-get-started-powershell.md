---
title: 'Schnellstart: Erstellen einer privaten Docker-Registrierung in Azure – PowerShell'
description: Hier lernen Sie, wie Sie schnell eine private Docker-Containerregistrierung in Azure mit PowerShell erstellen können.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 05/08/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: fa6b4de9282eec75747ca87b26058a47320f2fd3
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428136"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Schnellstart: Erstellen einer privaten Containerregistrierung mit Azure PowerShell

Azure Container Registry ist ein verwalteter privater Docker-Containerregistrierungsdienst zum Erstellen, Speichern und Bereitstellen von Docker-Containerimages. In diesem Schnellstart erfahren Sie, wie eine Azure Container Registry mit PowerShell erstellt wird. Nachdem Sie die Registrierung erstellt haben, pushen Sie ein Containerimage in diese und stellen den Container dann aus Ihrer Registrierung in Azure Container Instances (ACI) bereit.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart ist das Azure PowerShell-Modul Version 5.7.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um Ihre installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Darüber hinaus muss Docker lokal installiert sein. Docker stellt Pakete für [macOS][docker-mac]-, [Windows][docker-windows]- und [Linux][docker-linux]-Systeme bereit.

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

## <a name="log-in-to-registry"></a>Anmelden bei der Registrierung

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der Registrierung anmelden. Verwenden Sie den Befehl [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential], um zunächst die Administratoranmeldeinformationen für die Registrierung abzurufen:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Führen Sie dann [docker login][docker-login] aus, um sich anzumelden:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Eine erfolgreiche Anmeldung gibt `Login Succeeded` zurück:

```console
PS Azure:\> $creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
Login Succeeded
```

## <a name="push-image-to-registry"></a>Pushen eines Image in die Registrierung

Da Sie nun bei der Registrierung angemeldet sind, können Sie Containerimages in sie pushen. Um ein Image abzurufen, das Sie in Ihre Registrierung pushen können, pullen Sie das öffentliche [aci-helloworld][aci-helloworld-image]-Image vom Docker-Hub. Das [aci-helloworld][aci-helloworld-github]-Image ist eine kleine Node.js-Anwendung, die eine statische HTML-Seite bereitstellt, die das Azure Container Instances-Logo anzeigt.

```powershell
docker pull microsoft/aci-helloworld
```

Während das Image gepullt wird, sieht die Ausgabe in etwa wie folgt aus:

```console
PS Azure:\> docker pull microsoft/aci-helloworld
Using default tag: latest
latest: Pulling from microsoft/aci-helloworld
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
3be0618266da: Pull complete
9e232827e52f: Pull complete
b53c152f538f: Pull complete
Digest: sha256:a3b2eb140e6881ca2c4df4d9c97bedda7468a5c17240d7c5d30a32850a2bc573
Status: Downloaded newer image for microsoft/aci-helloworld:latest
```

Bevor Sie ein Image mithilfe von Push in Ihre Azure Container Registry übertragen können, müssen Sie es mit dem vollqualifizierten Namen (FQDN) Ihrer Registrierung markieren. Der FQDN von Azure Container Registrys weist das Format *\<registrierungs-name\>.azurecr.io* auf.

Füllen Sie eine Variable mit dem vollständigen Imagetag auf. Schließen Sie den Anmeldeserver, den Repositorynamen („aci-helloworld“) und die Imageversion („v1“) ein:

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
```

Verwenden Sie nun [docker tag][docker-tag], um das Image zu kennzeichnen:

```powershell
docker tag microsoft/aci-helloworld $image
```

Und schließlich [docker push][docker-push], um es in die Registrierung zu pushen:

```powershell
docker push $image
```

Während der Docker-Client das Bild pusht, sollte die Ausgabe ähnlich wie das folgende Beispiel aussehen:

```console
PS Azure:\> docker push $image
The push refers to repository [myContainerRegistry007.azurecr.io/aci-helloworld]
31ba1ebd9cf5: Pushed
cd07853fe8be: Pushed
73f25249687f: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:565dba8ce20ca1a311c2d9485089d7ddc935dd50140510050345a1b0ea4ffa6e size: 1576
```

Glückwunsch! Sie haben Ihr erstes Containerimage in Ihre Registrierung gepusht.

## <a name="deploy-image-to-aci"></a>Bereitstellen eines Images für ACI

Da sich das Image jetzt in Ihrer Registrierung befindet, stellen Sie einen Container direkt in Azure Container Instances bereit, um die Ausführung in Azure zu beobachten.

Konvertieren Sie zuerst die Registrierungsanmeldeinformationen in ein *PSCredential*-Objekt. Der `New-AzureRmContainerGroup`-Befehl, den Sie zum Erstellen der Containerinstanz verwenden, erfordert dieses Format.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Darüber hinaus muss die DNS-Namensbezeichnung für Ihren Container eindeutig innerhalb der Azure-Region sein, in der Sie ihn erstellen. Führen Sie den folgenden Befehl zum Auffüllen einer Variablen mit einem generierten Namen aus:

```powershell
$dnsname = "aci-demo-" + (Get-Random -Maximum 9999)
```

Führen Sie schließlich [New-AzureRmContainerGroup][New-AzureRmContainerGroup] aus, um einen Container aus dem Image in Ihrer Registrierung mit einem CPU-Kern und 1 GB Arbeitsspeicher bereitzustellen:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
```

Sie sollten eine erste Antwort von Azure mit Details zu Ihrem Container erhalten, und sein Zustand ist zunächst „Pending“ (Ausstehend):

```console
PS Azure:\> New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials : {myContainerRegistry007}
RestartPolicy            : Always
IpAddress                : 40.117.255.198
DnsNameLabel             : aci-demo-8751
Fqdn                     : aci-demo-8751.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Linux
Volumes                  :
State                    : Pending
Events                   : {}
```

Um seinen Status zu überwachen und zu ermitteln, wann er ausgeführt wird, führen Sie den Befehl [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] mehrmals aus. Es sollte weniger als eine Minute dauern, bis der Container gestartet wird.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Hier können Sie erkennen, dass der ProvisioningState des Containers zunächst *Creating* (Wird erstellt) ist und dann in den Zustand *Succeeded* (Erfolgreich) wechselt, sobald er aktiv ist und ausgeführt wird:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Creating
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Succeeded
```

## <a name="view-running-application"></a>Anzeigen der ausgeführten Anwendung

Sobald die Bereitstellung in ACI erfolgreich war und Ihr Container aktiv ist und ausgeführt wird, navigieren Sie in Ihrem Browser zu dessen vollqualifizierten Domänennamen (FQDN), um zu sehen, wie die App in Azure ausgeführt wird.

Rufen Sie den FQDN für Ihren Container mit [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] ab:


```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
```

Die Ausgabe des Befehls ist der FQDN Ihrer Containerinstanz:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
aci-demo-8571.eastus.azurecontainer.io
```

Navigieren sie mit dem FQDN in Ihrem Browser zu dieser Instanz:

![„Hello World“-Anwendung im Browser][qs-psh-01-running-app]

Glückwunsch! Sie haben einen Container erstellt, der eine Anwendung in Azure ausführt. Er wurde direkt aus einem Containerimage in Ihrer privaten Azure Container Registry bereitgestellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Arbeit mit den in diesem Schnellstart erstellten Ressourcen beendet haben, verwenden Sie den Befehl [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup], um die Ressourcengruppe, die Containerregistrierung und die Containerinstanz zu entfernen:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure Container Registry mit der Azure CLI erstellt und eine Instanz davon in Azure Container Instances gestartet. Fahren Sie mit dem Azure Container Instances-Tutorial fort, um eingehendere Informationen zu ACI zu erhalten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[aci-helloworld-github]: https://github.com/Azure-Samples/aci-helloworld
[aci-helloworld-image]: https://hub.docker.com/r/microsoft/aci-helloworld/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-psh-01-running-app]: ./media/container-registry-get-started-powershell/qs-psh-01-running-app.png
