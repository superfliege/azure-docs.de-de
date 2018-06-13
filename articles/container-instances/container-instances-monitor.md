---
title: Überwachen von Containern in Azure Container Instances
description: Enthält Details zur Überwachung des Verbrauchs von Computeressourcen, z.B. CPU und Arbeitsspeicher, durch Ihre Container in Azure Container Instances.
services: container-instances
author: neilpeterson
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 04/24/2018
ms.author: nepeters
ms.openlocfilehash: 814346bd8021b996b64cd7f0311f31b13b32a8c8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32180401"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>Überwachen von Containerressourcen in Azure Container Instances

Azure Monitor ermöglicht einen Einblick in die Computeressourcen, die von Ihren Containerinstanzen verwendet werden. Verwenden Sie Azure Monitor, um die CPU- und Arbeitsspeicherauslastung von Containergruppen und den darin enthaltenen Containern nachzuverfolgen. Mit diesen Daten zur Ressourcenverwendung können Sie die besten CPU- und Arbeitsspeichereinstellungen für Ihre Containergruppen ermitteln.

In diesem Dokument wird die Erfassung der CPU- und Arbeitsspeicherauslastung für Containerinstanzen per Azure-Portal und Azure CLI ausführlich beschrieben.

> [!IMPORTANT]
> Derzeit sind Metriken zur Ressourcenverwendung nur für Linux-Container verfügbar.
>

## <a name="available-metrics"></a>Verfügbare Metriken

Azure Monitor stellt Metriken zur Auslastung von **CPU** und **Arbeitsspeicher** für Azure Container Instances bereit. Beide Metriken sind für eine Containergruppe und einzelne Container verfügbar.

CPU-Metriken werden in **Millicore** ausgedrückt. Ein Millicore ist ein Tausendstel eines CPU-Kerns. 500 Millicore (bzw. 500 m) geben für einen CPU-Kern also eine Auslastung von 50% an.

Arbeitsspeichermetriken werden in **Byte** ausgedrückt.

## <a name="get-metrics---azure-portal"></a>Abrufen von Metriken – Azure-Portal

Wenn eine Containergruppe erstellt wird, sind im Azure-Portal Azure Monitor-Daten verfügbar. Wählen Sie die Ressourcengruppe und anschließend die Containergruppe aus, um die Metriken für eine Containergruppe anzuzeigen. Hier sind die vorab erstellten Diagramme für die CPU- und Arbeitsspeicherauslastung dargestellt.

![Duales Diagramm][dual-chart]

Wenn Sie über eine Containergruppe mit mehreren Containern verfügen, können Sie eine [Dimension][monitor-dimension] verwenden, um Metriken für jeden einzelnen Container darzustellen. Führen Sie die folgenden Schritte aus, um ein Diagramm mit einzelnen Containermetriken zu erstellen:

1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Monitor**.
2. Wählen Sie eine Containergruppe und eine Metrik (CPU oder Arbeitsspeicher) aus.
3. Wählen Sie die grüne Dimensionsschaltfläche und dann die Option **Containername**.

![Dimension][dimension]

## <a name="get-metrics---azure-cli"></a>Abrufen von Metriken – Azure CLI

Die CPU- und Arbeitsspeicherauslastung für Containerinstanzen kann auch per Azure CLI erfasst werden. Rufen Sie zuerst die ID der Containergruppe ab, indem Sie den folgenden Befehl verwenden. Ersetzen Sie `<resource-group>` durch Ihre Ressourcengruppennamen und `<container-group>` durch den Namen Ihrer Containergruppe.


```console
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

Verwenden Sie den folgenden Befehl, um die Metriken zur Auslastung der **CPU** abzurufen.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table

Timestamp            Name              Average
-------------------  ------------  -----------
2018-04-22 04:39:00  CPU Usage
2018-04-22 04:40:00  CPU Usage
2018-04-22 04:41:00  CPU Usage
2018-04-22 04:42:00  CPU Usage
2018-04-22 04:43:00  CPU Usage      0.375
2018-04-22 04:44:00  CPU Usage      0.875
2018-04-22 04:45:00  CPU Usage      1
2018-04-22 04:46:00  CPU Usage      3.625
2018-04-22 04:47:00  CPU Usage      1.5
2018-04-22 04:48:00  CPU Usage      2.75
2018-04-22 04:49:00  CPU Usage      1.625
2018-04-22 04:50:00  CPU Usage      0.625
2018-04-22 04:51:00  CPU Usage      0.5
2018-04-22 04:52:00  CPU Usage      0.5
2018-04-22 04:53:00  CPU Usage      0.5
```

Und den folgenden Befehl, um die Metriken zur Auslastung des **Arbeitsspeichers** abzurufen.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table

Timestamp            Name              Average
-------------------  ------------  -----------
2018-04-22 04:38:00  Memory Usage
2018-04-22 04:39:00  Memory Usage
2018-04-22 04:40:00  Memory Usage
2018-04-22 04:41:00  Memory Usage
2018-04-22 04:42:00  Memory Usage  6.76915e+06
2018-04-22 04:43:00  Memory Usage  9.22061e+06
2018-04-22 04:44:00  Memory Usage  9.83552e+06
2018-04-22 04:45:00  Memory Usage  8.42906e+06
2018-04-22 04:46:00  Memory Usage  8.39526e+06
2018-04-22 04:47:00  Memory Usage  8.88013e+06
2018-04-22 04:48:00  Memory Usage  8.89293e+06
2018-04-22 04:49:00  Memory Usage  9.2073e+06
2018-04-22 04:50:00  Memory Usage  9.36243e+06
2018-04-22 04:51:00  Memory Usage  9.30509e+06
2018-04-22 04:52:00  Memory Usage  9.2416e+06
2018-04-22 04:53:00  Memory Usage  9.1008e+06
```

Für eine Gruppe mit mehreren Containern kann die Dimension `containerName` hinzugefügt werden, um diese Daten pro Container zurückzugeben.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --dimension containerName --output table

Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2018-04-22 17:03:00  Memory Usage  aci-tutorial-app      1.95338e+07
2018-04-22 17:04:00  Memory Usage  aci-tutorial-app      1.93096e+07
2018-04-22 17:05:00  Memory Usage  aci-tutorial-app      1.91488e+07
2018-04-22 17:06:00  Memory Usage  aci-tutorial-app      1.94335e+07
2018-04-22 17:07:00  Memory Usage  aci-tutorial-app      1.97714e+07
2018-04-22 17:08:00  Memory Usage  aci-tutorial-app      1.96178e+07
2018-04-22 17:09:00  Memory Usage  aci-tutorial-app      1.93434e+07
2018-04-22 17:10:00  Memory Usage  aci-tutorial-app      1.92614e+07
2018-04-22 17:11:00  Memory Usage  aci-tutorial-app      1.90659e+07
2018-04-22 16:12:00  Memory Usage  aci-tutorial-sidecar  1.35373e+06
2018-04-22 16:13:00  Memory Usage  aci-tutorial-sidecar  1.28614e+06
2018-04-22 16:14:00  Memory Usage  aci-tutorial-sidecar  1.31379e+06
2018-04-22 16:15:00  Memory Usage  aci-tutorial-sidecar  1.29536e+06
2018-04-22 16:16:00  Memory Usage  aci-tutorial-sidecar  1.38138e+06
2018-04-22 16:17:00  Memory Usage  aci-tutorial-sidecar  1.41312e+06
2018-04-22 16:18:00  Memory Usage  aci-tutorial-sidecar  1.49914e+06
2018-04-22 16:19:00  Memory Usage  aci-tutorial-sidecar  1.43565e+06
2018-04-22 16:20:00  Memory Usage  aci-tutorial-sidecar  1.408e+06
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Überwachung mit Azure finden Sie unter [Überwachen von Azure-Anwendungen und -Ressourcen][azure-monitoring].

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - Internal -->
[azure-monitoring]: ../monitoring-and-diagnostics/monitoring-overview.md
[monitor-dimension]: ../monitoring-and-diagnostics/monitoring-metric-charts.md#what-are-multi-dimensional-metrics
