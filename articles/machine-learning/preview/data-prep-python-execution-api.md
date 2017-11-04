---
title: "Ausführliche Anleitung zur Verwendung der Ausführungs-API der Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält Informationen zur Ausführung von zuvor entworfenen Datenquellen und Datenvorbereitungspaketen."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 4a0e4bd58ffa4bc7062ee4844a090be43047e8a1
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Ausführen von Datenquellen- und Datenvorbereitungspaketen aus Python

So verwenden Sie ein Datenquellen- und Datenvorbereitungspaket für Azure Machine Learning in Python:

1. Navigieren Sie zur Registerkarte **Daten** Ihres Projekts.

2. Klicken Sie mit der rechten Maustaste auf die entsprechende Quelle.

3. Wählen Sie **Datei für den Datenzugriffscode generieren** aus.

Bei dieser Aktion wird ein kurzes Python-Skript erstellt, das das Paket ausführt und ein Datenframe zurückgibt.

## <a name="data-sources"></a>Datenquellen

Das Modul `azureml.dataprep.datasource` enthält eine einzelne Funktion zum Ausführen einer Datenquelle und Zurückgeben eines Datenframes: `load_datasource(path, secrets=None, spark=None)`.
- `path` ist der Pfad zur Datenquelle (DSOURCE-Datei).
- `secrets` ist ein optionales Wörterbuch, das Schlüssel Geheimnissen zuordnet.
- `spark` ist ein optionaler boolescher Wert, der angibt, ob ein Spark- oder Pandas-Datenrahmen zurückgegeben werden soll. Standardmäßig bestimmt Azure Machine Learning Workbench, welche Art von Datenrahmen basierend auf dem Kontext zur Laufzeit zurückgegeben werden soll.

## <a name="data-preparations-packages"></a>Datenvorbereitungspakete

Das Modul `azureml.dataprep.package` enthält drei Funktionen, die einen Datenfluss von einem Datenvorbereitungspaket ausführen.

### <a name="execution-functions"></a>Ausführungsfunktionen

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)` – übermittelt den angegebenen Datenfluss zur Ausführung, gibt jedoch keinen Datenrahmen zurück.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)` – führt den angegebenen Datenfluss aus und gibt die Ergebnisse als Datenrahmen zurück.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)` – führt den angegebenen Datenfluss basierend auf einer In-Memory-Datenquelle aus und gibt die Ergebnisse als Datenrahmen zurück. Bei dem Argument `user_config` handelt es sich um ein Wörterbuch, das den absoluten Pfad einer Datenquelle (DSOURCE-Datei) einer In-Memory-Datenquelle zuordnet, die als Liste mit Listen dargestellt wird.

### <a name="common-arguments"></a>Allgemeine Argumente

- `package_path` ist der Pfad zum Datenvorbereitungspaket (DPREP-Datei).
- `dataflow_idx` ist der nullbasierte Index, dessen Datenfluss im Paket ausgeführt werden muss. Wenn der angegebene Datenfluss auf andere Datenflüsse oder Datenquellen verweist, werden diese ebenfalls ausgeführt.
- `secrets` ist ein optionales Wörterbuch, das Schlüssel Geheimnissen zuordnet.
- `spark` ist ein optionaler boolescher Wert, der angibt, ob ein Spark- oder Pandas-Datenrahmen zurückgegeben werden soll. Standardmäßig bestimmt Workbench, welche Art von Datenrahmen basierend auf dem Kontext zur Laufzeit zurückgegeben werden soll.
