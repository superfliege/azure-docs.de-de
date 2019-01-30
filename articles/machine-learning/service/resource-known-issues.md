---
title: Bekannte Probleme und Problembehandlung
titleSuffix: Azure Machine Learning service
description: Hier finden Sie eine Liste mit bekannten Problemen, Problemumgehungen und Problembehandlungsinformationen für Azure Machine Learning Service.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 5634a1aae32b3e9895bf5c5b72837f29223bca27
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381824"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekannte Probleme und Problembehandlung für Azure Machine Learning Service
 
Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu ermitteln und zu beheben, die beim Verwenden von Azure Machine Learning Service auftreten können. 

## <a name="sdk-installation-issues"></a>SDK-Installationsprobleme

**Fehlermeldung: 'PyYAML' kann nicht deinstalliert werden.** 

Azure Machine Learning SDK für Python: Bei PyYAML handelt es sich um ein mit distutils installiertes Projekt. Daher können wir im Fall einer teilweisen Deinstallation nicht genau bestimmen, welche Dateien zu ihm gehören. Verwenden Sie diesen Befehl, um die Installation des SDKs fortzusetzen und diesen Fehler zu ignorieren:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problem beim Erstellen von Azure Machine Learning Compute

In seltenen Fällen kann es vorkommen, dass Benutzer, die vor dem allgemein verfügbaren Release ihren Azure Machine Learning-Arbeitsbereich über das Azure-Portal erstellt haben, Azure Machine Learning Compute nicht in diesem Arbeitsbereich erstellen können. In einem solchen Fall können Sie entweder eine Supportanfrage für den Dienst erstellen oder über das Portal oder das SDK einen neuen Arbeitsbereich erstellen und die Blockierung dadurch umgehend aufheben. 

## <a name="image-building-failure"></a>Fehler bei der Image-Erstellung

Fehler bei der Image-Erstellung beim Bereitstellen des Webdiensts. Die Problemumgehung besteht darin „pynacl==1.2.1“ als pip-Anhängigkeit zur Conda-Datei für die Imagekonfiguration hinzuzufügen.  

## <a name="deployment-failure"></a>Fehler bei der Bereitstellung

Wenn „DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>“ angezeigt wird, ändern Sie die SKU für virtuelle Computer in Ihrer Bereitstellung in Varianten mit mehr Arbeitsspeicher.

## <a name="fpgas"></a>FPGAs
Um Modelle auf FPGAs bereitzustellen, müssen Sie zuerst ein FPGA-Kontingent anfordern und dessen Genehmigung abwarten. Füllen Sie das Formular zur Kontingentanforderung aus, um Zugriff anzufordern: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Probleme mit Databricks und Azure Machine Learning:
 
1. Die Installation des Azure Machine Learning SDK (ML SDK) schlägt in Databricks fehl, wenn mehrere Pakete installiert werden.

   Einige Pakete, z.B. `psutil`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie beim Installieren der Pakete die Bibliotheksversion ein. Dieses Problem hängt mit Databricks und nicht mit dem Azure ML SDK zusammen und tritt ggf. auch mit anderen Bibliotheken auf. Beispiel:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Falls bei Python-Bibliotheken immer wieder Installationsprobleme auftreten, können Sie alternativ Initialisierungsskripts verwenden. Dieser Ansatz wird nicht offiziell unterstützt. Informationen finden Sie in [diesem Dokument](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

2. Wenn Sie bei Verwendung des automatisierten maschinellen Lernens in Databricks eine Ausführung abbrechen und eine neue Experimentausführung starten möchten, müssen Sie Ihren Azure Databricks-Cluster neu starten.

3. In den Einstellungen für das automatisierte maschinelle Lernen legen Sie, wenn Sie mehr als 10 Iterationen haben, „show_output“ auf „false“ fest, wenn Sie Ihre Ausführung übermitteln.


## <a name="azure-portal"></a>Azure-Portal
Wenn Sie Ihren Arbeitsbereich direkt über einen Freigabelink aus dem SDK oder dem Portal anzeigen, können Sie die normale Übersichtsseite mit Abonnementinformationen in der Erweiterung nicht anzeigen. Außerdem können Sie nicht zu einem anderen Arbeitsbereich wechseln. Wenn Sie einen anderen Arbeitsbereich anzeigen möchten, müssen Sie direkt zum [Azure-Portal](https://portal.azure.com) navigieren und nach dem Namen des Arbeitsbereichs suchen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Die Protkolldateien finden Sie hier:

## <a name="resource-quotas"></a>Ressourcenkontingente

Erfahren Sie mehr über die [Ressourcenkontingente](how-to-manage-quotas.md), die Ihnen im Rahmen von Azure Machine Learning begegnen können.

## <a name="get-more-support"></a>Weitere Unterstützung

Sie können Supportanfragen stellen und Hilfe vom technischen Support, in Foren usw. erhalten. [Weitere Informationen](support-for-aml-services.md)
