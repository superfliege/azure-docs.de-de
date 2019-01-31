---
title: Häufig gestellte Fragen (FAQ) – Bing-Vorschlagssuche-API
titlesuffix: Azure Cognitive Services
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Bing-Vorschlagssuche-API.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: b0ec10bbf03e8a8d005eece4b6496b74b2943233
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173257"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Häufig gestellte Fragen (FAQ) zur Bing-Vorschlagssuche-API
 
 In diesem Artikel finden Sie Antworten zu häufig gestellten Fragen zu Konzepten, Code und Szenarios der Vorschlagssuche-API für Azure Cognitive Services.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Wie rufe ich beim Aufruf der Bing-Vorschlagssuche-API über JavaScript die optionalen Clientheader ab?

Die folgenden Header sind optional, es wird jedoch empfohlen, sie als erforderlich zu betrachten. Mit diesen Headern kann die Bing-Vorschlagssuche-API genauere Ergebnisse zurückgeben.

- X-Search-Location
- X-MSEdge-ClientID
- X-MSEdge-ClientID

Wenn Sie jedoch die Bing-Vorschlagssuche-API über JavaScript aufrufen, verhindern die integrierten Sicherheitsfunktionen Ihres Browsers möglicherweise, dass Sie auf die Werte dieser Header zugreifen können.

Um dieses Problem zu lösen, können Sie die Anforderung der Bing-Vorschlagssuche-API über einen CORS-Proxy senden. In der Antwort eines solchen Proxys befindet sich ein `Access-Control-Expose-Headers`-Header. Dieser enthält eine Whitelist mit Antwortheadern und stellt diese JavaScript zur Verfügung.

Die Installation eines CORS-Proxys, mit dem die [Tutorial-App](tutorials/autosuggest.md) auf die optionalen Clientheader zugreifen kann, ist schnell und unkompliziert. [Installieren Sie Node.js](https://nodejs.org/en/download/), falls Sie dies noch nicht getan haben. Geben Sie dann an einer Eingabeaufforderung den folgenden Befehl ein.

    npm install -g cors-proxy-server

Passen Sie den Endpunkt der Bing-Vorschlagssuche-API in der HTML-Datei wie folgt an:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Starten Sie abschließend den CORS-Proxy mit folgendem Befehl:

    cors-proxy-server

Lassen Sie das Fenster während der Nutzung der Tutorial-App geöffnet. Wenn Sie das Fenster schließen, wird auch die Ausführung des Proxys beendet. Im Bereich mit den erweiterbaren HTTP-Headern unter den Suchergebnissen wird nun u.a. der `X-MSEdge-ClientID`-Header angezeigt. Hier können Sie überprüfen, ob dieser für alle Anforderungen identisch ist.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine Frage zu einem fehlenden Feature bzw. einer fehlenden Funktion? Sie können gerne auf der [UserVoice-Website](https://cognitive.uservoice.com/) eine entsprechende Anforderung erstellen oder dafür abstimmen.

## <a name="see-also"></a>Weitere Informationen

- [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
