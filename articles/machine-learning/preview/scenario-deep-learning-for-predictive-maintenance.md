---
title: "Deep Learning für Predictive Maintenance in der Praxis| Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie das Tutorial zu Deep Learning für Predictive Maintenance mit Azure Machine Learning Workbench replizieren können."
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: a55209256c29fa62cc2da72f9653fbc7fc0e7c54
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Deep Learning für Predictive Maintenance in der Praxis

Deep Learning ist einer der beliebtesten Trends im Bereich Machine Learning. Deep Learning wird in vielen Bereichen und Anwendungen eingesetzt, darunter fahrerlose Autos, Sprach- und Bilderkennung, Robotik und Finanzen. Deep Learning besteht aus einer Reihe von Algorithmen, die von der Form des Gehirns (biologische neuronale Netze) und Machine Learning inspiriert sind. Kognitionswissenschaftler bezeichnen Deep Learning in der Regel als künstliche neuronale Netzwerke.

Predictive Maintenance (vorausbestimmte Wartung) ist ebenfalls populär. Für Predictive Maintenance sind viele verschiedene Techniken entwickelt worden, um den Zustand von Ausrüstung bestimmen und voraussagen zu können, wann eine Wartung durchgeführt werden sollte. Einige gebräuchliche Verwendungen von Predictive Maintenance sind Fehlerprognosen, Fehlerdiagnosen (Ursachenanalyse), Fehlererkennungen, die Klassifizierung von Fehlertypen und die Empfehlung von Risikominderungs- oder Wartungsmaßnahmen nach einem Fehler.

In Predictive Maintenance-Szenarien werden Daten im Verlauf der Zeit gesammelt, um den Zustand von Ausrüstung zu überwachen. Ziel ist es, Muster zu erkennen, die ein Vorhersagen und letztlich ein Verhindern von Fehlern ermöglichen. Verwenden von [Long Short Term Memory (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)-Netzwerken ist eine Deep Learning-Methode, die insbesondere in Predictive Maintenance attraktiv ist. LSTM-Netzwerke sind gut darin, aus Sequenzen zu lernen. Zeitreihendaten können dazu verwendet werden, über einen längeren Zeitraum in die Vergangenheit zu schauen, um Fehlermuster zu erkennen.

In diesem Tutorial wird ein LSTM-Netzwerk für das Dataset und das Szenario erstellt, die unter [Predictive Maintenance](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) beschrieben sind. Es wird ein Netzwerk verwendet, um die verbleibende Nutzungsdauer von Flugzeugtiebwerken vorherzusagen. In der Vorlage werden simulierte Flugzeugsensorwerte verwendet, um vorherzusagen, wann ein Flugzeugtriebwerk zukünftig ausfallen wird. Durch Verwenden dieser Vorhersage kann Wartung im Voraus geplant werden, um einen Ausfall zu verhindern.

In diese Tutorial werden die Deep Learning-Bibliothek [Keras](https://keras.io/) und das Microsoft Cognitive Toolkit [CNTK](https://docs.microsoft.com/cognitive-toolkit/Using-CNTK-with-Keras) als Back-End verwendet.

Das öffentliche GitHub-Repository, das die Beispiele für dieses Tutorial enthält, befindet sich unter [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance).

## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

In diesem Tutorial werden simulierte Betriebsereignisse (Normalbetrieb bis Ausfall) eines Flugzeugtriebwerks verwendet, um den Modellierungsprozess für Predictive Maintenance zu veranschaulichen. 

Die implizite Annahme zu den hier beschriebenen Modellierungsdaten ist, dass das Triebwerk ein Muster fortschreitender Leistungsminderung hat. Das Muster schlägt sich in den Sensormessungen des Triebwerks nieder. Durch Auswerten der Sensorwerte des Triebwerks im zeitlichen Verlauf kann der Machine Learning-Algorithmus die Beziehung zwischen den Sensorwerten, den Änderungen der Sensorwerte und zurückliegenden Ausfällen lernen. Diese Beziehung wird verwendet, um zukünftige Ausfälle vorherzusagen. 

Sie sollten sich das Datenformat ansehen und alle drei Schritte der Vorlage vollständig ausführen, bevor Sie die Beispieldaten durch Ihre eigenen Daten ersetzen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
- Azure Machine Learning Workbench mit einem erstellten Arbeitsbereich
- Für Modelloperationalisierung: Azure Machine Learning-Operationalisierung mit einer lokal eingerichteten Bereitstellungsumgebung und einem [Azure Machine Learning-Modellverwaltung-Konto](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie mit diesem Beispiel als Vorlage ein neues Projekt:

1. Öffnen Sie Machine Learning Workbench.
2. Wählen Sie auf der Seite **Projekte** das Pluszeichen (**+**) aus, und wählen Sie dann **Neues Projekt** aus.
3. Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4. Geben Sie in das Suchfeld **Projektvorlagen suchen** den Namen **Predictive Maintenance** ein, und wählen Sie die Vorlage aus.
5. Klicken Sie auf **Erstellen**.

## <a name="prepare-the-notebook-server-computation-target"></a>Vorbereiten des Berechnungsziels des Notebook-Servers

Wählen Sie auf Ihrem lokalen Computer im Machine Learning Workbench-Menü **Datei** entweder **Eingabeaufforderung öffnen** oder **PowerShell öffnen** aus. Führen Sie im Eingabeaufforderungsfenster der von Ihnen ausgewählten Option die folgenden Befehle aus:

`az ml experiment prepare --target docker --run-configuration docker`

Es wird empfohlen, den Notebook-Server auf einem [virtuellen Computer für Data Science (Data Science Virtual Machine, DSVM) für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) auszuführen, der die Größe „Standard_DS4_v2“ hat. Nachdem der virtuelle Computer für Data Science konfiguriert ist, führen Sie die folgenden Befehle aus, um die Docker-Images vorzubereiten:

`az ml computetarget attach remotedocker --name [connection_name] --address [VM_IP_address] --username [VM_username] --password [VM_password]`

`az ml experiment prepare --target [connection_name] --run-configuration [connection_name]`

Mit den vorbereiteten Docker-Images öffnen Sie den Jupyter Notebook-Server. Um den Jupyter Notebook-Server zu öffnen, navigieren Sie zur Machine Learning Workbench-Registerkarte **Notebooks**, oder starten Sie einen browserbasierten Server:`az ml notebook start`.

Notebooks werden in der Jupyter-Umgebung im Verzeichnis „Code“ gespeichert. Führen Sie diese Notebooks nacheinander gemäß Nummerierung aus, wobei Sie mit „Code\1_data_ingestion_and_preparation.ipynb“ beginnen.

Wählen Sie den Kernel aus, der Ihrem „[Projektname]_Template [Verbindungsname]“ entspricht, und wählen Sie dann **Set Kernel** aus.

## <a name="data-description"></a>Datenbeschreibung

In der Vorlage werden drei Datasets als Eingaben verwendet. Dies sind die Dateien „PM_train.txt“, „PM_test.txt“ und „PM_truth.txt“.

-  **Trainingsdaten**: Die Messdaten des Flugzeugtriebwerks für Normalbetrieb bis zum Ausfall. Die Trainingsdaten (PM_train.txt) bestehen aus mehreren multivariaten Zeitreihen, wobei *cycle* (Zyklus) als Zeiteinheit fungiert. Die Daten enthalten 21 Sensormesswerte für jeden Zyklus. 

    Für jede Zeitreihe wird davon ausgegangen, dass sie für ein anderes Triebwerk desselben Typs generiert wurde. Für jedes Triebwerk wird davon ausgegangen, dass mit unterschiedlich großem Anfangsverschleiß und unterschiedlicher Fertigungsabweichung begonnen wird. Diese Informationen sind dem Benutzer unbekannt. 

    In diesen simulierten Daten wird für das Triebwerk davon ausgegangen, dass es am Anfang jeder Zeitreihe im Normalbetrieb läuft. Zu irgendeinem Zeitpunkt in der Reihe der Betriebszyklen fängt der Leistungsabfall für das Triebwerk an. Die Leistungsminderung schreitet voran und wird ständig größer. 

    Wenn ein vordefinierter Schwellenwert erreicht ist, wird das Triebwerk als unsicher für den weiteren Betrieb eingestuft. Der letzte Zyklus in jeder Zeitreihe kann als Ausfallpunkt des entsprechenden Triebwerks betrachtet werden.

-   **Testdaten**: Die Betriebsdaten des Flugzeugtriebwerks, ohne aufgezeichnete Ausfallereignisse. Die Testdaten (PM_test.txt) haben dasselbe Datenschema wie die Trainingsdaten. Der einzige Unterschied besteht darin, dass die Daten nicht angegeben, wann der Ausfall auftritt (der letzte Zeitraum stellt *nicht* den Ausfallpunkt dar). Es ist nicht bekannt, wie viele weitere Zyklen dieses Triebwerk verwendet werden kann, bevor es ausfällt.

- **Truth-Daten**: Die Informationen von tatsächlich verbleibenden Zyklen für jedes Triebwerk in den Testdaten. Die Truth-Basisdaten geben die Anzahl von verbleibenden Betriebszyklen für die Triebwerke in den Testdaten an.

## <a name="scenario-structure"></a>Szenariostruktur

Die Inhalte für das Szenario sind im [GitHub-Repository] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) verfügbar. 

Im Repository sind in einer [readme] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md)-Datei die Prozesse skizziert, angefangen beim Vorbereiten der Daten bis hin zum Erstellen und Operationalisieren des Modells. Die drei Jupyter-Notebooks sind im Ordner [Code] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) im Repository verfügbar. 

Als Nächstes wird der Workflow des Szenarios Schritt für Schritt beschrieben.

### <a name="task-1-data-ingestion-and-preparation"></a>Aufgabe 1: Datenerfassung und -vorbereitung

Das Jupyter-Notebook zur Datenerfassung in „Code/1_data_ingestion_and_preparation.ipnyb“ lädt die drei Eingabedatasets im DataFrame-Format von Pandas. Danach bereitet es die Daten für die Modellierung vor und führt einige vorläufige Datenvisualisierungen aus. Die Daten werden in das PySpark-Format transformiert und in einem Azure Blob Storage-Container in Ihrem Azure-Abonnement gespeichert, damit sie für die nächste Modellierungsaufgabe verwendet werden können.

### <a name="task-2-model-building-and-evaluation"></a>Aufgabe 2: Modellerstellung und -auswertung

Das Jupyter-Notebook zur Modellerstellung in „Code/2_model_building_and_evaluation.ipnyb“ liest die PySpark-Trainings- und -Testdatasets aus dem Blob-Speicher. Anschließend wird ein LSTM-Netzwerk mit den Trainingsdatasets erstellt. Die Modellleistung wird für den Testsatz gemessen. Das resultierende Modell wird serialisiert und im lokalen Computekontext gespeichert, damit es in der Operationalisierungsaufgabe verwendet werden kann.

### <a name="task-3-operationalization"></a>Aufgabe 3: Operationalisierung

Das Jupyter-Notebook für Operationalisierung in „Code/3_operationalization.ipnyb“ verwendet das gespeicherte Modell, um Funktionen und ein Schema zu erstellen, die dazu erforderlich sind, dass das Modell in einem in Azure gehosteten Webdienst aufgerufen werden kann. Das Notebook testet die Funktionen und komprimiert dann die Operationalisierungsobjekte in eine ZIP-Datei.

Die ZIP-Datei enthält diese Dateien:

- **modellstm.json**: Die Schemadefinitionsdatei für die Bereitstellung. 
- **lstmscore.py**: Die Funktionen **init ()** und **run ()**, die der Azure-Webdienst benötigt.
- **lstm.model**: Das Modelldefinitionsverzeichnis.

Das Notebook testet die Funktionen, indem es die Modelldefinition verwendet, bevor es die Operationalisierungsobjekte für die Bereitstellung packt. Anweisungen zur Bereitstellung sind am Ende des Notebooks zu finden.


## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial wird ein einfaches Szenario verwendet, in dem nur eine Datenquelle (Sensorwerte) verwendet wird, um Vorhersagen zu treffen. In anspruchsvolleren Predictive Maintenance-Szenarien, z. B. das [Predictive Maintenance Modelling Guide R Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), können viele Datenquellen verwendet werden. Andere Datenquellen können zurückliegende Wartungsdatensätze, Fehlerprotokolle sowie Maschinen- und Bedienermerkmale enthalten. Zusätzliche Datenquellen erfordern möglicherweise unterschiedliche Behandlungsarten, damit sie in Deep Learning-Netzwerken verwendet werden können. Außerdem ist es wichtig, Modelle für die richtigen Parameter, etwa die Fenstergröße, zu optimieren. 

Sie können maßgebliche Teile dieses Szenarios bearbeiten und verschiedene Problemszenarien ausprobieren, etwa diejenigen, in die im [Predictive Maintenance Modelling Guide](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1) beschrieben sind, wobei mehrere weitere Datenquellen einbezogen werden.


## <a name="references"></a>Referenzen

- [Predictive Maintenance Solution Template (Lösungsvorlage für die vorbeugende Wartung)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Predictive Maintenance Modeling Guide (Modellhandbuch für die vorbeugende Wartung)](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
- [Predictive Maintenance Modeling Guide Python Notebook (Modellhandbuch für die vorbeugende Wartung mit Python Notebook)](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
- [Predictive Maintenance using PySpark (Vorbeugende Wartung mit PySpark)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

