---
title: Anpassen der Aussprache – Speech Services
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Aussprache mit dem Speech Service anpassen. Durch die benutzerdefinierte Aussprache können Sie die phonetische Form und Darstellung eines Worts oder einer Benennung definieren. Dies ist für die Verarbeitung angepasster Benennungen wie Produktnamen oder Akronymen hilfreich. Erforderlich ist zu Beginn lediglich eine Aussprachedatei – eine einfache TXT-Datei.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 0a3dfce10fc8ea76bc8f99e2459295bc637017dc
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878407"
---
# <a name="enable-custom-pronunciation"></a>Aktivieren der benutzerdefinierten Aussprache

Durch die benutzerdefinierte Aussprache können Sie die phonetische Form und Darstellung eines Worts oder einer Benennung definieren. Dies ist für die Verarbeitung angepasster Benennungen wie Produktnamen oder Akronymen hilfreich. Erforderlich ist zu Beginn lediglich eine Aussprachedatei – eine einfache TXT-Datei.

Dies funktioniert folgendermaßen: In einer einzelnen TXT-Datei können Sie mehrere Einträge für die benutzerdefinierte Aussprache eingeben. Die Struktur sieht wie folgt aus:

```
Display form <Tab> Spoken form <Newline>
```

Die folgende Tabelle enthält einige Beispiele:

| Darstellungsform | Gesprochene Form |
|----------|-------|
| C3PO | see three pea o |
| L8R | late are |
| CNTK | see n tea k|

## <a name="requirements-for-the-spoken-form"></a>Anforderungen für die gesprochene Form
Die gesprochene Form muss in Kleinbuchstaben geschrieben werden, was beim Import erzwungen werden kann. Darüber hinaus müssen Sie Überprüfungen im Datenimportprogramm bereitstellen. Es sind keine Tabstopps in der gesprochenen Form oder der Darstellungsform zulässig. Es kann jedoch weitere verbotene Zeichen in der Darstellungsform geben (z.B. ~ und ^).

Jede .txt-Datei kann mehrere Einträge aufweisen, wie in der folgenden Abbildung gezeigt:

![Beispiele für die Akronymaussprache](media/stt/custom-speech-pronunciation-file.png)

Die gesprochene Form ist die Lautfolge der Darstellungsform. Sie besteht aus Buchstaben, Wörtern oder Silben. Zurzeit sind keine weiteren Leitfäden oder Standards verfügbar, die bei der Formulierung der gesprochenen Form Orientierung bieten könnten.

## <a name="supported-pronunciation-characters"></a>Unterstützte Zeichen für die Aussprache
Die benutzerdefinierte Aussprache wird derzeit nur für US-amerikanisches Englisch (en-US) und Deutsch (de-DE) unterstützt. Der Zeichensatz, der zum Ausdruck der gesprochenen Form einer Benennung (in der benutzerdefinierten Aussprachedatei) verwendet werden kann, wird in der folgenden Tabelle aufgeführt:

| Sprache | Zeichen |
|---------- |----------|
| Englisch (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |
| Deutsch (de-de) | ä, ö, ü, ?, a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |

> [!NOTE]
> Die Darstellungsform einer Benennung (in einer Aussprachedatei) sollte genauso geschrieben werden wie ein Dataset für die Sprachadaption.

## <a name="requirements-for-the-display-form"></a>Anforderungen für die Darstellungsform
Eine Darstellungsform kann ausschließlich ein benutzerdefiniertes Wort, eine Benennung, ein Akronym oder ein Kompositum sein, bei dem existierende Wörter zusammengesetzt werden. Sie können auch alternative Aussprachen für häufig verwendete Wörter eingeben.

>[!NOTE]
>Es wird davon abgeraten, dieses Feature zur Umformulierung häufig verwendeter Wörter oder zur Änderung der gesprochenen Form zu verwenden. Falls besondere Wörter (z.B. Abkürzungen, Fachwörter und Fremdwörter) nicht korrekt decodiert werden, wird empfohlen, den Decoder auszuführen. Werden diese korrekt decodiert, fügen Sie sie der Datei für die benutzerdefinierte Aussprache hinzu. Im Sprachmodell sollten Sie stets und ausschließlich die Darstellungsform eines Worts verwenden.

## <a name="requirements-for-the-file-size"></a>Anforderungen für die Dateigröße
Die Größe der TXT-Datei, die die Einträge für die Aussprache enthält, ist auf 1 Megabyte (1 KB bei Schlüsseln in einem kostenlosen Tarif) beschränkt. In der Regel müssen keine großen Datenmengen über diese Datei hochgeladen werden. Die meisten benutzerdefinierten Aussprachedateien nehmen allenfalls nur wenige Kilobyte (KBs) in Anspruch. Als Codierung der TXT-Datei für alle Gebietsschemas sollte die UTF-8-BOM verwendet werden. Für das englische Gebietsschema kann auch der ANSI-Zeichensatz verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
* Verbessern Sie die Spracherkennung, indem Sie ein [benutzerdefiniertes Akustikmodell](how-to-customize-acoustic-models.md) erstellen.
* Verbessern Sie die Spracherkennung, indem Sie ein [benutzerdefiniertes Sprachmodell](how-to-customize-language-model.md) erstellen.
