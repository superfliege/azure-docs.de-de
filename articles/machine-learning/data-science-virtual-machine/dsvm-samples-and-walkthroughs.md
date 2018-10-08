---
title: Beispiele und exemplarische Vorgehensweisen für die Data Science-VM – Azure | Microsoft-Dokumentation
description: Beispiele und exemplarische Vorgehensweisen für die Data Science-VM.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: a1f15b805d2f27152d9ba85608ce0dc1d1aac21e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392567"
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Beispiele auf Data Science-VMs (DSVMs)

Die DSVMs enthalten eine umfassende Gruppe von Beispielcode sowohl in Form von Jupyter-Notebooks als auch Skripts in Sprachen wie Python und R.    
> [!NOTE]
> Informationen zum Ausführen von Jupyter-Notebooks auf Ihrer DSVM finden Sie im Abschnitt [Zugreifen auf Jupyter](#access-jupyter).

## <a name="quick-reference-of-samples"></a>Kurzübersicht über die Beispiele
| Kategorie | BESCHREIBUNG | Standorte |
| ------------- | ------------- | ------------- |
| Sprache: **R**  | Beispiele in **R** zur Erläuterung von Szenarien wie dem Herstellen einer Verbindung mit Azure-Clouddatenspeichern, dem Vergleichen von Open Source R und Microsoft R sowie dem Operationalisieren von Modellen in Microsoft R Server oder SQL Server. <br/> [Screenshot](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Sprache: **Python**  | Beispiele in **Python** zur Erläuterung von Szenarien wie dem Herstellen einer Verbindung mit Azure-Clouddatenspeichern und dem Verwenden von **Azure Machine Learning**.  <br/> [Screenshot](#python-language) | <br/>`~notebooks` <br/><br/>|
| Sprache: **Julia**  | Beispiele in **Julia**, die unter anderem das Zeichnen in Julia, Deep Learning in Julia und das Aufrufen von C und Python über Julia veranschaulichen. <br/> [Screenshot](#julia-language) |<br/> **Windows:**<br/> `~notebooks/Julia_notebooks`<br/><br/> **Linux:**<br/> `~notebooks/julia`<br/><br/> |
| **Azure Machine Learning** (AzureML)  | Erstellen Sie Machine Learning- und Deep Learning-Modelle mit dem **Azure Machine Learning**-Dienst, und stellen sie Modelle in einem beliebigen Dienst bereit. Nutzen Sie Funktionen wie automatisiertes ML, intelligente Hyperparameteroptimierung, Modellverwaltung und verteiltes Training. <br/> [Screenshot](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| **PyTorch**-Notebooks  | Deep Learning-Beispiele mit **PyTorch**-basierten neuronalen Netzwerken. Es stehen verschiedene Notebooks zur Verfügung – von einfachen bis hin zu komplexen Szenarien.  <br/> [Screenshot](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| **TensorFlow**  | Mehrere verschiedene Beispiele und Techniken für neuronale Netzwerke, die mithilfe des **TensorFlow**-Frameworks implementiert werden. <br/> [Screenshot](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| **CNTK** <br/> (Microsoft Cognitive Toolkit)  | Deep Learning-Beispiele des Cognitive Toolkit-Teams von Microsoft.  <br/> [Screenshot](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> **Linux:**<br/> `~notebooks/CNTK`<br/> <br/>|
| **Caffe2** | Deep Learning-Beispiele mit **Caffe2**-basierten neuronalen Netzwerken. Benutzer können sich anhand von mehreren Notebooks mit Caffe2 und der effektiven Nutzung dieses Frameworks vertraut machen. Dazu stehen Beispiele wie Bildvorverarbeitung, Dataseterstellung, Regression und Verwendung bereits trainierter Modelle zur Verfügung. <br/> [Screenshot](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| **H2O**   | Python-basierte Beispiele mit **H2O** für zahlreiche praxisorientierte Aufgabenstellungen. <br/> [Screenshot](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| Sprache: **SparkML**  | Beispiel mit Features und Funktionen des Spark **MLlib** Toolkits über **pySpark** und **MMLSpark – Microsoft Machine Learning Library für Apache Spark** in **Apache Spark 2.x**.  <br/> [Screenshot](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| **XGBoost** | Standardmäßige Machine Learning-Beispiele in **XGBoost** für Szenarien wie Klassifizierung, Regression und dergleichen. <br/> [Screenshot](#xgboost) | <br/>**Windows:**<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Zugreifen auf Jupyter 

Klicken Sie zum Aufrufen von Jupyter auf das Symbol `Jupyter` (auf dem Desktop oder im Anwendungsmenü). Sie können auch remote unter Linux-Editionen von DSVM über einen Webbrowser auf Jupyter zugreifen, indem Sie unter Ubuntu **`https://<Full Domain Name or IP Address of the DSVM>:8000`** aufrufen.

Der Screenshot zeigt, wie Sie die Ausnahme hinzufügen und den Jupyter-Zugriff über den Browser ermöglichen.


![Aktivieren der Jupyter-Ausnahme](./media/ubuntu-jupyter-exception.png)


Melden Sie sich mit dem Kennwort an, das Sie auch für die Anmeldung bei der DSVM verwenden.
<br/>

**Jupyter-Startseite**
<br/>![Jupyter-Startseite](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R 
<br/>![Beispiele für R](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python
<br/>![Beispiele für Python](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Julia 
<br/>![Beispiele für Julia](./media/julia-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![Beispiele für AzurekML](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![PyTorch-Beispiele](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![Beispiele für TensorFlow](./media/tensorflow-samples.png)<br/>


## <a name="cntk"></a>CNTK 
<br/>![CNTK-Beispiele](./media/cntk-samples.png)<br/>


## <a name="caffe2"></a>Caffe2 
<br/>![Beispiele für Caffe2](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Beispiele für H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![Beispiele für SparkML](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![Beispiele für XGBoost](./media/xgboost-samples.png)<br/>

