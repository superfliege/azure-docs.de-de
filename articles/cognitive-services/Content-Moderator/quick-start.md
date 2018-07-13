---
title: Erste Schritte mit Azure Content Moderator | Microsoft-Dokumentation
description: Erste Schritte mit Azure Content Moderator
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: ae4333047ebd95733c7baaed0323a0c2c477d323
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374315"
---
# <a name="get-started-with-content-moderator"></a>Erste Schritte mit Content Moderator

Sie können die Content Moderator-APIs und das Prüfungstool folgendermaßen nutzen:

- [Mit dem Prüfungstool](#start-with-the-review-tool) können Sie sowohl API-Schlüssel als auch ein Prüfungsteam erstellen. Erkunden Sie das Prüfungstool, und erfahren Sie, wie Sie mit Content Moderator-APIs Integrationen vornehmen.
- [Abonnieren Sie Content Moderator](#start-with-the-apis) im Azure-Portal. Sie müssen sich online registrieren, um ein Prüfungsteam zu erstellen.
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

Sie haben die Inhaltsmoderation und das Prüfungstool kennengelernt. Lesen Sie nun, wie Sie Content Moderator in Ihre Geschäftsanwendungen integrieren. Der folgende Abschnitt enthält weitere Informationen zu Software Development Kits (SDKs) und Beispiele.

## <a name="start-with-the-apis"></a>Erste Schritte mit den APIs

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

Erfahren Sie mehr über die Inhaltsmoderation mit der [Bildmoderations-API](image-moderation-api.md).
