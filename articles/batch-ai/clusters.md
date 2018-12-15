---
title: Arbeiten mit Azure Batch AI-Clustern | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Batch AI-Clusterkonfiguration auswählen sowie eine AI-Cluster erstellen und verwalten.
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 9af8ce84805e48dd3c91dd7fb4fcf0b136fbfc60
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410090"
---
# <a name="work-with-batch-ai-clusters"></a>Arbeiten mit Batch AI-Clustern 

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

In diesem Artikel wird das Arbeiten mit Clustern in Azure Batch AI erläutert. Eingeführt werden das Konzept von Clustern, die möglichen Arten von Konfigurationen und Beispiele. In den meisten Beispielen zum Erstellen und Verwalten eines Clusters wird in diesem Artikel die Azure-Befehlszeilenschnittstelle verwendet. Sie können zum Arbeiten mit Clustern jedoch auch andere Tools verwenden, z. B. das Azure-Portal und die Azure Batch AI-SDKs.

Ein Batch AI-Cluster ist eine von mehreren Ressourcen in diesem Dienst. In der [Übersicht über Ressourcen in Batch AI](resource-concepts.md) erhalten Sie Informationen zum Bereich von Clustern im Dienst.

## <a name="introduction-to-clusters"></a>Einführung in Cluster

Ein Cluster in Batch AI enthält die Computerressourcen zum Ausführen von Machine Learning-Aufträgen und KI-Trainingsaufträgen. Alle Knoten in einem Cluster haben die gleiche VM-Größe und das gleiche Betriebssystemimage. Batch AI umfasst viele Optionen zum Erstellen von Clustern, die an unterschiedliche Anforderungen angepasst sind. Normalerweise richten Sie jeweils einen Cluster für jede Kategorie der zum Abschließen eines Projekts erforderlichen Verarbeitungsleistung ein. Sie können die Anzahl der Knoten in einem Cluster je nach Nachfrage und Budget zentral hoch- und herunterskalieren. Cluster können in einem Team bereitgestellt und freigegeben werden, oder Einzelpersonen können jeweils einen eigenen Cluster bereitstellen. 

Jeder Cluster ist in einer Batch AI-Ressource enthalten, die als *Arbeitsbereich* bezeichnet wird. Vor der Bereitstellung von Clustern müssen Sie daher einen Batch AI-Arbeitsbereich einrichten. Bei Verwendung der Azure-Befehlszeilenschnittstelle können Sie beispielsweise mit dem Befehl [az batchai workspace create](/cli/azure/batchai/workspace?view=azure-cli-latest#az-batchai-workspace-create) einen Arbeitsbereich einrichten. 

Nach dem Erstellen eines Clusters können Sie jeweils einen Auftrag in eine Warteschlange übermitteln. Das Ressourcenzuordnungsverfahren für die Ausführung der Aufträge im Cluster wird dann in Batch AI verarbeitet. 

## <a name="cluster-configuration-options"></a>Konfigurationsoptionen für Cluster

Bestimmen Sie bei der Planung eines Clusters zunächst Ihre Computeanforderungen. Batch AI bietet flexible Konfigurationsoptionen, sodass Sie einen Cluster entsprechend Ihren Anforderungen anpassen können. Folgende Hauptfunktionen sind beim Bereitstellen eines Clusters zu berücksichtigen:

* **VM-Größe:** Wählen Sie eine beliebige [VM-Größe](../virtual-machines/linux/sizes.md) aus, die in einer der [unterstützten Region](https://azure.microsoft.com/global-infrastructure/services/) für die Clusterknoten verfügbar ist. Jeder Cluster darf nur eine VM-Größe enthalten. Wenn für Ihre Aufgaben unterschiedliche Typen von virtuellen Computern erforderlich sind, müssen Sie daher für jede Art von Computeanforderung einen separaten Cluster erstellen. Informationen zum Trainieren von mit Frameworks entwickelten Machine Learning- oder KI-Modellen, bei denen GPUs genutzt werden, finden Sie unter [GPU-optimierte VM-Größen](../virtual-machines/linux/sizes-gpu.md) in Azure.
  
* **VM-Priorität:** Batch AI bietet entweder dedizierte virtuelle Computer oder virtuelle Computer mit niedriger Priorität für einen Cluster. Dedizierte virtuelle Computer sind für die Verwendung im Cluster reserviert. Mit der Option für virtuelle Computer mit niedriger Priorität wird nicht verwendete Azure-VM-Kapazität zu erheblichen Kosteneinsparungen zugeordnet im Tausch gegen die Möglichkeit, Aufträge vorzeitig zu entfernen, wenn die virtuellen Computer in Azure freigegeben werden. Aufträge, die länger als 24 Stunden auf einem virtuellen Computer mit niedriger Priorität ausgeführt werden, werden auch automatisch vorzeitig entfernt. Im Hinblick auf das Budget können Sie virtuelle Computer mit niedriger Priorität für kurze Experimentaufträge verwenden. Wechseln Sie dann zu dedizierten virtuellen Computern, wenn längere Aufträge ausgeführt werden sollen.

* **Knotenanzahl:** Batch AI umfasst Optionen für die manuelle und automatische Skalierung für die Anzahl der Knoten im Cluster. Mit der Option zur manuellen Skalierung steuern Sie, wann ein Cluster zentral hoch- oder herunterskaliert wird, sodass Sie Ihre Computekosten besser verwalten können. Mit der Option zur automatischen Skalierung wird sichergestellt, dass der Cluster immer zentral herunterskaliert wird, wenn er nicht verwendet wird, sodass Ihre Computekosten minimiert werden. 

  Wenn Sie den Cluster manuell skalieren möchten, definieren Sie das anfängliche Ziel während der Clustererstellung. Das Ziel ist die Anzahl der Knoten, die in Batch AI anfänglich zugeordnet werden. Später können Sie die Anzahl der Knoten manuell ändern.  

  Wenn Sie die Option zur automatischen Skalierung auswählen, definieren Sie die maximale und die minimale Anzahl der Zielknoten während der Clustererstellung. Das Ziel kann von 0 bis zur maximalen Anzahl von Knoten reichen, die von Ihrem [Kernkontingent für Batch AI](quota-limits.md) unterstützt werden. Bei dem Ziel 0 können Sie Ihre Clusterkonfiguration verwalten, ohne dass Computekosten für Sie anfallen. Wenn Sie ein Ziel anfordern, das über Ihrer Kontingentgrenze liegt, treten bei der Bereitstellung Fehler auf. 

* **VM-Image:** In Batch AI werden die Cluster-VMs standardmäßig mit einem Ubuntu-Standardserverimage bereitgestellt, mit dem Containerworkloads unterstützt werden. Im Azure Marketplace können Sie ein anderes vorkonfiguriertes Linux-Image auswählen, z.B. [Data Science Virtual Machine](../machine-learning/data-science-virtual-machine/overview.md). 

* **Speicher:** Batch AI umfasst eine Option zur automatischen Speicherung, die Sie auswählen können, wenn Sie einen Cluster über die Azure-Befehlszeilenschnittstelle erstellen. Mit dieser Option werden automatisch eine Azure-Dateifreigabe und ein Blobcontainer unter einem neuen Speicherkonto erstellt. Diese Speicherressourcen werden während der Ausführungszeit in die einzelnen Knoten eingebunden, sodass über einen lokalen Pfad auf die Dateien zugegriffen werden kann. Die Speicherkontonamen, der Dateifreigabename, der Blobcontainername und die Einbindungspfade haben alle Standardwerte, die während der Clustererstellung über zusätzliche Parameter auch angepasst werden können. 

  Wenn keine Speicheroptionen definiert sind, müssen Sie die Speicherressourcen separat erstellen und beim Übermitteln von Aufträgen definieren. Diese Option ist nützlich, wenn der Cluster auf Organisationsebene, der Speicher jedoch auf Benutzerebene verwaltet wird. Weitere Informationen zum Hochladen von Dateien in Azure Storage und zum Zugreifen auf die Dateien während der Ausführungszeit finden Sie unter [Speichern von Batch AI-Auftragseingabe und -ausgabe mit Azure Storage](use-azure-storage.md).

* **Benutzerzugriff:** Mit Batch AI können Sie beim Erstellen eines Clusters öffentliche und private SSH-Schlüsseldateien generieren oder eigene SSH-Schlüssel angeben, sodass Sie eine SSH-Verbindung mit einzelnen Knoten herstellen können. Zudem können Sie einen Benutzernamen (standardmäßig auf den aktuellen Benutzer festgelegt) und ein Kennwort definieren. Diese Anmeldeinformationen können während der Ausführung für den Zugriff auf die Knoten oder zum Anzeigen verschiedener Metriken oder für weitere Einblicke in die Aufträge verwendet werden.

* **Setuptask:** Mit Batch AI können Sie Befehlszeilenargumente definieren, die nach der Zuordnung in den einzelnen Knoten ausgeführt werden sollen. Sie können zudem den Pfad definieren, in dem die Ausgabedatei protokolliert werden soll. Verwenden Sie diese Option, wenn Sie über das Basisimage hinaus zusätzliche Bereitstellungsschritte ausführen.

* **Zusätzliche Konfiguration:** Es gibt mehrere weniger häufig verwendete Szenarien, für die möglicherweise speziellere Konfigurationen erforderlich sind. In diesem Fall kann zum Erstellen eines Clusters über den Azure CLI-Befehl eine [Clusterkonfigurationsdatei](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#using-cluster-configuration-file) angefügt werden. 

## <a name="create-the-cluster"></a>Erstellen des Clusters

Nachdem Sie die Optionen der Clusterkonfiguration festgelegt haben, können Sie den Cluster über die Azure-Befehlszeilenschnittstelle, das Azure-Portal oder Batch AI-APIs erstellen. Um einen Cluster z.B. über die Azure-Befehlszeilenschnittstelle zu erstellen, können Sie entsprechend der Dokumentation zu [az batchai cluster create](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-create) den genauen Befehl erstellen, über den die gewünschten Konfigurationen eingerichtet werden. 

In der einfachsten Konfiguration wird mit dem folgenden Befehl ein Standard_NC6-Cluster mit einem Knoten und mit SSH-Zugriff bereitgestellt. Standardmäßig enthält dieser Cluster dedizierte virtuelle Computer, auf denen das neueste Ubuntu-Standardserverimage ausgeführt wird.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --target 1 \
    --generate-ssh-keys
```

Im folgenden Beispiel wird ein Standard_NC6-Cluster bereitgestellt, in dem automatisch von 0 auf 4 Knoten skaliert wird und der Knoten mit niedriger Priorität und ein Konto mit automatischer Speicherung verwendet. Dieses Setup ist eine geeignete Konfiguration, wenn Sie einen kostengünstigen und einfach zu verwaltenden Cluster erstellen möchten. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --vm-priority lowpriority \
    --max 4 \
    --min 0 \
    --use-auto-storage 
```

Im folgenden Beispiel wird ein Standard_NC6-Cluster bereitgestellt, der ein Data Science Virtual Machine-Image, benutzerdefinierte Speicher- und Einbindungsoptionen, benutzerdefinierte SSH-Anmeldeinformationen, einen Setuptask, mit dem das *unzip*-Paket installiert wird, und eine Clusterkonfigurationsdatei für zusätzliches Setup enthält. Diese Konfiguration ist ein Beispiel für einen Cluster, der besser an Ihre Anforderungen angepasst ist.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --image UbuntuDSVM \ 
    --config-file cluster.json \
    --setup-task 'apt install unzip -y'
    --storage-account-name <STORAGE ACCOUNT NAME> \
    --nfs-name nfsmount \
    --afs-name afsmount \
    --bfs-name bfsmount \
    --user-name adminuser \
    --ssh-key id_rsa.pub \
    --password secretpassword 
```

## <a name="monitor-the-cluster"></a>Überwachen des Clusters

Mit den Befehlen [az batchai cluster list](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-list), [az batchai cluster show](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-show) und [az batchai cluster node list](/cli/azure/batchai/cluster/node?view=azure-cli-latest#az-batchai-cluster-node-list) können verschiedene Informationen zu den einzelnen Clustern überwacht werden.

### <a name="list-all-clusters"></a>Auflisten aller Cluster

Mit dem folgenden Befehl werden alle Cluster in einem Arbeitsbereich aufgelistet.

```azurecli-interactive
az batchai cluster list \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
```

### <a name="show-information-about-a-cluster"></a>Anzeigen von Informationen zu einem Cluster

Mit dem folgenden Befehl werden die vollständigen Informationen zu einem bestimmten Cluster in Tabellenform angezeigt.

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --output table
```

Wenn der Cluster unter Verwendung der Option zur automatischen Speicherung bereitgestellt wurde, können Sie das Speicherkonto abrufen, das beim Hochladen von Skripts und Trainieren von Aufträgen verwendet werden soll. Verwenden Sie den folgenden Befehl:

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --query "nodeSetup.mountVolumes.azureFileShares[0].{storageAccountName:accountName}"
```

Die Ausgabe sollte in etwa wie folgt aussehen.

```JSON
{
  "storageAccountName": "baixxxxxxxxx"
}
```

### <a name="list-cluster-nodes"></a>Auflisten von Clusterknoten

Wenn Sie eine Verbindung mit den Clusterknoten herstellen möchten, können mit dem folgenden Befehl die Liste der Knoten und die Verbindungsinformationen abgerufen werden.  

```azurecli-interactive
az batchai cluster node list \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
 ```

Die Ausgabe für die einzelnen Knoten ähnelt folgendem Beispiel:

```JSON
[
  {
    "ipAddress": "40.68.xxx.xxx",
    "nodeId": "tvm-xxxxxxxxxx-xxxxxxxx",
    "port": 50000.0
  }
]
```
Anhand dieser Informationen können Sie mit einem Befehl, der dem folgenden ähnelt, eine SSH-Verbindung herstellen.

```bash
ssh myusername@40.68.xxx.xxx -p 50000
``` 

## <a name="submit-jobs-to-the-cluster"></a>Übermitteln von Aufträgen an den Cluster

Nach der Bereitstellung des Clusters können Sie Aufträge übermitteln, die auf den Knoten ausgeführt werden. Unter der Beschreibung zum Befehl [az batchai job](/cli/azure/batchai/job?view=azure-cli-latest) finden Sie verschiedene Möglichkeiten zum Vorbereiten, Übermitteln und Überwachen von Aufträgen über die Azure-Befehlszeilenschnittstelle. 

## <a name="downscale-cluster-for-later-use"></a>Zentrales Herunterskalieren des Clusters zur späteren Verwendung

Nachdem Sie die Aufträge ausgeführt haben, können Sie den Cluster zentral herunterskalieren. Es empfiehlt sich immer, Cluster zentral herunterzuskalieren, wenn sie nicht verwendet werden, um Computekosten zu sparen. Durch zentrales Herunterskalieren von Clustern auf 0 Knoten fallen keine Abrechnungskosten mehr für Sie an. Zudem müssen die Cluster zukünftig nicht erneut bereitgestellt werden, wenn Sie wieder eine zentrale Hochskalierung durchführen möchten. Wenn bei der Clustererstellung die automatische Skalierung ausgewählt wurde, werden die Cluster automatisch auf das minimale Ziel zentral herunterskaliert. Wenn die manuelle Skalierung ausgewählt wurde, skalieren Sie den Cluster mit dem folgenden Befehl zentral herunter.

```azurecli-interactive
az batchai cluster resize \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE> \
    --target 0
```

## <a name="delete-a-cluster"></a>Löschen eines Clusters

Führen Sie nach Abschluss der Verwendung des Clusters den Befehl [az batchai cluster delete](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-delete) aus, um den Cluster zu löschen.

```azurecli-interactive
az batchai cluster delete \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE>
```

Durch Löschen der Ressourcengruppe werden automatisch auch alle Cluster gelöscht, die unter dieser Ressourcengruppe bereitgestellt wurden.

```azurecli-interactive
az group delete --name <RESOURCE GROUP>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele zum Erstellen eines Batch AI-Clusters finden Sie im Schnellstart zum [Portal](quickstart-create-cluster-portal.md) oder der [Azure-Befehlszeilenschnittstelle](quickstart-create-cluster-cli.md) oder in den [Batch AI-Anleitungen](https://github.com/Azure/BatchAI/tree/master/recipes) auf GitHub.
