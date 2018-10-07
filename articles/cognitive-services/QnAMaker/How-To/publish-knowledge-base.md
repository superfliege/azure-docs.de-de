---
title: 'Veröffentlichen einer Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Die Veröffentlichung Ihrer Wissensdatenbank ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen zur Verfügung zu stellen. Wenn Sie eine Wissensdatenbank veröffentlichen, werden die QnA-Inhalte Ihrer Wissensdatenbank aus dem Testindex in einen Produktionsindex in Azure Search verschoben.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: eceb0554d35935f1aee77c4c054cb7e65b327845
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033413"
---
# <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

Die Veröffentlichung Ihrer Wissensdatenbank ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen zur Verfügung zu stellen. 

Wenn Sie eine Wissensdatenbank veröffentlichen, werden die QnA-Inhalte Ihrer Wissensdatenbank aus dem Testindex in einen Produktionsindex in Azure Search verschoben.

![Produktionstestindex veröffentlichen](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

## <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

1. Wählen Sie auf der oberen Navigationsleiste die Option **Veröffentlichen** aus, nachdem Sie die Änderungen in Ihrer Wissensdatenbank (Knowledge Base, KB) vorgenommen haben. Sie können bis zur zugewiesenen Anzahl von Wissensdatenbanken für Azure Search veröffentlichen. 

    ![Wissensdatenbank veröffentlichen](../media/qnamaker-how-to-publish-kb/publish.png)

2. Wählen Sie erneut **Veröffentlichen** aus, um die Endpunktdetails anzuzeigen, die in Ihrer Anwendung oder im Botcode verwendet werden können.

    ![Veröffentlichen einer Wissensdatenbank](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen von Analysen zu Ihrer Wissensdatenbank](./get-analytics-knowledge-base.md)
