---
title: Vorbeugende Wartung in der Praxis| Microsoft-Dokumentation
description: Vorbeugende Wartung in der Praxis mit PySpark
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
ms.openlocfilehash: 21cf8201236224244e6ed34f91f9c5c601ab9a79
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="predictive-maintenance-real-world-scenario"></a>Vorbeugende Wartung in der Praxis.

Die Auswirkungen von ungeplanten Ausfällen von Ausrüstung können für jedes Unternehmen schädlich sein. Es ist entscheidend, für den ausfallsicheren Betrieb der Feldausrüstung zu sorgen, um die Auslastung und Leistung zu erhöhen, indem teure, ungeplante Ausfallzeiten minimiert werden. Eine frühzeitige Identifizierung von Problemen ermöglicht die Bereitstellung eingeschränkter Wartungsressourcen auf kosteneffektive Weise und eine Verbesserung der Qualitäts- und Lieferkettenprozesse. 

In diesem Szenario wird ein relativ [umfangreiches simuliertes Dataset](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) untersucht, um durch ein Predictive Maintenance Data Science-Projekt mit Datenerfassung, Featureentwicklung, Modellerstellung sowie Modellbetrieb und Modellbereitstellung zu führen. Der Code für den gesamten Prozess ist in Jupyter-Notebooks mit PySpark in Azure ML-Workbench geschrieben. Das endgültige Modell wird mithilfe der Azure Machine Learning-Modellverwaltung implementiert, um in Echtzeit Vorhersagen über Geräteausfälle zu machen.   

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Im Folgenden finden Sie den Link zum öffentlichen GitHub-Repository für Problemberichte und Beiträge: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

Ein großes Problem von Unternehmen mit Vermögensreichtum stellen die erheblichen Kosten für Verzögerungen aufgrund von mechanischen Problemen dar. Die meisten Unternehmen sind daran interessiert, vorherzusehen, wann diese Probleme auftreten, um sie vor dem Eintreten proaktiv zu verhindern. Ziel ist es, die Kosten durch Reduzierung von Ausfallzeiten zu senken und die Sicherheit möglicherweise zu erhöhen. 

Dieses Szenario greift Ideen aus dem [Predictive Maintenance Playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) auf, um den Aufbau eines prädiktiven Modells für ein simuliertes Dataset zu demonstrieren. Die Beispieldaten werden aus gemeinsamen Elementen abgeleitet, die in vielen Predictive Maintenance-Anwendungsfällen beobachtet wurden.

Das Geschäftsproblem für diese simulierten Daten ist die Vorhersage von Problemen, die durch Komponentenfehler verursacht wurden. Aus diesem Grund lautet die Frage hier: „*Wie hoch ist die Wahrscheinlichkeit, dass ein Computer aufgrund eines Komponentenfehlers ausfällt*?“ Dieses Problem wird als ein mehrklassiges Klassifizierungsproblem (mehrere Komponenten pro Computer) formatiert, und ein Machine Learning-Algorithmus wird verwendet, um das Vorhersagemodell zu erstellen. Das Modell wird mit Verlaufsdaten trainiert, die von Computern gesammelt wird. In diesem Szenario durchläuft ein Benutzer verschiedene Schritte zur Implementierung eines solchen Modells in der Azure Machine Learning Workbench-Umgebung.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/en-us/free/) (kostenlose Testversionen verfügbar)
* Eine installierte Kopie der [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) nach dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.
* Die Azure Machine Learning-Operationalisierung erfordert eine lokal eingerichteten Bereitstellungsumgebung und ein [Konto zur Modellverwaltung](model-management-overview.md).

Dieses Beispiel kann in beliebigen AML Workbench-Computekontexten ausgeführt werden. Es wird jedoch empfohlen, für die Ausführung einen Computer mit mindestens 16 GB Arbeitsspeicher zu wählen. Dieses Szenario wurde auf einem Windows 10-Computer mit einer remote ausgeführten DS4_V2 Standard [Data Science Virtual Machine für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) erstellt und getestet.

Die Modelloperationalisierung erfolgte mit der Version 0.1.0a22 der Azure ML CLI.

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein
4.  Geben Sie „Vorbeugende Wartung“ ins Suchfeld **Projektvorlagen suchen** ein, und wählen Sie die Vorlage **Predictive Maintenance** aus.
5.  Klicken Sie auf die Schaltfläche **Erstellen**.

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

Die [simulierten Daten](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestehen aus fünf (.csv-)Dateien mit durch Trennzeichen getrennten Werten. Folgen Sie den Links, um eine detailliertere Beschreibung der Datasets zu erhalten.

* [Computer](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): Features, mit denen die Computer unterschieden werden können. Beispielsweise Alter und Modell.
* [Fehler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): Das Fehlerprotokoll enthält ausgelöste, nicht unterbrechende Fehler, während der Computer weiterhin funktionsfähig ist. Diese Fehler werden nicht als Fehler betrachtet, obwohl sie möglicherweise ein zukünftiges Fehlerereignis vorhersehen. Datum und Uhrzeit der Fehler werden auf die Stunde gerundet, da die Telemetriedaten stündlich gesammelt werden.
* [Wartung](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Das Wartungsprotokoll enthält sowohl geplante als auch nicht geplanter Wartungsdatensätze. Eine geplante Wartung besteht aus regelmäßigen Überprüfungen von Komponenten. Eine nicht geplante Wartung geschieht, wenn mechanische Fehler oder andere Leistungseinbußen auftreten. Datum und Uhrzeit der Wartung werden auf die Stunde gerundet, da die Telemetriedaten stündlich gesammelt werden.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): Die Telemetriedaten bestehen aus Zeitreihenmessungen von mehreren Sensoren innerhalb der einzelnen Computer. Die Daten werden durch Mittelwertbildung der Sensorwerte über jeweils ein Stundenintervall protokolliert.
* [Fehler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): Fehler sind ein Austausch von Komponenten innerhalb des Wartungsprotokolls. Jedes Dataset enthält die Computer-ID, den Typ der Komponente und das Datum und die Uhrzeit des Austauschs. Diese Datensätze werden verwendet, um Machine Learning-Benennungen zu erstellen, die das Modell vorhersagen soll.

Navigieren Sie zum Jupyter Notebook-Szenario [Datenerfassung](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) im Code-Abschnitt, um die unformatierten Datasets aus dem GitHub-Repository herunterzuladen und die PySpark-Datasets für diese Analyse zu erstellen.

## <a name="scenario-structure"></a>Szenariostruktur
Der Inhalt für das Szenario ist im [GitHub-Repository](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance) verfügbar. 

Die [Infodatei](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) beschreibt den Arbeitsablauf von der Vorbereitung der Daten über die Erstellung eines Modells bis hin zur Bereitstellung einer Lösung für die Produktion. Jeder Schritt des Workflows ist in einem Jupyter-Notebook im Ordner [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) im Repository gekapselt.   

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Dieses Notebook dient zum Herunterladen der fünf CSV-Eingabedateien, zur vorläufigen Datenbereinigung und Visualisierung. Das Notebook konvertiert die einzelnen Datasets in das PySpark-Format und speichert sie in einem Azure-Blobcontainer zur Verwendung im Notebook für die Featureentwicklung.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Unter Verwendung des unformatierten Datasets von Azure-Blob werden Modellfeatures unter Verwendung des Standardzeitreihenansatzes für Telemetrie-, Fehler- und Wartungsdaten konstruiert. Anhand des ausfallbedingten Komponentenwechsels werden die Modellbeschriftungen erstellt, die beschreiben, welche Komponente ausgefallen ist. Die beschrifteten Featuredaten werden für das Modellerstellungsnotebook in einem Azure-Blob gespeichert.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Mithilfe des beschrifteten Featuredatasets unterteilt das Modellierungsnotebook die Daten anhand des Datum-/Uhrzeitstempels in Trainings- und Entwicklungsdatasets. Das Notebook ist für das Experimentieren mit `pyspark.ml.classification`-Modellen eingerichtet. Die Trainingsdaten sind vektorisiert, und der Benutzer kann entweder mit einem `DecisionTreeClassifier` oder einem `RandomForestClassifier` experimentieren und dabei Hyperparameter verändern, um das Modell mit der besten Leistung zu finden. Die Leistung wird durch die Auswertung von Messstatistiken für das Entwicklungsdataset ermittelt. Diese Statistiken werden zur Nachverfolgung wieder auf dem Laufzeitbildschirm der AML Workbench protokolliert. Bei jeder Ausführung speichert das Notebook das resultierende Modell auf der lokalen Festplatte, auf der der Jupyter Notebook-Kernel ausgeführt wird. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Mithilfe des letzten Modells, das auf der lokalen Festplatte (Jupyter-Notebook-Kernel) gespeichert wurde, erstellt dieses Notebook die Komponenten zur Bereitstellung des Modells in einem Azure-Webdienst. Die gesamten Betriebsobjekte werden in der Datei `o16n.zip` komprimiert, die in einem anderen Azure-Blobcontainer gespeichert ist. Die ZIP-Datei enthält Folgendes:

* `service_schema.json`: Die Schemadefinitionsdatei für die Bereitstellung. 
* `pdmscore.py`: Die Funktionen „init ()“ und „run ()“, die der Azure-Webdienst benötigt.
* `pdmrfull.model`: Das Modelldefinitionsverzeichnis.
    
 Das Notebook testet die Funktionen mit der Modelldefinition, bevor es die Operationalisierungsobjekte für die Bereitstellung packt. Anweisungen zur Bereitstellung sind am Ende des Notebooks zu finden.

## <a name="conclusion"></a>Zusammenfassung

Dieses Szenario bietet dem Leser einen Überblick über die Erstellung einer vorbeugenden End-to-End-Wartungslösung mit PySpark innerhalb der Jupyter Notebook-Umgebung in Azure ML Workbench. In diesem Beispielszenario wird auch die Modellbereitstellung durch die Azure Machine Learning-Modellverwaltungsumgebung detailliert beschrieben, um Vorhersagen zu Geräteausfällen in Echtzeit zu machen.

## <a name="references"></a>Referenzen

Weitere Beispiele für Predictive Maintenance-Anwendungsfälle sind für eine Vielzahl von Plattformen verfügbar:

* [Predictive Maintenance Solution Template (Lösungsvorlage für die vorbeugende Wartung)](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Predictive Maintenance Modeling Guide (Modellhandbuch für die vorbeugende Wartung)](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predictive Maintenance Modeling Guide using SQL R Services (Modellhandbuch für die vorbeugende Wartung mit SQL R Services)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Predictive Maintenance Modeling Guide Python Notebook (Modellhandbuch für die vorbeugende Wartung mit Python Notebook)](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Predictive Maintenance using PySpark (Vorbeugende Wartung mit PySpark)](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Deep Learning für Predictive Maintenance](
 https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Nächste Schritte

Es gibt viele weitere Beispielszenarien innerhalb von Azure Machine Learning Workbench, die zusätzliche Funktionen des Produkts veranschaulichen. 