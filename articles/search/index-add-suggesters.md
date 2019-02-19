---
title: Hinzufügen von Vorschlagsfunktionen zu einem Azure Search-Index
description: Aktiviert Felder für Aktionen für Eingabevorschläge für Abfragen, bei denen vorgeschlagene Abfragen aus Text aus Feldern in einem Azure Search-Index gebildet werden.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 35025d69865aa6890e1cd921e31ac6c26c015789
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56007698"
---
# <a name="add-suggesters-to-an-azure-search-index"></a>Hinzufügen von Vorschlagsfunktionen zu einem Azure Search-Index

Eine **Vorschlagsfunktion** ist ein Azure Search-Konstrukt, das das Feature [Vorschläge während der Eingabe](https://docs.microsoft.com/rest/api/searchservice/suggestions) und das Feature [AutoVervollständigen (Vorschauversion)](search-autocomplete-tutorial.md) unterstützt. Bevor Sie die Vorschlags-API aufrufen können, müssen Sie eine **Vorschlagsfunktion** in einem Index definieren, um Vorschläge auf Basis bestimmter Felder zu ermöglichen.

Eine **Vorschlagsfunktion** verfügt zwar über mehrere Eigenschaften, in erster Linie handelt es sich aber um eine Sammlung von Feldern, für die Sie die [Vorschlags-API](https://docs.microsoft.com/rest/api/searchservice/suggestions) aktivieren. In einer Reise-App können Eingabevorschläge beispielsweise für die Suche nach Zielen, Städten und Attraktionen aktiviert sein. Somit würden alle drei Felder in die Feldsammlung aufgenommen.

Sie können nur eine **Vorschlagsfunktion**-Ressource für jeden Index verwenden (nämlich eine **Vorschlagsfunktion** in der Sammlung **Vorschlagsfunktionen**).

Sie können jederzeit eine **Vorschlagsfunktion** erstellen, aber die Auswirkungen auf Ihren Index basieren auf den Feldern. Neue Felder, die einer Vorschlagsfunktion im Rahmen derselben Aktualisierung hinzugefügt werden, haben am wenigsten Auswirkungen, da keine Neuerstellung des Index erforderlich ist. Das Hinzufügen vorhandener Felder ändert jedoch die Felddefinition, was eine vollständige Neuerstellung des Index erforderlich macht.

## <a name="usage"></a>Verwendung  

 **Vorschlagsfunktionen** funktionieren am besten, wenn damit anstelle von zusammenhangslosen Begriffen oder Ausdrücken spezielle Dokumente vorgeschlagen werden sollen. Als Kandidaten eignen sich am besten Felder für Titel, Namen und sonstige relativ kurze Ausdrücke, anhand derer ein Element identifiziert werden kann. Weniger effektiv sind sich wiederholende Felder, wie etwa Kategorien und Tags, oder extrem lange Felder, etwa für Beschreibungen oder Kommentare.  

Nachdem eine Vorschlagsfunktion erstellt wurde, fügen Sie die [Vorschlags-API](https://docs.microsoft.com/rest/api/searchservice/suggestions) zu Ihrer Abfragelogik hinzu, um das Feature aufzurufen.  

Zu den Eigenschaften, mit denen eine **Vorschlagsfunktion** definiert wird, zählen unter anderem:  

|Eigenschaft|BESCHREIBUNG|  
|--------------|-----------------|  
|`name`|Der Name der **Vorschlagsfunktion**. Verwenden Sie den Namen des der **Vorschlagsfunktion** beim Aufrufen der [Vorschläge &#40;Azure Search-Dienst-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions).|  
|`searchMode`|Die Strategie, mit der nach möglichen Ausdrücken gesucht wird. Derzeit wird nur der Modus `analyzingInfixMatching` unterstützt. Darin werden Ausdrücke am Anfang oder in der Mitte von Sätzen flexibel verglichen.|  
|`sourceFields`|Eine Liste mit einem oder mehreren Feldern, die als Quelle für den Inhalt von Vorschlägen dienen. Als Vorschlagsquellen sind nur Felder vom Typ `Edm.String` und `Collection(Edm.String)` zulässig. Es können nur Felder ohne benutzerdefinierte Sprachanalyse verwendet werden. |  

## <a name="suggester-example"></a>Vorschlagsfunktion (Beispiel)  
 Eine **Vorschlagsfunktion** ist Teil der Indexdefinition. In der aktuellen Version kann in der **Vorschlagsfunktion**-Sammlung neben der **Feldsammlung** und den **Bewertungsprofilen** nur eine **Vorschlagsfunktion** vorhanden sein.  

```  
{  
  "name": "hotels",  
  "fields": [  
     . . .   
   ],  
  "suggesters": [  
    {  
    "name": "sg",  
    "searchMode": "analyzingInfixMatching",  
    "sourceFields": ["hotelName", "category"]  
    }  
  ],  
  "scoringProfiles": [  
     . . .   
  ]  
}  

```  

## <a name="see-also"></a>Weitere Informationen  
 [Erstellen eines Index &#40;Azure Search-Dienst-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
 [Aktualisieren eines Index &#40;Azure Search-Dienst-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/update-index)   
 [Vorschläge &#40;Azure Search-Dienst-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions)   
 [Indexvorgänge &#40;Azure Search-Dienst-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/index-operations)   
 [Azure Search-Dienst-REST-API](https://docs.microsoft.com/rest/api/searchservice/)   
 [Azure Search .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
