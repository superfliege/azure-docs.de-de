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
ms.openlocfilehash: cd88fd85ce6d18287c700a54e42b6237a42ea5c9
ms.sourcegitcommit: eea74d11a6d6ea6d187e90e368e70e46b76cd2aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2019
ms.locfileid: "65035379"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Verwenden von Jupyter-Notebooks zum Erkunden des Azure Machine Learning-Diensts

Zur Vereinfachung haben wir eine Reihe von Jupyter Python-Notebooks entwickelt, die Sie zum Erkunden des Azure Machine Learning-Diensts verwenden können. 

Es wird beschrieben, wie Sie den Dienst mit der Dokumentation auf dieser Website nutzen und diese Notebooks zur Anpassung an Ihre Anforderungen verwenden. 

Verwenden Sie einen der unten stehenden Pfade, um einen Notebook-Server mit diesen Beispielnotebooks auszuführen.  Sobald der Server ausgeführt wird, finden Sie im Ordner **tutorials** die Notebooks für das Tutorial. Zudem können Sie im Ordner **how-to-use-azureml** verschiedene Features erkunden.

## <a name="a-managed-cloud-notebook-server"></a>Ein verwalteter cloudbasierter Notebook-Server

Die ersten Schritte mit Ihrem eigenen cloudbasierten Notebook-Server sind ganz einfach. Das Beispielnotebook und das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) werden für Sie installiert und konfiguriert, wenn Sie diese Cloudressource erstellen.  

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Die Beispiele sind auf der Notebook-Webseite verfügbar.

## <a name="a-data-science-virtual-machine-dsvm"></a>Eine Data Science Virtual Machine-Instanz (DSVM)

Das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) und der Notebookserver sind für Sie auf einer DSVM bereits installiert und konfiguriert. 

Nach dem [Erstellen einer Data Science Virtual Machine (DSVM)](how-to-configure-environment.md#dsvm) führen Sie auf der DSVM die folgenden Schritte durch, um die Notebooks auszuführen.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="your-own-jupyter-notebook-server"></a>Ihr eigener Jupyter Notebook-Server

Führen Sie diese Schritte aus, um auf Ihrem Computer einen lokalen Jupyter Notebook-Server zu erstellen.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Mit der Setupanleitung werden die Pakete installiert, die Sie zum Ausführen der Schnellstart- und Tutorial-Notebooks benötigen.  Für andere Beispiel-Notebooks müssen ggf. zusätzliche Komponenten installiert werden.  Weitere Informationen zu diesen Komponenten finden Sie unter [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Installieren des Azure Machine Learning SDK für Python).

## <a name="azure-notebooks"></a>Azure-Notebooks

Die Beispielnotebooks und das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk) wurden unter [Azure Notebooks](https://notebooks.azure.com/) bereits für Sie installiert und konfiguriert. Die Installation und zukünftige Updates werden automatisch über Azure-Dienste verwaltet.

Verwenden Sie das [Azure-Portal](https://portal.azure.com), um das Arbeiten mit Azure Notebooks zu beginnen.  Öffnen Sie Ihren Arbeitsbereich, und wählen Sie im Abschnitt **Übersicht** die Option **Erste Schritte in Azure Notebooks** aus.

## <a name="next-steps"></a>Nächste Schritte

+ Erkunden der Beispielnotebooks für Azure Machine Learning Service in diesem GitHub-Repository: https://aka.ms/aml-notebooks

Probieren Sie diese Tutorials aus:
+ [Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst](tutorial-train-models-with-aml.md)

+ [Vorbereiten von Daten für die Regressionsmodellierung](tutorial-data-prep.md)