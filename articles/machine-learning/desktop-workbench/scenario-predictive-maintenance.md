---
title: Predictive Maintenance in der Praxis – Szenarien| Microsoft-Dokumentation
description: Predictive Maintenance in der Praxis – Szenarien mit Verwendung von PySpark
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: 791c34785fa817fd68d0bec8111bf23e606c9b64
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Predictive Maintenance in der Praxis – Szenarien

Die Auswirkungen von ungeplanten Ausfällen von Ausrüstung können für jedes Unternehmen schädlich sein. Es ist entscheidend, für den ausfallsicheren Betrieb der Feldausrüstung zu sorgen, um die Auslastung und Leistung zu erhöhen und teure, ungeplante Ausfallzeiten zu minimieren. Eine frühzeitige Identifizierung von Problemen ermöglicht die Bereitstellung eingeschränkter Wartungsressourcen auf kosteneffektive Weise und eine Verbesserung der Qualitäts- und Lieferkettenprozesse. 

In diesem Szenario wird ein relativ [umfangreiches simuliertes Dataset](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) untersucht, um durch ein Predictive Maintenance Data Science-Projekt mit Datenerfassung, Featureentwicklung, Modellerstellung sowie Modellbetrieb und Modellbereitstellung zu führen. Der Code für den gesamten Prozess ist in Jupyter-Notebook mithilfe von PySpark in Azure ML Workbench geschrieben. Das endgültige Modell wird mithilfe der Azure Machine Learning-Modellverwaltung implementiert, um in Echtzeit Vorhersagen über Geräteausfälle zu machen.   

### <a name="cortana-intelligence-gallery-github-repository"></a>GitHub-Repository des Cortana Intelligence-Katalogs

Die Azure AI Gallery für das PM-Tutorial ist ein öffentliches GitHub-Repository ([https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)), in dem Sie Probleme melden und Beiträge beisteuern können.


## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

Ein großes Problem von Unternehmen in objektlastigen Branchen stellen die erheblichen Kosten für Verzögerungen aufgrund von mechanischen Problemen dar. Die meisten Unternehmen sind daran interessiert, vorherzusehen, wann diese Probleme auftreten, um sie vor dem Eintreten proaktiv zu verhindern. Ziel ist es, die Kosten durch Reduzierung von Ausfallzeiten zu senken und die Sicherheit möglicherweise zu erhöhen. 

Dieses Szenario greift Ideen aus dem [Predictive Maintenance Playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) auf, um die Erstellung eines prädiktiven Modells für ein simuliertes Dataset zu demonstrieren. Die Beispieldaten werden aus gemeinsamen Elementen abgeleitet, die in vielen Predictive Maintenance-Anwendungsfällen beobachtet wurden.

Das Geschäftsproblem für diese simulierten Daten ist die Vorhersage von Problemen, die durch Komponentenfehler verursacht werden. Die Frage lautet: „*Wie hoch ist die Wahrscheinlichkeit, dass ein Computer aufgrund eines Komponentenfehlers ausfällt*?“ Dieses Problem wird als Klassifizierung mit mehreren Klassen (mehrere Komponenten pro Computer) formatiert. Zum Erstellen des Vorhersagemodells wird ein Machine Learning-Algorithmus verwendet. Das Modell wird mit Verlaufsdaten trainiert, die von Computern gesammelt werden. In diesem Szenario durchläuft ein Benutzer verschiedene Schritte zur Implementierung des Modells in der Machine Learning Workbench-Umgebung.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
* Eine installierte Kopie von [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Gehen Sie zum Installieren des Programms und zum Erstellen eines Arbeitsbereichs nach der [Schnellstartanleitung für die Installation](../service/quickstart-installation.md) vor.
* Die Azure Machine Learning-Operationalisierung erfordert eine lokal eingerichtete Bereitstellungsumgebung und ein [Azure Machine Learning-Modellverwaltungskonto](model-management-overview.md).

Dieses Beispiel kann für jeden Machine Learning Workbench-Computekontext ausgeführt werden. Es wird jedoch empfohlen, das Beispiel mit mindestens 16 GB Arbeitsspeicher auszuführen. Dieses Szenario wurde auf einem Windows 10-Computer mit einem remote ausgeführten [virtuellen Data Science-Computer (DSVM) für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) vom Typ „DS4_V2 Standard“ erstellt und getestet.

Die Modelloperationalisierung erfolgte mit der Version 0.1.0a22 der Azure Machine Learning-CLI.

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie mit diesem Beispiel als Vorlage ein neues Projekt:
1.  Öffnen Sie Machine Learning Workbench.
2.  Wählen Sie auf der Seite **Projekte** das Pluszeichen (**+**) aus, und wählen Sie dann **Neues Projekt** aus.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4.  Geben Sie „Predictive Maintenance“ in das Suchfeld **Projektvorlagen suchen** ein, und wählen Sie die Vorlage **Predictive Maintenance** aus.
5.  Klicken Sie auf **Erstellen**.

## <a name="prepare-the-notebook-server-computation-target"></a>Vorbereiten des Berechnungsziels des Notebook-Servers

Wählen Sie für die Ausführung auf Ihrem lokalen Computer im Machine Learning Workbench-Menü **Datei** entweder **Eingabeaufforderung öffnen** oder **PowerShell-CLI öffnen** aus. Die CLI-Schnittstelle ermöglicht Ihnen den Zugriff auf Ihre Azure-Dienste mit den `az`-Befehlen. Melden Sie sich zunächst mit dem folgenden Befehl bei Ihrem Azure-Konto an:

```
az login
``` 

Dieser Befehl stellt einen Authentifizierungsschlüssel für die Verwendung mit der URL „https:\\aka.ms\devicelogin“ bereit. Die Befehlszeilenschnittstelle wartet, bis der Geräteanmeldungsvorgang zurückkehrt und einige Verbindungsinformationen bereitstellt. Wenn Sie über eine lokale [Docker](https://www.docker.com/get-docker)-Installation verfügen, bereiten Sie als nächstes die lokale Compute-Umgebung mit dem folgenden Befehl vor:

```
az ml experiment prepare --target docker --run-configuration docker
```

Hinsichtlich der Arbeitsspeicher- und Speicherplatzanforderungen wird die Ausführung auf einem [DSVM für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) empfohlen. Nachdem die DSVM konfiguriert ist, bereiten Sie die Remote-Docker-Umgebung mit den folgenden zwei Befehlen vor:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Nachdem Sie die Verbindung mit dem Remote-Docker-Container hergestellt haben, bereiten Sie die DSVM-Docker-Compute-Umgebung mit dem folgenden Befehl vor: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Öffnen Sie unter Verwendung der vorbereiteten Docker-Compute-Umgebung den Jupyter-Notebook-Server auf der Azure Machine Learning Workbench-Registerkarte **Notebooks**, oder starten Sie mit dem folgenden Befehl einen browserbasierten Server: 

```
az ml notebook start
```

Die Beispielnotebooks werden im Verzeichnis „Code“ gespeichert. Die Notebooks sind so eingerichtet, dass sie nacheinander ausgeführt werden, wobei mit dem ersten Notebook (Code\1_data_ingestion.ipynb) begonnen wird. Beim Öffnen der einzelnen Notebooks werden Sie aufgefordert, den Computekernel auszuwählen. Wählen Sie den Kernel „[Projektname]_Vorlage [Verbindungsname]“ für die Ausführung auf dem zuvor konfigurierten DSVM aus.

## <a name="data-description"></a>Datenbeschreibung

Die [simulierten Daten](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestehen aus fünf (.csv-)Dateien mit durch Trennzeichen getrennten Werten. Verwenden Sie die folgenden Links, um ausführliche Beschreibungen der Datasets abzurufen.

* [Computer](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): Features, mit denen die Computer unterschieden werden können. Beispielsweise Alter und Modell.
* [Fehler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): Das Fehlerprotokoll enthält nicht unterbrechende Fehler, die ausgelöst werden, während der Computer weiterhin funktionsfähig ist. Diese Fehler werden nicht als Ausfälle betrachtet, obwohl sie auf ein zukünftiges Ausfallereignis hindeuten können. Die Datums- und Uhrzeitwerte für die Fehler werden auf die Stunde gerundet, da die Telemetriedaten stündlich gesammelt werden.
* [Wartung](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Das Wartungsprotokoll enthält sowohl geplante als auch nicht geplanter Wartungsdatensätze. Die geplante Wartung entspricht regelmäßigen Überprüfungen von Komponenten. Eine nicht geplante Wartung erfolgt, wenn mechanische Fehler oder andere Leistungseinbußen auftreten. Die Datums- und Uhrzeitwerte für die Wartung werden auf die Stunde gerundet, da die Telemetriedaten stündlich gesammelt werden.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): Die Telemetriedaten bestehen aus Zeitreihenmessungen von mehreren Sensoren innerhalb der einzelnen Computer. Die Daten werden durch Mittelwertbildung der Sensorwerte über jeweils ein Stundenintervall protokolliert.
* [Fehler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): Fehler sind ein Austausch von Komponenten innerhalb des Wartungsprotokolls. Jedes Dataset enthält die Computer-ID, den Typ der Komponente und das Datum und die Uhrzeit des Austauschs. Diese Datensätze werden verwendet, um Machine Learning-Benennungen zu erstellen, die das Modell vorhersagen soll.

Zum Herunterladen der unformatierten Datasets aus dem GitHub-Repository und zum Erstellen der PySpark-Datasets für diese Analyse verwenden Sie das Jupyter Notebook-Szenario [Datenerfassung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) im Code-Ordner.

## <a name="scenario-structure"></a>Szenariostruktur
Der Inhalt für das Szenario ist im [GitHub-Repository](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance) verfügbar. 

Die [Infodatei](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) beschreibt den Workflow von der Vorbereitung der Daten über die Erstellung eines Modells bis hin zur Bereitstellung einer Lösung für die Produktion. Jeder Schritt des Workflows ist in einem Jupyter-Notebook im Ordner [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) im Repository gekapselt.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Dieses Notebook dient zum Herunterladen der fünf CSV-Eingabedateien und zur vorläufigen Datenbereinigung und Visualisierung. Das Notebook konvertiert die einzelnen Datasets in das PySpark-Format und speichert sie in einem Azure-Blobcontainer zur Verwendung im Notebook für die Featureentwicklung.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Die Modellfeatures werden unter Verwendung eines Standardzeitreihenansatzes für Telemetrie-, Fehler- und Wartungsdaten aus dem unformatierten Dataset aus dem Azure Blob-Speicher konstruiert. Anhand des ausfallbedingten Komponentenwechsels werden die Modellbeschriftungen erstellt, die beschreiben, welche Komponente ausgefallen ist. Die beschrifteten Featuredaten werden für das Modellerstellungsnotebook in einem Azure-Blob gespeichert.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Mithilfe des beschrifteten Featuredatasets unterteilt das Modellerstellungsnotebook die Daten anhand des Datum-/Uhrzeitstempels in Trainings- und Entwicklungsdatasets. Das Notebook wird als Experiment mit pyspark.ml.classification-Modellen eingerichtet. Die Trainingsdaten werden vektorisiert. Benutzer kann entweder mit einem **DecisionTreeClassifier**- oder einem **RandomForestClassifier**-Element experimentieren und dabei Hyperparameter verändern, um das Modell mit der besten Leistung zu finden. Die Leistung wird durch die Auswertung von Messstatistiken für das Entwicklungsdataset ermittelt. Diese Statistiken werden zur Nachverfolgung wieder auf dem Laufzeitbildschirm von Machine Learning Workbench protokolliert. Bei jeder Ausführung speichert das Notebook das resultierende Modell auf der lokalen Festplatte, auf der der Jupyter Notebook-Kernel ausgeführt wird. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Dieses Notebook verwendet das letzte auf der lokalen Festplatte (Jupyter-Notebook-Kernel) gespeicherte Modell zum Erstellen der Komponenten zur Bereitstellung des Modells in einem Azure-Webdienst. Die gesamten Betriebsobjekte werden in der Datei „o16n.zip“ komprimiert, die in einem anderen Azure-Blobcontainer gespeichert ist. Die ZIP-Datei enthält Folgendes:

* **service_schema.json**: Die Schemadefinitionsdatei für die Bereitstellung. 
* **pdmscore.py**: Die Funktionen **init ()** und **run ()**, die der Azure-Webdienst benötigt.
* **pdmrfull.model**: Das Modelldefinitionsverzeichnis.
    
Das Notebook testet die Funktionen mit der Modelldefinition, bevor es die Operationalisierungsobjekte für die Bereitstellung packt. Anweisungen zur Bereitstellung sind am Ende des Notebooks zu finden.

## <a name="conclusion"></a>Zusammenfassung

Dieses Szenario bietet eine Übersicht über die Erstellung einer umfassenden Predictive Maintenance-Lösung mit PySpark innerhalb der Jupyter-Notebook-Umgebung in Azure Machine Learning Workbench. In diesem Beispielszenario wird auch die Modellbereitstellung durch die Machine Learning-Modellverwaltungsumgebung für Vorhersagen zu Geräteausfällen in Echtzeit detailliert beschrieben.

## <a name="references"></a>Referenzen

Die folgenden Referenzen bieten Beispiele für andere Predictive Maintenance-Anwendungsfälle für verschiedene Plattformen:

* [Predictive Maintenance Solution Template (Lösungsvorlage für die vorbeugende Wartung)](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Predictive Maintenance Modeling Guide (Modellhandbuch für die vorbeugende Wartung)](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predictive Maintenance Modeling Guide using SQL R Services (Modellhandbuch für die vorbeugende Wartung mit SQL R Services)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Predictive Maintenance Modeling Guide Python Notebook (Modellhandbuch für die vorbeugende Wartung mit Python Notebook)](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Predictive Maintenance using PySpark (Vorbeugende Wartung mit PySpark)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Deep Learning für Predictive Maintenance](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Nächste Schritte

Mehr Beispielszenarien, die weitere Funktionen des Produkts veranschaulichen, sind in Azure Machine Learning Workbench verfügbar. 
