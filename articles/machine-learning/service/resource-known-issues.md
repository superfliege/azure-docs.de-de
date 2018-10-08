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
ms.date: 09/24/2018
ms.openlocfilehash: d84040dc440c373ae9bae6dbac7a95109a387ba7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162745"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekannte Probleme und Problembehandlung für Azure Machine Learning Service
 
Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu ermitteln und zu beheben, die beim Verwenden von Azure Machine Learning Service auftreten können. 


## <a name="databricks"></a>Databricks

Probleme mit Databricks und Azure Machine Learning:

1. Empfehlung für Databricks-Cluster:
   
   Erstellen Sie Ihr Azure Databricks-Cluster als Version 4.x mit Python 3. Es wird empfohlen, einen Cluster für hohe Parallelität zu verwenden.
 
1. Die Installation des Azure Machine Learning SDK (ML SDK) schlägt in Databricks fehl, wenn mehrere Pakete installiert werden.

   Einige Pakete, z.B. `psutil upgrade libs`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie beim Installieren der Pakete die Bibliotheksversion ein. Dieses Problem betrifft Databricks – nicht das ML SDK. Beispiel:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="gather-diagnostics-information"></a>Sammeln von Diagnoseinformationen
Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Die Protkolldateien finden Sie hier:

## <a name="resource-quotas"></a>Ressourcenkontingente

Erfahren Sie mehr über die [Ressourcenkontingente](how-to-manage-quotas.md), die Ihnen im Rahmen von Azure Machine Learning begegnen können.

## <a name="get-more-support"></a>Weitere Unterstützung

Sie können Supportanfragen stellen und Hilfe vom technischen Support, in Foren usw. erhalten. [Weitere Informationen](support-for-aml-services.md)
