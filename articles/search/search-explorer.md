---
title: Abfragen eines Index (Portal – Azure Search) | Microsoft-Dokumentation
description: Eingeben einer Suchabfrage in den Suchexplorer im Azure-Portal.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.topic: quickstart
ms.date: 07/10/2017
ms.author: heidist
ms.openlocfilehash: f952587150fa2d49709bdce6cbc5fe1a7aea16b1
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a>Abfragen eines Azure Search-Index mithilfe des Suchexplorers im Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie einen Azure Search-Index mithilfe des **Suchexplorers** im Azure-Portal abfragen. Mit dem Suchexplorer können Sie einfache oder vollständige Lucene-Abfragezeichenfolgen an einen beliebigen Index in Ihrem Dienst übermitteln.

## <a name="open-the-service-dashboard"></a>Öffnen des Servicedashboards
1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) auf der Navigationsleiste auf **Alle Ressourcen**.
2. Wählen Sie Ihren Azure Search-Dienst aus.

## <a name="select-an-index"></a>Auswählen eines Index

Wählen Sie über die Kachel **Indizes** den Index aus, den Sie durchsuchen möchten.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Öffnen des Suchexplorers

Klicken Sie auf die Kachel für den Suchexplorer, um die Suchleiste und den Ergebnisbereich zu öffnen.

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