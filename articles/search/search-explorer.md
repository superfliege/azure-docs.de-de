---
title: Suchexplorer zum Abfragen von Indizes in Azure Search | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mit dem Suchexplorer Indizes in Azure Search abfragen.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 520d9e7b1899c54d922ff6fb77e0901f9609b029
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004132"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Gewusst wie: Abfragen von Indizes in Azure Search mit dem Suchexplorer 

In diesem Artikel erfahren Sie, wie Sie einen vorhandenen Azure Search-Index mithilfe des **Suchexplorers** im Azure-Portal abfragen. Mit dem Suchexplorer können Sie einfache oder vollständige Lucene-Abfragezeichenfolgen an einen beliebigen Index in Ihrem Dienst übermitteln.

## <a name="open-the-service-dashboard"></a>Öffnen des Servicedashboards
1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) auf der Navigationsleiste auf **Alle Ressourcen**.
2. Wählen Sie Ihren Azure Search-Dienst aus.

## <a name="select-an-index"></a>Auswählen eines Index

Wählen Sie über die Kachel **Indizes** den Index aus, den Sie durchsuchen möchten.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Öffnen des Suchexplorers

Klicken Sie auf die Kachel „Suchexplorer“, um die Suchleiste und den Ergebnisbereich zu öffnen.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Beginnen Sie mit der Suche.

Bei Verwendung des Suchexplorers können Sie [Abfrageparameter](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) angeben, um die Abfrage zu formulieren.

1. Geben Sie unter **Abfragezeichenfolge** eine Abfrage ein, und klicken Sie anschließend auf **Suchen**. 

   Die Abfragezeichenfolge wird automatisch in die richtige Anforderungs-URL umgewandelt, um eine HTTP-Anforderung für die Azure Search REST-API zu übermitteln.   
   
   Die Anforderung kann auf der Grundlage einer beliebigen gültigen einfachen oder vollständigen Lucene-Abfragesyntax erstellt werden. Das `*`-Zeichen entspricht einer leeren oder unspezifischen Suche, die alle Dokumente ohne bestimmte Reihenfolge zurückgibt.

2. Im Abschnitt **Ergebnisse** werden die Abfrageergebnisse in einem JSON-Rohformat dargestellt (entsprechend der Nutzlast, die in einem HTTP-Antworttext zurückgegeben wird, wenn Anforderungen programmgesteuert ausgegeben werden).

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Ressourcen enthalten zusätzliche Informationen und Beispiele für die Abfragesyntax:

 + [Einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Beispiele für die Lucene-Abfragesyntax](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData-Filterausdruckssyntax](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 