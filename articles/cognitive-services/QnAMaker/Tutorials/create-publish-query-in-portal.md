---
title: 'Tutorial: Erstellen und Veröffentlichen einer Wissensdatenbank und Beantworten von Fragen auf der Grundlage der Wissensdatenbank über das QnA Maker-Portal'
titleSuffix: Azure Cognitive Services
description: In diesem portalbasierten Tutorial erfahren Sie Schritt für Schritt, wie Sie eine Wissensdatenbank programmgesteuert erstellen und veröffentlichen und anschließend zur Beantwortung von Fragen heranziehen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: diberry
ms.openlocfilehash: 08f708f740b90f27af5443b46c5d03bef688bd45
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221601"
---
# <a name="tutorial-create-a-knowledge-base-then-answer-question-via-the-qna-maker-portal"></a>Tutorial: Erstellen einer Wissensdatenbank und Beantworten von Fragen über das QnA Maker-Portal

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie eine Knowledge Base erstellen und veröffentlichen und anschließend zur Beantwortung von Fragen heranziehen.

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
* Erstellen einer Wissensdatenbank über das QnA Maker-Portal
* Überprüfen, Speichern und Trainieren der Wissensdatenbank
* Veröffentlichen der Wissensdatenbank
* Abfragen der Wissensdatenbank mithilfe von Curl

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
    |„Microsoft Azure Directory Id“ (Microsoft Azure-Verzeichnis-ID)|Ihre _Microsoft Azure-Verzeichnis-ID_ ist dem Konto zugeordnet, das Sie für die Anmeldung beim Azure-Portal und beim QnA Maker-Portal verwenden. |
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
    |„Chit-chat personality“ (Persönlichkeit für Geplauder)|Der Freund|Dadurch erhalten Sie eine freundliche und lockere Persönlichkeit für allgemeine Fragen und Antworten. Die Fragen und Antworten können später bearbeitet werden. |

    ![Schritt 4 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Wählen Sie **Create your KB** (Wissensdatenbank erstellen) aus, um den Erstellungsprozess abzuschließen.

    ![Schritt 5 des Erstellungsprozesses für die Wissensdatenbank](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-kb-save-and-train"></a>Überprüfen, Speichern und Trainieren der Wissensdatenbank

1. Überprüfen Sie die Fragen und Antworten. Auf der ersten Seite befinden sich Fragen und Antworten von der URL. 

    ![Speichern und trainieren](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Wählen Sie die letzte Seite mit Fragen und Antworten vom Ende der Tabelle aus. Auf der Seite werden Fragen und Antworten der Persönlichkeit für Geplauder angezeigt. 

1. Wählen Sie auf der Symbolleiste über der Liste mit Fragen und Antworten das Zahnradsymbol aus. Daraufhin werden die Filter für die einzelnen Fragen und Antworten angezeigt. Für Fragen im Plauderton ist bereits der Filter **editorial: chitchat** festgelegt. Dieser Filter wird zusammen mit der ausgewählten Antwort an die Clientanwendung zurückgegeben. Anhand dieses Filters kann die Clientanwendung (beispielsweise ein Chatbot) zusätzliche Verarbeitungsschritte oder Interaktionen mit dem Benutzer bestimmen.

    ![Anzeigen von Filtern](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Wählen Sie auf der oberen Menüleiste die Option **Save and train** (Speichern und trainieren) aus.

## <a name="publish-to-get-kb-endpoints"></a>Veröffentlichen, um Endpunkte für die Wissensdatenbank zu erhalten

Wählen Sie im oberen Menü die Schaltfläche **Veröffentlichen** aus. Wählen Sie auf der Veröffentlichungsseite neben der Schaltfläche **Abbrechen** die Schaltfläche **Veröffentlichen** aus.

![Veröffentlichen](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Nachdem die Wissensdatenbank veröffentlicht wurde, wird der Endpunkt angezeigt.

![Veröffentlichen](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

## <a name="use-curl-to-query-for-an-faq-answer"></a>Abfragen einer FAQ-Antwort mithilfe von Curl

1. Wählen Sie die Registerkarte **Curl** aus. 

    ![Curl-Befehl](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Kopieren Sie den Text auf der Registerkarte **Curl**, und führen Sie ihn in einem Curl-fähigen Terminal oder an einer Curl-fähigen Befehlszeile aus. Der Wert des Autorisierungsheaders enthält den Text `Endpoint `, gefolgt von einem Leerzeichen und dem Schlüssel.

1. Ersetzen Sie `<Your question>` durch `How large can my KB be?`. Dies ähnelt der Frage `How large a knowledge base can I create?`, ist aber nicht exakt das Gleiche. QnA Maker ermittelt mittels Verarbeitung natürlicher Sprache, dass die beiden Fragen gleich sind.     

1. Führen Sie den Curl-Befehl aus, um die JSON-Antwort mit der Bewertung und der Antwort auf die Frage zu erhalten. 

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Abfragen einer Antwort im Plauderton mithilfe von Curl

1. Ersetzen Sie `How large can my KB be?` im Curl-fähigen Terminal durch eine Benutzeraussage, die das Ende der Botunterhaltung signalisiert (etwa `Thank you`).   

1. Führen Sie den Curl-Befehl aus, um die JSON-Antwort mit der Bewertung und der Antwort auf die Frage zu erhalten. 

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

    Da die Frage `Thank you` exakt einer Frage im Plauderton entspricht, ist QnA Maker zu 100 Prozent überzeugt. QnA Maker hat auch alle dazugehörigen Fragen sowie die Metadateneigenschaft mit den Informationen zum Filter für Geplauder zurückgegeben.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Abfragen der Standardantwort mithilfe von Curl

Jede Frage, bei deren Antwort QnA Maker nicht sicher ist, wird mit einer Standardantwort beantwortet. Diese Antwort wird im Azure-Portal konfiguriert. 

1. Ersetzen Sie `Thank you` im Curl-fähigen Terminal durch `x`. 

1. Führen Sie den Curl-Befehl aus, um die JSON-Antwort mit der Bewertung und der Antwort auf die Frage zu erhalten. 

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
    
    QnA Maker hat als Bewertung den Wert 0 (nicht überzeugt) sowie die Standardantwort zurückgegeben. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu unterstützten Datenformaten finden Sie unter [Datenquellen für QnA Maker-Inhalt](../Concepts/data-sources-supported.md). 

Weitere Informationen zu Persönlichkeiten für Geplauder finden Sie [hier](../Concepts/best-practices.md#chit-chat).

Weitere Informationen zur Standardantwort finden Sie unter [Keine Übereinstimmung gefunden](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Wichtige Konzepte für Wissensdatenbanken](../Concepts/knowledge-base.md)