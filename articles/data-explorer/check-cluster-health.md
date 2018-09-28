---
title: Überprüfen der Integrität eines Azure-Daten-Explorer-Clusters
description: In diesem Artikel wird beschrieben, wie Sie in mehreren Schritten bestimmen, ob Ihr Azure-Daten-Explorer-Cluster fehlerfrei ausgeführt wird.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0746247d2c912ba66e81b95f45b168e32b522130
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988426"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Überprüfen der Integrität eines Azure-Daten-Explorer-Clusters

Die Integrität eines Azure-Daten-Explorer-Clusters wird von verschiedenen Faktoren beeinflusst, einschließlich der CPU, dem Arbeitsspeicher und dem Datenträgersubsystem. In diesem Artikel werden einige grundlegende Schritte aufgeführt, die Sie ausführen können, um die Integrität eines Clusters zu bestimmen.

1. Melden Sie sich bei [https://dataexplorer.azure.com](https://dataexplorer.azure.com) an.

1. Klicken Sie im linken Bereich auf Ihren Cluster, und führen Sie den folgenden Befehl aus.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Wenn 1 ausgegeben wird, ist der Cluster fehlerfrei. Die Ausgabe 0 deutet auf Fehler hin.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Cluster.

1. Klicken Sie unter **Überwachung** erst auf **Metriken** und anschließend wie im folgenden Bild gezeigt auf **Keep Alive**. Wenn die Ausgabe ungefähr 1 beträgt, ist der Cluster fehlerfrei.

    ![Keep-Alive-Metrik für den Cluster](media/check-cluster-health/portal-metrics.png)

1. Fügen Sie andere Metriken (z.B. solche für die CPU oder einen Speichercache) hinzu, um die Ressourcennutzung im Zusammenhang mit dem Cluster zu bestimmen.

1. Wenn Sie Hilfe bei der Diagnose von Problemen mit der Integrität eines Clusters benötigen, erstellen Sie eine Supportanfrage im [Azure-Portal](https://portal.azure.com).