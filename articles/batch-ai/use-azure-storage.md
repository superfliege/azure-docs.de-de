---
title: Speichern von Batch AI-Auftragseingabe und -ausgabe mit Azure Storage | Microsoft Docs
description: Verwenden von Azure Storage mit Batch AI für schnelle und einfache Cloudspeicherung von Eingabe- und Ausgabedateien
services: batch-ai
documentationcenter: ''
author: kevwang1
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 42697f7f4bb8c6b9ef785eef0fe2f5f33b2b38a7
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615602"
---
# <a name="store-batch-ai-job-input-and-output-with-azure-storage"></a>Speichern von Batch AI-Auftragseingabe und -ausgabe mit Azure Storage

In diesem Leitfaden wird beschrieben, wie Sie Azure Storage zum Speichern von Ein- und Ausgabedateien beim Ausführen eines Auftrags verwenden. Azure Storage ist eine von mehreren Speicheroptionen, die von Batch AI unterstützt werden. Batch AI kann in Azure Storage integriert werden, indem Azure Storage-Systeme für ein Batch AI-Auftrags- oder -Clusterdateisystem bereitgestellt werden. Dies ermöglicht einen nahtlosen Zugriff auf in der Cloud gespeicherte Dateien. 

## <a name="introduction-to-azure-storage"></a>Einführung in Azure Storage

Azure Storage ist die Cloudspeicherlösung von Microsoft. Batch AI unterstützt die Bereitstellung von Azure Blob-Containern und Azure-Dateifreigaben in Batch AI-Aufträgen oder -Clustern, sodass von einem Auftrag aus so auf Dateien zugegriffen werden kann, als würden sie sich im nativen Dateisystem befinden. Batch AI stellt Azure Blob-Container mit [blobfuse](https://github.com/Azure/azure-storage-fuse) und Azure-Dateifreigaben über das SMB-Protokoll bereit. Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Azure Storage](../storage/common/storage-introduction.md).

## <a name="store-datasets-and-input-scripts-in-azure-storage"></a>Speichern von Datasets und Eingabeskripts in Azure Storage

Wenn Sie Azure Storage für Ihre Batch AI-Umgebung auswählen, wird empfohlen, Ihre Eingabedateien (z.B. Datasets) in einem Blob-Container (der einen höheren Durchsatz aufweist) und Ihre Trainingsausgabe in einer Dateifreigabe zu speichern, die Streaming unterstützt (und somit das Lesen von Ausgabeprotokollen ermöglicht, während der Auftrag gleichzeitig ausgeführt wird). 

Bevor Sie Azure Storage verwenden können, müssen Sie [ein Azure Storage-Konto erstellen](../storage/common/storage-quickstart-create-account.md). Batch AI unterstützt die Bereitstellung von Volumes aus Azure Storage GPv1- (General Purpose v1) und GPv2-Konten (General Purpose v2). Das Azure Storage-Konto kann mehrere Blob-Container oder Dateifreigabeinstanzen enthalten. Berücksichtigen Sie bei der Auswahl des Typs des zu erstellenden Speicherkontos Ihre Kosten- und Leistungsanforderungen. Weitere Informationen finden Sie unter [Azure-Speicherkonto – Übersicht](../storage/common/storage-account-overview.md). 

Um einen Blob-Container zu erstellen und Ihr Dataset in einen Azure Blob-Container hochzuladen, wählen Sie eine der folgenden Methoden aus:
- [Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md) für das Hochladen mit einer webbasierten grafischen Benutzeroberfläche (GUI). Um eine kleine Anzahl von Dateien hochzuladen, stellt das Azure-Portal die einfachste Vorgehensweise dar.
- [Azure Storage-CLI](../storage/blobs/storage-quickstart-blobs-cli.md) für das Hochladen über die Befehlszeile (unterstützt Verzeichnisupload). Verwenden Sie zum Hochladen von Verzeichnissen mit Dateien `az storage blob upload-batch`.
- [Andere Techniken](../storage/common/storage-moving-data.md) (einschließlich der Verwendung von Anwendungs-SDKs).

Um eine Azure-Dateifreigabe zu erstellen, wählen Sie analog dazu eine der folgenden Methoden aus:
- [Azure-Portal](../storage/files/storage-how-to-use-files-portal.md)
- [Azure Storage-CLI](../storage/files/storage-how-to-use-files-cli.md)
- [Andere Techniken](../storage/common/storage-moving-data.md)

### <a name="auto-storage-with-batch-ai"></a>Automatische Speicherung mit Batch AI

Alternativ können Sie ein Azure Storage-Konto mit einer Azure-Dateifreigabe und einem Blob-Container erstellen (und diese Volumes automatisch für einen Batch AI-Cluster bereitstellen), indem Sie den Parameter `--use-auto-storage` mit `az batchai cluster create` verwenden. Weitere Informationen finden Sie [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#auto-storage-account).

## <a name="mount-azure-storage"></a>Bereitstellen von Azure Storage 

### <a name="mount-volumes-to-a-job"></a>Bereitstellen von Volumes für einen Auftrag

Das Bereitstellen eines Azure Storage-Volumes ermöglicht den Zugriff über das für jeden Auftrag erstellte Dateisystem. Daher kann ein Auftrag Dateien nahtlos so aus dem Cloudspeicher lesen und in ihn schreiben, als handelte es sich um lokale Dateien.

Um ein Azure Storage-Volume für einen mit der Azure CLI erstellten Auftrag bereitzustellen, verwenden Sie die Eigenschaft `mountVolumes` in Ihrer Datei `job.json`, wenn Sie `az batchai job create` ausführen. Ein Beispiel finden Sie in der [Anleitung für verteilte TensorFlow-GPUs](https://github.com/Azure/BatchAI/blob/master/recipes/TensorFlow/TensorFlow-GPU-Distributed/job.json). Das Schema für `mountVolumes` ist:
```json
{
    "mountVolumes": {
        "azureFileShares": [{
            "azureFileUrl": "https://<STORAGE_ACCOUNT_NAME>.file.core.windows.net/<FILE_SHARE_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }],
        "azureBlobFileSystems": [{
            "accountName": "<STORAGE_ACCOUNT_NAME>",
            "containerName": "<BLOB_CONTAINER_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }]
    }
}
```

`azureFileShares` und `azureBlobFileSystems` sind beides Arrays von Objekten, die die bereitzustellenden Volumes darstellen. Beschreibungen der Platzhalter:

- <RELATIVE_MOUNT_PATH>: Das Volume wird in diesem Pfad bereitgestellt. Wenn `relativeMountPath` z.B. `jobs` ist, befindet sich das Volume unter `$AZ_BATCHAI_JOB_MOUNT_ROOT/jobs`).
- <STORAGE_ACCOUNT_NAME>: Der Name des Azure Storage-Kontos, das die Dateifreigabe oder den Blob-Container enthält.
- <FILE_SHARE_NAME>: Der Name der Dateifreigabe.
- <BLOB_CONTAINER_NAME>: Der Name des Blob-Containers.

Um Azure Storage-Volumes mit den Azure Batch AI SDKs bereitzustellen, legen Sie die `mount_volumes`-Eigenschaft (Python) oder `MountVolumes`-Eigenschaft (C#, Java) auf `JobCreateParameters` fest. Sie müssen die Anmeldeinformationen des Speicherkontos angeben, wenn Sie Volumes mit den Azure Batch AI SDKs bereitstellen. Zeigen Sie die Schemas für das Bereitstellen von Volumes in [Python](https://docs.microsoft.com/python/api/azure-mgmt-batchai/azure.mgmt.batchai.models.MountVolumes?view=azure-python), [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.batchai.models.mountvolumes?view=azure-dotnet) und [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.management.batchai._mount_volumes?view=azure-java-stable) an.

### <a name="mount-volumes-to-a-cluster"></a>Bereitstellen von Volumes für einen Cluster

Batch AI unterstützt auch das Bereitstellen von Azure Storage-Volumes für einen Batch AI-Cluster. Wenn ein Volume für einen Cluster bereitgestellt wird, können alle Aufträge, die auf diesem Cluster ausgeführt werden, für diesen Cluster bereitgestellte Volumes verwenden. Während die Bereitstellung auf Auftragsebene die größte Flexibilität bietet (jeder Auftrag kann über verschiedene bereitgestellte Volumes verfügen), kann die Bereitstellung auf Clusterebene in einfachen Szenarien ausreichend sein.

Um ein Azure Storage-Volume für einen mit der Azure CLI erstellten Cluster bereitzustellen, verwenden Sie die Eigenschaft `mountVolumes` in Ihrer Datei `cluster.json`, wenn Sie `az batchai cluster create` ausführen. Das Schema für `mountVolumes` beim Bereitstellen eines Clusters entspricht dem Schema beim Bereitstellen eines Auftrags. 

Auf ähnliche Weise können Sie die `mount_volumes`- (Python) oder `MountVolumes`-Eigenschaft (C#, Java) für `ClusterCreateParameters` bei der Bereitstellung mit den Azure Batch AI SDKs verwenden. 

## <a name="access-mounted-filesystem-in-a-job"></a>Zugreifen auf ein bereitgestelltes Dateisystem in einem Auftrag

### <a name="azbatchaijobmountroot-environment-variable"></a>$AZ_BATCHAI_JOB_MOUNT_ROOT-Umgebungsvariable

Innerhalb der Ausführungsumgebung des Auftrags kann mit der Umgebungsvariablen `$AZ_BATCHAI_JOB_MOUNT_ROOT` auf das Verzeichnis mit den bereitgestellten Speichersystemen zugegriffen werden (wenn Sie Bereitstellung auf Auftragsebene verwenden). Wenn Sie Bereitstellung auf Clusterebene verwenden, lautet diese Umgebungsvariable `$AZ_BATCHAI_MOUNT_ROOT`. In den folgenden Beispielen wird davon ausgegangen, dass Sie Bereitstellung auf Auftragsebene verwenden.

Um den Pfad der Daten in einem bereitgestellten Volume anzugeben, verwenden Sie die Umgebungsvariable `$AZ_BATCHAI_JOB_MOUNT_ROOT` zusammen mit den bereitgestellten Pfad. Wenn das Trainingsskript `train.py` z.B. in eine Azure-Dateifreigabe hochgeladen wurde, die im relativen Bereitstellungspfad `scripts` bereitgestellt wird, ist die Datei unter `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py` verfügbar.

Wenn Ihr Trainingsskript Informationen zu einem Pfad benötigt, sollten Sie ihn als Befehlszeilenargument übergeben. Wenn Sie Ihre Daten beispielsweise in einem Ordner namens `train_data` in einem Azure Blob-Container gespeichert haben, der im Pfad `data` bereitgestellt wird, können Sie `--data-dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/data/train_data` als Befehlszeilenargument an Ihr Skript übergeben. Dementsprechend müssen Sie Ihr Skript so ändern, dass es Befehlszeilenargumente annimmt.

### <a name="abbreviate-input-paths"></a>Abkürzen von Eingabepfaden

Um Eingabepfade als Umgebungsvariable abzukürzen, verwenden Sie die `inputDirectories`-Eigenschaft Ihrer Datei `job.json` (oder `models.JobCreateParameters.input_directories` bei Verwendung des Batch AI SDK). Das Schema von `inputDirectories` ist:

```json
{
    "inputDirectories": [{
        "id": "<ID>",
        "path": "<PATH>" 
    }]
}
```

Jeder angegebene Pfad wird in einer Umgebungsvariablen namens `$AZ_BATCHAI_INPUT_<ID>` platziert. Durch die Verwendung dieser Methode können die Pfade zu Eingabedateien oder Verzeichnissen vereinfacht werden. Um beispielsweise den Pfad zu einem Trainingsskript abzukürzen: Wenn `"id"` `"SCRIPT"` und `"path"` `"$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py"` ist, dann ist dieser Pfad unter `$AZ_BATCHAI_INPUT_SCRIPT` in der Ausführungsumgebung des Auftrags verfügbar.

Weitere Informationen finden Sie [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#input-directories).

### <a name="abbreviate-output-paths"></a>Abkürzen von Ausgabepfaden

Um Ausgabepfade als Umgebungsvariable abzukürzen, verwenden Sie die `outputDirectories`-Eigenschaft Ihrer Datei `job.json` (oder `models.JobCreateParameters.output_directories` bei Verwendung des Batch AI SDK). Mit dieser Methode können Sie die Pfade für Ausgabedateien vereinfachen. Das Schema von `outputDirectories` ist:

```json
{
    "outputDirectories": [{
        "id": "<ID>",
        "pathPrefix": "<PATH_PREFIX>",
        "pathSuffix": "<PATH_SUFFIX>"
    }]
}
```

Jeder angegebene Pfad wird in einer Umgebungsvariablen namens `$AZ_BATCHAI_OUTPUT_<ID>` platziert. Das `pathPrefix` bestimmt das bereitgestellte Volume zum Speichern der Ausgabe (z.B. `"$AZ_BATCHAI_JOB_MOUNT_ROOT/output"`). Das `pathSuffix` bestimmt den Ordnernamen der Ausgabe (z.B. `"logs"`, `"checkpoints"`). Der tatsächliche Pfad der Ausgabe ist eine Verkettung von `pathPrefix`, `jobOutputDirectoryPathSegment` (automatisch für jeden Auftrag generiert) und `pathSuffix`.

Weitere Informationen finden Sie [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#output-directories).

## <a name="retrieve-job-output-from-azure-storage"></a>Abrufen der Auftragsausgabe aus Azure Storage

### <a name="use-azure-portal"></a>Verwenden des Azure-Portals

Das Azure-Portal bietet eine komfortable Möglichkeit, die Ausgabe von Aufträgen mit Hilfe eines GUI-Datei-Explorers anzuzeigen. Wenn Sie die Ausgabe jedoch aus Stdout und Stderr oder aus einem Pfad in `outputDirectories` anzeigen möchten, werden die Dateien in einem automatisch generierten Pfad in Ihrem Azure Storage-Volume platziert. Weitere Informationen finden Sie unten.

### <a name="access-stdout-and-stderr-output"></a>Zugreifen auf die Stdout- und Stderr-Ausgabe

Verwenden Sie die `stdOutErrPathPrefix`-Eigenschaft von `job.json`, um den Auftrag anzuweisen, wo die Ausführungsprotokolle des Auftrags und die Stdout- und Stderr-Ausgabe platziert werden sollen. Wenn Sie beispielsweise eine Dateifreigabe im relativen Bereitstellungspfad `outputs` bereitgestellt haben und das `stdOutErrPathPrefix` als `"$AZ_BATCHAI_JOB_MOUNT_ROOT/outputs"` angeben, ist die Stdout- und Stderr-Auftragsausgabe unter `{subscription id}/{resource group}/workspaces/{workspace name}/experiments/{experiment name}/jobs/{job name}/{job uuid}/stdouterr` in diesem bereitgestellten Volume verfügbar. Dieser automatisch generierte-Pfad wird verwendet, um Ausgabekonflikte zwischen Aufträgen mit dem gleichen Namen zu verhindern.

Weitere Informationen finden Sie [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#standard-and-error-output).

### <a name="list-the-output-files"></a>Auflisten der Ausgabedateien

Sie können die Azure CLI zum Auflisten der verfügbaren Ausgabedateien eines Auftrags mit dem Befehl `az batchai job file list` verwenden. Verwenden Sie zum Auflisten der Dateien im Standardausgabeverzeichnis eines Auftrags z.B. `az batchai job file list -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME>`.

Weitere Informationen und Beispiele finden Sie [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

### <a name="stream-output-files"></a>Streamen von Ausgabedateien

Sie können die Azure CLI zum Streamen von Dateien mit dem Befehl `az batchai job file stream` verwenden. Beispielsweise zum Anzeigen von:
- Stdout: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stdout.txt`
- Stderr: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stderr.txt`

Weitere Informationen und Beispiele finden Sie [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu CLI-Befehlen für die Verwendung mit Azure Storage finden Sie in der [Azure Batch AI-CLI-Dokumentation](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md).
- Weitere Verwendungsbeispiele für Batch AI (einschließlich der Bereitstellung von Speicher und dem Lesen der Ausgabedateien) finden Sie in den [Jupyter Notebook-Anleitungen für Batch AI](https://github.com/Azure/BatchAI).
- Informieren Sie sich über weitere Optionen für die Bereitstellung von Speicher (einschließlich [Bereitstellung eines NFS-Servers](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs) und [Bereitstellung eines eigenen NFS, Cifs oder GlusterFS-Clusters](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs)).