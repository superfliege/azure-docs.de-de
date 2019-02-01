---
title: Moderieren von Text mit der Textmoderations-API – Content Moderator
titlesuffix: Azure Cognitive Services
description: Testen Sie die Textmoderation mit der Textmoderations-API in der Onlinekonsole.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 926c291c3f81214c543e7c9f8ca36285e4e59400
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226586"
---
# <a name="moderate-text-from-the-api-console"></a>Moderieren von Text über die API-Konsole

Verwenden Sie die [Textmoderations-API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) in Azure Content Moderator, um Ihren Textinhalt zu scannen. Bei diesem Vorgang werden Ihre Inhalte auf unerwünschte Ausdrücke überprüft und mit dem Inhalt benutzerdefinierter und gemeinsam genutzter Blacklists verglichen.


## <a name="get-your-api-key"></a>Abrufen Ihres API-Schlüssels
Bevor Sie die API in der Onlinekonsole testen können, benötigen Sie Ihren Abonnementschlüssel. Dieser befindet sich auf der Registerkarte **Einstellungen** im Feld **Ocp-Apim-Subscription-Key**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

## <a name="navigate-to-the-api-reference"></a>Navigieren zur API-Referenz
Greifen Sie auf die [Referenz zur Textmoderations-API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) zu. 

  Die Seite **Text – Screen** (Text – Überprüfen) wird geöffnet.

## <a name="open-the-api-console"></a>Öffnen der API-Konsole
Wählen Sie für **Open API testing console** (API-Testkonsole öffnen) die Region aus, die Ihrem Standort am ehesten nahekommt. 

  ![Seite „Text – Screen“ (Text – Überprüfen) – Auswahl der Region](images/test-drive-region.png)

  Die API-Konsole **Text – Screen** (Text – Überprüfen) wird geöffnet.

## <a name="select-the-inputs"></a>Auswählen der Eingaben

### <a name="parameters"></a>Parameter
Wählen Sie die Abfrageparameter aus, die Sie für Ihre Textüberprüfung verwenden möchten. Verwenden Sie beispielsweise den Standardwert für **language**. Sie können das Feld auch leer lassen, da der Vorgang die zutreffende Sprache im Rahmen der Ausführung automatisch erkennt.

> [!NOTE]
> Weisen Sie für den Parameter **language** entweder `eng` zu, oder lassen Sie ihn leer, um die computergestützte **Klassifizierungsantwort** (Vorschaufeature) zu erhalten. **Dieses Feature wird nur für Englisch unterstützt**.
>
> Verwenden Sie für die Erkennung **anstößiger Begriffe** den [ISO 639-3-Code](http://www-01.sil.org/iso639-3/codes.asp) der unterstützten Sprachen aus diesem Artikel, oder lassen Sie ihn leer.

Wählen Sie für **autocorrect**, **PII** und **classify (preview)** die Option **true**. Lassen Sie das Feld **ListId** leer.

  ![Konsole „Text – Screen“ (Text – Überprüfen) – Abfrageparameter](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>Content-Typ
Wählen Sie für **Content-Type** den Inhaltstyp aus, den Sie überprüfen möchten. Verwenden Sie für dieses Beispiel den Standardinhaltstyp **text/plain**. Geben Sie in das Feld **Ocp-Apim-Subscription-Key** Ihren Abonnementschlüssel ein.

### <a name="sample-text-to-scan"></a>Beispieltext für Scan
Geben Sie im Feld **Anforderungstext** Text ein. Das folgende Beispiel enthält im Text einen absichtlichen Rechtschreibfehler.

> [!NOTE]
> Die ungültige US-Sozialversicherungsnummer im folgenden Beispieltext ist beabsichtigt. Hier soll das Format der Beispieleingabe und -ausgabe vermittelt werden.

```
    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
    Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="text-classification-feature"></a>Feature für Textklassifizierung

Im folgenden Beispiel ist die computergestützte Antwort zur Textklassifizierung von Content Moderator dargestellt. So können Sie Inhalte erkennen, die ggf. unerwünscht sind. Die gekennzeichneten Inhalte sind je nach Kontext unter Umständen ungeeignet. Neben der Vermittlung der Wahrscheinlichkeit einer Kategorie empfiehlt das Feature ggf. auch eine menschliche Überprüfung der Inhalte. Für das Feature wird ein trainiertes Modell verwendet, um Äußerungen zu identifizieren, die unter Umständen beleidigend, abfällig oder diskriminierend sind. Hierzu gehören auch Jargon und Abkürzungen sowie anstößige und absichtlich falsch geschriebene Wörter, die überprüft werden. 

#### <a name="explanation"></a>Erklärung

- `Category1` gibt an, dass unter Umständen Sprache vorhanden ist, die in bestimmten Situationen als sexuell freizügig oder nicht jugendfrei betrachtet werden kann.
- `Category2` gibt an, dass unter Umständen Sprache vorhanden ist, die in bestimmten Situationen als zweideutig bzw. anzüglich oder als nur für Erwachsene bestimmt betrachtet werden kann.
- `Category3` gibt an, dass unter Umständen Sprache vorhanden ist, die in bestimmten Situationen als anstößig betrachtet werden kann.
- `Score` ist ein Wert zwischen 0 und 1. Je höher der Wert, desto höher die vom Modell ermittelte Wahrscheinlichkeit, dass die Kategorie zutreffend ist. Diese Vorschauversion basiert nicht auf manuell programmierten Ergebnissen, sondern auf einem statistischen Modell. Es wird empfohlen, anhand Ihrer eigenen Inhalte zu testen, wie die jeweiligen Kategorien zu Ihren Anforderungen passen.
- `ReviewRecommended` ist entweder „true“ oder „false“ (abhängig von den internen Ergebnisschwellenwerten). Kunden müssen entscheiden, ob sie diesen Wert verwenden oder auf der Grundlage ihrer Inhaltsrichtlinien eigene Schwellenwerte festlegen möchten.

### <a name="analyze-the-response"></a>Analysieren der Antwort
In der folgenden Antwort sind die verschiedenen Erkenntnisse dargestellt, die über die API gewonnen werden können. Diese umfassen potenzielle Obszönitäten, personenbezogene Informationen, Klassifizierungen (Vorschauversion) und die automatisch korrigierte Version.

> [!NOTE]
> Das computergestützte Feature „Klassifizierung“ befindet sich in der Vorschauphase und unterstützt nur Englisch.

```
{
    "OriginalText": "Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
    "NormalizedText": "Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation": null,
    "PII": {
            "Email": [{
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 32
                }],
            "IPA": [{
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 72
                }],
            "Phone": [{
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 56
                }, {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                }, {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                }, {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                }, {
                "CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
            }],
         "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                "Index": 89
            }],
            "SSN": [{
                "Text": "999999999",
                "Index": 56
            }, {
                "Text": "999-99-9999",
                "Index": 266
            }]
        },
    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
        },
        "Category2": {
            "Score": 0.12747249007225037
        },
        "Category3": {
            "Score": 0.98799997568130493
        }
        },
    "Language": "eng",
    "Terms": [{
            "Index": 21,
            "OriginalIndex": 21,
            "ListId": 0,
         "Term": "crap"
        }],
    "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        },
     "TrackingId": "2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

Eine ausführliche Erklärung aller Abschnitte in der JSON-Antwort finden Sie in der [Übersicht zur Textmoderations-API](text-moderation-api.md).

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die REST-API in Ihrem Code, oder beginnen Sie mit dem [.NET-Schnellstart zur Textmoderation](text-moderation-quickstart-dotnet.md), um die Integration in Ihre Anwendung durchzuführen.
