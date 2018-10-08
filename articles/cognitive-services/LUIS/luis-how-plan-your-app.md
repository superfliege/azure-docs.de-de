---
title: Planen Ihrer LUIS-Anwendungen (Language Understanding)
titleSuffix: Azure Cognitive Services
description: Beschreiben Sie kurz relevante App-Absichten und -Entitäten, und erstellen Sie dann Ihre Anwendungspläne in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: a7e137c864d216b6b936181c3482358bd10f175a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037268"
---
# <a name="plan-your-luis-app"></a>Planen Ihrer LUIS-App

Es ist wichtig, dass Sie Ihre App planen. Identifizieren Sie Ihre Domäne, einschließlich möglicher Absichten und Entitäten, die für Ihre Anwendung relevant sind.  

## <a name="identify-your-domain"></a>Identifizieren Ihrer Domäne
Eine LUIS-App baut auf einem domänenspezifischen Thema auf.  Beispielsweise könnten Sie eine Reise-App erstellen, die Buchungen von Tickets, Flügen, Hotels und Mietwagen ausführt. Eine andere App könnte Inhalte im Zusammenhang mit Sportaktivitäten, dem Nachverfolgen von Fitnessübungen und dem Festlegen von Zielen bereitstellen. Durch die Identifizierung der Domäne können Sie Wörter oder Ausdrücke finden, die für Ihre Domäne wichtig sind.

> [!TIP]
> LUIS bietet [vordefinierte Domänen](luis-how-to-use-prebuilt-domains.md) für viele häufige Szenarien.
> Überprüfen Sie, ob Sie eine vordefinierte Domäne als Ausgangspunkt für Ihre App verwenden können.

## <a name="identify-your-intents"></a>Identifizieren Ihrer Absichten
Überlegen Sie sich, welche [Absichten](luis-concept-intent.md) wichtig für die Aufgabe Ihrer Anwendung sind. Beispiel: eine Reise-App mit Funktionen zum Buchen eines Flugs und Überprüfen des Wetters am Ziel des Benutzers. Sie können die Absichten „BookFlight“ und „GetWeather“ für diese Aktionen definieren. In einer komplexeren App mit umfassenderen Funktionen gibt es weitere Absichten, und Sie sollten diese sorgfältig definieren, damit sie nicht zu spezifisch sind. Beispielsweise sind „BookFlight“ und „BookHotel“ möglicherweise separate Absichten, aber „BookInternationalFlight“ und „BookDomesticFlight“ könnten zu ähnlich sein.

> [!NOTE]
> Als bewährte Methode sollten Sie nur so viele Absichten verwenden, wie zum Ausführen der Funktionen Ihrer App erforderlich sind. Wenn Sie zu viele Absichten definieren, wird es für LUIS schwieriger, Äußerungen richtig zu klassifizieren. Wenn Sie zu wenige definieren, sind sie möglicherweise so allgemein gehalten, dass sie sich überschneiden.

## <a name="create-example-utterances-for-each-intent"></a>Erstellen von Beispieläußerungen für jede Absicht
Nachdem Sie die Absichten festgelegt haben, können Sie 10 bis 15 Beispieläußerungen für jede Absicht erstellen. Zunächst sollten Sie nicht weniger als diese Anzahl von Äußerungen oder viele Äußerungen für jede Absicht erstellen. Jede Äußerung sollte sich von der vorherigen Äußerung unterscheiden. Eine gute Vielfalt in den Äußerungen umfasst die Gesamtzahl der Wörter, die Wortwahl, die Verbform und die Interpunktion. 

## <a name="identify-your-entities"></a>Identifizieren Ihrer Entitäten
Identifizieren Sie in den Beispieläußerungen die Entitäten, die extrahiert werden sollen. Um einen Flug zu buchen, benötigen Sie einige Informationen wie das Ziel, das Datum, die Fluggesellschaft, die Ticketkategorie und die Reiseklasse. Sie erstellen Entitäten für diese Datentypen und markieren dann die [Entitäten](luis-concept-entity-types.md) in den Beispieläußerungen, weil sie für das Erreichen einer Absicht wichtig sind. 

Wenn Sie festlegen, welche Entitäten in Ihrer App verwendet werden sollen, bedenken Sie, dass es verschiedene Typen von Entitäten zum Aufzeichnen der Beziehungen zwischen Objekttypen gibt. Unter [Entitäten in LUIS](luis-concept-entity-types.md) finden Sie weitere Details zu den verschiedenen Typen.

### <a name="simple-entity"></a>Entität vom Typ „Simple“
Eine einfache Entität beschreibt ein einzelnes Konzept.

![Entität vom Typ „Simple“](./media/luis-plan-your-app/simple-entity.png)

Weitere Informationen zum Extrahieren der einfachen Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#simple-entity-data). Probieren Sie diesen [Schnellstart](luis-quickstart-primary-and-secondary-data.md) aus, um mehr über das Verwenden einer einfachen Entität zu erfahren.

### <a name="hierarchical-entity"></a>Entität vom Typ „Hierarchical“
Eine hierarchische Entität ist ein besonderer Typ einer **einfachen** Entität, in der eine Kategorie mit ihren Membern in Form einer Beziehung zwischen über- und untergeordneten Elementen definiert wird. Die Beziehung wird durch den Kontext innerhalb der Äußerung bestimmt. Untergeordnete Elemente einer hierarchischen Entität sind auch einfache Entitäten.

![Entität vom Typ „Hierarchical“](./media/luis-plan-your-app/hierarchical-entity.png)

Weitere Informationen zum Extrahieren der hierarchischen Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#hierarchical-entity-data). Probieren Sie diesen [Schnellstart](luis-quickstart-intent-and-hier-entity.md) aus, um mehr über das Verwenden einer hierarchischen Entität zu erfahren.

### <a name="composite-entity"></a>Entität vom Typ „Composite“
Eine zusammengesetzte Entität besteht aus anderen Entitäten, die Teile eines ganzen Elements bilden. Eine zusammengesetzte Entität enthält eine Vielzahl von Entitätstypen.

![Entität vom Typ „Composite“](./media/luis-plan-your-app/composite-entity.png)

Weitere Informationen zum Extrahieren der zusammengesetzten Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#composite-entity-data). Probieren Sie dieses [Tutorial](luis-tutorial-composite-entity.md) aus, um mehr über das Verwenden einer zusammengesetzten Entität zu erfahren.

### <a name="prebuilt-entity"></a>Vordefinierte Entität
LUIS bietet [vordefinierte Entitäten](luis-prebuilt-entities.md) für häufig verwendete Datentypen wie z. B. Anzahl, Daten, E-Mail-Adresse und URL. Sie können die vordefinierte Entität „number“ (Anzahl) für die Anzahl von Tickets in einer Ticketbestellung verwenden.

![Vordefinierte Entität „number“](./media/luis-plan-your-app/number-entity.png)

Weitere Informationen zum Extrahieren der vordefinierten Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#prebuilt-entity-data). 

### <a name="list-entity"></a>Entität vom Typ „List“ 
Eine Listenentität ist eine explizit angegebene Liste von Werten. Jeder Wert besteht aus mindestens einem Synonym. In einer Reise-App können Sie eine Listenentität zum Darstellen von Flughafennamen erstellen.

![Entität vom Typ „List“](./media/luis-plan-your-app/list-entity.png)

Weitere Informationen zum Extrahieren von Listenentitäten aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#list-entity-data). Probieren Sie diesen [Schnellstart](luis-quickstart-intent-and-list-entity.md) aus, um mehr über das Verwenden einer Listenentität zu erfahren.

### <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“
Mit einer Entität für reguläre Ausdrücke kann LUIS korrekt formatierte Daten aus einer Äußerung basierend auf einem regulären Ausdruck extrahieren.

![Entität vom Typ „RegEx“](./media/luis-plan-your-app/regex-entity.png)

Weitere Informationen zum Extrahieren von Entitäten für reguläre Ausdrücke aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#regular-expression-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intents-regex-entity.md) aus, um mehr über das Verwenden einer Entität für reguläre Ausdrücke zu erfahren.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Ihre App trainiert und veröffentlicht wurde und Endpunktäußerungen erhält, sollten Sie planen, Vorhersageverbesserungen mit [aktivem Lernen](luis-how-to-review-endoint-utt.md), [Ausdruckslisten](luis-concept-feature.md) und [Mustern](luis-concept-patterns.md) zu implementieren. 


* Eine kurze exemplarische Vorgehensweise zum Erstellen einer LUIS-App finden Sie unter [Erstellen Ihrer ersten LUIS-App (Language Understanding Intelligent Service)](luis-get-started-create-app.md).
