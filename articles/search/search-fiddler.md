---
title: 'Gewusst wie: Verwenden von Fiddler zum Auswerten und Testen von Azure Search REST-APIs | Microsoft Docs'
description: "Verwenden Sie Fiddler für einen Code-Ansatz zum Überprüfen der Verfügbarkeit von Azure Search und testen die REST-APIs."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
ms.openlocfilehash: c38b73fa69bee34ce2434c6274cb017c99ef3c35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Verwenden von Fiddler zum Auswerten und Testen von Azure Search REST-APIs
> [!div class="op_single_selector"]
>
> * [Übersicht](search-query-overview.md)
> * [-Explorer durchsuchen](search-explorer.md)
> * [Fiddler](search-fiddler.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

In diesem Artikel wird erläutert, wie Fiddler, als verfügbar verwenden eine [kostenlos von Telerik](http://www.telerik.com/fiddler), um HTTP-Anforderungen zum Ausstellen und Anzeigen von Antworten mithilfe der Azure-Suchdienst-REST-API ohne Code schreiben zu müssen. Azure-Suchdienst ist vollständig verwalteten, Cloud-Suchdiensts in Microsoft Azure, .NET und REST-APIs leichter programmierbare gehostet. REST-APIs sind dokumentiert, auf Azure-Suchdienst [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

In den folgenden Schritten müssen Sie einen Index erstellen, Hochladen von Dokumenten, Abfragen den Index und dann Abfragen das System Service-Informationen.

Um diese Schritte abzuschließen, benötigen Sie ein Azure-Suchdiensts und `api-key`. Finden Sie unter [Erstellen eines Azure-Suchdiensts im Portal](search-create-service-portal.md) Anleitungen für den Einstieg.

## <a name="create-an-index"></a>Erstellen Sie einen index
1. Starten Sie Fiddler. Auf der **Datei** Menü deaktivieren **Datenverkehr erfassen** , irrelevante HTTP-Aktivitäten zu verbergen, die nicht mit der aktuellen Aufgabe verknüpft ist.
2. Auf der **Composer** Registerkarte, müssen Sie eine Anforderung im folgenden Screenshot sieht formulieren.

      ![][1]
3. Wählen Sie **PUT**.
4. Geben Sie eine URL, die die Dienst-URL, Anforderungsattribute und die api-Version angibt. Einige Zeiger zu bedenken:

   * Verwenden Sie HTTPS als Präfix an.
   * Anforderung-Attribut ist "/ Indexes/Hotels". Dies teilt die Suche einen Index mit dem Namen "Hotels" erstellen.
   * API-Version ist Kleinbuchstaben, angegeben als "? api-Version = 2016-09-01". API-Versionen sind wichtig, da Azure Search regelmäßig Updates bereitgestellt. In seltenen Fällen kann ein Dienstupdate eine unterbrechende Änderung an der API führen. Aus diesem Grund erfordert Azure Search eine api-Version für jede Anforderung, damit Sie vollständige Kontrolle sind über welche verwendet wird.

     Die vollständige URL sollte ähnlich wie im folgenden Beispiel aussehen.

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. Geben Sie den Anforderungsheader, ersetzen den Host und den api-Schlüssel mit Werten, die für den Dienst gültig sind.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. Fügen Sie im Anforderungstext in den Feldern, die die Definition des Indexes bilden.

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
7. Klicken Sie auf **ausführen**.

In wenigen Sekunden sehen Sie eine Antwort "HTTP 201" in der Sitzungsliste, der angibt, dass der Index wurde erfolgreich erstellt wurde.

Wenn Sie HTTP 504 erhalten haben, stellen Sie sicher, dass die URL HTTPS angibt. Wenn Sie HTTP 400 oder 404 angezeigt wird, überprüfen Sie den Hauptteil der Anforderung, um sicherzustellen, dass keine Fehler kopieren und einfügen. Eine HTTP 403 gibt in der Regel ein Problem mit der api-Schlüssel (ein ungültiger Schlüssel oder ein Problem Syntax mit wie der api-Schlüssel angegeben wird).

## <a name="load-documents"></a>Laden von Dokumenten
Auf der **Composer** Registerkarte Ihre Anfrage zum Dokumente bereitgestellt wird wie folgt aussehen. Der Text der Anforderung enthält die Suchdaten Bucket für Hotels, 4.

   ![][2]

1. Wählen Sie **POST**.
2. Geben Sie eine URL, die Folgen beginnt mit HTTPS, gefolgt von Ihrem Dienst-URL "/indexes/ < Indexname >/Dokumente/Index? api-Version = 2016-09-01". Die vollständige URL sollte ähnlich wie im folgenden Beispiel aussehen.

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. Anforderungsheader sollten identisch sein. Denken Sie daran, dass Sie den Host und den api-Schlüssel durch die Werte ersetzt, die für den Dienst gültig sind.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Der Anforderungstext enthält vier Dokumente, die die Hotels Index hinzugefügt werden.

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
5. Klicken Sie auf **ausführen**.

In ein paar Sekunden sollte eine Antwort "HTTP 200" in der Sitzungsliste angezeigt werden. Dies gibt an, dass die Dokumente erfolgreich erstellt wurden. Wenn Sie eine 207 erhalten, konnte nicht über mindestens ein Dokument hochladen. Wenn Sie eine 404 erhalten, müssen Sie einen Syntaxfehler in der Kopf- oder der Text der Anforderung.

## <a name="query-the-index"></a>Abfragen von index
Nun, dass ein Index und Dokumente geladen werden, können Sie Abfragen für diese ausgeben.  Auf der **Composer** Registerkarte ein **abrufen** -Befehl, der Ihr Dienst abgefragt wird im folgenden Screenshot ähnelt.

   ![][3]

1. Wählen Sie **abrufen**.
2. Geben Sie eine URL, die Folgen beginnt mit HTTPS, gefolgt von Ihrem Dienst-URL "/indexes/ < Indexname > / Docs?", gefolgt von Abfrageparametern. Verwenden Sie beispielsweise die folgende URL ein, und Ersetzen Sie den Hostnamen Beispiel durch einen gültigen für Ihren Dienst aus.

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   Diese Abfrage sucht nach dem Begriff "Autohof" und ruft das Facet Kategorien für Bewertungen ab.
3. Anforderungsheader sollten identisch sein. Denken Sie daran, dass Sie den Host und den api-Schlüssel durch die Werte ersetzt, die für den Dienst gültig sind.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

Der Antwortcode 200 werden soll, und die antwortausgabe sollte ähnlich wie im folgenden Screenshot aussehen.

   ![][4]

Die folgende Beispielabfrage stammt aus dem [Suchindex-Vorgang (Azure Search-API)](http://msdn.microsoft.com/library/dn798927.aspx) auf MSDN. Viele der Beispiele für Abfragen in diesem Thema werden Räume, wobei in Fiddler nicht zulässig sind. Jedes Leerzeichen mit a + Zeichen vor dem Einfügen in die Abfrage string, bevor Sie versuchen, die Abfrage in Fiddler ersetzen.

**Bevor ein Leerzeichen ersetzt werden:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

**Nachdem durch Leerzeichen ersetzt werden +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-the-system"></a>Abfragen des Systems
Sie können auch das System zum Abrufen von Dokument Anzahlen und Speicherverbrauch Abfragen. Auf der **Composer** Registerkarte Ihre Anforderung sieht etwa wie folgt und Zurückgeben der Antwort eine Anzahl für die Anzahl der Dokumente und verwendeter Speicherplatz.

 ![][5]

1. Wählen Sie **abrufen**.
2. Geben Sie eine URL, die die Dienst-URL, gefolgt von "/ Indizes/Hotels/Stats? api-Version = 2016-09-01":

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. Geben Sie den Anforderungsheader, ersetzen den Host und den api-Schlüssel mit Werten, die für den Dienst gültig sind.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Der Anforderungstext leer sein.
5. Klicken Sie auf **ausführen**. Daraufhin sollte einen Statuscode "HTTP 200" in der Sitzungsliste. Wählen Sie den Eintrag, der für den Befehl bereitgestellt wurde.
6. Klicken Sie auf die **Inspektoren** Registerkarte, klicken Sie auf die **Header** Registerkarte, und wählen Sie dann das JSON-Format. Daraufhin sollte die Dokumentgröße Anzahl und den Speicher (in KB).

## <a name="next-steps"></a>Nächste Schritte
Finden Sie unter [Verwalten Ihres Suchdiensts in Azure](search-manage.md) für ein ohne-Verwaltung und Verwendung von Azure Search eine Methode.

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
