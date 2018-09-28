---
title: Abrufen von Bildern aus der benutzerdefinierten Ansicht | Microsoft-Dokumentation
titleSuffix: Cognitive Services
description: Hier finden Sie eine allgemeine Übersicht über das Abrufen von Bildern aus Ihrer benutzerdefinierten Ansicht im Web mithilfe der benutzerdefinierten Bing-Suche.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/10/2018
ms.author: scottwhi
ms.openlocfilehash: ba836bbafaf67238664862ee2afce7840a573e44
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953890"
---
# <a name="get-images-from-your-custom-view"></a>Abrufen von Bildern aus der benutzerdefinierten Ansicht

Mit der benutzerdefinierten Bing-Bildersuche stehen Ihnen erweiterte Funktionen für die benutzerdefinierte Suche zur Verfügung. Ähnlich wie bei Webergebnissen wird bei der benutzerdefinierten Suche das Suchen nach Bildern in der Websiteliste in Ihrer Instanz ermöglicht. Sie können die Bilder mithilfe der Bing-API für die benutzerdefinierte Bildersuche oder über das Feature für die gehostete Benutzeroberfläche abrufen. Das Feature für die gehostete Benutzeroberfläche ist benutzerfreundlich und wird für die schnelle Einrichtung Ihrer Suchumgebung empfohlen.  Informationen zum Konfigurieren der gehosteten Benutzeroberfläche zum Berücksichtigen von Bilder finden Sie unter [Konfigurieren der gehosteten Benutzeroberfläche](hosted-ui.md).

Wenn Sie die Anzeige von Suchergebnissen präziser steuern möchten, können Sie die Bing-API für die benutzerdefinierte Bildersuche verwenden. Die Vorgehensweise zum Aufrufen der API ähnelt dem Aufrufen der Bing-Bildersuche-API. Sehen Sie sich daher die Beispiele zum Aufrufen der API unter [Was ist die Bing-Bildersuche?](../Bing-Image-Search/overview.md) an. Machen Sie sich zuvor jedoch mit dem Inhalt in der Referenz zur [API für die benutzerdefinierte Bildersuche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference) vertraut. Sie unterscheiden sich hauptsächlich in Bezug auf die unterstützten Abfrageparameter (Abfrageparameter „customConfig“ ist erforderlich) und den Endpunkt, an den Sie Anforderungen senden.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->