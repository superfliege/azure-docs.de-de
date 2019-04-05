---
title: Arbeiten mit großen Datasets
description: Hier erfahren Sie, wie Sie bei der Verwendung von Azure Resource Graph umfangreiche Datasets abrufen und steuern.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/26/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: ef61314ae124668fc8970e6d68a0f927bdf771bc
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889034"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Arbeiten mit großen Datasets von Azure-Ressourcen

Azure Resource Graph ist für das Arbeiten mit Informationen zu Ressourcen in Ihrer Azure-Umgebung und das Abrufen von diesen konzipiert. Mit Resource Graph erhalten Sie diese Daten schnell, selbst beim Abfragen von Tausenden von Datensätzen. Resource Graph umfasst mehrere Optionen für das Arbeiten mit diesen großen Datasets.

## <a name="data-set-result-size"></a>Ergebnisgröße von Datasets

Standardmäßig sind in Resource Graph alle Abfragen so beschränkt, dass nur **100** Datensätze zurückgegeben werden. Durch diese Steuerung werden sowohl Benutzer als auch der Dienst vor unbeabsichtigten Abfragen geschützt, die zu großen Datasets führen. Dieser Fall tritt meistens auf, wenn ein Kunde Abfragen testet, um Ressourcen seinen speziellen Anforderungen entsprechend zu suchen und zu filtern. Dieses Steuerelement unterscheidet sich von der Verwendung der Azure Data Explorer-Sprachoperatoren [top](/azure/kusto/query/topoperator) und [limit](/azure/kusto/query/limitoperator) zum Einschränken der Ergebnisse.

> [!NOTE]
> Bei Verwendung von **First** sollten die Ergebnisse nach mindestens einer Spalte mit `asc` oder `desc` sortiert werden. Ohne Sortierung sind die zurückgegebenen Ergebnisse zufällig und nicht wiederholbar.

Das Standardlimit kann über alle Methoden für die Interaktion mit Resource Graph überschrieben werden. Die folgenden Beispiele zeigen, wie die Größenbeschränkung von Datasets in _200_ geändert wird:

```azurecli-interactive
az graph query -q "project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -First 200
```

In der [REST-API](/rest/api/azureresourcegraph/resources/resources) wird das Steuerelement **$top** verwendet, das Teil von **QueryRequestOptions** ist.

Das jeweils _restriktivste_ Steuerelement hat Vorrang. Wenn in einer Abfrage beispielsweise die Operatoren **top** oder **limit** verwendet werden und sich dabei mehr Datensätze als bei **First** ergeben, entspricht die maximale Anzahl der zurückgegebenen Datensätze der durch **First** vorgegebenen Anzahl. Wenn **top** oder **limit** kleiner ist als **First**, wird als Datensatzgruppe jeweils der kleinere Wert der durch **top** oder **limit** konfigurierten Werte zurückgegeben.

Für **First** ist derzeit _5000_ als maximal zulässiger Wert festgelegt.

## <a name="skipping-records"></a>Überspringen von Datensätzen

Als weitere Option für das Arbeiten mit großen Datasets kann das Steuerelement **Skip** verwendet werden. Über dieses Steuerelement kann bei der Abfrage die definierte Anzahl von Datensätzen vor dem Zurückgeben der Ergebnisse übersprungen werden. **Skip** eignet sich für Abfragen, die Ergebnisse auf sinnvolle Weise sortieren, um so Datensätze abzurufen, die sich in der Mitte des Resultsets befinden. Wenn sich die erforderlichen Ergebnisse am Ende des zurückgegebenen Datasets befinden, ist es effizienter, eine andere Sortierungskonfiguration zu verwenden und stattdessen die Ergebnisse vom Anfang des Datasets abzurufen.

> [!NOTE]
> Bei Verwendung von **Skip** sollten die Ergebnisse nach mindestens einer Spalte mit `asc` oder `desc` sortiert werden. Ohne Sortierung sind die zurückgegebenen Ergebnisse zufällig und nicht wiederholbar.

Die folgenden Beispiele zeigen, wie die ersten _10_ Datensätze übersprungen werden, die sich bei einer Abfrage ergeben, anstatt das zurückgegebene Resultset mit dem 11. Datensatz zu beginnen:

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

In der [REST-API](/rest/api/azureresourcegraph/resources/resources) wird das Steuerelement **$skip** verwendet, das Teil von **QueryRequestOptions** ist.

## <a name="paging-results"></a>Auslagerungsergebnisse

Wenn es erforderlich ist, ein Resultset in kleinere Datensatzgruppen zu unterteilen, um sie zu verarbeiten oder weil das Resultset den maximal zulässigen Wert von _5000_ zurückgegebenen Datensätzen überschreitet, verwenden Sie die Paginierung. Die **QueryResponse**-[REST-API](/rest/api/azureresourcegraph/resources/resources) enthält folgende Werte, um anzugeben, dass ein Resultset aufgeteilt wurde: **resultTruncated** und **$skipToken**.
**resultTruncated** ist ein boolescher Wert, über den der Consumer informiert wird, wenn zusätzliche Datensätze vorhanden sind, die in der Antwort nicht zurückgegeben werden. Diese Bedingung kann auch erkannt werden, wenn der Wert der Eigenschaft **count** kleiner ist als der Wert der Eigenschaft **totalRecords**. **totalRecords** definiert die Anzahl der Datensätze, die mit der Abfrage übereinstimmen.

Wenn **resultTruncated** auf **true** festgelegt ist, wird in der Antwort die Eigenschaft **$skipToken** festgelegt. Dieser Wert wird mit den gleichen Werten für die Abfrage und das Abonnement verwendet, um die nächste Datensatzgruppe abzurufen, die mit der Abfrage übereinstimmt.

> [!IMPORTANT]
> Die Abfrage muss das Feld **id** **projizieren**, damit die Paginierung funktioniert. Wenn das Feld in der Abfrage nicht vorhanden ist, enthält die Antwort der REST-API nicht die Eigenschaft **$skipToken**.

Ein Beispiel finden Sie unter [Next page query](/rest/api/azureresourcegraph/resources/resources#next_page_query) (Abfrage der nächsten Seite) in der REST-API-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Lernen Sie, [Ressourcen zu untersuchen](explore-resources.md)