---
title: Was ist Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Content Moderator verwenden, um nicht angemessene Inhalte in von Benutzern generiertem Material nachzuverfolgen, zu kennzeichnen, zu bewerten und zu filtern.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 10/05/2018
ms.author: sajagtap
ms.openlocfilehash: 5756e8fb451b073c68271359848ab27373ad85ed
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309551"
---
# <a name="what-is-content-moderator"></a>Was ist Content Moderator?

Inhaltsmoderation ist die Überwachung von Text-, Bild- oder Videoinhalten auf möglicherweise anstößiges, unerwünschtes oder riskantes Material. Gekennzeichneter Inhalt kann dann ausgeblendet oder anderweitig behandelt werden, um Vorgaben zu erfüllen oder den Benutzern eine bestimmte Umgebung zu bieten.

## <a name="where-it-is-used"></a>Einsatzgebiete

Die folgende Liste enthält einige Beispielszenarien für Content Moderator:

- Onlinemarktplätze, in denen Produktkataloge und von Benutzern generierte Inhalte moderiert werden
- Gamingunternehmen, die von Benutzern generierte Spielartefakte und Chatrooms moderieren
- Messagingplattformen sozialer Medien, auf denen von den Benutzern hinzugefügte Bilder, Texte und Videos moderiert werden
- Medienunternehmen, die eine zentrale Inhaltsmoderation für ihre Inhalte implementieren
- Anbieter im Bildungswesen, die unangemessene und anstößige Inhalte für Schüler und Lehrer herausfiltern

## <a name="what-it-includes"></a>Lieferumfang

Content Moderator besteht aus verschiedenen Webdienst-APIs und einem integrierten Tool für die Überprüfung durch Personen – die APIs und das Tool helfen beim Moderieren von Bildern, Texten und Videos.

![Content Moderator – Aufbau](images/content-moderator-block-diagram.png)

### <a name="apis"></a>APIs

Der Content Moderator-Dienst verfügt über folgende APIs:
  - [**Textmoderations-API**](text-moderation-api.md): Verwenden Sie diese API, um Texte nach möglichen obszönen, freizügigen, zweideutigen oder anstößigen Inhalten sowie nach personenbezogenen Informationen zu durchsuchen.
  - [**API für benutzerdefinierte Begriffslisten**](try-terms-list-api.md): Verwenden Sie diese API, um Texte nicht nur mit den integrierten Begriffen, sondern auch mit benutzerdefinierten Begriffslisten abzugleichen. Mit diesen Listen können Sie Inhalten gemäß Ihren Inhaltsrichtlinien blockieren oder zulassen.  
  - [**Bildmoderations-API**](image-moderation-api.md): Verwenden Sie diese API, um Bilder nach anstößigen und freizügigen Inhalten zu durchsuchen, mithilfe der optischen Zeichenerkennung Text in Bildern zu entdecken und Gesichter zu erkennen.
  - [**API für benutzerdefinierte Bildlisten**](try-image-list-api.md): Verwenden Sie diese API, um Bilder mit benutzerdefinierten Bildlisten abzugleichen – vordefinierte Inhalte, die Sie nicht erneut klassifizieren müssen.
  - [**Videomoderations-API**](video-moderation-api.md): Verwenden Sie diese API, um Videos nach möglichen anstößigen und freizügigen Inhalten zu durchsuchen.
  - [**Überprüfungs-APIs**](try-review-api-job.md): Verwenden Sie die Vorgänge [Jobs](try-review-api-job.md), [Reviews](try-review-api-review.md) und [Workflow](try-review-api-workflow.md), um innerhalb des Überprüfungstools Workflows für die Überprüfung durch Personen zu erstellen und zu automatisieren.

### <a name="human-review-tool"></a>Tool für die Überprüfung durch Personen

Ihr Content Moderator-Abonnement umfasst das integrierte [Tool für die Überprüfung durch Personen](Review-Tool-User-Guide/human-in-the-loop.md). Verwenden Sie die bereits erwähnte Überprüfungs-API, um Überprüfungen von Texten, Bildern und Videos zu erstellen, damit Ihre menschlichen Moderatoren endgültige Entscheidungen treffen können.

![Content Moderator-Tool für die Überprüfung von Videos](images/video-review-default-view.png)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die [Schnellstartanleitung](quick-start.md), um mit Content Moderator loszulegen.
