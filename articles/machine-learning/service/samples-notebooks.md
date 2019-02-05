---
title: Jupyter-Beispiel-Notebooks
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie Beispiele für Jupyter-Notebooks finden und verwenden, um den Azure Machine Learning-Dienst in Python zu erkunden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 5ba555ad31545e1ae1aa822ec58b0bd22ef486ac
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295150"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Verwenden von Jupyter-Notebooks zum Erkunden des Azure Machine Learning-Diensts

Zur Vereinfachung haben wir eine Reihe von Jupyter Python-Notebooks entwickelt, die Sie zum Erkunden des Azure Machine Learning-Diensts verwenden können. 

Es wird beschrieben, wie Sie den Dienst mit der Dokumentation auf dieser Website nutzen und diese Notebooks zur Anpassung an Ihre Anforderungen verwenden. 

Verwenden Sie einen der unten stehenden Pfade, um einen Notebook-Server mit diesen Beispielnotebooks auszuführen.  Sobald der Server ausgeführt wird, finden Sie im Ordner **tutorials** die Notebooks für das Tutorial. Zudem können Sie im Ordner **how-to-use-azureml** verschiedene Features erkunden.


## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Azure Notebooks ausprobieren: Kostenlose Jupyter-Notebooks in der Cloud

Der Einstieg in Azure Notebooks ist einfach! Das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) wurde unter [Azure Notebooks](https://notebooks.azure.com/) bereits für Sie installiert und konfiguriert. Die Installation und zukünftige Updates werden automatisch über Azure-Dienste verwaltet.
  
[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Verwenden einer Data Science Virtual Machine (DSVM)

Das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) und der Notebookserver sind für Sie auf einer DSVM bereits installiert und konfiguriert. 

Nach dem [Erstellen einer Data Science Virtual Machine (DSVM)](how-to-configure-environment.md#dsvm) führen Sie auf der DSVM die folgenden Schritte durch, um die Notebooks auszuführen.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]


## <a name="use-your-own-jupyter-notebook-server"></a>Verwenden Ihres eigenen Jupyter Notebook-Servers

Führen Sie diese Schritte aus, um auf Ihrem Computer einen lokalen Jupyter Notebook-Server zu erstellen.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

<a name="automated-ml-setup"></a>

## <a name="automated-machine-learning-setup"></a>Einrichtung für automatisiertes maschinelles Lernen 

_Diese Schritte gelten nur für die Notebooks im Ordner **how-to-use-azureml/automated-machine-learning**._

Sie können zwar die obigen Optionen beliebig verwenden, aber Sie können auch mit der folgenden Anleitung die Umgebung installieren und gleichzeitig einen Arbeitsbereich erstellen. 

1. Installieren Sie [mini-conda](https://conda.io/miniconda.html). Wählen Sie Version 3.7 oder höher. Befolgen Sie die Eingabeaufforderungen, um die Installation durchzuführen. 
   >[!NOTE]
   >Sie können eine vorhandene Conda-Installation verwenden, solange es sich dabei um Version 4.4.10 oder höher handelt. Verwenden Sie den Befehl `conda -V`, um die Version anzuzeigen. Sie können eine Conda-Version mit dem folgenden Befehl aktualisieren: `conda update conda`. Es ist nicht erforderlich, eine spezifische Installation von mini-conda durchzuführen.

1. Laden Sie die Beispielnotebooks von [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) als ZIP-Dateien herunter, und extrahieren Sie den Inhalt in ein lokales Verzeichnis. Die Notebooks für automatisiertes maschinelles Lernen befinden sich im Ordner `how-to-use-azureml/automated-machine-learning`.

1. Richten Sie eine neue Conda-Umgebung ein. 
   1. Öffnen Sie auf Ihrem lokalen Computer eine Conda-Eingabeaufforderung.
   
   1. Navigieren Sie zu den Dateien, die Sie auf Ihrem lokalen Computer extrahiert haben.
   
   1. Öffnen Sie den Ordner **automated-machine-learning**.
   
   1. Führen Sie `automl_setup.cmd` an der Conda-Eingabeaufforderung für Windows bzw. die `.sh`-Datei für Ihr Betriebssystem aus. Die Ausführung kann ca. 10 Minuten dauern.

      Mit dem Setupskript wird Folgendes durchgeführt:
      + Erstellung einer neuen Conda-Umgebung
      + Installation der erforderlichen Pakete
      + Konfiguration des Widgets
      + Starten eines Jupyter-Notebooks
      
   >[!NOTE]
   > Im Skript wird der Name der Conda-Umgebung als optionaler Parameter verwendet. Der Standardname der Conda-Umgebung lautet `azure_automl`. Der genaue Wortlaut des Befehls richtet sich nach dem Betriebssystem. Dies ist nützlich, wenn Sie eine neue Umgebung erstellen oder ein Upgrade auf eine neue Version durchführen. Sie können z.B. „automl_setup.cmd azure_automl_sandbox“ verwenden, um eine Umgebung namens „azure_automl_sandbox“ zu erstellen. 
      
1. Nachdem das Skript abgeschlossen wurde, wird in Ihrem Browser die Jupyter Notebook-Startseite angezeigt.

1. Navigieren Sie zum Speicherort, an dem Sie die Notebooks gespeichert haben. 

1. Öffnen Sie den Ordner „automated-machine-learning“ und dann das Notebook **configuration.ipynb**. 

1. Führen Sie die Zellen in den Notebooks aus, um den Machine Learning Services-Ressourcenanbieter zu registrieren und einen Arbeitsbereich zu erstellen.

Sie können die Notebooks, die auf Ihrem lokalen Computer gespeichert sind, jetzt öffnen und ausführen.


## <a name="next-steps"></a>Nächste Schritte

Erkunden des [GitHub-Notebook-Repositorys für den Azure Machine Learning-Dienst](https://aka.ms/aml-notebooks)

Probieren Sie diese Tutorials aus:
+ [Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst](tutorial-train-models-with-aml.md)

+ [Vorbereiten von Daten für die Regressionsmodellierung](tutorial-data-prep.md)
