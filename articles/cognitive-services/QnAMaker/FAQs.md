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
ms.date: 11/27/2018
ms.author: tulasim
ms.openlocfilehash: 28153e71ba6f2336398517454a81cfce39b93956
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496336"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

## <a name="manage-the-knowledge-base"></a>Verwalten der Wissensdatenbank

### <a name="i-accidentally-deleted-a-part-of-my-qna-maker-what-should-i-do"></a>Ich habe versehentlich einen Teil von QnA Maker gelöscht. Was soll ich tun? 

Alle Löschvorgänge sind endgültig, dazu gehört auch das Löschen von Frage/Antwort-Paaren, Dateien, URLs, benutzerdefinierten Fragen und Antworten, Wissensdatenbanken oder Azure-Ressourcen. Stellen Sie sicher, dass Sie Ihre Wissensdatenbank von der Seite **Einstellungen** exportieren, bevor Sie Teile der Wissensdatenbank löschen. 

### <a name="why-is-my-urlsfiles-not-extracting-question-answer-pairs"></a>Warum extrahieren meine URLs/Dateien keine Frage-Antwort-Paare?

Es ist möglich, dass QnA Maker einige Frage-und-Antwort-Inhalte (Question and Answer, QnA) aus gültigen FAQ-URLs nicht automatisch extrahieren kann. In solchen Fällen können Sie den QnA-Inhalt in eine TXT-Datei einfügen und prüfen, ob das Tool diesen Inhalt erfassen kann. Alternativ dazu können Sie Ihrer Wissensdatenbank über das [QnA Maker-Portal](https://qnamaker.ai) redaktionell Inhalte hinzufügen.

### <a name="how-large-a-knowledge-base-can-i-create"></a>Wie groß darf eine von mir erstellte Wissensdatenbank sein?

Die Größe der Wissensdatenbank hängt von der beim Erstellen des QnA Maker-Diensts ausgewählten SKU für Azure Search ab. Ausführlichere Informationen finden Sie [hier](./Tutorials/choosing-capacity-qnamaker-deployment.md).

### <a name="why-cant-i-see-anything-in-the-drop-down-when-i-try-to-create-a-new-knowledge-base"></a>Warum werden in der Dropdownliste keine Einträge angezeigt, wenn ich versuche, eine neue Wissensdatenbank zu erstellen?

Sie haben noch keine QnA Maker-Dienste in Azure erstellt. Klicken Sie [hier](./How-To/set-up-qnamaker-service-azure.md), um zu erfahren, wie das funktioniert.

### <a name="how-do-i-share-a-knowledge-base-with-others"></a>Wie gebe ich eine Wissensdatenbank für andere Personen frei?

Die Freigabe erfolgt auf der Ebene eines QnA Maker-Diensts, was bedeutet, dass alle Wissensdatenbanken in diesem Dienst freigegeben werden. [Hier](./How-To/collaborate-knowledge-base.md) finden Sie Informationen zum Zusammenarbeiten an einer Wissensdatenbank.

### <a name="can-you-share-a-knowledge-base-with-a-contributor-that-is-not-in-the-same-aad-tenant-to-modify-a-knowledge-base"></a>Kann ich eine Wissensdatenbank zur Bearbeitung für einen Mitwirkenden freigeben, der sich nicht im selben AAD-Mandanten befindet? 

Die Freigabe basiert auf der rollenbasierten Zugriffssteuerung in Azure. Wenn Sie _jede_ Ressource in Azure für einen anderen Benutzer freigeben können, können Sie auch QnA Maker freigeben.

### <a name="if-you-have-an-app-service-plan-with-5-qnamaker-knowledge-bases-can-you-assign-readwrite-rights-to-5-different-users-so-each-of-them-can-access-only-1-qnamaker-knowledge-base"></a>Angenommen, ich verfüge über einen App Service-Plan mit fünf QnA Maker-Wissensdatenbanken. Kann ich die Lese-/Schreibberechtigungen fünf verschiedenen Benutzern zuweisen, sodass jeder dieser Benutzer nur auf eine QnA Maker-Wissensdatenbank zugreifen kann?

Sie können den gesamten QnA Maker-Dienst freigeben, nicht aber einzelne Wissensdatenbanken.

### <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Wie kann ich die Standardmeldung ändern, die angezeigt wird, wenn keine gute Übereinstimmung gefunden wurde?

Die Standardmeldung ist Teil der Einstellungen in Ihrem App-Dienst.
- Wechseln Sie im Azure-Portal zu Ihrer App Service-Ressource.

![QnA Maker-App-Dienst](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Klicken Sie auf die Option **Einstellungen**.

![Einstellungen des QnA Maker-App-Diensts](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Ändern Sie den Wert der Einstellung **DefaultAnswer**.
- Starten Sie Ihren App-Dienst neu.

![QnA Maker-App-Dienst neu starten](./media/qnamaker-faq/qnamaker-appservice-restart.png)

### <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Warum wird mein SharePoint-Link nicht extrahiert?

Weitere Informationen finden Sie unter [Speicherorte von Datenquellen](./Concepts/data-sources-supported.md#data-source-locations).

### <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>Die Aktualisierungen, die ich an meiner Wissensdatenbank vorgenommen habe, sind beim Veröffentlichen nicht mehr vorhanden. Warum nicht?

Jeder Bearbeitungsvorgang, sei es eine Tabellenaktualisierung, ein Test oder eine Änderung der Einstellungen, muss gespeichert werden, bevor die Veröffentlichung erfolgen kann. Klicken Sie nach jedem Bearbeitungsvorgang unbedingt auf die Schaltfläche zum  **Speichern und Trainieren** .

### <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Unterstützt die Wissensdatenbank komplexe Daten oder Multimedia?

Die Wissensdatenbank unterstützt Markdown. Bei der automatischen Extrahierung aus URLs ist die Funktionalität zur Konvertierung von HTML zu Markdown jedoch eingeschränkt. Wenn Sie Markdown im vollen Umfang nutzen möchten, können Sie Ihre Inhalte direkt in der Tabelle bearbeiten oder eine Wissensdatenbank mit den komplexen Inhalten hochladen.

Multimediainhalte wie Bilder und Videos werden zu diesem Zeitpunkt nicht unterstützt.

### <a name="does-qna-maker-support-non-english-languages"></a>Unterstützt QnA Maker andere Sprachen als Englisch?

Weitere Informationen hierzu finden Sie unter [Unterstützte Sprachen](./Overview/languages-supported.md).

Wenn Sie über Inhalte in verschiedenen Sprachen verfügen, stellen Sie sicher, dass Sie für jede Sprache einen separaten Dienst erstellen.

## <a name="manage-service"></a>Verwalten eines Diensts

### <a name="when-should-i-restart-my-app-service"></a>Wann sollte ich meinen App-Dienst neu starten? 

Aktualisieren Sie Ihren App-Dienst, wenn sich das Symbol „Vorsicht“ neben dem Versionswert für die Wissensdatenbank in der Tabelle **Endpunktschlüssel** auf der [Seite](https://www.qnamaker.ai/UserSettings) **Benutzereinstellungen** befindet.

### <a name="when-should-i-refresh-my-endpoint-keys"></a>Wann sollte ich meine Endpunktschlüssel aktualisieren?

Aktualisieren Sie Ihre Endpunktschlüssel, wenn Sie den Verdacht hegen, dass sie gestohlen oder ausgespäht wurden.

### <a name="can-i-use-the-same-azure-search-resource-for-knowledge-bases-using-multiple-languages"></a>Kann ich die gleiche Azure Search-Ressource für Wissensdatenbanken in mehreren Sprachen verwenden?

Um mehrere Wissensdatenbanken und mehrere Sprachen zu nutzen, müssen Benutzer eine QnA Maker-Ressource für jede Sprache erstellen. Dadurch wird ein separater Azure Search-Dienst pro Sprache erstellt. Das Kombinieren von Wissensdatenbanken verschiedener Sprachen in einem einzelnen Azure Search-Dienst führt zur einer niedrigeren Relevanz der Ergebnisse.

### <a name="how-can-i-change-the-name-of-the-azure-search-resource-used-by-qna-maker"></a>Wie kann ich den Namen der Azure Search-Ressource, die von QnA Maker verwendet wird, ändern?

Der Name der Azure Search-Ressource ist der Name der QnA Maker-Ressource, an den einige zufällige Buchstaben angefügt sind. Dadurch wird es schwierig, mehrere Search-Ressourcen für QnA Maker zu unterscheiden. Erstellen Sie einen separaten Azure Search-Dienst (benennen Sie ihn nach Belieben), und verbinden Sie ihn mit Ihrem QnA-Dienst. Die Schritte sind ähnlich denen für ein [Upgrade von Azure Search](How-To/upgrade-qnamaker-service.md#upgrade-azure-search-service).

## <a name="integrate-with-other-services-including-bots"></a>Integration in andere Dienste, einschließlich Bots

### <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Muss ich Bot Framework nutzen, um QnA Maker verwenden zu können?

Nein, Sie müssen Bot Framework nicht in Verbindung mit QnA Maker verwenden. QnA Maker wird jedoch als eine von mehreren Vorlagen in Azure Bot Service angeboten. Bot Service ermöglicht die schnelle, intelligente Botentwicklung über Microsoft Bot Framework und wird in einer serverlosen Umgebung ausgeführt.

### <a name="how-can-i-create-a-bot-with-qna-maker"></a>Wie kann ich einen Bot mit QnA Maker erstellen?

Folgen Sie den Anweisungen in [dieser](./Tutorials/create-qna-bot.md) Dokumentation, um Ihren Bot mit Azure Bot Service zu erstellen.

### <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Wie bette ich den QnA Maker-Dienst in meine Website ein?

Führen Sie die folgenden Schritte aus, um den QnA Maker-Dienst als Webchat-Steuerelement in Ihre Website einzubetten:

1. Erstellen Sie Ihren FAQ-Bot, indem Sie den [hier](./Tutorials/create-qna-bot.md) angegebenen Anweisungen folgen.
2. Aktivieren Sie den Webchat, indem Sie die [hier](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat) aufgeführten Schritte ausführen.

## <a name="data-storage"></a>Datenspeicher

### <a name="what-data-is-stored-and-where-is-it-stored"></a>Welche Daten werden gespeichert, und wo werden sie gespeichert? 

Als Sie den QnA Maker-Dienst erstellt haben, haben Sie eine Azure-Region ausgewählt. Ihre Wissensdatenbanken und Protokolldateien werden in dieser Region gespeichert. 
