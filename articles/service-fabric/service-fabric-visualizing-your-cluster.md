---
title: Visualisieren des Clusters mit Azure Service Fabric Explorer | Microsoft-Dokumentation
description: Service Fabric Explorer ist eine Anwendung zum Untersuchen und Verwalten von Cloudanwendungen und Knoten in einem Microsoft Azure Service Fabric-Cluster.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2018
ms.author: mikhegn
ms.openlocfilehash: dc71bf8045abf72953f678dc9295346dc9204e59
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisieren Ihres Clusters mit Service Fabric Explorer

Bei Service Fabric Explorer (SFX) handelt es sich um ein Open Source-Tool zum Untersuchen und Verwalten von Azure Service Fabric-Clustern. Service Fabric Explorer ist eine Desktopanwendung für Windows und Linux. Die Unterstützung für macOS ist bereits in Arbeit.

## <a name="service-fabric-explorer-download"></a>Herunterladen von Service Fabric Explorer

Service Fabric Explorer kann über die folgenden Links als Desktopanwendung heruntergeladen werden:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

- macOS
  - https://aka.ms/sfx-macos

> [!NOTE]
> Die Desktopversion von Service Fabric Explorer kann über mehr oder weniger Features verfügen als der Cluster unterstützt. Zur Gewährleistung einer uneingeschränkten Featurekompatibilität können Sie auf die für den Cluster bereitgestellte Service Fabric Explorer-Version zurückgreifen.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Ausführen von Service Fabric Explorer über den Cluster

Service Fabric Explorer wird auch an einem HTTP-Verwaltungsendpunkt des Service Fabric-Clusters gehostet. Wenn Sie SFX in einem Webbrowser starten möchten, navigieren Sie in einem beliebigen Browser zum HTTP-Verwaltungsendpunkt des Clusters (beispielsweise https://clusterFQDN:19080).

Zur Einrichtung einer Entwicklerarbeitsstation können Sie Service Fabric Explorer in Ihrem lokalen Cluster starten, indem Sie zu https://localhost:19080/Explorer navigieren. Informationen zum Vorbereiten Ihrer Entwicklungsumgebung finden Sie in [diesem Artikel](service-fabric-get-started.md).

## <a name="connect-to-a-service-fabric-cluster"></a>Herstellen einer Verbindung mit einem Service Fabric-Cluster
Für die Verbindungsherstellung mit einem Service Fabric-Cluster benötigen Sie den Verwaltungsendpunkt (FQDN/IP) und den Port des HTTP-Verwaltungsendpunkts für den Cluster (standardmäßig 19080). Beispiel: https://mysfcluster.westus.cloudapp.azure.com:19080. Aktivieren Sie das Kontrollkästchen „Mit Localhost verbinden“, falls Sie eine Verbindung mit einem lokalen Cluster auf Ihrer Arbeitsstation herstellen möchten.

### <a name="connect-to-a-secure-cluster"></a>Herstellen einer Verbindung mit einem sicheren Cluster
Sie können den Clientzugriff auf Ihren Service Fabric-Cluster entweder mit Zertifikaten oder über Azure Active Directory (AAD) steuern.

Wenn Sie versuchen, eine Verbindung mit einem sicheren Cluster herzustellen, müssen Sie je nach Konfiguration des Clusters entweder ein Clientzertifikat bereitstellen oder sich mithilfe von AAD anmelden.

## <a name="video-tutorial"></a>Videotutorial

Im folgenden Microsoft Virtual Academy-Video erfahren Sie, wie Sie den Service Fabric Explorer verwenden:

> [!NOTE]
> Dieses Video zeigt nicht die Desktopversion von Service Fabric Explorer, sondern die in einem Service Fabric-Cluster gehostete Version.
>
>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="understand-the-service-fabric-explorer-layout"></a>Grundlegendes zum Layout von Service Fabric Explorer
Sie können in Service Fabric Explorer navigieren, indem Sie die Strukturansicht auf der linken Seite verwenden. Auf der Stammebene der Struktur bietet das Clusterdashboard eine Clusterübersicht, einschließlich einer Zusammenfassung der Anwendungs- und Knotenintegrität.

![Service Fabric Explorer-Clusterdashboard][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Anzeigen des Cluster-Layouts
Knoten in einem Service Fabric-Cluster werden in einem zweidimensionalen Raster aus Fehlerdomänen und Upgradedomänen angeordnet. Durch diese Anordnung wird dafür gesorgt, dass Ihre Anwendungen auch bei Hardwarefehlern und Anwendungsupgrades verfügbar bleiben. Mit der Clusterzuweisung können Sie anzeigen, wie der aktuelle Cluster angeordnet ist.

![Service Fabric Explorer-Clusterzuweisung][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Anzeigen von Anwendungen und Diensten
Der Cluster enthält zwei Unterstrukturen: eine für Anwendungen und eine für Knoten.

Sie können die Anwendungsansicht für die Navigation durch die logische Hierarchie von Service Fabric verwenden: Anwendungen, Dienste, Partitionen und Replikate.

Im Beispiel unten besteht die Anwendung **MyApp** aus zwei Diensten: **MyStatefulService** und **WebService**. Da **MyStatefulService** zustandsbehaftet ist, enthält er eine Partition mit einem primären und zwei sekundären Replikaten. Im Gegensatz dazu ist „WebSvcService“ zustandslos und enthält eine einzelne Instanz.

![Service Fabric Explorer-Anwendungsansicht][sfx-application-tree]

Auf jeder Ebene der Struktur werden im Hauptbereich relevante Informationen zum Element angezeigt. Beispielsweise werden der Integritätsstatus und die Version für einen bestimmten Dienst angezeigt.

![Service Fabric Explorer-Bereich für essentielle Informationen][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Anzeigen der Knoten des Clusters
Die Knotenansicht zeigt das physische Layout des Clusters. Für einen Knoten können Sie überprüfen, für welche Anwendungen Code auf dem Knoten bereitgestellt wurde. Genauer gesagt können Sie anzeigen, welche Replikate derzeit darauf ausgeführt werden.

## <a name="actions"></a>Actions
Service Fabric Explorer bietet eine schnelle Möglichkeit zum Aufrufen von Aktionen für die Knoten, Anwendungen und Dienste in Ihrem Cluster.

Um beispielsweise eine Anwendungsinstanz zu löschen, wählen Sie die Anwendung in der Struktur auf der linken Seite und dann **Aktionen** > **Anwendung löschen** aus.

![Löschen einer Anwendung in Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> Sie können die gleichen Aktionen ausführen, indem Sie auf die Auslassungspunkte neben jedem Element klicken.
>
> Jede Aktion, die mit Service Fabric Explorer ausgeführt werden kann, kann auch mithilfe von PowerShell oder einer REST-API ausgeführt werden, um die Automatisierung zu ermöglichen.
>
>

Sie können Service Fabric Explorer auch verwenden, um Anwendungsinstanzen für einen bestimmten Anwendungstyp und eine bestimmte Anwendungsversion zu erstellen. Wählen Sie in der Strukturansicht den gewünschten Anwendungstyp aus, und klicken Sie dann neben der im rechten Bereich angezeigten gewünschten Version auf den Link **App-Instanz erstellen** .

![Erstellen einer Anwendung in Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Beim Erstellen von Anwendungsinstanzen unterstützt Service Fabric Explorer keine Parameter. Für Anwendungsinstanzen werden Standardparameterwerte verwendet.
>
>

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric-Anwendungsbereitstellung per PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png