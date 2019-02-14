---
title: Häufig gestellte Fragen zur Bing-Rechtschreibprüfungs-API
titlesuffix: Azure Cognitive Services
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Bing-Rechtschreibprüfungs-API in Azure.
services: cognitive-services
author: HeidiSteen
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 552e6fb3375c04e2f157842b13f72151e122f17b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866337"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Häufig gestellte Fragen zur Bing-Rechtschreibprüfungs-API

 Hier finden Sie Antworten auf häufig gestellte Fragen zu Konzepten, Code und Szenarien der Bing-Rechtschreibprüfungs-API für Microsoft Cognitive Services in Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Wie rufe ich beim Aufruf der Bing-Rechtschreibprüfungs-API über JavaScript die optionalen Clientheader ab?

Die folgenden Header sind optional, es wird jedoch empfohlen, sie als erforderlich zu betrachten. Mit diesen Headern kann die Bing-Rechtschreibprüfungs-API genauere Ergebnisse zurückgeben.

- X-Search-Location
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

Wenn Sie jedoch die Bing-Rechtschreibprüfungs-API über JavaScript aufrufen, verhindern die Sicherheitsfunktionen Ihres Browsers möglicherweise, dass Sie auf die Werte dieser Header zuzugreifen können.

Um dieses Problem zu lösen, können Sie die Anforderung der Bing-Rechtschreibprüfungs-API über einen CORS-Proxy senden. In der Antwort eines solchen Proxys befindet sich ein `Access-Control-Expose-Headers`-Header. Dieser enthält eine Whitelist mit Antwortheadern und stellt diese JavaScript zur Verfügung.

Die Installation eines CORS-Proxys, mit dem die [Tutorial-App](tutorials/spellcheck.md) auf die optionalen Clientheader zugreifen kann, ist schnell und unkompliziert. [Installieren Sie Node.js](https://nodejs.org/en/download/), falls Sie dies noch nicht getan haben. Geben Sie dann an einer Eingabeaufforderung den folgenden Befehl ein.

    npm install -g cors-proxy-server

Passen Sie den Endpunkt der Bing-Rechtschreibprüfungs-API in der HTML-Datei wie folgt an:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Starten Sie abschließend den CORS-Proxy mit folgendem Befehl:

    cors-proxy-server

Lassen Sie das Befehlsfenster während der Nutzung der Tutorial-App geöffnet. Wenn Sie das Fenster schließen, wird auch die Ausführung des Proxys beendet. Im Abschnitt mit den erweiterbaren HTTP-Headern unterhalb der Suchergebnisse wird nun u.a. der `X-MSEdge-ClientID`-Header angezeigt. Hier können Sie überprüfen, ob dieser für alle Anforderungen identisch ist.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine Frage zu einem fehlenden Feature bzw. einer fehlenden Funktion? Sie können gerne auf der [UserVoice-Website](https://cognitive.uservoice.com/) eine entsprechende Anforderung erstellen oder dafür abstimmen.

## <a name="see-also"></a>Weitere Informationen

 [StackOverflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
