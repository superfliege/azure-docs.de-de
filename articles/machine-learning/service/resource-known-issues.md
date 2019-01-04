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
ms.openlocfilehash: 4a4f1691162ab9c9fbd5bc8802ecf7ebc4894d74
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53193670"
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

## <a name="fpgas"></a>FPGAs
Um Modelle auf FPGAs bereitzustellen, müssen Sie zuerst ein FPGA-Kontingent anfordern und dessen Genehmigung abwarten. Füllen Sie das Formular zur Kontingentanforderung aus, um Zugriff anzufordern: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Probleme mit Databricks und Azure Machine Learning:

1. Empfehlung für Databricks-Cluster:
   
   Erstellen Sie Ihr Azure Databricks-Cluster als Version 4.x mit Python 3. Es wird empfohlen, einen Cluster für hohe Parallelität zu verwenden.
 
2. Die Installation des Azure Machine Learning SDK (ML SDK) schlägt in Databricks fehl, wenn mehrere Pakete installiert werden.

   Einige Pakete, z.B. `psutil`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie beim Installieren der Pakete die Bibliotheksversion ein. Dieses Problem hängt mit Databricks und nicht mit dem Azure ML SDK zusammen und tritt ggf. auch mit anderen Bibliotheken auf. Beispiel:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Falls bei Python-Bibliotheken immer wieder Installationsprobleme auftreten, können Sie alternativ Initialisierungsskripts verwenden. Dieser Ansatz wird nicht offiziell unterstützt. Informationen finden Sie in [diesem Dokument](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

3. Bei Verwendung von automatisiertem maschinellem Lernen wird Folgendes angezeigt: `Import error: numpy.core.multiarray failed to import`

   Problemumgehung: Importieren Sie die Python-Bibliothek `numpy==1.14.5` in Ihren Databricks-Cluster, und verwenden Sie dabei die Bibliothekerstellungsfunktion zum [Installieren und Anfügen](https://docs.databricks.com/user-guide/libraries.html#create-a-library).

## <a name="azure-portal"></a>Azure-Portal
Wenn Sie Ihren Arbeitsbereich direkt über einen Freigabelink aus dem SDK oder dem Portal anzeigen, können Sie die normale Übersichtsseite mit Abonnementinformationen in der Erweiterung nicht anzeigen. Außerdem können Sie nicht zu einem anderen Arbeitsbereich wechseln. Wenn Sie einen anderen Arbeitsbereich anzeigen möchten, müssen Sie direkt zum [Azure-Portal](https://portal.azure.com) navigieren und nach dem Namen des Arbeitsbereichs suchen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Die Protkolldateien finden Sie hier:

## <a name="resource-quotas"></a>Ressourcenkontingente

Erfahren Sie mehr über die [Ressourcenkontingente](how-to-manage-quotas.md), die Ihnen im Rahmen von Azure Machine Learning begegnen können.

## <a name="get-more-support"></a>Weitere Unterstützung

Sie können Supportanfragen stellen und Hilfe vom technischen Support, in Foren usw. erhalten. [Weitere Informationen](support-for-aml-services.md)
