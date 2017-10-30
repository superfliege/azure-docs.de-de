---
title: Untersuchen von REST-APIs in Fiddler oder Postman (Azure Search-REST) | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Fiddler oder Postman zum Senden von HTTP-Anforderungen und REST-API-Aufrufen an Azure Search verwenden.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/17/2017
ms.author: heidist
ms.openlocfilehash: d8da3f02fab90e0c690e320736409a4d113d634c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="explore-azure-search-rest-apis-using-fiddler-or-postman"></a>Untersuchen von Azure Search-REST-APIs mit Fiddler oder Postman

Eine der einfachsten Möglichkeiten zum Untersuchen der [Azure Search-REST-API](https://docs.microsoft.com/rest/api/searchservice) ist die Verwendung von Fiddler oder Postman, um HTTP-Anforderungen zu formulieren und die Antworten zu prüfen. In diesem Artikel experimentieren Sie mit Anforderungs- und Antwortnutzlasten, ohne dass Sie Code schreiben müssen.

> [!div class="checklist"]
> * Herunterladen eines Web-API-Testtools
> * Abrufen des API-Schlüssels und Endpunkts für Ihren Suchdienst
> * Konfigurieren von Anforderungsheadern
> * Erstellen eines Index
> * Laden eines Index
> * Durchsuchen eines Index

Wenn Sie kein Azure-Abonnement besitzen, können Sie zuerst ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen und dann die [Registrierung für Azure Search](search-create-service-portal.md) durchführen.

## <a name="download-and-install-tools"></a>Herunterladen und Installieren der Tools

Die folgenden Tools werden häufig für die Webentwicklung eingesetzt, aber auch wenn Sie ein anderes Tool nutzen, sollte die Anleitung in diesem Artikel trotzdem gelten.

+ [Postman (Google Chrome-Add-In)](https://www.getpostman.com/)
+ [Telerik Fiddler](http://www.telerik.com/fiddler)

## <a name="get-the-api-key-and-endpoint"></a>Abrufen des API-Schlüssels und des Endpunkts

Für REST-Aufrufe sind die Dienst-URL und ein Zugriffsschlüssel für jede Anforderung erforderlich. Hierfür wird jeweils ein Suchdienst erstellt. Wenn Sie Azure Search also Ihrem Abonnement hinzugefügt haben, können Sie diese Schritte ausführen, um die erforderlichen Informationen zu erhalten:

1. Öffnen Sie im Azure-Portal im Dashboard die Seite mit dem Suchdienst, oder [suchen Sie nach Ihrem Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in der Liste mit den Diensten.
2. Den Endpunkt erhalten Sie unter **Übersicht** > **Zusammenfassung** > **URL**. Ein Beispiel für einen Endpunkt ist `https://my-service-name.search.windows.net`.
3. Den API-Schlüssel finden Sie unter **Einstellungen** > **Schlüssel**. Aus Redundanzgründen sind zwei Administratorschlüssel vorhanden, falls Sie einen Rollover für Schlüssel durchführen möchten. Mit Administratorschlüsseln werden die Schreibberechtigungen für Ihren Dienst gewährt, die zum Erstellen und Laden von Indizes erforderlich sind. Sie können für Schreibvorgänge entweder den primären oder den sekundären Schlüssel verwenden.

## <a name="configure-request-headers"></a>Konfigurieren von Anforderungsheadern

Jedes Tool behält Anforderungsheaderinformationen für die Sitzung bei. Dies bedeutet, dass Sie URL-Endpunkt, API-Version, API-Schlüssel und Inhaltstyp nur einmal eingeben müssen.

Die vollständige URL sollte bei Ihnen ähnlich wie im folgenden Beispiel aussehen, aber es sollte ein gültiger Wert für den Platzhalternamen **`my-app`** vorhanden sein: `https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01`

Die Komposition der Dienst-URL enthält die folgenden Elemente:

+ HTTPS-Präfix
+ Dienst-URL aus dem Portal
+ Ressource (ein Vorgang, bei dem in Ihrem Dienst ein Objekt erstellt wird). In diesem Schritt ist dies ein Index mit dem Namen „hotels“.
+ API-Version (erforderliche Zeichenfolge in Kleinbuchstaben, die für die aktuelle Version als „?api-version=2016-09-01“ angegeben wird). [API-Versionen](search-api-versions.md) werden regelmäßig aktualisiert. Wenn Sie die API-Version in jede Anforderung einfügen, haben Sie die vollständige Kontrolle darüber, welche jeweils verwendet wird.  

Die Komposition des Anforderungsheaders enthält die beiden Elemente „content-type“ (Inhaltstyp) und „api-key“ (API-Schlüssel), die im vorherigen Abschnitt beschrieben sind:

         content-type: application/json
         api-key: <placeholder>

### <a name="fiddler"></a>Fiddler

Formulieren Sie eine Anforderung, die wie im folgenden Screenshot aussieht. Wählen Sie **PUT** als Verb aus. Fiddler fügt `User-Agent=Fiddler` hinzu. Sie können die beiden zusätzlichen Anforderungsheader in neuen Zeilen darunter einfügen. Fügen Sie den Inhaltstyp und den API-Schlüssel für Ihren Dienst ein, indem Sie den Administratorschlüssel für Ihren Dienst verwenden.

![Fiddler-Anforderungsheader][1]

> [!Tip]
> Sie können den Webdatenverkehr deaktivieren, um überschüssige HTTP-Aktivitäten auszublenden, die für die durchgeführten Aufgaben nicht relevant sind. Navigieren Sie in Fiddler zum Menü **File** (Datei), und deaktivieren Sie die Option **Capture Traffic** (Datenverkehr erfassen). 

### <a name="postman"></a>Postman

Formulieren Sie eine Anforderung, die wie im folgenden Screenshot aussieht. Wählen Sie **PUT** als Verb aus. 

![Postman-Anforderungsheader][6]

## <a name="create-the-index"></a>Erstellen des Index

Der Textkörper der Anforderung enthält die Indexdefinition. Wenn Sie den Anforderungstext hinzufügen, ist die Anforderung zum Erzeugen Ihres Index abgeschlossen.

Neben dem Indexnamen ist die wichtigste Komponente in der Anforderung die Sammlung „fields“. Mit der Sammlung „fields“ wird das Indexschema definiert. Geben Sie für jedes Feld den Typ an. Da Zeichenfolgenfelder in der Volltextsuche verwendet werden, kann es ratsam sein, numerische Daten in Zeichenfolgen umzuwandeln, wenn dieser Inhalt durchsuchbar sein soll.

Die Attribute im Feld bestimmen die zulässige Aktion. Die REST-APIs ermöglichen standardmäßig viele Aktionen. Beispielsweise sind alle Zeichenfolgen durchsuchbar, abrufbar, filterbar und facettierbar. Häufig müssen Sie Attribute nur festlegen, wenn Sie ein Verhalten deaktivieren möchten. Weitere Informationen zu Attributen finden Sie unter [Create Index (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Create Index (Azure Search-Dienst-REST-API)).

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }


Wenn Sie diese Anforderung senden, sollten Sie die Antwort „HTTP 201“ erhalten, mit der angezeigt wird, dass die Erstellung des Index erfolgreich war. Sie können diese Aktion im Portal überprüfen, aber Sie sollten berücksichtigen, dass für die Portalseite bestimmte Aktualisierungsintervalle gelten. Es kann also ein oder zwei Minuten dauern, bis alles angezeigt wird.

Falls Sie eine "HTTP 504"-Antwort erhalten, prüfen Sie, ob die URL mit HTTPS beginnt. Wenn Sie eine "HTTP 400"- oder "HTTP 404"-Antwort erhalten, prüfen Sie den Hauptteil der Anforderung auf Fehler, die durch Kopieren und Einfügen entstanden sind. "HTTP 403" deutet in der Regel auf ein Problem mit dem api-key hin (entweder ein ungültiger Schlüssel oder ein Syntaxproblem bei der Angabe des API-Schlüssels).

### <a name="fiddler"></a>Fiddler

Kopieren Sie die Indexdefinition in den Anforderungstext (ähnlich wie im folgenden Screenshot), und klicken Sie anschließend oben rechts auf **Execute** (Ausführen), um die fertige Anforderung zu senden.

![Fiddler-Anforderungstext][7]

### <a name="postman"></a>Postman

Kopieren Sie die Indexdefinition in den Anforderungstext (ähnlich wie im folgenden Screenshot), und klicken Sie anschließend oben rechts auf **Send** (Senden), um die fertige Anforderung zu senden.

![Postman-Anforderungstext][8]

## <a name="load-documents"></a>Laden von Dokumenten

Das Erstellen und das Auffüllen des Index sind separate Schritte. In Azure Search enthält der Index alle durchsuchbaren Daten, die Sie als JSON-Dokumente bereitstellen können. Informationen zur API für diesen Vorgang finden Sie unter [Add, Update or Delete Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) (Hinzufügen, Aktualisieren oder Löschen von Dokumenten (Azure Search-Dienst-REST-API)).

+ Ändern Sie das Verb für diesen Schritt in **POST**.
+ Ändern Sie den Endpunkt so, dass er `/docs/index` enthält. Die vollständige URL sollte wie folgt lauten: `https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01`
+ Übernehmen Sie die Anforderungsheader unverändert. 

Der Anforderungstext enthält vier Dokumente, die zum Index "hotels" hinzugefügt werden sollen.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

Innerhalb weniger Sekunden sollten Sie eine "HTTP 200"-Antwort in der Sitzungsliste sehen. Dies bedeutet, dass die Dokumente erfolgreich erstellt wurden. 

Falls Sie eine 207-Antwort erhalten, ist der Upload von mindestens einem Dokument fehlgeschlagen. Wenn Sie eine 404-Antwort erhalten, ist entweder im Header oder im Text der Anforderung ein Syntaxfehler enthalten: Vergewissern Sie sich, dass Sie den Endpunkt so geändert haben, dass er `/docs/index` enthält.

> [!Tip]
> Für ausgewählte Datenquellen können Sie als Alternative auch den *Indexer*-Ansatz verwenden, bei dem die für die Indizierung erforderliche Menge an Code vereinfacht und reduziert wird. Weitere Informationen finden Sie unter [Indexer operations](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) (Indexer-Vorgänge).

### <a name="fiddler"></a>Fiddler

Ändern Sie das Verb in **POST**. Ändern Sie die URL so, dass sie `/docs/index` enthält. Kopieren Sie die Dokumente in den Anforderungstext (ähnlich wie im folgenden Screenshot), und führen Sie anschließend die Anforderung aus.

![Fiddler-Anforderungsnutzlast][9]

### <a name="postman"></a>Postman

Ändern Sie das Verb in **POST**. Ändern Sie die URL so, dass sie `/docs/index` enthält. Kopieren Sie die Dokumente in den Anforderungstext (ähnlich wie im folgenden Screenshot), und führen Sie anschließend die Anforderung aus.

![Postman-Anforderungsnutzlast][10]

## <a name="query-the-index"></a>Indexabfragen
Sie haben nun einen Index erstellt und Dokumente hochgeladen und können beginnen, Abfragen auszuführen. Weitere Informationen zu dieser API finden Sie unter [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Search Documents (Azure Search-Dienst-REST-API)).  

+ Ändern Sie das Verb für diesen Schritt in **GET**.
+ Ändern Sie den Endpunkt so, dass er Abfrageparameter enthält, einschließlich Suchzeichenfolgen. Eine Abfrage-URL kann beispielsweise wie folgt lauten: `https://my-app.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2016-09-01`.
+ Übernehmen Sie die Anforderungsheader unverändert.

Mit dieser Abfrage wird nach dem Wort „motel“ gesucht und die Anzahl von Dokumenten in den Suchergebnissen zurückgegeben. Die Anforderung und die Antwort sollten für Postman in etwa wie im folgenden Screenshot aussehen, nachdem Sie auf **Send** (Senden) geklickt haben. Der Statuscode sollte 200 lauten.

 ![Postman-Abfrageantwort][11]

### <a name="tips-for-running-our-sample-queries-in-fiddler"></a>Tipps zum Ausführen unserer Beispielabfragen in Fiddler

Die folgende Beispielabfrage stammt aus dem Artikel [Search Documents (Azure Search Service REST API)](http://msdn.microsoft.com/library/dn798927.aspx) (Search Documents (Azure Search-Dienst-REST-API)). Viele der Beispielabfragen in diesem Artikel enthalten Leerzeichen, die von Fiddler nicht unterstützt werden. Ersetzen Sie alle Leerzeichen vor dem Einfügen durch ein +-Zeichen, bevor Sie die Abfrage in Fiddler ausführen.

**Vor dem Ersetzen der Leerzeichen (in „lastRenovationDate desc“):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

**Nach dem Ersetzen der Leerzeichen durch „+“ (in „lastRenovationDate+desc“):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-index-properties"></a>Abfragen von Indexeigenschaften
Sie können Systeminformationen auch abfragen, um die Anzahl von Dokumenten oder die Speicherauslastung zu erhalten: `https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01`.

In Postman sollte Ihre Anforderung in etwa wie folgt aussehen, und die Antwort enthält dann eine Anzahl von Dokumenten und den belegten Speicher in Byte.

 ![Postman-Systemabfrage][12]

Beachten Sie, dass die Syntax für die API-Version hier anders ist. Verwenden Sie für diese Anforderung `?`, um die API-Version anzufügen. Mit dem Zeichen „?“ wird der URL-Pfad von der Abfragezeichenfolge getrennt, und mit „&“ wird jeweils ein Name-Wert-Paar in der Abfragezeichenfolge getrennt. Für diese Abfrage ist die API-Version das erste und einzige Element in der Abfragezeichenfolge.

Weitere Informationen zu dieser API finden Sie unter [Get Index Statistics (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) (Get Index Statistics (Azure Search-Dienst-REST-API)).


### <a name="tips-for-viewing-index-statistic-in-fiddler"></a>Tipps zum Anzeigen der Indexstatistik in Fiddler

Klicken Sie in Fiddler auf die Registerkarte **Inspectors** (Inspektoren) und dann auf die Registerkarte **Headers** (Header), und wählen Sie das JSON-Format aus. Sie sollten nun die Dokumentenanzahl und die Speichergröße (in KB) sehen.

## <a name="next-steps"></a>Nächste Schritte

REST-Clients sind sehr nützlich für spontane Erkundungen, aber da Sie jetzt mit der Funktionsweise der REST-APIs vertraut sind, können Sie mit dem Code fortfahren. Die nächsten Schritte finden Sie unter den folgenden Links:

+ [Erstellen eines Azure Search-Indexes mit der REST-API](search-create-index-rest-api.md)
+ [Hochladen von Daten in Azure Search über die REST-API](search-import-data-rest-api.md)
+ [Abfragen des Azure Search-Indexes mit der REST-API](search-query-rest-api.md)

<!--Image References-->
[1]: ./media/search-fiddler/fiddler-url.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-fiddler/postman-url.png
[7]: ./media/search-fiddler/fiddler-request.png
[8]: ./media/search-fiddler/postman-request.png
[9]: ./media/search-fiddler/fiddler-docs.png
[10]: ./media/search-fiddler/postman-docs.png
[11]: ./media/search-fiddler/postman-query.png
[12]: ./media/search-fiddler/postman-system-query.png