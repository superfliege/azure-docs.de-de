---
title: Erstellen, Veröffentlichen, Antworten in QnA Maker
titleSuffix: Azure Cognitive Services
description: Erstellen Sie eine neue Wissensdatenbank mit Fragen und Antworten aus einer öffentlichen webbasierten FAQ-Instanz. Speichern, trainieren und veröffentlichen Sie die Wissensdatenbank. Senden Sie nach dem Veröffentlichen der Wissensdatenbank mithilfe eines cURL-Befehls eine Frage, und erhalten Sie eine Antwort. Erstellen Sie anschließend einen Bot, und testen Sie ihn mit der gleichen Frage.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: diberry
ms.openlocfilehash: 299dd61055503f0b5a11cbe97e137e4760edadda
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59266952"
---
# <a name="tutorial-from-qna-maker-portal-create-a-knowledge-base"></a>Tutorial: Erstellen einer Wissensdatenbank über das QnA Maker-Portal

Erstellen Sie eine neue Wissensdatenbank mit Fragen und Antworten aus einer öffentlichen webbasierten FAQ-Instanz. Speichern, trainieren und veröffentlichen Sie die Wissensdatenbank. Senden Sie nach dem Veröffentlichen der Wissensdatenbank mithilfe eines cURL-Befehls eine Frage, und erhalten Sie eine Antwort. Erstellen Sie anschließend einen Bot, und testen Sie ihn mit der gleichen Frage. 

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Erstellen einer Wissensdatenbank über das QnA Maker-Portal
> * Überprüfen, Speichern und Trainieren der Wissensdatenbank
> * Veröffentlichen der Knowledge Base
> * Abfragen der Wissensdatenbank mithilfe von Curl
> * Erstellen eines Bots
> 
> [!NOTE]
> Die programmgesteuerte Version dieses Tutorials steht zusammen mit einer vollständigen Projektmappe im [GitHub-Repository **Azure-Samples/cognitive-services-qnamaker-csharp**](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial muss ein [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md) vorhanden sein. 

## <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank 

1. Melden Sie sich beim [QnA Maker-Portal](https://www.qnamaker.ai) an. 

1. Wählen Sie im oberen Menü die Option **Create a knowledge base** (Wissensdatenbank erstellen) aus.

    ![Schritt 1 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Da Sie Ihren bereits vorhandenen QnA Maker-Dienst verwenden, können Sie den ersten Schritt überspringen. 

1. Wählen Sie im nächsten Schritt Ihre bereits vorhandenen Einstellungen aus:  

    |Einstellung|Zweck|
    |--|--|
    |„Microsoft Azure Directory Id“ (Microsoft Azure-Verzeichnis-ID)|Ihre _Microsoft Azure-Verzeichnis-ID_ ist dem Konto zugeordnet, das Sie für die Anmeldung beim Azure-Portal und beim QnA Maker-Portal verwenden. |
    |„Azure Subscription name“ (Name des Azure-Abonnements)|Das Abrechnungskonto, unter dem Sie die QnA Maker-Ressource erstellt haben.|
    |„Azure QnA Service“ (Azure QnA-Dienst)|Ihre vorhandene QnA Maker-Ressource.|

    ![Schritt 2 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Geben Sie im nächsten Schritt den Namen Ihres Wissensdatenbank (`My Tutorial kb`) ein.

    ![Schritt 3 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Füllen Sie Ihre Wissensdatenbank im nächsten Schritt mit den folgenden Einstellungen auf:  

    |Einstellungsname|Einstellungswert|Zweck|
    |--|--|--|
    |URL|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |Die Inhalte der häufig gestellten Fragen unter dieser URL sind jeweils als Frage-Antwort-Paar formatiert. QnA Maker kann dieses Format interpretieren, um Fragen und die dazugehörigen Antworten zu extrahieren.|
    |Datei |_In diesem Tutorial nicht verwendet_|Dient zum Hochladen von Dateien für Fragen und Antworten. |
    |„Chit-chat personality“ (Smalltalk-Persönlichkeit)|Der Freund|Dadurch erhalten Sie eine freundliche und lockere Persönlichkeit für allgemeine Fragen und Antworten. Die Fragen und Antworten können später bearbeitet werden. |

    ![Schritt 4 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Wählen Sie **Create your KB** (Wissensdatenbank erstellen) aus, um den Erstellungsprozess abzuschließen.

    ![Schritt 5 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-kb-save-and-train"></a>Überprüfen, Speichern und Trainieren der Wissensdatenbank

1. Überprüfen Sie die Fragen und Antworten. Auf der ersten Seite befinden sich Fragen und Antworten von der URL. 

    ![Speichern und trainieren](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Wählen Sie die letzte Seite mit Fragen und Antworten vom Ende der Tabelle aus. Auf der Seite werden Fragen und Antworten der Smalltalk-Persönlichkeit angezeigt. 

1. Wählen Sie auf der Symbolleiste über der Liste mit Fragen und Antworten das Metadatensymbol aus. Daraufhin werden die Metadatentags für die einzelnen Fragen und Antworten angezeigt. Für Fragen im Plauderton ist bereits das Metadatentag **editorial: chit-chat** festgelegt. Dieses Metadatentag wird zusammen mit der ausgewählten Antwort an die Clientanwendung zurückgegeben. Anhand dieser gefilterten Metadaten kann die Clientanwendung (beispielsweise ein Chatbot) zusätzliche Verarbeitungsschritte oder Interaktionen mit dem Benutzer bestimmen.

    ![Anzeigen der Metadatentags](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Wählen Sie auf der oberen Menüleiste die Option **Save and train** (Speichern und trainieren) aus.

## <a name="publish-to-get-kb-endpoints"></a>Veröffentlichen, um Endpunkte für die Wissensdatenbank zu erhalten

Wählen Sie im oberen Menü die Schaltfläche **Veröffentlichen** aus. Wählen Sie auf der Veröffentlichungsseite neben der Schaltfläche **Abbrechen** die Schaltfläche **Veröffentlichen** aus.

![Veröffentlichen](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Nachdem die Wissensdatenbank veröffentlicht wurde, wird der Endpunkt angezeigt.

![Veröffentlichen der Endpunkteinstellungen der Seite](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Schließen Sie die Seite **Veröffentlichen** nicht. Sie wird später noch zum Erstellen eines Bots benötigt. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>Abfragen einer FAQ-Antwort mithilfe von cURL

1. Wählen Sie die Registerkarte **Curl** aus. 

    ![Curl-Befehl](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Kopieren Sie den Text auf der Registerkarte **Curl**, und führen Sie ihn in einem Curl-fähigen Terminal oder an einer Curl-fähigen Befehlszeile aus. Der Wert des Autorisierungsheaders enthält den Text `Endpoint`, gefolgt von einem Leerzeichen und dem Schlüssel.

1. Ersetzen Sie `<Your question>` durch `How large can my KB be?`. Dies ähnelt der Frage `How large a knowledge base can I create?`, ist aber nicht exakt das Gleiche. QnA Maker ermittelt mittels Verarbeitung natürlicher Sprache, dass die beiden Fragen gleich sind.     

1. Führen Sie den cURL-Befehl aus, um die JSON-Antwort mit der Bewertung und der Antwort auf die Frage zu erhalten. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   581  100   543  100    38    418     29  0:00:01  0:00:01 --:--:--   447{
      "answers": [
        {
          "questions": [
            "How large a knowledge base can I create?"
          ],
          "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
          "score": 42.81,
          "id": 2,
          "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
          "metadata": []
        }
      ]
    }
    
    ```

    QnA Maker ist zu 42,81 Prozent von dem Ergebnis überzeugt.  

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Abfragen einer Smalltalk-Antwort mithilfe von cURL

1. Ersetzen Sie im cURL-fähigen Terminal `How large can my KB be?` durch eine Benutzeraussage, die das Ende der Botunterhaltung signalisiert (etwa `Thank you`).   

1. Führen Sie den cURL-Befehl aus, um die JSON-Antwort mit der Bewertung und der Antwort auf die Frage zu erhalten. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   525  100   501  100    24    525     25 --:--:-- --:--:-- --:--:--   550{
      "answers": [
        {
          "questions": [
            "Thank you",
            "Thanks",
            "Thnx",
            "Kthx",
            "I appreciate it",
            "Thank you so much",
            "I thank you",
            "My sincere thank"
          ],
          "answer": "You're very welcome.",
          "score": 100.0,
          "id": 109,
          "source": "qna_chitchat_the_friend.tsv",
          "metadata": [
            {
              "name": "editorial",
              "value": "chitchat"
            }
          ]
        }
      ]
    }
   
    ```

    Da die Frage `Thank you` exakt einer Smalltalk-Frage entspricht, ist QnA Maker zu 100 Prozent überzeugt. QnA Maker hat auch alle dazugehörigen Fragen sowie die Metadateneigenschaft mit den Informationen zum Metadatentag für Geplauder zurückgegeben.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Abfragen der Standardantwort mithilfe von cURL

Jede Frage, bei deren Antwort QnA Maker nicht sicher ist, wird mit einer Standardantwort beantwortet. Diese Antwort wird im Azure-Portal konfiguriert. 

1. Ersetzen Sie `Thank you` im Curl-fähigen Terminal durch `x`. 

1. Führen Sie den cURL-Befehl aus, um die JSON-Antwort mit der Bewertung und der Antwort auf die Frage zu erhalten. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   186  100   170  100    16    272     25 --:--:-- --:--:-- --:--:--   297{
      "answers": [
        {
          "questions": [],
          "answer": "No good match found in KB.",
          "score": 0.0,
          "id": -1,
          "metadata": []
        }
      ]
    }
    ```
    
    QnA Maker hat als Bewertung den Wert `0` (nicht überzeugt) sowie die Standardantwort zurückgegeben. 

## <a name="create-a-knowledge-base-bot"></a>Erstellen einer Wissensdatenbank-Bots

Weitere Informationen finden Sie unter [Tutorial: Erstellen eines QnA-Bots mit Azure Bot Service v4](create-qna-bot.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Wissensdatenbank-Bot nicht mehr benötigen, entfernen Sie die Ressourcengruppe `my-tutorial-rg`, um alle im Rahmen des Botprozesses erstellten Azure-Ressourcen zu entfernen.

Wenn Sie die Wissensdatenbank nicht mehr benötigen, wählen Sie im QnA Maker-Portal die Option **My knowledge bases** (Meine Knowledge Bases) aus. Wählen Sie anschließend die Wissensdatenbank **My Tutorial kb** und schließlich ganz rechts in der Zeile das Löschsymbol aus.  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu unterstützten Datenformaten finden Sie unter [Datenquellen für QnA Maker-Inhalt](../Concepts/data-sources-supported.md). 

Weitere Informationen zu Smalltalk-Persönlichkeiten finden Sie [hier](../Concepts/best-practices.md#chit-chat).

Weitere Informationen zur Standardantwort finden Sie unter [Keine Übereinstimmung gefunden](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Tutorial: Erstellen eines QnA-Bots mit Azure Bot Service v4](create-qna-bot.md)