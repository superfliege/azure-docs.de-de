---
title: Containerworkloads in Azure Batch | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anwendungen aus Containerimages in Azure Batch ausführen.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 10/24/2018
ms.author: danlep
ms.openlocfilehash: 458b0f7bbf581c7f2490a8122f351dac612b4ff0
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155617"
---
# <a name="run-container-applications-on-azure-batch"></a>Ausführen von Containeranwendungen in Azure Batch

Mit Azure Batch können Sie eine große Anzahl von Batchcomputingaufträgen in Azure ausführen und skalieren. Stapelverarbeitungsaufgaben lassen sich direkt auf virtuellen Computern (Knoten) in einem Batch-Pool ausführen, doch Sie können auch einen Batch-Pool zum Ausführen von Aufgaben in Docker-kompatiblen Containern auf den Knoten einrichten. In diesem Artikel wird erläutert, wie Sie einen Pool von Computeknoten erstellen, die das Ausführen von Containeraufgaben unterstützen. Außerdem erfahren Sie, wie Sie anschließend Containeraufgaben im Pool ausführen. 

Sie sollten mit Containerkonzepten sowie dem Erstellen von Batch-Pools und -aufträgen vertraut sein. In den Codebeispielen werden der .NET-Batch und Python-SDKs verwendet. Sie können auch andere Batch-SDKs und -Tools, einschließlich des Azure-Portals, verwenden, um containerfähige Batch-Pools zu erstellen und Containeraufgaben auszuführen.

## <a name="why-use-containers"></a>Gründe für die Verwendung von Containern

Die Verwendung von Containern bietet eine einfache Möglichkeit zum Ausführen von Batch-Aufgaben, ohne eine Umgebung und Abhängigkeiten zum Ausführen von Anwendungen verwalten zu müssen. Container stellen Anwendungen als einfache, portable, eigenständige Einheiten bereit, die in vielen verschiedenen Umgebungen ausgeführt werden können. Sie können z. B. einen Container lokal erstellen und testen und dann das Containerimage in eine Registrierung in Azure oder an anderer Stelle hochladen. Das Containerbereitstellungsmodell stellt sicher, dass die Laufzeitumgebung der Anwendung immer ordnungsgemäß installiert und konfiguriert ist, und zwar ganz unabhängig davon, wo Sie die Anwendung hosten. Containerbasierte Vorgänge in Batch können auch Funktionen von Nicht-Containeraufgaben nutzen, z.B. Anwendungspakete und die Verwaltung von Ressourcendateien und Ausgabedateien. 

## <a name="prerequisites"></a>Voraussetzungen

* **SDK-Versionen**: Die Batch SDKs unterstützen Containerimages ab den folgenden Versionen:
    * Batch REST API, Version 2017-09-01.6.0
    * Batch .NET SDK, Version 8.0.0
    * Batch Python SDK, Version 4.0
    * Batch Java SDK, Version 3.0
    * Batch Node.js SDK, Version 3.0

* **Konten**: Sie müssen in Ihrem Azure-Abonnement ein Batch-Konto und optional ein Azure Storage-Konto erstellen.

* **Ein unterstütztes VM-Image**: Container werden nur in Pools unterstützt, die mit der Konfiguration des virtuellen Computers aus Images erstellt wurden, die im folgenden Abschnitt „Unterstützte Images virtueller Computer“ angegeben sind. Wenn Sie ein benutzerdefiniertes Image bereitstellen, finden Sie hierzu Überlegungen im folgenden Abschnitt und Anforderungen unter [Verwenden eines verwalteten benutzerdefinierten Images zum Erstellen eines VM-Pools](batch-custom-images.md). 

### <a name="limitations"></a>Einschränkungen

* Batch bietet RDMA-Unterstützung nur für Container, die in Linux-Pools ausgeführt werden.

* Für Windows-Containerworkloads wird empfohlen, eine Größe für VMs mit mehreren Kernen für Ihren Pool auszuwählen.

## <a name="supported-virtual-machine-images"></a>Unterstützte Images virtueller Computer

Sie müssen eines der folgenden unterstützten Windows- oder Linux-Images verwenden, um einen Pool mit VM-Computeknoten für Containerworkloads zu erstellen. Weitere Informationen zu Marketplace-Images, die mit Batch kompatibel sind, finden Sie in der [Liste mit VM-Images](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Windows-Images

Für Windows-Containerworkloads unterstützt Batch derzeit das Image **Windows Server 2016 Datacenter mit Containern** in Azure Marketplace. Nur die Docker-Containerimages werden unter Windows unterstützt.

Sie können auch benutzerdefinierte Images über VMs erstellen, die Docker unter Windows ausführen.

### <a name="linux-images"></a>Linux-Images

Für Linux-Containerworkloads unterstützt Batch derzeit die folgenden von Microsoft Azure Batch in Azure Marketplace veröffentlichten Linux-Images:

* **CentOS für Azure Batch-Container-Pools**

* **CentOS (mit RDMA-Treibern) für Azure Batch-Container-Pools**

* **Ubuntu Server für Azure Batch-Container-Pools**

* **Ubuntu Server (mit RDMA-Treibern) für Azure Batch-Container-Pools**

Diese Images werden nur für die Verwendung in Azure Batch-Pools unterstützt. Sie bieten:

* Eine vorinstallierte [Moby](https://github.com/moby/moby)-Containerlaufzeit 

* Vorinstallierte NVIDIA GPU-Treiber zum Optimieren der Bereitstellung auf Azure-VMs der N-Serie

* Images mit oder ohne vorinstallierte RDMA-Treiber; durch diese Treiber wird Poolknoten bei der Bereitstellung auf RDMA-fähigen VM-Größen der Zugriff auf das RDMA-Netzwerk von Azure ermöglicht  

Sie können auch benutzerdefinierte Images über VMs erstellen, die Docker unter einer der Linux-Distributionen ausführen, die mit Batch kompatibel sind. Wenn Sie ein eigenes benutzerdefiniertes Linux-Image bereitstellen möchten, finden Sie Anleitungen dazu unter [Verwenden eines verwalteten benutzerdefinierten Images zum Erstellen eines VM-Pools](batch-custom-images.md).

Für Docker-Unterstützung für ein benutzerdefiniertes Image installieren Sie [Docker Community Edition (CE)](https://www.docker.com/community-edition) oder [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Weitere Überlegungen zum Verwenden eines benutzerdefinierten Linux-Images:

* Wenn Sie die GPU-Leistung von Azure-Größen der N-Serie bei der Verwendung eines benutzerdefinierten Images optimal nutzen möchten, installieren Sie im Voraus NVIDIA-Treiber. Außerdem müssen Sie das Docker-Engine-Hilfsprogramm für NVIDIA-GPUs, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker), installieren.

* Verwenden Sie für den Zugriff auf das RDMA-Netzwerk von Azure eine RDMA-fähige VM-Größe. Erforderliche RDMA-Treiber sind in den von Batch unterstützten CentOS-HPC- und Ubuntu-Images installiert. Zum Ausführen von MPI-Workloads können zusätzliche Konfigurationsschritte erforderlich sein. Informationen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>Containerkonfiguration für Batch-Pool

Damit ein Batch-Pool Containerworkloads ausführen kann, müssen Sie [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration)-Einstellungen im [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration)-Objekt des Pools festlegen. (Dieser Artikel enthält Links zur .NET-API-Referenz von Batch. Die entsprechenden Einstellungen befinden sich in der [Batch Python](/python/api/azure.batch)-API.)

Sie können einen containerfähigen Pool mit oder ohne vorabgerufene Containerimages erstellen, wie in den folgenden Beispielen gezeigt. Mithilfe des Pull-Vorgangs (oder Vorabrufs) können Sie Containerimages entweder über Docker Hub oder eine andere Containerregistrierung im Internet vorab laden. Verwenden Sie für optimale Leistung eine [Azure-Containerregistrierung](../container-registry/container-registry-intro.md) in der gleichen Region wie das Batch-Konto.

Der Vorteil vorabgerufener Containerimages ist, dass bei der ersten Ausführung von Aufgaben diese nicht auf das Herunterladen des Containerimages warten müssen. Die Containerkonfiguration führt beim Erstellen des Pools eine Pullübertragung von Containerimages auf die virtuellen Computer durch. Für den Pool ausgeführte Aufgaben können dann auf die Liste der Containerimages und auf die Containerausführungsoptionen verweisen.


### <a name="pool-without-prefetched-container-images"></a>Pool ohne vorabgerufene Containerimages

Zum Konfigurieren eines containerfähigen Pools ohne vorabgerufene Containerimages definieren Sie die Objekte `ContainerConfiguration` und `VirtualMachineConfiguration`, wie im folgenden Python-Beispiel gezeigt. In diesem Beispiel wird für das Azure Batch-Container-Pool-Image aus Marketplace der Ubuntu Server verwendet.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Vorabrufen von Images für die Containerkonfiguration

Zum Vorabrufen von Containerimages im Pool fügen Sie die Liste der Containerimages (`container_image_names`, in Python) zur `ContainerConfiguration` hinzu. 

Im folgenden grundlegenden Python-Beispiel wird gezeigt, wie ein standardmäßiges Ubuntu-Containerimage aus [Docker Hub](https://hub.docker.com) vorabgerufen wird.

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


Im folgenden C#-Beispiel wird davon ausgegangen, dass Sie vorab ein TensorFlow-Image über [Docker Hub](https://hub.docker.com) abrufen möchten. Dieses Beispiel enthält eine Startaufgabe, die auf den Poolknoten im VM-Host ausgeführt wird. Beispielsweise können Sie eine Startaufgabe auf dem Host ausführen, um einen Dateiserver einzubinden, der über die Container zugänglich ist.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Vorabrufen von Images aus einer privaten Containerregistrierung

Sie können Containerimages auch durch Authentifizierung bei einem Server für die Containerregistrierung vorabrufen. Beim folgenden Beispiel rufen die Objekte `ContainerConfiguration` und `VirtualMachineConfiguration` ein privates TensorFlow-Image aus einer privaten Azure-Containerregistrierung vorab ab. Der Bildverweis ist derselbe wie im vorherigen Beispiel.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## <a name="container-settings-for-the-task"></a>Containereinstellungen für die Aufgabe

Zur Ausführung von Containeraufgaben auf den Computeknoten müssen Sie containerspezifische Einstellungen angeben, z.B. Optionen für die Containerausführung, zu verwendende Images und die Registrierung.

Verwenden Sie die `ContainerSettings`-Eigenschaft der Aufgabenklassen zum Konfigurieren containerspezifischer Einstellungen. Diese Einstellungen werden durch die [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings)-Klasse definiert.

Wenn Sie Aufgaben in Containerimages ausführen, sind für die [Cloudaufgabe](/dotnet/api/microsoft.azure.batch.cloudtask) und die [Auftrags-Manager-Aufgabe](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) Containereinstellungen erforderlich. Die [Startaufgabe](/dotnet/api/microsoft.azure.batch.starttask), die [Auftragsvorbereitungsaufgabe](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask) und die [Auftragsfreigabeaufgabe](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) benötigen jedoch keine Containereinstellungen (diese können in einem Containerkontext oder direkt auf dem Knoten ausgeführt werden).

Die optionalen Optionen für die Containerausführung ([ContainerRunOptions](/dotnet/api/microsoft.azure.batch.taskcontainersettings.containerrunoptions)) sind zusätzliche Argumente für den Befehl `docker create`, der von der Aufgabe zum Erstellen des Containers ausgeführt wird.

### <a name="container-task-working-directory"></a>Arbeitsverzeichnis für Containeraufgaben

Die Befehlszeile für eine Azure Batch-Containeraufgabe wird in einem Arbeitsverzeichnis in dem Container ausgeführt, der sehr stark der Umgebung ähnelt, die von Batch für eine normale Aufgabe (Nicht-Containeraufgabe) eingerichtet wird:

* Alle Verzeichnisse rekursiv unter dem `AZ_BATCH_NODE_ROOT_DIR` (Stamm der Azure Batch-Verzeichnisse auf dem Knoten) werden dem Container zugeordnet.
* Alle Variablen der Aufgabenumgebung werden dem Container zugeordnet.
* Das Arbeitsverzeichnis der Anwendung ist wie für eine normale Aufgabe festgelegt, sodass Sie Funktionen wie Anwendungspakete und Ressourcendateien verwenden können.

Da das Standardarbeitsverzeichnis im Container von Batch geändert wird, wird die Aufgabe an einem anderen Ort als dem typischen Containerarbeitsverzeichnis ausgeführt (z.B. standardmäßig `c:\` in einem Windows-Container bzw. `/` unter Linux oder in einem anderen Verzeichnis, wenn dies im Containerimage konfiguriert ist). Führen Sie eines der folgenden Verfahren aus, um sicherzustellen, dass Ihre Containeranwendungen im Batch-Kontext korrekt ausgeführt werden: 

* Stellen Sie sicher, dass in der Aufgabenbefehlszeile (oder im Containerarbeitsverzeichnis) ein absoluter Pfad angegeben wird, sofern nicht bereits eine entsprechende Konfiguration vorliegt.

* Legen Sie unter dem Element „ContainerSettings“ der Aufgabe in den Optionen für die Containerausführung ein Arbeitsverzeichnis fest. Beispiel: `--workdir /app`.

Der folgende Python-Codeausschnitt zeigt eine grundlegende Befehlszeile, die in einem aus Docker Hub gepullten Ubuntu-Container ausgeführt wird. Hier entfernt die Option `--rm` für die Containerausführung den Container, nachdem die Aufgabe abgeschlossen ist.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/echo hello',
    container_settings=task_container_settings
)

```

Im folgenden Beispiel C# werden die grundlegenden Containereinstellungen für eine Cloudaufgabe gezeigt:

```csharp
// Simple container task command

string cmdLine = "c:\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich auch über das Toolkit [Batch Shipyard](https://github.com/Azure/batch-shipyard) für die einfache Bereitstellung von Containerworkloads in Azure Batch durch [Shipyard-Rezepte](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Weitere Informationen zur Installation und Verwendung von Docker CE unter Linux finden Sie in der [Docker](https://docs.docker.com/engine/installation/)-Dokumentation.

* Weitere Informationen zur Verwendung benutzerdefinierter Images finden Sie unter [Verwenden eines verwalteten benutzerdefinierten Images zum Erstellen eines VM-Pools](batch-custom-images.md).

* Informieren Sie sich über das [Moby-Projekt](https://mobyproject.org/), ein Framework zum Erstellen containerbasierter Systeme.