---
title: Tutorial für Kubernetes in Azure – Überwachen von Kubernetes
description: 'AKS-Tutorial: Überwachen von Kubernetes mit Azure Log Analytics'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 86ae0c5ab302c49fa58df887d9dffef6cec31708
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-monitor-azure-container-service-aks"></a>Tutorial: Überwachen von Azure Container Service (AKS)

Die Überwachung des Kubernetes-Clusters und der Kubernetes-Container ist vor allem dann entscheidend, wenn Sie einen umfangreichen Produktionscluster mit mehreren Anwendungen verwalten.

In diesem Tutorial konfigurieren Sie die Überwachung Ihres AKS-Clusters mithilfe der [Containerlösung für Log Analytics][log-analytics-containers].

Dieses Tutorial – Teil 7 von 8 – behandelt folgende Aufgaben:

> [!div class="checklist"]
> * Konfigurieren der Containerüberwachungslösung
> * Konfigurieren der Überwachungs-Agents
> * Zugreifen auf Überwachungsinformationen im Azure-Portal

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurden eine Anwendung in Containerimages gepackt, diese Images in Azure Container Registry hochgeladen und ein Kubernetes-Cluster erstellt.

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.

## <a name="configure-the-monitoring-solution"></a>Konfigurieren der Überwachungslösung

Klicken Sie im Azure-Portal auf **Ressource erstellen**, und suchen Sie nach `Container Monitoring Solution`. Nachdem der gesuchte Begriff gefunden wurde, klicken Sie auf **Erstellen**.

![Hinzufügen der Lösung](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Erstellen Sie einen neuen Log Analytics-Arbeitsbereich, oder wählen Sie einen bereits vorhandenen Arbeitsbereich aus. Das Log Analytics-Arbeitsbereichsformular unterstützt Sie bei diesem Vorgang.

Aktivieren Sie beim Erstellen des Arbeitsbereichs das Kontrollkästchen **An das Dashboard anheften**, damit Informationen leicht abrufbar sind.

![Log Analytics-Arbeitsbereich](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Wählen Sie dann die Option **OK**. Klicken Sie nach Abschluss der Überprüfung auf **Erstellen**, um die Containerüberwachungslösung zu erstellen.

Nachdem der Arbeitsbereich erstellt wurde, wird dieser im Azure-Portal angezeigt.

## <a name="get-workspace-settings"></a>Abrufen von Arbeitsbereichseinstellungen

Zum Konfigurieren des Lösungs-Agents auf den Kubernetes-Knoten werden die ID und der Schlüssel des Log Analytics-Arbeitsbereichs benötigt.

Klicken Sie zum Abrufen dieser Werte auf den **OMS-Arbeitsbereich** im linken Menü der Containerlösung. Klicken Sie anschließend auf **Erweiterte Einstellungen**, und notieren Sie sich die **Arbeitsbereich-ID** und den **Primärschlüssel**.

## <a name="create-kubernetes-secret"></a>Erstellen eines Kubernetes-Geheimnisses

Speichern Sie die Log Analytics-Arbeitsbereichseinstellungen mit dem Befehl [kubectl create secret][kubectl-create-secret] in einem Kubernetes-Geheimnis namens `omsagent-secret`. Aktualisieren Sie `WORKSPACE_ID` mit der ID Ihres Log Analytics-Arbeitsbereichs und `WORKSPACE_KEY` mit dem Arbeitsbereichsschlüssel.

```console
kubectl create secret generic omsagent-secret --from-literal=WSID=WORKSPACE_ID --from-literal=KEY=WORKSPACE_KEY
```

## <a name="configure-monitoring-agents"></a>Konfigurieren der Überwachungs-Agents

Die folgende Kubernetes-Manifestdatei kann zum Konfigurieren der Containerüberwachungs-Agents in einem Kubernetes-Cluster verwendet werden. Mit der Datei wird ein Kubernetes-[DaemonSet][kubernetes-daemonset] erstellt, das einen einzelnen Pod auf jedem Clusterknoten ausführt.

Speichern Sie den folgenden Text in einer Datei mit dem Namen `oms-daemonset.yaml`.

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

```azurecli
kubectl create -f oms-daemonset.yaml
```

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob das DaemonSet erstellt wurde:

```azurecli
kubectl get daemonset
```

Die Ausgabe sieht in etwa wie folgt aus:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Wenn die Agents ausgeführt werden, dauert es einige Minuten, bis die Daten von Log Analytics erfasst und verarbeitet wurden.

## <a name="access-monitoring-data"></a>Zugreifen auf Überwachungsdaten

Wählen Sie im Azure-Portal den Log Analytics-Arbeitsbereich aus, der an das Dashboard des Portals angeheftet wurde. Klicken Sie auf die Kachel **Containerüberwachungslösung**. Hier finden Sie Informationen zum AKS-Cluster und zu den Containern des Clusters.

![Dashboard](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Ausführliche Anweisungen zum Abfragen und Analysieren von Überwachungsdaten finden Sie in der [Dokumentation zu Azure Log Analytics][log-analytics-docs].

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihren Kubernetes-Cluster mit Log Analytics überwacht. Sie haben folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der Containerüberwachungslösung
> * Konfigurieren der Überwachungs-Agents
> * Zugreifen auf Überwachungsinformationen im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Aktualisierung von Kubernetes auf eine neue Version zu erfahren.

> [!div class="nextstepaction"]
> [Kubernetes aktualisieren][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubectl-create-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
