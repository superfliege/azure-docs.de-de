---
title: Einrichten einer Windows-Entwicklungsumgebung für Azure-Microservices | Microsoft-Dokumentation
description: Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Anwendungen unter Windows beginnen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.openlocfilehash: 993d5c804490540a9c71647f52692fb55b27c405
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113298"
---
# <a name="prepare-your-development-environment-on-windows"></a>Vorbereiten Ihrer Entwicklungsumgebung unter Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

Zum Erstellen und Ausführen von [Azure Service Fabric-Anwendungen][1] müssen auf dem Windows-Entwicklungscomputer die Service Fabric-Laufzeit, das SDK und Tools installiert werden. Außerdem müssen Sie die [Ausführung der im SDK enthaltenen Windows PowerShell-Skripts aktivieren](#enable-powershell-script-execution).

## <a name="prerequisites"></a>Voraussetzungen
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7-Unterstützung:
> - Windows 7 enthält standardmäßig nur Windows PowerShell 2.0. Für Service Fabric PowerShell-Cmdlets ist PowerShell 3.0 oder höher erforderlich. Sie können [Windows PowerShell 5.0][powershell5-download] aus dem Microsoft Download Center herunterladen.
> - Der Service Fabric-Reverseproxy ist unter Windows 7 nicht verfügbar.
>

## <a name="install-the-sdk-and-tools"></a>Installieren des SDKs und der Tools
### <a name="to-use-visual-studio-2017"></a>So verwenden Sie Visual Studio 2017
Die Service Fabric-Tools sind Bestandteil der Workload für die Azure-Entwicklung in Visual Studio 2017. Aktivieren Sie diese Workload im Rahmen der Visual Studio-Installation.
Zudem müssen Sie das Microsoft Azure Service Fabric SDK und die Laufzeit mithilfe des Webplattform-Installers installieren.

* [Installieren des Microsoft Azure Service Fabric-SDKs][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>So verwenden Sie Visual Studio 2015 (erfordert mindestens Visual Studio 2015 Update 2)
Für Visual Studio 2015 werden die Service Fabric-Tools zusammen mit dem SDK und der Laufzeit mithilfe des Webplattform-Installers installiert:

* [Installieren des Microsoft Azure Service Fabric-SDKs und der Tools][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Nur SDK-Installation
Wenn Sie nur das SDK benötigen, können Sie dieses Paket installieren:
* [Installieren des Microsoft Azure Service Fabric-SDKs][core-sdk]

Aktuelle Versionen:
* Service Fabric SDK und -Tools 3.1.301
* Service Fabric-Runtime 6.2.301
* Service Fabric-Tools für Visual Studio 2015 2.1.20180510.2
* Visual Studio 2017 15.7 enthält Service Fabric-Tools für Visual Studio 2.1.20180423.1 

Eine Liste mit unterstützten Versionen finden Sie unter [Service Fabric-Unterstützung](service-fabric-support.md).

## <a name="enable-powershell-script-execution"></a>Aktivieren der PowerShell-Skriptausführung
Service Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines lokalen Entwicklungsclusters und zum Bereitstellen von Anwendungen aus Visual Studio. Die Ausführung dieser Skripts wird von Windows standardmäßig blockiert. Um die Skripts zu aktivieren, müssen Sie die PowerShell-Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie folgenden Befehl ein:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Entwicklungsumgebung eingerichtet haben, können Sie nun mit dem Erstellen und Ausführen von Apps beginnen.

* [Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Weitere Informationen zum Bereitstellen und Verwalten von Anwendungen in Ihrem lokalen Cluster](service-fabric-get-started-with-a-local-cluster.md)
* [Vorbereiten einer Linux-Entwicklungsumgebung unter Windows](service-fabric-local-linux-cluster-windows.md)
* [Weitere Informationen zu Programmiermodellen: Reliable Services und Reliable Actors](service-fabric-choose-framework.md)
* [Regelmäßiges Sichern und Wiederherstellen in Azure Service Fabric](service-fabric-backuprestoreservice-quickstart-azurecluster.md)
* [Service Fabric-Codebeispiele auf GitHub](https://aka.ms/servicefabricsamples)
* [Visualisieren des Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Folgen Sie dem Service Fabric-Lernpfad, um eine umfassende Einführung in die Plattform zu erhalten.](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).
* [Automatisieren von Betriebssystempatches im Cluster](service-fabric-patch-orchestration-application.md)

[1]: http://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric-Kampagnenseite"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "WebPI-Link für VS 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "WebPI-Link für Dev15"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "WebPI-Link für Core SDK"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
