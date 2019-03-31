---
title: Lebenszyklus einer Wissensdatenbank – QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker lernt am besten in einem iterativen Zyklus aus Modelländerungen, Ausdrucksbeispielen, Veröffentlichungen und dem Sammeln von Daten aus Endpunktabfragen.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/21/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: bacfb5fed4d72a7be2239ba97a68f15766b3ff59
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650444"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Lebenszyklus einer Wissensdatenbank in QnA Maker
QnA Maker lernt am besten in einem iterativen Zyklus aus Modelländerungen, Ausdrucksbeispielen, Veröffentlichungen und dem Sammeln von Daten aus Endpunktabfragen. 

![Erstellungszyklus](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Erstellen einer QnA Maker-Knowledge Base
Der Endpunkt der QnA Maker-Knowledge Base (KB) stellt die am besten übereinstimmende Antwort auf eine Benutzerfrage basierend auf dem Inhalt der Knowledge Base bereit. Das Erstellen einer Knowledge Base ist eine einmalige Aktion zum Einrichten eines Inhaltsrepositorys von Fragen, Antworten und zugeordneten Metadaten. Eine Knowledge Base kann durch Crawlen durch bereits vorhandene Inhalte wie Seiten mit häufig gestellten Fragen, Produkthandbüchern oder strukturierten Frage-Antwort-Paaren erstellt werden. Erfahren Sie mehr über das [Erstellen einer Knowledge Base](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Testen und Aktualisieren der Knowledge Base

Die Knowledge Base ist bereit für das Testen, nachdem sie mit manuell bearbeiteten oder automatisch extrahierten Inhalten aufgefüllt wurde. Interaktive Tests können im QnA Maker-Portal über den Bereich **Testen** ausgeführt werden, indem häufige Benutzerabfragen eingegeben werden und dann überprüft wird, ob die zurückgegebenen Antworten richtig sind und eine ausreichende Zuverlässigkeitsbewertung aufweisen. 

* **Optimieren niedriger Zuverlässigkeitsbewertungen**: Fügen Sie alternative Fragen hinzu. 
* **Wenn eine Abfrage fälschlicherweise die [Standardantwort zurückgibt](confidence-score.md#change-default-answer)**: Fügen Sie neue Antworten auf die richtige Frage hinzu. 

Diese nahe Abfolge von Tests und Updates wird so lange fortgesetzt, bis Sie mit den Ergebnissen zufrieden sind. Erfahren Sie mehr über das [Testen Ihrer Knowledge Base](../How-To/test-knowledge-base.md).

Verwenden Sie für große KBs automatisiertes Testen mit der [generateAnswer-API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) und dem Abfragezeichenfolgenparameter `isTest=true`, der die `test`-Wissensdatenbank anstelle der veröffentlichten Wissensdatenbank abfragt. 

## <a name="publish-the-knowledge-base"></a>Veröffentlichen der Knowledge Base
Nachdem Sie die Tests der Knowledge Base abgeschlossen haben, können Sie diese veröffentlichen. Beim Veröffentlichen wird die neueste Version der getesteten Knowledge Base in einen dedizierten Azure Search-Index gepusht, der die **veröffentlichte** Knowledge Base darstellt. Außerdem wird ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.

Auf diese Weise haben Änderungen an der Testversion der Knowledge Base keine Auswirkungen auf die veröffentlichte Version, die möglicherweise in einer aktiven Produktionsanwendung verwendet wird.

Jede dieser Knowledge Bases kann zu Testzwecken separat angesprochen werden. Mithilfe der APIs können Sie mit dem `isTest=true`-Flag im GenerateAnswer-Aufruf die Testversion der Knowledge Base als Ziel verwenden.

Erfahren Sie mehr über das [Veröffentlichen Ihrer Knowledge Base](../How-To/publish-knowledge-base.md).

## <a name="monitor-usage"></a>Überwachen der Nutzung
Um die Chatprotokolle Ihres Diensts erfassen zu können, müssen Sie beim [Erstellen Ihres QnA Maker-Diensts](../How-To/set-up-qnamaker-service-azure.md) Application Insights aktivieren.

Sie können verschiedene Analysen Ihrer Dienstnutzung abrufen. Erfahren Sie mehr über die Verwendung von Application Insights zum Abrufen von [Analysen zu Ihrem QnA Maker-Dienst](../How-To/get-analytics-knowledge-base.md).

Je nach den Ergebnissen Ihrer Analysen können Sie entsprechende [Änderungen an Ihrer Knowledge Base](../How-To/edit-knowledge-base.md) vornehmen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zuverlässigkeitsbewertung](./confidence-score.md)

## <a name="see-also"></a>Weitere Informationen 

[Knowledge Base](./knowledge-base.md)
[QnA Maker – Übersicht](../Overview/overview.md)
