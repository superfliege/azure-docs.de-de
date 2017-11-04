---
title: "Ermitteln der genauesten und kürzesten Ausführungen in Azure Machine Learning Workbench | Microsoft-Dokumentation"
description: "Ein End-to-End-Anwendungsfall, um die beste Genauigkeit über die CLI mithilfe von Azure Machine Learning Workbench zu ermitteln."
services: machine-learning
author: totekp
ms.author: kefzhou
manager: akannava
ms.reviewer: akannava, haining, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: aaadf526577b9b6c254204aae90200661d40f325
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2017
---
# <a name="find-runs-with-the-best-accuracy-and-lowest-duration"></a>Ermitteln der genauesten und kürzesten Ausführungen
Bei mehreren Ausführungen ist ein Anwendungsfall, die Ausführungen mit der besten Genauigkeit zu ermitteln. Ein Ansatz besteht darin, die Befehlszeilenschnittstelle (Command Line Interface, CLI) mit einer [JMESPath](http://jmespath.org/)-Abfrage zu verwenden. Weitere Informationen zum Verwenden von JMESPath in der Azure CLI finden Sie unter [Verwenden von JMESPath-Abfragen mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest). Im folgenden Beispiel werden vier Ausführungen mit den Genauigkeitswerten von 0, 0,98, 1 und 1 erstellt. Die Ausführungen werden gefiltert, wenn diese sich im Bereich `[MaxAccuracy-Threshold, MaxAccuracy]` befinden, in dem `Threshold = .03`.

## <a name="sample-data"></a>Beispieldaten
Wenn keine Ausführungen mit dem Wert `Accuracy` vorhanden sind, generieren die folgenden Schritte Ausführungen für Abfragen.

Erstellen Sie zunächst eine Python-Datei in der Azure Machine Learning Workbench, nennen Sie diese `log_accuracy.py`, und kopieren Sie sie in den folgenden Code:
```python
from azureml.logging import get_azureml_logger

logger = get_azureml_logger()

accuracy_value = 0.5

if len(sys.argv) > 1:
     accuracy_value = float(sys.argv[1])

logger.log("Accuracy", accuracy_value)
```

Erstellen Sie dann die Datei `run.py`, und kopieren Sie sie in den folgenden Code:
```python
import os

accuracy_values = [0, 0.98, 1.0, 1.0]
for value in accuracy_values:
    os.system('az ml experiment submit -c local ./log_accuracy.py {}'.format(value))
```

Öffnen Sie schließlich die CLI über die Workbench, und führen Sie den Befehl `python run.py` aus, um vier Experimente zu übermitteln. Nach dem Abschluss des Skripts sollten sich vier weitere Ausführungen in der Registerkarte `Run History` befinden.

## <a name="query-the-run-history"></a>Abfragen des Ausführungsverlaufs
Der erste Befehl sucht den maximalen Genauigkeitswert.
```powershell
az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
```

Wenn der maximale Genauigkeitswert von `1` und ein Schwellenwert von `0.03` verwendet werden, filtert der zweite Befehl die Ausführungen mithilfe von `Accuracy` und sortiert diese dann absteigend nach `duration`.
```powershell
az ml history list --query '@[?Accuracy >= sum(`[1, -0.03]`)] | sort_by(@, &duration)'
```
> [!NOTE]
> Wenn Sie eine strikte Überprüfung der Obergrenze möchten, ist das Format der Abfrage ``@[?Accuracy >= sum(`[$max_accuracy_value, -$threshold]`) && Accuracy <= `$max_accuracy_value`]``.

Wenn Sie PowerShell verwenden, verwendet der folgende Code lokale Variablen, um den Schwellen- und den maximalen Genauigkeitswert zu speichern:
```powershell
$threshold = 0.03
$max_accuracy_value = az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
$find_runs_query = '@[?Accuracy >= sum(`[{0}, -{1}]`)] | sort_by(@, &duration)' -f $max_accuracy_value, $threshold
az ml history list --query $find_runs_query
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Protokollierung finden Sie unter [Verwenden des Ausführungsverlaufs und der Modellmetriken in Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md).    
