---
title: Docker-Containerworkloads in Azure Batch | Microsoft Docs
description: "Erfahren Sie, wie Sie Anwendungen aus Docker-Containerimages in Azure Batch ausführen."
services: batch
author: v-dotren
manager: timlt
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 11/15/2017
ms.author: v-dotren
ms.openlocfilehash: fc15b2db051b5ebbf39665b803b22d3a5e4885f9
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="run-docker-container-applications-on-azure-batch"></a>Ausführen von Docker-Containeranwendungen in Azure Batch

Mit Azure Batch können Sie eine sehr große Anzahl von Batchcomputingaufträgen in Azure ausführen und skalieren. Bisher wurden Batch-Aufgaben direkt auf virtuellen Computern (VMs) in einem Batch-Pool ausgeführt, doch nun können Sie einen Batch-Pool zum Ausführen von Aufgaben in Docker-Containern einrichten.

Die Verwendung von Containern bietet eine einfache Möglichkeit zum Ausführen von Batch-Aufgaben, ohne Anwendungspakete und Abhängigkeiten verwalten zu müssen. Container stellen Anwendungen als einfache, portable, eigenständige Einheiten bereit, die in einer Vielzahl von Umgebungen ausgeführt werden können. Sie können z. B. einen Container lokal erstellen und testen und dann das Containerimage in eine Registrierung in Azure oder an anderer Stelle hochladen. Das Containerbereitstellungsmodell stellt sicher, dass die Laufzeitumgebung der Anwendung immer ordnungsgemäß installiert und konfiguriert ist, und zwar unabhängig davon, wo Sie die Anwendung hosten. In diesem Tutorial wird erläutert, wie Sie das Batch .NET SDK zum Erstellen eines Pools von Computeknoten verwenden, die das Ausführen von Containeraufgaben unterstützen. Außerdem erfahren Sie, wie Sie Containeraufgaben im Pool ausführen.

Dieser Artikel setzt Kenntnisse über die Docker-Containerkonzepte und das Erstellen eines Batch-Pools und -Auftrags mit dem .NET SDK voraus. Die Codeausschnitte sind für die Verwendung in einer Clientanwendung ähnlich dem [DotNetTutorial-Beispiel](batch-dotnet-get-started.md) gedacht und sind Beispiele für Code, der zur Unterstützung von Containeranwendungen in Batch erforderlich ist.


## <a name="prerequisites"></a>Voraussetzungen

* SDK-Versionen: Die Batch SDKs unterstützen Containerimages in den folgenden Versionen:
    * Batch REST API, Version 2017-09-01.6.0
    * Batch .NET SDK, Version 8.0.0
    * Batch Python SDK, Version 4.0
    * Batch Java SDK, Version 3.0
    * Batch Node.js SDK, Version 3.0

* Konten: Sie müssen in Ihrem Azure-Konto ein Batch-Konto und optional ein allgemeines Speicherkonto erstellen.

* Ein unterstütztes VM-Image. Container werden nur in Pools unterstützt, die mit der Konfiguration des virtuellen Computers aus Images erstellt wurden, die im folgenden Abschnitt „Unterstützte Images virtueller Computer“ angegeben sind.

* Wenn Sie ein benutzerdefiniertes Image bereitstellen, muss Ihre Anwendung die Azure Active Directory-Authentifizierung (Azure AD) verwenden, um containerbasierte Workloads auszuführen. Wenn Sie ein Azure Marketplace-Image verwenden, benötigen Sie keine Azure AD-Authentifizierung, sondern eine Authentifizierung mit gemeinsam verwendetem Schlüssel. Die Azure Batch-Unterstützung für Azure AD ist unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md) dokumentiert.


## <a name="supported-virtual-machine-images"></a>Unterstützte Images virtueller Computer

Sie müssen ein Windows- oder Linux-Image bereitstellen, um einen Pool mit VM-Computeknoten zu erstellen.

### <a name="windows-images"></a>Windows-Images

Für Windows-Containerworkloads unterstützt Batch derzeit benutzerdefinierte Images, die Sie von VMs erstellen, auf denen Docker unter Windows ausgeführt wird, oder Sie können das Image „Windows Server 2016 Datacenter mit Containern“ aus dem Azure Marketplace verwenden. Dieses Image ist mit der Knoten-Agent-SKU-ID von `batch.node.windows amd64` kompatibel. Der unterstützte Containertyp ist derzeit auf Docker beschränkt.

### <a name="linux-images"></a>Linux-Images

Für Linux-Containerworkloads unterstützt Batch derzeit nur benutzerdefinierte Images, die Sie von VMs erstellen, auf denen Docker unter den folgenden Linux-Distributionen ausgeführt wird: Ubuntu 16.04 LTS oder CentOS 7.3. Wenn Sie ein eigenes benutzerdefiniertes Linux-Image bereitstellen möchten, finden Sie Anleitungen dazu unter [Verwenden eines verwalteten benutzerdefinierten Images zum Erstellen eines VM-Pools](batch-custom-images.md).

Sie können [Docker Community Edition (CE)](https://www.docker.com/community-edition) oder [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition) verwenden.

Wenn Sie die GPU-Leistung von virtuellen Azure-NC- oder -NV-Computern nutzen möchten, müssen Sie NVIDIA-Treiber auf dem Image installieren. Außerdem müssen Sie das Docker-Modul-Hilfsprogramm für NVIDIA-GPUs, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker), installieren und ausführen.

Verwenden Sie für den Zugriff auf das Azure RDMA-Netzwerk virtuelle Computer der folgenden Größen: A8, A9, H16r, H16mr oder NC24r. Erforderliche RDMA-Treiber werden in den CentOS 7.3 HPC- und Ubuntu 16.04 LTS-Images aus dem Azure Marketplace installiert. Zum Ausführen von MPI-Workloads können zusätzliche Konfigurationsschritte erforderlich sein. Informationen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).


## <a name="limitations"></a>Einschränkungen

* Batch bietet RDMA-Unterstützung nur für Container, die in Linux-Pools ausgeführt werden.


## <a name="authenticate-using-azure-active-directory"></a>Authentifizieren mit Azure Active Directory

Wenn Sie ein benutzerdefiniertes VM-Image zum Erstellen des Batch-Pools verwenden, muss Ihre Clientanwendung eine Authentifizierung anhand der in Azure AD integrierten Authentifizierung vornehmen (eine Authentifizierung mit gemeinsam verwendetem Schlüssel kann nicht verwendet werden). Stellen Sie vor dem Ausführen der Anwendung sicher, dass Sie sie in Azure AD registrieren, um eine Identität dafür einzurichten und die jeweiligen Berechtigungen für andere Anwendungen anzugeben.

Wenn Sie ein benutzerdefiniertes VM-Image verwenden, müssen Sie der Anwendung außerdem die IAM-Zugriffssteuerung für den Zugriff auf das VM-Image erteilen. Öffnen Sie im Azure-Portal **Alle Ressourcen**, wählen Sie das Containerimage aus, und klicken Sie auf dem Imageblatt im Abschnitt **Zugriffssteuerung (IAM)** auf **Hinzufügen**. Geben Sie auf dem Blatt **Berechtigungen hinzufügen** eine **Rolle** an, wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus, und geben Sie dann unter **Auswählen** den Anwendungsnamen ein.

Übergeben Sie in Ihrer Anwendung ein Azure AD-Authentifizierungstoken, wenn Sie den Batch-Client mit [BatchClient.Open](/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_) erstellen, wie es unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md) beschrieben ist.


## <a name="reference-a-vm-image-for-pool-creation"></a>Verweisen auf ein VM-Image zur Poolerstellung

Geben Sie in Ihrem Anwendungscode einen Verweis auf das VM-Image an, das beim Erstellen der Computeknoten des Pools verwendet werden soll. Dazu erstellen Sie ein [ImageReference](/dotnet/api/microsoft.azure.batch.imagereference)-Objekt. Sie können das zu verwendende Image auf eine der folgenden Arten angeben:

* Wenn Sie ein benutzerdefiniertes Image verwenden, geben Sie einen Azure Resource Manager-Ressourcenbezeichner für das Image des virtuellen Computers an. Der Imagebezeichner weist ein Pfadformat wie im folgenden Beispiel auf:

  ```csharp
  // Provide a reference to a custom image using an image ID
  ImageReference imageReference = new ImageReference("/subscriptions/<subscription-ID>/resourceGroups/<resource-group>/providers/Microsoft.Compute/images/<imageName>");
  ```

    Zum Abrufen dieser Image-ID aus dem Azure-Portal öffnen Sie **Alle Ressourcen**, wählen Sie das benutzerdefinierte Image aus, und kopieren Sie auf dem Imageblatt im Abschnitt **Übersicht** den Pfad in **Ressourcen-ID**.

* Wenn Sie ein [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute?page=1&subcategories=windows-based)-Image verwenden, geben Sie eine Gruppe von Parametern zur Beschreibung des Images an: Angebotstyp, Herausgeber, SKU und Version des Images gemäß der [Liste mit VM-Images](batch-linux-nodes.md#list-of-virtual-machine-images):

  ```csharp
  // Provide a reference to an Azure Marketplace image for
  // "Windows Server 2016 Datacenter with Containers"
  ImageReference imageReference = new ImageReference(
    publisher: "MicrosoftWindowsServer",
    offer: "WindowsServer",
    sku: "2016-Datacenter-with-Containers",
    version: "latest");
  ```


## <a name="container-configuration-for-batch-pool"></a>Containerkonfiguration für Batch-Pool

Der Batch-Pool ist die Sammlung von Computeknoten, auf denen Batch die Aufgaben eines Auftrags ausführt. Beim Erstellen des Pools geben Sie die VM-Konfiguration für die Computeknoten an. Das [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration)-Objekt enthält einen Verweis auf das [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration)-Objekt. Um Containerworkloads im Pool zu aktivieren, geben Sie die `ContainerConfiguration`-Einstellungen an. In der VM-Konfiguration geben Sie auch die Imagereferenz und die Knoten-Agent-SKU-ID des Images an, wie es in den folgenden Beispielen gezeigt ist.

Es gibt mehrere Möglichkeiten der Poolerstellung. Sie können einen Pool mit oder ohne vorabgerufene Containerimages erstellen. 

Mithilfe des Pull-Vorgangs (oder Vorabrufs) können Sie Containerimages entweder über Docker Hub oder eine andere Containerregistrierung im Internet vorab laden. Der Vorteil vorabgerufener Containerimages ist, dass bei der ersten Ausführung von Aufgaben diese nicht auf das Herunterladen des Containerimages warten müssen. Die Containerkonfiguration führt beim Erstellen des Pools eine Pullübertragung von Containerimages auf die virtuellen Computer durch. Für den Pool ausgeführte Aufgaben können dann auf die Liste der Containerimages und auf die Containerausführungsoptionen verweisen.



### <a name="pool-without-prefetched-container-images"></a>Pool ohne vorabgerufene Containerimages

Zum Konfigurieren des Pools ohne vorabgerufene Containerimages verwenden Sie eine `ContainerConfiguration`, wie es im folgenden Beispiel gezeigt ist. Bei diesem und den folgenden Beispielen wird davon ausgegangen, dass Sie ein benutzerdefiniertes Ubuntu 16.04 LTS-Image mit installiertem Docker-Modul verwenden.

```csharp
// Specify container configuration
ContainerConfiguration containerConfig = new ContainerConfiguration(
    type: "Docker");

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

// Commit pool creation
pool.Commit();
```

### <a name="prefetch-images-for-container-configuration"></a>Vorabrufen von Images für die Containerkonfiguration

Zum Vorabrufen von Containerimages für den Pool fügen Sie die Liste der Containerimages (`containerImageNames`) zur Containerkonfiguration hinzu, und geben Sie der Imageliste einen Namen. Beim folgenden Beispiel wird davon ausgegangen, dass Sie ein benutzerdefiniertes Ubuntu 16.04 LTS-Image verwenden, ein TensorFlow-Image über [Docker Hub](https://hub.docker.com) vorabrufen und TensorFlow in einer Startaufgabe starten.

```csharp
// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    type: "Docker",
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Define container settings
TaskContainerSettings startTaskContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu");
StartTask startTaskContainer = new StartTask(
    CommandLine: "<docker-image-command-line>",
    TaskContainerSettings: startTaskContainerSettings);

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);

// Commit pool creation
pool.Commit();
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Vorabrufen von Images aus einer privaten Containerregistrierung

Sie können Containerimages auch durch Authentifizierung bei einem Server für die Containerregistrierung vorabrufen. Beim folgenden Beispiel wird davon ausgegangen, dass Sie ein benutzerdefiniertes Ubuntu 16.04 LTS-Image verwenden und ein privates TensorFlow-Image aus einer privaten Azure-Containerregistrierung vorabrufen.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    type: "Docker",
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

// Commit pool creation
pool.Commit();
```


## <a name="container-settings-for-the-task"></a>Containereinstellungen für die Aufgabe

Wenn Sie Aufgaben zur Ausführung auf den Computeknoten einrichten, müssen Sie containerspezifische Einstellungen wie z. B. Optionen für die Ausgabenausführung, zu verwendende Images und die Registrierung angeben.

Verwenden Sie die ContainerSettings-Eigenschaft der Aufgabenklassen zum Konfigurieren containerspezifischer Einstellungen. Diese Einstellungen werden durch die [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings)-Klasse definiert.

Wenn Sie Aufgaben in Containerimages ausführen, sind für die [Cloudaufgabe](/dotnet/api/microsoft.azure.batch.cloudtask) und die [Auftrags-Manager-Aufgabe](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) Containereinstellungen erforderlich. Die [Startaufgabe](/dotnet/api/microsoft.azure.batch.starttask), die [Auftragsvorbereitungsaufgabe](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask) und die [Auftragsfreigabeaufgabe](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) benötigen jedoch keine Containereinstellungen (diese können in einem Containerkontext oder direkt auf dem Knoten ausgeführt werden).

Beim Konfigurieren der Containereinstellungen werden alle Verzeichnisse rekursiv unter dem `AZ_BATCH_NODE_ROOT_DIR` (Stamm der Azure Batch-Verzeichnisse auf dem Knoten) dem Container zugeordnet, alle Variablen der Aufgabenumgebung werden dem Container zugeordnet, und die Befehlszeile für die Aufgabe wird im Container ausgeführt.

Im Codebeispiel unter [Vorabrufen von Images für die Containerkonfiguration](#prefetch-images-for-container-configuration) wurde gezeigt, wie Sie eine Containerkonfiguration für eine Startaufgabe angeben. Im folgenden Codebeispiel wird gezeigt, wie Sie eine Containerkonfiguration für eine Cloudaufgabe angeben:

```csharp
// Simple task command

string cmdLine = "<my-command-line>";

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

* Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).

* Weitere Informationen zur Installation und Verwendung von Docker CE unter Linux finden Sie in der [Docker](https://docs.docker.com/engine/installation/)-Dokumentation.

* Weitere Informationen zur Verwendung benutzerdefinierter Images finden Sie unter [Verwenden eines verwalteten benutzerdefinierten Images zum Erstellen eines VM-Pools](batch-custom-images.md).
