---
title: "Ausführen von Aufgaben in Containern in Azure Container Instances"
description: "Hier erfahren Sie, wie Sie mit Azure Container Instances Aufgaben ausführen, die bis zum Abschluss ausgeführt werden, z.B. bei Build-, Test- oder Image-Rendering-Aufträgen."
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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 06368cb5084dd49e24c1c25aa8100a7680aacefb
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="run-a-containerized-task-in-azure-container-instances"></a>Ausführen von Aufgaben in Containern in Azure Container Instances

Da Container in Azure Container Instances sehr schnell und bequem bereitgestellt werden können, ist dies eine ideale Plattform zum Ausführen von einmaligen Aufgaben, wie Erstellen, Testen und Image-Rendering in einer Containerinstanz.

Mit einer konfigurierbaren Neustartrichtlinie können Sie angeben, dass Container beendet werden, wenn ihre Prozesse abgeschlossen wurden. Da Containerinstanzen nach Sekunden abgerechnet werden, fallen nur Gebühren für die Computerressourcen an, die beim Ausführen des Containers verwendet werden, der Ihre Aufgabe ausführt.

In den Beispielen in diesem Artikel wird die Azure-Befehlszeilenschnittstelle (Azure CLI) verwendet. Sie benötigen Azure CLI-Version 2.0.21 oder höher mit einer [lokalen Installation](/cli/azure/install-azure-cli), oder Sie verwenden die CLI in der [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Container-Neustartrichtlinie

Wenn Sie einen Container in Azure Container Instances erstellen, können Sie eine von drei Einstellungen für Neustartrichtlinien festlegen.

| Neustartrichtlinie   | Beschreibung |
| ---------------- | :---------- |
| `Always` | Container in der Containergruppe werden immer neu gestartet. Dies ist die **Standardeinstellung**, wenn beim Erstellen des Containers keine Neustartrichtlinie angegeben wird. |
| `Never` | Container in der Containergruppe werden nie neu gestartet. Die Container werden höchstens einmal ausgeführt. |
| `OnFailure` | Container in der Containergruppe werden nur dann neu gestartet, wenn bei dem im Container ausgeführten Prozess ein Fehler auftritt (d.h., wenn er mit einem Exitcode ungleich Null beendet wird). Die Container werden mindestens einmal ausgeführt. |

## <a name="specify-a-restart-policy"></a>Angeben einer Neustartrichtlinie

Wie Sie eine Richtlinie für den Neustart angeben, hängt davon ab, wie Sie die Containerinstanzen erstellen, z.B. mit der Azure CLI, Azure PowerShell-Cmdlets oder im Azure-Portal. Geben Sie in der Azure CLI den `--restart-policy`-Parameter an, wenn Sie [az container create][az-container-create] aufrufen.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Beispiel für das Ausführen bis zum Abschluss

Um die Richtlinie für den Neustart in der Praxis zu sehen, erstellen Sie eine Containerinstanz aus dem Image [microsoft/aci-wordcount](https://hub.docker.com/r/microsoft/aci-wordcount/), und geben Sie die Neustartrichtlinie `OnFailure` an. Bei diesem Beispielcontainer wird ein Python-Skript ausgeführt, das standardmäßig den Text von Shakespeares [Hamlet](http://shakespeare.mit.edu/hamlet/full.html) analysiert, die 10 häufigsten Wörter an STDOUT schreibt und dann beendet wird.

Führen Sie den Beispielcontainer mit dem folgenden [az container create][az-container-create]-Befehl aus:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances startet den Container und beendet ihn dann, wenn die Anwendung (oder wie in diesem Fall das Skript) beendet wird. Wenn Azure Container Instances einen Container beendet, dessen Neustartrichtlinie `Never` oder `OnFailure` lautet, wird der Status des Containers auf **Beendet** festgelegt. Sie können den Status des Containers mit dem Befehl [az container show][az-container-show] überprüfen:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Beispielausgabe:

```bash
"Terminated"
```

Sobald der Status des Beispielcontainers *Beendet* lautet, können Sie die Taskausgabe in den Containerprotokollen anzeigen. Führen Sie den Befehl [az container logs][az-container-logs] aus, um die Ausgabe des Skripts anzuzeigen:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Ausgabe:

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

Dieses Beispiel zeigt die Ausgabe, die das Skript an STDOUT gesendet hat. Die in Containern ausgeführten Aufgaben schreiben ihre Ausgabe allerdings möglicherweise in permanenten Speicher, um sie später abrufen zu können. Dies kann beispielsweise eine [Azure-Dateifreigabe](container-instances-mounting-azure-files-volume.md) sein.

## <a name="configure-containers-at-runtime"></a>Konfigurieren von Containern zur Laufzeit

Wenn Sie eine Containerinstanz erstellen, können Sie **Umgebungsvariablen** festlegen sowie eine benutzerdefinierte **Befehlszeile** angeben, die beim Starten des Containers ausgeführt wird. Sie können diese Einstellungen in Batchaufträgen verwenden, um die einzelnen Container mit einer aufgabenspezifischen Konfiguration vorzubereiten.

## <a name="environment-variables"></a>Umgebungsvariablen

Legen Sie Umgebungsvariablen in Ihrem Container fest, um eine dynamische Konfiguration der Anwendung oder des Skripts bereitzustellen, die bzw. das vom Container ausgeführt wird. Dies ist vergleichbar mit dem Befehlszeilenargument `--env` für `docker run`.

Sie können beispielsweise das Verhalten des Skripts im Beispielcontainer ändern, indem Sie beim Erstellen der Containerinstanz die folgenden Umgebungsvariablen angeben:

*NumWords*: Die Anzahl der Wörter, die an STDOUT gesendet wird.

*MinLength*: Die Mindestanzahl von Zeichen in einem Wort, damit es gezählt wird. Bei einem höheren Wert werden häufige Wörter wie „von“ und „das“ ignoriert.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Wenn Sie `NumWords=5` und `MinLength=8` als Umgebungsvariablen für den Container angeben, sollten die Containerprotokolle verschiedene Ausgaben anzeigen. Sobald der Containerstatus als *Beendet* angezeigt wird (Sie können den Status mit `az container show` überprüfen), zeigen Sie die Protokolle an, um die neue Ausgabe zu sehen:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Ausgabe:

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="command-line-override"></a>Außerkraftsetzung der Befehlszeile

Geben Sie beim Erstellen einer Containerinstanz eine Befehlszeile an, um die im Containerimage integrierte Befehlszeile außer Kraft zu setzen. Dies ist vergleichbar mit dem Befehlszeilenargument `--entrypoint` für `docker run`.

Sie können z.B. den Beispielcontainer einen anderen Text als *Hamlet* analysieren lassen, indem Sie eine andere Befehlszeile angeben. Das vom Container ausgeführte Python-Skript (*wordcount.py*) akzeptiert eine URL als Argument und verarbeitet den Inhalt dieser Seite anstelle des Standardwerts.

So bestimmen Sie z.B. die drei häufigsten Wörter mit fünf Buchstaben in *Romeo und Julia*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer3 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Zeigen Sie auch hier, wenn der Container den Status *Beendet* aufweist, die Ausgabe durch Aufrufen der Containerprotokolle an:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer3
```

Ausgabe:

```bash
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Nächste Schritte

Details zum dauerhaften Speichern der Ausgabe von Containern, die bis zum Abschluss ausgeführt werden, finden Sie unter [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](container-instances-mounting-azure-files-volume.md).

<!-- LINKS -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show