---
title: Tutorial für Azure Container Service – Überwachen von Kubernetes
description: 'Tutorial für Azure Container Service: Überwachen von Kubernetes mit Log Analytics'
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: e7d55f1579ce45a39f9b07225bc88c8ef8ff6b66
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="monitor-a-kubernetes-cluster-with-log-analytics"></a>Überwachen eines Kubernetes-Clusters mit Log Analytics

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Die Überwachung des Kubernetes-Clusters und der Kubernetes-Container ist wichtig, vor allem wenn Sie einen umfangreichen Produktionscluster mit mehreren Apps verwalten. 

Sie können verschiedene Kubernetes-Überwachungslösungen nutzen, entweder von Microsoft oder von anderen Anbietern. In diesem Tutorial überwachen Sie Ihren Kubernetes-Cluster mithilfe der Containerlösung in [Log Analytics](../../operations-management-suite/operations-management-suite-overview.md) (cloudbasierte IT-Verwaltungslösung von Microsoft). (Die Containerlösung befindet sich in der Vorschauphase.)

Dieses Tutorial – Teil 7 von 7 – behandelt folgende Aufgaben:

> [!div class="checklist"]
> * Abrufen der Einstellungen für den Log Analytics-Arbeitsbereich
> * Einrichten von OMS-Agents auf den Kubernetes-Knoten
> * Zugreifen auf Überwachungsinformationen im OMS-Portal oder Azure-Portal

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurden eine Anwendung in Containerimages gepackt, diese Images in Azure Container Registry hochgeladen und ein Kubernetes-Cluster erstellt. 

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. 

## <a name="get-workspace-settings"></a>Abrufen von Arbeitsbereichseinstellungen

Wenn Sie Zugriff auf das [OMS-Portal](https://mms.microsoft.com) haben, wechseln Sie zu **Einstellungen** > **Verbundene Quellen** > **Linux-Server**. Dort finden Sie die *Arbeitsbereichs-ID* und einen primären oder sekundären *Arbeitsbereichsschlüssel*. Notieren Sie sich diese Werte, Sie benötigen sie zum Einrichten von OMS-Agents im Cluster.

## <a name="create-kubernetes-secret"></a>Erstellen eines Kubernetes-Geheimnisses

Speichern Sie die Log Analytics-Arbeitsbereichseinstellungen mit dem Befehl [kubectl create secret][kubectl-create-secret] in einem Kubernetes-Geheimnis namens `omsagent-secret`. Aktualisieren Sie `WORKSPACE_ID` mit der ID Ihres Log Analytics-Arbeitsbereichs und `WORKSPACE_KEY` mit dem Arbeitsbereichsschlüssel.

```console
kubectl create secret generic omsagent-secret --from-literal=WSID=WORKSPACE_ID --from-literal=KEY=WORKSPACE_KEY
```

## <a name="set-up-oms-agents"></a>Einrichten von OMS-Agents

Hier sehen Sie ein Beispiel für eine YAML-Datei zum Einrichten von OMS-Agents in den Linux-Clusterknoten. Mit der Datei wird ein Kubernetes-[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) erstellt, das einen einzelnen identischen Pod auf jedem Clusterknoten ausführt. Die DaemonSet-Ressource eignet sich ideal für das Bereitstellen eines Überwachungs-Agents. 

Speichern Sie den folgenden Text in einer Datei mit dem Namen `oms-daemonset.yaml`, und ersetzen Sie die Platzhalterwerte für *myWorkspaceID* und *myWorkspaceKey* durch Ihre Log Analytics-Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel. (In der Produktion können Sie diese Werte als Geheimnisse codieren.)

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: 1.4.3-174
    dockerProviderVersion: 1.0.0-30
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        - mountPath: /var/lib/docker/containers 
          name: containerlog-path  
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   nodeSelector:
    beta.kubernetes.io/os: linux    
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"     
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log 
    - name: omsagent-secret
      secret:
       secretName: omsagent-secret
    - name: containerlog-path
      hostPath:
       path: /var/lib/docker/containers 
```

Erstellen Sie das DaemonSet mit dem folgenden Befehl:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob das DaemonSet erstellt wurde:

```azurecli-interactive
kubectl get daemonset
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Wenn die Agents ausgeführt werden, dauert es einige Minuten, bis die Daten von Log Analytics erfasst und verarbeitet wurden.

## <a name="access-monitoring-data"></a>Zugreifen auf Überwachungsdaten

Sie können die Containerüberwachungsdaten mit der [Containerlösung](../../log-analytics/log-analytics-containers.md) im OMS-Portal oder im Azure-Portal anzeigen und analysieren. 

Um die Containerlösung über das [OMS-Portal](https://mms.microsoft.com) zu installieren, wechseln Sie zu **Lösungskatalog**. Fügen Sie dann **Containerlösung** hinzu. Alternativ können Sie die Containerlösung über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview) hinzufügen.

Suchen Sie im OMS-Portal die Zusammenfassungskachel **Container** auf dem Dashboard. Klicken Sie auf die Kachel, um Details anzuzeigen, z.B. Containerereignisse, Fehler, Status, Imageinventar sowie CPU- und Arbeitsspeicherauslastung. Detaillierte Informationen erhalten Sie, wenn Sie auf eine Zeile auf einer Kachel klicken oder eine [Protokollsuche](../../log-analytics/log-analytics-log-searches.md) durchführen.

![Containerdashboard im OMS-Portal](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Im Azure-Portal navigieren Sie zu **Log Analytics** und wählen den Namen Ihres Arbeitsbereichs aus. Klicken Sie zum Anzeigen der Zusammenfassungskachel **Container** auf **Lösungen** > **Container**. Klicken Sie auf die Kachel, um Details anzuzeigen.

Ausführliche Anweisungen zum Abfragen und Analysieren von Überwachungsdaten finden Sie in der [Dokumentation zu Azure Log Analytics](../../log-analytics/index.yml).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihren Kubernetes-Cluster mit Log Analytics überwacht. Sie haben folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Abrufen der Einstellungen für den Log Analytics-Arbeitsbereich
> * Einrichten von OMS-Agents auf den Kubernetes-Knoten
> * Zugreifen auf Überwachungsinformationen im OMS-Portal oder Azure-Portal


Klicken Sie auf diesen Link, um vordefinierte Skriptbeispiele für Container Service anzuzeigen.

> [!div class="nextstepaction"]
> [Skriptbeispiele für Azure Container Service](cli-samples.md)
