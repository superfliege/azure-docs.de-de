---
title: Planen Ihrer LUIS-Anwendungen | Microsoft-Dokumentation
description: Beschreiben Sie kurz relevante App-Absichten und -Entitäten, und erstellen Sie dann Ihre Anwendungspläne in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: diberry
ms.openlocfilehash: cba156d784bfcdd2586073ab5562faf604569cd8
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505527"
---
# <a name="plan-your-luis-app"></a>Planen Ihrer LUIS-App

Es ist wichtig, Ihre App zu planen, bevor Sie mit dem Erstellen in LUIS beginnen. Bereiten Sie eine kurze Beschreibung oder ein Schema der möglichen Absichten und Entitäten, die für das domänenspezifische Thema Ihrer Anwendung relevant sind, vor.  

## <a name="identify-your-domain"></a>Identifizieren Ihrer Domäne
Eine LUIS-App baut auf einem domänenspezifischen Thema auf.  Beispielsweise könnten Sie eine Reise-App erstellen, die Buchungen von Tickets, Flügen, Hotels und Mietwagen ausführt. Eine andere App könnte Inhalte im Zusammenhang mit Sportaktivitäten, dem Nachverfolgen von Fitnessübungen und dem Festlegen von Zielen bereitstellen. 

> [!TIP]
> LUIS bietet [vordefinierte Domänen](luis-how-to-use-prebuilt-domains.md) für viele häufige Szenarien.
> Überprüfen Sie, ob Sie eine vordefinierte Domäne als Ausgangspunkt für Ihre App verwenden können.

## <a name="identify-your-intents"></a>Identifizieren Ihrer Absichten
Überlegen Sie sich, welche [Absichten](luis-concept-intent.md) wichtig für die Aufgabe Ihrer Anwendung sind. Beispiel: eine Reise-App mit Funktionen zum Buchen eines Flugs und Überprüfen des Wetters am Ziel des Benutzers. Sie können die Absichten „BookFlight“ und „GetWeather“ für diese Aktionen definieren. In einer komplexeren App mit umfassenderen Funktionen gibt es weitere Absichten, und Sie sollten diese sorgfältig definieren, damit sie nicht zu spezifisch sind. Beispielsweise sind „BookFlight“ und „BookHotel“ möglicherweise separate Absichten, aber „BookInternationalFlight“ und „BookDomesticFlight“ könnten zu ähnlich sein.

> [!NOTE]
> Als bewährte Methode sollten Sie nur so viele Absichten verwenden, wie zum Ausführen der Funktionen Ihrer App erforderlich sind. Wenn Sie zu viele Absichten definieren, wird es für LUIS schwieriger, Äußerungen richtig zu klassifizieren. Wenn Sie zu wenige definieren, sind sie möglicherweise so allgemein gehalten, dass sie sich überschneiden.


## <a name="identify-your-entities"></a>Identifizieren Ihrer Entitäten
Um einen Flug zu buchen, benötigen Sie einige Informationen wie das Ziel, das Datum, die Fluggesellschaft, die Ticketkategorie und die Reiseklasse. Sie können diese als [Entitäten](luis-concept-entity-types.md) hinzufügen, da sie zum Umsetzen einer Absicht wichtig sind. 

Wenn Sie festlegen, welche Entitäten in Ihrer App verwendet werden sollen, bedenken Sie, dass es verschiedene Typen von Entitäten zum Aufzeichnen der Beziehungen zwischen Objekttypen gibt. Unter [Entitäten in LUIS](luis-concept-entity-types.md) finden Sie weitere Details zu den verschiedenen Typen.

### <a name="simple-entity"></a>Entität vom Typ „Simple“
Eine einfache Entität beschreibt ein einzelnes Konzept.

![Entität vom Typ „Simple“](./media/luis-plan-your-app/simple-entity.png)

Weitere Informationen zum Extrahieren der einfachen Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#simple-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-primary-and-secondary-data.md) für einfache Entitäten aus, um mehr über das Verwenden einer einfachen Entität zu erfahren.

### <a name="hierarchical-entity"></a>Entität vom Typ „Hierarchical“
Eine hierarchische Entität ist ein besonderer Typ einer **einfachen** Entität, in der eine Kategorie mit ihren Membern in Form einer Beziehung zwischen über- und untergeordneten Elementen definiert wird.

![Entität vom Typ „Hierarchical“](./media/luis-plan-your-app/hierarchical-entity.png)

Weitere Informationen zum Extrahieren der hierarchischen Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#hierarchical-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intent-and-hier-entity.md) für hierarchische Entitäten aus, um mehr über das Verwenden einer hierarchischen Entität zu erfahren.

### <a name="composite-entity"></a>Entität vom Typ „Composite“
Eine zusammengesetzte Entität besteht aus anderen Entitäten, die Teile eines ganzen Elements bilden. 

![Entität vom Typ „Composite“](./media/luis-plan-your-app/composite-entity.png)

Weitere Informationen zum Extrahieren der zusammengesetzten Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#composite-entity-data). Probieren Sie das [Tutorial](luis-tutorial-composite-entity.md) für zusammengesetzte Entitäten aus, um mehr über das Verwenden einer zusammengesetzten Entität zu erfahren.

### <a name="prebuilt-entity"></a>Vordefinierte Entität
LUIS bietet [vordefinierte Entitäten](luis-prebuilt-entities.md) für allgemeine Typen wie `Number`, die Sie für die Anzahl von Tickets in einer Ticketbestellung verwenden können.

![Vordefinierte Entität „number“](./media/luis-plan-your-app/number-entity.png)

Weitere Informationen zum Extrahieren der vordefinierten Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#prebuilt-entity-data). 

### <a name="list-entity"></a>Entität vom Typ „List“ 
Eine Listenentität ist eine explizit angegebene Liste von Werten. Jeder Wert besteht aus mindestens einem Synonym. In einer Reise-App können Sie eine Listenentität zum Darstellen von Flughafennamen erstellen.

![Entität vom Typ „List“](./media/luis-plan-your-app/list-entity.png)

Weitere Informationen zum Extrahieren von Listenentitäten aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#list-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intent-and-list-entity.md) aus, um mehr über das Verwenden einer Listenentität zu erfahren.

### <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“
Mit einer Entität für reguläre Ausdrücke kann LUIS Daten aus einer Äußerung basierend auf einem regulären Ausdruck extrahieren.

![Entität vom Typ „RegEx“](./media/luis-plan-your-app/regex-entity.png)

Weitere Informationen zum Extrahieren von Entitäten für reguläre Ausdrücke aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#regular-expression-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intents-regex-entity.md) aus, um mehr über das Verwenden einer Entität für reguläre Ausdrücke zu erfahren.

## <a name="after-getting-endpoint-utterances"></a>Nach dem Abrufen von Endpunktäußerungen
Nachdem Ihre App Endpunktäußerungen erhält, sollten Sie planen, Vorhersageverbesserungen mit [aktivem Lernen](luis-how-to-review-endoint-utt.md), [Ausdruckslisten](luis-concept-feature.md) und [Mustern](luis-concept-patterns.md) zu implementieren. 

### <a name="patternany-entity"></a>Entität „Pattern.any“
Pattern.any ist ein Platzhalter variabler Länge, der nur in der Vorlagenäußerung eines [Musters](luis-concept-patterns.md) verwendet wird, um zu kennzeichnen, wo die Entität beginnt und endet. Vorlagenäußerungen richten sich nach einer [ordnungsgemäßen Syntax](luis-concept-patterns.md#pattern-syntax), um Entitäten und ignorierbaren Text zu identifizieren.


## <a name="next-steps"></a>Nächste Schritte
* Eine kurze exemplarische Vorgehensweise zum Erstellen einer LUIS-App finden Sie unter [Erstellen Ihrer ersten LUIS-App (Language Understanding Intelligent Service)](luis-get-started-create-app.md).
