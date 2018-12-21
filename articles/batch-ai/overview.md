---
title: Azure Batch AI-Dienst – AI-Training | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie den verwalteten Azure Batch AI-Dienst verwenden, um KI-Modelle (Artificial Intelligence, AI) und andere Machine Learning-Modelle in Clustern mit GPUs und CPUs trainieren.
services: batch-ai
documentationcenter: ''
author: alexsuttonms
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/01/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 37f1bf5dc20d097f7f5f560e3bf1fdd25dc38045
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408050"
---
# <a name="what-is-azure-batch-ai"></a>Was ist Azure Batch AI?

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Azure Batch AI ist ein verwalteter Dienst, mit dem Data Scientists und KI-Forscher Machine Learning- und KI-Modelle in Azure bedarfsorientiert trainieren und testen können, ohne eine komplexe Infrastruktur verwalten zu müssen. Sie müssen nur beschreiben, welche Computeressourcen und Aufträge Sie ausführen möchten, wo die Eingaben und Ausgaben der Modelle gespeichert werden, und die Batch AI kümmert sich um den Rest.

Sie können Batch AI als eigenständige Lösung ausführen oder zum Trainieren des Modells als Teil eines größeren Entwicklungsworkflows verwenden:

* Verwenden Sie die Batch AI selbst, um Machine Learning- und AI-Modelle für [GPU](../virtual-machines/linux/sizes-gpu.md)- oder CPU-Cluster zu trainieren, zu testen und um eine Batchbewertung durchzuführen. 

* Wählen Sie in einem Workflow mit [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) oder anderen [Tools der Azure AI-Platform](https://azure.microsoft.com/overview/ai-platform/) als Ziel einen Batch AI-Cluster. Azure ML bietet eine reichhaltige Erfahrung für die Datenaufbereitung, Experimente und den Auftragsverlauf. Azure ML kann auch ein trainiertes Modell in einen Container packen und ein Modell für Rückschlüsse oder für IoT-Geräte bereitstellen.  

## <a name="train-machine-learning-and-ai-models"></a>Trainieren von Machine Learning- und KI-Modellen

Verwenden Sie Batch AI, um Machine Learning-Modelle sowie tiefe neuronale Netzwerke (Deep Learning) und andere KI-Ansätze zu trainieren. Batch AI bietet integrierte Unterstützung für beliebte Open Source-KI-Bibliotheken und Frameworks wie [TensorFlow](https://github.com/tensorflow/tensorflow), [PyTorch](https://github.com/pytorch/pytorch), [Chainer](https://github.com/chainer/chainer) und [Microsoft Cognitive Toolkit (CNTK)](https://github.com/Microsoft/CNTK).

Nachdem Sie Ihren Problembereich identifiziert und Ihre Daten vorbereitet haben, arbeiten Sie interaktiv mit Batch AI zusammen, um Modellideen zu testen. Wenn Sie dann bereit sind, zu experimentieren, starten Sie Aufträge über mehrere GPUs mit MPI oder anderen Kommunikationsbibliotheken, und führen Sie weitere Experimente parallel durch.

Mit Batch AI können Sie die Modelle auf verschiedene Weise umfangreich trainieren. Beispiel:  

|  |  |
|---------|---------|
| **Verteilten eines Trainings**<br/>![Verteiltes Training](./media/overview/distributed-training.png)  | Skalieren Sie das Training für einen einzelnen Auftrag über viele, mit dem Netzwerk verbundene GPUs hinweg zentral hoch, um größere Netzwerke mit großen Datenmengen zu trainieren.|
| **Experiment**<br/>![Experimentieren](./media/overview/experimentation.png) | Skalieren Sie das Training horizontal mit mehreren Aufträgen hoch. Führen Sie Parameter-Sweep-Vorgänge durch, um neue Ideen zu testen, oder optimieren Sie Hyperparameter, um die Genauigkeit und Leistung zu optimieren. |
| **Parallele Ausführung**![Parallele Ausführung](./media/overview/parallel-execution.png) | Trainieren oder bewerten Sie mehrere Modellen gleichzeitig, die parallel für eine Reihe von Servern ausgeführt werden, um Aufgaben schneller zu erledigen.|

Wenn Sie ein Modell trainiert haben, verwenden Sie das Batch AI zum Testen des Modells, wenn die nicht schon Teil Ihres Trainingsskripts war, oder führen Sie eine Batchbewertung aus.

## <a name="how-it-works"></a>So funktioniert's

Verwenden Sie Batch AI SDKs, Befehlszeilenskripte oder das Azure-Portal, um Computeressourcen zu verwalten und Aufträge für KI-Schulungen und Tests zu planen: 

* **Bereitstellung und Skalierung von Clustern mit VMs** – Wählen Sie die Anzahl der Knoten (VMs), und wählen Sie eine GPU-fähige oder eine andere VM-Größe, die Ihren Trainingsanforderungen entspricht. Skalieren Sie die Anzahl der Knoten automatisch oder manuell nach oben oder unten, sodass Sie nur bei Bedarf Ressourcen verwenden. 

* **Verwalten von Abhängigkeiten und Containern** – Standardmäßig führen Batch AI-Cluster Linux-VM-Images mit vorinstallierten Abhängigkeiten aus, um containerbasierte Trainingsframeworks entweder auf GPUs oder CPUs auszuführen. Für zusätzliche Konfiguration verwenden Sie benutzerdefinierte Images oder führen Sie ein Startskript aus.

* **Verteilen von Daten** – Wählen Sie eine oder mehrere Speicheroptionen, um Eingabedaten und Skripte sowie die Auftragsausgabe zu verwalten: Azure Files, Azure Blob Storage oder ein verwalteter NFS-Server. Batch AI unterstützt auch benutzerdefinierte Speicherlösungen, einschließlich hochleistungsfähige parallele Dateisysteme. Mithilfe einfacher Konfigurationsdateien können Sie Speicherdateisysteme in die Clusterknoten und Auftragscontainer einbinden.

* **Geplante Aufträge** – Übermitteln Sie Aufträge an prioritätsbasierte Auftragswarteschlange zum gemeinsamen Nutzen von Clusterressourcen und verwenden Sie virtuelle Computer mit niedriger Priorität und reservierten Instanzen.

* **Behandeln von Fehlern** – Überwachen Sie den Auftragsstatus und starten Sie Aufträge neu, wenn es bei potenziell lang laufenden Aufträgen zu VM-Ausfällen kommt.

* **Erfassen der Ergebnisse** – Greifen Sie einfach auf Ausgabeprotokolle, Stdout, Stderr und trainierte Modelle zu. Konfigurieren Sie Ihre Batch AI-Aufträge, um die Ausgabe direkt in den bereitgestellten Speicher zu übertragen.

Als Azure-Dienst unterstützt Batch AI gängige Tools wie rollenbasierte Zugriffssteuerung (RBAC) und virtuelle Azure-Netzwerke für die Sicherheit.  

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [Batch AI-Ressourcen](resource-concepts.md) zum Trainieren eines Machine Learning- oder AI-Modells.

* Erste Schritte beim [Trainieren eines Beispielmodells für Deep Learning](quickstart-tensorflow-training-cli.md) mit Batch AI.

* Sehen Sie sich die Beispiele für [Trainingsrezepte](https://github.com/Azure/BatchAI/blob/master/recipes) für beliebte AI-Frameworks an.
