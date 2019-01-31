---
title: Verwenden der benutzerdefinierten Aussprache mit Custom Speech Service in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Custom Speech Service in Microsoft Cognitive Services ein Sprachmodell erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/23/2017
ms.author: panosper
ms.openlocfilehash: 0459fdb63134598e65657e5e26020c2ee0dc0efc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209790"
---
# <a name="enable-custom-pronunciation"></a>Aktivieren der benutzerdefinierten Aussprache

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Durch die benutzerdefinierte Aussprache können Benutzer die phonetische Form und Darstellung eines Worts oder einer Benennung definieren. Dies ist für die Verarbeitung angepasster Benennungen wie Produktnamen oder Akronymen hilfreich. Erforderlich ist lediglich eine Aussprachedatei (eine einfache TXT-Datei).

Dies funktioniert folgendermaßen: In einer einzelnen TXT-Datei können Sie mehrere Einträge für die benutzerdefinierte Aussprache eingeben. Die Struktur sieht wie folgt aus:

```
Display form <Tab> Spoken form <Newline>
```

*Beispiele*:

| Darstellungsform | Gesprochene Form |
|----------|-------|
| C3PO | see three pea o |
| L8R | late are |
| CNTK | see n tea k|

## <a name="requirements-for-the-spoken-form"></a>Anforderungen für die gesprochene Form
Die gesprochene Form muss in Kleinbuchstaben geschrieben werden, was beim Import erzwungen werden kann. Darüber hinaus müssen Sie diese im Datenimportprogramm überprüfen. Es sind keine Tabstopps in der gesprochenen Form oder der Darstellungsform zulässig. Darüber hinaus kann es jedoch weitere verbotene Zeichen in der Darstellungsform geben (z.B. ~ und ^).

Jede TXT-Datei kann mehrere Einträge enthalten. Dies wird beispielsweise im folgenden Screenshot gezeigt:

![Screenshot von Editor mit mehreren Einträgen für die Aussprache von Akronymen](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

Die gesprochene Form ist die Lautfolge der Darstellungsform. Sie besteht aus Buchstaben, Wörtern oder Silben. Derzeit sind keine weiteren Leitfäden oder Standards verfügbar, die bei der Formulierung der gesprochenen Form Orientierung bieten könnten. 

## <a name="supported-pronunciation-characters"></a>Unterstützte Zeichen für die Aussprache
Die benutzerdefinierte Aussprache wird derzeit nur für US-amerikanisches Englisch (en-US) und Deutsch (de-DE) unterstützt. Der Zeichensatz, der zum Ausdruck der gesprochenen Form einer Benennung (in der benutzerdefinierten Aussprachedatei) verwendet werden kann, wird in der folgenden Tabelle aufgeführt: 

| Sprache | Zeichen |
|---------- |----------|
| Englisch (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |
| Deutsch (de-de) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |

>[HINWEIS] Die Darstellungsform einer Benennung (in einer Aussprachedatei) sollte einem Dataset für die Sprachadaption entsprechen.

## <a name="requirements-for-the-display-form"></a>Anforderungen für die Darstellungsform
Eine Darstellungsform kann ausschließlich ein benutzerdefiniertes Wort, eine Benennung, ein Akronym oder ein Kompositum sein, bei dem existierende Wörter zusammengesetzt werden. Sie können auch alternative Aussprachen für häufig verwendete Wörter eingeben. 

>[!NOTE]
Es wird davon abgeraten, dieses Feature zur Umformulierung häufig verwendeter Wörter oder zur Änderung der gesprochenen Form zu verwenden. Falls besondere Wörter (z.B. Abkürzungen, Fachwörter und Fremdwörter) nicht korrekt decodiert werden, wird empfohlen, den Decoder auszuführen. Werden diese korrekt decodiert, fügen Sie sie der Datei für die benutzerdefinierte Aussprache hinzu. Im Sprachmodell sollten Sie stets und ausschließlich die Darstellungsform eines Worts verwenden. 

## <a name="requirements-for-the-file-size"></a>Anforderungen für die Dateigröße
Die Größe der TXT-Datei, die die Einträge für die Aussprache enthält, ist auf 1 MB beschränkt. In der Regel müssen keine großen Datenmengen über diese Datei hochgeladen werden. Die meisten benutzerdefinierten Aussprachedateien nehmen allenfalls nur wenige KBs in Anspruch. Als Codierung der TXT-Datei für alle Gebietsschemas sollte die UTF-8-BOM verwendet werden. Für das englische Gebietsschema kann auch der ANSI-Zeichensatz verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
* Verbessern Sie die Spracherkennung, indem Sie ein [benutzerdefiniertes Akustikmodell](cognitive-services-custom-speech-create-acoustic-model.md) erstellen.
* [Erstellen Sie einen benutzerdefinierten Endpunkt für die Spracherkennung](cognitive-services-custom-speech-create-endpoint.md), die Sie von einer App verwenden können.
