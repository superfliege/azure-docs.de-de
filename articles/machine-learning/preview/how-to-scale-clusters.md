---
title: "Skalieren eines Azure Container Service-Clusters für Machine Learning | Microsoft-Dokumentation"
description: "Skalieren eines Azure Container Service-Clusters (ACS-Cluster): automatisches Skalieren und statisches Skalieren – Skalieren der Anzahl von Knoten im Cluster"
services: machine-learning
author: raymondl
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/04/2017
ms.openlocfilehash: 44aa167375355433851453010cebe5b49ef56ebd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="scaling-the-cluster-to-manage-web-service-throughput"></a>Skalieren des Clusters zum Verwalten des Durchsatzes von Webdiensten

## <a name="why-scale-the-cluster"></a>Gründe für das Skalieren des Clusters

Das Skalieren des ACS-Clusters ist eine effektive Methode zur Optimierung des Durchsatzes von Diensten auf der einen Seite, während auf der anderen Seite die Größe des Clusters minimal gehalten wird, um Kosten zu reduzieren. 

Das folgende Beispiel soll Ihnen dabei helfen, ein besseres Verständnis der automatischen Skalieren zu erlangen. Es handelt sich um einen Cluster, der drei Webdienste ausführt.

![Beispiel: drei Dienste auf einem Cluster](media/how-to-scale-clusters/three-services.png)

Die Dienste haben verschiedene Lastspitzen: Für Dienst 1 (blaue Linie) sind 40 Pods zu Lastspitzenzeiten erforderlich, für Dienst 2 (orange Linie) 38 Pods und für Dienst 3 (graue Linie) 50 Pods. Wenn Sie die benötigten Spitzenleistungen für jeden Dienst einzeln reservieren, benötigt dieser Cluster insgesamt mindestens 128 Pods (40+38+50=128).

Beachten Sie dabei die tatsächlich verwendete Anzahl von Pods zu einem beliebigen Zeitpunkt, die in der Grafik von der schwarz gestrichelten Linie dargestellt wird. In diesem Fall *werden höchstens 64 Pods zu einem beliebigen Zeitpunkt verwendet*. Dies geschieht um 20 Uhr, wenn die Lastspitze von Dienst 3 erreicht wird. Zu diesem Zeitpunkt verwendet Dienst 3 50 Pods, Dienst 2 9 Pods und Dienst 1 5 Pods. Beachten Sie, dass es sich dabei um die *Spitzenauslastung* für diesen Cluster handelt. Das bedeutet, dass der Cluster zu keinem Zeitpunkt mehr als 64 Pods verwendet – das sind nur halb so viele Pods wie die berechneten erforderlichen 128 Pods, die für die Spitzenauslastungszeiten der drei Dienste individuell skaliert wurden.

Sie können die Größe Ihres Clusters verringern, indem Sie die Pods im Cluster neu zuweisen – d.h. indem Sie ein erneutes Skalieren durchführen –, damit die Anzahl der Pods der aktuellen Nachfrage jedes Dienstes entspricht, anstatt ausreichende Ressourcen für die Lastspitzen aller Dienste zu verlangen. In diesem einfachen Beispiel wird die durch das automatische Skalieren erforderliche Anzahl von Pods von 128 auf 64 verringert – d.h., die erforderliche Clustergröße wird halbiert.

Das Skalieren der Anzahl von Pods kann relativ schnell, in weniger als einer Minute, abgeschlossen werden. Daher wird die Reaktionsfähigkeit nicht ernsthaft beeinträchtigt.

> [!NOTE]
> Durch das Skalieren eines Clusters können allerdings keine Probleme mit der Anforderungswartezeit gelöst werden. Im Hinblick auf die Operationalisierung sollte die Anzahl von erfolgreichen Ausführungen durch das zentrale Hochskalieren steigen und die Fehlermeldung „Dienst ist nicht verfügbar“ seltener angezeigt werden. 

## <a name="how-to-scale-web-services-on-your-acs-cluster"></a>Skalieren von Webdiensten auf Ihrem ACS-Cluster

Die Setupoption des Clusters für die CLI für die Modellverwaltung konfiguriert standardmäßig zwei Agents und einen Pod in Ihrer Umgebung. Außerdem installiert sie die Kubernetes-CLI.

Sie können die Webdienste, die Sie für ACS bereitgestellt haben, skalieren, indem Sie Folgendes anpassen:

* Die Anzahl der Agentknoten im Cluster
* Die Anzahl der auf den Agentknoten ausgeführten Kubernetes-Pod-Replikate

### <a name="scaling-the-number-of-nodes-in-the-cluster"></a>Skalieren der Anzahl der Knoten im Cluster

Mit dem folgenden Befehl können Sie die Anzahl der Agentknoten im Cluster festlegen:

```
az acs scale -g <resource group> -n <cluster name> --new-agent-count <new scale>
```

Dies nimmt einige Minuten in Anspruch. Weitere Informationen zum Skalieren der Anzahl der Knoten im Cluster finden Sie unter: [Skalieren von Agentknoten in einem Container Service-Cluster](https://docs.microsoft.com/en-us/azure/container-service/container-service-scale).

### <a name="scaling-the-number-of-kubernetes-pod-replicas-in-a-cluster"></a>Skalieren der Anzahl von Kubernetes-Pod-Replikaten in einem Cluster
 
Sie können die Anzahl der Kubernetes-Pod-Replikate mithilfe der Azure Machine Learning-CLI oder dem Kubernetes-Dashboard mit dem Cluster skalieren (https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

Weitere Informationen zu Kubernetes-Pod-Replikaten finden Sie in der [Kubernetes-Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/)-Dokumentation.

#### <a name="scaling-a-cluster-with-the-azure-machine-learning-cli"></a>Skalieren eines Clusters mit der Azure Machine Learning-CLI

Es gibt zwei Möglichkeiten, einen Cluster mithilfe der CLI zu skalieren:

- Autoscale
- Statisches Skalieren

Das automatische Skalieren ist standardmäßig aktiviert, wenn der Dienst erstellt wird, und in den meisten Fällen wird diese Skaliermethode bevorzugt.

##### <a name="autoscale"></a>Autoscale

Mit dem folgenden Befehl können Sie das automatische Skalieren aktivieren sowie die Mindest- und Höchstanzahl von Replikaten für den Dienst festlegen.

```
az ml service update realtime -i <service id> --autoscale-enabled true --autoscale-min-replicas <positive number> --autoscale-max-replicas <positive number>
```

Wenn Sie beispielsweise für die Einstellung `autoscale-min-replicas` fünf festlegen, werden fünf Replikate erstellt. Um die optimale Anzahl für die Webdienste zu finden, legen Sie einen Wert fest – z.B. 10 –, und beobachten Sie, wie häufig die Fehlermeldung 503 erscheint. Passen Sie anschließend die Zahl entsprechend an.


| Parametername | Typ | Beschreibung |
|--------------------|--------------------|--------------------|
| `autoscale-enabled` | Boolescher Wert | Gibt an, ob das automatische Skalieren aktiviert ist. Standardwert: TRUE |
| `autoscale-min-replicas` | integer | Gibt die Mindestanzahl von Pods an. Muss 0 sein oder höher liegen. Standardwert: 1 |
| `autoscale-max-replicas` | integer | Gibt die Höchstanzahl von Pods an. Muss 1 oder größer sein. Wenn die Anzahl von autoscale-max-replicas kleiner ist als die Anzahl von autoscale-min-replicas, werden die autoscale-max-replicas ignoriert. Standard: 10 |
| `autoscale-refresh-period-seconds` | integer | Gibt die Dauer in Sekunden an, bis sich das automatische Skalieren aktualisiert. Standardwert: 1 |
| `autoscale-target-utilization` | integer | Gibt die Zielauslastung des automatischen Skalierens in Prozent an (Wert zwischen 1 und 100). Standardwert: 70 |

Mithilfe des automatischen Skalierens können die folgenden beiden Bedingungen gewährleistet werden:

1. Die Zielauslastung wird erreicht
2. Das Skalieren übersteigt niemals die Mindest- und Höchsteinstellungen

Dienste in einem Cluster beanspruchen die gleichen Clusterressourcen. Je höher die Anzahl der Abfragen pro Sekunde ist, desto mehr Clusterressourcen beansprucht ein automatisch skalierter Dienst. Erst wenn die Anzahl der Abfragen pro Sekunde sinkt, werden langsam wieder Ressourcen freigegeben. Clusterressourcen werden nach Bedarf solange abgerufen, bis für den Dienst keine Ressourcen mehr zur Verfügung stehen.

Weitere Informationen zum Verwenden der Parameter für das automatische Skalieren finden Sie in der Dokumentation unter: [Referenz zur Befehlszeilenschnittstelle für die Modellverwaltung](model-management-cli-reference.md).

##### <a name="static-scale"></a>Statisches Skalieren

Grundsätzlich sollte das statische Skalieren vermieden werden, da durch diese Methode die Clustergröße nicht im selben Maße reduziert wird, wie durch das automatische Skalieren. Trotzdem ist es in einigen Fällen möglicherweise sinnvoll, das statische Skalieren anzuwenden. Wenn ein Cluster beispielsweise nur einem Dienst zugeordnet ist, wird durch das automatische Skalieren kein Vorteil erzielt, da alle Clusterressourcen diesem Dienst zugewiesen sein sollten.

Damit ein Cluster statisch skaliert werden kann, muss das automatische Skalieren deaktiviert sein. Deaktivieren Sie das automatische Skalieren mit dem folgenden Befehl:

```
az ml service update realtime -i <service id> --autoscale-enabled false
```

Wenn Sie das automatische Skalieren deaktiviert haben, können Sie über den folgenden Befehl die Anzahl der Replikate für einen Dienst direkt skalieren.

```
az ml service update realtime -i <service id> -z <replica count>
```
 
Weitere Informationen zum Skalieren der Anzahl der Knoten im Cluster finden Sie unter „Skalieren von Agentknoten in einem Container Service-Cluster“.

#### <a name="scaling-number-of-replicas-using-the-kubernetes-dashboard"></a>Skalieren der Anzahl von Replikaten mithilfe des Kubernetes-Dashboard

Geben Sie in der Befehlszeile folgenden Befehl ein:

```
kubectl proxy
```

Unter Windows wird der Installationspfad für Kubernetes nicht automatisch zu dem Pfad hinzugefügt. Navigieren Sie zunächst zum Installationsordner:

```
c:\users\<user name>\bin
```

Sobald Sie den Befehl ausführen, sollte Ihnen die folgende Meldung angezeigt werden:

```
Starting to serve on 127.0.0.1:8001
```

Wenn der Port bereits verwendet wird, sollte Ihnen eine Meldung ähnlich wie im folgenden Beispiel angezeigt werden:

```
F0612 21:49:22.459111   59621 proxy.go:137] listen tcp 127.0.0.1:8001: bind: address already in use
```

Sie können mithilfe des *--port*-Parameters eine alternative Portnummer festlegen.

```
kubectl proxy --port=8010
Starting to serve on 127.0.0.1:8010
```

Sobald Sie den Dashboard-Server gestartet haben, öffnen Sie den Browser und geben Sie die folgende URL ein:

```
127.0.0.1:<port number>/ui
```

Klicken Sie in der Hauptanzeige auf der linken Navigationsleiste des Dashboards auf **Bereitstellungen**. Wenn der Navigationsbereich nicht angezeigt wird, wählen Sie oben links das folgende Symbol aus: ![Menu consisting of three short horizontal lines](media/how-to-scale-clusters/icon-hamburger.png) (Aus drei kurzen, horizontalen Zeilen bestehendes Menü).

Suchen Sie die Bereitstellung, um erst das Symbol ![Menu icon consisting of three vertical dots](media/how-to-scale-clusters/icon-kebab.png) (Aus drei Punkten untereinander bestehendes Menüsymbol) auf der rechten Seite zu ändern und anzuklicken und anschließend **View/edit YAML** (YAML ansehen/bearbeiten) auszuwählen.

Suchen Sie in der Anzeige „Bereitstellung bearbeiten“ den *Spezifikationsknoten*, ändern Sie den Wert der *Replikate*, und klicken Sie auf **Aktualisieren**.
