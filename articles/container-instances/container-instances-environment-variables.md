---
title: Festlegen von Umgebungsvariablen in Azure Container Instances
description: Es wird beschrieben, wie Sie Umgebungsvariablen in den Containern festgelegen, die Sie in Azure Container Instances ausführen.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/16/2018
ms.author: marsma
ms.openlocfilehash: 1a025ce647cb3c071a6549a433e6505b85409fdc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34199006"
---
# <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Das Festlegen von Umgebungsvariablen in Ihren Containerinstanzen ermöglicht es Ihnen, eine dynamische Konfiguration der Anwendung oder des Skripts bereitzustellen, die bzw. das vom Container ausgeführt wird. Um Umgebungsvariablen in einem Container festzulegen, geben Sie diese bei der Erstellung einer Containerinstanz an. Sie können Umgebungsvariablen beim Starten eines Containers über die [Azure CLI](#azure-cli-example), mit [Azure PowerShell](#azure-powershell-example) und im [Azure-Portal](#azure-portal-example) festlegen.

Wenn Sie beispielsweise das Containerimage [microsoft/aci-wordcount][aci-wordcount] ausführen, können Sie dessen Verhalten ändern, indem Sie die folgenden Umgebungsvariablen angeben:

*NumWords*: Die Anzahl der Wörter, die an STDOUT gesendet wird.

*MinLength*: Die Mindestanzahl von Zeichen in einem Wort, damit es gezählt wird. Bei einem höheren Wert werden häufige Wörter wie „von“ und „das“ ignoriert.

## <a name="azure-cli-example"></a>Azure CLI-Beispiel

Um die Standardausgabe des [microsoft/aci-wordcount][aci-wordcount]-Containers anzuzeigen, führen Sie ihn zunächst mit diesem [az container create][az-container-create]-Befehl aus (ohne Umgebungsvariablen):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Um die Ausgabe zu ändern, starten Sie einen zweiten Container, wobei Sie das Argument `--environment-variables` hinzufügen und Werte für die Variablen *NumWords* und *MinLength* angeben:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Sobald für beide Container der Status *Beendet* angezeigt wird, (verwenden Sie [az container show][az-container-show] zum Überprüfen des Status), zeigen Sie deren Protokolle mit [az container logs][az-container-logs] an, um die Ausgabe zu sehen.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

Die Ausgabe der Container zeigt, wie Sie die das Skriptverhalten des zweiten Containers durch Festlegen von Umgebungsvariablen geändert haben.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
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

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Azure PowerShell-Beispiel

Das Festlegen von Umgebungsvariablen in PowerShell ähnelt dem Vorgang über die Befehlszeilenschnittstelle, es wird jedoch das Befehlszeilenargument `-EnvironmentVariable` verwendet.

Starten Sie zuerst den Container [microsoft/aci-wordcount][aci-wordcount] in seiner Standardkonfiguration mit diesem [New-AzureRmContainerGroup][new-azurermcontainergroup]-Befehl:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Führen Sie nun den folgenden [New-AzureRmContainerGroup][new-azurermcontainergroup]-Befehl aus. Dieser gibt die Umgebungsvariablen *NumWords* und *MinLength* nach dem Auffüllen einer Arrayvariablen (`envVars`) an:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Sobald für beide Container der Status *Beendet* angezeigt wird (verwenden Sie [Get-AzureRmContainerInstanceLog][azure-instance-log] zum Überprüfen des Status), rufen Sie mit dem [Get-AzureRmContainerInstanceLog][azure-instance-log]-Befehl deren Protokolle ab.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

Die Ausgabe für jeden Container zeigt, wie Sie das vom Container ausgeführte Skript durch Festlegen von Umgebungsvariablen geändert haben.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
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

Azure:\
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Azure-Portal-Beispiel

Um Umgebungsvariablen beim Starten eines Containers im Azure-Portal festzulegen, geben Sie diese auf der Seite **Konfiguration** an, wenn Sie den Container erstellen.

Wenn Sie die Bereitstellung mit dem Portal vornehmen, sind Sie derzeit auf drei Variablen beschränkt und müssen diese im folgenden Format eingeben: `"variableName":"value"`

Um ein Beispiel anzuzeigen, starten Sie den Container [microsoft/aci-wordcount][aci-wordcount] mit den Variablen *NumWords* und *MinLength*.

1. Legen Sie unter **Konfiguration** die **Neustartrichtlinie** auf *Bei einem Fehler* fest.
2. Geben Sie für die erste Variable `"NumWords":"5"` ein, wählen Sie unter **Weitere Umgebungsvariablen hinzufügen** die Option **Ja** aus, und geben Sie für die zweite Variable `"MinLength":"8"` ein. Wählen Sie **OK** aus, um den Container zu überprüfen, und stellen Sie ihn dann bereit.

![Portalseite mit Umgebungsvariable, Aktivieren-Schaltfläche und Textfeldern][portal-env-vars-01]

Klicken Sie zum Anzeigen der Protokolle des Containers unter **EINSTELLUNGEN** auf **Container** > **Protokolle**. Ähnlich wie in der in den vorherigen Abschnitten zur Befehlszeilenschnittstelle und zu PowerShell angezeigten Ausgabe sehen Sie, wie das Skriptverhalten durch die Umgebungsvariablen geändert wurde. Nur fünf Wörter werden angezeigt, jeweils mit einer minimalen Länge von acht Zeichen.

![Portal mit der Containerprotokollausgabe][portal-env-vars-02]

## <a name="next-steps"></a>Nächste Schritte

Aufgabenbasierte Szenarien, z. B. Stapelverarbeitung eines großen Datasets mit mehreren Containern, können von benutzerdefinierten Umgebungsvariablen zur Laufzeit profitieren. Weitere Informationen zum Ausführen aufgabenbasierter Containern finden Sie unter [Ausführen von Aufgaben in Containern in Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com