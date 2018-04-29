---
title: Festlegen von Umgebungsvariablen in Azure Container Instances
description: Es wird beschrieben, wie in Azure Container Instances Umgebungsvariablen festgelegt werden.
services: container-instances
author: david-stanford
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/13/2018
ms.author: dastanfo
ms.openlocfilehash: 37fde41b6dc2ea0a4d3b4b38a0e3df81a297c125
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Das Festlegen von Umgebungsvariablen in Ihren Containerinstanzen ermöglicht es Ihnen, eine dynamische Konfiguration der Anwendung oder des Skripts bereitzustellen, die bzw. das vom Container ausgeführt wird.

Derzeit können Sie Umgebungsvariablen über die CLI und PowerShell festlegen. In beiden Fällen verwenden Sie in den Befehlen ein Flag, um die Umgebungsvariablen festzulegen. Das Festlegen von Umgebungsvariablen in ACI ähnelt dem Befehlszeilenargument `--env` für `docker run`. Wenn Sie beispielsweise das Containerimage „microsoft/aci-wordcount“ verwenden, können Sie das Verhalten ändern, indem Sie die folgenden Umgebungsvariablen angeben:

*NumWords*: Die Anzahl der Wörter, die an STDOUT gesendet wird.

*MinLength*: Die Mindestanzahl von Zeichen in einem Wort, damit es gezählt wird. Bei einem höheren Wert werden häufige Wörter wie „von“ und „das“ ignoriert.

## <a name="azure-cli-example"></a>Azure CLI-Beispiel

Führen Sie den folgenden Befehl aus, um die Standardausgabe des Containers anzuzeigen:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Wenn Sie `NumWords=5` und `MinLength=8` als Umgebungsvariablen für den Container angeben, sollten die Containerprotokolle verschiedene Ausgaben anzeigen.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Sobald der Containerstatus als *Beendet* angezeigt wird (Sie können den Status mit [az container show][az-container-show] überprüfen), können Sie die Protokolle anzeigen, um die Ausgabe zu prüfen.  Legen Sie „--name“ nicht auf „mycontainer2“, sondern auf „mycontainer1“ fest, um die Ausgabe des Containers ohne Umgebungsvariablen anzuzeigen.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

## <a name="azure-powershell-example"></a>Azure PowerShell-Beispiel

Führen Sie den folgenden Befehl aus, um die Standardausgabe des Containers anzuzeigen:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Wenn Sie `NumWords=5` und `MinLength=8` als Umgebungsvariablen für den Container angeben, sollten die Containerprotokolle verschiedene Ausgaben anzeigen.

```azurepowershell-interactive
$envVars = @{NumWord=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Sobald der Containerstatus als *Beendet* angezeigt wird (Sie können den Status mit [Get-AzureRmContainerInstanceLog][azure-instance-log] überprüfen), können Sie die Protokolle anzeigen, um die Ausgabe zu prüfen. Legen Sie „ContainerGroupName“ nicht auf „mycontainer2“, sondern auf „mycontainer1“ fest, um die Containerprotokolle ohne Umgebungsvariablen anzuzeigen.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog `
    -ResourceGroupName myResourceGroup `
    -ContainerGroupName mycontainer2
```

## <a name="example-output-without-environment-variables"></a>Beispielausgabe ohne Umgebungsvariablen

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

## <a name="example-output-with-environment-variables"></a>Beispielausgabe mit Umgebungsvariablen

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Sie die Eingabe in Ihren Container anpassen können, können Sie sich über die Beibehaltung der Ausgabe von Containern informieren, die bis zum Ende ausgeführt werden.
> [!div class="nextstepaction"]
> [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](container-instances-mounting-azure-files-volume.md)

<!-- LINKS Internal -->
[azure-cloud-shell]: ../cloud-shell/overview.md
[azure-cli-install]: /cli/azure/
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show