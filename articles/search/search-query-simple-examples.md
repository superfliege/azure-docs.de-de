---
title: Einfache Abfragebeispiele für Azure Search | Microsoft-Dokumentation
description: Enthält einfache Abfragebeispiele für die Volltextsuche, Filtersuche, geografische Suche, Facettensuche und andere Abfragezeichenfolgen, die zum Abfragen eines Azure Search-Index verwendet werden.
author: HeidiSteen
manager: cgronlun
tags: Simple query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: heidist
ms.openlocfilehash: e4bb72eb8ad6f15b0e5bc14e0e07556e76d0477b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39368598"
---
# <a name="simple-syntax-query-examples-for-building-queries-in-azure-search"></a>Abfragebeispiele mit einfacher Syntax zum Erstellen von Abfragen in Azure Search

Bei der [einfachen Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) wird der Standardabfrageparser aufgerufen, um Volltextsuchabfragen für einen Azure Search-Index auszuführen. Die einfache Abfrageanalyse ist schnell und verarbeitet gängige Szenarien in Azure Search, z.B. Volltextsuche, Filter- und Facettensuche und geografische Suche. In diesem Artikel werden anhand von Beispielen Abfragevorgänge, die bei Verwendung der einfachen Syntax verfügbar sind, Schritt für Schritt beschrieben.

Die Alternative zu dieser Abfragesyntax ist die [vollständige Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), bei der auch komplexere Abfragestrukturen unterstützt werden, z.B. Fuzzy- und Platzhaltersuche, sodass sich die Verarbeitungsdauer verlängern kann. Weitere Informationen und Beispiele zur Veranschaulichung der vollständigen Syntax finden Sie unter [Beispiele für Lucene-Syntaxabfragen](search-query-lucene-examples.md).

## <a name="formulate-requests-in-postman"></a>Formulieren von Anforderungen in Postman

Der in den folgenden Beispielen verwendete Suchindex „NYC Jobs“ besteht aus Stellenangeboten basierend auf einem Dataset, das von der Initiative [City of New York OpenData](https://nycopendata.socrata.com/) bereitgestellt wird. Diese Daten sollten weder als aktuell noch als vollständig betrachtet werden. Der Index wird über einen Sandboxdienst von Microsoft bereitgestellt. Dies bedeutet, dass Sie kein Azure-Abonnement und keine Azure Search-Instanz benötigen, um diese Abfragen auszuprobieren.

Sie benötigen lediglich Postman oder ein gleichwertiges Tool zum Senden einer HTTP-Anforderung per GET. Weitere Informationen finden Sie unter [Testen mit REST-Clients](search-fiddler.md).

### <a name="set-the-request-header"></a>Festlegen des Anforderungsheaders

1. Legen Sie im Anforderungsheader **Content-Type** auf `application/json` fest.

2. Fügen Sie einen **api-key** hinzu, und legen Sie ihn auf diese Zeichenfolge fest: `252044BE3886FE4A8E3BAA4F595114BB`. Dies ist ein Abfrageschlüssel für den Sandbox-Suchdienst, der den Index „NYC Jobs“ hostet.

Nachdem Sie den Anforderungsheader angegeben haben, können Sie ihn für alle Abfragen in diesem Artikel wiederverwenden, indem Sie lediglich die Zeichenfolge **search=** austauschen. 

  ![Postman-Anforderungsheader](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>Festlegen der Anforderungs-URL

Die Anforderung ist ein GET-Befehl, der mit einer URL gekoppelt ist, in der der Azure Search-Endpunkt und die Suchzeichenfolge enthalten sind.

  ![Postman-Anforderungsheader](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

Die URL-Komposition umfasst die folgenden Elemente:

+ **`https://azs-playground.search.windows.net/`** ist ein Sandbox-Suchdienst, der vom Entwicklungsteam von Azure Search gewartet wird. 
+ **`indexes/nycjobs/`** ist der Index „NYC Jobs“ in der Indexsammlung dieses Diensts. Sowohl der Dienstname als auch der Index sind für die Anforderung erforderlich.
+ **`docs`** ist die Dokumentsammlung, die den gesamten durchsuchbaren Inhalt enthält. Der im Anforderungsheader angegebene Abfrage-API-Schlüssel funktioniert nur für Lesevorgänge, die die Dokumentsammlung betreffen.
+ **`api-version=2017-11-11`** legt die API-Version fest. Dies ist für alle Anforderungen ein erforderlicher Parameter.
+ **`search=*`** ist die Abfragezeichenfolge, die in der ersten Abfrage null ist und (standardmäßig) die ersten 50 Ergebnisse zurückgibt.

## <a name="send-your-first-query"></a>Senden Ihrer ersten Abfrage

Fügen Sie als Überprüfungsschritt die folgende Anforderung in GET ein, und klicken Sie auf **Senden**. Die Ergebnisse werden als ausführliche JSON-Dokumente zurückgegeben. Sie können diese URL kopieren und unten in das erste Beispiel einfügen.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=*
  ```

Die Abfragezeichenfolge **`search=*`** entspricht einer Suche ohne Angaben, die einer NULL-Suche oder leeren Suche gleicht. Dies hat keinen besonderen Nutzen, aber es ist die einfachste Suche, die Sie durchführen können.

Optional können Sie der URL **`$count=true`** hinzufügen, um eine Anzahl für die Dokumente zurückzugeben, die die Suchkriterien erfüllen. In einer leeren Suchzeichenfolge entspricht dies allen Dokumenten im Index (2802 für „NYC Jobs“).

## <a name="how-to-invoke-simple-query-parsing"></a>Aufrufen der einfachen Abfrageanalyse

Für interaktive Abfragen müssen Sie nichts angeben: einfache Abfragen sind die Standardeinstellung. Wenn Sie zuvor **queryType=full** für die vollständige Abfragesyntax aufgerufen haben, können Sie im Code die Standardeinstellung wiederherstellen, indem Sie **queryType=simple** verwenden.

## <a name="example-1-field-scoped-query"></a>Beispiel 1: Feldbezogene Abfrage

Die erste Abfrage ist nicht syntaxspezifisch (sie funktioniert sowohl für einfache als auch für vollständige Syntax). Wir beginnen mit diesem Beispiel, um ein Grundkonzept für eine Abfrage vorzustellen, bei der eine relativ gut lesbare JSON-Antwort produziert wird. Um das Beispiel kurz zu halten, zielt die Abfrage nur auf das Feld *business_title* ab, und es werden nur Berufsbezeichnungen zurückgegeben. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&search=*
```

Der Parameter **searchFields** begrenzt die Suche auf das Feld mit den Berufsbezeichnungen. Mit dem Parameter **select** wird ermittelt, welche Felder in das Resultset einbezogen werden.

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  ![Postman-Beispielantwort](media/search-query-lucene-examples/postman-sample-results.png)

Unter Umständen haben Sie bemerkt, dass für jedes Dokument auch die Suchbewertung zurückgegeben wird, obwohl keine Suchbewertungen angegeben wurden. Der Grund ist, dass es sich bei der Suchbewertung um Metadaten handelt, bei denen der Wert die Rangfolge der Ergebnisse angibt. Zu einer einheitlichen Bewertung von „1“ kommt es, wenn kein Rang vorhanden ist, weil es entweder keine Volltextsuche war oder weil keine anzuwendenden Kriterien vorhanden waren. Bei einer NULL-Suche gibt es keine Kriterien, und die zurückgegebenen Zeilen haben eine willkürliche Reihenfolge. Wenn die Suchkriterien genauer definiert sind, werden Sie sehen, dass sich für die Suchbewertungen aussagekräftigere Werte ergeben.

## <a name="example-2-look-up-by-id"></a>Beispiel 2: Suchen anhand der ID

Dieses Beispiel ist etwas ungewöhnlich. Beim Auswerten des Suchverhaltens ist es aber ratsam, den gesamten Inhalt eines Dokuments zu untersuchen, um zu ermitteln, warum er in die Ergebnisse einbezogen bzw. ausgeschlossen wurde. Verwenden Sie zum Zurückgeben eines gesamten Dokuments einen [Suchvorgang](https://docs.microsoft.com/rest/api/searchservice/lookup-document), um die Dokument-ID zu übergeben.

Alle Dokumente verfügen über einen eindeutigen Bezeichner. Beim Ausprobieren der Syntax für eine Suchabfrage können Sie zuerst eine Liste mit Dokument-IDs zurückgeben, um eine geeignete ID auszuwählen. Für „NYC Jobs“ sind diese Bezeichner im Feld `id` gespeichert.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=id&$select=id&search=*
 ```

Das nächste Beispiel ist eine Suchabfrage, bei der ein bestimmtes Dokument basierend auf `id` „9E1E3AF9-0660-4E00-AF51-9B654925A2D5“ ausgegeben wird, das zuerst in der vorherigen Antwort enthalten war. Mit der folgenden Abfrage wird das gesamte Dokument zurückgegeben, nicht nur die ausgewählten Felder. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2017-11-11&$count=true&search=*
 ```

## <a name="example-3-search-precision"></a>Beispiel 3: Suchgenauigkeit

Begriffsabfragen sind einzelne Begriffe (ggf. in größerer Zahl), die unabhängig voneinander ausgewertet werden. Ausdrucksabfragen sind in Anführungszeichen gesetzt und werden Wort für Wort als Zeichenfolge ausgewertet. Die Genauigkeit der Übereinstimmung wird mithilfe von Operatoren und des searchMode-Elements gesteuert.

Beispiel 1: **`&search=fire`** gibt 150 Ergebnisse zurück, wobei alle Übereinstimmungen das Wort „fire“ an einer Stelle im Dokument enthalten.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire
```

Beispiel 2: **`&search=fire department`** gibt 2.002 Ergebnisse zurück. Übereinstimmungen werden für Dokumente zurückgegeben, die entweder den Begriff „fire“ oder „department“ enthalten.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire department
```

Beispiel 3: **`&search="fire department"`** gibt 82 Ergebnisse zurück. Wenn die Zeichenfolge in Anführungszeichen gesetzt wird, wird für beide Begriffe eine „wörtliche“ Suche durchgeführt, und es werden Übereinstimmungen für tokenisierte Begriffe im Index gefunden, die aus einer Kombination der Begriffe bestehen. Dies ist der Grund dafür, warum die Suche nach **`search=+fire +department`** hiermit nicht gleichwertig ist. Beide Begriffe sind erforderlich, aber es wird unabhängig danach gesucht. 

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search="fire department"
```

## <a name="example-4-booleans-with-searchmode"></a>Beispiel 4: Boolesche Werte mit searchMode

Bei der einfachen Syntax werden boolesche Operatoren in Form von Zeichen (`+, -, |`) unterstützt. Mit dem Parameter „searchMode“ werden Kompromisse in Bezug auf die Genauigkeit und Trefferquote angegeben, wobei bei `searchMode=any` die Trefferquote (eine Übereinstimmung mit einem der Kriterien qualifiziert ein Dokument für das Resultset) und bei `searchMode=all` die Genauigkeit (alle Kriterien müssen übereinstimmen) im Mittelpunkt steht. Die Standardeinstellung ist `searchMode=any`. Dies kann verwirrend sein, wenn Sie für eine Abfrage mehrere Operatoren verwenden und keinen engeren Bereich von Ergebnissen erhalten, sondern einen weiteren Bereich. Dies gilt besonders für den Operator NOT, bei dem die Ergebnisse alle Dokumente umfassen, die einen bestimmten Begriff „nicht enthalten“.

Bei Verwendung der Standardeinstellung für „searchMode“ (any) werden 2.800 Dokumente zurückgegeben: Dokumente, die den mehrteiligen Begriff „fire department“ enthalten, sowie alle Dokumente, die den Begriff „Metrotech Center“ nicht enthalten.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```
Wenn Sie searchMode in `all` ändern, kommt es für die Kriterien zu einem kumulativen Effekt, und es wird ein kleineres Resultset mit nur 21 Dokumenten zurückgegeben. Dies sind Dokumente, die den gesamten Begriff „fire department“ enthalten, abzüglich der Jobs unter der Metrotech Center-Adresse.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```


## <a name="example-5-structuring-results"></a>Beispiel 5: Strukturieren von Ergebnissen

Mit mehreren Parametern wird gesteuert, welche Felder in den Suchergebnissen enthalten sind, wie viele Dokumente in jedem Batch zurückgegeben werden und welche Sortierreihenfolge verwendet wird. In diesem Beispiel wird auf einige der vorherigen Beispiele zurückgegriffen, indem Ergebnisse mit der **$select**-Anweisung auf bestimmte Felder beschränkt und „wörtliche“ Suchkriterien verwendet werden. Es werden 82 Übereinstimmungen zurückgegeben. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Basierend auf dem vorherigen Beispiel können Sie nach der Bezeichnung sortieren. Diese Sortierung funktioniert, weil „civil_service_title“ im Index *sortierbar* ist.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

Die Auslagerungsergebnisse werden mit dem Parameter **$top** implementiert, und in diesem Fall werden die obersten fünf Dokumente zurückgegeben:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Überspringen Sie den ersten Batch, um die nächsten fünf Dokumente zu erhalten:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Nächste Schritte
Versuchen Sie, Abfragen in Ihrem Code anzugeben. Unter den folgenden Links wird erläutert, wie Sie Suchabfragen sowohl für .NET als auch für die REST-API einrichten, indem Sie die einfache Standardsyntax verwenden.

* [Abfragen des Azure Search-Indexes mit dem .NET SDK](search-query-dotnet.md)
* [Abfragen des Azure Search-Indexes mit der REST-API](search-query-rest-api.md)

Eine zusätzliche Syntaxreferenz, eine Abfragearchitektur und Beispiele finden Sie unter den folgenden Links:

+ [Beispiele für die Lucene-Abfragesyntax zum Erstellen von Abfragen in Azure Search](search-query-lucene-examples.md)
+ [Funktionsweise der Volltextsuche in Azure Search](search-lucene-query-architecture.md)
+ [Einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene query syntax in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (Lucene-Abfragesyntax in Azure Search)
