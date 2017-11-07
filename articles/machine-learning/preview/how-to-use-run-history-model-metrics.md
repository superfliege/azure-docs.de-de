---
title: "Verwenden des Ausführungsverlaufs und der Modellmetriken in Azure Machine Learning Workbench | Microsoft-Dokumentation"
description: "Handbuch für die Verwendung der Funktionen für den Ausführungsverlauf und die Modellmetriken für Azure Machine Learning Workbench"
services: machine-learning
author: jopela
ms.author: jopela
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 920155b2e70409fe1c7c3569326cc5a1fa01b222
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-use-run-history-and-model-metrics-in-azure-machine-learning-workbench"></a>Verwenden des Ausführungsverlaufs und der Modellmetriken in Azure Machine Learning Workbench

Azure Machine Learning Workbench unterstützt das Data Science-Experimentieren über die Funktionen für den **Ausführungsverlauf** und die **Modellmetriken**.
Der **Ausführungsverlauf** bietet die Möglichkeit, die Ausgaben der Machine Learning-Experimente nachzuverfolgen und dann das Filtern und Vergleichen der Ergebnisse zu aktivieren.
Die **Modellmetriken** können von jedem Punkt Ihres Skripts aus protokolliert werden, wodurch die wichtigsten Werte Ihrer Data Science-Experimente nachverfolgt werden.
Dieser Artikel beschreibt die effektive Nutzung dieser Funktionen, um Rate und Qualität Ihres Data Science-Experimentierens zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung müssen Sie folgende Vorbereitungen treffen:
* [Create and Install Azure Machine Learning (Erstellen und Installieren von Azure Machine Learning)](quickstart-installation.md)
- [Create a Project (Erstellen eines Projekts)](quickstart-installation.md)


## <a name="azure-ml-logging-api-overview"></a>Übersicht über die Azure ML-Protokollierungs-API
Die [Azure ML-Protokollierungs-API](reference-logging-api.md) ist über das Modul **azureml.logging** in Python verfügbar (das mit Azure ML Workbench installiert wird). Nach dem Importieren dieses Moduls können Sie die Methode **get_azureml_logger** verwenden, um ein **Protokollierungsobjekt** zu instanziieren.
Sie können dann die Methode **log** der Protokollierung verwenden, um Schlüssel/Wert-Paare zu speichern, die von Ihren Python-Skripts erstellt wurden.
Derzeit wird das Protokollieren der Modellmetriken des Typs Skalar, Wörterbuch, Liste und Dataframe wie dargestellt unterstützt.

```Python
# create a logger instance in already set up environment 
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# log scalar (any integer or floating point type is fine)
logger.log("simple value", 7)


# log list
logger.log("all values", [5, 6, 7])
```
Es ist einfach, die Protokollierung innerhalb Ihrer Azure ML Workbench-Projekte zu verwenden. Die Vorgehensweise wird in diesem Artikel erläutert.

## <a name="create-a-project-in-azure-ml-workbench"></a>Erstellen eines Projekts in Azure ML Workbench
Wenn Sie noch nicht über ein Projekt verfügen, können Sie eines über [Create and Install Quickstart (Schnellstart für das Erstellen und Installieren)](quickstart-installation.md) im **Projektdashboard** erstellen und das Skript **iris_sklearn.py** wie dargestellt öffnen.

![Zugreifen auf ein Skript aus der Registerkarte „Dateien“](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-01b.png)

Sie können dieses Skript als Leitfaden für die erwartete Implementierung der Protokollierung der Modellmetrik in Azure ML verwenden.

## <a name="parameterize-and-log-model-metrics-from-script"></a>Parametrisieren und Protokollieren der Modellmetriken aus dem Skript
Im Skript **iris_sklearn.py** kann das erwartete Muster für das Importieren und Erstellen der Protokollierung in Python auf die folgenden Codezeilen reduziert werden.

```Python
from azureml.logging import get_azureml_logger
run_logger = get_azureml_logger()
```

Nach der Erstellung können Sie die Methode **log** mit einem beliebigen Name/Wert-Paar aufrufen.

Wenn die Entwicklung abgeschlossen ist, ist es oft hilfreich, die Skripts zu parametrisieren, damit die Werte über die Befehlszeile übergeben werden können.
Im folgenden Beispiel wird das Akzeptieren von Befehlszeilenparametern (falls vorhanden) mithilfe der Python-Standardbibliotheken dargestellt.
In diesem Skript wird ein einzelner Parameter für die verwendete Regularisierungsrate (*reg*) verwendet, um ein Klassifizierungsmodell anzupassen, um die *Genauigkeit* ohne Überanpassung zu erhöhen.
Diese Variablen werden dann als *Regularisierungsrate* und *Genauigkeit* protokolliert, sodass das Modell mit den optimalen Ergebnissen einfach identifiziert werden kann.

```Python
# change regularization rate and you will likely get a different accuracy.
reg = 0.01
# load regularization rate from argument if present
if len(sys.argv) > 1:
    reg = float(sys.argv[1])

print("Regularization rate is {}".format(reg))

# log the regularization rate
run_logger.log("Regularization Rate", reg)

# train a logistic regression model on the training set
clf1 = LogisticRegression(C=1/reg).fit(X_train, Y_train)
print (clf1)

# evaluate the test set
accuracy = clf1.score(X_test, Y_test)
print ("Accuracy is {}".format(accuracy))

# log accuracy
run_logger.log("Accuracy", accuracy)
```

Das Ausführen dieser Schritte in Ihren Skripts ermöglicht diesen die optimale Verwendung des **Ausführungsverlaufs**.

## <a name="launch-runs-from-project-dashboard"></a>Starten der Ausführung über das Projektdashboard
Wenn Sie in das **Projektdashboard** zurückkehren, können Sie eine **nachverfolgte Ausführung** starten, indem Sie das Skript **iris_sklearn.py** auswählen und den Parameter **Regularisierungsrate** im Bearbeitungsfeld **Argumente** eingeben.

![Eingeben der Parameter und Starten der Ausführung](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-05.png)

Da Azure ML Workbench nicht durch das Starten der nachverfolgten Ausführungen blockiert wird, können mehrere gleichzeitig gestartet werden.
Der Status jeder nachverfolgten Ausführung wird wie im Folgenden dargestellt im Bereich **Aufträge** angezeigt.

![Nachverfolgen von Ausführungen im Bereich „Aufträge“](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-06.png)

Dadurch wird die optimale Ressourcennutzung aktiviert, ohne jeden Auftrag seriell ausführen zu müssen.

## <a name="view-results-in-run-history"></a>Anzeigen der Ergebnisse im Ausführungsverlauf
Der Fortschritt und die Ergebnisse der nachverfolgten Ausführungen sind für die Analyse im **Ausführungsverlauf** von Azure ML Workbench verfügbar.
Der **Ausführungsverlauf** bietet drei verschiedene Ansichten:
- Dashboard
- Details
- Vergleich

Die Ansicht **Dashboard** zeigt Daten für alle Ausführungen eines angegebenen Skripts an, die in grafischer und tabellarischer Form gerendert wurden.
Die Ansicht **Details** zeigt alle Daten an, die bei einer bestimmten Ausführung eines angegebenen Skripts generiert wurden, einschließlich der protokollierten Metriken und Ausgabedateien (z.B. gerenderte Plots). Die Ansicht **Vergleich** ermöglicht das parallele Anzeigen der Ergebnisse von zwei bis drei Ausführungen, ebenfalls einschließlich der protokollierten Metriken und Ausgabedateien.

Die Werte für den Parameter **Regularisierungsrate** und das Ergebnis der **Genauigkeit** wurden für acht nachverfolgte Ausführungen von **iris_sklearn.py** protokolliert, um darzustellen, wie die Ansichten des Ausführungsverlaufs verwendet werden.

### <a name="run-history-dashboard"></a>Dashboard des Ausführungsverlaufs
Die Ergebnisse aller acht Ausführungen werden im **Dashboard des Ausführungsverlaufs** angezeigt.
Da **iris_sklearn.py** die *Regularisierungsrate* und die *Genauigkeit* protokolliert, zeigt das **Dashboard des Ausführungsverlaufs** standardmäßig Diagramme für diese Werte an.

![Dashboard des Ausführungsverlaufs](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-07.png)

Das **Dashboard des Ausführungsverlaufs** kann angepasst werden, sodass die protokollierten Werte auch im Raster angezeigt werden.  Durch das Klicken auf das Symbol **Anpassen** wird wie im Folgenden dargestellt das Dialogfeld **List View Customization** (Anpassen der Listenansicht) angezeigt.

![Anpassen der Raster des Dashboards des Ausführungsverlaufs](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-08.png)

Alle Werte, die während der nachverfolgten Ausführungen protokolliert werden, können angezeigt werden. Durch das Auswählen von **Regularisierungsrate** und **Genauigkeit** werden diese zum Raster hinzugefügt.

![Protokollierte Werte im angepassten Raster](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-09.png)

Es ist einfach, interessante Ausführungen zu ermitteln, indem Sie mit der Maus auf die Punkte in den Diagrammen zeigen.  In diesem Fall ergibt Ausführung 7 eine gute Genauigkeit in Kombination mit einer niedrigen Dauer.

![Ermitteln einer interessanten Ausführung](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-10.png)

Wenn Sie in einem Diagramm auf einen Punkt klicken, der Ausführung 7 zugeordnet ist, oder auf einen Link zu Ausführung 7 im Raster, werden die **Details des Ausführungsverlaufs** angezeigt.

### <a name="run-history-details"></a>Details des Ausführungsverlaufs
In dieser Ansicht werden die vollständigen Ergebnisse von Ausführung 7 zusammen mit den von Ausführung 7 erstellten Artefakten angezeigt.

![Details des Ausführungsverlaufs](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-11.png)

Die Ansicht **Details des Ausführungsverlaufs** bietet auch die Möglichkeit, alle Dateien **herunterzuladen**, die in den Ordner **./outputs** geschrieben wurden. Die Dateien werden im Cloudspeicher von Azure ML Workbench für den Ausführungsverlauf gesichert, dies ist jedoch Thema eines anderen Artikels.

Schließlich bieten die **Details des Ausführungsverlaufs** eine Möglichkeit, um den Zustand Ihres Projekts zum Zeitpunkt dieser Ausführung wiederherzustellen.
Durch das Klicken auf die Schaltfläche **Wiederherstellen** wird wie im Folgenden dargestellt ein Bestätigungsdialogfeld angezeigt.

![Bestätigen der Ausführung der Wiederherstellung](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-13.png)

Wenn Sie bestätigen, können Dateien überschrieben oder entfernt werden, also nutzen Sie diese Funktion mit Bedacht.

### <a name="run-history-comparison"></a>Vergleich von Ausführungsverläufen
Durch das Auswählen von zwei oder drei Ausführungen im **Dashboard des Ausführungsverlaufs** und das Klicken auf **Vergleichen** wird Ihnen die Ansicht **Vergleich von Ausführungsverläufen** angezeigt.
Klicken Sie alternativ auf **Vergleichen**, und wählen Sie eine Ausführung aus der Ansicht **Details des Ausführungsverlaufs** aus, um zur Ansicht **Vergleich von Ausführungsverläufen** zu gelangen.
In beiden Fällen bietet die Ansicht **Vergleich von Ausführungsverläufen** eine Möglichkeit, um die protokollierten Ergebnisse und Artefakte der zwei bis drei Ausführungen parallel anzuzeigen.

![Vergleich von Ausführungsverläufen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-12.png)

Diese Ansicht ist besonders nützlich für den Vergleich von Plots. Im Allgemeinen können jedoch alle Eigenschaften von Ausführungen hier verglichen werden.

### <a name="command-line-interface"></a>Befehlszeilenschnittstelle
Azure Machine Learning Workbench bietet auch Zugriff auf den Ausführungsverlauf über die **Befehlszeilenschnittstelle**.
Klicken Sie für den Zugriff auf die **Befehlszeilenschnittstelle** wie im Folgenden dargestellt auf das Menü **Eingabeaufforderung öffnen**.

![Öffnen der Eingabeaufforderung](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-14.png)

Auf die Befehle, die für den Ausführungsverlauf verfügbar sind, kann über `az ml history` zugegriffen werden. Eine Onlinehilfe ist verfügbar, indem Sie das Flag `-h` hinzufügen.
```
$ az ml history -h

Group
    az ml history: View run history of machine learning experiments.

Commands:
    compare : Compare two runs.
    download: Download all the artifacts from a run into the destination path.
    info    : Details about one run.
    last    : Get detail about most recent run.
    list    : List runs.
    promote : Promote Artifacts.
```
Diese Befehle bieten die gleichen Funktionen und geben die gleichen Daten zurück wie in den **Ansichten des Ausführungsverlaufs** dargestellt.
Das Ergebnis der letzten Ausführung kann beispielsweise als JSON-Objekt angezeigt werden.
```
$ az ml history last
{
  "Accuracy": 0.6792452830188679,
  "Regularization Rate": 0.078125,
  "attachments": "control_log, control_log.txt, driver_log, driver_log.txt, pid.txt, dataprep/backgroundProcess.log, dataprep/backgroundProcess_Engine.log, dataprep/backgroundProcess_EngineHost.log, dataprep/backgroundProcess_ProjectProvider.log, dataprep/backgroundProcess_Sampling.log, dataprep/backgroundProcess_Telemetry.log, outputs/cm.png, outputs/model.pkl, outputs/sdk_debug.txt, outputs/roc.png",
  "created_utc": "2017-09-08T00:58:01.611105+00:00",
  "description": null,
  "duration": "0:00:04.892389",
  "end_time_utc": "2017-09-08T00:58:07.951403+00:00",
  "experiment_id": "ce92d0a9-3e2c-4d51-85de-93ef22249ce1",
  "heartbeat_enabled": true,
  "hidden": false,
  "name": null,
  "parent_run_id": null,
  "properties": {
    "CommitId": "e77a5f0df2af1a482bbe39b70bfbb16b62228cb3"
  },
  "run_id": "IrisDemo_1504832281116",
  "run_number": 8,
  "script_name": "iris_sklearn.py",
  "start_time_utc": "2017-09-08T00:58:03.059014+00:00",
  "status": "Completed",
  "target": "local",
  "user_id": "e9fafe06-b0e4-4154-8374-aae34f9977b2"
}
```
Die Liste der Ausführungen kann ebenfalls in tabellarischem Format angezeigt werden.
```
$ az ml history list -o table
  Accuracy    Regularization Rate  Duration        Run_id                  Script_name      Start_time_utc                    Status
----------  ---------------------  --------------  ----------------------  ---------------  --------------------------------  ---------
  0.679245               0.078125  0:00:04.892389  IrisDemo_1504832281116  iris_sklearn.py  2017-09-08T00:58:03.059014+00:00  Completed
  0.679245               0.15625   0:00:04.734956  IrisDemo_1504832255198  iris_sklearn.py  2017-09-08T00:57:38.507196+00:00  Completed
  0.660377               0.3125    0:00:04.913762  IrisDemo_1504832234904  iris_sklearn.py  2017-09-08T00:57:16.849881+00:00  Completed
  0.660377               0.625     0:00:06.107764  IrisDemo_1504832210767  iris_sklearn.py  2017-09-08T00:56:54.602813+00:00  Completed
  0.641509               1.25      0:00:04.742727  IrisDemo_1504832171373  iris_sklearn.py  2017-09-08T00:56:13.879280+00:00  Completed
  0.660377               2.5       0:00:04.915401  IrisDemo_1504832148526  iris_sklearn.py  2017-09-08T00:55:52.937083+00:00  Completed
  0.641509               5         0:00:04.730627  IrisDemo_1504832127172  iris_sklearn.py  2017-09-08T00:55:29.612382+00:00  Completed
  0.641509              10         0:00:06.059082  IrisDemo_1504832109906  iris_sklearn.py  2017-09-08T00:55:14.739806+00:00  Completed

```
Die **Befehlszeilenschnittstelle** ist ein alternativer Weg, um auf Azure Machine Learning Workbench zuzugreifen.

## <a name="next-steps"></a>Nächste Schritte
Diese Funktionen sind verfügbar, um Sie beim Prozess des Data Science-Experimentierens zu unterstützen.
Wir hoffen, dass Sie diese nützlich finden und freuen uns über Ihr Feedback.
Dies ist nur die erste Implementierung und zahlreiche Erweiterungen sind geplant.
Wir freuen uns darauf, diese kontinuierlich für Azure Machine Learning Workbench bereitzustellen. 
