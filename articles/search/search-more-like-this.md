---
title: moreLikeThis in Azure Search (Vorschau) | Microsoft-Dokumentation
description: "Vorläufige Dokumentation für das Feature moreLikeThis (Vorschau), das in der REST-API für Azure Search verfügbar gemacht wird."
services: search
documentationCenter: na
authors: mhko
manager: jlembicz
editor: na
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: nateko
ms.openlocfilehash: 20f6a1166d47799f15275cb5b6a9ea4934ae57e2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis in Azure Search (Vorschau)

`moreLikeThis=[key]` ist ein Abfrageparameter in der [Such-API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Durch Angabe des Parameters `moreLikeThis` in einer Suchabfrage können Sie Dokumente suchen, die mit dem über den Dokumentschlüssel angegebenen Dokument übereinstimmen. Beim Ausführen einer Suchanforderung mit `moreLikeThis` wird eine Abfrage mit Suchbegriffen generiert, die aus dem angegebenen Dokument extrahiert werden und das Dokument am besten beschreiben. Anhand der generierten Abfrage wird dann die Suchanforderung erstellt. Standardmäßig wird der Inhalt aller Felder mit dem Attribut `searchable` berücksichtigt, es sei denn, die Felder werden durch den Parameter `searchFields` eingeschränkt. Der Parameter `moreLikeThis` kann nicht mit dem Suchparameter `search=[string]` verwendet werden.

## <a name="examples"></a>Beispiele 

Unten ist ein Beispiel für eine moreLikeThis-Abfrage angegeben. Mit der Abfrage werden Dokumente gesucht, deren Beschreibungsfelder dem Feld des Quelldokuments entsprechend der Angabe durch den Parameter `moreLikeThis` am ähnlichsten sind.

```  
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```  

```  
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {  
      "moreLikeThis": "1002",  
      "searchFields": "description"  
    }  
```  

## <a name="feature-availability"></a>Verfügbarkeit von Funktionen

Das Feature „moreLikeThis“ befindet sich derzeit in der Vorschauphase und wird nur in den API-Vorschauversionen `2015-02-28-Preview` und `2016-09-01-Preview` unterstützt. Da die API-Version in der Anforderung angegeben ist, können allgemein verfügbare und Vorschau-APIs in derselben App kombiniert werden. Vorschauversionen von APIs sind nicht durch ein SLA abgedeckt, und die Features können sich ändern, sodass die Verwendung in Produktionsanwendungen nicht empfehlenswert ist.