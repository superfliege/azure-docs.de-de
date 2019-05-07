---
title: 'K-Means Clustering: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul „K-Means Clustering“ in Azure Machine Learning Service verwenden, um Clusteringmodelle zu trainieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29aeb460581655190b7c3f4256647059f4642d3e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027832"
---
# <a name="k-means-clustering"></a>K-Means Clustering

In diesem Artikel wird beschrieben, wie Sie das Modul **K-Means Clustering** in Azure Machine Learning Studio verwenden können, um ein untrainiertes K-Means-Clusteringmodell zu erstellen. 
 
K-Means ist einer der einfachsten und bekanntesten *unüberwachten* Lernalgorithmen, der für eine Vielzahl von maschinellen Lernaufgaben verwendet werden kann, wie z.B. [Erkennung anormaler Daten](https://msdn.microsoft.com/magazine/jj891054.aspx), Clustering von Textdokumenten und Analyse eines Datasets vor der Verwendung anderer Klassifizierungs- oder Regressionsmethoden. Um ein Clusteringmodell zu erstellen, fügen Sie dieses Modul Ihrem Experiment hinzu. Stellen Sie dann eine Verbindung mit einem Dataset her. Legen Sie anschließend Parameter fest, wie z.B. die erwartete Anzahl von Clustern, die bei der Erstellung der Cluster verwendete Entfernungsmetrik u.v.m. 
  
Nachdem Sie die Hyperparameter des Moduls konfiguriert haben, verbinden Sie das untrainierte Modell mit dem [Modell zum Trainieren von Clustern](train-clustering-model.md). Da der k-Means-Algorithmus eine unüberwachte Lernmethode ist, ist eine Bezeichnungsspalte optional. 

+ Wenn Ihre Daten eine Bezeichnung enthalten, können Sie die Bezeichnungswerte verwenden, um die Auswahl der Cluster zu steuern und das Modell zu optimieren. 

+ Wenn Ihre Daten keine Bezeichnung haben, erstellt der Algorithmus Cluster, die mögliche Kategorien darstellen, ausschließlich basierend auf den Daten.  
  

  
##  <a name="understanding-k-means-clustering"></a>Grundlegendes zu K-Means Clustering
 
Im Allgemeinen werden beim Clustering iterative Techniken befolgt, um Fälle in einem Datensatz in Clustern zu gruppieren, die ähnliche Merkmale enthalten. Diese Gruppierungen eignen sich für die Erkundung von Daten, die Erkennung von Anomalien in den Daten und möglicherweise für Vorhersagen. Clusteringmodelle können Ihnen auch helfen, Beziehungen in einem Dataset zu erkennen, die sich möglicherweise durch Durchsuchen oder einfaches Beobachten nicht logisch zu erkennen geben. Aus diesen Gründen erfolgt das Clustering oft in der Frühphase maschineller Lernaufgaben, um die Daten zu untersuchen und unerwartete Korrelationen zu entdecken.  
  
 Wenn Sie ein Clusteringmodell mit der k-Means-Methode konfigurieren, müssen Sie einen Zielwert *k* angeben, der die Anzahl der im Modell gewünschten *Schwerpunkte* angibt. Der Schwerpunkt ist ein Punkt, der für jeden Cluster repräsentativ ist. Der k-Means-Algorithmus ordnet jeden eingehenden Datenpunkt einem der Cluster zu, indem er die Summe der Quadrate innerhalb des Clusters minimiert. 
 
Bei der Verarbeitung der Trainingsdaten beginnt der k-Means-Algorithmus mit einer ersten Reihe zufällig gewählter Schwerpunkte, die als Ausgangspunkt jedes Clusters dienen, und wendet den Lloyd-Algorithmus an, um die Positionen der Schwerpunkte iterativ zu verbessern. Der k-Means-Algorithmus beendet die Bildung und Verbesserung von Clustern, sobald er eine oder mehrere dieser Bedingungen erfüllt:  
  
-   Die Schwerpunkte stabilisieren sich, was bedeutet, dass sich die Clusterzuweisungen für einzelne Punkte nicht mehr ändern und der Algorithmus zu einer Lösung konvergiert ist.  
  
-   Der Algorithmus hat die angegebene Anzahl von Iterationen ausgeführt.  
  
 Nach der Trainingsphase verwenden Sie das Modul [Assign Data to Clusters](assign-data-to-clusters.md) (Zuweisen von Daten zu Clustern), um neue Fälle einem der Cluster zuzuordnen, die vom k-Means-Algorithmus gefunden wurden. Die Clusterzuweisung erfolgt durch Berechnung der Entfernung zwischen dem neuen Fall und dem Schwerpunkt jedes Clusters. Jeder neue Fall wird dem Cluster mit dem nächstgelegenen Schwerpunkt zugeordnet.  

## <a name="how-to-configure-k-means-clustering"></a>Konfigurieren des K-Means-Clusterings
  
1.  Fügen Sie Ihrem Experiment das Modul **K-Means Clustering** hinzu.  
  
2.  Geben Sie an, wie das Modell trainiert werden soll, indem Sie die Option **Create trainer mode** (Trainermodus erstellen) aktivieren.  
  
    -   **Single Parameter** (Einzelner Parameter): Wenn Sie die genauen Parameter kennen, die Sie im Clusteringmodell verwenden möchten, können Sie eine bestimmte Menge von Werten als Argumente angeben.  
  
   
  
3.  Für **Number of Centroids** (Anzahl der Schwerpunkte) geben Sie die Anzahl von Clustern ein, mit der der Algorithmus beginnen soll.  
  
     Es gibt keine Garantie dafür, dass das Modell genau diese Anzahl von Clustern produziert. Der Algorithmus beginnt mit dieser Anzahl von Datenpunkten und iteriert, um die optimale Konfiguration zu finden.  
  
     
  
4.  Die Eigenschaften unter **Initialization** (Initialisierung) werden verwendet, um den Algorithmus anzugeben, der zum Definieren der anfänglichen Clusterkonfiguration verwendet wird.  
  
    -   **First N** (Erste N): Eine bestimmte Anfangsanzahl von Datenpunkten wird im Dataset ausgewählt und als Anfangsmittelwert verwendet.  
  
         Dies wird auch als *Forgy-Methode* bezeichnet.  
  
    -   **Random** (Zufällig): Der Algorithmus platziert einen Datenpunkt nach dem Zufallsprinzip in einem Cluster und berechnet dann den Anfangsmittelwert als Schwerpunkt der zufällig zugewiesenen Punkte des Clusters.  
  
         Wird auch *Random Partition*-Methode bezeichnet.  
  
    -   **k-Means++**: Dies ist die Standardmethode für die Initialisierung von Clustern.  
  
         Der **k-Means ++** Algorithmus wurde 2007 von David Arthur und Sergei Vassilvitskii vorgeschlagen, um schlechtes Clustering durch den standardmäßigen k-Means Algorithmus zu vermeiden. **k-Means ++** verbessert den standardmäßigen k-Means-Algorithmus durch Verwenden einer anderen Methode zur Auswahl der anfänglichen Clusterzentren.  
  
    
5.  Geben Sie für **Random number seed** (zufällig gewählter Startwert) optional einen Wert ein, der als Startwert für die Clusterinitialisierung verwendet wird. Dieser Wert kann erhebliche Auswirkungen auf die Clusterauswahl haben.  
  
    
  
6.  Wählen Sie für **Metric** (Metrik) die Funktion, mit der Sie die Entfernung zwischen Clustervektoren oder zwischen neuen Datenpunkten und dem zufällig gewählten Schwerpunkt messen möchten. Azure Machine Learning unterstützt die folgenden Metriken für die Clusterentfernung:  
  
    -   **Euclidean** (Euklidisch): Die euklidische Entfernung wird beim K-Means-Clustering häufig als ein Maß der Clusterstreuung verwendet. Diese Metrik wird bevorzugt, da sie die mittlere Entfernung zwischen Punkten und den Schwerpunkten minimiert.
  

  
7.  Geben Sie für **Iterations** (Iterationen) die Anzahl der Iterationen des Algorithmus über die Trainingsdaten ein, bevor Sie die Auswahl der Schwerpunkte abschließen.  
  
     Sie können diesen Parameter so anpassen, dass die Genauigkeit im Verhältnis zur Trainingszeit ausgeglichen wird.  
  
8.  Wählen Sie für **Assign label mode** (Bezeichnungsmodus zuweisen) eine Option, die angibt, wie eine Bezeichnungsspalte, falls im Datensatz vorhanden, behandelt werden soll.  
  
     Da K-Means-Clustering eine unüberwachte maschinelle Lernmethode ist, sind Bezeichnungen optional. Wenn Ihr Dataset jedoch bereits eine Bezeichnungsspalte aufweist, können Sie diese Werte verwenden, um die Auswahl der Cluster zu steuern, oder Sie können festlegen, dass die Werte ignoriert werden sollen.  
  
    -   **Ignore label column** (Bezeichnungsspalte ignorieren): Die Werte in der Bezeichnungsspalte werden ignoriert und nicht für die Erstellung des Modells verwendet.
  
    -   **Fill missing values** (Fehlende Werte auffüllen): Die Werte der Bezeichnungsspalte werden als Features verwendet, um die Erstellung der Cluster zu unterstützen. Wenn in Zeilen eine Bezeichnung fehlt, wird der Wert durch Verwendung anderer Features impliziert.  
  
    -   **Overwrite from closest to center** (Vom nächstgelegenen zum Zentrum überschreiben): Die Werte der Bezeichnungsspalte werden durch vorhergesagte Bezeichnungswerte ersetzt, wobei die Bezeichnung des Punkts verwendet wird, der dem aktuellen Schwerpunkt am nächsten liegt.  

8.  Wählen Sie die Option **Normalize features** (Features normalisieren), wenn Sie Features vor dem Training normalisieren möchten.
  
     Wenn Sie die Normalisierung anwenden, werden die Datenpunkte vor dem Training durch den MinMaxNormalizer zu `[0,1]` normalisiert.

10. Trainieren Sie das Modell.  
  
    -   Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, fügen Sie ein mit Tags versehenes Dataset hinzu, und trainieren Sie das Modell mit dem Modul [Train Clustering Model](train-clustering-model.md) (Trainieren des Clusteringsmodells).  
  

### <a name="results"></a>Ergebnisse

Nach Abschluss der Konfiguration und des Trainings des Modells haben Sie ein Modell, mit dem Sie Ergebnisse generieren können. Es gibt jedoch mehrere Möglichkeiten, das Modell zu trainieren, die Ergebnisse anzuzeigen und sie zu nutzen: 

#### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Erfassen einer Momentaufnahme des Modells in Ihrem Arbeitsbereich

+ Bei Verwendung des Moduls [Train Clustering Model](train-clustering-model.md) (Trainieren des Clusteringmodells)
    1. Klicken Sie mit der rechten Maustaste auf das Modul [Train Clustering Model](train-clustering-model.md).
    2. Wählen Sie **Trained model** (Trainiertes Modell) aus, und klicken Sie dann auf **Save as Trained Model** (Als trainiertes Modell speichern).


Das gespeicherte Modell stellt die Trainingsdaten zum Zeitpunkt der Speicherung des Modells dar. Wenn Sie die im Experiment verwendeten Trainingsdaten später aktualisieren, wird das gespeicherte Modell nicht aktualisiert. 



#### <a name="see-the-clustering-result-dataset"></a>Anzeigen des Ergebnisdatasets des Clusterings 

+ Bei Verwendung des Moduls [Train Clustering Model](train-clustering-model.md)
    1. Klicken Sie mit der rechten Maustaste auf das Modul [Train Clustering Model](train-clustering-model.md).
    2. Wählen Sie **Results dataset** (Ergebnisdataset) aus, und klicken Sie dann auf **Visualize** (Visualisieren).


### <a name="tips-for-generating-the-best-clustering-model"></a>Tipps zum Generieren des besten Clusteringmodells  

Es ist bekannt, dass der beim Clustering verwendete **Seeding-**-Prozess das Modell erheblich beeinflussen kann. Seeding bedeutet die anfängliche Platzierung von Punkten in potenziellen Schwerpunkten.
 
Wenn das Dataset beispielsweise viele Ausreißer enthält und ein Ausreißer für das Seeding der Cluster gewählt wird, passen keine anderen Datenpunkte gut zu diesem Cluster, sodass der Cluster ein Singleton sein könnte: also ein Cluster mit nur einem Punkt.  
  
Es gibt verschiedene Möglichkeiten, dieses Problem zu vermeiden:  
  
-   Ändern Sie die Anzahl der Schwerpunkte, und probieren Sie mehrere Startwerte aus.  
  
-   Erstellen Sie mehrere Modelle, wobei Sie die Metrik variieren oder mehr Iterationen vornehmen.  
  
  
Im Allgemeinen ist es bei Clusteringmodellen möglich, dass eine bestimmte Konfiguration zu einer lokal optimierten Menge von Clustern führt. Mit anderen Worten, die vom Modell zurückgegebene Menge von Clustern passt nur zu den aktuellen Datenpunkten und ist nicht für andere Daten generalisierbar. Wenn Sie eine andere Anfangskonfiguration verwendet haben, kann die k-Means-Methode eine andere, vielleicht sogar bessere Konfiguration finden. 
