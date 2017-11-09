---
title: "Tutorial für Kubernetes in Azure – Überwachen von Kubernetes"
description: "AKS-Tutorial – Überwachen von Kubernetes mit Microsoft Operations Management Suite (OMS)"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a41f699291a65129906680cbb6719c2478c0d830
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="monitor-azure-container-service-aks"></a>Überwachen von Azure Container Service (AKS)

Die Überwachung des Kubernetes-Clusters und der Kubernetes-Container ist vor allem dann entscheidend, wenn Sie einen umfangreichen Produktionscluster mit mehreren Anwendungen verwalten.

In diesem Tutorial konfigurieren Sie die Überwachung Ihres AKS-Clusters mithilfe der [Containerlösung für Log Analytics](../log-analytics/log-analytics-containers.md).

Dieses Tutorial – Teil 7 von 8 – behandelt folgende Aufgaben:

> [!div class="checklist"]
> * Konfigurieren der Containerüberwachungslösung
> * Konfigurieren der Überwachungs-Agents
> * Zugreifen auf Überwachungsinformationen im Azure-Portal

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurden eine Anwendung in Containerimages gepackt, diese Images in Azure Container Registry hochgeladen und ein Kubernetes-Cluster erstellt.

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./tutorial-kubernetes-prepare-app.md) zurück.

## <a name="configure-the-monitoring-solution"></a>Konfigurieren der Überwachungslösung

Klicken Sie im Azure-Portal auf **Neu**, und suchen Sie nach `Container Monitoring Solution`. Nachdem der gesuchte Begriff gefunden wurde, klicken Sie auf **Erstellen**.

![Hinzufügen der Lösung](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Erstellen Sie einen neuen OMS-Arbeitsbereich, oder wählen Sie einen vorhandenen aus. Das OMS-Arbeitsbereichsformular unterstützt Sie bei diesem Vorgang.

Aktivieren Sie beim Erstellen des Arbeitsbereichs das Kontrollkästchen **	An das Dashboard anheften**, damit Informationen leicht abrufbar sind.

![OMS-Arbeitsbereich](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Wählen Sie dann die Option **OK**. Klicken Sie nach Abschluss der Überprüfung auf **Erstellen**, um die Containerüberwachungslösung zu erstellen.

Nachdem der Arbeitsbereich erstellt wurde, wird dieser im Azure-Portal angezeigt.

## <a name="get-workspace-settings"></a>Abrufen von Arbeitsbereichseinstellungen

Zum Konfigurieren des Lösungs-Agents auf den Kubernetes-Knoten werden die ID und der Schlüssel des Log Analytics-Arbeitsbereichs benötigt.

Klicken Sie zum Abrufen dieser Werte auf den **OMS-Arbeitsbereich** im linken Menü der Containerlösung. Klicken Sie anschließend auf **Erweiterte Einstellungen**, und notieren Sie sich die **Arbeitsbereich-ID** und den **Primärschlüssel**.

## <a name="configure-monitoring-agents"></a>Konfigurieren der Überwachungs-Agents

Die folgende Kubernetes-Manifestdatei kann zum Konfigurieren der Containerüberwachungs-Agents in einem Kubernetes-Cluster verwendet werden. Mit der Datei wird ein Kubernetes-[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) erstellt, das einen einzelnen Pod auf jedem Clusterknoten ausführt.

Speichern Sie den folgenden Text in einer Datei mit dem Namen `oms-daemonset.yaml`, und ersetzen Sie die Platzhalterwerte für `WSID` und `KEY` durch die ID und den Schlüssel Ihres Log Analytics-Arbeitsbereichs.

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
    agentVersion: 1.4.0-12
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: <WSID>
       - name: KEY
         value: <KEY>
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
        - mountPath: /var/opt/microsoft/omsagent/state/containerhostname
          name: container-hostname
        - mountPath: /var/log
          name: host-log
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
    - name: container-hostname
      hostPath:
       path: /etc/hostname
    - name: host-log
      hostPath:
       path: /var/log
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

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Nachdem die Agents ausgeführt werden, dauert es einige Minuten, bis die Daten in der OMS erfasst und verarbeitet werden.

## <a name="access-monitoring-data"></a>Zugreifen auf Überwachungsdaten

Wählen Sie im Azure-Portal den Log Analytics-Arbeitsbereich aus, der an das Dashboard des Portals angeheftet wurde. Klicken Sie auf die Kachel **Containerüberwachungslösung**. Hier finden Sie Informationen zum AKS-Cluster und zu den Containern des Clusters.

![Dashboard](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Ausführliche Anweisungen zum Abfragen und Analysieren von Überwachungsdaten finden Sie in der [Dokumentation zu Azure Log Analytics](../log-analytics/index.yml).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihren Kubernetes-Cluster mit der OMS überwacht. Sie haben folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der Containerüberwachungslösung
> * Konfigurieren der Überwachungs-Agents
> * Zugreifen auf Überwachungsinformationen im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Aktualisierung von Kubernetes auf eine neue Version zu erfahren.

> [!div class="nextstepaction"]
> [Kubernetes aktualisieren](./tutorial-kubernetes-upgrade-cluster.md)