---
title: Erstellen eines QnA-Bots mit Azure Bot Service – Azure Cognitive Services | Microsoft-Dokumentation
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: fc430bf3aa7cad279d7a93bb6892aa19abee3378
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109268"
---
# <a name="create-a-qna-bot-with-azure-bot-service"></a>Erstellen eines QnA-Bots mit Azure Bot Service
In diesem Tutorial wird erläutert, wie Sie einen QnA-Bot mit Azure Bot Service im Azure-Portal erstellen.

## <a name="prerequisite"></a>Voraussetzung
Führen Sie vor der Erstellung des Bots die unter [Erstellen einer Knowledge Base](../How-To/create-knowledge-base.md) beschriebenen Schritte aus, um einen QnA Maker-Dienst mit Fragen und Antworten zu erstellen.

Der Bot antwortet über QnAMakerDialog auf Fragen aus der von Ihnen erstellten Knowledge Base.

## <a name="create-a-qna-bot"></a>Erstellen eines QnA-Bots
1. Wählen Sie im [Azure-Portal](https://portal.azure.com) auf dem Menüblatt die Option zum **Erstellen** einer neuen Ressource und dann **Alle anzeigen** aus.

    ![Erstellen des Botdiensts](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Suchen Sie im Suchfeld nach **Web-App-Bot**.

    ![Auswahl des Botdiensts](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. Geben Sie im **Blatt für den Botdienst** die erforderlichen Informationen an, und wählen Sie **Erstellen** aus. Der Botdienst wird über QnAMakerDialog in Azure erstellt und bereitgestellt.

    - Legen Sie den **App-Namen** auf den Namen Ihres Bots fest. Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. meinnotizbot.azurewebsites.net).
    - Wählen Sie das Abonnement, die Ressourcengruppe, den App Service-Plan und den Standort aus.
    - Wählen Sie die Vorlage **Question and Answer** (Node.js oder C#) für das Feld „Bot template“ (Botvorlage) aus.
    - Aktivieren Sie das Kontrollkästchen zur Bestätigung der rechtlichen Hinweise. Die Bestimmungen der rechtlichen Hinweise werden unterhalb des Kontrollkästchens angezeigt.

        ![Auswahl des Botdiensts](../media/qnamaker-tutorials-create-bot/bot-service-qna-template.PNG)

4. Vergewissern Sie sich, dass der Botdienst bereitgestellt wurde.

    - Wählen Sie **Benachrichtigungen** (Glockensymbol im oberen Bereich des Azure-Portals) aus. Die Benachrichtigung ändert sich von **Bereitstellung gestartet** in **Bereitstellung erfolgreich**.
    - Nachdem die Benachrichtigung sich in **Bereitstellung erfolgreich** geändert hat, wählen Sie **Zu Ressource wechseln** für diese Benachrichtigung aus.

## <a name="chat-with-the-bot"></a>Chatten mit dem Bot
Durch Auswählen von **Zu Ressource wechseln** wird das Ressourcenblatt des Bots angezeigt.

Klicken Sie nach der Registrierung des Bots auf **Test in Web Chat** (Im Webchat testen), um den Bereich für den Webchat zu öffnen. Geben Sie „hello“ im Webchat ein.

![Webchat mit QnA-Bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

Der Bot antwortet mit „Please set QnAKnowledgebaseId and QnASubscriptionKey in App Settings. Learn how to get them at https://aka.ms/qnaabssetup.“ Diese Antwort bestätigt, dass der QnA-Bot die Meldung erhalten hat, mit dem Bot jedoch noch keine QnA Maker-Knowledge Base verbunden ist. Führen Sie dazu den nächsten Schritt aus.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Verbinden der QnA Maker-Knowledge Base mit dem Bot

1. Öffnen Sie **Anwendungseinstellungen**, und bearbeiten Sie die Felder **QnAKnowledgebaseId**, **QnAAuthKey** und **QnAEndpointHostName** so, dass sie die Werte Ihrer QnA Maker-Knowledge Base enthalten.

    ![app settings](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Rufen Sie die Knowledge Base-ID, die Host-URL und den Endpunktschlüssel auf der Registerkarte „Einstellungen“ Ihrer Knowledge Base in https://qnamaker.ai ab.
    - Melden Sie sich bei [QnA Maker](https://qnamaker.ai) an.
    - Navigieren Sie zu Ihrer Knowledge Base.
    - Klicken Sie auf die Registerkarte **Einstellungen**.
    - **Veröffentlichen** Sie Ihre Knowledge Base, sofern dies noch nicht erfolgt ist.

    ![QnA Maker-Werte](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Wenn Sie die Vorschauversion der Knowledge Base mit dem QnA-Bot verbinden möchten, legen Sie den Wert von **Ocp-Apim-Subscription-Key** auf **QnAAuthKey** fest. Lassen Sie **QnAEndpointHostName** leer.

## <a name="test-the-bot"></a>Testen des Bots
Klicken Sie im Azure-Portal auf **Test in Web Chat** (Im Webchat testen), um den Bot zu testen. 

![QnA Maker-Bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Der QnA-Bot antwortet nun auf Fragen aus Ihrer Knowledge Base.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrieren von QnA Maker und LUIS](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Weitere Informationen

- [Verwalten Ihrer Knowledge Base](https://qnamaker.ai)
- [Aktivieren Ihres Bots in verschiedenen Kanälen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
