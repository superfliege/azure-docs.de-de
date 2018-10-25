---
title: Bekannte Probleme und Problembehandlung für Azure Machine Learning Service
description: Erfahren Sie, wie Sie mit bekannten Problemen umgehen sowie Probleme umgehen und behandeln.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 02cee5a3e088c919ec94aee6f46ef6f428b9bb48
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249416"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekannte Probleme und Problembehandlung für Azure Machine Learning Service
 
Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu ermitteln und zu beheben, die beim Verwenden von Azure Machine Learning Service auftreten können. 

## <a name="sdk-installation-issues"></a>SDK-Installationsprobleme

**Fehlermeldung: 'PyYAML' kann nicht deinstalliert werden**. 

Bei PyYAML handelt es sich um ein mit distutils installiertes Projekt. Daher können wir im Fall einer teilweisen Deinstallation nicht genau bestimmen, welche Dateien zu ihm gehören. Verwenden Sie diesen Befehl, um die Installation des SDKs fortzusetzen und diesen Fehler zu ignorieren:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="image-building-failure"></a>Fehler bei der Image-Erstellung

Fehler bei der Image-Erstellung beim Bereitstellen des Webdiensts. Die Problemumgehung besteht darin „pynacl==1.2.1“ als pip-Anhängigkeit zur Conda-Datei für die Imagekonfiguration hinzuzufügen.  

## <a name="pipelines"></a>Pipelines
Beim mehrfachen Aufrufen von PythonScriptStep in einer Zeile ohne Wechsel von Skript oder Parametern tritt ein Fehler auf. Die Problemumgehung besteht im erneuten Erstellen des PipelineData-Objekts.

## <a name="fpgas"></a>FPGAs
Um Modelle auf FPGAs bereitzustellen, müssen Sie zuerst ein FPGA-Kontingent anfordern und dessen Genehmigung abwarten. Füllen Sie das Formular zur Kontingentanforderung aus, um Zugriff anzufordern: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Probleme mit Databricks und Azure Machine Learning:

1. Empfehlung für Databricks-Cluster:
   
   Erstellen Sie Ihr Azure Databricks-Cluster als Version 4.x mit Python 3. Es wird empfohlen, einen Cluster für hohe Parallelität zu verwenden.
 
1. Die Installation des Azure Machine Learning SDK (ML SDK) schlägt in Databricks fehl, wenn mehrere Pakete installiert werden.

   Einige Pakete, z.B. `psutil upgrade libs`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie beim Installieren der Pakete die Bibliotheksversion ein. Dieses Problem betrifft Databricks – nicht das ML SDK. Beispiel:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Die Protkolldateien finden Sie hier:

## <a name="resource-quotas"></a>Ressourcenkontingente

Erfahren Sie mehr über die [Ressourcenkontingente](how-to-manage-quotas.md), die Ihnen im Rahmen von Azure Machine Learning begegnen können.

## <a name="get-more-support"></a>Weitere Unterstützung

Sie können Supportanfragen stellen und Hilfe vom technischen Support, in Foren usw. erhalten. [Weitere Informationen](support-for-aml-services.md)
