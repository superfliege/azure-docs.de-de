---
title: Kopieren von Daten aus einer Webtabelle mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie mehr über den Webtabellenconnector von Azure Data Factory, mit dem Sie Daten aus einer Webtabelle als Senken in von Data Factory unterstützte Datenspeicher kopieren können."
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
ms.openlocfilehash: affe0d7da4b6e40da3b7fdb10d53b9e793ec19bd
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-web-table-by-using-azure-data-factory"></a>Kopieren von Daten aus einer Webtabelle mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-web-table-connector.md)
> * [Version 2: Vorschauversion](connector-web-table.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Webtabellendatenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.


> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Webtabellenconnector in V1](v1/data-factory-web-table-connector.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Webtabellendatenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Webconnector unterstützt insbesondere das **Extrahieren von Tabelleninhalten einer HTML-Seite**. Verwenden Sie zum Abrufen von Daten von einem HTTP(S)-Endpunkt stattdessen den [HTTP-Connector](connector-http.md).

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](create-self-hosted-integration-runtime.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Webtabellenconnector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit einer Webtabelle verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die Typeigenschaft muss auf **Web** |Ja |
| url | URL der Webquelle |Ja |
| authenticationType | Zulässiger Wert: **Anonymous**. |Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "url" : "https://en.wikipedia.org/wiki/",
            "authenticationType": "Anonymous"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Dataset „Webtabelle“ unterstützt werden.

Legen Sie zum Kopieren von Daten aus einer Webtabelle die „type“-Eigenschaft des Datasets auf **RelationalTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die „type“-Eigenschaft des Datasets muss auf **WebTable** festgelegt werden. | Ja |
| path |Eine relative URL zu der Ressource, die die Tabelle enthält. |Nein. Wenn der Pfad nicht angegeben ist, wird nur die URL verwendet, die in der Definition des verknüpften Diensts angegeben ist. |
| index |Der Index der Tabelle in der Ressource. Im Abschnitt [Abrufen des Indexes einer Tabelle auf einer HTML-Seite](#get-index-of-a-table-in-an-html-page) werden die Schritte zum Abrufen des Indexes einer Tabelle auf einer HTML-Seite beschrieben. |Ja |

**Beispiel:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Quelle „Webtabelle“ unterstützt werden.

### <a name="web-table-as-source"></a>Webtabelle als Quelle

Legen Sie zum Kopieren von Daten aus der Webtabelle den Quelltyp in der Kopieraktivität auf **WebSource** fest. Weitere Eigenschaften werden nicht unterstützt.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromWebTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Web table input dataset name>",
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
                "type": "WebSource"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="get-index-of-a-table-in-an-html-page"></a>Abrufen des Indexes einer Tabelle auf einer HTML-Seite

1. Starten Sie **Excel 2016**, und wechseln Sie zur Registerkarte **Daten**.
2. Klicken Sie auf der Symbolleiste auf **Neue Abfrage**, zeigen Sie auf **Aus anderen Quellen**, und klicken Sie auf **Aus dem Web**.

    ![Power Query-Menü](./media/copy-data-from-web-table/PowerQuery-Menu.png)
3. Geben Sie im Dialogfeld **Aus dem Web** die **URL**, die Sie im JSON-Code für den verknüpften Dienst verwenden möchten (Beispiel: https://en.wikipedia.org/wiki/), sowie den Pfad ein, den Sie für das Dataset angeben möchten (Beispiel: AFI%27s_100_Years... 100_Movies), und klicken Sie anschließend auf **OK**.

    ![Aus dem Web (Dialogfeld) ](./media/copy-data-from-web-table/FromWeb-DialogBox.png)

    In diesem Beispiel verwendete URL: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Falls das Dialogfeld **Auf Webinhalt zugreifen** angezeigt wird, wählen Sie die richtige **URL** und **Authentifizierung** aus, und klicken Sie auf **Verbinden**.

   ![Webinhalt aufrufen (Dialogfeld)](./media/copy-data-from-web-table/AccessWebContentDialog.png)
5. Klicken Sie in der Strukturansicht auf ein **Tabellenelement** um Inhalte aus der Tabelle anzuzeigen, und klicken Sie dann am unteren Rand auf **Bearbeiten**.  

   ![Navigator (Dialogfeld)](./media/copy-data-from-web-table/Navigator-DialogBox.png)
6. Klicken Sie im Fenster **Abfrage-Editor** auf der Symbolleiste auf die Schaltfläche **Erweiterter Editor**.

    ![Erweiterter Editor (Schaltfläche)](./media/copy-data-from-web-table/QueryEditor-AdvancedEditorButton.png)
7. Im Dialogfeld „Erweiterter Editor“ ist die Zahl neben „Quelle“ der Index.

    ![Erweiterter Editor – Index](./media/copy-data-from-web-table/AdvancedEditor-Index.png)

Rufen Sie den Index bei Verwendung von Excel 2013 mit [Microsoft Power Query für Excel](https://www.microsoft.com/download/details.aspx?id=39379) ab. Ausführlichere Informationen finden Sie im Artikel [Connect to a web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) (Verbinden mit einer Webseite). Bei Verwendung von [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)werden ähnliche Schritte verwendet.


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).