---
title: Einrichten einer Windows-Entwicklungsumgebung zum Erstellen von Service Fabric Mesh-Apps | Microsoft-Dokumentation
description: Einrichten der Windows-Entwicklungsumgebung, damit Sie eine Service Fabric Mesh-Anwendung erstellen und in Azure Service Fabric Mesh bereitstellen können.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 08/08/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 0531985cbab9c10b4df8ea3f27ac6c7903790da5
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978229"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Einrichten der Windows-Entwicklungsumgebung zum Erstellen von Service Fabric Mesh-Anwendungen

Zum Erstellen und Ausführen von Azure Service Fabric Mesh-Anwendungen auf dem Windows-Entwicklungscomputer müssen die Service Fabric Mesh-Laufzeit, das SDK und Tools installiert werden.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen

Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Windows 10 (Enterprise, Professional oder Education)
* Windows Server 2016

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 ist erforderlich, um Service Fabric Mesh-Anwendungen bereitzustellen. [Installieren Sie Version 15.6.0][download-visual-studio] oder höher, und aktivieren Sie die folgenden Workloads:

* ASP.NET und Webentwicklung
* Azure-Entwicklung

## <a name="install-docker"></a>Installieren von Docker

#### <a name="windows-10"></a>Windows 10

Laden Sie die aktuelle Version der [Docker Community Edition für Windows][download-docker] herunter, und installieren Sie sie, um die containerbasierten Service Fabric-Apps zu unterstützen, die von Service Fabric Mesh verwendet werden.

Wählen Sie während der Installation **Windows-Container anstelle von Linux-Containern verwenden** aus, falls Sie dazu aufgefordert werden.

Wenn Hyper-V auf Ihrem Computer nicht aktiviert ist, wird dieser Schritt vom Docker-Installationsprogramm angeboten. Klicken Sie auf **OK**, wenn die entsprechende Aufforderung angezeigt wird.

#### <a name="windows-server-2016"></a>Windows Server 2016

Ist die Hyper-V-Rolle nicht aktiviert, öffnen Sie PowerShell als Administrator, und führen Sie den folgenden Befehl aus, um Hyper-V zu aktivieren. Starten Sie anschließend Ihren Computer neu. Weitere Informationen finden Sie in der [Docker Enterprise Edition für Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Starten Sie den Computer neu.

Öffnen Sie PowerShell als Administrator, und führen Sie den folgenden Befehl aus, um Docker zu installieren:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDKs und Tools

Installieren Sie die Service Fabric Mesh-Laufzeit, das SDK und die Tools in der folgenden Reihenfolge.

1. Installieren Sie das [Service Fabric Mesh SDK][download-sdkmesh] mithilfe des Webplattform-Installers. Dadurch werden auch das Microsoft Azure Service Fabric SDK und die Laufzeit installiert.
2. Installieren Sie [Service Fabric Mesh Tools für Visual Studio (Vorschauversion)][download-tools] aus Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Erstellen eines Clusters

Bei Verwendung von Visual Studio können Sie diesen Abschnitt überspringen, da Visual Studio einen lokalen Cluster für Sie erstellt, falls dieser nicht vorhanden ist.

Für eine optimale Debugleistung beim Erstellen und Ausführen von Service Fabric-Apps wird empfohlen, einen lokalen Entwicklungscluster mit einem Knoten zu erstellen. Dieser Cluster muss immer dann ausgeführt werden, wenn Sie ein Service Fabric Mesh-Projekt bereitstellen oder debuggen.

Docker **muss** ausgeführt werden, bevor Sie einen Cluster erstellen können. Testen Sie, ob Docker ausgeführt wird, indem Sie ein Terminalfenster öffnen und `docker ps` ausführen, um zu sehen, ob ein Fehler auftritt. Wenn in der Antwort kein Fehler angegeben wird, wird Docker ausgeführt, und Sie sind bereit, einen Cluster zu erstellen.

Nachdem Sie die Laufzeit, SDKs und Visual Studio-Tools installiert haben, erstellen Sie einen Entwicklungscluster.

1. Schließen Sie das PowerShell-Fenster.
2. Öffnen Sie als Administrator ein neues PowerShell-Fenster mit erhöhten Rechten. Dieser Schritt ist erforderlich, um die zuletzt installierten Service Fabric-Module zu laden.
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

Antworten auf [häufig gestellte Fragen](service-fabric-mesh-faq.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/