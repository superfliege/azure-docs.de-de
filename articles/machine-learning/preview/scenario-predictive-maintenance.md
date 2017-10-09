--- 
title: Vorbeugende Wartung in der Praxis| Microsoft-Dokumentation
description: Vorbeugende Wartung in der Praxis mit PySpark
services: machine-learning
author: jaymathe
ms.author: jaymathe
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 09/25/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 7bb5672a2b9de0a62af8b6155bfdead6a0105eb5
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

--- 

# <a name="predictive-maintenance-real-world-scenario"></a>Vorbeugende Wartung in der Praxis


Die Auswirkungen von ungeplanten Ausfällen von Ausrüstung können für jedes Unternehmen schädlich sein. Es ist deshalb entscheidend, für den ausfallsicheren Betrieb der Feldausrüstung zu sorgen, um die Auslastung und Leistung zu erhöhen, indem teure, ungeplante Ausfallzeiten minimiert werden. Eine frühzeitige Identifizierung von Problemen ermöglicht die Bereitstellung eingeschränkter Wartungsressourcen auf kosteneffektive Weise und eine Verbesserung der Qualitäts- und Lieferkettenprozesse. 

In diesem Szenario verwenden wir relativ [umfangreiche Datenmengen](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data), um Benutzer über wichtige Schritte wie Datenerfassung, Featureentwicklung, Modellerstellung und letztendlich Modellbetrieb und Modellbereitstellung zu informieren. Der Code für den gesamten Prozess ist in PySpark geschrieben und wird mithilfe von Jupyter Notebooks in Azure ML-Workbench implementiert. Das beste Modell wird schließlich mithilfe von Azure Machine Learning-Modellverwaltung für die Verwendung in einer Produktionsumgebung zum Vorhersagen von Fehlern in Echtzeit operationalisiert.   

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Im Folgenden finden Sie den Link zum öffentlichen GitHub-Repository: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Übersicht über Anwendungsfälle

Ein großes Problem von Unternehmen mit Vermögensreichtum stellen die erheblichen Kosten für Verzögerungen aufgrund von mechanischen Problemen dar. Die meisten Unternehmen sind daran interessiert, vorherzusehen, wann diese Probleme auftreten, um sie vor dem Eintreten proaktiv zu verhindern. Dies reduziert die Kosten durch das Reduzieren von Ausfallzeiten und erhöht in einigen Fällen die Sicherheit. Eine ausführliche Erläuterung der allgemeinen Anwendungsfälle und den Modellierungsansatz für die vorbeugende Wartung finden Sie im [Playbook für die vorbeugende Wartung](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance).

Dieses Szenario nutzt die Ideen aus dem Playbook mit dem Ziel, ein Vorhersagemodell für ein Szenario zu implementieren, das auf einer Synthese von mehreren, praktischen Geschäftsproblemen basiert. Dieses Beispiel vereint allgemeine Datenelemente, die in vielen Anwendungsfällen der vorbeugende Wartung beobachtet wurden.

Das Geschäftsproblem für diese simulierten Daten ist die Vorhersage von Problemen, die durch Komponentenfehler verursacht wurden. Aus diesem Grund lautet die Frage hier: „*Wie hoch ist die Wahrscheinlichkeit, dass ein Computer aufgrund eines Komponentenfehlers ausfällt*?“ Dieses Problem wird als ein mehrklassiges Klassifizierungsproblem (mehrere Komponenten pro Computer) formatiert, und ein Machine Learning-Algorithmus wird verwendet, um das Vorhersagemodell zu erstellen. Das Modell wird mit Verlaufsdaten trainiert, die von Computern gesammelt wird. In diesem Szenario durchläuft ein Benutzer verschiedene Schritte zur Implementierung eines solchen Modells in der Azure Machine Learning Workbench-Umgebung.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/en-us/free/) (kostenlose Testversionen verfügbar).
* Eine installierte Kopie der [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) nach dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.
* Zwischenergebnisse für Jupyter Notebooks in diesem Szenario sind in einem Azure Blob Storage-Container gespeichert. Anweisungen zum Einrichten eines Azure Storage-Kontos stehen unter diesem [Link](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) zur Verfügung. 
* Für die [Operationalisierung](https://github.com/Azure/Machine-Learning-Operationalization) des Modells wird empfohlen, dass der Benutzer ein [Docker-Modul](https://www.docker.com/) installiert und lokal ausführt. Wenn dies nicht der Fall ist, können Sie die Clusteroption verwenden. Beachten Sie allerdings, dass die Ausführung eines [Azure Container Service (ACS)](https://azure.microsoft.com/en-us/services/container-service/) häufig sehr teuer sein kann.
* In diesem Szenario wird davon ausgegangen, dass der Benutzer die Azure ML Workbench mit einem lokal installierten Docker-Modul auf einem Windows 10-Computer ausführt. 
* Das Szenario wurde auf einem Windows 10-Computer mit den folgenden Spezifikationen erstellt und getestet: Intel® Core i7-4600U CPU @ 2.10 GHz, 8 GB RAM, 64-Bit-Betriebssystem, X64-basierter Prozessor mit Docker Version 17.06.0-ce-win19 (12801). 
* Die Modelloperationalisierung erfolgte mit dieser Version von Azure ML-CLI: Azure-Cli-ml==0.1.0a22

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein
4.  Geben Sie im Suchfeld **Suche Projektvorlagen** „Vorbeugende Wartung“ ein, und wählen Sie die Vorlage aus
5.  Klicken Sie auf **Erstellen**

## <a name="data-description"></a>Datenbeschreibung

Die [simulierten Daten](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestehen aus fünf (.csv-)Dateien mit durch Trennzeichen getrennten Werten. 

* [Computer](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): Features, mit denen die Computer unterschieden werden können. Beispielsweise Alter und Modell.
* [Fehler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): Das Fehlerprotokoll enthält ausgelöste, nicht unterbrechende Fehler, während der Computer weiterhin funktionsfähig ist. Diese Fehler werden nicht als Fehler betrachtet, obwohl sie möglicherweise ein zukünftiges Fehlerereignis vorhersehen. Datum und Uhrzeit der Fehler werden auf die Stunde gerundet, da die Telemetriedaten stündlich gesammelt werden.
* [Wartung](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Das Wartungsprotokoll enthält sowohl geplante als auch nicht geplanter Wartungsdatensätze. Eine geplante Wartung besteht aus regelmäßigen Überprüfungen von Komponenten. Eine nicht geplante Wartung geschieht, wenn mechanische Fehler oder andere Leistungseinbußen auftreten. Datum und Uhrzeit der Wartung werden auf die Stunde gerundet, da die Telemetriedaten stündlich gesammelt werden.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): Die Zeitreihentelemetriedaten bestehen aus Messungen von Spannung, Drehung, Druck und Vibrationssensor, die von jedem Computer in Echtzeit gesammelt werden. Die Daten stellen den Durchschnitt einer Stunde dar und werden in Telemetrieprotokollen gespeichert.
* [Fehler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): Fehler sind ein Austausch von Komponenten innerhalb des Wartungsprotokolls. Jedes Dataset enthält die Computer-ID, den Typ der Komponente und das Datum und die Uhrzeit des Austauschs. Diese Datensätze werden verwendet, um Machine Learning-Benennungen zu erstellen, die das Modell vorhersagen soll.

Navigieren Sie zum Jupyter Notebook-Szenario [Datenerfassung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) im Code-Abschnitt, um die unformatierten Datasets aus dem GitHub-Repository herunterzuladen und die PySpark-Datasets für diese Analyse zu erstellen.

## <a name="scenario-structure"></a>Szenariostruktur
Der Inhalt für das Szenario ist im [GitHub-Repository](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance) verfügbar. 

Es gibt eine [Infodatei](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) im Repository, die die Prozesse der Datenvorbereitung bis zur Erstellung einiger Modelle und der Operationalisierung eines der besten Modelle erläutert. Die vier Jupyter Notebooks stehen im [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code)-Ordner innerhalb des Repositorys zur Verfügung.   

Als Nächstes wird der Workflow des Szenarios Schritt für Schritt beschrieben. Das End-to-End-Szenario ist in PySpark geschrieben, und wie im Folgenden erläutert, in vier Notebooks unterteilt:

* [Datenerfassung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Dieses Notebook verarbeitet die Datenerfassung der fünf CSV-Eingabedateien, führt einige vorbereitenden Bereinigungen durch, erstellt zusammenfassende Grafiken, um den Datendownload zu überprüfen, und speichert die resultierenden Datasets schließlich in einem Azure Blob-Container für die Verwenden im nächsten Notebook.

* [Featureentwicklung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Verwenden Sie das bereinigte Dataset aus dem vorherigen Schritt, um Verzögerungsfeatures für die Telemetriesensoren und zusätzliche Funktionen zum Erstellen von Variablen (Anzahl der Tage seit dem letzten Austausch) zu erstellen. Schließlich werden die entsprechenden Datensätze mit Fehlern gekennzeichnet, um ein endgültiges Dataset zu erstellen, das für den nächsten Schritt in einem Azure Blob gespeichert wird. 

* [Modellerstellung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Das letzte entwickelte Dataset wird dann in zwei Teile unterteilt, nämlich in ein Schul- und ein Testdataset, die auf einem Datums-/ Uhrzeitstempel basieren. Die zwei Modelle, nämlich ein Random Forest Classifier und ein Decision Tree Classifier, werden auf dem Trainingsdataset erstellt und dann auf dem Testdataset bewertet. 

* [Modelloperationalisierung & -bereitstellung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Das beste Modell, das im vorherigen Schritt erstellt wurde, wird dann als .model-Datei zusammen mit der relevanten JSON-Schema-Datei für die Bereitstellung gespeichert. Die init()- und run()-Funktionen werden zuerst lokal getestet, bevor Sie das Modell unter Verwendung der Azure Machine Learning-Modellverwaltungsumgebung das Modell für die Verwendung in einer Produktionsumgebung für Fehlervorhersagen in Echtzeit operationalisieren.  

## <a name="conclusion"></a>Zusammenfassung

Dieses Szenario bietet dem Leser einen Überblick über die Erstellung einer vorbeugenden End-to-End-Wartungslösung mit PySpark innerhalb der Jupyter Notebook-Umgebung in Azure ML Workbench. Das Szenario zeigt dem Leser auch, wie das beste Modell mithilfe der Azure Machine Learning-Modellverwaltung für die Verwendung in einer Produktionsumgebung zum Vorhersagen von Fehlern in Echtzeit einfach operationalisiert und bereitgestellt werden kann. Der Leser kann dann relevante Teile des Szenarios bearbeiten, um sie an seine Geschäftsanforderungen anzupassen.  

## <a name="references"></a>Referenzen

Dieser Anwendungsfall wurde zuvor auf mehreren Plattformen bereitgestellt, wie im Folgenden aufgeführt:

* [Predictive Maintenance Solution Template (Lösungsvorlage für die vorbeugende Wartung)](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Predictive Maintenance Modeling Guide (Modellhandbuch für die vorbeugende Wartung)](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predictive Maintenance Modeling Guide using SQL R Services (Modellhandbuch für die vorbeugende Wartung mit SQL R Services)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Predictive Maintenance Modeling Guide Python Notebook (Modellhandbuch für die vorbeugende Wartung mit Python Notebook)](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Predictive Maintenance using PySpark (Vorbeugende Wartung mit PySpark)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)



