---
title: Erstellen eines zuverlässigen Azure Service Fabric-Diensts mit C#
description: Hier erfahren Sie, wie Sie mit Visual Studio eine auf Azure Service Fabric basierende Reliable Services-Anwendung erstellen, bereitstellen und debuggen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/14/2018
ms.author: ryanwi
ms.openlocfilehash: 858e322fd7e516f756aa209be92745efa6cf75f7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Erstellen Ihrer ersten zustandsbehafteten Reliable Services-Anwendung mit C# und Service Fabric

Hier erfahren Sie, wie Sie in wenigen Minuten Ihre erste Azure Service Fabric-Anwendung für .NET unter Windows bereitstellen. Am Ende dieses Artikels verfügen Sie über einen lokalen Cluster mit einer Reliable Services-Anwendung.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass [Ihre Entwicklungsumgebung eingerichtet ist](service-fabric-get-started.md). Dazu gehört auch das Installieren des Service Fabric SDKs sowie von Visual Studio 2017 oder 2015.

## <a name="create-the-application"></a>Erstellen der Anwendung

1. Starten Sie Visual Studio als Administrator.

2. Drücken Sie STRG+UMSCHALT+N, um ein Projekt zu erstellen.

3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Cloud** > **Service Fabric-Anwendung**.

4. Nennen Sie die Anwendung **MyApplication**. Wählen Sie dann **OK**aus.

   ![Dialogfeld „Neues Projekt“ in Visual Studio][1]

5. Im nächsten Dialogfeld können Sie eine beliebige Art von Service Fabric-Anwendung erstellen. Wählen Sie für diese Schnellstartanleitung die Option **.Net Core 2.0** > **Zustandsbehafteter Dienst** aus.

6. Nennen Sie den Dienst **MyStatefulService**. Wählen Sie dann **OK**aus.

    ![Dialogfeld „Neuer Dienst“ in Visual Studio][2]

    Visual Studio erstellt das Anwendungsprojekt und das Projekt für den zustandsbehafteten Dienst. Anschließend werden beide im Projektmappen-Explorer angezeigt.

    ![Projektmappen-Explorer nach der Erstellung einer Anwendung mit zustandsbehaftetem Dienst][3]

    Das Anwendungsprojekt (**MyApplication**) enthält den keinerlei Code. Stattdessen verweist es eine Reihe von Dienstprojekten. Außerdem enthält es drei weitere Arten von Inhalt:

    * **Veröffentlichungsprofile**  
    Dies sind Profile für die Bereitstellung in anderen Umgebungen.

    * **Skripts**  
    PowerShell-Skripts zum Bereitstellen oder Upgraden Ihrer Anwendung.

    * **Anwendungsdefinition**  
Enthält die Datei „ApplicationManifest.xml“ unter *ApplicationPackageRoot*, in der die Zusammensetzung Ihrer Anwendung beschrieben ist. Die zugeordneten Anwendungsparameterdateien befinden sich unter *ApplicationParameters* und können zum Angeben von umgebungsspezifischen Parametern verwendet werden. Visual Studio wählt eine Anwendungsparameterdatei aus, die im zugeordneten Veröffentlichungsprofil angegeben ist.
    
Eine Übersicht über den Inhalt des Dienstprojekts finden Sie unter [Erste Schritte mit Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Bereitstellen und Debuggen der Anwendung

Nachdem Sie nun über eine Anwendung verfügen, können Sie sie wie folgt ausführen, bereitstellen und debuggen.

1. Drücken Sie in Visual Studio F5, um die Anwendung für das Debugging bereitzustellen.

    >[!NOTE]
    >Wenn Sie die Anwendung zum ersten Mal lokal ausführen und bereitstellen, erstellt Visual Studio einen lokalen Cluster für das Debuggen. Dies kann eine Weile dauern. Der Status der Clustererstellung wird im Ausgabefenster von Visual Studio angezeigt.

    Wenn der Cluster bereit ist, erhalten Sie eine Benachrichtigung von der Taskleistenanwendung Local Cluster Manager aus dem SDK.
    
    ![Benachrichtigung in der Taskleiste zum lokalen Cluster][4]

    Nach dem Start der Anwendung zeigt Visual Studio automatisch die Diagnoseereignisanzeige mit der Ausgabe der Ablaufverfolgung Ihrer Dienste an.
    
    ![Diagnoseereignisanzeige][5]

    >[!NOTE]
    >Ereignisse sollten in der Diagnoseereignisanzeige automatisch nachverfolgt werden. Falls Sie die Diagnoseereignisanzeige manuell konfigurieren müssen, öffnen Sie zunächst die Datei `ServiceEventSource.cs` aus dem Projekt **MyStatefulService**. Kopieren Sie am oberen Rand der Klasse `ServiceEventSource` den Wert des Attributs `EventSource`. Im folgenden Beispiel hat die Ereignisquelle den Namen `"MyCompany-MyApplication-MyStatefulService"`. In Ihrem Szenario lautet der Name möglicherweise anders.
>
    >![Ermitteln des Namens der Ereignisquelle][service-event-source-name]


2. Öffnen Sie als Nächstes das Dialogfeld **ETW-Anbieter**. Klicken Sie anschließend auf der Registerkarte **Diagnoseereignisse** auf das Zahnradsymbol. Fügen Sie den kopierten Namen der Ereignisquelle in das Eingabefeld **ETW-Anbieter** ein. Klicken Sie anschließend auf die Schaltfläche **Anwenden**. Die Nachverfolgung von Ereignissen wird automatisch gestartet.

    ![Festlegen des Namens der Diagnoseereignisquelle][setting-event-source-name]

    Im Fenster mit den Diagnoseereignissen sollten nun Ereignisse angezeigt werden.

    In der Vorlage für den zustandsbehafteten Dienst wird ein Zählerwert angezeigt, der in der `RunAsync`-Methode von **MyStatefulService.cs** inkrementiert wird.

3. Erweitern Sie eines der Ereignisse, um weitere Details anzuzeigen, einschließlich des Knotens, in dem der Code ausgeführt wird. In diesem Fall ist das **\_Node\_0**. Auf Ihrem Computer kann sich der Wert allerdings unterscheiden.
   
    ![Detail der Diagnoseereignisanzeige][6]

4. Der lokale Cluster enthält fünf Knoten, die auf einem einzelnen Computer gehostet werden. In einer Produktionsumgebung wird jeder Knoten auf einem eigenen physischen oder virtuellen Computer gehostet. Deaktivieren Sie einen der Knoten im lokalen Cluster, um den Ausfall eines Computers zu simulieren, während der Visual Studio-Debugger aktiv ist.

5. Öffnen Sie im **Projektmappen-Explorer**-Fenster die Datei **MyStatefulService.cs**. 

6. Suchen Sie nach der `RunAsync`-Methode, und legen Sie für die erste Zeile der Methode einen Haltepunkt fest.

    ![Haltepunkt in der RunAsync-Methode des zustandsbehafteten Diensts][7]

7. Starten Sie das Service Fabric Explorer-Tool, indem Sie mit der rechten Maustaste auf die Infobereich-Anwendung **Local Cluster Manager** (Manager für lokale Cluster) klicken und anschließend auf **Manage Local Cluster** (Lokalen Cluster verwalten) klicken.

    ![Starten von Service Fabric Explorer über den Manager für den lokalen Cluster][systray-launch-sfx]

    [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) bietet eine visuelle Darstellung eines Clusters. Sie enthält die darin bereitgestellten Anwendungen und die Gruppe der physischen Knoten des Clusters.

8. Erweitern Sie im linken Bereich **Cluster** > **Knoten**, und suchen Sie den Knoten, auf dem der Code ausgeführt wird. Klicken Sie als Nächstes auf **Aktionen** > **Deaktivieren (Neustarten)**, um einen Neustart des Computers zu simulieren.

    ![Beenden eines Knotens im Service Fabric Explorer][sfx-stop-node]

    Sofort sollten Sie den Haltepunkt in Visual Studio sehen, während die Berechnung, die Sie auf einem Knoten durchgeführt haben, nahtlos auf einem anderen fortgeführt wird.

9. Kehren Sie als Nächstes zur Diagnoseereignisanzeige zurück, und beobachten Sie die Meldungen. Der Zähler wurde weiter erhöht, obwohl die Ereignisse tatsächlich von einem anderen Knoten stammen.

    ![Diagnoseereignisanzeige nach einem Failover][diagnostic-events-viewer-detail-post-failover]

## <a name="clean-up-the-local-cluster-optional"></a>Bereinigen des lokalen Clusters (optional)

Bedenken Sie, dass dies ein echter lokaler Cluster ist. Wenn Sie den Debugger beenden, wird Ihre Anwendungsinstanz entfernt, und die Registrierung des Anwendungstyps wird aufgehoben. Der Cluster wird im Hintergrund aber weiter ausgeführt. Wenn Sie zum Beenden des lokalen Clusters bereit sind, können Sie zwischen zwei Optionen wählen.

### <a name="keep-application-and-trace-data"></a>Beibehalten der Anwendungs- und Ablaufverfolgungsdaten

Fahren Sie den Cluster herunter, indem Sie mit der rechten Maustaste auf die Infobereich-Anwendung **Local Cluster Manager** (Manager für lokalen Cluster) klicken und anschließend auf **Stop Local Cluster** (Lokalen Cluster beenden) klicken.

### <a name="delete-the-cluster-and-all-data"></a>Löschen des Clusters und aller Daten

Entfernen Sie den Cluster, indem Sie mit der rechten Maustaste auf die Infobereich-Anwendung **Local Cluster Manager** (Manager für lokalen Cluster) klicken. Klicken Sie dann auf **Remove Local Cluster** (Lokalen Cluster entfernen). 

Wenn Sie diese Option wählen, stellt Visual Studio den Cluster bei der nächsten Ausführung der Anwendung erneut bereit. Verwenden Sie diese Option, wenn Sie den lokalen Cluster voraussichtlich eine Weile nicht verwenden oder wenn Sie Ressourcen freigeben müssen.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich ausführlicher über [Reliable Services](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
