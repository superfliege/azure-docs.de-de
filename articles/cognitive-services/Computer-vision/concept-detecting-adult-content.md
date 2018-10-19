---
title: Beschreiben nicht jugendfreier und anzüglicher Inhalte – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung nicht jugendfreier und anzüglicher Inhalte in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: b1ba8e7556b6ba134624548142bf73e84d875c6a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984521"
---
# <a name="detecting-adult-and-racy-content"></a>Erkennen von nicht jugendfreien und freizügigen Inhalten

Die verschiedenen visuellen Kategorien umfassen auch die Gruppe der nicht jugendfreien und anzüglichen Inhalte, die eine Erkennung solcher jugendgefährdenden Inhalte ermöglicht und die Anzeige von Bildern mit sexuellen Inhalten einschränkt. Der Filter zur Erkennung nicht jugendfreier und anzüglicher Inhalte kann durch einen Schieberegler entsprechend den Anforderungen des Benutzers eingestellt werden.

## <a name="defining-adult-and-racy-content"></a>Definieren von nicht jugendfreien und anzüglichen Inhalten

Unter den verschiedenen visuellen Merkmalen, die von der [Methode zum Analysieren von Bildern](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) abgedeckt werden, ermöglicht das visuelle Merkmal „Adult“ (Erwachsen) die Erkennung von nicht jugendfreien und anzüglichen Bildern. „Adult“ (Erwachsen) Bilder sind gemäß Definition pornografischer Natur und stellen oft Nacktheit und sexuelle Handlungen dar. „Racy“ (Anzüglich) Bilder sind gemäß Definition sexuell suggestiv und enthalten häufig in sexueller Hinsicht weniger explizite Inhalte als Bilder, die als „Adult“ (Erwachsen) gekennzeichnet sind. Der visuelle Merkmalstyp „Adult“ (Erwachsen) wird häufig verwendet, um die Anzeige von Bildern mit sexuell suggestiven und explizit sexuellen Inhalten einzuschränken.

## <a name="identifying-adult-and-racy-content"></a>Identifizieren von nicht jugendfreien und freizügigen Inhalten

Die Methode zum Analysieren von Bildern gibt zwei Eigenschaften, `isAdultContent` und `isRacyContent`, in der JSON-Antwort der Methode zurück, um nicht jugendfreie und anzügliche Inhalte zu identifizieren. Beide Eigenschaften geben einen booleschen Wert zurück, entweder „true“ oder „false“. Die Methode gibt auch zwei Eigenschaften zurück, `adultScore` und `racyScore`, die jeweils die Zuverlässigkeitsbewertungen für die Identifizierung von nicht jugendfreien und anzüglichen Inhalten darstellen. Ein Zuverlässigkeitsfilter für anzügliche und nicht jugendfreie Inhalte kann mithilfe eines Schiebereglers nach Bedarf auf Basis Ihres bestimmten Szenarios angepasst werden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte zum [Erkennen domänenspezifischer Inhalte](concept-detecting-domain-content.md) und [Erkennen von Gesichtern](concept-detecting-faces.md).