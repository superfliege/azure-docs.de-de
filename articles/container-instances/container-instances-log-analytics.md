---
title: Protokollierung für Containerinstanzen mit Azure Log Analytics
description: Hier erfahren Sie, wie Sie Containerausgaben (STDOUT und STDERR) an Azure Log Analytics senden.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 07/17/2018
ms.author: danlep
ms.openlocfilehash: 4dbcccc1a4b23ca37918495dc536df08a70cade7
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337885"
---
# <a name="container-instance-logging-with-azure-log-analytics"></a>Protokollierung für Containerinstanzen mit Azure Log Analytics

Log Analytics-Arbeitsbereiche bieten einen zentralen Ort für das Speichern und Abfragen von Protokolldaten – und zwar nicht nur von Azure-Ressourcen, sondern auch von lokalen Ressourcen und Ressourcen in anderen Clouds. In Azure Container Instances ist die Unterstützung für das Senden von Daten an Log Analytics bereits integriert.

Um Containerinstanzendaten an Log Analytics senden zu können, müssen Sie mithilfe der Azure CLI (oder Cloud Shell) und einer YAML-Datei eine Containergruppe erstellen. In den folgenden Abschnitten wird beschrieben, wie Sie eine Containergruppe mit aktivierter Protokollierung erstellen und Protokolle abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Aktivieren der Protokollierung in Ihren Containerinstanzen benötigen Sie Folgendes:

* [Log Analytics-Arbeitsbereich](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (oder [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Abrufen der Log Analytics-Anmeldeinformationen

Azure Container Instances benötigt die Berechtigung zum Senden von Daten an Ihren Log Analytics-Arbeitsbereich. Wenn Sie diese Berechtigung erteilen und die Protokollierung aktivieren möchten, müssen Sie beim Erstellen der Containergruppe die Log Analytics-Arbeitsbereichs-ID und einen der Schlüssel (Primär- oder Sekundärschlüssel) angeben.

So erhalten Sie die Log Analytics-Arbeitsbereichs-ID und den Primärschlüssel:

1. Navigieren Sie im Azure-Portal zu Ihrem Log Analytics-Arbeitsbereich.
1. Wählen Sie unter **EINSTELLUNGEN** die Option **Erweiterte Einstellungen** aus.
1. Klicken Sie auf **Verbundene Quellen** > **Windows-Server** (oder **Linux-Server**, ID und Schlüssel sind für beide identisch).
1. Notieren Sie sich Folgendes:
   * **ARBEITSBEREICHS-ID**
   * **PRIMÄRSCHLÜSSEL**

## <a name="create-container-group"></a>Erstellen einer Containergruppe

Mit der Log Analytics-Arbeitsbereichs-ID und dem Primärschlüssel können Sie nun eine Containergruppe mit aktivierter Protokollierung erstellen.

In den folgenden Beispielen werden zwei Möglichkeiten zum Erstellen einer Containergruppe mit einem einzelnen [fluentd][fluentd]-Container gezeigt: Azure CLI und Azure CLI mit einer YAML-Vorlage. Der Fluentd-Container generiert mehrere Ausgabezeilen in der Standardkonfiguration. Da diese Ausgabe an Ihren Log Analytics-Arbeitsbereich gesendet wird, eignet sie sich gut zur Veranschaulichung der Anzeige und Abfrage von Protokollen.

### <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

Für eine Bereitstellung über die Azure-Befehlszeilenschnittstelle geben Sie die Parameter `--log-analytics-workspace` und `--log-analytics-workspace-key` im Befehl [az container create][az-container-create] an. Ersetzen Sie die beiden Arbeitsbereichswerte durch die Werten, die Sie im vorherigen Schritt erhalten haben (und aktualisieren Sie den Ressourcengruppennamen), bevor Sie den folgenden Befehl ausführen.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainergroup001 \
    --image fluent/fluentd \
    --log-analytics-workspace <WORKSPACE_ID> \
    --log-analytics-workspace-key <WORKSPACE_KEY>
```

### <a name="deploy-with-yaml"></a>Bereitstellen mit YAML

Verwenden Sie diese Methode, wenn Sie Containergruppen lieber mit YAML bereitstellen möchten. Der folgende YAML-Code definiert eine Containergruppe mit einem einzelnen Container. Kopieren Sie den YAML-Code in eine neue Datei, und ersetzen Sie `LOG_ANALYTICS_WORKSPACE_ID` und `LOG_ANALYTICS_WORKSPACE_KEY` durch die im vorherigen Schritt erhaltenen Werte. Speichern Sie die Datei unter dem Namen **deploy-aci.yaml**.

```yaml
apiVersion: 2018-06-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Führen Sie als Nächstes den folgenden Befehl aus, um die Containergruppe bereitzustellen. Ersetzen Sie `myResourceGroup` durch eine Ressourcengruppe in Ihrem Abonnement (oder erstellen Sie zuerst eine Ressourcengruppe namens „myResourceGroup“):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Sie sollten kurz nach Ausgabe des Befehls eine Antwort von Azure mit den Bereitstellungsdetails erhalten.

## <a name="view-logs-in-log-analytics"></a>Anzeigen von Protokollen in Log Analytics

Nach der Bereitstellung der Containergruppe kann es bis zu 10 Minuten dauern, bis die ersten Protokolleinträge im Azure-Portal angezeigt werden. Öffnen Sie zum Anzeigen der Protokolle der Containergruppe Ihren Log Analytics-Arbeitsbereich, und führen Sie die folgenden Schritte aus:

1. Wählen Sie in der Übersicht **OMS-Arbeitsbereich** die Option **Protokollsuche** aus. OMS-Arbeitsbereiche werden jetzt als Log Analytics-Arbeitsbereiche bezeichnet.  
1. Klicken Sie unter **Weitere Abfragen zum Ausprobieren** auf den Link **Alle gesammelten Daten**.

Von der Abfrage `search *` sollten mehrere Ergebnisse angezeigt werden. Falls anfangs keine Ergebnisse angezeigt werden, warten Sie einige Minuten, und klicken Sie dann auf die Schaltfläche **AUSFÜHREN**, um die Abfrage erneut auszuführen. Standardmäßig werden Protokolleinträge als Listenansicht angezeigt. Wählen Sie **Tabelle**, um die Protokolleinträge in einem komprimierten Format anzuzeigen. Sie können dann eine Zeile erweitern, um den Inhalt eines einzelnen Protokolleintrags anzuzeigen.

![Protokollsuchergebnisse im Azure-Portal][log-search-01]

## <a name="query-container-logs"></a>Abfragen von Containerprotokollen

Log Analytics beinhaltet eine umfassende [Abfragesprache][query_lang], um Informationen aus Tausenden von Protokollausgabezeilen abzurufen.

Der Azure Container Instances-Protokollierungs-Agent sendet Einträge an die Tabelle `ContainerInstanceLog_CL` in Ihrem Log Analytics-Arbeitsbereich. Die Grundstruktur einer Abfrage ist die Quelltabelle (`ContainerInstanceLog_CL`), gefolgt von einer Reihe von Operatoren, die durch einen senkrechten Strich (`|`) getrennt sind. Sie können mehrere Operatoren verketten, um die Ergebnisse einzugrenzen und erweiterte Funktionen auszuführen.

Fügen Sie zum Anzeigen von Beispielabfrageergebnissen die folgende Abfrage in das Abfragetextfeld (unter „Legacysprachkonverter anzeigen“) ein, und klicken Sie auf die Schaltfläche **AUSFÜHREN**, um die Abfrage auszuführen. Diese Abfrage zeigt alle Protokolleinträge an, deren Feld „Message“ das Wort „warn“ enthält:

```query
ContainerInstanceLog_CL
| where Message contains("warn")
```

Komplexere Abfragen werden ebenfalls unterstützt. Diese Abfrage zeigt beispielsweise nur die Protokolleinträge für die Containergruppe „mycontainergroup001“ an, die innerhalb der letzten Stunde generiert wurden:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Nächste Schritte

### <a name="log-analytics"></a>Log Analytics

Weitere Informationen zum Abfragen von Protokollen und Konfigurieren von Warnungen in Azure Log Analytics finden Sie in den folgenden Themen:

* [Grundlegendes zu Protokollsuchvorgängen in Log Analytics](../log-analytics/log-analytics-log-search.md)
* [Die neue Oberfläche „Warnungen“ in Azure Monitor](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>Überwachen von Container-CPU und -Arbeitsspeicher

Informationen zur Überwachung der CPU- und Arbeitsspeicherressourcen von Containerinstanzen finden Sie unter folgendem Link:

* [Überwachen von Containerressourcen in Azure Container Instances](container-instances-monitor.md)

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://aka.ms/LogAnalyticsLanguage

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create