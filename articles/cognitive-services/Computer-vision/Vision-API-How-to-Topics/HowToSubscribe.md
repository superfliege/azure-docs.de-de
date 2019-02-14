---
title: Abrufen von Abonnementschlüsseln – maschinelles Sehen
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Abonnementschlüssel für Aufrufe der Maschinelles Sehen-API in Azure Cognitive Services abrufen können.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 08838ce0af16cc4ae768bd5d2ecf72c57f8fae97
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858075"
---
# <a name="how-to-obtain-subscription-keys"></a>Gewusst wie: Erhalten von Abonnementschlüsseln

Dienste für maschinelles Sehen erfordern spezielle Abonnementschlüssel. Für jeden Aufruf der Maschinelles Sehen-API ist ein Abonnementschlüssel erforderlich. Dieser Schlüssel muss entweder über einen Abfragezeichenfolgen-Parameter übergeben oder im Anforderungsheader angegeben werden.

Informationen zur Registrierung für Abonnementschlüssel finden Sie unter [Abonnements](https://azure.microsoft.com/try/cognitive-services/). Die Registrierung ist kostenlos. Die Preise für diese Dienste unterliegen Änderungen.

>[!NOTE]
Ihre Abonnementschlüssel sind nur für eine der folgenden [Microsoft Azure-Regionen](https://azure.microsoft.com/regions/) gültig. 

| Region | Adresse |
|---|---|
| USA (Westen) | westus.api.cognitive.microsoft.com |
| USA (Ost) 2 | eastus2.api.cognitive.microsoft.com |
| USA, Westen-Mitte | westcentralus.api.cognitive.microsoft.com |
| Europa, Westen | westeurope.api.cognitive.microsoft.com |
| Asien, Südosten | southeastasia.api.cognitive.microsoft.com |

Wenn Sie sich mit der kostenlosen Testversion für maschinelles Sehen anmelden, gelten Ihre Abonnementschlüssel für die Region **USA, Westen-Mitte** (`https://westcentralus.api.cognitive.microsoft.com/`). Das ist der häufigste Fall. Wenn Sie sich jedoch mit Ihrem Microsoft Azure-Konto über die Website [https://azure.microsoft.com/](https://azure.microsoft.com/) für maschinelles Sehen anmelden, geben Sie die Region für Ihre Testversion aus der obigen Liste der Regionen an.

Wenn Sie sich z. B. mit Ihrem Microsoft Azure-Konto für maschinelles Sehen registrieren und `westus` für Ihre Region angeben, müssen Sie die Region `westus` für Ihre REST-API-Aufrufe verwenden (`https://westus.api.cognitive.microsoft.com/`).

Wenn Sie die Region für Ihren Abonnementschlüssel nach Erhalt Ihres Testschlüssels vergessen haben, finden Sie Ihre Region unter [https://azure.microsoft.com/try/cognitive-services/my-apis/](https://azure.microsoft.com/try/cognitive-services/my-apis/).

### <a name="related-links"></a>Verwandte Links:

* [Preisoptionen für Azure Cognitive Services-APIs](https://azure.microsoft.com/pricing/details/cognitive-services/)
