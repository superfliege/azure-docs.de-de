---
title: Indizieren von JSON-Blobs aus Azure Blob-Indexer für die Volltextsuche – Azure Search
description: Durchforsten Sie Azure JSON-Blobs mithilfe des Azure Search-Blobindexers nach Textinhalten. Indexer automatisieren die Datenerfassung für ausgewählte Datenquellen wie Azure Blob Storage.
ms.date: 12/21/2018
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: cafb48f28e38794ce0757d50a5d87432b237e17c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467161"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indizieren von JSON-Blobs mit dem Azure Search-Blobindexer
In diesem Artikel wird beschrieben, wie Sie einen Azure Search-Blobindexer konfigurieren, um strukturierten Inhalt aus JSON-Blobs in Azure Blob Storage zu extrahieren.

Sie können das [Portal](#json-indexer-portal), [REST-APIs](#json-indexer-rest) oder das [.NET SDK](#json-indexer-dotnet) verwenden, um JSON-Inhalt zu indizieren. JSON-Dokumente befinden sich grundsätzlich in einem Blobcontainer in einem Azure-Speicherkonto, egal für welche Option Sie sich entscheiden. Anleitungen zum Übertragen von JSON-Dokumenten mithilfe von Push von anderen Nicht-Azure-Plattformen finden Sie unter [Indizieren externer Daten für Abfragen in Azure Search](search-what-is-data-import.md).

JSON-Blobs in Azure Blob Storage bestehen normalerweise entweder aus einem einzelnen JSON-Dokument oder einem JSON-Array. Mit dem Blobindexer in Azure Search können beide Fälle analysiert werden. Dies richtet sich danach, wie Sie den Parameter **parsingMode** in der Anforderung festlegen.

> [!IMPORTANT]
> Die JSON-Blobindizierung ist allgemein verfügbar. Die JsonArray-Analyse befindet sich allerdings noch in der Public Preview-Phase und darf nicht in Produktionsumgebungen verwendet werden. Weitere Informationen finden Sie unter [REST api-version=2017-11-11-Preview](search-api-2017-11-11-preview.md). 

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Verwenden des Portals

Die einfachste Methode zum Indizieren von JSON-Dokumenten ist die Verwendung eines Assistenten im [Azure-Portal](https://portal.azure.com/). Der [**Datenimport**](search-import-data-portal.md)-Assistent kann durch das Analysieren von Metadaten im Azure-Blobcontainer einen Standardindex erstellen, Quellfelder Zielindexfeldern zuordnen und den Index in einem einzigen Vorgang laden. Je nach Größe und Komplexität der Quelldaten können Sie auch innerhalb von Minuten einen funktionsfähigen Volltextsuchindex erstellen.

### <a name="1---prepare-source-data"></a>1. Vorbereiten von Quelldaten

Sie müssen über ein Azure-Speicherkonto mit einem Blobspeicher und über einen Container mit JSON-Elementen verfügen. Wenn Sie nicht mit diesen Aufgaben vertraut sind, lesen Sie unbedingt den Abschnitt „Einrichten des Azure Blob-Diensts und Laden von Beispieldaten“ unter [Schnellstart: Erstellen einer Pipeline für die kognitive Suche mithilfe von Qualifikationen und Beispieldaten](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data).

### <a name="2---start-import-data-wizard"></a>2. Starten des Datenimport-Assistenten

Sie können den Assistenten auf der Seite des Azure Search-Diensts [über die Befehlsleiste](search-import-data-portal.md) starten. Alternativ können Sie im Abschnitt **Blob-Dienst**, der sich im linken Navigationsbereich Ihres Speicherkontos befindet, auf **Azure Search hinzufügen** klicken.

### <a name="3---set-the-data-source"></a>3. Einrichten der Datenquelle

Auf der Seite **Datenquelle** muss die Quelle **Azure Blob Storage** lauten und folgende Spezifikationen aufweisen:

+ **Zu extrahierende Daten** muss *Inhalt und Metadaten* sein. Durch die Auswahl dieser Option kann der Assistent ein Indexschema ableiten und so die Felder für den Importvorgang zuordnen.
   
+ **Analysemodus** muss auf *JSON* oder *JSON-Array* festgelegt sein. 

  *JSON* definiert jedes Blob als einzelnes Suchdokument, das als unabhängiges Element in Suchergebnissen angezeigt wird. 

  *JSON-Array* ist für Blobs, die aus mehreren Elementen bestehen, wobei jedes Element als eigenständiges unabhängiges Suchdokument angesehen werden soll. Wenn Blobs komplex sind und Sie *JSON-Array* nicht auswählen, dann wird das gesamte Blob als einzelnes Dokument erfasst.
   
+ **Speichercontainer** muss Ihr Speicherkonto sowie den Container angeben oder alternativ eine Verbindungszeichenfolge, die im Container aufgelöst wird. Sie können Verbindungszeichenfolgen auf der Seite des Blob-Dienst-Portals abrufen.

   ![Definition der Blobdatenquelle](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4. Überspringen der Seite „Kognitive Suche hinzufügen“ im Assistenten

Das Hinzufügen kognitiver Qualifikationen ist für den Import eines JSON-Dokuments nicht erforderlich. Wenn Sie nicht unbedingt [Cognitive Services-APIs und Transformationen](cognitive-search-concept-intro.md) zu Ihrer Indizierungspipeline hinzufügen müssen, können Sie diesen Schritt überspringen.

Klicken Sie auf die nächste Seite, **Zielindex anpassen**, um den Schritt zu überspringen.

### <a name="5---set-index-attributes"></a>5. Festlegen von Indexattributen

Auf der **Indexseite** sollte eine Liste von Feldern mit einem Datentyp sowie mehrere Kontrollkästchen zum Festlegen von Indexattributen aufgeführt sein. Der Assistent kann einen Standardindex basierend auf Metadaten und durch Sampling der Quelldaten erstellen. 

Durch die Standardeinstellungen erhalten Sie oft eine praktikable Lösung: Wählen Sie ein Feld (als Zeichenfolge umgewandelt), das als Schlüssel- oder Dokument-ID fungiert, um jedes Dokument eindeutig zu identifizieren, sowie Felder, die gute Kandidaten für die Volltextsuche sind und in einem Resultset abgerufen werden können. Für Blobs ist das `content`-Feld der geeignete Kandidat für durchsuchbaren Inhalt.

Sie können die Standardeinstellungen akzeptieren oder die Beschreibung der [Indexattribute](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) und [Sprachanalysen](https://docs.microsoft.com/rest/api/searchservice/language-support) überprüfen, um die ursprünglichen Werte zu überschreiben oder zu ergänzen. 

Nehmen Sie sich einen Moment Zeit, um Ihre Auswahl zu überprüfen. Wenn Sie den Assistenten ausführen, werden physische Datenstrukturen erstellt, und Sie können diese Felder nicht bearbeiten, ohne alle Objekte zu löschen und neu zu erstellen.

   ![Blobindexdefinition](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6: Erstellen des Indexers

Wenn alle Angaben gemacht wurden, erstellt der Assistent drei unterschiedliche Objekte in Ihrem Suchdienst. Ein Datenquellenobjekt und ein Indexobjekt werden als benannte Ressourcen in Ihrem Azure Search-Dienst gespeichert. Mit dem letzten Schritt wird ein Indexerobjekt erstellt. Wenn der Indexer benannt wird, kann er als eigenständige Ressource existieren. Diese können Sie unabhängig vom Index und Datenquellenobjekt, die im selben Durchlauf des Assistenten erstellt wurden, planen und verwalten.

Falls Sie noch nicht mit Indexern vertraut sind: Ein *Indexer* ist eine Ressource in Azure Search, die eine externe Datenquelle nach durchsuchbarem Inhalt durchforstet. Die Ausgabe des **Datenimport**-Assistenten ist ein Indexer, der Ihre JSON-Datenquelle durchforstet, durchsuchbaren Inhalt extrahiert und diesen in einen Index im Azure Search-Dienst importiert.

   ![Definition des Blobindexers](media/search-howto-index-json/import-wizard-json-indexer.png)

Klicken Sie auf **OK**, um den Assistenten auszuführen und alle Objekte zu erstellen. Die Indizierung wird sofort durchgeführt.

Sie können den Datenimport auf den Portalseiten überwachen. Fortschrittsbenachrichtigungen geben den Indizierungsstatus an und wie viele Dokumente hochgeladen werden. Wenn die Indizierung abgeschlossen ist, können Sie den [Such-Explorer](search-explorer.md) zum Abfragen Ihres Indexes verwenden.

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>Verwenden von REST-APIs

Die Indizierung von JSON-Blobs ähnelt der regulären Dokumentextraktion in einem dreiteiligen Workflow, der für alle Indexer in Azure Search gleich ist: Erstellen einer Datenquelle, eines Indexes und eines Indexers.

Für die auf Code basierende JSON-Indizierung können Sie die REST-API mit APIs für [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer), [Datenquellen](https://docs.microsoft.com/rest/api/searchservice/create-data-source) und [Indizes](https://docs.microsoft.com/rest/api/searchservice/create-index) verwenden. Im Gegensatz zum Portal-Assistenten ist für einen Code-Ansatz erforderlich, dass Sie über einen direkten Index verfügen, der sofort die JSON-Dokumente akzeptiert, wenn Sie die Anforderung **Indexer erstellen** senden.

JSON-Blobs in Azure Blob Storage bestehen normalerweise entweder aus einem einzelnen JSON-Dokument oder einem JSON-Array. Mit dem Blobindexer in Azure Search können beide Fälle analysiert werden. Dies richtet sich danach, wie Sie den Parameter **parsingMode** in der Anforderung festlegen.

| JSON-Dokument | parsingMode | BESCHREIBUNG | Verfügbarkeit |
|--------------|-------------|--------------|--------------|
| Ein Dokument pro Blob | `json` | JSON-Blobs werden als einzelne Textblöcke analysiert. Jedes JSON-Blob wird zu einem einzelnen Azure Search-Dokument. | Allgemein verfügbar in [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)- und [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer)-APIs. |
| Mehrere Dokumente pro Blob | `jsonArray` | Analysiert ein JSON-Array im Blob, und jedes Element des Arrays wird zu einem separaten Azure Search-Dokument.  | Allgemein verfügbar in [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)- und [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer)-APIs. |


### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

Der erste Schritt umfasst die Bereitstellung der Datenquellen-Verbindungsinformationen, die vom Indexer verwendet werden. Mit dem Datenquellentyp, der hier als `azureblob` angegeben ist, wird bestimmt, welches Datenextraktionsverhalten vom Indexer aufgerufen wird. Bei der JSON-Blobindizierung ist die Datenquellendefinition für JSON-Dokumente und -Arrays gleich. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Schritt 2: Erstellen eines Zielsuchindex 

Indexer sind mit einem Indexschema gekoppelt. Bereiten Sie bei Verwendung der API (anstelle des Portals) vorher einen Index vor, damit Sie ihn für den Indexervorgang angeben können.

Der Index speichert durchsuchbaren Inhalt in Azure Search. Stellen Sie ein Schema bereit, mit dem die Felder in einem Dokument, Attribute und andere Konstrukte für die Suchoberfläche angegeben werden, um einen Index zu erstellen. Wenn Sie einen Index erstellen, der über dieselben Feldnamen und Datentypen wie die Quelle verfügt, entspricht der Indexer der Quelle und den Zielfeldern. Dadurch wird Ihnen die Arbeit erspart, die Felder explizit zuordnen zu müssen.

Im Anschluss sehen Sie ein Beispiel für die Anforderung [Index erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index). Der Index verfügt über ein durchsuchbares `content`-Feld, in dem der aus den Blobs extrahierten Text gespeichert wird:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="step-3-configure-and-run-the-indexer"></a>Schritt 3: Konfigurieren und Ausführen des Indexers

Bisher waren Definitionen für die Datenquelle und den Index parsingMode-agnostisch. Aber in Schritt 3 der Indexerkonfiguration teilt sich die Vorgehensweise auf. Der weitere Weg hängt davon ab, wie der JSON-Blobinhalt analysiert und in einem Azure Search-Index strukturiert werden soll. Zu diesen Möglichkeiten gehören `json` oder `jsonArray`:

+ Legen Sie **parsingMode** auf `json` fest, um jedes Blob als einzelnes Dokument zu indizieren.

+ Legen Sie **parsingMode** auf `jsonArray` fest, wenn Ihre Blobs aus JSON-Arrays bestehen und Sie möchten, dass jedes Element des Arrays zu einem einzelnen Dokument in Azure Search wird. Sie können sich ein Dokument als einzelnes Element in Suchergebnissen vorstellen. Wenn Sie möchten, dass jedes Element im Array in den Suchergebnissen als unabhängiges Element angezeigt wird, verwenden Sie die `jsonArray`-Option.

Innerhalb der Indexerdefinition können Sie optional **Feldzuordnungen** verwenden, um auszuwählen, welche Eigenschaften des JSON-Quelldokuments zum Auffüllen Ihres Zielsuchindex verwendet werden sollen. Für JSON-Arrays gilt Folgendes: Wenn das Array als Eigenschaft auf niedrigerer Ebene vorhanden ist, können Sie einen Dokumentstamm festlegen, mit dem angegeben wird, wo das Array im Blob angeordnet ist.

> [!IMPORTANT]
> Bei Verwendung der Modi `json` oder `jsonArray` geht Azure Search davon aus, dass alle Blobs in Ihrer Datenquelle JSON enthalten. Wenn Sie eine Kombination von JSON- und Nicht-JSON-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).


### <a name="how-to-parse-single-json-blobs"></a>Analysieren von einzelnen JSON-Blobs

In der Standardeinstellung analysiert der [Azure Search-Blob-Indexer](search-howto-indexing-azure-blob-storage.md) JSON-Blobs als einen einzelnen Textblock. Häufig möchten Sie die Struktur Ihrer JSON-Dokumente beibehalten. Angenommen, Sie verwenden in Azure Blob Storage das folgende JSON-Dokument:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Der Blobindexer löst das JSON-Dokument in ein einzelnes Azure Search-Dokument auf. Der Indexer lädt einen Index, indem die Elemente „text“, „datePublished“ und „tags“ aus der Quelle mit Zielindexfeldern abgeglichen werden, die den gleichen Namen und Typ aufweisen.

Die Konfiguration wird im Text eines Indexervorgangs bereitgestellt. Wir wissen, dass mit dem zuvor definierten Datenquellenobjekt der Datenquellentyp und die Verbindungsinformationen angegeben werden. Außerdem muss der Zielindex als leerer Container in Ihrem Dienst vorhanden sein. Zeitplan und Parameter sind optional. Aber wenn Sie diese Angaben weglassen, wird der Indexer sofort ausgeführt, indem `json` als Analysemodus verwendet wird.

Eine vollständige Anforderung kann beispielsweise wie folgt aussehen:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Wie schon erwähnt, sind Feldzuordnungen nicht erforderlich. Bei einem Index mit den Feldern „text“, „datePublished“ und „tags“ kann der Blobindexer die richtige Zuordnung ableiten, ohne dass in der Anforderung eine Feldzuordnung enthalten ist.

### <a name="how-to-parse-json-arrays"></a>Analysieren von JSON-Arrays

Alternativ können Sie auch das JSON-Array-Feature verwenden. Diese Funktion ist nützlich, wenn Blobs ein *Array mit JSON-Objekten* enthalten und jedes Element zu einem separaten Azure Search-Dokument werden soll. Wenn beispielsweise das folgende JSON-Blob vorhanden ist, können Sie Ihren Azure Search-Index mit drei separaten Dokumenten auffüllen, die jeweils die Felder „id“ und „text“ aufweisen.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Für ein JSON-Array sollte die Indexerdefinition dem folgenden Beispiel ähneln. Beachten Sie, dass der parsingMode-Parameter den `jsonArray`-Parser angibt. Die einzigen beiden arrayspezifischen Anforderungen zum Indizieren von JSON-Blobs ist die Angabe des richtigen Parsers und dass die richtigen Daten eingegeben werden.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Beachten Sie auch hierbei wieder, dass die Feldzuordnungen ausgelassen werden können. Angenommen, Sie verfügen über einen Index mit den gleichnamigen Feldern „id“ und „text“, dann kann der Blobindexer die richtige Zuordnung ohne explizite Feldzuordnungsliste ableiten.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>Geschachtelte JSON-Arrays
Was geschieht, wenn Sie ein Array aus JSON-Objekten indizieren möchten, dieses Array aber irgendwo im Dokument geschachtelt ist? Sie können mithilfe der Konfigurationseigenschaft `documentRoot` auswählen, welche Eigenschaft das Array enthält. Angenommen, Ihre Blobs sehen folgendermaßen aus:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Verwenden Sie diese Konfiguration, um das in der `level2`-Eigenschaft enthaltene Array zu indizieren:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

### <a name="using-field-mappings-to-build-search-documents"></a>Verwenden von Feldzuordnungen zum Erstellen von Suchdokumenten

Wenn Quell- und Zielfelder nicht genau aneinander ausgerichtet sind, können Sie im Anforderungstext einen Abschnitt für die Feldzuordnung definieren, um explizite Feld-zu-Feld-Zuordnungen zu ermöglichen.

Derzeit kann Azure Search JSON-Dokumente nicht willkürlich direkt indizieren, da nur primitive Datentypen, Zeichenfolgenarrays und GeoJSON-Punkte unterstützt werden. Sie können jedoch mit **Feldzuordnungen** Teile des JSON-Dokuments auswählen und diese in die Felder der obersten Ebene des Suchdokuments „heben“. Informationen zu den Grundlagen von Feldzuordnungen finden Sie unter [Feldzuordnungen in Azure Search-Indexern](search-indexer-field-mappings.md).

Wir verwenden wieder unser JSON-Beispieldokument:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Angenommen, Sie haben einen Suchindex mit den folgenden Feldern: `text` vom Typ `Edm.String`, `date` vom Typ `Edm.DateTimeOffset` und `tags` vom Typ `Collection(Edm.String)`. Beachten Sie die Abweichung zwischen „datePublished“ in der Quelle und dem Feld `date` im Index. Um das JSON-Objekt in der gewünschten Form zuzuordnen, verwenden Sie die folgenden Feldzuordnungen:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Die Quellenfeldnamen in den Zuordnungen werden mit der [JSON-Zeiger](https://tools.ietf.org/html/rfc6901) -Notation angegeben. Sie beginnen mit einem Schrägstrich, um auf das Stammverzeichnis des JSON-Dokuments zu verweisen, und wählen dann mittels eines schrägstrichgetrennten Weiterleitungspfads den Pfad zur gewünschten Eigenschaft (auf beliebiger Schachtelungsebene) aus.

Sie können auch mit einem nullbasierten Index auf einzelne Arrayelemente verweisen. Um z. B. das erste Element des Arrays „tags“ aus dem obigen Beispiel auszuwählen, verwenden Sie eine Feldzuordnung wie folgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Wenn ein Quellfeldname in einer Feldzuordnung auf eine Eigenschaft verweist, die nicht in JSON vorhanden ist, wird die Zuordnung ohne Fehler übersprungen. Dies geschieht, damit Dokumente mit einem anderen Schema unterstützt werden können (ein häufiger Anwendungsfall). Da keine Überprüfung erfolgt, müssen Sie darauf achten, Tippfehler in Ihrer Feldzuordnungspezifikation zu vermeiden.
>
>

### <a name="rest-example-indexer-request-with-field-mappings"></a>Beispiel für REST: Indexeranforderung mit Feldzuordnungen

Das folgende Beispiel enthält eine vollständig angegebene Indexernutzlast mit Feldzuordnungen:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>Verwenden von .NET SDK

Das .NET SDK ist der REST-API vollständig gleichgestellt. Es wird empfohlen, dass Sie den vorherige Abschnitt zur REST-API genau lesen, um die Konzepte, den Workflow und die Anforderungen zu verstehen. Sie können sich anschließend auf die folgende .NET-API-Referenzdokumentation beziehen, um einen JSON-Indexer in verwalteten Code zu implementieren.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

## <a name="see-also"></a>Weitere Informationen

+ [Indexer in Azure Search](search-indexer-overview.md)
+ [Indizieren von Azure Blob Storage mit Azure Search](search-howto-index-json-blobs.md)
+ [Indizierung von CSV-Blobs mit Azure Search-Blobindexer](search-howto-index-csv-blobs.md)
+ [Tutorial: Durchsuchen von teilweise strukturierten Daten in Azure Blob Storage](search-semi-structured-data.md)
