---
title: 'Azure-Schnellstart: Erstellen eines Batch AI-Clusters – Azure CLI | Microsoft-Dokumentation'
description: 'Schnellstart: Erstellen eines Batch AI-Clusters für das Training von Machine Learning- und KI-Modellen – Azure CLI'
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 09/03/2018
ms.author: danlep
ms.openlocfilehash: 0d4ba7edfb22a6710222c854ceb2bf86284d2d77
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057592"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-cli"></a>Schnellstart: Erstellen eines Clusters für Batch AI-Trainingsaufträge über die Azure CLI

Dieser Schnellstart zeigt, wie Sie die Azure CLI verwenden können, um einen Batch AI-Cluster zu erstellen, den Sie für das Training von KI- und Machine Learning-Modellen verwenden können. Batch AI ist ein verwalteter Dienst, mit dem Data Scientists und KI-Forscher KI- und Machine Learning-Modelle in Clustern mit virtuellen Azure-Computern trainieren können.

Der Cluster hat Anfangs einen einzelnen GPU-Knoten. Nach Abschluss dieser Schnellstartanleitung haben Sie einen Cluster, den Sie zentral hochskalieren und zum Trainieren Ihrer Modelle verwenden können. Übermitteln Sie Trainingsaufträge an den Cluster mit Batch AI, [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md)-Tools oder den [Visual Studio Tools for AI](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.38 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

Dieser Schnellstart setzt voraus, dass Sie Befehle in einer Bash-Shell ausführen, entweder in der Cloud Shell oder auf Ihrem lokalen Computer.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl `az group create` eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus2* erstellt. Achten Sie darauf, dass Sie einen Standort, z.B. USA, Osten 2 auswählen, in dem der Batch AI-Dienst verfügbar ist.

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-batch-ai-cluster"></a>Erstellen eines Batch AI-Clusters

Verwenden Sie zunächst den Befehl `az batchai workspace create`, um einen Batch AI-*Arbeitsbereich* zu erstellen. Sie benötigen einen Arbeitsbereich, um Ihre Batch AI-Cluster und andere Ressourcen zu organisieren.

```azurecli-interactive
az batchai workspace create \
    --workspace myworkspace \
    --resource-group myResourceGroup 
```

Verwenden Sie zum Erstellen eines Batch AI-Clusters den Befehl `az batchai cluster create`. Im folgenden Beispiel wird ein Cluster mit den folgenden Eigenschaften erstellt:

* Enthält einen einzelnen Knoten in der NC6-VM-Größe, die über eine NVIDIA Tesla K80-GPU verfügt. 
* Führt ein Standard-Ubuntu-Server-Image aus, das für das Hosting von containerbasierten Anwendungen entwickelt wurde, die Sie für die meisten Trainingsworkloads verwenden können. 
* Fügt ein Benutzerkonto mit dem Namen *myusername* hinzu und generiert SSH-Schlüssel, wenn sie nicht bereits am Standardspeicherort für Schlüssel (*~/.ssh*) in Ihrer lokalen Umgebung vorhanden sind. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --vm-size Standard_NC6 \
    --target 1 \
    --user-name myusername \
    --generate-ssh-keys
```

Die Befehlsausgabe zeigt die Clustereigenschaften an. Es dauert einige Minuten, um den Knoten zu erstellen und zu starten. Um den Status des Clusters anzuzeigen, führen Sie den Befehl `az batchai cluster show` aus. 

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

Zu Beginn der Clustererstellung ähnelt die Ausgabe der folgenden Ausgabe, die zeigt, dass sich der Cluster im Zustand `resizing` befindet:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```
Der Cluster ist einsatzbereit, wenn der Status `steady` und der einzelne Knoten `Idle` ist.

## <a name="list-cluster-nodes"></a>Auflisten von Clusterknoten 

Wenn Sie sich mit den Clusterknoten (in diesem Fall einem einzelnen Knoten) verbinden müssen, um Anwendungen zu installieren oder Wartungsarbeiten durchzuführen, erhalten Sie Verbindungsinformationen, wenn Sie den Befehl `az batchai cluster node list` ausführen:


```azurecli-interactive
az batchai cluster node list \
    --cluster mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup 
 ```

Die JSON-Ausgabe sieht in etwa wie folgt aus:

```JSON
[
  {
    "ipAddress": "40.68.254.143",
    "nodeId": "tvm-1816144089_1-20180626t233430z",
    "port": 50000.0
  }
]
```
Verwenden Sie diese Informationen, um die SSH-Verbindung zum Knoten herzustellen. Ersetzen Sie z.B. die richtige IP-Adresse Ihres Knotens in folgendem Befehl:

```bash
ssh myusername@40.68.254.143 -p 50000
``` 
Beenden Sie die SSH-Sitzung, um fortzufahren.

## <a name="resize-the-cluster"></a>Ändern der Clustergröße

Wenn Sie Ihren Cluster zum Ausführen eines Trainingsauftrags verwenden, benötigen Sie möglicherweise mehr Computeressourcen. Um die Größe für einen verteilten Trainingsauftrag auf 2 Knoten zu erhöhen, führen Sie den Befehl [batch ai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize) aus:

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 2
```

Es dauert einige Minuten, bis die Größe des Clusters geändert wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Batchtutorials und -beispielen fortfahren möchten, können Sie den mithilfe dieser Schnellstartanleitung erstellten Batch AI-Arbeitsbereich verwenden. 

Es entstehen Kosten für den Batch AI-Cluster, wenn die Knoten ausgeführt werden. Wenn Sie die Clusterkonfiguration beibehalten möchten, wenn Sie keine Aufträge ausführen müssen, passen Sie die Größe des Clusters auf 0 Knoten an. 

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 0
```

Ändern Sie die Größe später auf 1 oder mehr Knoten, um Ihre Aufträge auszuführen. Wenn Sie einen Cluster nicht mehr benötigen, löschen Sie ihn mit dem Befehl `az batchai cluster delete`:

```azurecli-interactive
az batchai cluster delete \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
```

Wenn Ressourcengruppe für die Batch AI-Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl `az group delete` entfernen. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie einen Batch AI-Cluster mit der Azure CLI erstellen. Um zu erfahren, wie Sie einen Batch AI-Cluster zum Trainieren eines Modells verwenden können, fahren Sie mit dem Schnellstart zum Trainieren eines Deep Learning-Modells fort.

> [!div class="nextstepaction"]
> [Trainieren eines Deep Learning-Modells](./quickstart-tensorflow-training-cli.md)
