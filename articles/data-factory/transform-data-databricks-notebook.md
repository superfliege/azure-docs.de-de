---
title: Transformieren von Daten mit einem Databricks-Notebook – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten verarbeiten oder transformieren, indem Sie ein Databricks-Notebook ausführen.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: 5f21f33678b8cf09d9dbd8966d42b1a5ebac9ffb
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224651"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Transformieren von Daten durch Ausführen eines Databricks-Notebooks

Die Azure Databricks-Notebook-Aktivität in einer [Data Factory-Pipeline](concepts-pipelines-activities.md) führt ein Databricks-Notebook in Ihrem Azure Databricks-Arbeitsbereich aus. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet. Azure Databricks ist eine verwaltete Plattform für die Ausführung von Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>Definition der Databricks-Notebook-Aktivität

Dies ist die JSON-Beispieldefinition der Databricks-Notebook-Aktivität:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            },
            "libraries": [
                {
                "jar": "dbfs:/docs/library.jar"
                }
            ]
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Eigenschaften der Databricks-Notebook-Aktivität

Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden:

|Eigenschaft|BESCHREIBUNG|Erforderlich|
|---|---|---|
|name|Der Name der Aktivität in der Pipeline.|JA|
|Beschreibung|Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.|Nein |
|type|Bei Databricks-Notebook-Aktivitäten lautet der Aktivitätstyp DatabricksNotebook.|JA|
|linkedServiceName|Der Name des verknüpften Databricks-Diensts, in dem das Databricks-Notebook ausgeführt wird. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md).|JA|
|notebookPath|Der absolute Pfad des Notebooks, das im Databricks-Arbeitsbereich ausgeführt werden soll. Dieser Pfad muss mit einem Schrägstrich beginnen.|JA|
|baseParameters|Ein Array aus Schlüssel-Wert-Paaren. Für jede Aktivitätsausführung können Basisparameter verwendet werden. Wenn das Notebook einen nicht spezifizierten Parameter akzeptiert, wird der Standardwert des Notebooks verwendet. Erfahren Sie mehr über Parameter in [Databricks-Notebooks](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Nein |
|libraries|Eine Liste der Bibliotheken, die in dem Cluster installiert werden, der den Auftrag ausführen wird. Es kann ein Array vom Typ \<Zeichenfolge, Objekt> sein.|Nein |


## <a name="supported-libraries-for-databricks-activities"></a>Unterstützte Bibliotheken für Databricks-Aktivitäten

In der oben genannten Definition der Databricks-Aktivität geben Sie diese Bibliothekstypen an: *JAR*, *EGG*, *Maven*, *PyPI*, *CRAN*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "http://cran.us.r-project.org"
            }
        }
    ]
}

```

Weitere Informationen zu Bibliothekstypen finden Sie in der [Databricks-Dokumentation](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary).

## <a name="how-to-upload-a-library-in-databricks"></a>Hochladen einer Bibliothek in Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Benutzeroberfläche des Databricks-Arbeitsbereichs](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Sie können den DBFS-Pfad der hinzugefügten Bibliothek über die Benutzeroberfläche mithilfe der [Databricks-Benutzerzeilenschnittstelle (Installation)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli) abrufen. 

JAR-Bibliotheken werden beim Verwenden der Benutzeroberfläche in der Regel unter dbfs:/FileStore/jars gespeichert. Sie können alle über die Benutzerzeilenschnittstelle auflisten: *databricks fs ls dbfs:/FileStore/jars*



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Kopieren von Bibliotheken mit der Databricks-Benutzerzeilenschnittstelle](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Beispiel: *databricks fs cp SparkPi-assembly-0.1.jar dbfs:/FileStore/jars*.
