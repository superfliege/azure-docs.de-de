---
title: Verwenden von MMLSpark Machine Learning | Microsoft-Dokumentation
description: Anleitung zur Verwendung der MMLSpark-Bibliothek mit Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 6714e8ad77693f0cdefe3e40c99153299e1c72d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Verwenden der Microsoft Machine Learning-Bibliothek für Apache Spark

## <a name="introduction"></a>Einführung

Die [Microsoft Machine Learning-Bibliothek für Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) bietet Tools, mit denen Sie problemlos skalierbare Machine Learning-Modelle für große Datasets erstellen können. MMLSpark bietet die Integration von SparkMLearning-Pipelines in das [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK) und [OpenCV](http://www.opencv.org/), sodass Sie Folgendes durchführen können: 
 * Eingang und Vorverarbeitung von Bilddaten
 * Ausstattung von Bildern und Texten mit Features mithilfe vortrainierter Deep Learning-Modelle
 * Trainieren und Bewerten von Klassifizierungs- und Regressionsmodellen per impliziter Ausstattung mit Features.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in dieser Anleitung müssen Sie folgende Vorbereitungen treffen:
- [Installieren der Azure Machine Learning Workbench](quickstart-installation.md)
- [Einrichten des Azure HDInsight Spark-Clusters](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Ausführen des Experiments im Docker-Container

Ihre Azure Machine Learning Workbench wird zur Verwendung von MMLSpark konfiguriert, wenn Sie entweder lokal oder in Remote-VMs Experimente im Docker-Container ausführen. Diese Funktion ermöglicht Ihnen, mühelos Ihre PySpark-Modelle zu debuggen und mit ihnen zu experimentieren, bevor Sie sie entsprechend skaliert in einem Cluster ausführen. 

Erstellen Sie zum Einstieg anhand eines Beispiels ein neues Projekt, und wählen Sie das Katalogbeispiel „MMLSpark on Adult Census“. Wählen Sie „Docker“ als Computekontext aus dem Projektdashboard, und klicken Sie auf „Run“. Azure Machine Learning Workbench erstellt den Docker-Container zum Ausführen des PySpark-Programms und führt dann das Programm aus.

Nach Abschluss der Ausführung können Sie die Ergebnisse in der Ausführungsverlaufsansicht von Azure Machine Learning Workbench anzeigen.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Installieren Sie MMLSpark in einem Azure HDInsight Spark-Cluster.

Um diesen und den folgenden Schritt abzuschließen, müssen Sie zuerst [einen Azure HDInsight Spark-Cluster erstellen](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Standardmäßig installiert Azure Machine Learning Workbench das MMLSpark-Paket in Ihrem Cluster, wenn Sie Ihr Experiment ausführen. Sie können dieses Verhalten steuern und andere Spark-Pakete installieren, indem Sie eine Datei mit dem Namen _aml_config/spark_dependencies.yml_ in Ihrem Projektordner bearbeiten.

```
# Spark configuration properties.
configuration:
  "spark.app.name": "Azure ML Experiment"
  "spark.yarn.maxAppAttempts": 1

repositories:
  - "https://mmlspark.azureedge.net/maven"
packages:
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.7.9"
```

Sie können MMLSpark auch direkt mit einer [Skriptaktion](https://github.com/Azure/mmlspark#hdinsight) in Ihrem HDInsight Spark-Cluster installieren.

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Einrichten des Azure HDInsight Spark-Clusters als Computeziel

Öffnen Sie das CLI-Fenster über Azure Machine Learning Workbench, indem Sie zum Menü „File“ navigieren und auf „Open Command Prompt“ klicken.

Führen Sie im CLI-Fenster folgende Befehle aus:

```
az ml computetarget attach --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> --type cluster
```

```
az ml experiment prepare -c <myhdi>
```

Der Cluster ist jetzt für das Projekt als Computeziel verfügbar.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Führen Sie das Experiment in einem Azure HDInsight Spark-Cluster aus.

Wechseln Sie zurück zum Projektdashboard des Beispiels „MMLSpark on Adult Census“. Wählen Sie Ihren Cluster als Computeziel aus, und klicken Sie auf „Run“.

Azure Machine Learning Workbench übermittelt den Spark-Auftrag an den Cluster. Sie können den Fortschritt überwachen und die Ergebnisse in der Ausführungsverlaufsansicht anzeigen.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur MMLSpark-Bibliothek sowie Beispiele finden Sie unter [MMLSpark-GitHub-Repository](https://github.com/Azure/mmlspark).

*Apache®, Apache Spark und Spark® sind entweder eingetragene Marken oder Marken der Apache Software Foundation in den USA und/oder anderen Ländern.*
