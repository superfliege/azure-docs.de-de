---
title: Kopieren von Daten aus Cassandra mit Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus Cassandra mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: eba08c38a5502368beda7ca7f84559ecca011133
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Kopieren von Daten aus Cassandra mit Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-onprem-cassandra-connector.md)
> * [Version 2 – Vorschau](connector-cassandra.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Cassandra-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.


> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (General Availability, GA) ist, lesen Sie [Cassandra-Connector in V1](v1/data-factory-onprem-cassandra-connector.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Cassandra-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Cassandra-Connector unterstützt insbesondere Folgendes:

- Cassandra **Version 2.X**
- Kopieren von Daten unter Verwendung der **Standard**- oder **anonymen** Authentifizierung

## <a name="prerequisites"></a>Voraussetzungen

Zum Kopieren von Daten aus einer Cassandra-Datenbank, die nicht öffentlich zugänglich ist, müssen Sie eine selbstgehostete Integrationslaufzeit einrichten. Im Artikel [Selbstgehostete Integrationslaufzeit](create-self-hosted-integration-runtime.md) finden Sie Details. Die Integrationslaufzeit bietet einen integrierten Cassandra-Treiber. Daher müssen beim Kopieren von Daten aus bzw. nach Cassandra keine Treiber manuell installiert werden.

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Cassandra-Connector verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Cassandra verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ |Die type-Eigenschaft muss auf **Cassandra** festgelegt werden. |Ja |
| host |Mindestens eine IP-Adresse oder ein Hostname von Cassandra-Servern.<br/>Geben Sie eine durch Trennzeichen getrennte Liste mit IP-Adressen oder Hostnamen an, um gleichzeitig mit allen Servern Verbindungen herzustellen. |Ja |
| port |Der TCP-Port, den der Cassandra-Server verwendet, um auf Clientverbindungen zu lauschen. |Nein (Standard = 9042) |
| authenticationType | Typ der Authentifizierung für die Verbindung mit der Cassandra-Datenbank.<br/>Zulässige Werte sind **Basic** oder **Anonymous**. |Ja |
| username |Geben Sie einen Benutzernamen für das Benutzerkonto an. |Ja, wenn authenticationType auf „Basic“ (Standard) festgelegt ist. |
| password |Geben Sie ein Kennwort für das Benutzerkonto an. Legen Sie für dieses Feld „SecureString“ fest. |Ja, wenn authenticationType auf „Basic“ (Standard) festgelegt ist. |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "Cassandra",
        "typeProperties": {
            "host": "<host>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Cassandra-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Cassandra die type-Eigenschaft des Datasets auf **RelationalTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **CassandraTable** festgelegt werden. | Ja |
| keyspace |Name des Keyspace oder Schemas in der Cassandra-Datenbank. |Nein (wenn „query“ für „CassandraSource“ nicht angegeben ist) |
| tableName |Name der Tabelle in der Cassandra-Datenbank. |Nein (wenn „query“ für „CassandraSource“ nicht angegeben ist) |

**Beispiel:**

```json
{
    "name": "CassandraDataset",
    "properties": {
        "type": "CassandraTable",
        "linkedServiceName": {
            "referenceName": "<Cassandra linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "keySpace": "<keyspace name>",
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität


Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Cassandra-Quelle unterstützt werden.

### <a name="cassandra-as-source"></a>Cassandra als Quelle

Legen Sie zum Kopieren von Daten aus Cassandra den Quelltyp in der Kopieraktivität auf **CassandraSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **CassandraSource** festgelegt werden. | Ja |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-92-Abfrage oder CQL-Abfrage. Weitere Informationen finden Sie in der [Referenz zu CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Geben Sie beim Verwenden der SQL-Abfrage **keyspace name.table name** für die Tabelle an, die Sie abfragen möchten. |Nein (wenn „tableName“ und „keyspace“ im Dataset angegeben sind) |
| consistencyLevel |Mit der Konsistenzebene (consistencyLevel) wird angegeben, wie viele Replikate auf eine Leseanforderung reagieren müssen, bevor Daten an die Clientanwendung zurückgegeben werden. Cassandra überprüft die angegebene Anzahl von Replikaten auf Daten, um die Leseanforderung zu erfüllen. Ausführliche Informationen finden Sie unter [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) (Konfigurieren der Datenkonsistenz).<br/><br/>Zulässige Werte: **ONE**, **TWO**, **THREE**, **QUORUM**, **ALL**, **LOCAL_QUORUM**, **EACH_QUORUM** und **LOCAL_ONE**. |Nein (Standard = `ONE`) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromCassandra",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cassandra input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "CassandraSource",
                "query": "select id, firstname, lastname from mykeyspace.mytable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-cassandra"></a>Datentypzuordnung für Cassandra

Beim Kopieren von Daten aus Cassandra werden die folgenden Zuordnungen von Cassandra-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Cassandra-Datentyp | Data Factory-Zwischendatentyp |
|:--- |:--- |
| ASCII |String |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAN |BOOLEAN |
| DECIMAL |DECIMAL |
| DOUBLE |DOUBLE |
| FLOAT |Single |
| INET |String |
| INT |Int32 |
| TEXT |String |
| TIMESTAMP |DateTime |
| TIMEUUID |GUID |
| UUID |GUID |
| VARCHAR |String |
| VARINT |DECIMAL |

> [!NOTE]
> Informationen zu Sammlungstypen (map, set, list usw.) finden Sie im Abschnitt [Verwenden von Cassandra-Sammlungstypen mit einer virtuellen Tabelle](#work-with-collections-using-virtual-table) .
>
> Benutzerdefinierte Typen werden nicht unterstützt.
>
> Die Länge von „Binary Column“ und „String Column“ darf 4.000 nicht übersteigen.
>

## <a name="work-with-collections-using-virtual-table"></a>Verwenden von Cassandra-Sammlungstypen mit einer virtuellen Tabelle

Azure Data Factory verwendet einen integrierten ODBC-Treiber, um eine Verbindung mit der Cassandra-Datenbank herzustellen und Daten daraus zu kopieren. Für Sammlungstypen, z.B. „map“, „set“ und „list“, normalisiert der Treiber die Daten erneut in die entsprechenden virtuellen Tabellen. Wenn eine Tabelle Sammlungsspalten enthält, generiert der Treiber die folgenden virtuellen Tabellen:

* Eine **Basistabelle**, die die gleichen Daten wie die tatsächliche Tabelle enthält, mit Ausnahme der Sammlungsspalten. Für die Basistabelle wird der gleiche Name wie für die echte Tabelle verwendet, die sie repräsentiert.
* Eine **virtuelle Tabelle** für jede Sammlungsspalte (Erweiterung der geschachtelten Daten). Die virtuellen Tabellen, die Sammlungen repräsentieren, werden mit dem Namen der tatsächlichen Tabelle benannt und erhalten zusätzlich das Trennzeichen „*vt*“ und den Namen der Spalte.

Virtuelle Tabellen beziehen sich auf die Daten in der echten Tabelle, sodass der Treiber auf die denormalisierten Daten zugreifen kann. Details finden Sie unten im Abschnitt mit dem Beispiel. Sie können auf den Inhalt von Cassandra-Sammlungen zugreifen, indem Sie die virtuellen Tabellen abfragen und verknüpfen.

### <a name="example"></a>Beispiel

Die folgende Beispieltabelle „ExampleTable“ ist beispielsweise eine Cassandra-Datenbanktabelle, die eine Spalte mit dem Namen „pk_int“ für ganzzahlige Primärschlüssel, eine Textspalte mit dem Namen „value“ und die Spalten „list“, „map“ und „set“ („StringSet“) enthält.

| pk_int | Wert | Auflisten | Map | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"sample value 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"sample value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Der Treiber erzeugt mehrere virtuelle Tabellen, um diese einzelne Tabelle zu repräsentieren. Die Fremdschlüsselspalten in den virtuellen Tabellen verweisen auf die Primärschlüsselspalten in der echten Tabelle und geben an, welcher Zeile der echten Tabelle die Zeile der virtuellen Tabelle entspricht.

Die erste virtuelle Tabelle ist die unten dargestellte Basistabelle mit dem Namen „ExampleTable“: 

| pk_int | Wert |
| --- | --- |
| 1 |"sample value 1" |
| 3 |"sample value 3" |

Die Basistabelle enthält die gleichen Daten wie die ursprüngliche Datenbanktabelle, mit Ausnahme der Sammlungen, die in dieser Tabelle fehlen und in anderen virtuellen Tabellen enthalten sind.

Die folgenden Tabellen enthalten die virtuellen Tabellen, in denen die Daten aus den Spalten „List“, „Map“ und „StringSet“ erneut normalisiert werden. Die Spalten mit Namen, die auf „_index“ oder „_key“ enden, geben die Position der Daten in der Originalliste (list) bzw. -zuordnung (map) an. Die Spalten mit Namen, die auf „_value“ enden, enthalten die erweiterten Daten aus der Sammlung.

**Tabelle „ExampleTable_vt_List“:**

| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

**Tabelle „ExampleTable_vt_Map“:**

| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |Eine  |
| 1 |S2 |b |
| 3 |S1 |t |

**Tabelle „ExampleTable_vt_StringSet“:**

| pk_int | StringSet_value |
| --- | --- |
| 1 |Eine  |
| 1 |b |
| 1 |C |
| 3 |Eine  |
| 3 |E |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).