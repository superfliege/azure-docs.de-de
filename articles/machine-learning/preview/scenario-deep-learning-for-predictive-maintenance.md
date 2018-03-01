---
title: "Deep Learning für Predictive Maintenance in der Praxis| Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie das Tutorial zu Deep Learning für Predictive Maintenance mit Azure Machine Learning Workbench replizieren können."
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 7d4fe98b5c45767fb06391218e80789fc0c96a3b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Szenario: Deep Learning für Predictive Maintenance in der Praxis

Deep Learning ist einer der beliebtesten Trends im Bereich Machine Learning mit Anwendungen für viele Bereiche wie beispielsweise:
- selbstfahrende Kraftfahrzeuge und Robotik
- Sprach- und Bilderkennung
- Finanzprognosen.

Auch bekannt als Deep Neural Networks (DNN) sind diese Methoden von den einzelnen Neuronen innerhalb des Gehirns (biologische neuronale Netze) inspiriert.

Die Auswirkungen von ungeplanten Ausfällen von Ausrüstung können für jedes Unternehmen schädlich sein. Es ist entscheidend, für den ausfallsicheren Betrieb der Feldausrüstung zu sorgen, um die Auslastung und Leistung zu erhöhen, indem teure, ungeplante Ausfallzeiten minimiert werden. Eine frühzeitige Identifizierung von Problemen ermöglicht die Bereitstellung eingeschränkter Wartungsressourcen auf kosteneffektive Weise und eine Verbesserung der Qualitäts- und Lieferkettenprozesse. 

Eine Strategie für die vorbeugende Wartung (Predictive Maintenance, PM) verwendet maschinelle Lernmethoden, um den Zustand von Geräten zu ermitteln und präventive Wartungsarbeiten durchzuführen, um eine negative Computerleistung zu vermeiden. In PM werden im Laufe der Zeit erfasste Daten zur Überwachung des Computerstatus analysiert, um Muster zu finden, die zum Vorhersagen von Ausfällen genutzt werden können. [Long Short Term Memory (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)-Netzwerke sind für diese Einstellung attraktiv, da sie für das Lernen aus Datensequenzen vorgesehen sind.

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Im Folgenden finden Sie den Link zum öffentlichen GitHub-Repository für Problemberichte und Beiträge: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

In diesem Tutorial werden simulierte Betriebsereignisse (Normalbetrieb bis Ausfall) eines Flugzeugtriebwerks verwendet, um den Modellierungsprozess für Predictive Maintenance zu veranschaulichen. Das Szenario ist unter [Predictive Maintenance](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) beschrieben.

Die wichtigste Annahme in dieser Einstellung ist, dass das Triebwerk während seiner Lebensdauer zunehmend verschleißt ist. Der Verschleiß ist in den Triebwerk-Sensormessungen erkennbar. PM versucht, die Beziehung zwischen den Änderungen in diesen Sensorwerten und den zurückliegenden Ausfällen zu modellieren. Das Modell kann dann basierend auf dem aktuellen Zustand von Sensormessungen vorhersagen, wann ein Triebwerk in der Zukunft ausfallen wird.

In diesem Szenario wird ein LSTM-Netzwerk erstellt, um die verbleibende Nutzungsdauer von Flugzeugtriebwerken anhand der bisherigen Sensorwerte vorherzusagen. Unter Verwendung von [Keras](https://keras.io/) mit dem Deep Learning-Framework [Tensorflow](https://www.tensorflow.org/) als Computemodul trainiert das Szenario das LSTM mit einen Satz von Triebwerken und testet das Netzwerk mit einem Satz unbekannter Triebwerke.

## <a name="prerequisites"></a>Voraussetzungen
- Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
- Azure Machine Learning Workbench mit einem erstellten Arbeitsbereich
- Für Modelloperationalisierung: Azure Machine Learning-Operationalisierung mit einer lokal eingerichteten Bereitstellungsumgebung und einem [Azure Machine Learning-Modellverwaltung-Konto](model-management-overview.md)

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie mit diesem Beispiel als Vorlage ein neues Projekt:

1. Öffnen Sie Machine Learning Workbench.
2. Wählen Sie auf der Seite **Projekte** das Pluszeichen (**+**) aus, und wählen Sie dann **Neues Projekt** aus.
3. Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4. Geben Sie im Suchfeld **Projektvorlagen suchen** den Begriff „Predictive Maintenance“ ein, und wählen Sie die Vorlage **Szenario: Deep Learning für Predictive Maintenance** aus.
5. Klicken Sie auf die Schaltfläche **Erstellen**.

## <a name="prepare-the-notebook-server-computation-target"></a>Vorbereiten des Berechnungsziels des Notebook-Servers

Für die Ausführung auf Ihrem lokalen Computer wählen Sie im AML Workbench-Menü `File` entweder `Open Command Prompt` oder `Open PowerShell CLI` aus. Die CLI-Schnittstelle ermöglicht es Ihnen, mit den `az`-Befehlen auf Ihre Azure-Dienste zuzugreifen. Melden Sie sich zunächst mit dem folgenden Befehl bei Ihrem Azure-Konto an:

```
az login
``` 

Dieser Befehl bietet einen Authentifizierungsschlüssel für die Verwendung mit der URL `https:\\aka.ms\devicelogin`. Die Befehlszeilenschnittstelle wartet, bis der Geräteanmeldungsvorgang zurückkehrt und einige Verbindungsinformationen bereitstellt. Wenn Sie über eine lokale [Docker](https://www.docker.com/get-docker)-Installation verfügen, bereiten Sie als nächstes die lokale Compute-Umgebung mit den folgenden Befehlen vor:

```
az ml experiment prepare --target docker --run-configuration docker
```

Hinsichtlich der Arbeitsspeicher- und Speicherplatzanforderungen wird die Ausführung auf einem [virtuellen Data Science-Computer für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) empfohlen. Nachdem die DSVM konfiguriert ist, bereiten Sie die Remote-Docker-Umgebung mit den folgenden zwei Befehlen vor:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Sobald die Verbindung mit dem Remote-Docker-Container hergestellt wurde, bereiten Sie die DSVM-Docker-Compute-Umgebung mit Folgendem vor: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Öffnen Sie unter Verwendung der vorbereiteten Docker-Compute-Umgebung den Jupyter-Notebook-Server entweder auf der AML-Workbench-Registerkarte „Notebooks“, oder führen Sie zum Starten eines browserbasierten Servers Folgendes aus: 
```
az ml notebook start
```

Die Beispielnotebooks werden im Verzeichnis `Code` gespeichert. Die Notebooks werden so eingerichtet, dass sie nacheinander ausgeführt werden, wobei mit dem der ersten Notebook (`Code\1_data_ingestion.ipynb`) begonnen wird. Beim Öffnen der einzelnen Notebooks werden Sie aufgefordert, den Computekernel auszuwählen. Wählen Sie den `[Project_Name]_Template [Connection_Name]`-Kernel aus, damit die Ausführung auf der zuvor konfigurierten DSVM erfolgt.

## <a name="data-description"></a>Datenbeschreibung

In der Vorlage werden drei Datasets als Eingaben verwendet. Dies sind die Dateien **PM_train.txt**, **PM_test.txt** und **PM_truth.txt**. 

-  **Trainingsdaten**: Die Messdaten des Flugzeugtriebwerks für Normalbetrieb bis zum Ausfall. Die Trainingsdaten (PM_train.txt) bestehen aus mehreren multivariaten Zeitreihen, wobei *cycle* (Zyklus) als Zeiteinheit fungiert. Die Daten enthalten 21 Sensormesswerte für jeden Zyklus. 

    - Jede Zeitreihe wurde für ein anderes Triebwerk desselben Typs generiert. Bei jedem Triebwerk wird mit unterschiedlich großem Anfangsverschleiß und unterschiedlicher Fertigungsabweichung begonnen. Diese Informationen sind dem Benutzer unbekannt. 

    - In diesen simulierten Daten wird für das Triebwerk davon ausgegangen, dass es am Anfang jeder Zeitreihe im Normalbetrieb läuft. Zu irgendeinem Zeitpunkt in der Reihe der Betriebszyklen fängt der Leistungsabfall für das Triebwerk an. Die Leistungsminderung schreitet voran und wird ständig größer. 

    - Wenn ein vordefinierter Schwellenwert erreicht ist, wird das Triebwerk als unsicher für den weiteren Betrieb eingestuft. Der letzte Zyklus jeder Zeitreihe ist der Zeitpunkt des Triebwerkausfalls.

-   **Testdaten**: Die Betriebsdaten des Flugzeugtriebwerks, ohne aufgezeichnete Ausfallereignisse. Die Testdaten (PM_test.txt) haben dasselbe Datenschema wie die Trainingsdaten. Der einzige Unterschied besteht darin, dass die Daten nicht angegeben, wann der Ausfall auftritt (der letzte Zeitraum stellt *nicht* den Ausfallpunkt dar). Es ist nicht bekannt, wie viele weitere Zyklen dieses Triebwerk verwendet werden kann, bevor es ausfällt.

- **Truth-Daten**: Die Informationen von tatsächlich verbleibenden Zyklen für jedes Triebwerk in den Testdaten. Die Truth-Basisdaten geben die Anzahl von verbleibenden Betriebszyklen für die Triebwerke in den Testdaten an.

## <a name="scenario-structure"></a>Szenariostruktur

Der Szenario-Workflow ist in drei Schritte unterteilt, die jeweils in einem Jupyter-Notebook ausgeführt werden. Jedes Notebook erzeugt Datenartefakte, die für die Verwendung in den folgenden Notebooks lokal beibehalten werden: 

### <a name="task-1-data-ingestion-and-preparation"></a>Aufgabe 1: Datenerfassung und -vorbereitung

Das Jupyter-Notebook zur Datenerfassung in `Code/1_data_ingestion_and_preparation.ipnyb` lädt die drei Eingabedatasets im DataFrame-Format von Pandas. Danach bereitet es die Daten für die Modellierung vor und führt einige vorläufige Datenvisualisierungen aus. Die Datasets werden zur Verwendung im Notebook für die Modellerstellung lokal im Computekontext gespeichert.

### <a name="task-2-model-building-and-evaluation"></a>Aufgabe 2: Modellerstellung und -auswertung

Das Jupyter-Notebook für die Modellerstellung `Code/2_model_building_and_evaluation.ipnyb` liest die Trainings- und Testdatasets vom Datenträger und erstellt ein LSTM-Netzwerk aus dem Trainingsdataset. Die Modellleistung wird für den Testsatz gemessen. Das resultierende Modell wird serialisiert und im lokalen Computekontext gespeichert, damit es in der Operationalisierungsaufgabe verwendet werden kann.

### <a name="task-3-operationalization"></a>Aufgabe 3: Operationalisierung

Das Jupyter-Notebook für Operationalisierung in `Code/3_operationalization.ipnyb` verwendet das gespeicherte Modell, um Funktionen und ein Schema für das Aufrufen des Modells in einem in Azure gehosteten Webdienst zu erstellen. Das Notebook testet die Funktionen und komprimiert dann die Objekte in der `LSTM_o16n.zip`-Datei, die zur Bereitstellung in Ihren Azure-Speichercontainer geladen wird.

Die `LSTM_o16n.zip`-Bereitstellungsdatei enthält die folgenden Artefakte:

- `webservices_conda.yaml` definiert die erforderlichen Python-Pakete zum Ausführen des LSTM-Modells auf dem Bereitstellungsziel.  
- `service_schema.json` definiert das LSTM-Modell erwartete Datenschema.     
- `lstmscore.py` definiert die vom Bereitstellungsziel zum Bewerten neuer Daten ausgeführten Funktionen.    
- `modellstm.json` definiert die LSTM-Architektur. Die Funktionen vom Typ „lstmscore.py“ lesen die Architektur und Gewichtungen, um das Modell zu initialisieren.
- `modellstm.h5` definiert die Modellgewichtungen.
- `test_service.py`: Ein Testskript, das den Bereitstellungsendpunkt mit Testdatensätzen aufruft. 
- `PM_test_files.pkl`: Das `test_service.py`-Skript liest historische Triebwerkdaten aus der `PM_test_files.pkl`-Datei und sendet dem Webdienst genug Zyklen, damit das LSTM eine Wahrscheinlichkeit für einen Triebwerkausfall zurückgeben kann.

Das Notebook testet die Funktionen, indem es die Modelldefinition verwendet, bevor es die Operationalisierungsobjekte für die Bereitstellung packt. Anweisungen zum Einrichten und Testen des Webdiensts befinden sich am Ende des `Code/3_operationalization.ipnyb`-Notebooks.

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial wird ein einfaches Szenario mit Sensorwerten zum Treffen von Vorhersagen verwendet. Anspruchsvollere Predictive Maintenance-Szenarien wie z.B. das [Predictive Maintenance Modelling Guide R Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) können mehrere Datenquellen wie historische Wartungsdatensätze, Fehlerprotokolle und Maschineneigenschaften nutzen. Zusätzliche Datenquellen erfordern möglicherweise unterschiedliche Behandlungen, damit sie mit Deep Learning verwendet werden können.


## <a name="references"></a>Referenzen

Weitere Beispiele für Predictive Maintenance-Anwendungsfälle sind für eine Vielzahl von Plattformen verfügbar:

* [Predictive Maintenance Solution Template (Lösungsvorlage für die vorbeugende Wartung)](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Predictive Maintenance Modeling Guide (Modellhandbuch für die vorbeugende Wartung)](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predictive Maintenance Modeling Guide using SQL R Services (Modellhandbuch für die vorbeugende Wartung mit SQL R Services)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Predictive Maintenance Modeling Guide Python Notebook (Modellhandbuch für die vorbeugende Wartung mit Python Notebook)](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Predictive Maintenance using PySpark (Vorbeugende Wartung mit PySpark)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

* [Szenario: Predictive Maintenance in der Praxis](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Nächste Schritte

Es gibt viele weitere Beispielszenarien innerhalb von Azure Machine Learning Workbench, die zusätzliche Funktionen des Produkts veranschaulichen. 