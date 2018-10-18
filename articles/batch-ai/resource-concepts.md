---
title: Informationen zu Batch AI-Ressourcen – Azure | Microsoft-Dokumentation
description: Übersicht über Arbeitsbereiche, Cluster, Dateiserver, Experimente und Aufträge im Batch AI-Dienst in Microsoft Azure.
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
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 4a9e3529f9d68ecdc614ea69cffc6897891f4548
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44056816"
---
# <a name="overview-of-resources-in-batch-ai"></a>Übersicht über die Ressourcen in Batch AI

Wenn Sie den Batch AI-Dienst zum ersten Mal verwenden, sollten Sie sich mit den verfügbaren Batch AI-Ressourcen vertraut machen. Wie bei anderen Azure-Diensten erstellen Sie Batch AI-Ressourcen in einer oder mehreren Azure-*Ressourcengruppen*. Sie erstellen einen oder mehrere Batch AI-*Arbeitsbereiche* in einer Ressourcengruppe. Jeder Arbeitsbereich enthält eine Mischung aus *Clustern*, *Dateiservern* und *Experimenten* von Batch AI. In einem Batch AI-Experiment wird eine Gruppe von *Aufträgen* gekapselt.

In der folgenden Abbildung sehen Sie ein Beispiel für eine Ressourcenhierarchie für Batch AI. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

In den folgenden Abschnitten werden die Batch AI-Ressourcen ausführlicher beschrieben.

## <a name="workspace"></a>Arbeitsbereich

Ein Arbeitsbereich in Batch AI ist eine allgemeine Sammlung der übrigen Batch AI-Ressourcen. In Arbeitsbereichen können Aufgaben, die unterschiedlichen Gruppen oder Projekten zugeordnet sind, getrennt werden. Beispielsweise können Sie einen Arbeitsbereich für die Entwicklung und einen Arbeitsbereich für Tests erstellen.

## <a name="cluster"></a>Cluster

Ein Cluster in Batch AI enthält die Computeressourcen zum Ausführen von Aufträgen. Alle Knoten in einem Cluster haben die gleiche VM-Größe und das gleiche Betriebssystemimage. Batch AI umfasst viele Optionen zum Erstellen von Clustern, die an unterschiedliche Anforderungen angepasst sind. Normalerweise richten Sie jeweils einen Cluster für jede Kategorie der zum Abschließen eines Projekts erforderlichen Verarbeitungsleistung ein. Sie können die Batch AI-Cluster je nach Nachfrage und Budget zentral hoch- und herunterskalieren. Weitere Informationen finden Sie unter [Arbeiten mit Batch AI-Clustern](clusters.md).

## <a name="file-server"></a>Dateiserver

Erstellen Sie optional einen Dateiserver in Batch AI zum Speichern von Daten, Trainingsskripts und Ausgabeprotokollen. Ein Batch AI-Dateiserver ist ein verwaltetes NFS mit einem Knoten, das automatisch in Clusterknoten eingebunden werden kann und so einen einfachen und zentral zugänglichen Speicherort für Aufträge darstellt. In den meisten Fällen wird in einem Arbeitsbereich nur ein Dateiserver benötigt. Sie können dann Daten für Ihre Trainingsaufträge in verschiedenen Verzeichnissen speichern. Wenn das NFS nicht für Ihre Workloads geeignet ist, unterstützt Batch AI andere Speicheroptionen, z.B. [Azure Storage](use-azure-storage.md) oder benutzerdefinierte Lösungen wie ein Gluster- oder Lustre-Dateisystem.

## <a name="experiment"></a>Experiment

Ein Experiment gruppiert eine Sammlung verbundener Aufträge, die Sie zusammen abfragen und verwalten. Jeder Arbeitsbereich kann mehrere Experimente umfassen, wobei in jedem Experiment versucht wird, ein spezifisches Problem zu lösen.

## <a name="job"></a>Auftrag

Ein Auftrag ist ein einzelner Task oder ein Skript, der bzw. das ausgeführt werden muss, beispielsweise um ein Deep Learning-Modell zu trainieren. Jeder Auftrag führt ein bestimmtes Skript in einem Cluster im Arbeitsbereich aus. (Das Skript kann auf einem Batch AI-Dateiserver oder in einer anderen Speicherlösung gespeichert werden.) Jedem Batch AI-Auftag ist ein Frameworktyp zugeordnet: TensorFlow, Horovod, CNTK, Caffe, Caffe2, pyTorch, Chainer, benutzerdefiniertes MPI oder ein benutzerdefinierter Typ. Für jedes Framework richtet der Batch AI-Dienst die erforderliche Infrastruktur ein und verwaltet die Auftragsprozesse. Jedes Experiment kann mehrere Aufträge umfassen, die sich abgesehen von einigen Änderungen an unterschiedlichen Parametern ähneln.

## <a name="next-steps"></a>Nächste Schritte

* Führen Sie Ihren ersten [Batch AI-Trainingsauftrag](quickstart-tensorflow-training-cli.md) aus.

* Sehen Sie sich die Beispiele für [Trainingsrezepte](https://github.com/Azure/BatchAI/tree/master/recipes) für verschiedene Frameworks an.