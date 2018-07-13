---
title: LUIS-Regionen (Language Understanding Intelligent Service) | Microsoft-Dokumentation
titleSuffix: Azure
description: Dieser Artikel enthält Listen der LUIS-Regionen für die LUIS-Website, Azure-Abonnements und Regionen weltweit.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: d81fbc03689788066fb9275523a5e96647117c58
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346451"
---
# <a name="regions-and-keys"></a>Regionen und Schlüssel

Die Region, in der Sie Ihre LUIS-App veröffentlichen, entspricht der gewählten Region oder dem Standort, die bzw. den Sie im Azure-Portal angeben, wenn Sie einen Azure-LUIS-Endpunktschlüssel erstellen. Wenn Sie [eine App veröffentlichen](./luis-how-to-publish-app.md), generiert LUIS automatisch eine Endpunkt-URL für die Region, die dem Schlüssel zugeordnet ist. Zum Veröffentlichen einer LUIS-App in mehreren Regionen benötigen Sie mindestens einen Schlüssel pro Region. 

## <a name="luis-website"></a>LUIS-Website
Es gibt basierend auf der Region drei LUIS-Websites. Sie müssen die Erstellung und Veröffentlichung in derselben Region durchführen. 

|LUIS|Region|
|--|--|
|[www.luis.ai][www.luis.ai]|Daten<br>nicht Europa<br>nicht Australien|
|[au.luis.ai][au.luis.ai]|Australien|
|[eu.luis.ai][eu.luis.ai]|Europa|


## <a name="publishing-regions"></a>Veröffentlichungsregionen

LUIS-Apps, die in https://www.luis.ai erstellt wurden, können auf allen Endpunkten außer in den Regionen in [Europa](#publishing-to-europe) und [Australien](#publishing-to-australia) veröffentlicht werden. 

Die App der Erstellungsregion kann nur in einer entsprechenden Veröffentlichungsregion veröffentlicht werden. Wenn sich die App derzeit in der falschen Erstellungsregion befindet, exportieren Sie die App und importieren sie in die richtige Erstellungsregion für Ihre Veröffentlichungsregion. 

 Globale Region | Erstellungsregion | Veröffentlichungs- und Abfrageregion   |   LUIS-Website | Format der Endpunkt-URL   |
|-----|------|------|------|------|
| Asien | USA (Westen)| Asien, Osten     | [www.luis.ai][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asien | USA (Westen)| Asien, Südosten     | [www.luis.ai][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Australien](#publishing-to-australia) | Australien (Osten)| Australien (Osten)     |   [au.luis.ai][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| Europa, Westen| Nordeuropa     | [eu.luis.ai][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | Europa, Westen| Europa, Westen     | [eu.luis.ai][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Nordamerika | USA (Westen) | USA (Ost)      |[www.luis.ai][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Nordamerika | USA (Westen) | USA (Ost) 2     | [www.luis.ai][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Nordamerika | USA (Westen) | USA Süd Mitte     | [www.luis.ai][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Nordamerika | USA (Westen) | USA, Westen-Mitte     |[www.luis.ai][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Nordamerika | USA (Westen) | USA (Westen) |  [www.luis.ai][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Nordamerika | USA (Westen) | USA, Westen 2    | [www.luis.ai][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Südamerika | USA (Westen) | Brasilien Süd     | [www.luis.ai][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Veröffentlichen in Europa

Zum Veröffentlichen in den europäischen Regionen erstellen Sie nur in https://eu.luis.ai LUIS-Apps. Wenn Sie es in einer anderen Region mit einem Schlüssel für die Region Europa versuchen, zeigt LUIS eine Warnmeldung an. Verwenden Sie stattdessen https://eu.luis.ai. LUIS-Apps, die in [https://eu.luis.ai][eu.luis.ai] erstellt wurden, werden nicht automatisch zu anderen Regionen migriert. Exportieren Sie die LUIS-App, und importieren Sie sie dann, um sie zu migrieren.

## <a name="publishing-to-australia"></a>Veröffentlichen in Australien

Zum Veröffentlichen in den australischen Regionen erstellen Sie nur in https://au.luis.ai LUIS-Apps. Wenn Sie es in einer anderen Region mit einem Schlüssel für die Region Australien versuchen, zeigt LUIS eine Warnmeldung an. Verwenden Sie stattdessen https://au.luis.ai. LUIS-Apps, die in [https://au.luis.ai][au.luis.ai] erstellt wurden, werden nicht automatisch zu anderen Regionen migriert. Exportieren Sie die LUIS-App, und importieren Sie sie dann, um sie zu migrieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vordefinierter Entitätsverweis](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai