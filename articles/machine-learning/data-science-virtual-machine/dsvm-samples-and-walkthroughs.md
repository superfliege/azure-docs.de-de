---
title: "Beispiele und exemplarische Vorgehensweisen für die Data Science-VM – Azure | Microsoft-Dokumentation"
description: "Beispiele und exemplarische Vorgehensweisen für die Data Science-VM."
keywords: "Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 89bb5d255db9ab266d04169a3101e2b694236029
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Beispiele auf Data Science-VMs (DSVMs)

Auf den DSVMs stehen vorgefertigte Beispiele in Form von Jupyter-Notebooks sowie einige nicht auf Jupyter basierende Beispiele zur Verfügung. Klicken Sie zum Aufrufen von Jupyter auf das Symbol `Jupyter` (auf dem Desktop oder im Anwendungsmenü).  
> [!NOTE]
> Informationen zum Aktivieren von Jupyter-Notebooks auf Ihrer DSVM finden Sie im Abschnitt [Zugreifen auf Jupyter](#access-jupyter).

## <a name="quick-reference-of-samples"></a>Kurzübersicht über die Beispiele
| Kategorie | Beschreibung | Standorte |
| ------------- | ------------- | ------------- |
| Sprache: **R**  | Beispiele in **R** zur Erläuterung von Szenarien wie dem Herstellen einer Verbindung mit Azure-Clouddatenspeichern, dem Vergleichen von Open Source R und Microsoft R sowie dem Operationalisieren von Modellen in Microsoft R Server oder SQL Server. <br/> [Screenshot](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Sprache: **Python**  | Beispiele in **Python** zur Erläuterung von Szenarien wie dem Herstellen einer Verbindung mit Azure-Clouddatenspeichern und dem Verwenden von **Azure Machine Learning**.  <br/> [Screenshot](#python-language) | <br/>`~notebooks` <br/><br/>|
| Sprache: **Julia**  | Beispiele in **Julia**, die unter anderem das Zeichnen in Julia, Deep Learning in Julia und das Aufrufen von C und Python über Julia veranschaulichen. <br/> [Screenshot](#julia-language) |<br/> **Windows:**<br/> `~notebooks/Julia_notebooks`<br/><br/>`~notebooks`<br/><br/> **Linux:**<br/> `~notebooks/julia`<br/><br/> |
| **CNTK** <br/> (Microsoft Cognitive Toolkit)  | Deep Learning-Beispiele des Cognitive Toolkit-Teams von Microsoft.  <br/> [Screenshot](#cntk) | <br/>**Windows:**<br/> `~notebooks/CNTK/Tutorials`<br/><br/>`~/samples/CNTK-Samples-2-0/Examples`<br/><br/> **Linux:**<br/> `~notebooks/CNTK`<br/> <br/>|
| **MXNet**-Notebooks  | Deep Learning-Beispiele mit **MXNet**-basierten neuronalen Netzwerken. Es stehen verschiedene Notebooks zur Verfügung – von einfachen bis hin zu komplexen Szenarien.  <br/> [Screenshot](#mxnet) | <br/>`~notebooks/mxnet`<br/> <br/>|
| **Azure Machine Learning** (AzureML)  | Interaktion mit **Azure Machine Learning** Studio und Erstellung von Webdienst-Endpunkten auf der Grundlage lokal trainierter Modelle für cloudbasierte Bewertungsworkflows. <br/> [Screenshot](#azureml) | <br/>`~notebooks/azureml`<br/> <br/>|
| **Caffe2** | Deep Learning-Beispiele mit **Caffe2**-basierten neuronalen Netzwerken. Benutzer können sich anhand von mehreren Notebooks mit Caffe2 und der effektiven Nutzung dieses Frameworks vertraut machen. Dazu stehen Beispiele wie Bildvorverarbeitung, Dataseterstellung, Regression und Verwendung bereits trainierter Modelle zur Verfügung. <br/> [Screenshot](#caffe2) | <br/>`~notebooks/caffe2`<br/><br/> |
| **H2O**   | Python-basierte Beispiele mit **H2O** für zahlreiche praxisorientierte Aufgabenstellungen. <br/> [Screenshot](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| Sprache: **SparkML**  | Beispiel mit Verwendung von Features und Funktionen des **MLlib**-Toolkits von Spark über **pySpark 2.0** in **Apache Spark 2.0**.  <br/> [Screenshot](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/><br/> |
| Sprache: **MMLSpark**  | Verschiedenste Beispiele mit **MMLSpark (Microsoft Machine Learning für Apache Spark)** – einem Framework mit einer Reihe von Deep Learning- und Data Science-Tools für **Apache Spark**. <br/> [Screenshot](#sparkml) | <br/>`~notebooks/MMLSpark`<br/><br/> |
| **TensorFlow**  | Mehrere verschiedene Beispiele und Techniken für neuronale Netzwerke, die mithilfe des **TensorFlow**-Frameworks implementiert werden. <br/> [Screenshot](#tensorflow) | <br/>`~notebooks/tensorflow`<br/><br/> |
| **XGBoost** | Standardmäßige Machine Learning-Beispiele in **XGBoost** für Szenarien wie Klassifizierung, Regression und dergleichen. <br/> [Screenshot](#xgboost) | <br/>`~samples/xgboost/demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Zugreifen auf Jupyter 

Navigieren Sie zum Aufrufen der Jupyter-Startseite zu **`https://localhost:9999`** (Windows) bzw. zu **`https://localhost:8000`** (Ubuntu).


### <a name="enabling-jupyter-access-from-browser"></a>Ermöglichen des Jupyter-Zugriffs über den Browser

**DSVM unter Windows**

Führen Sie über den Desktop **`Jupyter SetPassword`** aus, und folgen Sie den Anweisungen, um Ihr Kennwort festzulegen/zurückzusetzen und den Jupyter-Prozess zu starten. 
<br/>![Aktivieren der Jupyter-Ausnahme](./media/jupyter-setpassword.png)<br/>
Sie können die Jupyter-Startseite in Ihrem Browser unter **`https://localhost:9999`** aufrufen, nachdem der Jupyter-Prozesses auf dem virtuellen Computer erfolgreich gestartet wurde. Der Screenshot zeigt, wie Sie die Ausnahme hinzufügen und den Jupyter-Zugriff über den Browser ermöglichen.
<br/>![Aktivieren der Jupyter-Ausnahme](./media/windows-jupyter-exception.png)<br/>
Melden Sie sich mit dem neuen Kennwort an, das Sie soeben festgelegt haben.
<br/>
**DSVM unter Linux**

Die Jupyter-Startseite können Sie im Browser Ihres virtuellen Computers unter **`https://localhost:8000`** aufrufen. Der Screenshot zeigt, wie Sie die Ausnahme hinzufügen und den Jupyter-Zugriff über den Browser ermöglichen.
<br/>![Aktivieren der Jupyter-Ausnahme](./media/ubuntu-jupyter-exception.png)<br/>
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

## <a name="cntk"></a>CNTK 
<br/>![Beispiele für CNTK](./media/cntk-samples2.png)<br/>
<br/>![Beispiele für CNTK](./media/cntk-samples.png)<br/>

## <a name="mxnet"></a>MXNet
<br/>![Beispiele für MXNet](./media/mxnet-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![Beispiele für AzurekML](./media/azureml-samples.png)<br/>

## <a name="caffe2"></a>Caffe2 
<br/>![Beispiele für Caffe2](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Beispiele für H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![Beispiele für SparkML](./media/sparkml-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![Beispiele für TensorFlow](./media/tensorflow-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![Beispiele für XGBoost](./media/xgboost-samples.png)<br/>

