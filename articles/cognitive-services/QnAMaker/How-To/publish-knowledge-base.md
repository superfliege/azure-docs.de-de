---
title: Veröffentlichen einer Wissensdatenbank
titleSuffix: QnA Maker - Azure Cognitive Services
description: Die Veröffentlichung Ihrer Wissensdatenbank ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen zur Verfügung zu stellen. Wenn Sie eine Wissensdatenbank veröffentlichen, werden die QnA-Inhalte Ihrer Wissensdatenbank aus dem Testindex in einen Produktionsindex in Azure Search verschoben.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/11/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2db8a87f28905ab03fb45d56943ae40d01908527
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225379"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-portal"></a>Veröffentlichen einer Wissensdatenbank über das QnA Maker-Portal

Die Veröffentlichung Ihrer Wissensdatenbank ist der letzte Schritt, um Ihre Wissensdatenbank als Endpunkt für die Beantwortung von Fragen für eine Clientanwendung zur Verfügung zu stellen. 

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
