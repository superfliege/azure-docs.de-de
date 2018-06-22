---
title: Migrieren zu aktualisierter Azure Batch AI-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie beim Aktualisieren von Azure Batch AI-Code und -Skripts im Arbeitsbereich vorgehen und mit Ressourcen experimentieren.
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
ms.devlang: multiple
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: b59173259aa86a429b9f926a8e5ffbfd046451a1
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294876"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Migrieren zu aktualisierter Batch AI-API

Mit der Batch AI-REST-API-Version 2018-05-01 und den dazugehörigen Batch AI-SDKs und -Tools wurden wesentliche Änderungen und neue Funktionen eingeführt.

Wenn Sie eine frühere Version der Batch AI-API verwendet haben, erfahren Sie in diesem Artikel, wie Sie Ihren Code und Ihre Skripts ändern können, um mit der neuen API zu arbeiten. Nehmen Sie diese Änderungen erst vor, wenn die neue API verfügbar ist.

## <a name="whats-changing"></a>Was hat sich geändert?

Als Reaktion auf das Feedback unserer Kunden heben wir Beschränkung für die Anzahl der Aufträge auf und vereinfachen die Verwaltung der Batch AI-Ressourcen. Die neue API führt zwei neue Ressourcen ein: *Arbeitsbereich* und *Experiment*. Sammeln Sie verwandte Trainingsaufträge unter einem Experiment, und organisieren Sie alle verwandten Batch AI-Ressourcen (Cluster, Dateiserver, Experimente, Aufträge) unter einem Arbeitsbereich.  

* **Arbeitsbereich**: Eine allgemeine Sammlung aller Typen von Batch AI-Ressourcen. Cluster und Dateiserver sind in einem Arbeitsbereich enthalten. In Arbeitsbereichen werden Arbeiten, die zu unterschiedlichen Gruppen oder Projekten gehören, in der Regel getrennt. Beispielsweise können ein Arbeitsbereich für die Entwicklung und einer für Tests vorhanden sein. Wahrscheinlich benötigen Sie nur eine begrenzte Anzahl von Arbeitsbereichen pro Abonnement. 

* **Experiment**: Eine Sammlung von verwandten Aufträgen, die gemeinsam abgefragt und verwaltet werden können. Verwenden Sie beispielsweise ein Experiment, um alle Aufträge zu gruppieren, die als Teil einer Hyperparameteroptimierung ausgeführt werden. 

In der folgenden Abbildung sehen Sie ein Beispiel für eine Ressourcenhierarchie. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>Überwachen und Verwalten vorhandener Ressourcen
In jeder Ressourcengruppe, in der Sie bereits Batch AI-Cluster, Aufträge oder Dateiserver erstellt haben, erstellt der Batch AI-Dienst einen Arbeitsbereich namens `migrated-<region>` (z.B. `migrated-eastus`) und ein Experiment namens `migrated`. Um auf die zuvor erstellten Aufträge, Cluster oder Dateiserver zugreifen zu können, müssen Sie den migrierten Arbeitsbereich und das migrierte Experiment verwenden. 

### <a name="portal"></a>Portal 
Damit Sie über das Portal auf zuvor erstellte Aufträge, Cluster oder Dateiserver zugreifen können, wählen Sie zunächst den Arbeitsbereich `migrated-<region>` aus. Nachdem Sie den Arbeitsbereich ausgewählt haben, führen Sie die gewünschten Vorgänge aus, z.B. Größenänderungen oder das Löschen eines Clusters sowie die Anzeige des Auftragsstatus und der Ausgaben. 

### <a name="sdks"></a>SDKs 
Für den Zugriff auf Aufträge, Cluster oder Dateiserver, die zuvor über die Batch AI-SDKs erstellt wurden, geben Sie die Parameter für Arbeitsbereichsname und Experimentname an. 

Wenn Sie das Python-SDK verwenden, werden relevante Änderungen in den folgenden Beispielen gezeigt. Die Änderungen sind in den anderen Batch AI-SDKs ähnlich. 


#### <a name="get-old-cluster"></a>Abrufen von altem Cluster 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Löschen von altem Cluster 

```python
client.clusters.delete(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="get-old-file-server"></a>Abrufen von altem Dateiserver 

```python
cluster = client.fileservers.get(resource_group_name, 'migrated-<region>', fileserver_name)
```

#### <a name="delete-old-file-server"></a>Löschen von altem Dateiserver  

```python
client.fileservers.delete(resource_group_name, 'migrated-<region>', fileserver_name)
``` 


#### <a name="get-old-job"></a>Abrufen von altem Auftrag 

```python
cluster = client.jobs.get(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```

#### <a name="delete-old-job"></a>Löschen von altem Auftrag

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle 
 
Wenn Sie über die Azure CLI auf zuvor erstellte Aufträge, Cluster oder Dateiserver zugreifen, verwenden Sie die Parameter `-w` und `-e`, um Arbeitsbereichs- und Experimentnamen anzugeben. 


#### <a name="get-old-cluster"></a>Abrufen von altem Cluster

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Löschen von altem Cluster 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Abrufen von altem Dateiserver

```azurecli
az batchai file-server show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Löschen von altem Dateiserver 

```azurecli
az batchai file-server delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Abrufen von altem Auftrag

```azurecli
az batchai job show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Löschen von altem Auftrag 

```azurecli
az batchai job delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Erstellen von Batch AI-Ressourcen 
 
Wenn Sie eines der vorhandenen Batch AI-SDKs verwenden, können Sie damit auch Batch AI-Ressourcen (Aufträge, Cluster oder Dateiserver) ohne Codeänderungen erstellen. Nach dem Upgrade auf das neue SDK müssen Sie jedoch die folgenden Änderungen vornehmen.
 
### <a name="create-workspace"></a>Erstellen eines Arbeitsbereichs 
Je nach Szenario können Sie einen Arbeitsbereich einmalig manuell über das Portal oder die CLI erstellen. Wenn Sie die CLI verwenden, erstellen Sie einen Arbeitsbereich mit dem folgenden Befehl: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Erstellen eines Experiments 


Je nach Szenario können Sie ein Experiment einmalig manuell über das Portal oder die CLI erstellen. Wenn Sie die CLI verwenden, erstellen Sie ein Experiment mit dem folgenden Befehl: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Erstellen von Clustern, Dateiservern und Aufträgen
 
Wenn Sie das Portal zum Erstellen von Aufträgen, Clustern oder Dateiservern verwenden, führt Sie das Portal durch die Erstellung der Parameter für Arbeitsbereichsname und Experimentname.

Um Aufträge, Cluster oder Dateiserver über das aktualisierte SDK zu erstellen, geben Sie den Parameter für den Arbeitsbereichsnamen an. Wenn Sie das Python-SDK verwenden, werden relevante Änderungen in den folgenden Beispielen gezeigt. Ersetzen Sie *workspace_name* und *experiment_name* durch den Arbeitsbereich und das Experiment, die Sie zuvor erstellt haben. Die Änderungen sind in den anderen Batch AI-SDKs ähnlich. 


#### <a name="create-cluster"></a>Erstellen eines Clusters 

```python
_ = client.clusters.create(resource_group_name, workspace_name, cluster_name, cluster_create_parameters).result()
```

#### <a name="create-file-server"></a>Erstellen einer Dateifreigabe 

```python
_ = client.fileservers.create(resource_group_name, workspace_name, fileserver_name, fileserver_create_parameters).result()
```

#### <a name="create-job"></a>Erstellen eines Auftrags 

```python
_ = client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name job_create_parameters).result()
```


## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die Batch AI-API-Referenz: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [Node.js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai) und [REST](/rest/api/batchai)