---
title: Was ist Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Content Moderator verwenden, um nicht angemessene Inhalte in von Benutzern generiertem Material nachzuverfolgen, zu kennzeichnen, zu bewerten und zu filtern.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 6fe69cc56ec874b73083aefab9df8994942368c6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872171"
---
# <a name="what-is-azure-content-moderator"></a>Was ist Azure Content Moderator?

Die Azure Content Moderator-API gehört zu Cognitive Services und überprüft Text-, Bild- und Videoinhalte auf potenziell anstößiges, riskantes oder anderweitig unerwünschtes Material. Gefundenes Material dieser Art wird mithilfe von Flags entsprechend gekennzeichnet. Mit Flags versehene Inhalte können dann von Ihrer App angemessen behandelt werden, um Vorgaben zu erfüllen oder den Benutzern die vorgesehene Umgebung zu bieten. Weitere Informationen zur Bedeutung der verschiedenen Inhaltsflags finden Sie im Abschnitt [Content Moderator-APIs](#content-moderator-apis).

## <a name="where-it-is-used"></a>Einsatzgebiete

Im Anschluss finden Sie einige Szenarien, in denen ein Softwareentwickler oder Team Content Moderator verwenden kann:

- Onlinemarktplätze, die Produktkataloge und andere benutzergenerierte Inhalte moderieren
- Gamingunternehmen, die benutzergenerierte Spielartefakte und Chatrooms moderieren
- Messagingplattformen sozialer Medien, die von Benutzern hinzugefügte Bilder, Texte und Videos moderieren
- Medienunternehmen, die eine zentrale Moderation für ihre Inhalte implementieren
- Anbieter im Bildungswesen, die unangemessene Inhalte für Schüler und Lehrer herausfiltern

## <a name="what-it-includes"></a>Lieferumfang

Der Content Moderator-Dienst umfasst mehrere Webdienst-APIs, die sowohl über REST-Aufrufe als auch über ein .NET SDK verfügbar sind. Darüber hinaus steht ein Tool für die Überprüfung durch Personen zur Verfügung, mit dem Personen den Dienst unterstützen und seine Moderationsfunktion verbessern oder optimieren können.

![Blockdiagramm für Content Moderator mit den Moderations-APIs, den Überprüfungs-APIs und dem Tool für die Überprüfung durch Personen](images/content-moderator-block-diagram.png)

### <a name="content-moderator-apis"></a>Content Moderator-APIs

Der Content Moderator-Dienst verfügt über APIs für folgende Szenarien:

| Aktion | BESCHREIBUNG |
| ------ | ----------- |
|[**Textmoderation**](text-moderation-api.md)| Durchsucht Text nach anstößigen Inhalten, explizit sexuellen oder anzüglichen Inhalten, Obszönitäten und personenbezogenen Informationen (Personally Identifiable Information, PII).|
|[**Benutzerdefinierte Begriffslisten**](try-terms-list-api.md)| Überprüft Text auf Begriffe aus einer benutzerdefinierten Begriffsliste (zusätzlich zu den integrierten Begriffen). Mithilfe von benutzerdefinierten Listen können Sie Inhalte im Einklang mit Ihren eigenen Inhaltsrichtlinien blockieren oder zulassen.|  
|[**Bildmoderation**](image-moderation-api.md)| Überprüft Bilder auf nicht jugendfreie oder freizügige Inhalte, erkennt Text in Bildern mithilfe der optischen Zeichenerkennung (Optical Character Recognition, OCR) und erkennt Gesichter.|
|[**Benutzerdefinierte Bildlisten**](try-image-list-api.md)| Überprüft Bilder anhand einer benutzerdefinierten Liste von Bildern. Mithilfe benutzerdefinierter Bildlisten können Sie Instanzen häufig wiederkehrender Inhalte herausfiltern, die Sie nicht erneut klassifizieren möchten.|
|[**Videomoderation**](video-moderation-api.md)| Überprüft Videos auf nicht jugendfreie oder freizügige Inhalte und gibt Zeitmarkierungen für entsprechende Inhalte zurück.|
|[**Überprüfung**](try-review-api-job.md)| Verwenden Sie die Vorgänge [Jobs](try-review-api-job.md), [Reviews](try-review-api-review.md) und [Workflow](try-review-api-workflow.md), um mit dem Tool für die Überprüfung durch Personen Workflows für die Überprüfung durch Personen zu erstellen und zu automatisieren. Die Workflow-API ist noch nicht über das .NET SDK verfügbar.|

### <a name="human-review-tool"></a>Tool für die Überprüfung durch Personen

Der Content Moderator-Dienst enthält auch das webbasierte [Tool für die Überprüfung durch Personen](Review-Tool-User-Guide/human-in-the-loop.md). 

![Startseite des Content Moderator-Tools für die Überprüfung durch Personen](images/homepage.PNG)

Mithilfe der Review-APIs können Sie Teamüberprüfungen für Text-, Bild- und Videoinhalte auf der Grundlage von selbst angegebenen Filtern einrichten. Die endgültigen Moderationsentscheidungen können dann von menschlichen Moderatoren getroffen werden. Der Dienst wird durch die menschlichen Eingaben nicht trainiert. Die Zusammenarbeit zwischen Dienst und menschlichen Überprüfungsteams ermöglicht es Entwicklern jedoch, ein ausgewogenes Verhältnis zwischen Effizienz und Genauigkeit zu erzielen.

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit
Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Content Moderator-Dienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Anweisungen zu den ersten Schritten mit dem Content Moderator-Dienst finden Sie unter [Schnellstart: Erste Schritte mit Content Moderator](quick-start.md).
