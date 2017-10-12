---
title: "Referenz zur Modelldatensammlungs-API für Azure Machine Learning | Microsoft-Dokumentation"
description: "Referenz zur Modelldatensammlungs-API für Azure Machine Learning"
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 7a0fda8a44d13bcaba84b4124d9b693c05874154
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Referenz zur Modelldatensammlungs-API für Azure Machine Learning

Mithilfe der Modelldatensammlung können Sie Modelleingaben und Vorhersagen eines Machine Learning-Webdiensts archivieren. In der [Anleitung zur Modelldatensammlung](how-to-use-model-data-collection.md) erfahren Sie, wie Sie `azureml.datacollector` auf Ihrem Windows- und Linux-Computer installieren.

In dieser API-Referenz befolgen wir schrittweise einen Ansatz zum Sammeln von Modelleingaben und Vorhersagen eines Machine Learning-Webdiensts.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Aktivieren der Modelldatensammlung in der Azure ML Workbench-Umgebung

 Suchen Sie in Ihrem Projekt im Ordner „aml_config“ die Datei „conda\_dependencies.yml“, und fügen Sie Ihrer Datei „conda\_dependencies.yml“ im Abschnitt „pip“ das Modul „azureml.datacollector“ wie nachstehend gezeigt hinzu. Beachten Sie, dass dies nur ein Teilabschnitt der vollständigen Datei „conda\_dependencies.yml“ ist:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>Derzeit können Sie das Datensammlermodul in Azure ML Workbench bei Ausführung im Docker-Modus verwenden. Der lokale Modus funktioniert möglicherweise nicht für alle Umgebungen.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Aktivieren der Modelldatensammlung in der Bewertungsdatei

Importieren Sie in die Bewertungsdatei, die für die Operationalisierung verwendet wird, das Datensammlermodul und die „ModelDataCollector“-Klasse:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Instanziierung des Modelldatensammlers
Instanziieren Sie eine neue Instanz der „ModelDataCollector“-Klasse:

dc = ModelDataCollector(model_name, identifier='default', feature_names=None, model_management_account_id='unknown', webservice_name='unknown', model_id='unknown', model_version='unknown')

Siehe die Details unter „Klasse“ und „Parameter“:

### <a name="class"></a>Klasse
| Name | Beschreibung |
|--------------------|--------------------|
| ModelDataCollector | Eine Klasse im Namespace „azureml.datacollector“. Eine Instanz dieser Klasse wird zum Sammeln von Modelldaten verwendet. Eine einzelne Bewertungsdatei kann mehrere „ModelDataCollector“-Klassen enthalten. Jede Instanz sollte zum Sammeln von Daten an einer einzelnen Stelle in der Bewertungsdatei verwendet werden, damit das Schema der gesammelten Daten konsistent bleibt (d.h. Eingaben und Vorhersage).|


### <a name="parameters"></a>Parameter

| Name | Typ | Beschreibung |
|-------------|------------|-------------------------|
| model_name | string | Name des Modells, für das Daten gesammelt werden |
| Bezeichner | string | Stelle im Code, die diese Daten identifiziert, d.h. „RawInput“ oder „Prediction“ |
| feature_names | Liste von Zeichenfolgen | Liste von Featurenamen, die zum CSV-Header werden, falls angegeben |
| model_management_account_id | string | Bezeichner des Modellverwaltungskontos, in dem dieses Modell gespeichert ist. Wird automatisch aufgefüllt, wenn Modelle über AML operationalisiert werden |
| webservice_name | string | Name des Webdiensts, für den dieses Modell derzeit bereitgestellt ist. Wird automatisch aufgefüllt, wenn Modelle über AML operationalisiert werden |
| model_id | string | Der eindeutige Bezeichner dieses Modells im Kontext eines Modellverwaltungskontos. Wird automatisch aufgefüllt, wenn Modelle über AML operationalisiert werden |
| model_version | string | Versionsnummer dieses Modells im Kontext eines Modellverwaltungskontos. Wird automatisch aufgefüllt, wenn Modelle über AML operationalisiert werden |



 

## <a name="collecting-the-model-data"></a>Sammeln der Modelldaten

Sie können die Modelldaten mithilfe einer Instanz der zuvor erstellten „ModelDataCollector“-Klasse sammeln.

    dc.collect(input_data, user_correlation_id="")

Siehe die Details unter „Methode“ und „Parameter“:

### <a name="method"></a>Methode
| Name | Beschreibung |
|--------------------|--------------------|
| collect | Dient zum Sammeln der Daten für eine Modelleingabe oder Vorhersage|


### <a name="parameters"></a>Parameter

| Name | Typ | Beschreibung |
|-------------|------------|-------------------------|
| input_data | Mehrere Typen | Die zu sammelnden Daten (derzeit werden die Liste „types“, „numpy.array, pandas.DataFrame“ und „pyspark.sql.DataFrame“ akzeptiert). Bei Datenrahmentypen, wenn ein Header mit Featurenamen vorhanden ist, werden diese Informationen in das Datenziel eingefügt (ohne dass Featurenamen explizit an den „ModelDataCollector“-Konstruktor übergeben werden müssen) |
| user_correlation_id | string | Optionaler Korrelationsbezeichner, der vom Benutzer zum Korrelieren dieser Vorhersage bereitgestellt werden kann |

