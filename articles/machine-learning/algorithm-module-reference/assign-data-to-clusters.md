---
title: 'Assign Data to Cluster: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul „Assign Data to Cluster“ in Azure Machine Learning Service verwenden, um Clusteringmodelle zu bewerten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: b370c6ef5be311ed9c8df2737fa1b01144d61011
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027720"
---
# <a name="assign-data-to-clusters"></a>Assign Data to Clusters

In diesem Artikel wird beschrieben, wie Sie das Modul [Assign Data to Clusters](assign-data-to-clusters.md) (Zuweisung von Daten zu Clustern) auf der grafischen Benutzeroberfläche verwenden können, um Vorhersagen mit einem Clusteringmodell zu erstellen, das mit dem k-Means-Algorithmus für das Clustering trainiert wurde.

Das Modul gibt ein Dataset zurück, das die wahrscheinlichen Zuweisungen für jeden neuen Datenpunkt enthält. 


## <a name="how-to-use-assign-data-to-clusters"></a>Verwenden von „Assign Data to Clusters“
  
1.  Navigieren Sie auf der grafischen Benutzeroberfläche zu einem zuvor trainierten Clusteringmodell. Sie können ein Clusteringmodell mit einer dieser beiden Methoden erstellen und trainieren:  
  
    - Konfigurieren Sie den k-Means-Algorithmus mithilfe des Moduls [K-Means Clustering](k-means-clustering.md), und trainieren Sie anschließend das Modell mit einem Dataset und dem Modul [Train Clustering Model](train-clustering-model.md) (Trainieren des Clusteringmodells).  
  
  
    Sie können auch ein bestehendes trainiertes Clusteringmodell aus der Gruppe **Saved Models** (Gespeicherte Modelle) Ihrem Arbeitsbereich hinzufügen.

2. Fügen Sie das trainierte Modell an den linken Eingabeport von [Assign Data to Clusters](assign-data-to-clusters.md) an.  

3. Fügen Sie ein neues Dataset als Eingabe an. In diesem Dataset sind Bezeichnungen optional. Im Allgemeinen ist Clustering eine unüberwachte Lernmethode, weshalb nicht erwartet wird, dass Sie Kategorien im Voraus kennen.

    Die Eingabespalten müssen jedoch mit den Spalten übereinstimmen, die beim Training des Clusteringmodells verwendet wurden, da sonst ein Fehler auftritt.

    > [!TIP]
    > Um die Anzahl der Spalten zu reduzieren, die von Clustervorhersagen ausgegeben werden, verwenden Sie [Select Columns in Dataset](select-columns-in-dataset.md) (Spalten im Dataset auswählen), und wählen Sie eine Teilmenge der Spalten aus. 
    
4. Lassen Sie die Option **Check for Append or Uncheck for Result Only** (Für Anfügen aktivieren oder für reine Ergebnisse deaktivieren) aktiviert, wenn die Ergebnisse das gesamte Eingabedataset sowie eine Spalte mit den Ergebnissen (Clusterzuweisungen) enthalten sollen.
  
    Wenn Sie diese Option deaktivieren, erhalten Sie bloß die Ergebnisse. Dies kann nützlich sein, wenn Sie Vorhersagen als Teil eines Webdiensts erstellen.
  
5.  Führen Sie das Experiment aus.  
  
### <a name="results"></a>Ergebnisse

 
+  Um die Werte im Dataset anzuzeigen, klicken Sie mit der rechten Maustaste auf das Modul. Wählen Sie **Result datasets** (Ergebnisdatasets) aus, und klicken Sie auf **Visualize** (Visualisieren).

