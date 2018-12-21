---
title: 'Schnellstart: Abrufen von Antworten aus einer Wissensdatenbank mit Postman – QnA Maker'
titlesuffix: Azure Cognitive Services
description: In diesem Schnellstart wird Schritt für Schritt erläutert, wie Sie mit Postman eine Antwort aus einer Wissensdatenbank abrufen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: diberry
ms.openlocfilehash: 476e982bdddd41c1daf06c3a673d964ce2a85f98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270891"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Schnellstart: Abrufen einer Antwort aus einer Wissensdatenbank mit Postman

In diesem Schnellstart auf Postman-Basis wird Schritt für Schritt erläutert, wie Sie eine Antwort aus einer Wissensdatenbank abrufen.

## <a name="prerequisites"></a>Voraussetzungen

* Neueste Version von [**Postman**](https://www.getpostman.com/).
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md) und eine [Wissensdatenbank mit Fragen und Antworten](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Veröffentlichen zum Abrufen des Endpunkts

Wenn Sie bereit sind, eine Antwort auf eine Frage aus Ihrer Wissensdatenbank zu generieren, [veröffentlichen](../How-to/publish-knowledge-base.md) Sie Ihre Wissensdatenbank.

## <a name="use-production-endpoint-with-postman"></a>Verwenden des Produktionsendpunkts mit Postman

Wenn Ihre Wissensdatenbank veröffentlicht wird, werden auf der Seite **Veröffentlichen** die HTTP-Anforderungseinstellungen zum Generieren einer Antwort angezeigt. Die Standardansicht zeigt die erforderlichen Einstellungen zum Generieren einer Antwort von [Postman](https://www.getpostman.com) an.

[![Veröffentlichen der Ergebnisse](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Um eine Antwort mit Postman zu generieren, führen Sie die folgenden Schritte aus:

1. Öffnen Sie Postman. 
1. Wählen Sie den Baustein zum Erstellen einer einfachen Anforderung aus.
1. Legen Sie für den **Request name (Anforderungsnamen)** `Generate QnA Maker answer`und für die **Collection (Sammlung)** `Generate QnA Maker answers` fest.
1. Wählen Sie im Arbeitsbereich die HTTP-Methode von **POST** aus.
1. Verketten Sie für die URL den HOST-Wert und den Post-Wert, um die vollständige URL zu erstellen. 

    [![Festlegen der Methode in Postman auf „Post“ und Festlegen der vollständigen URL](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Wählen Sie die Registerkarte **Headers (Kopfzeilen)** unter der URL und dann **Bulk Edit (Massenbearbeitung)** aus. 
1. Kopieren Sie die Kopfzeilen in den Textbereich.

    [![Festlegen der Kopfzeilen in Postman](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Wählen Sie die Registerkarte **Body (Hauptteil)** aus.
1. Wählen Sie das Format **raw (unformatiert)** aus, und geben Sie den JSON-Code ein, der die Frage darstellt.

    [![Festlegen des JSON-Werts für den Hauptteil in Postman](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Wählen Sie die Schaltfläche **Send (Senden)** aus.
1. Die Rückgabe enthält neben der Antwort andere Informationen, die für die Clientanwendung wichtig sein können. 

    [![Festlegen des JSON-Werts für den Hauptteil in Postman](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint-with-curl"></a>Verwenden des Stagingendpunkts mit cURL

Wenn Sie eine Antwort vom Stagingendpunkt erhalten möchten, verwenden Sie den booleschen Abfragezeichenfolgen-Parameter `isTest` mit dem Wert `true`.

`isTest=true`

## <a name="next-steps"></a>Nächste Schritte

Die Veröffentlichungsseite enthält auch Informationen zum [Generieren einer Antwort](get-answer-from-kb-using-curl.md) mit cURL. 

> [!div class="nextstepaction"]
> [Verwenden von Metadaten beim Generieren einer Antwort](../How-to/metadata-generateanswer-usage.md)