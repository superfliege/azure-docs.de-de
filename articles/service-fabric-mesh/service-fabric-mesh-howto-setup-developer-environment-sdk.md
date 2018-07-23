---
title: Einrichten einer Windows-Entwicklungsumgebung für Service Fabric Mesh-Apps | Microsoft Docs
description: Einrichten der Windows-Entwicklungsumgebung, damit Sie eine Service Fabric Mesh-Anwendung erstellen und in Azure Service Fabric Mesh bereitstellen können.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 96549696013a2dd94741090a0a017b57a3b1e19e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125160"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-applications"></a>Einrichten der Windows-Entwicklungsumgebung zum Erstellen von Service Fabric-Anwendungen

Um Azure Service Fabric-Apps auf Ihrem Windows-Entwicklungscomputer zu erstellen und auszuführen, installieren Sie die Service Fabric-Laufzeit, das SDK und die Tools.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen

Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Windows 10 (Enterprise, Professional oder Education)
* Windows Server 2016

## <a name="enable-hyper-v"></a>Aktivieren von Hyper-V

Hyper-V muss zum Erstellen von Service Fabric-Apps aktiviert sein. 

### <a name="windows-10"></a>Windows 10

Öffnen Sie PowerShell als Administrator, und führen Sie den folgenden Befehl aus:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Starten Sie den Computer neu. Weitere Informationen zum Aktivieren von Hyper-V finden Sie unter [Installieren von Hyper-V unter Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### <a name="windows-server-2016"></a>Windows Server 2016

Öffnen Sie PowerShell als Administrator, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Starten Sie den Computer neu. Weitere Informationen zum Aktivieren von Hyper-V finden Sie unter [Installieren der Hyper-V-Rolle unter Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server).

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 ist erforderlich, um Service Fabric-Apps bereitzustellen. [Installieren Sie Version 15.6.0][download-visual-studio] oder höher, und aktivieren Sie die folgenden Workloads:

- ASP.NET und Webentwicklung
- Azure-Entwicklung

## <a name="docker"></a>Docker

Installieren Sie Docker, um die von Service Fabric Mesh verwendeten containerisierten Service Fabric-Apps zu unterstützen.

### <a name="windows-10"></a>Windows 10

Laden Sie die neueste Version von [Docker Community Edition für Windows][download-docker] herunter, und installieren Sie diese. 

Wählen Sie während der Installation **Windows-Container anstelle von Linux-Containern verwenden** aus, wenn Sie dazu aufgefordert werden. Anschließend müssen Sie sich abmelden und dann erneut anmelden. Wenn Sie Hyper-V zuvor nicht aktiviert haben, werden Sie nach der erneuten Anmeldung möglicherweise aufgefordert, Hyper-V zu aktivieren. Sie müssen Hyper-V aktivieren und den Computer dann neu starten.

Nach dem Neustart des Computers fordert Docker Sie auf, das Feature **Container** zu aktivieren und den Computer neu zu starten.

### <a name="windows-server-2016"></a>Windows Server 2016

Verwenden Sie die folgenden PowerShell-Befehle, um Docker zu installieren. Weitere Informationen finden Sie in der [Docker Enterprise Edition für Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Starten Sie den Computer neu.

## <a name="sdk-and-tools"></a>SDKs und Tools

Installieren Sie die Service Fabric Mesh-Laufzeit, das SDK und die Tools in einer abhängigen Reihenfolge.

1. Installieren Sie das [Service Fabric Mesh SDK][download-sdkmesh] mithilfe des Webplattform-Installers. Dadurch werden auch das Microsoft Azure Service Fabric SDK und die Laufzeit installiert.
2. Installieren Sie die [Erweiterung Visual Studio Service Fabric-Tools (Vorschau)][download-tools] aus Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Erstellen eines Clusters

Für eine optimale Debugleistung beim Erstellen und Ausführen von Service Fabric-Apps wird empfohlen, einen lokalen Entwicklungscluster mit einem Knoten zu erstellen. Dieser Cluster muss immer dann ausgeführt werden, wenn Sie ein Service Fabric Mesh-Projekt bereitstellen oder debuggen.

Docker **muss** ausgeführt werden, bevor Sie einen Cluster erstellen können. Testen Sie, ob Docker ausgeführt wird, indem Sie ein Terminalfenster öffnen und `docker ps` ausführen, um zu sehen, ob ein Fehler auftritt. Wenn in der Antwort kein Fehler angegeben wird, wird Docker ausgeführt, und Sie sind bereit, einen Cluster zu erstellen.

Nachdem Sie die Laufzeit, SDKs und Visual Studio-Tools installiert haben, erstellen Sie einen Entwicklungscluster.

1. Schließen Sie das PowerShell-Fenster.
2. Öffnen Sie als Administrator ein neues PowerShell-Fenster mit erhöhten Rechten. Dieser Schritt ist erforderlich, um die Service Fabric-Module zu laden, die Sie installiert haben.
3. Führen Sie den folgenden PowerShell-Befehl aus, um einen Entwicklungscluster zu erstellen:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster -UseMachineName
    ```

4. Um das lokale Cluster-Manager-Tool zu starten, führen Sie den folgenden PowerShell-Befehl aus:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```

Jetzt können Sie Service Fabric Mesh-Anwendungen erstellen!

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie das Tutorial [Erstellen einer Azure Service Fabric-App](service-fabric-mesh-tutorial-create-dotnetcore.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/
