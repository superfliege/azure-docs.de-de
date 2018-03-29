---
title: Bereitstellen einer Azure Service Fabric-Anwendung in einem Cluster über Visual Studio | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie über Visual Studio eine Anwendung in einem Cluster bereitstellen können.
services: service-fabric
documentationcenter: .net
-author: mikkelhegn
-manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 1d8f8d903046f1d471f7abbe08a957b81522e391
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-deploy-an-application-to-a-service-fabric-cluster-in-azure"></a>Tutorial: Bereitstellen einer Anwendung in einem Service Fabric-Cluster in Azure
Dieses Tutorial ist der zweite Teil einer Reihe und zeigt, wie Sie eine Azure Service Fabric-Anwendung direkt über Visual Studio in einem neuen Cluster in Azure bereitstellen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Clusters über Visual Studio
> * Bereitstellung einer Anwendung in einem Remotecluster mithilfe von Visual Studio


In dieser Tutorialreihe lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * Bereitstellen der Anwendung in einem Remotecluster
> * [Konfigurieren von CI/CD mit Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
- [Installieren Sie das Service Fabric SDK](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Herunterladen der Beispielanwendung „Voting“
Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="deploy-the-sample-application"></a>Bereitstellen der Beispielanwendung

### <a name="select-a-service-fabric-cluster-to-which-to-publish"></a>Auswählen eines Service Fabric-Clusters für die Veröffentlichung
Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio in einem Cluster bereitstellen.

Für die Bereitstellung stehen zwei Optionen zur Verfügung:
- Erstellen Sie einen Cluster über Visual Studio. Mit dieser Option können Sie direkt über Visual Studio einen sicheren Cluster mit Ihrer bevorzugten Konfiguration erstellen. Diese Art von Cluster ist ideal für Testszenarien, in denen Sie den Cluster erstellen und direkt in Visual Studio als Veröffentlichungsziel verwenden können.
- Verwenden Sie einen bereits vorhandenen Cluster aus Ihrem Abonnement als Veröffentlichungsziel.

In diesem Tutorial erfahren Sie, wie Sie einen Cluster über Visual Studio erstellen. Für die anderen Optionen können Sie Ihren Verbindungsendpunkt kopieren und einfügen oder ihn aus Ihrem Abonnement auswählen.
> [!NOTE]
> Viele Dienste verwenden den Reverseproxy, um miteinander zu kommunizieren. Bei Clustern, die über Visual Studio erstellt werden, sowie bei Partyclustern ist der Reverseproxy standardmäßig aktiviert.  Bei Verwendung eines bereits vorhandenen Clusters müssen Sie den [Reverseproxy im Cluster aktivieren](service-fabric-reverseproxy.md#setup-and-configuration).

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>Bereitstellen der App im Service Fabric-Cluster
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Anwendungsprojekt, und klicken Sie anschließend auf **Veröffentlichen**.

2. Melden Sie sich unter Verwendung Ihres Azure-Kontos an, um auf Ihre Abonnements zugreifen zu können. Bei Verwendung eines Partyclusters ist dieser Schritt optional.

3. Wählen Sie in der Dropdownliste **Verbindungsendpunkt** die Option „<Create New Cluster...>“ aus.
    
    ![Dialogfeld „Veröffentlichen“](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)
    
4. Ändern Sie im Dialogfeld „Cluster erstellen“ die folgenden Einstellungen:

    1. Geben Sie im Feld „Clustername“ den Namen Ihres Clusters sowie das gewünschte Abonnement und den gewünschten Standort an.
    2. Optional: Ändern Sie ggf. die Knotenanzahl. Standardmäßig sind drei Knoten vorhanden (die Mindestanzahl zum Testen von Service Fabric-Szenarien).
    3. Klicken Sie auf die Registerkarte „Zertifikat“. Geben Sie auf dieser Registerkarte ein Kennwort ein, um das Zertifikat Ihres Clusters zu schützen. Dieses Zertifikat trägt zum Schutz Ihres Clusters bei. Sie können auch den Pfad ändern, unter dem das Zertifikat gespeichert werden soll. Visual Studio kann das Zertifikat auch für Sie importieren. Dieser Schritt ist erforderlich, um die Anwendung im Cluster bereitstellen zu können.
    4. Klicken Sie auf die Registerkarte „VM-Detail“. Geben Sie das Kennwort an, das Sie für die virtuellen Computer verwenden möchten, die den Cluster bilden. Die Kombination aus Benutzername und Kennwort kann zum Herstellen einer Remoteverbindung mit dem virtuellen Computer verwendet werden. Wählen Sie außerdem eine VM-Größe aus, und ändern Sie bei Bedarf das VM-Image.
    5. Optional: Auf der Registerkarte „Erweitert“ können Sie die Liste der Ports ändern, die in dem Lastenausgleich geöffnet sein sollen, der zusammen mit dem Cluster erstellt wird. Sie können auch einen vorhandenen Application Insights-Schlüssel hinzufügen, der als Routingziel für Anwendungsprotokolldateien verwendet werden soll.
    6. Klicken Sie nach Abschluss der Einstellungsänderungen auf die Schaltfläche „Erstellen“. Die Erstellung kann einige Minuten dauern. Im Ausgabefenster wird eine entsprechende Information angezeigt, wenn der Cluster vollständig erstellt wurde.
    
    ![Dialogfeld „Cluster erstellen“](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

4. Wenn der gewünschte Cluster bereit ist, klicken Sie mit der rechten Maustaste auf das Anwendungsprojekt, und klicken Sie anschließend auf **Veröffentlichen**.

    Nach Abschluss der Veröffentlichung können Sie über den Browser eine Anforderung an die Anwendung senden.

5. Öffnen Sie Ihren bevorzugten Browser, und geben Sie die Clusteradresse ein (den Verbindungsendpunkt ohne die Portinformationen – also beispielsweise „win1kw5649s.westus.cloudapp.azure.com“).

    Ihnen sollte nun das gleiche Ergebnis angezeigt werden wie bei der lokalen Ausführung der Anwendung.

    ![API-Antwort aus dem Cluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Clusters über Visual Studio
> * Bereitstellung einer Anwendung in einem Remotecluster mithilfe von Visual Studio

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Einrichten der Continuous Integration in Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
