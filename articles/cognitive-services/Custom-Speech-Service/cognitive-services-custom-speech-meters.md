---
title: Verbrauchseinheiten und Kontingente für Custom Speech Service in Azure | Microsoft-Dokumentation
description: Informationen zu Verbrauchseinheiten und Kontingenten für Custom Speech Service in Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: d2225dec818c600febfad2f9ebc42594f6ac09ac
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372858"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Verbrauchseinheiten und Kontingenten für Custom Speech Service

Mit dem cloudbasierten Custom Speech Service können Sie Sprachmodelle für die Sprache-in-Text-Transkription anpassen.

Wenn Sie Custom Speech Service verwenden möchten, rufen Sie das [Custom Speech Service-Portal](https://cris.ai) auf.

Die aktuellen Preise für die Verbrauchseinheiten finden Sie auf der Seite [Preise für den Custom Speech Service der Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/).

## <a name="tiers-explained"></a>Erläuterungen zu Tarifen
Für Tests und Prototypen empfehlen wir, nur den Tarif Free (F0) zu verwenden. Für Produktionssysteme schlagen wir den S2-Tarif vor. Durch die Verwendung des S2-Tarifs können Sie die Bereitstellung auf die Anzahl der Skalierungseinheiten (SUs) skalieren, die für Ihr Szenario erforderlich sind.

> [!NOTE]
> Eine Migration zwischen dem F0- und dem S2-Tarif ist *nicht* möglich.
>

## <a name="meters-explained"></a>Erläuterungen zu Verbrauchseinheiten

### <a name="scale-out"></a>Horizontales Skalieren
Dies ist eine neue Funktion, das wir mit dem neuen Preismodell veröffentlicht haben. Damit können Sie die Anzahl gleichzeitiger Anforderungen steuern, die das Modell verarbeiten kann.

Sie können gleichzeitige Anforderungen festlegen, indem Sie SU-Kennzahl in der Ansicht **Modellbereitstellung erstellen** verwenden. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Sprache-in-Text-Endpunkts](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Abhängig von der Datenverkehrsmenge, die das Modell verarbeiten soll, können Sie eine entsprechende Anzahl von SUs auswählen. 

> [!NOTE]
> Jede Skalierungseinheit garantiert 5 gleichzeitige Anforderungen. Bei Bedarf können Sie 1 oder mehr SUs erwerben. Da die Anzahl der SUs in 1er-Schritten steigt, wird die Anzahl der gleichzeitigen Anfragen garantiert in 5er-Schritten erhöht.
>

### <a name="log-management"></a>Protokollverwaltung
Sie können die Audioverfolgung für ein neu bereitgestelltes Modell gegen Aufpreis abschalten. Custom Speech Service protokolliert weder die Audioanforderungen noch die Transkripte von diesem Modell.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwenden von Custom Speech Service finden Sie im [Custom Speech Service-Portal](https://cris.ai).

* [Erste Schritte](cognitive-services-custom-speech-get-started.md)
* [HÄUFIG GESTELLTE FRAGEN](cognitive-services-custom-speech-faq.md)
* [Glossar](cognitive-services-custom-speech-glossary.md)
 