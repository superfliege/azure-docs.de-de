---
title: Bildklassifizierung mit CNTK in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie ein benutzerdefiniertes Bildklassifizierungsmodell mit Azure ML Workbench trainieren, evaluieren und bereitstellen.
services: machine-learning
documentationcenter: 
author: PatrickBue
ms.author: pabuehle
ms.reviewer: mawah, marhamil, mldocs
ms.service: machine-learning
ms.topic: article
ms.date: 10/17/2017
ms.openlocfilehash: eefede6196bedf208d9b14cee63632922223a6d6
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="image-classification-using-azure-machine-learning-workbench"></a>Bildklassifizierung per Azure Machine Learning Workbench

Ansätze für die Bildklassifizierung können verwendet werden, um eine große Zahl von Problemen zu lösen, die für das maschinelle Sehen auftreten können.
Hierzu gehört auch das Erstellen von Modellen, mit denen beispielsweise folgende Fragen beantwortet werden: *Ist ein OBJEKT im Bild vorhanden?*, wobei das OBJEKT beispielsweise *Hund*, *Auto* oder *Boot* lauten kann. Ein Beispiel für eine komplexere Frage ist: *Welche Augenkrankheit-Schweregradklasse zeigt der Netzhautscan des Patienten?*.

In diesem Tutorial wird die Lösung dieser Art von Problemen beschrieben. Es wird veranschaulicht, wie Sie Ihr eigenes Bildklassifizierungsmodell trainieren, evaluieren und bereitstellen, indem Sie das [Microsoft Cognitive Toolkit (CNTK) ](https://docs.microsoft.com/cognitive-toolkit/) für Deep Learning nutzen.
Es werden Beispielbilder bereitgestellt, aber Leser können auch ein eigenes Dataset verwenden und eigene benutzerdefinierte Modelle trainieren.

Für Lösungen für das maschinelle Sehen war bisher Expertenwissen erforderlich, um so genannte *Features*, mit denen gewünschte Informationen in Bildern hervorgehoben wurden, manuell zu identifizieren und zu implementieren.
Dieser manuelle Ansatz hat sich im Jahr 2012 mit dem berühmten Deep Learning-Dokument [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) [1] geändert, und zurzeit werden Deep Neural Networks (DNN) genutzt, um diese Features automatisch zu ermitteln.
DNNs ermöglichen eine starke Verbesserung in diesem Bereich – nicht nur in Bezug auf die Bildklassifizierung, sondern auch für andere Probleme des maschinellen Sehens, z.B. Objekterkennung und Bilder mit starker Ähnlichkeit.


## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys
[https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK](https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK)

## <a name="overview"></a>Übersicht

Dieses Tutorial ist in drei Teile unterteilt:

- In Teil 1 wird beschrieben, wie Sie ein System für die Bildklassifizierung trainieren, evaluieren und bereitstellen, indem Sie ein vorab trainiertes DNN als „Featurizer“ verwenden und eine SVM anhand der Ausgabe trainieren.
- In Teil 2 wird veranschaulicht, wie Sie die Genauigkeit verbessern, indem Sie beispielsweise das DNN optimieren, anstatt es als festen Featurizer einzusetzen.
- In Teil 3 wird beschrieben, wie Sie Ihr eigenes Dataset anstelle der bereitgestellten Beispielbilder verwenden und bei Bedarf Ihr eigenes Dataset erzeugen, indem Sie Bilder aus dem Netz nutzen.

Es ist zwar keine Erfahrung mit Machine Learning und CNTK erforderlich, aber sie ist für das Verständnis der zugrunde liegenden Prinzipien hilfreich. Genauigkeitswerte, Trainingszeiten usw., die im Tutorial angegeben sind, dienen nur zu Referenzzwecken, und die tatsächlichen Werte beim Ausführen des Codes unterscheiden sich mit hoher Wahrscheinlichkeit von diesen Angaben.


## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen müssen zum Ausführen dieses Beispiels erfüllt sein:

1. Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
2. [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) gemäß [Schnellstart-Installationshandbuch](./quickstart-installation.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs  
3. Ein Windows-Computer Das Windows-Betriebssystem ist erforderlich, da Workbench nur Windows und macOS unterstützt, während für das Cognitive Toolkit von Microsoft (als Deep Learning-Bibliothek verwendet) nur Windows und Linux unterstützt werden.
4. Eine dedizierte GPU ist nicht erforderlich, um das SVM-Training in Teil 1 durchzuführen, wird aber für die in Teil 2 beschriebene Optimierung der DNN benötigt. Wenn Sie keine leistungsstarke GPU haben, das Trainieren für mehrere GPUs durchführen möchten oder keinen Windows-Computer nutzen, können Sie erwägen, die Deep Learning Virtual Machine von Azure mit dem Windows-Betriebssystem zu verwenden. Eine Anleitung für die Bereitstellung mit nur einem Klick finden Sie [hier](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning). Stellen Sie nach der Bereitstellung per Remotedesktopverbindung eine Verbindung mit der VM her, installieren Sie Workbench darauf, und führen Sie den Code von der VM lokal aus.
5. Es müssen verschiedene Python-Bibliotheken installiert werden, z.B. OpenCV. Klicken Sie in der Workbench im Menü *Datei* auf *Eingabeaufforderung öffnen*, und führen Sie die folgenden Befehle aus, um diese Abhängigkeiten zu installieren:  
    - `pip install https://cntk.ai/PythonWheel/GPU/cntk-2.0-cp35-cp35m-win_amd64.whl`  
    - `pip install opencv_python-3.3.0-cp35-cp35m-win_amd64.whl` nach dem Herunterladen des OpenCV-Wheelelements von „http://www.lfd.uci.edu/~gohlke/pythonlibs/“ (Dateiname und Version können sich ändern)
    - `conda install matplotlib numpy pillow`
    - `conda install -c conda-forge bqplot`

### <a name="troubleshooting--known-bugs"></a>Problembehandlung/Bekannte Fehler
- Für Teil 2 wird eine GPU benötigt. Andernfalls wird beim Versuch, das DNN zu optimieren, der Fehler „Batch normalization training on CPU is not yet implemented“ (Batchnormalisierungstraining für CPU ist noch nicht implementiert) ausgelöst.
- Beim DNN-Training können Fehler vom Typ „Nicht genügend Arbeitsspeicher“ verhindert werden, indem die Minibatch-Größe reduziert wird (Variable `cntk_mb_size` in `PARAMETERS.py`).
- Der Code wurde mit CNTK 2.0 und 2.1 getestet und sollte auch auf neueren Versionen ohne (oder nur mit wenigen) Änderungen ausgeführt werden können.
- Zum Zeitpunkt der Erstellung dieses Artikels gab es in Azure Machine Learning Workbench Probleme mit dem Anzeigen von Notebooks, die größer als 5 MB sind. Notebooks dieser Größe können entstehen, wenn sie mit der gesamten angezeigten Zellenausgabe gespeichert werden. Öffnen Sie bei Auftreten dieses Fehlers in Workbench über das Menü „Datei“ die Eingabeaufforderung, führen Sie den Befehl `jupyter notebook` aus, öffnen Sie das Notebook, löschen Sie die gesamte Ausgabe, und speichern Sie das Notebook. Nach dem Ausführen dieser Schritte wird das Notebook in Azure Machine Learning Workbench wieder richtig geöffnet.


## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Gehen Sie wie folgt vor, um ein neues Projekt mit diesem Beispiel als Vorlage zu erstellen:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf das Pluszeichen (**+**), und wählen Sie die Option **Neues Projekt**.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „Bildklassifizierung“ ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**.

Wenn Sie diese Schritte ausführen, wird die unten angegebene Projektstruktur erstellt. Das Projektverzeichnis ist auf weniger als 25 MB beschränkt, da von Azure Machine Learning Workbench nach jeder Ausführung eine Kopie dieses Ordners erstellt wird (zur Aktivierung des Ausführungsverlaufs). Daher werden alle Bilddateien und temporären Dateien über das Verzeichnis *~/Desktop/imgClassificationUsingCntk_data* gespeichert (in diesem Dokument als *DATA_DIR* bezeichnet).

  Ordner| Beschreibung
  ---|---
  aml_config/|                           Verzeichnis mit Azure Machine Learning Workbench-Konfigurationsdateien
  libraries/|                              Verzeichnis mit allen Python- und Jupyter-Hilfsprogrammfunktionen
  notebooks/|                              Verzeichnis mit allen Notebooks
  resources/|                              Verzeichnis mit allen Ressourcen (z.B. URL von Modebildern)
  scripts/|                              Verzeichnis mit allen Skripts
  PARAMETERS.py|                       Python-Skript zum Angeben aller Parameter
  readme.md|                           Diese Infodatei


## <a name="data-description"></a>Datenbeschreibung

In diesem Tutorial wird als auszuführendes Beispiel ein Dataset mit Daten zu Oberbekleidungsstoffmustern verwendet, das bis zu 428 Bilder enthält. Jedes Bild ist mit einer Anmerkung zu drei unterschiedlichen Stoffen versehen: „dotted“ (Gepunktet), „striped“ (Gestreift) oder „leopard“ (Leopardenmuster). Wir haben die Anzahl von Bildern bewusst gering gehalten, damit dieses Tutorial schnell ausgeführt werden kann. Der Code wurde aber sorgfältig getestet und funktioniert auch mit Zehntausenden von Bildern oder auch noch mehr Bildern. Alle Bilder wurden mit der Bing-Bildersuche erfasst und manuell mit Anmerkungen versehen. Dies ist in [Teil 3](#using-a-custom-dataset) beschrieben. Die Bild-URLs mit ihren jeweiligen Attributen sind in der Datei */resources/fashionTextureUrls.tsv* aufgeführt.

Mit dem Skript `0_downloadData.py` werden alle Bilder in das Verzeichnis *DATA_DIR/images/fashionTexture/* heruntergeladen. Einige der 428 URLs sind vermutlich fehlerhaft. Dies ist kein Problem und bedeutet nur, dass etwas weniger Bilder für das Trainieren und Testen zur Verfügung stehen.

Die folgende Abbildung enthält Beispiele für die Attribute „Gepunktet“ (die beiden linken Spalten), „Gestreift“ (die beiden mittleren Spalten) und „Leopardenmuster“ (die beiden rechten Spalten). Die Anmerkungen wurden je nach Oberbekleidungsartikel hinzugefügt.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/examples_dotted.jpg"  alt="alt text" height="200">
<img src="media/scenario-image-classification-using-cntk/examples_striped.jpg" alt="alt text" height="200">
<img src="media/scenario-image-classification-using-cntk/examples_leopard.jpg" alt="alt text" height="200">
</p>


## <a name="part-1---model-training-and-evaluation"></a>Teil 1: Trainieren und Evaluieren des Modells

Im ersten Teil dieses Tutorials trainieren wir ein System, in dem ein vorab trainiertes Deep Neural Network verwendet (aber nicht geändert) wird. Dieses vorab trainierte DNN wird als Featurizer verwendet, und es wird eine lineare SVM trainiert, um das Attribut (Gepunktet, Gestreift oder Leopardenmuster) vorherzusagen.

Wir haben diesen Ansatz jetzt ausführlich und Schritt für Schritt beschrieben und zeigen Ihnen, welche Skripts ausgeführt werden müssen. Wir empfehlen Ihnen, nach jedem Schritt zu untersuchen, welche Dateien geschrieben wurden und wohin der Schreibvorgang erfolgt ist.

Alle wichtigen Parameter werden an einem Ort angegeben (zusammen mit einer kurzen Erklärung), und zwar in der Datei `PARAMETERS.py`.




### <a name="step-1-data-preparation"></a>Schritt 1: Datenvorbereitung
`Script: 1_prepareData.py. Notebook: showImages.ipynb`

Das Notebook `showImages.ipynb` kann zum Visualisieren der Bilder und je nach Bedarf zum Korrigieren der Anmerkungen verwendet werden. Öffnen Sie das Notebook zum Ausführen in Azure Machine Learning Workbench, klicken Sie auf „Start Notebook Server“ (Notebook-Server starten), wenn die Option angezeigt wird, und führen Sie dann alle Zellen im Notebook aus. Wenn Sie einen Fehler mit dem Hinweis erhalten, dass das Notebook zu groß zum Anzeigen ist, helfen Ihnen die Informationen im Abschnitt zur Problembehandlung in diesem Dokument weiter.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showImages.jpg" alt="alt text" width="700"/>
</p>

Führen Sie jetzt das Skript mit dem Namen `1_prepareData.py` aus, mit dem alle Bilder entweder dem Trainingssatz oder dem Testsatz zugewiesen werden. Diese Zuweisung schließt sich gegenseitig aus: kein Trainingsbild wird auch für das Testen verwendet (und umgekehrt). Standardmäßig werden jeweils 75% der Bilder einer Attributklasse per zufälliger Auswahl für das Trainieren und die verbleibenden 25% für das Testen zugewiesen. Alle vom Skript generierten Daten werden im Ordner *DATA_DIR/proc/fashionTexture/* gespeichert.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_1_white.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-2-refining-the-deep-neural-network"></a>Schritt 2: Optimieren des Deep Neural Network
`Script: 2_refineDNN.py`

Wie in Teil 1 dieses Tutorials beschrieben, bleibt das vorab trainierte DNN unverändert (wird also nicht optimiert). Das Skript mit dem Namen `2_refineDNN.py` wird in Teil 1 trotzdem ausgeführt, da darüber ein vorab trainiertes [ResNet](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf)-Modell [2] geladen und modifiziert wird, um beispielsweise eine höhere Eingangsbildauflösung zuzulassen. Dieser Schritt wird schnell ausgeführt (wenige Sekunden), und es ist keine GPU erforderlich.

In Teil 2 des Tutorials bewirkt eine Modifizierung der Datei „PARAMETERS.py“, dass mit dem Skript `2_refineDNN.py` auch das vorab trainierte DNN optimiert wird. Standardmäßig werden während der Optimierung 45 Trainingsepochen ausgeführt.

In beiden Fällen wird das endgültige Modell dann in die Datei *DATA_DIR/proc/fashionTexture/cntk_fixed.model* geschrieben.

### <a name="step-3-evaluate-dnn-for-all-images"></a>Schritt 3: Evaluieren des DNN für alle Bilder
`Script: 3_runDNN.py`

Wir können jetzt das (ggf. optimierte) DNN aus dem letzten Schritt verwenden, um unsere Bilder mit Features auszustatten. Bei einem Bild als Eingabe für das DNN ist die Ausgabe der 512-Gleitkommazahl-Vektor aus der vorletzten Schicht des Modells. Dieser Vektor ist in Bezug auf die Dimension deutlich kleiner als das Bild selbst. Trotzdem sollte er alle Informationen des Bilds, die für die Erkennung des Bildattributs (Gepunktet, Gestreift oder Leopardenmuster) relevant sind, enthalten (und auch hervorheben).

Alle DNN-Bilddarstellungen werden in der Datei *DATA_DIR/proc/fashionTexture/cntkFiles/features.pickle* gespeichert.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_4_white.jpg" alt="alt text" width="700"/>
</p>


### <a name="step-4-support-vector-machine-training"></a>Schritt 4: Support Vector Machine-Training
`Script: 4_trainSVM.py`

Die im letzten Schritt berechneten 512-Gleitkommazahl-Darstellungen werden jetzt zum Trainieren einer SVM-Klassifizierung verwendet: Bei einem Bild als Eingabe gibt die SVM eine Bewertung für jedes vorhandene Attribut aus. In unserem Beispieldataset ist dies eine Bewertung für „Gestreift“, „Gepunktet“ und „Leopardenmuster“.

Mit dem Skript `4_trainSVM.py` werden die Trainingsbilder geladen, es wird eine SVM für unterschiedliche Werte des Regularisierungsparameters C (Slack) trainiert, und die SVM mit der höchsten Genauigkeit wird beibehalten. Die Klassifizierungsgenauigkeit wird in der Konsole und in Workbench ausgegeben. Für die bereitgestellten Stoffmusterdaten sollten diese Werte in der Nähe von 100% bzw. 88% liegen. Abschließend wird die trainierte SVM in die Datei *DATA_DIR/proc/fashionTexture/cntkFiles/svm.np* geschrieben.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/vienna_svm_log_zoom.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-5-evaluation-and-visualization"></a>Schritt 5: Evaluierung und Visualisierung
`Script: 5_evaluate.py. Notebook: showResults.ipynb`

Die Genauigkeit der trainierten Bildklassifizierung kann mit dem Skript `5_evaluate.py` gemessen werden. Das Skript bewertet alle Testbilder anhand der trainierten SVM-Klassifizierung, weist jedes Bild dem Attribut mit der höchsten Bewertung zu und vergleicht die vorhergesagten Attribute mit den tatsächlichen Anmerkungen.

Die Ausgabe des Skripts `5_evaluate.py` ist unten angegeben. Die Klassifizierungsgenauigkeit jeder einzelnen Klasse wird genauso berechnet wie die Genauigkeit für den vollständigen Testsatz („Gesamtgenauigkeit“) und der Durchschnitt der einzelnen Genauigkeiten („Durchschnittliche Gesamtgenauigkeit der Klassen“). 100% entspricht der bestmöglichen Genauigkeit und 0% der geringsten Genauigkeit. Reines Raten ergibt für die Anzahl von Attributen im Durchschnitt eine mittlere Genauigkeit für die Klassen von 1. In unserem Fall beträgt diese Genauigkeit 33,33%. Diese Ergebnisse verbessern sich deutlich, wenn eine höhere Eingangsauflösung verwendet wird, z.B. `rf_inputResoluton = 1000`. Allerdings führt dies zu längeren DNN-Berechnungsdauern.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_6_white.jpg" alt="alt text" width="700"/>
</p>

Zusätzlich zur Genauigkeit wird die ROC-Kurve mit der jeweiligen „Area Under Curve“, also dem Bereich unter der Kurve, ausgegeben (links). Außerdem wird die Konfusionsmatrix angezeigt (rechts):

<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat.jpg" alt="alt text" width="700"/>
</p>

Abschließend wird das Notebook `showResults.py` bereitgestellt, um durch die Testbilder zu scrollen und die entsprechenden Klassifizierungsbewertungen zu visualisieren:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showResults.jpg" alt="alt text" width="700"/>
</p>





### <a name="step-6-deployment"></a>Schritt 6: Bereitstellung
`Scripts: 6_callWebservice.py, deploymain.py. Notebook: deploy.ipynb`

Das trainierte System kann jetzt als REST-API veröffentlicht werden. Die Bereitstellung wird im Notebook `deploy.ipynb` und basierend auf der Funktionalität in Azure Machine Learning Workbench beschrieben. Lesen Sie auch den hervorragenden Abschnitt zur Bereitstellung im [Iris-Tutorial](https://docs.microsoft.com/azure/machine-learning/preview/tutorial-classifying-iris-part-3).

Nach der Bereitstellung kann der Webdienst mit dem Skript `6_callWebservice.py` aufgerufen werden. Beachten Sie, dass die IP-Adresse des Webdiensts (entweder lokal oder in der Cloud) zuerst im Skript festgelegt werden muss. Im Notebook `deploy.ipynb` wird beschrieben, wie Sie diese IP-Adresse finden.








## <a name="part-2---accuracy-improvements"></a>Teil 2: Verbesserungen bei der Genauigkeit

In Teil 1 wurde gezeigt, wie Sie ein Bild klassifizieren, indem Sie eine lineare Support Vector Machine in der 512-Gleitkommazahl-Ausgabe eines Deep Neural Network trainieren. Dieses DNN wurde mit Millionen von Bildern vorab trainiert, und die vorletzte Schicht wurde als Featurevektor zurückgegeben. Dieser Ansatz ist schnell, da das DNN unverändert verwendet wird, führt aber trotzdem häufig zu guten Ergebnissen.

Hier sind nun mehrere Möglichkeiten angegeben, um die Genauigkeit des Modells aus Teil 1 zu verbessern. Der wichtigste Punkt ist, dass wir das DNN optimieren, anstatt es unverändert zu lassen.

### <a name="dnn-refinement"></a>DNN-Optimierung

Anstatt per SVM kann die Klassifizierung auch direkt im neuronalen Netzwerk durchgeführt werden. Hierzu wird dem vorab trainierten DNN eine neue letzte Schicht hinzugefügt, für die die 512 Gleitkommazahlen der vorletzten Schicht als Eingabe verwendet werden. Der Vorteil einer Durchführung der Klassifizierung im DNN besteht darin, dass nun das gesamte Netzwerk per Rückpropagierung neu trainiert werden kann. Dieser Ansatz führt häufig zu viel besseren Klassifizierungsgenauigkeiten als bei Verwendung des unveränderten vorab trainierten DNN. Der Nachteil ist, dass das Trainieren deutlich länger dauert (auch mit GPU).

Das Trainieren des neuronalen Netzwerks anstelle einer SVM wird erreicht, indem die Variable `classifier` in `PARAMETERS.py` von `svm` in `dnn` geändert wird. Wie in Teil 1 beschrieben, müssen alle Skripts – mit Ausnahme der Datenvorbereitung (Schritt 1) und des SVM-Trainings (Schritt 3) – erneut ausgeführt werden. Für die DNN-Optimierung ist eine GPU erforderlich. Wenn keine GPU gefunden wurde oder die GPU gesperrt ist (z.B. durch eine vorherige CNTK-Ausführung), löst das Skript `2_refineDNN.py` einen Fehler aus. Beim DNN-Training kann für einige GPUs der Fehler „Nicht genügend Arbeitsspeicher“ ausgelöst werden. Sie können diesen Fehler vermeiden, indem Sie die Minibatch-Größe reduzieren (Variable `cntk_mb_size` in `PARAMETERS.py`).

Nachdem das Training abgeschlossen wurde, wird das optimierte Modell unter *DATA_DIR/proc/fashionTexture/cntk_refined.model* gespeichert. Außerdem wird dargestellt, wie sich die Fehler für die Trainings- und Testklassifizierung während des Trainings ändern. Beachten Sie, dass der Fehler im Trainingssatz deutlich kleiner als im Testsatz ist. Dieses als Überanpassung bezeichnete Verhalten kann beispielsweise reduziert werden, indem ein höherer Wert für die Abbruchrate (`rf_dropoutRate`) verwendet wird.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_3_plot.png" alt="alt text" height="300"/>
</p>

Wie in der folgenden Abbildung dargestellt, beträgt die Genauigkeit bei Verwendung der DNN-Optimierung für das bereitgestellte Dataset 92,35% gegenüber dem vorherigen Prozentwert von 88,92% (Teil 1). Besonders die Bilder vom Typ „Gepunktet“ wurden erheblich verbessert, und die ROC Area Under Curve hat nach der Optimierung den Wert 0,98 gegenüber dem vorherigen Wert von 0,94. Da wir ein kleines Dataset verwenden, unterscheiden sich die tatsächlichen Genauigkeiten für die Ausführung des Codes. Diese Diskrepanz ergibt sich aufgrund von Stochastikeffekten, z.B. der zufälligen Aufteilung der Bilder in Trainings- und Testsätze.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat_dnn.jpg" alt="alt text" width="700"/>
</p>

### <a name="run-history-tracking"></a>Nachverfolgen des Ausführungsverlaufs

In Azure Machine Learning Workbench wird der Verlauf jeder Ausführung in Azure gespeichert, um auch dann einen Vergleich von zwei oder mehr Ausführungen zu ermöglichen, wenn diese mehrere Wochen auseinander liegen. Dies wird im [Iris-Tutorial](https://docs.microsoft.com/azure/machine-learning/preview/tutorial-classifying-iris-part-2) ausführlich beschrieben. Dies ist auch in den folgenden Screenshots dargestellt, in denen zwei Ausführungen des Skripts `5_evaluate.py` verglichen werden, indem entweder die DNN-Optimierung (`classifier = "dnn"` (Ausführungsnummer 148)) oder SVM-Training (`classifier = "svm"` (Ausführungsnummer 150)) verwendet wird.

Im ersten Screenshot führt die DNN-Optimierung zu besseren Genauigkeiten als das SVM-Training für alle Klassen. Im zweiten Screenshot sind alle Metriken dargestellt, die nachverfolgt werden, z.B. die Art der Klassifizierung. Diese Nachverfolgung erfolgt im Skript `5_evaluate.py`, indem die Azure Machine Learning Workbench-Protokollierung aufgerufen wird. Außerdem speichert das Skript die ROC-Kurve und die Konfusionsmatrix im Ordner *outputs*. Der Ordner *outputs* ist ein besonderer Ordner, da sein Inhalt auch von der Workbench-Verlaufsfunktion nachverfolgt wird. Daher kann unabhängig davon, ob lokale Kopien überschrieben wurden, jederzeit auf die Ausgabedateien zugegriffen werden.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison1.jpg" alt="alt text" width="700"/>  
</p>

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison2b.jpg" alt="alt text" width="700"/>
</p>


### <a name="parameter-tuning"></a>Parameteroptimierung
Wie für die meisten anderen Machine Learning-Projekte auch, gilt Folgendes: Zum Erzielen guter Ergebnisse für ein neues Dataset ist es erforderlich, eine sorgfältige Parameteroptimierung und eine Abwägung unterschiedlicher Entwurfsentscheidungen durchzuführen. Als Hilfestellung bei dieser Aufgabe werden alle wichtigen Parameter an einem Ort angegeben (zusammen mit einer kurzen Erklärung), und zwar in der Datei `PARAMETERS.py`.

Hier sind einige Verbesserungen aufgeführt, die am meisten Erfolg versprechen:

- Datenqualität: Stellen Sie sicher, dass die Trainings- und Testsätze eine hohe Qualität aufweisen. Dies bedeutet Folgendes: Die Bilder sollten mit den richtigen Anmerkungen versehen sein, mehrdeutige Bilder sollten entfernt werden (z.B. Kleidungsstücke, die sowohl Streifen als auch Punkte aufweisen), und die Attribute sollten sich gegenseitig ausschließen (sodass ein Bild genau einem Attribut zugeordnet werden kann).
- Wenn das betreffende Objekt im Bild klein ist, funktionieren Ansätze, die auf der Bildklassifizierung basieren, meist nicht so gut. Erwägen Sie in diesen Fällen die Verwendung eines Ansatzes mit Verwendung der Objekterkennung, der in [diesem Tutorial](https://github.com/Azure/ObjectDetectionUsingCntk) beschrieben ist.
- DNN-Optimierung: Der wahrscheinlich wichtigste Parameter ist die Lerngeschwindigkeit `rf_lrPerMb`. Wenn die Genauigkeit des Trainingssatzes (erste Abbildung in Teil 2) nicht im Bereich 0 - 5% liegt, ist der Grund häufig eine falsche Lerngeschwindigkeit. Die anderen Parameter, die mit `rf_` beginnen, sind weniger wichtig. Normalerweise sollte der Trainingsfehler exponentiell verringert werden und nach dem Training in der Nähe von 0% liegen.
- Eingabeauflösung: Die Standardbildauflösung beträgt 224x224 Pixel. Wenn eine höhere Bildauflösung (Parameter: `rf_inputResoluton`) von beispielsweise 448x448 oder 896x896 Pixel verwendet wird, führt dies häufig zu einer erheblichen Verbesserung der Genauigkeit und gleichzeitig zu einer Verlangsamung der DNN-Optimierung. **Die Verwendung einer höheren Bildauflösung ist mit Vorteilen verbunden und führt fast immer zu einer höheren Genauigkeit**.
- DNN-Überanpassung: Vermeiden Sie eine große Lücke zwischen der Trainings- und Testgenauigkeit während der DNN-Optimierung (erste Abbildung in Teil 2). Diese Lücke kann reduziert werden, indem Abbruchraten (`rf_dropoutRate`) von 0,5 oder höher verwendet werden und die Regularisierungsgewichtung (`rf_l2RegWeight`) erhöht wird. Die Verwendung einer hohen Abbruchrate kann besonders hilfreich sein, wenn die DNN-Eingabebildauflösung hoch ist.
- Versuchen Sie, tiefer gehende DNNs zu verwenden, indem Sie `rf_pretrainedModelFilename` von `ResNet_18.model` entweder in `ResNet_34.model` oder `ResNet_50.model` ändern. Das Resnet-50-Modell ist nicht nur tiefer gehend, sondern die Ausgabe der vorletzten Schicht beträgt auch 2048 Gleitkommazahlen (gegenüber 512 Gleitkommazahlen bei den Modellen ResNet-18 und ResNet-34). Diese Erhöhung kann besonders hilfreich sein, wenn eine SVM-Klassifizierung trainiert wird.

## <a name="part-3---custom-dataset"></a>Teil 3: Benutzerdefiniertes Dataset

In Teil 1 und 2 haben wir ein Bildklassifizierungsmodell trainiert und evaluiert, indem die bereitgestellten Bilder mit Oberbekleidungsstoffen verwendet wurden. Jetzt wird beschrieben, wie Sie stattdessen ein benutzerdefiniertes und vom Benutzer bereitgestelltes Dataset verwenden. Falls kein Dataset dieser Art verfügbar ist, kann es auch mit der Bing-Bildersuche generiert und mit Anmerkungen versehen werden.

### <a name="using-a-custom-dataset"></a>Verwenden eines benutzerdefinierten Datasets

Zuerst sehen wir uns die Ordnerstruktur für die Daten zu den Bekleidungsstoffmustern an. Sie sehen, dass sich alle Bilder für die verschiedenen Attribute unter *DATA_DIR/images/fashionTexture/* in den entsprechenden Unterordnern *dotted*, *leopard und *striped* befinden. Beachten Sie auch, dass der Name des Bildordners auch in der Datei `PARAMETERS.py` vorkommt:
```python
datasetName = "fashionTexture"
```

Zum Verwenden eines benutzerdefinierten Datasets muss lediglich diese Ordnerstruktur reproduziert werden, in der sich alle Bilder je nach Attribut im entsprechenden Unterordner befinden. Anschließend müssen diese Unterordner dann in das neue Verzeichnis *DATA_DIR/images/newDataSetName/* kopiert werden, das vom Benutzer angegeben wird. Die einzige erforderliche Codeänderung ist das Festlegen der Variablen `datasetName` auf *newDataSetName*. Die Skripts 1 bis 5 können dann der Reihenfolge nach ausgeführt werden, und alle Zwischendateien werden in *DATA_DIR/proc/newDataSetName/* geschrieben. Es sind keine weiteren Codeänderungen erforderlich.

Es ist wichtig, dass jedes Bild genau einem Attribut zugewiesen werden kann. Beispielsweise wäre es falsch, Attribute für „Tier“ und „Leopard“ zu verwenden, da ein Bild vom Typ „Leopard“ auch zu „Tier“ gehören würde. Außerdem ist es ratsam, Bilder zu entfernen, die mehrdeutig sind und daher nicht leicht mit Anmerkungen versehen werden können.



### <a name="image-scraping-and-annotation"></a>Bilderfassung und Hinzufügen von Anmerkungen

Das Sammeln einer ausreichend hohen Zahl von mit Anmerkungen versehenen Bildern für Trainings- und Testzwecke kann schwierig sein. Eine Möglichkeit zur Lösung dieses Problems ist die Verwendung von Bildern aus dem Internet. Unten sind beispielsweise die Ergebnisse der Bing-Bildersuche für die Abfrage *t-shirt striped* (T-Shirt gestreift) angegeben. Erwartungsgemäß zeigen die meisten Bilder gestreifte T-Shirts. Die wenigen falschen oder mehrdeutigen Bilder (z.B. Spalte 1, Zeile 1 oder Spalte 3, Zeile 2) können leicht identifiziert und entfernt werden:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/bing_search_striped.jpg" alt="alt text" width="600"/>
</p>

Es sollten mehrere Abfragen verwendet werden, um ein großes und vielfältiges Dataset zu generieren. 7\*3 = 21 Abfragen können beispielsweise automatisch synthetisiert werden, indem alle möglichen Kombinationen von Bekleidungsstücken {Bluse, Hoodie, Pullover, Sweater, Hemd, T-Shirt, Weste} und Attributen {Gestreift, Gepunktet, Leopardenmuster} verwendet werden. Wenn dann die ersten 50 Bilder pro Abfrage heruntergeladen werden, ergeben sich maximal 21 * 50 = 1.050 Bilder.

Anstatt Bilder manuell aus der Bing-Bildersuche herunterzuladen, ist es viel einfacher, stattdessen die [Bing-Bildersuche-API der Cognitive Services](https://www.microsoft.com/cognitive-services/bing-image-search-api) zu verwenden. Hiermit wird für eine Abfragezeichenfolge ein Satz mit Bild-URLs zurückgegeben.

Einige der heruntergeladenen Bilder sind exakte oder nahezu identische Duplikate (beispielsweise besteht der Unterschied nur in der Bildauflösung oder in JPG-Artefakten). Diese Duplikate sollten entfernt werden, damit die Trainings- und Testaufteilungen nicht die gleichen Bilder enthalten. Das Entfernen von doppelten Bildern ist möglich, indem ein hashbasierter Ansatz verwendet wird, der aus zwei Schritten besteht: (i) Zuerst wird die Hashzeichenfolge für alle Bilder berechnet, (ii) und im zweiten Durchlauf für die Bilder werden nur die Bilder beibehalten, deren Hashzeichenfolge noch nicht vorgekommen ist. Alle anderen Bilder werden verworfen. Der Ansatz `dhash` in der Python-Bibliothek `imagehash`, der in diesem [Blog](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html) beschrieben ist, funktioniert gut, wenn der Parameter `hash_size` auf 16 festgelegt ist. Es ist zulässig, fälschlicherweise einige Bilder zu entfernen, bei denen es sich nicht um Duplikate handelt, solange der Großteil der richtigen Duplikate entfernt wird.





## <a name="conclusion"></a>Zusammenfassung

Einige der wichtigsten Highlights dieses Beispiels sind:
- Code zum Trainieren, Evaluieren und Bereitstellen von Bildklassifizierungsmodellen
- Bereitgestellte Demobilder, aber einfache Anpassung (Änderung von nur einer Zeile) zur Verwendung eines eigenen Bilddatasets
- Implementierung von modernsten Features für Experten zum Trainieren von Modellen mit hoher Genauigkeit basierend auf Lerntransfer
- Interaktive Modellentwicklung mit Azure Machine Learning Workbench und Jupyter Notebook


## <a name="references"></a>Referenzen

[1] Alex Krizhevsky, Ilya Sutskever, and Geoffrey E. Hinton, [_ImageNet Classification with Deep Convolutional Neural Networks_](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) (ImageNet-Klassifizierung mit Deep Convolutional Neural Networks) NIPS 2012.  
[2] Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun, [_Deep Residual Learning for Image Recognition_](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) (Deep Residual Learning für die Bilderkennung) CVPR 2016.
