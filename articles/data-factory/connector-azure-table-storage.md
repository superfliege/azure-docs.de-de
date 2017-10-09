---
title: Kopieren von Daten nach bzw. aus Azure Table Storage mit Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten von unterstützten Quellspeichern mithilfe von Data Factory nach Azure Table Storage oder aus Table Storage in unterstützte Senkenspeicher kopiert werden."
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 2fa03b82750585454430da0c29392db57b20d3c9
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="copy-data-to-or-from-azure-table-using-azure-data-factory"></a>Kopieren von Daten in eine oder aus einer Azure-Tabelle mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-azure-table-connector.md)
> * [Version 2 – Vorschau](connector-azure-table-storage.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten in eine und von einer Azure-Tabelle zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (General Availability, GA) ist, lesen Sie [Azure Table Storage-Connector in V1](v1/data-factory-azure-table-connector.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten aus einem beliebigen unterstützten Quelldatenspeicher in eine Azure-Tabelle bzw. Daten aus einer Azure-Tabelle in einen beliebigen unterstützten Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Azure-Tabellenconnector unterstützt insbesondere das Kopieren von Daten mithilfe der **Kontoschlüssel**- und **Dienst-SAS**-Authentifizierung (Shared Access Signature).

## <a name="get-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python-SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Azure Table Storage verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

### <a name="using-account-key"></a>Verwenden des Kontoschlüssels

Sie können mithilfe des Kontoschlüssels einen mit Azure Storage verknüpften Dienst erstellen, der der Data Factory globalen Zugriff auf Azure Storage gewährt. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureStorage** |Ja |
| connectionString | Geben Sie Informationen, die zur Verbindung mit dem Azure-Speicher erforderlich sind, für die connectionString-Eigenschaft ein. Legen Sie für dieses Feld „SecureString“ fest. |Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-service-sas-authentication"></a>Verwenden der Dienst-SAS-Authentifizierung

Sie können mithilfe einer Shared Access Signature (SAS) auch einen verknüpften Azure Storage-Dienst erstellen, der der Data Factory einen beschränkten bzw. zeitgebundenen Zugriff auf alle oder bestimmte Ressourcen im Speicher gewährt.

Shared Access Signatures (SAS) bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Sie ermöglichen es Ihnen, einem Client für einen bestimmten Zeitraum spezielle eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto zu erteilen, ohne Ihre Konto-Zugriffsschlüssel weitergeben zu müssen. Die SAS ist ein URI, dessen Abfrageparameter alle erforderlichen Informationen für den authentifizierten Zugriff auf eine Speicherressource enthalten. Für den Zugriff auf Speicherressourcen mit der SAS braucht der Client diese nur an den entsprechenden Konstruktor bzw. die entsprechende Methode zu übergeben. Weitere Informationen zu SAS finden Sie unter [Shared Access Signatures: Grundlagen zum SAS-Modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Azure Data Factory unterstützt nur **Dienst-SAS**, nicht Konto-SAS. Unter [Typen von Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) finden Sie ausführliche Informationen zu diesen beiden Typen und ihrer Erstellung. Bei der SAS-URL, die im Azure-Portal oder Storage-Explorer erstellt werden kann, handelt es sich um eine Konto-SAS. Diese wird nicht unterstützt.
>

Für die Verwendung der Dienst-SAS-Authentifizierung werden folgende Eigenschaften unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureStorage** |Ja |
| sasUri | Geben Sie den Shared Access Signature-URI für Azure-Speicher-Ressourcen wie BLOB, Container oder Tabelle an. Legen Sie für dieses Feld „SecureString“ fest. |Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Beim Erstellen eines **SAS-URI** sollten Sie folgende Punkte berücksichtigen:

- Legen Sie für Objekte basierend darauf, wie der verknüpfte Dienst (Lesen, Schreiben, Lesen/Schreiben) in Ihrer Data Factory verwendet wird, geeignete Lese-/Schreib-**Berechtigungen** fest.
- Legen Sie für **Ablaufzeit** einen geeigneten Wert fest. Stellen Sie sicher, dass der Zugriff auf Azure Storage-Objekte nicht im aktiven Zeitraum der Pipeline abläuft.
- Der URI sollte je nach Bedarf auf der richtigen Tabellenebene erstellt werden.

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Azure-Tabellendataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus einer bzw. in eine Azure-Tabelle die type-Eigenschaft des Datasets auf **AzureTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **AzureTable** festgelegt werden. |Ja |
| tableName |Name der Tabelle in der Azure-Tabellendatenbankinstanz, auf die der verknüpfte Dienst verweist. |Ja |

**Beispiel:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema per Data Factory

Bei schemafreien Datenspeichern, z. B. Azure-Tabellen, leitet der Data Factory-Dienst das Schema auf eine der folgenden Weisen ab:

1. Wenn Sie die Struktur der Daten mithilfe der **structure** -Eigenschaft in der Datasetdefinition angeben, berücksichtigt der Data Factory-Dienst diese Struktur als das Schema. Wenn in diesem Fall eine Zeile keinen Wert für eine Spalte enthält, wird ein NULL-Wert für sie angegeben.
2. Wenn Sie die Struktur der Daten nicht mithilfe der **structure** -Eigenschaft in der Datasetdefinition angeben, leitet Data Factory das Schema unter Verwendung der ersten Zeile in den Daten ab. Wenn in diesem Fall die erste Zeile nicht das vollständige Schema enthält, fehlen im Ergebnis des Kopiervorgangs einige Spalten.

Daher empfiehlt es sich bei schemafreien Datenquellen, die Struktur der Daten mithilfe der **structure** -Eigenschaft anzugeben.

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure-Tabellenquelle und -senke unterstützt werden.

### <a name="azure-table-as-source"></a>Azure-Tabelle als Quelle

Legen Sie zum Kopieren von Daten aus der Azure-Tabelle den Quelltyp in der Kopieraktivität auf **AzureTableSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureTableSource** festgelegt werden. |Ja |
| AzureTableSourceQuery |Verwendet die benutzerdefinierte Azure-Tabellenabfrage zum Lesen von Daten. Siehe Beispiele im nächsten Abschnitt. |Nein |
| azureTableSourceIgnoreTableNotFound |Gibt an, ob der Ausnahmefall, dass die Tabelle nicht vorhanden ist, ignoriert werden soll.<br/>Zulässige Werte sind **True** und **False** (Standard). |Nein |

### <a name="azuretablesourcequery-examples"></a>Beispiele für azureTableSourceQuery

Wenn die Spalte für die Azure-Tabelle vom Typ „Zeichenfolge“ ist:

```json
"azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', <datetime parameter>)"
```

Wenn die Spalte für die Azure-Tabelle vom Typ „datetime“ ist:

```json
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', <datetime parameter>, <datetime parameter>)"
```

### <a name="azure-table-as-sink"></a>Azure-Tabelle als Senke

Legen Sie zum Kopieren von Daten aus der Azure-Tabelle den Quelltyp in der Kopieraktivität auf **AzureTableSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureTableSink** festgelegt werden. |Ja |
| azureTableDefaultPartitionKeyValue |Standardmäßiger Partitionsschlüsselwert, der von der Senke verwendet werden kann. |Nein |
| azureTablePartitionKeyName |Geben Sie den Namen der Spalte an, deren Werte als Partitionsschlüssel verwendet werden. Wenn dieser nicht angegeben ist, wird „AzureTableDefaultPartitionKeyValue“ als Partitionsschlüssel verwendet. |Nein |
| azureTableRowKeyName |Geben Sie den Namen der Spalte an, deren Werte als Zeilenschlüssel verwendet werden. Wenn nicht angegeben, verwenden Sie für jede Zeile eine GUID. |Nein |
| azureTableInsertType |Der Modus zum Einfügen von Daten in eine Azure-Tabelle. Diese Eigenschaft steuert, ob die Werte von vorhandenen Zeilen in der Ausgabetabelle, deren Partitions- und Zeilenschlüssel übereinstimmen, ersetzt oder zusammengeführt werden. <br/><br/>Zulässige Werte sind **merge** (Standard) und **replace**. <br/><br> Diese Einstellung gilt auf Zeilenebene, nicht auf Tabellenebene, und keine der beiden Optionen löscht Zeilen in der Ausgabetabelle, die in der Eingabe nicht vorhanden sind. Informationen zur Funktionsweise dieser Einstellungen (Zusammenführen und Ersetzen) finden Sie in den Themen [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) (Entität einfügen oder zusammenführen) und [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) (Entität einfügen oder ersetzen). |Nein |
| writeBatchSize |Fügt Daten in die Azure-Tabelle ein, wenn "writeBatchSize" oder "writeBatchTimeout" erreicht wird.<br/>Zulässige Werte sind ganze Zahlen (Anzahl der Zeilen). |Nein (Standard = 10.000) |
| writeBatchTimeout |Fügt Daten in die Azure-Tabelle ein, wenn "writeBatchSize" oder "writeBatchTimeout" erreicht wird.<br/>Zulässige Werte sind Zeiträume. Beispiel: „00:20:00“ (20 Minuten). |Nein (standardmäßiger Timeoutwert von 90 Sekunden für Speicherclients) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Bevor Sie die Zielspalte als „azureTablePartitionKeyName“ verwenden können, müssen Sie einer Zielspalte mithilfe der Eigenschaft „translator“ eine Quellspalte zuordnen.

Im folgenden Beispiel wird die Quellspalte „DivisionID“ der Zielspalte „DivisionID“ zugeordnet.

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

„DivisionID“ ist als Partitionsschlüssel angegeben.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Datentypzuordnung für Azure-Tabellen

Beim Kopieren von Daten aus bzw. in die Azure-Tabelle werden die folgenden Zuordnungen von Azure-Tabellendatentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

Beim Verschieben von Daten in die und aus der Azure-Tabelle werden die folgenden [vom Azure-Tabellenspeicherdienst definierten Zuordnungen](https://msdn.microsoft.com/library/azure/dd179338.aspx) bei Verschiebungen von Azure-Tabellen-OData-Typen in den .NET-Typ und umgekehrt verwendet.

| Azure-Tabellendatentyp | Data Factory-Zwischendatentyp | Details |
|:--- |:--- |:--- |
| Edm.Binary |Byte[] |Ein Array von Bytes mit einer Größe bis zu 64KB. |
| Edm.Boolean |bool |Ein boolescher Wert. |
| Edm.DateTime |DateTime |Ein 64-Bit-Wert, ausgedrückt als koordinierte Weltzeit (UTC). Der unterstützte DateTime-Bereich beginnt um 00:00 Uhr, Mitternacht, 1. Januar, 1601 n. Chr. (unsere Zeitrechnung), UTC Der Bereich endet am 31. Dezember 9999. |
| Edm.Double |double |Ein 64-Bit-Gleitkommawert. |
| Edm.Guid |GUID |Ein 128-Bit-GUID. |
| Edm.Int32 |Int32 |Eine 32-Bit-Ganzzahl. |
| Edm.Int64 |Int64 |Eine 64-Bit-Ganzzahl. |
| Edm.String |String |Ein UTF-16-codierter Wert. Zeichenfolgenwerte können bis zu 64KB groß sein. |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
