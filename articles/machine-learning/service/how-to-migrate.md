---
title: Migrieren zum Azure Machine Learning Service
description: Erfahren Sie, wie Sie von einer früheren Version aus ein Upgrade oder eine Migration zur aktuellen Version von Azure Machine Learning Service durchführen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: haining
author: haining
ms.date: 09/24/2018
ms.openlocfilehash: e2b3545c020f41f25f19843eab158cfb1b419164
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253447"
---
# <a name="migrate-to-the-latest-version-of-azure-machine-learning-service"></a>Migrieren zur neuesten Version des Azure Machine Learning Services 

**Wenn Sie die Workbench-Anwendung (Vorschau) installiert haben und/oder über Experimentier- und Modellverwaltungskonten (Vorschau) verfügen, verwenden Sie die Informationen in diesem Artikel, um zur neuesten Version zu migrieren.**  Wenn Sie die Vorschauversion von Workbench nicht installiert haben und weder über Experimentier- noch über Modellverwaltungskonten verfügen, brauchen Sie nichts zu migrieren.

## <a name="what-can-i-migrate"></a>Was kann ich migrieren?
Die meisten in der ersten Vorschauversion von Azure Machine Learning Service erstellten Artefakte sind in Ihrem eigenen lokalen oder in Cloudspeicher gespeichert. Diese Artefakte verschwinden nicht. Um zu migrieren, registrieren Sie die Artefakte erneut beim aktualisierten Azure Machine Learning Service. 

In der folgenden Tabelle und im Artikel wird erläutert, was Sie vor oder nach dem Umstieg auf die neueste Version von Azure Machine Learning Service mit Ihren vorhandenen Posten und Ressourcen tun können. Ferner können Sie die Vorversion und Ihre Posten noch eine Zeit lang weiter verwenden ([siehe dazu die Zeitachse zur Umstellungsunterstützung](overview-what-happened-to-workbench.md#timeline)).

|Posten oder Ressource aus der alten Version|Migration möglich?|Aktionen|
|-----------------|:-------------:|-------------|
|Modelle für maschinelles Lernen (als lokale Dateien)|JA|None (Keine): Funktionieren wie zuvor.|
|Modellabhängigkeiten und Schemas (als lokale Dateien)|JA|None (Keine): Funktionieren wie zuvor.|
|Projekte|JA|[Fügen Sie den lokalen Ordner an den neuen Arbeitsbereich an](#projects).|
|Ausführungsverläufe|Nein |Noch eine Zeit lang [herunterladbar](#history).|
|Dateien für die Vorbereitung von Daten|Nein |[Vorbereiten von Datasets beliebiger Größe](#dataprep) für die Modellierung mithilfe des neuen Azure Machine Learning Data Prep SDKs, oder verwenden Sie Azure Databricks.|
|Computeziele|Nein |Registrieren Sie sie im neuen Arbeitsbereich.|
|Registrierte Modelle|Nein |Registrieren Sie das Modell unter einem neuen Arbeitsbereich erneut.|
|Registrierte Manifeste|Nein |None (Keine): Das Konzept von Manifesten kommt im neuen Arbeitsbereich nicht mehr zum Einsatz.|
|Registrierte Bilder|Nein |Erstellen Sie das Docker-Image zur Bereitstellung unter einem neuen Arbeitsbereich neu.|
|Bereitgestellte Webdienste|Nein |None (Keine): Sie funktionieren immer noch wie bisher, <br/>oder [stellen Sie sie erneut mit der neuesten Version bereit](#services).|
|Experimentier- <br/>und Modellverwaltungskonten|Nein |Stattdessen: [Erstellen Sie einen Arbeitsbereich](#resources).|
|Machine Learning-CLI und -SDK|Nein |Verwenden Sie für neue Arbeit das neue [CLI](reference-azure-machine-learning-cli.md) und [SDK](https://aka.ms/aml-sdk).|


Möchten Sie mehr dazu erfahren, [was sich in dieser Version geändert hat](overview-what-happened-to-workbench.md)?

>[!Warning]
>Dieser Artikel ist nicht für Benutzer von Azure Machine Learning Studio bestimmt. Er richtet sich an Kunden von Azure Machine Learning Service, die die Workbench-Anwendung (Vorschau) installiert haben und/oder über Experimentier- und Modellverwaltungskonten (Vorschau) verfügen.

<a name="resources"></a>

## <a name="azure-resources"></a>Azure-Ressourcen

Ressourcen wie Ihre Experimentierkonten, Modellverwaltungskonten und Compute-Umgebungen für maschinelles Lernen können nicht zur neuesten Version des Azure Machine Learning Services migriert werden. Informationen über die verbleibende Zeit, in der Ihre Posten noch funktionieren, finden Sie in der [Zeitachse](overview-what-happened-to-workbench.md#timeline).

Ihre ersten Schritte in der neuesten Version bestehen darin, im [Azure-Portal](quickstart-get-started.md) einen Azure Machine Learning Services-Arbeitsbereich zu erstellen. Das Arbeitsbereichdashboard im Portal wird nur in den Browsern Edge, Chrome und Firefox unterstützt.

Dieser neue Arbeitsbereich ist die Dienstressource der obersten Ebene und ermöglicht Ihnen, alle aktuellen Features des Azure Machine Learning Service zu nutzen. Weitere Informationen zu diesem [Arbeitsbereich und zur Architektur](concept-azure-machine-learning-architecture.md).

<a name="projects"></a>

## <a name="projects"></a>Projekte

Statt Ihre Projekte in einem Arbeitsbereich in der Cloud zu speichern, stellen Projekte in der aktuellen Version jetzt Verzeichnisse auf Ihrem lokalen Computer dar. [Diagramm der aktuellen Architektur](concept-azure-machine-learning-architecture.md). 

Um das lokale Verzeichnis, das Ihre Dateien und Skripts enthält, weiterhin zu verwenden, geben Sie den Namen des Verzeichnisses im Python-Befehl ['experiment.submit'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) ein, oder verwenden Sie am CLI den Befehl 'az ml project attach'.

Beispiel: 
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

<a name="services"></a>

## <a name="deployed-web-services"></a>Bereitgestellte Webdienste

Um Webdienste zu migrieren, stellen Sie Ihre Modelle erneut mithilfe des neuen SDKs oder CLIs an den neuen Bereitstellungszielen bereit. Ihre ursprüngliche Bewertungsdatei, Dateien mit Modelldateiabhängigkeiten, die Umgebungsdatei und Schemadateien brauchen nicht geändert zu werden. 

In der aktuellen Version werden Modelle als Webdienste in [Azure Container Instances](how-to-deploy-to-aci.md) (ACI) oder [Azure Kubernetes Service](how-to-deploy-to-aks.md)-Clustern (AKS) bereitgestellt. 

Weitere Informationen finden Sie in diesen Artikeln:
+ [Bereitstellen für ACI](how-to-deploy-to-aci.md)
+ [Bereitstellen für AKS](how-to-deploy-to-aks.md)
+ [Tutorial: Bereitstellen von Modellen im Azure Machine Learning Service](tutorial-deploy-models-with-aml.md)

Wenn die [Unterstützung für das bisherige CLI endet](overview-what-happened-to-workbench.md#timeline), können Sie die Webdienste, die Sie ursprünglich mit Ihrem Modellverwaltungskonto bereitgestellt haben, nicht mehr verwalten. Diese Webdienste funktionieren jedoch weiterhin, so lange der Azure Container Service (ACS) noch unterstützt wird.

<a name="history"></a>

## <a name="run-history-records"></a>Datensätze des Ausführungsverlaufs

Sie können ihre aktuellen Ausführungsverläufe zwar nicht mehr unter dem alten Arbeitsbereich hinzufügen, aber Sie können die vorhandenen Verläufe mithilfe des bisherigen CLIs exportieren. Wenn die [Unterstützung für das bisherige CLI endet](overview-what-happened-to-workbench.md#timeline), können Sie diese Ausführungsverläufe nicht mehr exportieren.

Beginnen Sie, Ihre Modelle mithilfe des neuen CLIs und SDKs zu trainieren und ihre Ausführungsverläufe mit diesen nachzuverfolgen. Wie das geht, können Sie mit dem [Tutorial: Trainieren von Modellen mit Azure Machine Learning Service](tutorial-train-models-with-aml.md) lernen.

So exportieren Sie den Ausführungsverlauf mit dem früheren CLI:

```azurecli
#list all runs
az ml history list

#get details about a particular run
az ml history info

# download all artifacts of a run
az ml history download
```

<a name="dataprep"></a>

## <a name="data-preparation-files"></a>Dateien zur Datenvorbereitung
Dateien zur Datenvorbereitung sind ohne die Workbench nicht portierbar. Trotzdem können Sie Datasets beliebiger Größe mithilfe des neuen Azure Machine Learning Data Prep SDK für die Modellierung vorbereiten oder für große Datasets Azure Databricks verwenden.  [Informationen zum Abrufen des Data Prep SDKs](how-to-data-prep.md). 

## <a name="next-steps"></a>Nächste Schritte

Eine Schnellstartanleitung, in der Sie erfahren, wie ein Arbeitsbereich und ein Projekt erstellt, ein Skript ausgeführt und der Ausführungsverlauf des Skripts mit der neuesten Version von Azure Machine Learning Service untersucht werden, finden Sie unter [Erste Schritte mit Azure Machine Learning Service](quickstart-get-started.md).

Einen detaillierteren Einblick in diesen Workflow erhalten Sie im vollständigen Tutorial, das detaillierte Schritte für das Trainieren und Bereitstellen von Modellen mit Azure Machine Learning Service enthält. 

> [!div class="nextstepaction"]
> [Tutorial: Trainieren und Bereitstellen von Modellen](tutorial-train-models-with-aml.md)
