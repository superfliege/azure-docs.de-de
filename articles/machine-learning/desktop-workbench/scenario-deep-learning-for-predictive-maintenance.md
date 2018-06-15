---
title: Deep Learning für Predictive Maintenance in der Praxis| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Tutorial zu Deep Learning für Predictive Maintenance mit Azure Machine Learning Workbench replizieren können.
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: mvc
ms.devlang: ''
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 26b061c6bd6ff3ec9d1edbb7d99053d17db9b773
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832600"
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Deep Learning für Predictive Maintenance in der Praxis

Deep Learning ist einer der beliebtesten Trends im Bereich Machine Learning, der Anwendungen für viele Bereiche beinhaltet, u.a.:
- Selbstfahrende Kraftfahrzeuge und Robotik
- Sprach- und Bilderkennung
- Finanzprognosen

Diese Methoden sind von den einzelnen Neuronen innerhalb des Gehirns (biologische neuronale Netze) inspiriert und werden auch als „Deep Neural Networks“ (DNN) bezeichnet.

Die Auswirkungen von ungeplanten Ausfällen von Ausrüstung können für jedes Unternehmen schädlich sein. Es ist entscheidend, für den ausfallsicheren Betrieb der Feldausrüstung zu sorgen, um die Auslastung und Leistung zu erhöhen, indem teure, ungeplante Ausfallzeiten minimiert werden. Eine frühzeitige Identifizierung von Problemen ermöglicht die Bereitstellung eingeschränkter Wartungsressourcen auf kosteneffektive Weise und eine Verbesserung der Qualitäts- und Lieferkettenprozesse. 

Eine Strategie für die vorbeugende Wartung (Predictive Maintenance, PM) verwendet maschinelle Lernmethoden, um den Zustand von Geräten zu ermitteln und präventive Wartungsarbeiten durchzuführen, um eine negative Computerleistung zu vermeiden. In PM werden im Laufe der Zeit Daten zur Überwachung des Computerstatus erfasst und dann analysiert, um Muster zum Vorhersagen von Ausfällen zu finden. [Long Short Term Memory (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)-Netzwerke sind für diese Einstellung attraktiv, da sie für das Lernen aus Datensequenzen konzipiert sind.

### <a name="cortana-intelligence-gallery-github-repository"></a>GitHub-Repository des Cortana Intelligence-Katalogs

Die Azure AI Gallery für das PM-Tutorial ist ein öffentliches GitHub-Repository ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)), in dem Sie Probleme melden und Beiträge beisteuern können.


## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

In diesem Tutorial werden simulierte Betriebsereignisse (Normalbetrieb bis Ausfall) eines Flugzeugtriebwerks verwendet, um den Modellierungsprozess für Predictive Maintenance zu veranschaulichen. Das Szenario ist unter [Predictive Maintenance](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) beschrieben.

Die wichtigste Annahme in dieser Einstellung ist, dass das Triebwerk während seiner Lebensdauer zunehmend verschleißt ist. Der Verschleiß ist in den Triebwerk-Sensormessungen erkennbar. PM versucht, die Beziehung zwischen den Änderungen in diesen Sensorwerten und den zurückliegenden Ausfällen zu modellieren. Das Modell kann dann basierend auf dem aktuellen Zustand von Sensormessungen vorhersagen, wann ein Triebwerk in der Zukunft ausfallen wird.

In diesem Szenario wird ein LSTM-Netzwerk erstellt, um die verbleibende Nutzungsdauer von Flugzeugtriebwerken anhand der bisherigen Sensorwerte vorherzusagen. Das Szenario verwendet die Bibliothek [Keras](https://keras.io/) mit dem Deep Learning-Framework [Tensorflow](https://www.tensorflow.org/) als Computemodul. Das Szenario trainiert das LSTM mit einen Satz von Triebwerken und testet das Netzwerk mit einem Satz unbekannter Triebwerke.

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
5. Klicken Sie auf **Erstellen**.

## <a name="prepare-the-notebook-server-computation-target"></a>Vorbereiten des Berechnungsziels des Notebook-Servers

Wählen Sie für die Ausführung auf Ihrem lokalen Computer im Machine Learning Workbench-Menü **Datei** entweder **Eingabeaufforderung öffnen** oder **PowerShell-CLI öffnen** aus. Die CLI-Schnittstelle ermöglicht Ihnen den Zugriff auf Ihre Azure-Dienste mit den `az`-Befehlen. Melden Sie sich zunächst mit dem folgenden Befehl bei Ihrem Azure-Konto an:

```
az login
``` 

Dieser Befehl bietet einen Authentifizierungsschlüssel für die Verwendung mit der URL https:\\aka.ms\devicelogin. Die Befehlszeilenschnittstelle wartet, bis der Geräteanmeldungsvorgang zurückkehrt und einige Verbindungsinformationen bereitstellt. Wenn Sie über eine lokale [Docker](https://www.docker.com/get-docker)-Installation verfügen, bereiten Sie als nächstes die lokale Compute-Umgebung mit dem folgenden Befehl vor:

```
az ml experiment prepare --target docker --run-configuration docker
```

Hinsichtlich der Arbeitsspeicher- und Speicherplatzanforderungen wird die Ausführung auf einem [virtuellen Data Science-Computer (Data Science Virtual Machine, DSVM) für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) empfohlen. Nachdem die DSVM konfiguriert ist, bereiten Sie die Remote-Docker-Umgebung mit den folgenden zwei Befehlen vor:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Nachdem Sie die Verbindung mit dem Remote-Docker-Container hergestellt haben, bereiten Sie die DSVM-Docker-Compute-Umgebung mit dem folgenden Befehl vor: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Öffnen Sie unter Verwendung der vorbereiteten Docker-Compute-Umgebung den Jupyter-Notebook-Server auf der Machine Learning Workbench-Registerkarte **Notebooks**, oder starten Sie mit dem folgenden Befehl einen browserbasierten Server: 

```
az ml notebook start
```

Die Beispielnotebooks werden im Verzeichnis „Code“ gespeichert. Die Notebooks sind so eingerichtet, dass sie nacheinander ausgeführt werden, wobei mit dem ersten Notebook (Code\1_data_ingestion.ipynb) begonnen wird. Beim Öffnen der einzelnen Notebooks werden Sie aufgefordert, den Computekernel auszuwählen. Wählen Sie den Kernel „[Projektname]_Vorlage [Verbindungsname]“ für die Ausführung auf dem zuvor konfigurierten DSVM aus.

## <a name="data-description"></a>Datenbeschreibung

In der Vorlage werden drei Datasets als Eingaben verwendet. Dies sind die Dateien „PM_train.txt“, „PM_test.txt“ und „PM_truth.txt“. 

- **Trainingsdaten**: Die Messdaten des Flugzeugtriebwerks für Normalbetrieb bis zum Ausfall. Die Trainingsdaten (PM_train.txt) bestehen aus mehreren multivariaten Zeitreihen, wobei *cycle* (Zyklus) als Zeiteinheit fungiert. Die Daten enthalten 21 Sensormesswerte für jeden Zyklus. 

    - Jede Zeitreihe wurde für ein anderes Triebwerk desselben Typs generiert. Bei jedem Triebwerk wird mit unterschiedlich großem Anfangsverschleiß und unterschiedlicher Fertigungsabweichung begonnen. Diese Informationen sind dem Benutzer unbekannt. 

    - In diesen simulierten Daten wird für das Triebwerk davon ausgegangen, dass es am Anfang jeder Zeitreihe im Normalbetrieb läuft. Zu irgendeinem Zeitpunkt in der Reihe der Betriebszyklen fängt der Leistungsabfall für das Triebwerk an. Die Leistungsminderung schreitet voran und wird ständig größer. 

    - Wenn ein vordefinierter Schwellenwert erreicht ist, wird das Triebwerk als unsicher für den weiteren Betrieb eingestuft. Der letzte Zyklus jeder Zeitreihe ist der Zeitpunkt des Triebwerkausfalls.

- **Testdaten**: Die Betriebsdaten des Flugzeugtriebwerks, ohne aufgezeichnete Ausfallereignisse. Die Testdaten (PM_test.txt) haben dasselbe Datenschema wie die Trainingsdaten. Der einzige Unterschied besteht darin, dass die Daten nicht angegeben, wann der Ausfall auftritt (der letzte Zeitraum stellt *nicht* den Ausfallpunkt dar). Es ist nicht bekannt, wie viele weitere Zyklen dieses Triebwerk verwendet werden kann, bevor es ausfällt.

- **Truth-Daten**: Die Informationen von tatsächlich verbleibenden Zyklen für jedes Triebwerk in den Testdaten. Die Truth-Basisdaten geben die Anzahl von verbleibenden Betriebszyklen für die Triebwerke in den Testdaten an.

## <a name="scenario-structure"></a>Szenariostruktur

Der Szenario-Workflow ist in drei Schritte unterteilt, die jeweils in einem Jupyter-Notebook ausgeführt werden. Jedes Notebook erzeugt Datenartefakte, die für die Verwendung in den Notebooks lokal beibehalten werden.

### <a name="task-1-data-ingestion-and-preparation"></a>Aufgabe 1: Datenerfassung und -vorbereitung

Das Jupyter-Notebook zur Datenerfassung in „Code/1_data_ingestion_and_preparation.ipnyb“ lädt die drei Eingabedatasets im DataFrame-Format von Pandas. Danach bereitet das Notebook die Daten für die Modellierung vor und führt einige vorläufige Datenvisualisierungen aus. Die Datasets werden zur Verwendung im Jupyter-Notebook für die Modellerstellung lokal im Computekontext gespeichert.

### <a name="task-2-model-building-and-evaluation"></a>Aufgabe 2: Modellerstellung und -auswertung

Das Jupyter-Notebook für die Modellerstellung in „Code/2_model_building_and_evaluation.ipnyb“ liest die Trainings- und Testdatasets vom Datenträger und erstellt ein LSTM-Netzwerk für das Trainingsdataset. Die Modellleistung wird für das Testdataset gemessen. Das resultierende Modell wird serialisiert und im lokalen Computekontext gespeichert, damit es in der Operationalisierungsaufgabe verwendet werden kann.

### <a name="task-3-operationalization"></a>Aufgabe 3: Operationalisierung

Das Jupyter-Notebook für Operationalisierung in „Code/3_operationalization.ipnyb“ verwendet das gespeicherte Modell, um Funktionen und ein Schema für das Aufrufen des Modells in einem in Azure gehosteten Webdienst zu erstellen. Das Notebook testet die Funktionen und komprimiert dann die Objekte in der Datei „LSTM_o16n.zip“. Die Datei wird in Ihren Azure-Speichercontainer für die Bereitstellung geladen.

Die Bereitstellungsdatei „LSTM_o16n.zip“ enthält die folgenden Artefakte:

- **webservices_conda.yaml**: Definiert die erforderlichen Python-Pakete zum Ausführen des LSTM-Modells auf dem Bereitstellungsziel.  
- **service_schema.json**: Definiert das vom LSTM-Modell erwartete Datenschema.   
- **lstmscore.py**: Definiert die vom Bereitstellungsziel zum Bewerten neuer Daten ausgeführten Funktionen.    
- **modellstm.json**: Definiert die LSTM-Architektur. Die Funktionen vom Typ „lstmscore.py“ lesen die Architektur und Gewichtungen, um das Modell zu initialisieren.
- **modellstm.h5**: Definiert die Modellgewichtungen.
- **test_service.py**: Ein Testskript, das den Bereitstellungsendpunkt mit Testdatensätzen aufruft. 
- **PM_test_files.pkl**: Das Skript „test_service.py“ liest historische Triebwerkdaten aus der Datei „PM_test_files.pkl“ und sendet dem Webdienst genug Zyklen, damit das LSTM eine Wahrscheinlichkeit für einen Triebwerkausfall zurückgeben kann.

Das Notebook testet die Funktionen, indem es die Modelldefinition verwendet, bevor es die Operationalisierungsobjekte für die Bereitstellung packt. Anweisungen zum Einrichten und Testen des Webdiensts befinden sich am Ende des Notebooks „Code/3_operationalization.ipnyb“.

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial wird ein einfaches Szenario mit Sensorwerten zum Treffen von Vorhersagen bereitgestellt. Anspruchsvollere Predictive Maintenance-Szenarien wie z.B. das [Predictive Maintenance Modelling Guide R Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) können mehrere Datenquellen wie historische Wartungsdatensätze, Fehlerprotokolle und Maschineneigenschaften nutzen. Zusätzliche Datenquellen erfordern möglicherweise unterschiedliche Behandlungen, damit sie mit Deep Learning verwendet werden können.


## <a name="references"></a>Referenzen

Die folgenden Referenzen bieten Beispiele für andere Predictive Maintenance-Anwendungsfälle für verschiedene Plattformen:

* [Predictive Maintenance Solution Template (Lösungsvorlage für die vorbeugende Wartung)](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Predictive Maintenance Modeling Guide (Modellhandbuch für die vorbeugende Wartung)](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predictive Maintenance Modeling Guide using SQL R Services (Modellhandbuch für die vorbeugende Wartung mit SQL R Services)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Predictive Maintenance Modeling Guide Python Notebook (Modellhandbuch für die vorbeugende Wartung mit Python Notebook)](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Predictive Maintenance using PySpark (Vorbeugende Wartung mit PySpark)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Predictive Maintenance in der Praxis – Szenarien](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance)

## <a name="next-steps"></a>Nächste Schritte

Mehr Beispielszenarien, die weitere Funktionen des Produkts veranschaulichen, sind in Azure Machine Learning Workbench verfügbar. 
