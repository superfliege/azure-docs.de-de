---
title: 'Azure-Schnellstart: Erstellen eines Batch AI-Clusters – Portal | Microsoft-Dokumentation'
description: 'Schnellstart: Erstellen eines Batch AI-Clusters für das Training von Machine Learning- und KI-Modellen – Azure-Portal'
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 8b9daa0fbbf84e0f602498a0847c9e120f709b17
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057555"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-portal"></a>Schnellstart: Erstellen eines Clusters für Batch AI-Trainingsaufträge über das Azure-Portal

Dieser Schnellstart zeigt, wie Sie das Azure-Portal verwenden können, um einen Batch AI-Cluster zu erstellen, den Sie für das Training von KI- und Machine Learning-Modellen verwenden können. Batch AI ist ein verwalteter Dienst, mit dem Data Scientists und KI-Forscher KI- und Machine Learning-Modelle in Clustern mit virtuellen Azure-Computern trainieren können.

Der Cluster hat Anfangs einen einzelnen GPU-Knoten und einen angehängten Dateiserver. Nach Abschluss dieser Schnellstartanleitung haben Sie einen Cluster, den Sie zentral hochskalieren und zum Trainieren von Deep Learning-Modellen verwenden können. Übermitteln Sie Trainingsaufträge an den Cluster mit Batch AI, [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md)-Tools oder den [Visual Studio Tools for AI](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Für diesen Schnellstart benötigen Sie ein SSH-Schlüsselpaar. Sie können diesen Schritt überspringen, wenn Sie bereits über ein vorhandenes SSH-Schlüsselpaar verfügen.

Wenn Sie ein SSH-Schlüsselpaar erstellen möchten, führen Sie den folgenden Befehl über eine Bash-Shell aus, und befolgen Sie die Anweisungen auf dem Bildschirm. Sie können beispielsweise [Azure Cloud Shell](../cloud-shell/overview.md) oder auf Windows das [Windows-Subsystem für Linux](/windows/wsl/install-win10) verwenden. Die Ausgabe des Befehls enthält den Dateinamen der Datei mit dem öffentlichen Schlüssel. Kopieren Sie den Inhalt der öffentlichen Schlüsseldatei (`cat ~/.ssh/id_rsa.pub`) in die Zwischenablage oder einen anderen Speicherort, auf den Sie in einem späteren Schritt zugreifen können.

```bash
ssh-keygen -t rsa -b 2048
```

Weitere Informationen zum Erstellen von SSH-Schlüsselpaaren finden Sie unter [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-batch-ai-workspace"></a>Erstellen eines Batch AI-Arbeitsbereichs

Erstellen Sie zunächst einen Batch AI-Arbeitsbereich, um Ihre Batch AI-Ressourcen zu organisieren. Ein Arbeitsbereich kann eine oder mehrere Cluster oder andere Batch AI-Ressourcen enthalten.

1. Wählen Sie **Alle Dienste** und filtern Sie nach **Batch AI**.

2. Wählen Sie **Arbeitsbereich hinzufügen**.

3. Geben Sie Werte für **Arbeitsbereichsname** und **Ressourcengruppe** ein. Wenn Sie möchten, können Sie verschiedene Optionen für das **Abonnement** und den **Speicherort** für den Arbeitsbereich auswählen. Wählen Sie **Arbeitsbereich erstellen** aus.

  ![Erstellen eines Batch AI-Arbeitsbereichs](./media/quickstart-create-cluster-portal/create-workspace.png)

Wenn die Meldung **Bereitstellung erfolgreich** angezeigt wird, wechseln Sie zur erstellten Ressource und wählen Sie den Arbeitsbereich aus.

## <a name="create-a-file-server"></a>Erstellen eines Dateiservers

Ein Batch AI-Dateiserver ist NFS mit einem Knoten, das automatisch auf Clusterknoten bereitgestellt werden kann. Es ist eine von mehreren Möglichkeiten, wie Sie Eingabedaten und Ausgaben aus Ihren Trainingsaufträgen speichern können.

1. Wählen Sie im Arbeitsbereich **Dateiserver** > **Batch Ai-Dateiserver hinzufügen**.

2. Geben Sie Werte für **Dateiservername** und **VM-Größe** ein. Im Rahmen dieser Schnellstartanleitung wird eine VM-Größe von *Standard D1_v2* für den Dateiserver empfohlen. Wählen Sie eine andere Größe, wenn Sie größere Mengen an Ein- oder Ausgabedaten für Trainingsaufträge speichern müssen.

3. Geben Sie einen **Administratorbenutzernamen** ein, und kopieren Sie den Inhalt Ihrer öffentlichen SSH-Schlüsseldatei in **SSH-Schlüssel**. Übernehmen Sie für die übrigen Werte die Standardwerte und wählen Sie **Dateiserver erstellen**.

  ![Batch AI-Dateiserver erstellen](./media/quickstart-create-cluster-portal/create-file-server.png)

Die Bereitstellung des Dateiservers dauert einige Minuten.

Nachdem der Server erstellt wurde, klicken Sie auf **Eigenschaften**, und notieren Sie sich die **Einbindungseinstellungen**. Sie können SSH an die öffentliche IP-Adresse des Servers senden, um Trainingsdaten und Ausgabedateien im angegebenen Verzeichnis (*/data*) hoch- und herunterzuladen.

![Dateiservereigenschaften](./media/quickstart-create-cluster-portal/file-server-properties.png)

## <a name="create-a-cluster"></a>Erstellen eines Clusters

Mit den folgenden Schritten erstellen Sie einen Clustern mit einem einzelnen GPU-Knoten. Der Clusterknoten führt ein Standard-Ubuntu-Server-Image aus, das für das Hosting von containerbasierten Anwendungen entwickelt wurde, die Sie für die meisten Trainingsworkloads verwenden können. Der Clusterknoten bindet den Dateiserver an seinem Bereitstellungspunkt ein. 

1. Wählen Sie in Ihrem Batch AI-Arbeitsbereich **Cluster** > **Batch Ai-Cluster hinzufügen** aus.

2. Geben Sie Werte für **Clustername** und die folgenden Einstellungen ein. Die empfohlene VM-Größe verfügt über eine NVIDIA Tesla K80-GPU.
  
   |Einstellung  |Wert  |
   |---------|---------|
   |**Größe des virtuellen Computers**     |Standard NC6|
   |**Vorgegebene Anzahl von Knoten**     |1|

3. Geben Sie einen **Administratorbenutzernamen** ein, und kopieren Sie den Inhalt Ihrer öffentlichen SSH-Schlüsseldatei in **SSH-Schlüssel**. Akzeptieren Sie die Standardwerte für die verbleibenden Werte auf dieser Seite, und wählen **Weiter: Knotensetup**.

   ![Grundlegende Clusterinformationen eingeben](./media/quickstart-create-cluster-portal/create-cluster.png)

4. Klicken Sie unter **Volumes einbinden** die Option **Dateiserververweisen** > **Hinzufügen**. Wählen Sie den zuvor erstellten Dateiserver. Geben Sie einen **relativen Einbindungspfad** ein, unter dem der Dateiserver auf jedem Clusterknoten eingebunden wird. Wählen Sie **Speichern und fortfahren** aus.

   ![Dateiserververweis hinzufügen](./media/quickstart-create-cluster-portal/file-server-reference.png)

Speichern Sie das Knotensetup, und wählen Sie **Cluster erstellen** aus.

Batch AI benötigt einige Minuten, um den Knoten zuzuordnen. Während dieses Zeitraums lautet der **Zuweisungsstatus** des Clusters **Größe ändern**. Nach einigen Minuten ändert sich der Status des Clusters in **Bereit**, und die Knoten werden gestartet.

![Cluster starten](./media/quickstart-create-cluster-portal/cluster-resizing.png)

Wählen Sie den Clusternamen, um den Status des Knotens zu überprüfen. Wenn der Status eines Knotens **Leerlauf** lautet, ist er zum Ausführen von Trainingsaufträgen bereit.

### <a name="list-cluster-nodes"></a>Auflisten von Clusterknoten

Wenn Sie sich mit den Clusterknoten (in diesem Fall einem einzelnen Knoten) verbinden müssen, um Anwendungen zu installieren oder Wartungsarbeiten durchzuführen, erhalten Sie Verbindungsinformationen im Portal. Nachdem der Cluster erstellt wurde, klicken Sie auf **Knoten** und beachten Sie die Einstellungen der SSH-**Verbindung** (IP-Adresse und Portnummer).

![Clusterknoten](./media/quickstart-create-cluster-portal/cluster-nodes.png)

Verwenden Sie diese Informationen, um die SSH-Verbindung zum Knoten herzustellen. Ersetzen Sie z.B. die richtige IP-Adresse und Portnummer Ihres Knotens in folgendem Befehl:

```bash
ssh myusername@137.135.82.15 -p 50000
``` 

### <a name="resize-the-cluster"></a>Ändern der Clustergröße

Wenn Sie Ihren Cluster zum Trainieren eines Modells verwenden, benötigen Sie möglicherweise mehr Computeressourcen. Um beispielsweise die Größe auf 2 Knoten für einen verteilten Trainingsauftrag zu erhöhen, wählen Sie **Skalieren** und stellen Sie die **Zielnummer des Knotens** auf 2. Speichern Sie die Konfiguration.

![Skalieren von Clustern](./media/quickstart-create-cluster-portal/scale-cluster.png)

Es dauert einige Minuten, bis die Größe des Clusters geändert wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Batchtutorials und -beispielen fortfahren möchten, können Sie den mithilfe dieser Schnellstartanleitung erstellten Batch AI-Arbeitsbereich, Dateiserver und Cluster verwenden.

Es entstehen Kosten für den Batch AI-Cluster und den Dateiserver, während die zugrunde liegenden virtuellen Maschinen ausgeführt werden, auch wenn keine Aufträge geplant sind. Wenn Sie die Clusterkonfiguration beibehalten möchten, wenn Sie keine Aufträge ausführen müssen, passen Sie die Größe des Clusters auf 0 Knoten an. Ändern Sie die Größe später auf 1 oder mehr Knoten, um Ihre Aufträge auszuführen. 

Wenn Sie ihn nicht mehr benötigen, löschen Sie den Batch AI-Arbeitsbereich, der den Cluster und den Dateiserver enthält. Wählen Sie dazu den Batch AI-Arbeitsbereich und dann **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mit dem Azure-Portal einen Batch AI-Cluster und einen angehängten Dateiserver erstellen. Um zu erfahren, wie Sie einen Batch AI-Cluster zum Trainieren eines Modells verwenden können, fahren Sie mit dem Schnellstart zum Trainieren eines Deep Learning-Modells fort.

> [!div class="nextstepaction"]
> [Trainieren eines Deep Learning-Modells](./quickstart-tensorflow-training-cli.md)
