---
title: 'FAQ: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Liste häufig gestellter Fragen zu QnA Maker-Dienst
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: efefd595c43d7f46ff1ead91577d070cf8fb90e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47164615"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>Warum extrahieren meine URLs/Dateien keine Frage-Antwort-Paare?

Es ist möglich, dass QnA Maker einige Frage-und-Antwort-Inhalte (Question and Answer, QnA) aus gültigen FAQ-URLs nicht automatisch extrahieren kann. In solchen Fällen können Sie den QnA-Inhalt in eine TXT-Datei einfügen und prüfen, ob das Tool diesen Inhalt erfassen kann. Alternativ können Sie Ihrer Wissensdatenbank Inhalte manuell hinzufügen.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Wie groß darf eine von mir erstellte Wissensdatenbank sein?

Die Größe der Wissensdatenbank hängt von der beim Erstellen des QnA Maker-Diensts ausgewählten SKU für Azure Search ab. Ausführlichere Informationen finden Sie [hier](./Tutorials/choosing-capacity-qnamaker-deployment.md).

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Warum werden in der Dropdownliste keine Einträge angezeigt, wenn ich versuche, eine neue Wissensdatenbank zu erstellen?

Sie haben noch keine QnA Maker-Dienste in Azure erstellt. [Hier](./How-To/set-up-qnamaker-service-azure.md) wird erläutert, wie das geht.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Wie gebe ich eine Wissensdatenbank für andere Personen frei?

Die Freigabe erfolgt auf der Ebene eines QnA Maker-Diensts, was bedeutet, dass alle Wissensdatenbanken in diesem Dienst freigegeben werden. [Hier](./How-To/collaborate-knowledge-base.md) finden Sie Informationen zum Zusammenarbeiten an einer Wissensdatenbank.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Wie kann ich die Standardmeldung ändern, die angezeigt wird, wenn keine gute Übereinstimmung gefunden wurde?

Die Standardmeldung ist Teil der Einstellungen in Ihrem App-Dienst.
- Wechseln Sie im Azure-Portal zu Ihrer App Service-Ressource

![QnA Maker-App-Dienst](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Klicken Sie auf die Option **Einstellungen**.

![Einstellungen des QnA Maker-App-Diensts](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Ändern Sie den Wert der Einstellung **DefaultAnswer**.
- Starten Sie Ihren App-Dienst neu.

![QnA Maker-App-Dienst neu starten](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Warum wird mein SharePoint-Link nicht extrahiert?

Das Tool analysiert nur öffentliche URLs und unterstützt zu diesem Zeitpunkt keine authentifizierten Datenquellen. Alternativ können Sie die Datei herunterladen und die Dateiuploadoption verwenden, um Fragen und Antworten zu extrahieren.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>Die Aktualisierungen, die ich an meiner Wissensdatenbank vorgenommen habe, sind beim Veröffentlichen nicht mehr vorhanden. Warum nicht?

Jeder Bearbeitungsvorgang, sei es eine Tabellenaktualisierung, ein Test oder eine Änderung der Einstellungen, muss gespeichert werden, bevor er veröffentlicht werden kann. Klicken Sie nach jedem Bearbeitungsvorgang unbedingt auf die Schaltfläche zum Speichern und Trainieren.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>Wann sollte ich meine Endpunktschlüssel aktualisieren?

Sie sollten Ihre Endpunktschlüssel aktualisieren, wenn Sie vermuten, dass sie gefährdet sind.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Unterstützt die Wissensdatenbank komplexe Daten oder Multimedia?

Die Wissensdatenbank unterstützt Markdown. Bei der automatischen Extrahierung aus URLs ist die Funktionalität zur Konvertierung von HTML zu Markdown jedoch eingeschränkt. Wenn Sie Markdown im vollen Umfang nutzen möchten, können Sie Ihre Inhalte direkt in der Tabelle bearbeiten oder eine Wissensdatenbank mit den komplexen Inhalten hochladen.

Multimediainhalte wie Bilder und Videos werden zu diesem Zeitpunkt nicht unterstützt.

## <a name="does-qna-maker-support-non-english-languages"></a>Unterstützt QnA Maker andere Sprachen als Englisch?

Weitere Informationen hierzu finden Sie unter [Unterstützte Sprachen](./Overview/languages-supported.md).

Wenn Sie über Inhalte in verschiedenen Sprachen verfügen, stellen Sie sicher, dass Sie für jede Sprache einen separaten Dienst erstellen.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Muss ich Bot Framework nutzen, um QnA Maker verwenden zu können?

Nein, Sie müssen Bot Framework nicht in Verbindung mit QnA Maker verwenden. QnA Maker wird jedoch als eine von mehreren Vorlagen in Azure Bot Service angeboten. Azure Bot Service ermöglicht die schnelle, intelligente Botentwicklung über Microsoft Bot Framework und wird in einer serverlosen Umgebung ausgeführt.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Wie kann ich einen Bot mit QnA Maker erstellen?

Folgen Sie den Anweisungen in [dieser](./Tutorials/create-qna-bot.md) Dokumentation, um Ihren Bot mit Azure Bot Service zu erstellen.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Wie bette ich den QnA Maker-Dienst in meine Website ein?

Führen Sie die folgenden Schritte aus, um den QnA Maker-Dienst als Webchat-Steuerelement in Ihre Website einzubetten:

1. Erstellen Sie Ihren FAQ-Bot, indem Sie den [hier](./Tutorials/create-qna-bot.md) angegebenen Anweisungen folgen.
2. Aktivieren Sie den Webchat, indem Sie die [hier](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat) aufgeführten Schritte ausführen.


