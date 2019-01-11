---
title: 'Ressourcen- und Schlüsselverwaltung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: 'Ihr QnA Maker-Dienst verwendet zwei Arten von Schlüsseln: Abonnementschlüssel und Endpunktschlüssel.'
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/26/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 3984ac84271728b9066aba15a582065ea3121c76
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581758"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Verwalten von Schlüsseln in QnA Maker

Ihr QnA Maker-Dienst verwendet zwei Arten von Schlüsseln, **Abonnementschlüssel** und **Endpunktschlüssel**.

![Schlüsselverwaltung](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonnementschlüssel:** Diese Schlüssel werden verwendet, um auf die [QnA Maker-Verwaltungsdienst-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) zuzugreifen. Mit diesen APIs können Sie Ihre Wissensdatenbank bearbeiten.  

2. **Endpunktschlüssel:** Diese Schlüssel werden für den Zugriff auf den Endpunkt der Wissensdatenbank verwendet, um eine Antwort für eine Benutzerfrage abzurufen. In der Regel verwenden Sie diesen Endpunkt in Ihrem Chatbot oder Clientanwendungscode, der den QnA Maker-Dienst nutzt.
 
## <a name="subscription-keys"></a>Abonnementschlüssel
Sie können Ihre Abonnementschlüssel im Azure-Portal anzeigen und zurücksetzen, in dem Sie die QnA Maker-Ressource erstellt haben. 
1. Wechseln Sie im Azure-Portal zur QnA Maker-Ressource.

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Wechseln Sie zu **Schlüssel**.

    ![Abonnementschlüssel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Endpunktschlüssel

Endpunktschlüssel können im [QnA Maker-Portal](https://qnamaker.ai) verwaltet werden.

1. Melden Sie sich beim [QnA Maker-Portal](https://qnamaker.ai) an, und wechseln Sie zu **Diensteinstellungen**.

    ![Endpunktschlüssel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Zeigen Sie Ihre Schlüssel an, oder setzen Sie die Schlüssel zurück.

    ![Endpunktschlüssel-Manager](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Aktualisieren Sie Ihre Schlüssel, wenn Sie das Gefühl haben, dass sie gefährdet sind. Dazu müssen möglicherweise entsprechende Änderungen an Ihrem Clientanwendungs- oder Botcode vorgenommen werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Wissensdatenbank in einer anderen Sprache](./language-knowledge-base.md)
