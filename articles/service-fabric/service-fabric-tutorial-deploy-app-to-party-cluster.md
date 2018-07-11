---
title: Bereitstellen einer Service Fabric-App in einem Cluster in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie über Visual Studio eine Anwendung in einem Cluster bereitstellen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: f83ebcce68a7abe53d7b8eaeff5913a907e3df9a
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344188"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Tutorial: Bereitstellen einer Service Fabric-Anwendung in einem Cluster in Azure

Dieses Tutorial ist der zweite Teil einer Reihe und zeigt, wie Sie eine Azure Service Fabric-Anwendung direkt über Visual Studio in einem neuen Cluster in Azure bereitstellen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Clusters über Visual Studio
> * Bereitstellung einer Anwendung in einem Remotecluster mithilfe von Visual Studio

In dieser Tutorialreihe lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * Bereitstellen der Anwendung in einem Remotecluster
> * [Hinzufügen eines HTTPS-Endpunkts zu einem ASP.NET Core-Front-End-Dienst](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Konfigurieren von CI/CD mit Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
* [Installieren Sie das Service Fabric SDK](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Herunterladen der Beispielanwendung „Voting“

Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="create-a-service-fabric-cluster"></a>Erstellen von Service Fabric-Clustern

Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio in einem Cluster bereitstellen. Ein [Service Fabric-Cluster](/service-fabric/service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden.

Für die Bereitstellung in Visual Studio stehen zwei Optionen zur Verfügung:

* Erstellen eines Clusters in Azure über Visual Studio: Mit dieser Option können Sie direkt über Visual Studio einen sicheren Cluster mit Ihrer bevorzugten Konfiguration erstellen. Diese Art von Cluster ist ideal für Testszenarien, in denen Sie den Cluster erstellen und direkt in Visual Studio als Veröffentlichungsziel verwenden können.
* Verwenden Sie einen bereits vorhandenen Cluster aus Ihrem Abonnement als Veröffentlichungsziel.  Sie können Service Fabric-Cluster über das [Azure-Portal](https://portal.azure.com), über [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md), mithilfe von [Azure CLI](./scripts/cli-create-cluster.md)-Skripts oder mithilfe einer [Azure Resource Manager-Vorlage](service-fabric-tutorial-create-vnet-and-windows-cluster.md) bereitstellen.

In diesem Tutorial wird ein Cluster über Visual Studio erstellt. Falls Sie bereits einen Cluster bereitgestellt haben, können Sie Ihren Verbindungsendpunkt kopieren und einfügen oder ihn aus Ihrem Abonnement auswählen.
> [!NOTE]
> Viele Dienste verwenden den Reverseproxy, um miteinander zu kommunizieren. Bei Clustern, die über Visual Studio erstellt werden, sowie bei Partyclustern ist der Reverseproxy standardmäßig aktiviert.  Bei Verwendung eines bereits vorhandenen Clusters müssen Sie den [Reverseproxy im Cluster aktivieren](service-fabric-reverseproxy.md#setup-and-configuration).

### <a name="find-the-votingweb-service-endpoint"></a>Suchen des VotingWeb-Dienstendpunkts

Suchen Sie zuerst den Endpunkt des Front-End-Webdiensts.  Der Front-End-Webdienst lauscht an einem bestimmten Port.  Bei der Bereitstellung eines Clusters in Azure durch die Anwendung werden sowohl der Cluster als auch die Anwendung hinter einem Azure-Lastenausgleichsmodul ausgeführt.  Der Anwendungsport muss im Azure-Lastenausgleichsmodul geöffnet sein, damit eingehender Datenverkehr an den Webdienst gesendet werden kann.  Den Port (z.B. 8080) finden Sie in der Datei *VotingWeb/PackageRoot/ServiceManifest.xml* im **Endpoint**-Element:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Geben Sie im nächsten Schritt diesen Port im Dialogfenster **Cluster erstellen** auf der Registerkarte **Erweitert** ein.  Bei der Bereitstellung der Anwendung in einem vorhandenen Cluster können Sie diesen Port im Azure-Lastenausgleichsmodul mithilfe eines [PowerShell-Skripts](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) oder im [Azure-Portal](https://portal.azure.com) öffnen.

### <a name="create-a-cluster-in-azure-through-visual-studio"></a>Erstellen eines Clusters in Azure über Visual Studio

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Anwendungsprojekt, und klicken Sie anschließend auf **Veröffentlichen**.

Melden Sie sich unter Verwendung Ihres Azure-Kontos an, um auf Ihre Abonnements zugreifen zu können. Bei Verwendung eines Partyclusters ist dieser Schritt optional.

Wählen Sie in der Dropdownliste **Verbindungsendpunkt** die Option **<Create New Cluster...>** aus.

![Dialogfeld „Veröffentlichen“](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

Ändern Sie im Dialogfeld **Cluster erstellen** die folgenden Einstellungen:

1. Geben Sie im Feld **Clustername** den Namen Ihres Clusters sowie das gewünschte Abonnement und den gewünschten Standort an.
2. Optional: Ändern Sie ggf. die Knotenanzahl. Standardmäßig sind drei Knoten vorhanden (die Mindestanzahl zum Testen von Service Fabric-Szenarien).
3. Klicken Sie auf die Registerkarte **Zertifikat**. Geben Sie auf dieser Registerkarte ein Kennwort ein, um das Zertifikat Ihres Clusters zu schützen. Dieses Zertifikat trägt zum Schutz Ihres Clusters bei. Sie können auch den Pfad ändern, unter dem das Zertifikat gespeichert werden soll. Visual Studio kann das Zertifikat auch für Sie importieren. Dieser Schritt ist erforderlich, um die Anwendung im Cluster bereitstellen zu können.
4. Klicken Sie auf die Registerkarte **VM-Detail**. Geben Sie das Kennwort an, das Sie für die virtuellen Computer verwenden möchten, die den Cluster bilden. Die Kombination aus Benutzername und Kennwort kann zum Herstellen einer Remoteverbindung mit dem virtuellen Computer verwendet werden. Wählen Sie außerdem eine VM-Größe aus, und ändern Sie bei Bedarf das VM-Image.
5. Auf der Registerkarte **Erweitert** können Sie die Liste der Ports ändern, die in dem Azure-Lastenausgleichsmodul geöffnet sein sollen, das zusammen mit dem Cluster erstellt wird.  Fügen Sie den VotingWeb-Dienstendpunkt hinzu, den Sie in einem vorherigen Schritt ermittelt haben. Sie können auch einen vorhandenen Application Insights-Schlüssel hinzufügen, der als Routingziel für Anwendungsprotokolldateien verwendet werden soll.
6. Klicken Sie nach Abschluss der Einstellungsänderungen auf die Schaltfläche **Erstellen**. Die Erstellung kann einige Minuten dauern. Im Ausgabefenster wird eine entsprechende Information angezeigt, wenn der Cluster vollständig erstellt wurde.

![Dialogfeld „Cluster erstellen“](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

## <a name="deploy-the-sample-application"></a>Bereitstellen der Beispielanwendung

Wenn der gewünschte Cluster bereit ist, klicken Sie mit der rechten Maustaste auf das Anwendungsprojekt, und klicken Sie anschließend auf **Veröffentlichen**.

Nach Abschluss der Veröffentlichung können Sie über den Browser eine Anforderung an die Anwendung senden.

Öffnen Sie Ihren bevorzugten Browser, und geben Sie die Clusteradresse ein (den Verbindungsendpunkt ohne die Portinformationen – also beispielsweise „win1kw5649s.westus.cloudapp.azure.com“).

Ihnen sollte nun das gleiche Ergebnis angezeigt werden wie bei der lokalen Ausführung der Anwendung.

![API-Antwort aus dem Cluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Clusters über Visual Studio
> * Bereitstellung einer Anwendung in einem Remotecluster mithilfe von Visual Studio

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Aktivieren von HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
