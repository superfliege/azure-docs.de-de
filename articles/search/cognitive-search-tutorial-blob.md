---
title: 'Tutorial: Aufrufen von APIs der kognitiven Suche in Azure Search | Microsoft Docs'
description: Beispiel für Extrahieren von Daten, natürliche Sprache und AI-Bildverarbeitung in der Azure Search-Indizierung für die Extrahierung und Transformation von Daten.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 023e55724124afb9c97d01540c6319a775681c84
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-learn-how-to-call-cognitive-search-apis-preview"></a>Tutorial: Informationen zum Aufrufen von APIs der kognitiven Suche (Vorschau)

In diesem Tutorial lernen Sie die Mechanismen des Programmierens von Datenanreicherung in Azure Search mithilfe von *kognitiven Qualifikationen* kennen. Kognitive Qualifikationen sind Vorgänge zur Verarbeitung von natürlicher Sprache (Natural Language Processing, NLP) und Bildanalyse, die Text und Textdarstellungen eines Bilds extrahieren sowie Sprache, Entitäten, wichtige Formulierungen und mehr erkennen. Das Endergebnis besteht in reichhaltigen zusätzlichen Inhalten in einem Azure Search-Index, die von einer Pipeline für kognitive Indizierung erstellt wurden. 

In diesem Tutorial werden Sie die folgenden Aufgaben mithilfe von REST-API-Aufrufen ausführen:

> [!div class="checklist"]
> * Erstellen einer Indizierungspipeline, die Quelldaten auf dem Weg in einen Index anreichert
> * Verwenden der integrierten Qualifikationen für Entitätserkennung, Spracherkennung, Textbearbeitung und Extrahierung für ein Beispieldataset
> * Erfahren Sie, wie Qualifikationen miteinander verkettet werden, indem die Eingänge den Ausgängen in einer Qualifikationsgruppe zugeordnet werden
> * Ausführen von Anforderungen und Überprüfen von Ergebnissen
> * Zurücksetzen des Index und der Indexer für die weitere Entwicklung

Die Ausgabe ist ein durchsuchbarer Volltextindex auf Azure Search. Sie können den Index mit weiteren Standardfunktionen erweitern, wie etwa [Synonymen](search-synonyms.md), [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [Analysetools](search-analyzers.md) und [Filtern](search-filters.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Neu bei Cognitive Search? Lesen Sie [„What is cognitive search?“](cognitive-search-concept-intro.md) (Was ist kognitive Suche), um sich vertraut zu machen, oder probieren Sie den [Schnellstart im Portal](cognitive-search-quickstart-blob.md) aus, wenn sie eine praktische Einführung in die wichtigen Konzepte bevorzugen.

Um REST-Aufrufe an Azure Search vorzunehmen, verwenden Sie PowerShell oder ein Web Test-Tool wie Telerik Fiddler oder Postman, um HTTP-Anforderungen zu formulieren. Wenn Sie mit diesen Tools nicht vertraut sind, lesen Sie [Explore Azure Search REST APIs using Fiddler or Postman](search-fiddler.md) (Durchsuchen von Azure Search REST-APIs mithilfe von Fiddler oder Postman).

Verwenden Sie das [Azure-Portal](https://portal.azure.com/), um Dienste zu erstellen, die in einem End-to-End-Workflow verwendet werden. 

### <a name="set-up-azure-search"></a>Einrichten von Azure Search

Registrieren Sie sich zuerst für den Azure Search-Dienst. 

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

1. Klicken Sie auf **Ressource erstellen**, suchen Sie nach Azure Search, und klicken Sie auf **Erstellen**. Wenn Sie zum ersten Mal einen Suchdienst einrichten, lesen Sie [Erstellen eines Azure Search-Diensts im Portal](search-create-service-portal.md).

  ![Dashboard-Portal](./media/cognitive-search-tutorial-blob/create-service-full-portal.png "Erstellen eines Azure Search-Diensts im Portal")

1. Erstellen Sie unter „Ressourcengruppe“ eine Ressourcengruppe zur Aufnahme aller Ressourcen, die Sie im Rahmen dieses Tutorials erstellen. Das macht Ihnen das Bereinigen der Ressourcen nach dem Abschluss des Tutorials leichter.

1. Wählen Sie als Standortort entweder **USA, Süden-Mitte** oder **Europa, Westen** aus. Aktuell ist die Vorschau nur in diesen Regionen verfügbar.

1. Für die Preisstufe können Sie einen **kostenlosen** Dienst erstellen, um Tutorials und Schnellstarts zu absolvieren. Wenn Sie eine gründlichere Untersuchung unter Verwendung Ihrer eigenen Daten wünschen, erstellen Sie einen [kostenpflichtigen Dienst](https://azure.microsoft.com/pricing/details/search/), wie etwa **Basic** oder **Standard**. 

  Ein kostenloser Dienst ist auf 3 Indizes, eine maximale Blobgröße von 16 MB und Indizierungsvorgänge von 2 Minuten Dauer beschränkt, was zum vollständigen Nutzen der Möglichkeiten der kognitiven Suche unzureichend ist. Informationen zum Überprüfen der Grenzwerte für verschiedene Stufen finden Sie unter [Diensteinschränkungen](search-limits-quotas-capacity.md).

  > [!NOTE]
  > Die kognitive Suche befindet sich derzeit in der öffentlichen Vorschauphase. Die Ausführung von Qualifikationsgruppen ist zurzeit in allen Stufen verfügbar, einschließlich der kostenlosen. Die Preisgestaltung für diese Funktion wird zu einem späteren Zeitpunkt bekannt gegeben.

1. Heften Sie den Dienst zwecks schnellem Zugriff auf Dienstinformationen an das Dashboard an.

  ![Dienstdefinitionsseite im Portal](./media/cognitive-search-tutorial-blob/create-search-service.png "Dienstdefinitionsseite im Portal")

1. Sammeln Sie nach dem Erstellen des Diensts die folgenden Informationen: **URL** von der Seite „Übersicht“ und **api-key** (entweder primär oder sekundär) von der Seite „Schlüssel“.

  ![Endpunkt- und Schlüsselinformationen im Portal](./media/cognitive-search-tutorial-blob/create-search-collect-info.png "Endpunkt- und Schlüsselinformationen im Portal")

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Einrichten des Azure Blob-Diensts und Laden von Beispieldaten

Die Anreicherungspipeline lädt per Pull aus Azure-Datenquellen herunter. Quelldaten müssen von einem unterstützten Datenquellentyp eines [Azure Search-Indexers](search-indexer-overview.md) stammen. Für diese Übung verwenden wir Blobspeicher, um verschiedene Inhaltstypen anschaulich vorzustellen.

1. [Laden Sie Beispieldaten herunter](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4). Die Beispieldaten bestehen aus einem kleinen Satz Dateien verschiedener Typen. 

1. Registrieren Sie sich für Azure Blob-Speicher, erstellen Sie ein Speicherkonto, melden Sie sich beim Storage-Explorer an, und erstellen Sie einen Container mit dem Namen `basicdemo`. Anweisungen zu all diesen Schritten finden Sie im [Azure Storage-Explorer-Schnellstart](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

1. Klicken Sie mithilfe des Azure Storage-Explorers in dem von Ihnen erstellten `basicdemo`-Container auf **Hochladen**, um die Beispieldateien hochzuladen.

1. Nachdem die Beispieldateien geladen wurden, rufen Sie den Containernamen und eine Verbindungszeichenfolge für Ihren Blobspeicher ab. Dazu können Sie im Azure-Portal zu Ihrem Speicherkonto navigieren. Auf **Zugriffsschlüssel**, und kopieren Sie dann das Feld **Verbindungszeichenfolge**.

  Bei der Verbindungszeichenfolge sollte es sich um eine URL ähnlich dem folgenden Beispiel handeln:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=y1NIlE9wFVBIabcd562GzZl+JO9TEGdqOerqfbT78C8zrn28Te8DsWlxvKKnjh67P/HM5k80zt4shOt9vqlbg==;EndpointSuffix=core.windows.net
      ```

Es gibt andere Möglichkeiten zum Angeben der Verbindungszeichenfolge, etwa das Bereitstellen einer Shared Access Signature (SAS). Weitere Informationen über Anmeldeinformationen für Datenquellen finden Sie unter [Indizieren von Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Jetzt, da Ihre Dienste und Datenquellen vorbereitet sind, beginnen Sie damit, die Komponenten Ihrer Indizierungspipeline zusammenzustellen. Beginnen Sie mit einem [Datenquellenobjekt](https://docs.microsoft.com/rest/api/searchservice/create-data-source), das Azure Search anweist, wie die externen Quelldaten abzurufen sind.

Verwenden Sie für dieses Lernprogramm die REST-API und ein Tool, das HTTP-Anforderungen formulieren und senden kann, z.B. PowerShell, Postman oder Fiddler. Geben Sie im Anforderungsnamen den Dienstnamen, den Sie beim Erstellen des Azure Search-Diensts verwendet haben, und den für Ihren Suchdienst generierten API-Schlüssel an. Geben Sie im Hauptteil der Anforderung den Namen des Blobcontainers und die Verbindungszeichenfolge an.

### <a name="sample-request"></a>Beispiel für eine Anforderung
```http
POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
Content-Type: application/json  
api-key: [admin key]  
```
#### <a name="request-body-syntax"></a>Syntax des Anforderungstexts
```json
{   
    "name" : "demodata",  
    "description" : "Demo files to demonstrate cognitive search capabilities.",  
    "type" : "azureblob",
    "credentials" :
    { "connectionString" :
      "DefaultEndpointsProtocol=https;AccountName=<your account name>;AccountKey=<your account key>;"
    },  
    "container" : { "name" : "<your blob container name>" }
}  
```
Senden Sie die Anforderung. Das Web Test-Tool sollte den Erfolg durch Rückgabe von Statuscode 201 bestätigen. 

Da es sich hier um Ihre erste Anforderung handelt, überprüfen Sie im Azure-Portal, ob die Datenquelle in Azure Search erstellt wurde. Überprüfen Sie auf der Dashboard-Seite des Suchdiensts, ob die Kachel „Datenquellen“ einen neuen Eintrag aufweist. Möglicherweise müssen Sie einige Minuten warten, bis die Portalseite aktualisiert wurde. 

  ![Kachel „Datenquellen“ im Portal](./media/cognitive-search-tutorial-blob/data-source-tile.png "Kachel „Datenquellen“ im Portal")

Wenn der Fehler 403 oder 404 angezeigt wird, überprüfen Sie die Konstruktion der Anforderung: `api-version=2017-11-11-Preview` sollte im Endpunkt vorhanden sein, `api-key` sollte im Header hinter `Content-Type` vorhanden sein, und sein Wert muss für einen Suchdienst gültig sein. Sie können den Header für die verbleibenden Schritte in diesem Tutorial wiederverwenden.

> [!TIP]
> Jetzt, bevor Sie eine Menge Arbeit investieren, ist der richtige Zeitpunkt, zu überprüfen, ob der Suchdienst an einem der unterstützten Standorte ausgeführt wird, die das Vorschaufeature bereitstellen: USA, Süden-Mitte oder Europa, Westen.

## <a name="create-a-skillset"></a>Erstellen einer Qualifikationsgruppe

In diesem Schritt definieren Sie eine Reihe von Anreicherungsschritten, die Sie auf Ihre Daten anwenden möchten. Jeder Anreicherungsschritt wird als *Qualifikation* und der Satz von Anreicherungsschritten als *Qualifikationsgruppe* bezeichnet. Dieses Tutorial verwendet für die Qualifikationsgruppe [vordefinierte kognitive Qualifikationen](cognitive-search-predefined-skills.md):

+ [Spracherkennung](cognitive-search-skill-language-detection.md), um die Sprache der Inhalte zu bestimmen.

+ [Text unterteilen](cognitive-search-skill-textsplit.md), um große Inhalte vor dem Aufrufen der Schlüsselbegriffserkennungs-Qualifikation in kleinere Stücke aufzuteilen. Die Schlüsselbegriffserkennung akzeptiert Eingaben von 50.000 Zeichen oder weniger. Für einige der Beispieldateien ist eine Aufteilung erforderlich, um diesen Grenzwert zu erfüllen.

+ [Eigennamenerkennung](cognitive-search-skill-named-entity-recognition.md), um die Namen von Organisationen aus Inhalten im Blobcontainer zu extrahieren.

+ [Schlüsselbegriffserkennung](cognitive-search-skill-keyphrases.md), um die wichtigsten Schlüsselbegriffe herauszuziehen. 

### <a name="sample-request"></a>Beispiel für eine Anforderung
Bevor Sie diesen REST-Aufruf ausführen, achten Sie darauf, den Dienstnamen und den Administratorschlüssel in der Anforderung unten zu ersetzen, wenn Ihr Tool den Anforderungsheader beim Wechsel des Aufrufs nicht beibehält. 

Mit dieser Anforderung wird eine Qualifikationsgruppe erstellt. Verweisen Sie für den verbleibenden Teil dieses Tutorials auf den Namen der Qualifikationsgruppe ```demoskillset```.

```http
PUT https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Syntax des Anforderungstexts
```json
{
  "description": 
  "Extract entities, detect language and extract key-phrases",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations", "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "languageCode",
          "targetName": "languageCode"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
      "textSplitMode" : "pages", 
      "maximumPageLength": 4000,
      "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      },
      { 
        "name": "languageCode",
        "source": "/document/languageCode"
      }
    ],
    "outputs": [
      {
            "name": "textItems",
            "targetName": "pages"
      }
    ]
  },
  {
      "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
      "context": "/document/pages/*",
      "inputs": [
        {
          "name": "text", "source": "/document/pages/*"
        },
        {
          "name":"languageCode", "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "keyPhrases",
          "targetName": "keyPhrases"
        }
      ]
    }
  ]
}
```

Senden Sie die Anforderung. Das Web Test-Tool sollte den Erfolg durch Rückgabe von Statuscode 201 bestätigen. 

#### <a name="about-the-request"></a>Informationen zur Anforderung

Beachten Sie, in welcher Weise die Schlüsselbegriffserkennungs-Qualifikation für die einzelnen Seiten angewendet wird. Durch Festlegen des Kontexts auf ```"document/pages/*"``` führen Sie diesen Anreicherungsschritt für jedes Element des Dokument-/Seitenarrays (für jede Seite im Dokument) aus.

Jede Qualifikation wird auf den Inhalten des Dokuments ausgeführt. Während der Verarbeitung bricht Azure Search jedes Dokument auf, um die Inhalte aus verschiedenen Dateiformaten zu lesen. Gefundener Text, der aus der Quelldatei stammt, wird in einem generierten Feld ```content``` gespeichert, einem für jedes Dokument. Legen Sie die Eingabe daher auf ```"/document/content"``` fest.

Unten finden Sie eine grafische Darstellung der Qualifikationsgruppe. 

![Grundlagen von Qualifikationsgruppen](media/cognitive-search-tutorial-blob/skillset.png "Grundlagen von Qualifikationsgruppen")

Ausgaben können einem Index zugeordnet, als Eingabe einer Downstream-Qualifikation verwendet oder in beider Weise zugleich eingesetzt werden, wie etwa bei Sprachcode. Im Index ist ein Sprachcode zu Filterungszwecken nützlich. Als Eingabe wird ein Sprachcode von Qualifikationen zur Textanalyse verwendet, um die Linguistikregeln über Wörtertrennung zu informieren.

Weitere Informationen zu den Grundlagen von Qualifikationsgruppen finden Sie unter [How to define a skillset](cognitive-search-defining-skillset.md) (Definieren von Qualifikationsgruppen).

## <a name="create-an-index"></a>Erstellen eines Index

In diesem Abschnitt definieren Sie das Indexschema, indem Sie angeben, welche Felder in den durchsuchbaren Index aufgenommen und mit welchen Suchattributen die einzelnen Felder versehen werden sollen. Felder besitzen einen Typ und können Attribute annehmen, die bestimmen, wie das Feld verwendet wird (durchsuchbar, sortierbar usw.). Feldname in einem Index müssen nicht exakt mit den Feldnamen in der Quelle übereinstimmen. In einem späteren Schritt fügen Sie in einem Indexer Feldzuordnungen hinzu, um die Quell- und Zielfelder zu verbinden. Definieren Sie für diesen Schritt den Index mit Feldbenennungskonventionen, die für Ihre Suchanwendung angemessen sind.

In dieser Übung werden die folgenden Felder und Feldtypen verwendet:

| Feldnamen: | id       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="sample-request"></a>Beispiel für eine Anforderung
Bevor Sie diesen REST-Aufruf ausführen, achten Sie darauf, den Dienstnamen und den Administratorschlüssel in der Anforderung unten zu ersetzen, wenn Ihr Tool den Anforderungsheader beim Wechsel des Aufrufs nicht beibehält. 

Mit dieser Anforderung wird ein Index erstellt. Verwenden Sie den Indexnamen ```demoindex``` für den verbleibenden Teil dieses Tutorials.

```http
PUT https://[servicename].search.windows.net/indexes/demoindex?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Syntax des Anforderungstexts

```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
Senden Sie die Anforderung. Das Web Test-Tool sollte den Erfolg durch Rückgabe von Statuscode 201 bestätigen. 

Weitere Informationen zum Definieren eines Index finden Sie unter [Index erstellen (Azure Search REST-API)](https://docs.microsoft.com/rest/api/searchservice/create-index).


## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Erstellen eines Indexers, Zuordnen von Feldern und Ausführen von Transformationen

Bisher haben Sie eine Datenquelle, eine Qualifikationsgruppe und einen Index erstellt. Diese drei Komponenten werden Teil eines [Indexers](search-indexer-overview.md), der jedes einzelne Stück per Pull in einen einzelnen mehrstufigen Vorgang herunterlädt. Um diese in einem Indexer zusammenzuführen, müssen Sie Feldzuordnungen definieren. Feldzuordnungen sind Bestandteil der Indexerdefinition und führen die Transformationen aus, wenn Sie die Anforderung senden.

Für nicht anreichernde Indizierung stellt der Indexer einen optionalen Abschnitt *fieldMappings* zur Verfügung, wenn die Feldnamen oder Datentypen nicht exakt übereinstimmen oder Sie eine Funktion verwenden möchten.

Für Workloads der kognitiven Suche, die eine Anreicherungspipeline aufweisen, benötigt ein Indexer *outputFieldMappings*. Diese Zuordnungen werden verwendet, wenn ein interner Vorgang (die Anreicherungspipeline) die Quelle der Feldwerte bildet. Zu den Verhalten, die *outputFieldMappings* exklusiv auszeichnen, gehört die Fähigkeit zum Verarbeiten komplexer Typen, die im Rahmen der Anreicherung (mithilfe der Shaper-Qualifikation) erstellt werden. Ferner können pro Dokument viele Elemente vorkommen (beispielsweise mehrere Organisationen in einem Dokument). Das Konstrukt *outputFieldMappings* kann das System anweisen, Sammlungen von Elementen in einem einzelnen Datensatz zu „vereinfachen“.

### <a name="sample-request"></a>Beispiel für eine Anforderung

Bevor Sie diesen REST-Aufruf ausführen, achten Sie darauf, den Dienstnamen und den Administratorschlüssel in der Anforderung unten zu ersetzen, wenn Ihr Tool den Anforderungsheader beim Wechsel des Aufrufs nicht beibehält. 

Geben Sie außerdem den Namen Ihres Indexers an. Sie können auf ihn für den Rest dieses Tutorials als ```demoindexer``` verweisen.

```http
PUT https://[servicename].search.windows.net/indexers/demoindexer?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Syntax des Anforderungstexts

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
        {
          "sourceFieldName" : "metadata_storage_path",
          "targetFieldName" : "id",
          "mappingFunction" : 
            { "name" : "base64Encode" }
        },
        {
          "sourceFieldName" : "content",
          "targetFieldName" : "content"
        }
   ],
  "outputFieldMappings" : 
  [
        {
          "sourceFieldName" : "/document/organizations", 
          "targetFieldName" : "organizations"
        },
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*", 
          "targetFieldName" : "keyPhrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "languageCode"
        }      
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "maxFailedItemsPerBatch":-1,
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
        }
  }
}
```

Senden Sie die Anforderung. Das Web Test-Tool sollte die erfolgreiche Verarbeitung durch Rückgabe von Statuscode 201 bestätigen. 

Sie sollten damit rechnen, dass die Ausführung dieses Schritts mehrere Minuten in Anspruch nimmt. Das Dataset ist zwar klein, Analysequalifikationen sind aber rechenintensiv. Einige Qualifikationen, wie etwa die Bildanalyse, weisen lange Ausführungszeiten auf.

> [!TIP]
> Die Pipeline wird durch Erstellen eines Indexers aufgerufen. Wenn beim Zugriff auf die Daten, dem Zuordnen von Ein- und Ausgaben oder der Reihenfolge der Vorgänge Probleme bestehen, äußern sie sich in dieser Phase. Um die Pipeline mit Code- oder Skriptänderungen auszuführen, müssen Sie möglicherweise zuerst Objekte löschen. Weitere Informationen finden Sie unter [Reset and re-run](#reset) (Zurücksetzen und erneut Ausführen).

### <a name="explore-the-request-body"></a>Untersuchen des Anforderungstexts

Das Skript legt ```"maxFailedItems"``` auf -1 fest, was die Indexengine anweist, Fehler beim Datenimport zu ignorieren. Dies ist nützlich, weil die Demodatenquelle nur wenige Dokumente enthält. Für eine größere Datenquelle sollten Sie den Wert größer als 0 festlegen.

Achten Sie außerdem auf die Anweisung ```"dataToExtract":"contentAndMetadata"``` in den Konfigurationsparametern. Diese Anweisung weist den Indexer an, die Inhalte aus verschiedenen Dateiformaten sowie die den einzelnen Dateien zugeordneten Metadaten automatisch zu extrahieren. 

Wenn die Inhalte extrahiert werden, können Sie ```ImageAction``` darauf festlegen, Text aus in der Datenquelle gefundenen Bildern zu extrahieren. ```"ImageAction":"generateNormalizedImages"``` weist den Indexer an, Text aus den Bildern zu extrahieren (beispielsweise das Wort „Stop“ aus einem Stop-Verkehrsschild) und ihn als Teil des Inhaltsfelds einzubetten. Dieses Verhalten betrifft sowohl die in den Dokumenten eingebetteten Bilder (denken Sie etwa an Bilder in PDF-Dateien) als auch die in der Datenquelle gefundenen Bilder, z.B. eine JPG-Datei.

In dieser Vorschau ist ```"generateNormalizedImages"``` der einzig gültige Wert für ```"ImageAction"```.

## <a name="check-indexer-status"></a>Überprüfen des Indexerstatus

Nachdem der Indexer definiert wurde, wird er automatisch ausgeführt, wenn Sie die Anforderung senden. Abhängig von den kognitiven Qualifikationen, die Sie definiert haben, kann die Indizierung länger als erwartet dauern. Um herauszufinden, ob der Indexer noch ausgeführt wird, senden Sie die folgende Anforderung, um den Indexerstatus zu überprüfen.

```http
GET https://[servicename].search.windows.net/indexers/demoindexer/status?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Aus der Antwort erfahren Sie, ob der Indexer noch ausgeführt wird. Verwenden Sie nach dem Abschluss der Indizierung einen weiteren HTTP GET-Aufruf an den STATUS-Endpunkt (wie oben), um Berichte über eventuelle Fehler und Warnungen anzuzeigen, die während der Anreicherung aufgetreten sind.  

Warnungen sind bei bestimmten Kombinationen aus Quelldatei und Qualifikation häufig und weisen nicht immer auf ein Problem hin. Im Rahmen dieses Tutorials sind die Warnungen gutartig (z.B. keine Texteingaben aus den JPEG-Dateien). Sie können die Statusantwort für ausführliche Informationen über Warnungen überprüfen, die während der Indizierung ausgegeben wurden.
 
## <a name="verify-content"></a>Inhaltsüberprüfung

Führen Sie nach dem Abschluss der Indizierung Abfragen aus, die die Inhalte einzelner Felder zurückgeben. Standardmäßig gibt Azure Search die obersten 50 Ergebnisse zurück. Die Beispieldaten sind klein, so dass die Standardeinstellung gut funktioniert. Beim Arbeiten mit größeren Datensets müssen Sie jedoch möglicherweise Parameter in die Abfragezeichenfolge aufnehmen, um mehr Ergebnisse zurückzugeben. Anweisungen finden Sie unter [How to page results in Azure Search](search-pagination-page-layout.md) (Seitenweise Ausgabe von Ergebnissen in Azure Search).

Fragen Sie als Überprüfungsschritt den Index nach allen Feldern ab.

```http
GET https://[servicename].search.windows.net/indexes/demoindex?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Die Ausgabe ist das Indexschema mit dem Namen, dem Typ und den Attributen für jedes Feld.

Senden Sie eine zweite Abfrage nach `"*"`, um alle Inhalte eines einzelnen Felds zurückzugeben, z.B. `organizations`.

```http
GET https://[servicename].search.windows.net/indexes/demoindex/docs?search=*&$select=organizations&api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Wiederholen Sie dies für weitere Felder: „content“, „language“, „keyphrases“ und „organizations“ in dieser Übung. Mithilfe von `$select` können Sie unter Einsatz einer durch Trennzeichen getrennten Liste mehrere Felder zurückgeben.

Sie können GET oder POST verwenden, abhängig von der Komplexität und Länge der Abfragezeichenfolge. Weitere Informationen finden Sie unter [Abfragen mithilfe der REST-API](https://docs.microsoft.com/azure/search/search-query-rest-api).

<a name="access-enriched-document"></a>

## <a name="accessing-the-enriched-document"></a>Zugreifen auf das angereicherte Dokument

Mithilfe der kognitiven Suche können Sie die Struktur des angereicherten Dokuments anzeigen. Angereicherte Dokumente sind temporäre Strukturen, die während der Anreicherung erstellt und nach dem Abschluss des Vorgangs gelöscht werden.

Um eine Momentaufnahme des angereicherten Dokuments zu erfassen, das während der Indizierung erstellt wird, fügen Sie Ihrem Index ein Feld ```enriched``` hinzu. Der Indexer lädt in diesem Feld automatisch eine Zeichenfolgendarstellung aller Anreicherungen für das betreffende Dokument ab.

Das ```enriched```-Feld enthält dann eine Zeichenfolge, die eine logische Darstellung des angereicherten Dokuments im Arbeitsspeicher in JSON darstellt.  Der Feldwert ist jedoch ein gültiges JSON-Dokument. Anführungszeichen werden mit Escapezeichen versehen, sodass Sie `\"` durch `"` ersetzen müssen, um das Dokument als formatierten JSON-Code anzuzeigen.  

Das ```enriched```-Feld ist für Debugzwecke vorgesehen und soll Sie beim Verständnis der logischen Form der Inhalte unterstützen, für die Ausdrücke ausgewertet werden. Es kann ein nützliches Tool zum Verstehen und Debuggen Ihrer Qualifikationsgruppe sein.

Wiederholen Sie die vorhergehende Übung, und schließen Sie ein `enriched`-Feld zum Erfassen der Inhalte eines angereicherten Dokuments ein:

### <a name="request-body-syntax"></a>Syntax des Anforderungstexts
```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
<a name="reset"></a>

## <a name="reset-and-rerun"></a>Zurücksetzen und erneut ausführen

In den frühen, experimentellen Phasen der Pipelineentwicklung besteht der praktikabelste Ansatz für den Übergang von einer Entwurfsphase zur nächsten darin, die Objekt aus Azure Search zu löschen und Ihrem Code zu erlauben, sie neu zu erstellen. Ressourcennamen sind eindeutig. Wenn Sie ein Objekt löschen, können Sie es unter dem gleichen Namen neu erstellen.

So indizieren Sie Ihre Dokumente mit den neuen Definitionen erneut:

1. Löschen Sie den Index, um persistente Daten zu entfernen. Löschen Sie den Indexer, um ihn für Ihren Dienst neu zu erstellen.
2. Ändern Sie eine Qualifikationsgruppe und die Indexdefinition.
3. Führen Sie eine Neuerstellung eines Index und des Indexers für den Dienst aus, um die Pipeline auszuführen. 

Sie können das Portal verwenden, um Indizes und Indexer zu löschen. Qualifikationsgruppen können nur mithilfe eines HTTP-Befehls gelöscht werden, sollten Sie sich für die Löschung entscheiden.

```http
DELETE https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Nach erfolgreichem Löschen wird der Statuscode 204 zurückgegeben.

In dem Maß, da Ihr Code reift, kann es sinnvoll sein, die Neuerstellungsstrategie neu zu definieren. Weitere Informationen finden  Sie unter [Neuerstellen eines Indexes](search-howto-reindex.md).

## <a name="takeaways"></a>Wesentliche Punkte

Dieses Tutorial veranschaulicht die grundlegenden Schritte beim Erstellen einer erweiterten Indizierungspipeline durch Erstellung von Komponenten: eine Datenquelle, eine Qualifikationsgruppe, ein Index und ein Indexer.

Es wurden [vordefinierte Qualifikationen](cognitive-search-predefined-skills.md) im Zusammenhang mit der Qualifikationsgruppendefinition und den Mechanismen zum Verketten von Qualifikationen mithilfe von Eingängen und Ausgängen vorgestellt. Sie haben darüber hinaus erfahren, dass `outputFieldMappings` in der Indexerdefinition erforderlich ist, um angereicherte Werte aus der Pipeline in einen durchsuchbaren Index in einem Azure Search-Dienst weiterzuleiten.

Ferner haben Sie erfahren, wie die Ergebnisse getestet werden und das System für weitere Entwicklungsschritte zurückgesetzt wird. Sie haben gelernt, dass das Ausgeben von Abfragen auf den Index die von der angereicherten Indizierungspipeline erstellte Ausgabe zurückgibt. In dieser Version ist ein Mechanismus zum Anzeigen von internen Konstrukten (vom System erstellten angereicherten Dokumenten) verfügbar. Darüber hinaus haben Sie die Überprüfung des Indexerstatus und das Löschen von Objekten vor der erneuten Ausführung einer Pipeline gelernt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die schnellste Möglichkeit, das System nach einem Tutorial aufzuräumen, besteht im Löschen der Ressourcengruppe, die den Azure Search-Dienst und den Azure Blob-Dienst enthält. Unter der Annahme, dass Sie beide Dienste in der gleichen Gruppe platziert haben, löschen Sie nun einfach die Ressourcengruppe, um endgültig ihren gesamten Inhalt zu löschen, einschließlich der Dienste und aller gespeicherten Inhalte, die Sie für dieses Tutorial erstellt haben. Im Portal finden Sie den Namen der Ressourcengruppe auf der Seite „Übersicht“ der einzelnen Dienste.

## <a name="next-steps"></a>Nächste Schritte

Anpassen oder Erweitern der Pipeline mit benutzerdefinierten Qualifikationen. Das Erstellen einer benutzerdefinierten Qualifikation die Sie einer Qualifikationsgruppe hinzufügen, ermöglicht Ihnen, eigene, von Ihnen selbst erstellte Text- oder Bildanalysen einzubeziehen. 

> [!div class="nextstepaction"]
> [Beispiel: Erstellen einer benutzerdefinierten Qualifikation](cognitive-search-create-custom-skill-example.md)
