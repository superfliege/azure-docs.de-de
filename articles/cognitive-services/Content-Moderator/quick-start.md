---
title: Erste Schritte mit Content Moderator
titlesuffix: Azure Cognitive Services
description: Beschreibt die ersten Schritte mit Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: c2ac0ccd89b5f1436a151e3d69c5d7423090f244
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225293"
---
# <a name="get-started-with-content-moderator"></a>Erste Schritte mit Content Moderator

Sie können die ersten Schritte mit Content Moderator folgendermaßen unternehmen:

- [Beginnen Sie mit dem Prüfungstool](#start-with-the-review-tool), um den API-Schlüssel abzurufen und ein Prüfungsteam zu erstellen. Der Vorteil ist, dass Sie den API-Schlüssel verwenden können, um die Moderations-APIs aufzurufen, um Inhalte zu überprüfen, sowie die Überprüfungs-APIs, um Bewertungen zu generieren, ohne zusätzliche Schritte zu unternehmen.
- [Abonnieren Sie Content Moderator](#start-with-the-apis) in Azure, um den API-Schlüssel abzurufen. Lesen Sie die [API-Referenz](api-reference.md) und die [SDKs](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Sie müssen sich online registrieren, um ein Prüfungsteam zu erstellen.
- [Verwenden Sie den Flow-Connector und Flow-Vorlagen](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/), um zahlreiche Integrationen mit einem einfach zu bedienenden Dienst zu testen.

Unabhängig von der ausgewählten Option erfahren Sie mehr über Ihre API-Anmeldeinformationen im Artikel [Verwalten von Anmeldeinformationen](review-tool-user-guide/credentials.md).

## <a name="start-with-the-review-tool"></a>Erste Schritte mit dem Prüfungstool
[Melden Sie sich](http://contentmoderator.cognitive.microsoft.com/) auf der Website des Content Moderator-Prüfungstools an.

![Content Moderator-Homepage](images/homepage.PNG)

### <a name="create-a-review-team"></a>Erstellen eines Prüfungsteams
Geben Sie Ihrem Team einen Namen. Sie können Ihre Kollegen einladen, indem Sie ihre E-Mail-Adressen eingeben.

![Einladen von Teammitgliedern](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Hochladen von Bildern oder Eingabe von Text
Klicken Sie auf **Versuchen > Bild** oder auf **Versuchen > Text**. Laden Sie für die Moderation bis zu fünf Beispielbilder hoch, oder geben Sie dafür Beispieltext ein.

![Bild- oder Textmoderation](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Übermittlung zur automatisierten Moderation
Übermitteln Sie Ihre Inhalte für die automatisierte Moderation. Intern ruft das Prüfungstool die Moderations-APIs auf, um Ihre Inhalte zu überprüfen. Wenn die Überprüfung abgeschlossen ist, informiert Sie eine Meldung über die zu überprüfenden Ergebnisse.

![Moderationsdateien](images/submitted.png)

### <a name="review-and-confirm-results"></a>Überprüfen und Bestätigen der Ergebnisse
Überprüfen Sie die automatisch moderierten Tags, ändern Sie diese bei Bedarf, und übermitteln Sie sie über die Schaltfläche **Weiter**. Wenn Ihre Geschäftsanwendung die Moderator-APIs aufruft, werden markierte Inhalte der Warteschlange hinzugefügt, damit die Prüfungsteams sie kontrollieren können. So lassen sich große Mengen von Inhalten schnell überprüfen.

![Überprüfen der Ergebnisse](images/reviewresults.png)

Lernen Sie alle [Funktionen des Prüfungstools](Review-Tool-User-Guide/human-in-the-loop.md) kennen, oder erfahren Sie im nächsten Abschnitt mehr über die APIs. Überspringen Sie die Registrierung. Den API-Schlüssel finden Sie im Prüfungstool, wie im Artikel [Verwalten von Anmeldeinformationen](review-tool-user-guide/credentials.md) gezeigt.

### <a name="use-the-apis"></a>Verwenden der APIs

Erfahren Sie, wie Sie Content Moderator in Ihre Geschäftsanwendungen integrieren. Lesen Sie die [API-Referenz](api-reference.md) und die [SDKs](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net).

## <a name="subscribe-in-the-azure-portal"></a>Abonnieren im Azure-Portal

[Abonnieren Sie Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) im Azure-Portal. Beginnen Sie mit einer der folgenden APIs:

### <a name="image-moderation"></a>Bildmoderation

Nutzen Sie die [API-Konsole](try-image-api.md) oder den [.NET-Schnellstart](image-moderation-quickstart-dotnet.md), um Bilder zu überprüfen und anzügliche sowie nur für Erwachsene geeignete Inhalte mithilfe von Tags, Zuverlässigkeitsbewertungen und anderen extrahierten Informationen zu erkennen.

### <a name="text-moderation"></a>Textmoderation

Nutzen Sie die [API-Konsole](try-text-api.md) oder den [.NET-Schnellstart](text-moderation-quickstart-dotnet.md), um Textinhalte auf obszöne, computergestützte unerwünschte Textklassifizierung (Vorschau) und personenbezogene Informationen zu überprüfen. 


### <a name="video-moderation"></a>Videomoderation

Nutzen Sie den [.NET-Schnellstart](video-moderation-api.md), um Videos zu überprüfen und anzügliche sowie nur für Erwachsene geeignete Inhalte zu erkennen. 


### <a name="review-apis"></a>Überprüfen von APIs

Wählen Sie zuerst die API für den Auftrag, die Überprüfung und den Workflow aus.

- Die [Auftrags-API](try-review-api-job.md) überprüft Ihre Inhalte mithilfe der Moderations-APIs und generiert im Prüfungstool Überprüfungen. 
- Die [Überprüfungs-API](try-review-api-review.md) erstellt direkt Bild-, Text- oder Videoüberprüfungen für Moderatoren, ohne die Inhalte zuerst zu scannen. 
- Die [Workflow-API](try-review-api-workflow.md) erstellt und aktualisiert Informationen zu benutzerdefinierten Workflows Ihres Teams und ruft diese ab.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [API-Referenz](api-reference.md) und die [SDKs](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Beschleunigen Sie Ihre Integration mit den [.NET SDK-Beispielen](sdk-and-samples.md#net-sdk-samples), den [REST-API-Beispiele in C#](https://github.com/sanjeev3/azure-docs-pr/blob/master/articles/cognitive-services/Content-Moderator/sdk-and-samples.md#rest-api-samples-in-c) und [Lernprogrammen](sdk-and-samples.md#tutorials).
