---
title: Überwachen von Azure-Kubernetes-Clustern – Operations Management
description: Überwachen eines Kubernetes-Clusters in Azure Container Service mithilfe von Log Analytics
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: efe4b3a1a63fa1986682a2fdde1a20221dc5d93a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="monitor-an-azure-container-service-cluster-with-log-analytics"></a>Überwachen eines Azure Container Service-Clusters mit Log Analytics

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Voraussetzungen
In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie mit [Azure Container Service einen Kubernetes-Cluster erstellt haben](container-service-kubernetes-walkthrough.md).

Außerdem wird angenommen, dass die Tools `az` (Azure-Befehlszeilenschnittstelle) und `kubectl` installiert sind.

Führen Sie Folgendes aus, um zu prüfen, ob das Tool `az` installiert ist:

```console
$ az --version
```

Wenn das Tool `az` nicht installiert ist, finden Sie [hier](https://github.com/azure/azure-cli#installation) Anweisungen.  
Alternativ können Sie [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) verwenden. In diesen Dienst sind die Azure-Befehlszeilenschnittstelle `az` und die `kubectl`-Tools bereits installiert.  

Führen Sie Folgendes aus, um zu prüfen, ob das Tool `kubectl` installiert ist:

```console
$ kubectl version
```

Wenn `kubectl` nicht installiert ist, können Sie folgenden Befehl ausführen:
```console
$ az acs kubernetes install-cli
```

Um zu testen, ob im kubectl-Tool Kubernetes-Schlüssel installiert sind, können Sie folgenden Befehl ausführen:
```console
$ kubectl get nodes
```

Wenn mit diesem Befehl Fehler auftreten, müssen Sie Kubernetes-Clusterschlüssel im kubectl-Tool installieren. Dies ist mit dem folgenden Befehl möglich:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Überwachen von Containern mit Log Analytics

Log Analytics ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer lokalen und cloudbasierten Infrastruktur erleichtert. Die Containerlösung ist eine Lösung in Log Analytics, mit der Sie das Inventar, die Leistung und die Protokolle von Containern an einer zentralen Stelle anzeigen können. Sie können Container überwachen und Problembehebungen durchführen, indem Sie die Protokolle an einer zentralen Stelle anzeigen und Container mit zu hoher Aktivität und zu hohem Ressourcenverbrauch auf einem Host suchen.

![](media/container-service-monitoring-oms/image1.png)

Weitere Informationen zur Containerlösung finden Sie unter [Log Analytics-Containerlösung](../../log-analytics/log-analytics-containers.md).

## <a name="installing-log-analytics-on-kubernetes"></a>Installieren von Log Analytics auf Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Abrufen von Arbeitsbereichs-ID und -Schlüssel
Damit der OMS-Agent mit dem Dienst kommunizieren kann, muss er mit einer Arbeitsbereichs-ID und einem Arbeitsbereichsschlüssel konfiguriert werden. Um die Arbeitsbereichs-ID und den Schlüssel zu erhalten, müssen Sie unter <https://mms.microsoft.com> ein Konto erstellen.
Führen Sie die entsprechenden Schritte aus, um ein Konto zu erstellen. Nachdem Sie das Konto erstellt haben, müssen Sie Ihre ID und Ihren Schlüssel abrufen. Klicken Sie hierzu wie unten gezeigt auf **Einstellungen** > **Verbundene Quellen** > **Linux-Server**.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a>Installieren des OMS-Agents mithilfe eines DaemonSets
DaemonSets werden von Kubernetes verwendet, um auf jedem Host im Cluster eine einzelne Instanz eines Containers auszuführen.
Sie eignen sich perfekt zum Ausführen von Überwachungs-Agents.

Im Anschluss sehen Sie die [DaemonSet-YAML-Datei](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Speichern Sie sie in einer Datei namens `oms-daemonset.yaml`, und ersetzen Sie die Platzhalterwerte für `WSID` und `KEY` durch Ihre Arbeitsbereichs-ID und den dazugehörigen Schlüssel.

Nachdem Sie der DaemonSet-Konfiguration Ihre Arbeitsbereichs-ID und den Schlüssel hinzugefügt haben, können Sie den OMS-Agent mithilfe des `kubectl`-Befehlszeilentools in Ihrem Cluster installieren:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a>Installieren des OMS-Agents mit einem Kubernetes-Geheimnis
Zum Schutz der Log Analytics-Arbeitsbereichs-ID und des Schlüssels können Sie ein Kubernetes-Geheimnis als Teil der DaemonSet-YAML-Datei verwenden.

 - Kopieren Sie das Skript, die Vorlagendatei für Geheimnisse und die DaemonSet-YAML-Datei (aus dem [Repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)), und vergewissern Sie sich, dass sich alle im selben Verzeichnis befinden. 
      - Skript zum Generieren von Geheimnissen – „secret-gen.sh“
      - Vorlage für Geheimnisse – „secret-template.yaml“
   - DaemonSet-YAML-Datei – „omsagent-ds-secrets.yaml“
 - Führen Sie das Skript aus. Im Skript erfolgt eine Aufforderung zur Eingabe der Log Analytics-Arbeitsbereichs-ID und des Primärschlüssels. Nehmen Sie diese Eingabe vor. Dann erstellt das Skript eine YAML-Datei für Geheimnisse, sodass Sie sie ausführen können.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Erstellen Sie den Pod für Geheimnisse durch Ausführen des folgenden Befehls: ``` kubectl create -f omsagentsecret.yaml ```
 
   - Führen Sie zur Überprüfung Folgendes aus: 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - Erstellen Sie Ihr OMS-Agent-DaemonSet durch Ausführen des folgenden Befehls: ``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Zusammenfassung
Das ist alles! Nach wenigen Minuten sollten Daten bei Ihrem OMS-Dashboard eingehen.
