---
title: Azure Service Fabric-Ereignisanalyse mit OMS | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ereignisse unter Verwendung der OMS zur Überwachung und Diagnose von Azure Service Fabric-Clustern visualisieren und analysieren."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: f0cefab15a115719ea9c378546a7e6004bd06187
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Ereignisanalyse und Visualisierung mit OMS

Die Operations Management Suite (OMS) ist eine Sammlung von Verwaltungsdiensten, die die Überwachung und Diagnose für in der Cloud gehostete Anwendungen und Dienste unterstützen. Eine ausführlichere Übersicht über die OMS und ihre Optionen finden Sie unter [Was ist die OMS?](../operations-management-suite/operations-management-suite-overview.md).

## <a name="log-analytics-and-the-oms-workspace"></a>Log Analytics und der OMS-Arbeitsbereich

Log Analytics sammelt Daten von verwalteten Ressourcen, z.B. von einer Azure Storage-Tabelle oder einem Agent, und verwaltet sie in einem zentralen Repository. Die Daten können dann für Analyse, Warnungen und Visualisierung oder für den weiteren Export verwendet werden. Log Analytics unterstützt Ereignisse, Leistungsdaten oder andere benutzerdefinierten Daten.

Wenn die OMS konfiguriert ist, haben Sie Zugriff auf einen bestimmten *OMS-Arbeitsbereich*, über den Daten abgefragt oder in Dashboards visualisiert werden können.

Nachdem Daten von Log Analytics empfangen wurden, verfügt die OMS über mehrere *Verwaltungslösungen*. Dabei handelt es sich um vorkonfigurierte Lösungen zum Überwachen eingehender Daten, die an verschiedene Szenarien angepasst sind. Dazu gehören eine *Service Fabric-Analyselösung* und eine *Containerlösung*. Dies sind die beiden wichtigsten Lösungen für die Diagnose und Überwachung bei Verwendung von Service Fabric-Clustern. Es gibt aber auch verschiedene andere Lösungen, die in Betracht gezogen werden können. Außerdem ermöglicht die OMS die Erstellung benutzerdefinierter Lösungen. Entsprechende Informationen finden Sie [hier](../operations-management-suite/operations-management-suite-solutions.md). Jede Lösung, für deren Verwendung Sie sich für einen Cluster entscheiden, kann mit Log Analytics im gleichen OMS-Arbeitsbereich konfiguriert werden. Arbeitsbereiche ermöglichen benutzerdefinierte Dashboards und die Visualisierung von Daten sowie Änderungen an den Daten, die Sie sammeln, verarbeiten und analysieren möchten.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Einrichten eines OMS-Arbeitsbereichs mit der Service Fabric-Analyse-Lösung
Es wird empfohlen, dass Sie die Service Fabric-Lösung in Ihrem OMS-Arbeitsbereich einfügen. Sie umfasst ein Dashboard, über das die verschiedenen eingehenden Protokollkanäle von der Plattform- und Anwendungsebene angezeigt werden und die Service Fabric-spezifischen Protokolle abgefragt werden können. Es folgt ein Beispiel für eine relativ einfache Service Fabric-Lösung, mit der eine einzelne Anwendung im Cluster bereitgestellt wird:

![OMS – SF-Lösung](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Es gibt zwei Möglichkeiten zum Bereitstellen und Konfigurieren eines OMS-Arbeitsbereichs: entweder über eine Resource Manager-Vorlage oder direkt über den Azure Marketplace. Verwenden Sie die erste Option, wenn Sie einen Cluster bereitstellen, und die zweite, wenn Sie bereits einen Cluster mit aktivierter Diagnose bereitgestellt haben.

### <a name="deploying-oms-using-a-resource-management-template"></a>Bereitstellen der OMS über eine Resource Manager-Vorlage

Beim Bereitstellen eines Clusters mithilfe einer Resource Manager-Vorlage kann mit der Vorlage auch ein neuer OMS-Arbeitsbereich erstellt, diesem die Service Fabric-Lösung hinzugefügt und der Arbeitsbereich so konfiguriert werden, dass Daten aus den entsprechenden Speichertabellen gelesen werden.

>[!NOTE]
>Dazu muss Diagnose aktiviert werden, sodass die Azure Storage-Tabelle für die OMS und Log Analytics vorhanden sind und Daten daraus gelesen werden können.

[Hier](https://azure.microsoft.com/resources/templates/service-fabric-oms/) finden Sie eine Beispielvorlage, die Sie verwenden und je nach Anforderung ändern können und über die die oben genannten Vorgänge durchgeführt werden. Für den Fall, dass Sie mehr Optionen benötigen, gibt es einige weitere Vorlagen, die verschiedene Optionen enthalten, abhängig davon, wo genau im Prozess Sie einen OMS-Arbeitsbereich einrichten. Diese finden Sie unter [Service Fabric und OMS-Vorlagen](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Bereitstellen der OMS über den Azure Marketplace

Wenn Sie einen OMS-Arbeitsbereich nach dem Bereitstellen eines Clusters hinzufügen möchten, navigieren Sie zum Azure Marketplace, und suchen Sie *Service Fabric-Analyse*.

![OMS – SF-Analyse im Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

* Klicken Sie auf **Erstellen**.
* Klicken Sie im Fenster für die Erstellung der Service Fabric-Analyse für das Feld *OMS-Arbeitsbereich* auf **Arbeitsbereich auswählen** und dann auf **Einen neuen Arbeitsbereich erstellen**. Nehmen Sie die erforderlichen Eingaben vor. Einzige Anforderung hier ist, dass für den Service Fabric-Cluster und den OMS-Arbeitsbereich dasselbe Abonnement verwendet werden muss. Nachdem die Eingaben überprüft wurden, wird Ihr OMS-Arbeitsbereich bereitgestellt. Dieser Vorgang sollte nur wenige Minuten dauern.
* Wenn Sie fertig sind, klicken Sie erneut auf **Erstellen** am unteren Rand des Fensters zur Erstellung der Service Fabric-Analyse. Stellen Sie sicher, dass der neue Arbeitsbereich unter *OMS-Arbeitsbereich* angezeigt wird. Dadurch wird die Lösung dem Arbeitsbereich hinzugefügt, den Sie gerade erstellt haben.


Obwohl dadurch die Lösung dem Arbeitsbereich hinzugefügt wird, muss dieser Arbeitsbereich noch immer mit den Diagnosedaten verknüpft werden, die aus Ihrem Cluster stammen. Navigieren Sie zur Ressourcengruppe, in der Sie die Lösung der Service Fabric-Analyse erstellt haben. Daraufhin sollte ein *ServiceFabric(\<nameOfOMSWorkspace\>)* angezeigt werden.

* Klicken Sie auf die Lösung, um zur Übersichtsseite zu gelangen. Von dort aus können Sie die Lösungseinstellungen und Arbeitsbereicheinstellungen ändern und zum OMS-Portal navigieren.
* Klicken Sie im linken Navigationsmenü auf **Speicherkontoprotokolle** unter *Arbeitsbereichsdatenquellen*.

    ![Service Fabric-Analyse-Lösung im Portal](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-portal.png)

* Klicken Sie auf der Seite *Speicherkontoprotokolle* ganz oben auf **Hinzufügen**, um die Protokolle Ihres Clusters zum Arbeitsbereich hinzuzufügen.
* Klicken Sie auf **Speicherkonto**, um das geeignete in Ihrem Cluster erstellte Konto hinzuzufügen. Wenn Sie den Standardnamen verwendet haben, heißt das Speicherkonto *sfdg\<resourceGroupName\>*. Sie können dies ebenso bestätigen, indem Sie die Azure Resource Manager-Vorlage prüfen, die zum Bereitstellen Ihres Clusters verwendet wurde, indem Sie den für den `applicationDiagnosticsStorageAccountName` verwendeten Wert überprüfen. Möglicherweise müssen Sie auch nach unten scrollen und auf **Weitere laden** klicken, falls der Kontoname nicht angezeigt wird. Klicken Sie auf den richtigen Speicherkontonamen, wenn er angezeigt wird, um ihn auszuwählen.
* Als Nächstes müssen Sie den *Datentyp* angeben, der **Service Fabric-Ereignisse** sein sollte.
* Die *Quelle* sollte automatisch auf *WADServiceFabric\*EventTable* festgelegt werden.
* Klicken Sie auf **OK**, um Ihren Arbeitsbereich mit den Protokollen Ihres Clusters zu verbinden.

    ![Hinzufügen von Speicherkontoprotokollen zu OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

* Das Konto sollte jetzt als Teil Ihrer *Speicherkontoprotokolle* in den Datenquellen Ihres Arbeitsbereichs angezeigt werden.

Somit haben Sie nun die Service Fabric-Analyse-Lösung in einem OMS Log Analytics-Arbeitsbereich hinzugefügt, der jetzt vollständig mit der Plattform Ihres Clusters sowie der Anwendungsprotokolltabelle verbunden ist. Sie können dem Arbeitsbereich zusätzliche Quellen auf die gleiche Weise hinzufügen.

## <a name="using-the-oms-agent"></a>Verwenden des OMS-Agents

Es wird empfohlen, EventFlow und WAD als Aggregationslösungen zu verwenden, da diese einen modularen Ansatz für die Diagnose und Überwachung bieten. Wenn Sie beispielsweise die Ausgaben von EventFlow ändern möchten, ist keine Änderung an der eigentlichen Instrumentierung, sondern lediglich eine einfache Änderung an der Konfigurationsdatei erforderlich. Wenn Sie sich jedoch für die Verwendung der OMS entscheiden und sie auch für die Ereignisanalyse verwenden möchten (sie muss nicht die einzige Plattform sein, die Sie verwenden, sondern mindestens eine der Plattformen sein), wird empfohlen, dass Sie sich mit der Einrichtung des [OMS-Agents](../log-analytics/log-analytics-windows-agents.md) vertraut machen. Den OMS-Agent sollten Sie auch verwenden, wenn Sie Container für Ihren Cluster bereitstellen, wie weiter unten erläutert.

Die dazu erforderlichen Schritte sind relativ einfach, da Sie den Agent lediglich als VM-Skalierungsgruppenerweiterung der Resource Manager-Vorlage hinzufügen und sicherstellen müssen, dass er auf einem Ihrer Knoten installiert wird. Eine Resource Manager-Beispielvorlage, über die der OMS-Arbeitsbereich mit der Service Fabric-Lösung bereitgestellt (wie oben erläutert) und der Agent Ihren Knoten hinzugefügt wird, steht für Cluster unter [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) oder [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) zur Verfügung.

Dies bietet die folgenden Vorteile:

* Umfangreichere Daten für die Leistungsindikatoren und Metriken
* Leicht konfigurierbare Metriken, die aus dem Cluster gesammelt werden, ohne dass Sie Ihre Clusterkonfiguration aktualisieren müssen. Änderungen an den Einstellungen des Agents können aus dem OMS-Portal erfolgen, und der Agent startet in Übereinstimmung mit der erforderlichen Konfiguration automatisch neu. Zum Konfigurieren des OMS-Agents für das Abrufen spezifischer Leistungsindikatoren wechseln Sie zu **Startseite des Arbeitsbereichs > Einstellungen > Daten > Windows-Leistungsindikatoren**, und wählen Sie die zu sammelnden Daten aus.
* Daten werden schneller angezeigt, da sie nicht gespeichert werden müssen, um dann von OMS und Log Analytics abgerufen zu werden.
* Die Überwachung von Containern ist sehr viel einfacher, da Docker-Protokolle (stdout, stderr) und Docker-Statistiken (Leistungsmetriken auf Container- und Knotenebene) abgerufen werden können.

Die Hauptüberlegung hierbei ist, dass möglicherweise die Leistung Ihrer Anwendungen im Cluster beeinflusst werden kann, da der Agent gemeinsam mit Ihren Anwendungen auf Ihrem Cluster bereitgestellt wird.

## <a name="monitoring-containers"></a>Überwachen von Containern

Bei der Bereitstellung von Containern in einem Service Fabric-Cluster wird empfohlen, dass der Cluster mit dem OMS-Agent eingerichtet und Ihrem OMS-Arbeitsbereich die Container-Lösung hinzugefügt wird, um die Überwachung und Diagnose zu ermöglichen. Hier ein Beispiel für die Container-Lösung in einem Arbeitsbereich:

![Einfaches OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Der Agent ermöglicht das Sammeln verschiedener containerspezifischer Protokolle, die in der OMS abgefragt oder zur Visualisierung von Leistungsindikatoren verwendet werden können. Folgende Protokolltypen werden gesammelt:

* ContainerInventory: zeigt Informationen über Containerspeicherort, -name und -images an
* ContainerImageInventory: zeigt Informationen zu bereitgestellten Images an, einschließlich IDs und Größen
* ContainerLog: zeigt bestimmte Fehlerprotokolle, Docker-Protokolle (stdout usw.) und andere Einträge an
* ContainerServiceLog: zeigt die Docker-Daemon-Befehle an, die ausgeführt wurden
* Perf: zeigt Leistungsindikatoren einschließlich Container-CPU, Arbeitsspeicher, Netzwerkdatenverkehr, Datenträger-E/A und benutzerdefinierter Metriken von den Hostcomputern an

In diesem Artikel werden die erforderlichen Schritte zum Einrichten der Containerüberwachung für Ihren Cluster beschrieben. Weitere Informationen zur OMS-Containerlösung finden Sie in der [Dokumentation](../log-analytics/log-analytics-containers.md).

Stellen Sie zum Einrichten der Container-Lösung in Ihrem Arbeitsbereich sicher, dass der OMS-Agent auf den Knoten Ihres Clusters bereitgestellt ist. Führen Sie dazu die oben genannten Schritte aus. Nachdem der Cluster eingerichtet wurde, können Sie einen Container in ihm bereitstellen. Bedenken Sie, dass es bei der ersten Bereitstellung eines Containerimages in einem Cluster mehrere Minuten dauern kann, bis das Image heruntergeladen wurde. Dies hängt von der Größe des Images ab.

Suchen Sie in Azure Marketplace nach *Container Monitoring Solution* (Containerüberwachungslösung), und erstellen Sie das Ergebnis **Container Monitoring Solution**, das angezeigt werden sollte, unter der Kategorie „Überwachung + Verwaltung“.

![Hinzufügen der Containerlösung](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

Im Erstellungsschritt wird ein OMS-Arbeitsbereich angefordert. Wählen Sie den Arbeitsbereich aus, der mit der Bereitstellung oben erstellt wurde. Dieser Schritt fügt die Containerlösung Ihrem OMS-Arbeitsbereich hinzu. Sie wird dann automatisch durch den OMS-Agent erkannt, der von der Vorlage bereitgestellt wurde. Der Agent beginnt das Sammeln von Daten für Containerprozesse im Cluster, und nach weniger als 15 Minuten sollten in der Lösung Daten wie in der Abbildung des Dashboards oben angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte

Zum Anpassen eines Arbeitsbereichs entsprechend Ihren Anforderungen können Sie die folgenden Tools und Optionen der OMS verwenden:

* Für lokale Cluster bietet die OMS ein Gateway (HTTP-Weiterleitungsproxy), über das Daten an die OMS gesendet werden können. Weitere Informationen dazu finden Sie unter [Verbinden von Computern ohne Internetzugriff mit der OMS über das OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).
* Konfigurieren Sie die OMS für die Einrichtung von [automatisierten Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.