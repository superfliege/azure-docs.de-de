---
title: Was ist QnA Maker?
titleSuffix: Azure Cognitive Services
description: QnA Maker ist ein cloudbasierter API-Dienst, bei dem benutzerdefinierte Intelligence-Technologie für maschinelles Lernen auf die Frage eines Benutzers in natürlicher Sprache angewendet wird, um die beste Antwort bereitstellen zu können.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: bd859183a13e0f8a21cdd2eabb464b718e949464
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212215"
---
# <a name="what-is-qna-maker"></a>Was ist QnA Maker?

QnA Maker ist ein cloudbasierter API-Dienst, mit dem eine Frage-und-Antwort-Ebene im Konversationsstil für Ihre Daten erstellt wird. 

Mit QnA Maker können Sie eine Wissensdatenbank (Knowledge Base, KB) für Ihre teilweise strukturierten Inhalte erstellen, z.B. URLs für häufig gestellte Fragen (FAQs), Produkthandbücher, unterstützende Dokumente und benutzerdefinierte Fragen und Antworten. Mit dem QnA Maker-Dienst werden die von Ihren Benutzern in natürlicher Sprache gestellten Fragen beantwortet, indem aus den Fragen und Antworten in Ihrer Wissensdatenbank die bestmögliche Antwort zugewiesen wird.

Über das einfach zu verwendende [Webportal](https://qnamaker.ai) können Sie Ihren Dienst ohne jegliche Entwicklererfahrung erstellen, verwalten, trainieren und veröffentlichen. Nachdem der Dienst auf einem Endpunkt veröffentlicht wurde, kann eine Clientanwendung, z.B. ein Chatbot, die Konversation mit einem Benutzer verwalten, um Fragen zu erhalten und darauf zu antworten. 

![Übersicht](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Schlüsselprozesse von QnA Maker

QnA Maker stellt zwei zentrale Dienste für Ihre Daten bereit:

* **Extraktion:** Strukturierte Frage-Antwort-Daten werden aus strukturierten und teilweise strukturierten [Datenquellen](../Concepts/data-sources-supported.md) extrahiert, z.B. FAQs und Produkthandbüchern. Diese Extraktion kann bei der [Erstellung](https://aka.ms/qnamaker-docs-createkb) der Wissensdatenbank oder im Rahmen des Bearbeitungsprozesses durchgeführt werden.

* **Abgleich:** Nach dem [Trainieren und Testen](https://aka.ms/qnamaker-docs-trainkb) Ihrer Wissensdatenbank können Sie sie [veröffentlichen](https://aka.ms/qnamaker-docs-publishkb). Hierdurch kann Ihre QnA Maker-Wissensdatenbank von einem Endpunkt genutzt werden, der wiederum in Ihrem Bot oder in Ihrer Client-App verwendet werden kann. Dieser Endpunkt nimmt eine Benutzerfrage entgegen und antwortet mit der besten Antwort aus der Wissensdatenbank sowie mit einer Zuverlässigkeitsbewertung für den Treffer.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>Architektur von QnA Maker

Die QnA Maker-Architektur umfasst die beiden folgenden Komponenten:

1. **QnA Maker-Verwaltungsdienste:** Die Verwaltungsumgebung für eine QnA Maker-Wissensdatenbank mit Ersterstellung, Aktualisierung, Training und Veröffentlichung. Diese Aktivitäten können über das [Portal](https://qnamaker.ai) oder über [Verwaltungs-APIs](https://aka.ms/qnamaker-v4-apis) ausgeführt werden. 

2. **QnA Maker-Daten und -Runtime:** Wird unter Ihrem Azure-Abonnement in Ihrer angegebenen Region bereitgestellt. Ihre Wissensdatenbank-Inhalte werden in [Azure Search](https://azure.microsoft.com/services/search/) gespeichert, und der Endpunkt wird als [App Service](https://azure.microsoft.com/services/app-service/) bereitgestellt. Sie können auch eine [Application Insights](https://azure.microsoft.com/services/application-insights/)-Ressource für Analysen bereitstellen.

![Architecture](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Highlights des Diensts

- Umfassende **codefreie** Umgebung zum [Erstellen eines FAQ-Bots](https://aka.ms/qnamaker-docs-create-faqbot).
- **Keine Netzwerkdrosselung für Vorhersagen**. Zahlen Sie für das Diensthosting, nicht für die Anzahl von Transaktionen. Weitere Informationen finden Sie auf der [Seite mit der Preisübersicht](https://aka.ms/qnamaker-docs-pricing).
- **Bedarfsgerechte Skalierung**. Wählen Sie die passenden SKUs der einzelnen Komponenten für Ihr individuelles Szenario. Informationen zum Auswählen der Kapazität für Ihren QnA Maker-Dienst finden Sie [hier](https://aka.ms/qnamaker-docs-capacity).
- **Umfassende Datencompliance:** Die Komponenten des Vorhersagediensts werden in Ihrem Azure-Abonnement und innerhalb der Konformitätsgrenzen bereitgestellt.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Create a QnA Maker service](../how-to/set-up-qnamaker-service-azure.md) (Erstellen eines QnA Maker-Diensts)
