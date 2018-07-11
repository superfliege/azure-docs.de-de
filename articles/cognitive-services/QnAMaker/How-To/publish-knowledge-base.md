---
title: Veröffentlichen einer Wissensdatenbank – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Veröffentlichen einer Wissensdatenbank
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e9dbeacfb0df98c6b8f084c263690c05fe966cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376170"
---
# <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

Die Veröffentlichung Ihrer Wissensdatenbank ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen zur Verfügung zu stellen. 

Wenn Sie eine Wissensdatenbank veröffentlichen, werden die QnA-Inhalte Ihrer Wissensdatenbank aus dem Testindex in einen Produktionsindex in Azure Search verschoben.

![Produktionstestindex veröffentlichen](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

## <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

1. Wählen Sie auf der oberen Navigationsleiste die Option **Veröffentlichen** aus, nachdem Sie die Änderungen in Ihrer Wissensdatenbank (Knowledge Base, KB) vorgenommen haben. Sie können bis zur zugewiesenen Anzahl von Wissensdatenbanken für Azure Search veröffentlichen. 

    ![Wissensdatenbank veröffentlichen](../media/qnamaker-how-to-publish-kb/publish.png)

2. Wählen Sie erneut **Veröffentlichen** aus, um die Endpunktdetails anzuzeigen, die in Ihrer Anwendung oder im Botcode verwendet werden können.

    ![Wissensdatenbank veröffentlichen](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen von Analysen zu Ihrer Wissensdatenbank](./get-analytics-knowledge-base.md)
