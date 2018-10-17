---
title: Rendern einer Blender-Szene mithilfe von Azure Batch und Batch Explorer
description: 'Tutorial: Rendern mehrerer Frames einer Blender-Szene mithilfe von Azure Batch und der Batch Explorer-Clientanwendung'
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: tutorial
ms.openlocfilehash: 46c65cd7ac5734134fa7c4ad6fd85f39d1188e28
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392552"
---
# <a name="tutorial-render-a-blender-scene-using-batch-explorer"></a>Tutorial: Rendern einer Blender-Szene mithilfe von Batch Explorer

Dieses Tutorial zeigt, wie mehrere Frames einer Blender-Demoszene gerendert werden. Für dieses Tutorial wird Blender verwendet, da diese Software sowohl für den Client als auch die Rendering-VMs kostenlos ist. Der Prozess ist jedoch bei anderen Anwendungen wie Maya oder 3ds Max ähnlich.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Hochladen einer Blender-Szene in Azure-Speicher
> * Erstellen eines Batch-Pools mit mehreren Knoten zum Ausführen des Renderings
> * Rendern mehrerer Frames
> * Anzeigen und Herunterladen der gerenderten Frame-Dateien

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Abonnement mit nutzungsbasierter Bezahlung oder eine andere Azure-Kaufoption, um die Renderinganwendungen in Batch auf nutzungsbasierter Basis zu verwenden. Nutzungsbasierte Lizenzierung wird nicht unterstützt, wenn Sie ein kostenloses Azure-Angebot verwenden, das mit monetären Gutschriften verbunden ist.

Sie benötigen ein Azure Batch-Konto mit einem zugeordneten Speicherkonto.  Informationen zum Erstellen eines Batch-Kontos finden Sie in den Batch-Schnellstartartikeln, etwa [Schnellstart: Ausführen Ihres ersten Batch-Auftrags mit der Azure CLI](https://docs.microsoft.com/azure/batch/quick-create-cli).

Für die in diesem Tutorial angegebene VM-Größe und -Anzahl ist ein Kernkontingent mit niedriger Priorität von mindestens 50 Kernen erforderlich. Das Standardkontingent kann zwar verwendet werden, Sie benötigen dann aber eine kleinere VM-Größe, was bedeutet, dass das Rendern der Bilder länger dauert. Die Vorgehensweise zum Anfordern eines höheren Kernkontingents wird in [diesem Artikel](https://docs.microsoft.com/azure/batch/batch-quota-limit) ausführlich beschrieben.

Abschließend muss [Batch Explorer](https://azure.github.io/BatchExplorer/) installiert werden. Dieses Hilfsprogramm ist für Windows, OSX und Linux verfügbar. Wenn [Blender](https://www.blender.org/download/) installiert ist, kann die Beispielmodelldatei angezeigt werden. Dies ist jedoch optional.

## <a name="download-the-demo-scene"></a>Herunterladen der Demoszene

Laden Sie die ZIP-Demodatei „Class room“ von der [Webseite mit Blender-Demodateien](https://www.blender.org/download/demo-files/) herunter, und entzippen Sie sie auf einem lokalen Laufwerk.

## <a name="upload-a-scene-to-azure-storage"></a>Hochladen einer Szene in Azure-Speicher

Erstellen Sie einen Speicherkontocontainer für die Dateien mit den Demoszenen:

* Starten Sie Batch Explorer.
* Klicken Sie im Hauptmenü auf der linken Seite auf das Menüelement „Daten“.
* Vergewissern Sie sich, dass in der Dropdownliste die Option „Dateigruppen“ ausgewählt ist.
* Klicken Sie auf die Schaltfläche mit dem Pluszeichen (+), und erstellen Sie eine neue leere Dateigruppe mit dem Namen „blender-classroom“.
  * Eine Dateigruppe ist einfach ein Azure Storage-Blobcontainer mit dem Präfix „fgrp-“. Mit dieser Konvention werden andere Container im Speicherkonto herausgefiltert.

![Dateigruppe für Szenedateien](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_scene_filegroup.png)

Laden Sie die Szenedateien hoch:

* Wählen Sie den neuen Container aus, und verschieben Sie den Inhalt des Ordners „classroom“ per Drag & Drop in den Container in Batch Explorer.

![Hochgeladene Szenedateien](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_scene_filegroup_uploaded.png)

## <a name="create-azure-storage-container-for-output-images"></a>Erstellen eines Azure-Speichercontainers für Ausgabebilder

Erstellen Sie einen Speicherkontocontainer für die Ausgabedateien mit den Demoszenen:

* Klicken Sie auf der linken Seite im Hauptmenü auf das Menüelement „Daten“.
* Klicken Sie auf die Schaltfläche mit dem Pluszeichen (+), und erstellen Sie eine neue leere Dateigruppe mit dem Namen „render-output“.

![Dateigruppe für Ausgabedateien](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_output_filegroup.png)

## <a name="create-a-pool-of-vms-for-rendering"></a>Erstellen eines Pools mit virtuellen Computern für das Rendering

Erstellen Sie einen Batch-Pool mithilfe des Azure Marketplace-VM-Images zum Rendern, das die Blender-Anwendung enthält:

* Klicken Sie auf der linken Seite im Hauptmenü auf das Menüelement „Katalog“.
* Wählen Sie das Element „Blender“, um die Liste der Anwendungselemente anzuzeigen.
* Wählen Sie die Elemente für das Rendering von Frames unter Windows Server aus.
* Optional können Sie auf das Linksymbol rechts neben dem Element klicken, um die Vorlagendateien anzuzeigen, die zum Erstellen eines Pools und Auftrags verwendet werden.

![Blender-Katalogelemente](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_gallery_item.png)

* Klicken Sie auf die Schaltfläche „Create pool for later use“ (Pool zur späteren Verwendung erstellen).
  * Behalten Sie den Poolnamen „blender-windows“ bei.
  * Legen Sie „Dedicated vm count“ (Anzahl dedizierter virtueller Computer) auf „0“ fest.
  * Legen Sie „Low priority vm count“ (Anzahl virtueller Computer mit niedriger Priorität) auf „3“ fest.
  * Legen Sie „Node size“ (Knotengröße) auf „Standard_F16“ fest. Sie können auch eine andere VM-Größe auswählen, die Dauer zum Rendern eines Frames hängt jedoch hauptsächlich von der Anzahl von Kernen ab.
* Klicken Sie auf die grüne Schaltfläche, um den Pool zu erstellen.
  * Der Pool wird nahezu sofort erstellt, es dauert jedoch einige Minuten, bis die virtuellen Computer zugeordnet und gestartet werden.

![Poolvorlage für Blender](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_pool_template.png)

> [!WARNING]
> Wenn virtuelle Computer in einem Pool vorhanden sind, fallen die Gebühren für diese virtuellen Computer in Ihrem Azure-Abonnement an. Damit keine Gebühren mehr anfallen, müssen Sie den Pool oder die virtuellen Computer löschen. Löschen Sie am Ende des Tutorials den Pool, damit keine laufenden Gebühren anfallen.

Der Status des Pools und der virtuellen Computer kann in der Ansicht „Pools“ überwacht werden. Das folgende Beispiel zeigt, dass alle drei virtuellen Computer zugewiesen wurden, zwei gestartet wurden und sich im Leerlauf befinden und einer noch gestartet wird: ![Pool-Heatmap](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_pool_heatmap.png)

## <a name="create-a-rendering-job"></a>Erstellen eines Renderingauftrags

Erstellen Sie einen Renderingauftrag, um mithilfe des erstellten Pools einige Frames zu rendern:
* Klicken Sie auf der linken Seite im Hauptmenü auf das Menüelement „Katalog“.
* Wählen Sie das Element „Blender“, um die Liste der Anwendungselemente anzuzeigen.
* Wählen Sie die Elemente für das Rendering von Frames unter Windows Server aus.
* Klicken Sie auf die Schaltfläche „Run job with existing pool“ (Auftrag mit vorhandenem Pool ausführen).
* Wählen Sie den Pool „blender-windows“ aus.
* Legen Sie für „Job name“ (Auftragsname) den Namen „blender-render-tutorial1“ fest.
* Wählen Sie unter „Input data“ (Eingabedaten) die Option „fgrp-blender-classroom“ aus.
* Klicken Sie unter „Blend file“ (Blend-Datei) auf das Dateisymbol, und wählen Sie „classroom.blend“ aus.
* Übernehmen Sie „1“ für „Frame start“ (Frame-Start) und „5“ für „Frame end“ (Frame-Ende).
* Legen Sie für „Outputs“ (Ausgaben) die Option „fgrp-render-output“ fest.
* Klicken Sie auf die grüne Schaltfläche, um den Auftrag zu erstellen. Ein Auftrag wird mit fünf Aufgaben (eine für jeden Frame) erstellt.

![Auftragsvorlage für Blender](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_job_template.png)

Nachdem der Auftrag und alle Aufgaben erstellt wurden, wird der Auftrag zusammen mit den Auftragsaufgaben angezeigt: ![Liste der Auftragsaufgaben](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_task_list.png)

Wenn die Ausführung einer Aufgabe auf einem virtuellen Poolcomputer zum ersten Mal gestartet wird, wird eine Aufgabe zur Batch-Auftragsvorbereitung ausgeführt. Dabei werden die Szenedateien aus der Speicherdateigruppe auf den virtuellen Computer kopiert, sodass darauf von Blender zugegriffen werden kann.
Sehen Sie sich die von Blender generierte Protokolldatei „stdout.txt“ an, um den Status des Renderings zu ermitteln.  Wählen Sie eine Aufgabe aus. „Task Outputs“ (Aufgabenausgaben) wird standardmäßig angezeigt, und Sie können die Datei „stdout.txt“ auswählen und anzeigen.
![Datei „stdout“](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_stdout.png)

Wenn der Pool „blender-windows“ ausgewählt ist, werden die virtuellen Computer im Pool mit dem Status „Wird ausgeführt“ angezeigt: ![Pool-Heatmap mit ausgeführten Knoten](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_pool_heatmap_running.png)

Die Erstellung der gerenderten Bilder dauert abhängig von der ausgewählten VM-Größe mehrere Minuten.  Bei Verwendung des zuvor angegebenen virtuellen Computers „F16“ dauerte das Rendern von Frames etwa 16 Minuten.

## <a name="view-the-rendering-output"></a>Anzeigen der Renderingausgabe

Wenn das Rendern von Frames abgeschlossen ist, werden diese Aufgaben als abgeschlossen angezeigt: ![Abgeschlossene Aufgaben](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_tasks_complete.png)

Das gerenderte Bild wird zuerst auf den virtuellen Computer geschrieben und kann durch Auswählen des Ordners „wd“ angezeigt werden: ![Gerendertes Bild im Poolknoten](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_output_image.png)

In der Auftragsvorlage ist darüber hinaus angegeben, dass der Ausgabe-Frame und die Protokolldateien in die Dateigruppe des Azure Storage-Kontos zurückgeschrieben werden, die beim Erstellen des Auftrags angegeben wurde.  Über die Benutzeroberfläche „Data“ (Daten) können die Ausgabedateien und Protokolle angezeigt sowie die Dateien heruntergeladen werden: ![Gerenderte Bilder in einer Speicherdateigruppe](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_output_image_storage.png)

Wenn alle Aufgaben abgeschlossen sind, wird der Auftrag als abgeschlossen markiert: ![Auftrag und alle Aufgaben abgeschlossen](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_job_alltasks_complete.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

> [!WARNING]
> Der Pool muss gelöscht (oder auf null Knoten verkleinert) werden, damit im Azure-Abonnement keine Gebühren mehr für die virtuellen Computer entstehen.

* Klicken Sie auf „Pools“.
* Wählen Sie den Pool „blender-windows“ aus.
* Klicken Sie entweder mit der rechten Maustaste auf „Delete“ (Löschen), oder klicken Sie oberhalb des Pools auf das Papierkorbsymbol.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich im Abschnitt „Gallery“ (Katalog) die über Batch Explorer verfügbaren Renderinganwendungen an.
* Für jede Anwendung stehen verschiedene Vorlagen zur Verfügung, und die Auswahl wird ständig erweitert.  Für Blender sind beispielsweise Vorlagen verfügbar, die ein einzelnes Bild in Kacheln unterteilen, sodass Teile eines Bilds parallel gerendert werden können.
* Eine umfassende Beschreibung der Renderingfunktionen finden Sie in [diesen](https://docs.microsoft.com/azure/batch/batch-rendering-service) Artikeln.
