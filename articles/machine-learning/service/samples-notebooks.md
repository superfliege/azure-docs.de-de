---
title: Jupyter-Beispiel-Notebooks
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie Beispiele für Jupyter-Notebooks finden und verwenden, um den Azure Machine Learning-Dienst in Python zu erkunden.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6befe3a3fee80dd65fd3ac5be241c558707224e6
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811093"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Verwenden von Jupyter-Notebooks zum Erkunden des Azure Machine Learning-Diensts


Zur Vereinfachung haben wir eine Reihe von Jupyter Python-Notebooks entwickelt, die Sie zum Erkunden des Azure Machine Learning-Diensts verwenden können. 

Es wird beschrieben, wie Sie den Dienst mit der Dokumentation auf dieser Website nutzen und diese Notebooks zur Anpassung an Ihre Anforderungen verwenden. 

## <a name="prerequisite"></a>Voraussetzung

Arbeiten Sie die [Azure Machine Learning-Python-Schnellstartanleitung](quickstart-get-started.md) durch, um einen Arbeitsbereich zu erstellen und Azure Notebooks zu starten.

## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Azure Notebooks ausprobieren: Kostenlose Jupyter-Notebooks in der Cloud

Der Einstieg in Azure Notebooks ist einfach! Das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) wurde unter [Azure Notebooks](https://notebooks.azure.com/) bereits für Sie installiert und konfiguriert. Die Installation und zukünftige Updates werden automatisch über Azure-Dienste verwaltet.
  
+ Führen Sie die **wichtigsten Notebooks des Tutorials** wie folgt aus:
  1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com/).
    
  1. Suchen Sie in der Bibliothek **Erste Schritte** nach dem Ordner **tutorials**, den Sie während der Schnellstartanleitung zur Erfüllung der Voraussetzungen erstellt haben.
    
  1. Öffnen Sie das Notebook, das Sie ausführen möchten.
    
+ Führen Sie **andere Notebooks** wie folgt aus:

  1. [Importieren Sie die Beispielnotebooks](https://aka.ms/aml-clone-azure-notebooks) in Azure Notebooks.

  1. Fügen Sie der Bibliothek eine Datei für die Arbeitsbereichskonfiguration hinzu, indem Sie eine dieser Methoden verwenden:
     + Kopieren Sie die Datei **config.json** aus der Bibliothek **Erste Schritte** in die neue geklonte Bibliothek.

     + Erstellen Sie einen neuen Arbeitsbereich, indem Sie den in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) enthaltenen Code verwenden.
    
  1. Öffnen Sie das Notebook, das Sie ausführen möchten.     


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Verwenden einer Data Science Virtual Machine (DSVM)

Das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) und der Notebookserver sind für Sie auf einer DSVM bereits installiert und konfiguriert. Verwenden Sie diese Schritte, um die Notebooks auszuführen.

1. [Erstellen einer DSVM](how-to-configure-environment.md#dsvm).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

1. Fügen Sie der Bibliothek eine Datei für die Arbeitsbereichskonfiguration hinzu, indem Sie eine dieser Methoden verwenden:
    * Kopieren Sie die Datei **aml_config\config.json**, die Sie anhand der Schnellstartanleitung für die Erfüllung der Voraussetzungen erstellt haben, in das geklonte Verzeichnis.

    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) enthaltenen Code verwenden.

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

## <a name="use-your-own-jupyter-notebook-server"></a>Verwenden Ihres eigenen Jupyter Notebook-Servers

Führen Sie diese Schritte aus, um auf Ihrem Computer einen lokalen Jupyter Notebook-Server zu erstellen.

1. Stellen Sie sicher, dass Sie die Schnellstartanleitung für die Erfüllung der Voraussetzungen durchgearbeitet haben, um die Azure Machine Learning SDKs zu installieren.

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

1. Fügen Sie der Bibliothek eine Datei für die Arbeitsbereichskonfiguration hinzu, indem Sie eine dieser Methoden verwenden:
    * Kopieren Sie die Datei **aml_config\config.json**, die Sie anhand der Schnellstartanleitung für die Erfüllung der Voraussetzungen erstellt haben, in das geklonte Verzeichnis.
    
    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) enthaltenen Code verwenden.

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

1. Navigieren Sie zum Ordner mit dem Notebook.

1. Öffnen Sie das Notebook.

<a name="auto"></a>

## <a name="automated-ml-setup"></a>Automatisiertes ML-Setup 

**Diese Schritte gelten nur für die Notebooks im Ordner `automated-machine-learning`.**

Sie können zwar die obigen Optionen beliebig verwenden, aber Sie können auch mit der folgenden Anleitung die Umgebung installieren und gleichzeitig einen Arbeitsbereich erstellen. 

1. Installieren Sie [mini-conda](https://conda.io/miniconda.html). Wählen Sie Version 3.7 oder höher. Befolgen Sie die Eingabeaufforderungen, um die Installation durchzuführen. 
   >[!NOTE]
   >Sie können eine vorhandene Conda-Installation verwenden, solange es sich dabei um Version 4.4.10 oder höher handelt. Verwenden Sie den Befehl `conda -V`, um die Version anzuzeigen. Sie können eine Conda-Version mit dem folgenden Befehl aktualisieren: `conda update conda`. Es ist nicht erforderlich, eine spezifische Installation von mini-conda durchzuführen.

1. Laden Sie die Beispielnotebooks von [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) als ZIP-Dateien herunter, und extrahieren Sie den Inhalt in ein lokales Verzeichnis. Die Notebooks für automatisiertes maschinelles Lernen befinden sich im Ordner `how-to-use-azureml/automated-machine-learning`.

1. Richten Sie eine neue Conda-Umgebung ein. 
   1. Öffnen Sie auf Ihrem lokalen Computer eine Conda-Eingabeaufforderung.
   
   1. Navigieren Sie zu den Dateien, die Sie auf Ihrem lokalen Computer extrahiert haben.
   
   1. Öffnen Sie den Ordner `automated-machine-learning`.
   
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

1. Öffnen Sie den Ordner „automated-machine-learning“ und dann das Notebook `configuration.ipynb`. 

1. Führen Sie die Zellen in den Notebooks aus, um den Machine Learning Services-Ressourcenanbieter zu registrieren und einen Arbeitsbereich zu erstellen.

Sie können die Notebooks, die auf Ihrem lokalen Computer gespeichert sind, jetzt öffnen und ausführen.


## <a name="next-steps"></a>Nächste Schritte

Erkunden des [GitHub-Notebook-Repositorys für den Azure Machine Learning-Dienst](https://aka.ms/aml-notebooks)

Probieren Sie diese Tutorials aus:
+ [Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst](tutorial-train-models-with-aml.md)

+ [Vorbereiten von Daten für die Regressionsmodellierung](tutorial-data-prep.md)
