---
title: QnA-Bot – Azure Bot Service – QnA Maker
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial wird erläutert, wie Sie einen QnA-Bot mit Azure Bot Service v3 im Azure-Portal erstellen.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker`
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.openlocfilehash: 1e1152f810bffb2ab4dfe230c2d3b3c2a8198492
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55811918"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v3"></a>Tutorial: Erstellen eines QnA-Bots mit Azure Bot Service v3

In diesem Tutorial wird erläutert, wie Sie einen QnA-Bot mit Azure Bot Service v3 im [Azure-Portal](https://portal.azure.com) erstellen, ohne Code schreiben zu müssen. Das Herstellen einer Verbindung zwischen einer veröffentlichten Wissensdatenbank (Knowledge Base, KB) und einem Bot ist genau so einfach wie das Ändern der Bot-Anwendungseinstellungen. 

> [!Note] 
> Dieses Thema gilt für Version 3 des Bot SDK. Version 4 finden Sie [hier](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs). 

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen einer Azure Bot Service-Instanz mit der QnA Maker-Vorlage
> * Chatten mit dem Bot, um die richtige Funktionsweise des Codes zu überprüfen 
> * Herstellen einer Verbindung zwischen Ihrer veröffentlichten Wissensdatenbank und dem Bot
> * Testen des Bots mit einer Frage

In diesem Artikel können Sie den kostenlosen QnA Maker-[Dienst](../how-to/set-up-qnamaker-service-azure.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine veröffentlichte Wissensdatenbank. Besitzen Sie keine, führen Sie die unter [Erstellen einer Knowledge Base](../How-To/create-knowledge-base.md) beschriebenen Schritte aus, um einen QnA Maker-Dienst mit Fragen und Antworten zu erstellen.

## <a name="create-a-qna-bot"></a>Erstellen eines QnA-Bots

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

    ![Erstellen des Botdiensts](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Suchen Sie im Suchfeld nach **Web-App-Bot**.

    ![Auswahl des Botdiensts](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. Geben Sie in **Botdienst** die erforderlichen Informationen ein:

    - Legen Sie den **App-Namen** auf den Namen Ihres Bots fest. Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. meinnotizbot.azurewebsites.net).
    - Wählen Sie das Abonnement, die Ressourcengruppe, den App Service-Plan und den Standort aus.

4. Um die v3-Vorlagen zu verwenden, wählen Sie die SDK-Version **SDK v3** und die SDK-Sprache **C#** oder **Node.js** aus.

    ![Bot-SDK-Einstellungen](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Wählen Sie die Vorlage **Frage und Antwort** für das Feld „Bot-Vorlage“ aus, und speichern Sie die Vorlageneinstellungen, indem Sie **Auswählen** auswählen.

    ![Speichern der Bot Service-Vorlagenauswahl](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Überprüfen Sie Ihre Einstellungen, und wählen Sie dann **Erstellen** aus. Dadurch wird der Botdienst erstellt und in Azure bereitgestellt.

    ![Erstellen eines Bots](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Vergewissern Sie sich, dass der Botdienst bereitgestellt wurde.

    - Wählen Sie **Benachrichtigungen** (Glockensymbol im oberen Bereich des Azure-Portals) aus. Die Benachrichtigung ändert sich von **Bereitstellung gestartet** in **Bereitstellung erfolgreich**.
    - Nachdem die Benachrichtigung sich in **Bereitstellung erfolgreich** geändert hat, wählen Sie **Zu Ressource wechseln** für diese Benachrichtigung aus.

## <a name="chat-with-the-bot"></a>Chatten mit dem Bot

Durch Auswählen von **Zu Ressource wechseln** wird die Botressource angezeigt.

Wählen Sie **Test in Web Chat** (Im Webchat testen) aus, um den Bereich für den Webchat zu öffnen. Geben Sie im Webchat „Hi“ ein.

![Webchat mit QnA-Bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

Der Bot antwortet mit „Please set QnAKnowledgebaseId and QnASubscriptionKey in App Settings. Diese Antwort bestätigt, dass der QnA-Bot die Meldung erhalten hat, mit dem Bot jedoch noch keine QnA Maker-Knowledge Base verbunden ist. 

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Verbinden der QnA Maker-Knowledge Base mit dem Bot

1. Öffnen Sie **Anwendungseinstellungen**, und bearbeiten Sie die Felder **QnAKnowledgebaseId**, **QnAAuthKey** und **QnAEndpointHostName** so, dass sie die Werte Ihrer QnA Maker-Knowledge Base enthalten.

    ![app settings](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

1. Rufen Sie die Wissensdatenbank-ID, die Host-URL und den Endpunktschlüssel auf der Registerkarte „Einstellungen“ Ihrer Wissensdatenbank im QnA Maker-Portal ab.

    - Melden Sie sich bei [QnA Maker](https://qnamaker.ai) an.
    - Navigieren Sie zu Ihrer Knowledge Base.
    - Wählen Sie die Registerkarte **Einstellungen** aus.
    - **Veröffentlichen** Sie Ihre Knowledge Base, sofern dies noch nicht erfolgt ist.

    ![QnA Maker-Werte](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

## <a name="test-the-bot"></a>Testen des Bots

Wählen Sie im Azure-Portal **Test in Web Chat** (Im Webchat testen) aus, um den Bot zu testen. 

![QnA Maker-Bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Der QnA-Bot antwortet auf über Ihre Wissensdatenbank.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Bot dieses Tutorials nicht mehr benötigen, entfernen Sie ihn im Azure-Portal. Zu diesen Botdiensten gehören:

* Der App Service-Plan
* Der Suchdienst
* Der Cognitive Service
* Der App-Dienst
* Optional sind darüber hinaus der Application Insights-Dienst und -Speicher für die Application Insights-Daten enthalten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzept: Wissensdatenbank](../concepts/knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen

- [Verwalten Ihrer Knowledge Base](https://qnamaker.ai)
- [Aktivieren Ihres Bots in verschiedenen Kanälen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
