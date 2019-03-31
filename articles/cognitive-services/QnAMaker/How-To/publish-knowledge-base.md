---
title: Veröffentlichen einer Wissensdatenbank
titleSuffix: QnA Maker API - Azure Cognitive Services
description: Die Veröffentlichung Ihrer Wissensdatenbank mit dem QnA Maker-API-Dienst ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen zur Verfügung zu stellen. Wenn Sie eine Wissensdatenbank veröffentlichen, werden die QnA-Inhalte Ihrer Wissensdatenbank aus dem Testindex in einen Produktionsindex in Azure Search verschoben.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/11/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: c65654e7d6b2e66a07116ea9555ed316ace88912
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091952"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Veröffentlichen einer Wissensdatenbank über das QnA Maker-API-Dienstportal

Die Veröffentlichung Ihrer Wissensdatenbank mit dem QnA Maker-API-Dienst ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen zur Verfügung zu stellen. 

Wenn Sie eine Wissensdatenbank veröffentlichen, werden die Frage-Antwort-Inhalte Ihrer Wissensdatenbank aus dem Testindex in einen Produktionsindex in Azure Search verschoben.

![Produktionstestindex veröffentlichen](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

1. Wählen Sie auf der oberen Navigationsleiste die Option **Veröffentlichen** aus, nachdem Sie die Änderungen in Ihrer Wissensdatenbank (Knowledge Base, KB) vorgenommen haben. Sie können bis zur zugewiesenen Anzahl von Wissensdatenbanken für Azure Search veröffentlichen. 

    ![Wissensdatenbank veröffentlichen](../media/qnamaker-how-to-publish-kb/publish.png)

2. Wählen Sie erneut **Veröffentlichen** aus, um die Endpunktdetails anzuzeigen, die in Ihrer Anwendung oder im Botcode verwendet werden können.

    ![Erfolgreich veröffentlichte Wissensdatenbank](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit der Wissensdatenbank fertig sind, entfernen Sie sie im QnA Maker-Portal.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen von Analysen zu Ihrer Wissensdatenbank](./get-analytics-knowledge-base.md)
