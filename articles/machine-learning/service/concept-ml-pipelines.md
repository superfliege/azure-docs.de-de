---
title: Erstellen von Machine Learning-Pipelines – Azure Machine Learning-Dienst
description: In diesem Artikel lernen Sie die Machine Learning-Pipelines, die Sie mit dem Azure Machine Learning SDK für Python erstellen können, und die Vorteile der Verwendung von Pipelines kennen. Machine Learning-Pipelines werden von Data Scientists verwendet, um Workflows für das maschinelle Lernen zu erstellen, zu optimieren und zu verwalten.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 09/24/2018
ms.openlocfilehash: 45aa954d2f85267b2c7c9aa2a7ba04e436765433
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50023925"
---
# <a name="pipelines-and-azure-machine-learning"></a>Pipelines und Azure Machine Learning

In diesem Artikel lernen Sie die Machine Learning-Pipelines, die Sie mit dem Azure Machine Learning SDK für Python erstellen können, und die Vorteile der Verwendung von Pipelines kennen.

## <a name="what-are-machine-learning-pipelines"></a>Einführung in Machine Learning-Pipelines

Machine Learning-Pipelines werden von Data Scientists verwendet, um Workflows für das maschinelle Lernen zu erstellen, zu optimieren und zu verwalten. Eine gewöhnliche Pipeline besteht aus mehreren Schritten, die die folgenden Bereiche abdecken:

+ Datenaufbereitung, z.B. Normalisierung und Transformation
+ Modelltraining, z.B. Hyperparameteroptimierung und Überprüfung
+ Modellimplementierung und -bewertung  

Das folgende Diagramm zeigt eine Beispielpipeline:

[ ![Machine Learning-Pipelines in Azure Machine Learning Service](./media/concept-ml-pipelines/pipelines.png) ] (./media/concept-ml-pipelines/machine-learning-pipelines-big.png#lightbox)

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Erstellen von Pipelines mit Azure Machine Learning

Das [Azure Machine Learning SDK für Python](#the-python-sdk-for-pipelines) kann zum Erstellen von Machine Learning-Pipelines sowie zum Übermitteln und Nachverfolgen einzelner Pipelineausführungen verwendet werden.

Mit Pipelines können Sie Ihren Workflow durch Einfachheit, Geschwindigkeit, Portabilität und Wiederverwendung optimieren. Beim Erstellen von Pipelines mit Azure Machine Learning können Sie sich auf das Wesentliche konzentrieren &mdash; nämlich maschinelles Lernen &mdash; und nicht auf die Infrastruktur.

Da verschiedene Schritte verwendet werden, können Sie nur die Schritte erneut ausführen, die Sie zum Optimieren und Testen Ihres Workflows benötigen. Ein Schritt ist eine Computeeinheit in der Pipeline. Wie in der obigen Abbildung dargestellt, kann die Aufgabe der Datenaufbereitung viele Schritte umfassen, unter anderem Normalisierung, Transformation, Überprüfung und Featurebereitstellung.

Nachdem die Pipeline entworfen wurde, wird ihr Trainingsprozess i.d.R. weiter optimiert. Wenn Sie eine Pipeline erneut ausführen, springt die Ausführung zu den Schritten, die wiederholt werden müssen, z.B. zu einem aktualisierten Trainingsskript, und überspringt, was sich nicht geändert hat. Das gleiche gilt für unveränderte Skripts, die zum Ausführen des Schritts verwendet werden. 

Mit Azure Machine Learning können Sie verschiedene Toolkits und Frameworks wie Microsoft Cognitive Toolkit oder TensorFlow für jeden Pipelineschritt verwenden. Azure koordiniert zwischen den verschiedenen [Computezielen](concept-azure-machine-learning-architecture.md), die Sie verwenden, sodass Ihre Zwischendaten problemlos für die nachgelagerten Computeziele freigegeben werden können. 

## <a name="key-advantages"></a>Hauptvorteile

Die Übersicht enthält die wichtigsten Argumente, die für das Erstellen von Pipelines für die Workflows des maschinellen Lernens sprechen:

|Vorteil|BESCHREIBUNG|
|:-------:|-----------|
|**Unbeaufsichtigte&nbsp;Ausführung**|Planen Sie mehrere Schritte, die zuverlässig und unbeaufsichtigt parallel oder nacheinander ausgeführt werden. Da die Datenaufbereitung und das Modellieren Tage oder Wochen dauern können, können Sie sich nun auf andere Aufgaben konzentrieren, während die Pipeline ausgeführt wird. |
|**Gemischte und verschiedenen Computeressourcen**|Verwenden Sie mehrere Pipelines, die zuverlässig über heterogene und skalierbare Computeressourcen und Speicher koordiniert werden. Einzelne Pipelineschritte können auf verschiedenen Computezielen ausgeführt werden, z.B. HDInsight, Data Science VM mit GPU und Databricks, um die verfügbaren Computeoptionen effizient zu nutzen.|
|**Wiederverwendbarkeit**|Pipelines können für bestimmte Szenarien wie erneutes Training und Batchbewertung als Vorlagen verwendet werden.  Sie können von externen Systemen über einfache REST-Aufrufe ausgelöst werden.|
|**Nachverfolgung und Versionierung**|Anstatt Daten und Ergebnispfade beim Durchlaufen manuell nachzuverfolgen, können Sie mit dem Pipeline-SDK Ihre Datenquellen, -eingaben und -ausgaben explizit benennen und versionieren sowie Skripts und Daten separat verwalten, um die Produktivität zu steigern.|

## <a name="the-python-sdk-for-pipelines"></a>Python SDK für Pipelines

Verwenden Sie Python, um Ihre Machine Learning-Pipelines zu erstellen. Das Azure Machine Learning SDK bietet imperative Konstrukte zum Sequenzieren und Parallelisieren der Pipelineschritte, wenn keine Datenabhängigkeit besteht. Sie können damit in Jupyter Notebook oder in einer anderen bevorzugten IDE interagieren. 

Mit deklarativen Datenabhängigkeiten können Sie Ihre Aufgaben optimieren. Das SDK enthält ein Framework von vorgefertigten Modulen für allgemeine Aufgaben wie Datenübertragung, Computezielerstellung und Modellveröffentlichung. Sie können das Framework erweitern, um eigene Konventionen zu modellieren. Implementieren Sie dafür benutzerdefinierte Schritte, die über Pipelines hinweg wiederverwendbar sind.

Pipelines lassen sich als Vorlagen speichern und auf einem REST-Endpunkt bereitstellen, damit Sie Batchbewertungen und erneute Trainingsaufträge planen können.

In den [Referenzdokumenten für das Python SDK für Pipelines](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und dem Notebook im nächsten Abschnitt erfahren Sie, wie Sie Ihre eigenen Pipelines erstellen.

## <a name="example-notebooks"></a>Beispielnotebooks
 
Das folgende Notebook veranschaulicht die Verwendung von Pipelines mit Azure Machine Learning: [pipeline/pipeline-batch-scoring.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb).
 
Notebook abrufen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
