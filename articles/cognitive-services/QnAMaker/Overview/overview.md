---
title: Was ist QnA Maker?
titleSuffix: Azure Cognitive Services
description: Mit QnA Maker können Sie einen Frage-und-Antwort-Dienst auf der Grundlage Ihrer teilweise strukturierten Inhalte bereitstellen. Bei solchen Inhalten kann es sich beispielsweise um FAQ-Dokumente oder um URLs und Produkthandbücher handeln.
services: cognitive-services
author: tulasim88
manager: cjgronlund
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 08f3b6424d94195d2606e4cdb9be470e2f130909
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985815"
---
# <a name="what-is-qna-maker"></a>Was ist QnA Maker?

Mit [QnA Maker](https://qnamaker.ai) können Sie einen Frage-und-Antwort-Dienst auf der Grundlage Ihrer teilweise strukturierten Inhalte bereitstellen. Bei solchen Inhalten kann es sich beispielsweise um FAQ-Dokumente (Frequently Asked Questions, häufig gestellte Fragen) oder um URLs und Produkthandbücher handeln. Sie können ein flexibles Modell mit Fragen und Antworten für Benutzerabfragen erstellen und Antworten liefern, die ein trainierter Bot auf natürliche Weise wie in einem Gespräch verwendet.

Über eine benutzerfreundliche grafische Benutzeroberfläche können Sie Ihren Dienst ganz ohne Entwicklerkenntnisse erstellen, verwalten, trainieren und einrichten.

![Übersicht](../media/qnamaker-overview-learnabout/overview.png)

## <a name="highlights"></a>Highlights

- Umfassende **codefreie** Umgebung zum [Erstellen eines FAQ-Bots](https://aka.ms/qnamaker-docs-create-faqbot).
- **Keine Netzwerkdrosselung mehr:** Zahlen Sie für das Diensthosting, nicht für die Anzahl von Transaktionen. Weitere Informationen finden Sie auf der [Seite mit der Preisübersicht](https://aka.ms/qnamaker-docs-pricing).
- **Bedarfsgerechte Skalierung:** Wählen Sie die passenden SKUs der einzelnen Komponenten für Ihr individuelles Szenario. Informationen zum Auswählen der Kapazität für Ihren QnA Maker-Dienst finden Sie [hier](https://aka.ms/qnamaker-docs-capacity).
- **Umfassende Datencompliance:** Die Daten und Laufzeitkomponenten werden im Azure-Abonnement des Benutzers und innerhalb seiner Compliancegrenzen bereitgestellt. Weitere Informationen finden Sie weiter unten.

## <a name="key-qna-maker-processes"></a>Schlüsselprozesse von QnA Maker

Eine QnA Maker-Instanz stellt zwei zentrale Dienste für Ihre Daten bereit:

* **Extraktion:** Strukturierte Frage-Antwort-Daten werden aus teilweise strukturierten Datenquellen wie FAQs und Produkthandbüchern extrahiert. Diese Extraktion erfolgt bei der Erstellung der Wissensdatenbank. Informationen zum Erstellen Ihrer Wissensdatenbank finden Sie [hier](https://aka.ms/qnamaker-docs-createkb).

* **Abgleich:** Nach dem [Trainieren und Testen](https://aka.ms/qnamaker-docs-trainkb) Ihrer Wissensdatenbank können Sie sie [veröffentlichen](https://aka.ms/qnamaker-docs-publishkb). Dadurch kann Ihre QnA Maker-Wissensdatenbank von einem Endpunkt genutzt werden, der wiederum in Ihrem Bot oder in Ihrer App verwendet werden kann. Dieser Endpunkt nimmt eine Benutzerfrage entgegen und antwortet mit dem besten Frage-Antwort-Treffer aus der Wissensdatenbank sowie mit einer Zuverlässigkeitsbewertung für den Treffer.

## <a name="qna-maker-architecture"></a>Architektur von QnA Maker

Der QnA Maker-Stack umfasst folgende Komponenten:

1. **QnA Maker-Verwaltungsdienste (Steuerungsebene):** Die Verwaltungsumgebung für eine QnA Maker-Wissensdatenbank mit Ersterstellung, Aktualisierung, Training und Veröffentlichung. Diese Aktivitäten können über das [Portal](https://qnamaker.ai) oder über [Verwaltungs-APIs](https://aka.ms/qnamaker-v4-apis) ausgeführt werden. Die Verwaltungsdienste kommunizieren mit den weiter unten angegebenen Laufzeitkomponenten.

2. **QnA Maker-Laufzeit (Datenebene):** Daten und Laufzeit werden im Azure-Abonnement des Benutzers in einer Region seiner Wahl bereitgestellt. Der Inhalt von Kundenfragen/-antworten wird in [Azure Search](https://azure.microsoft.com/services/search/) gespeichert. Die Laufzeit wird als [App Service](https://azure.microsoft.com/services/app-service/) bereitgestellt. Optional können Sie auch eine [Application Insights](https://azure.microsoft.com/services/application-insights/)-Ressource für Analysen bereitstellen.

![Architecture](../media/qnamaker-overview-learnabout/architecture.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Create a QnA Maker service](../how-to/set-up-qnamaker-service-azure.md) (Erstellen eines QnA Maker-Diensts)
