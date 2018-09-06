---
title: Konformitätsüberwachung für Azure Security Center mithilfe der REST-API | Microsoft-Dokumentation
description: Überprüfen Sie Ergebnisse von automatisierten Konformitätsprüfungen mithilfe der REST-API von Azure Security Center.
services: security-center
documentationcenter: na
author: rloutlaw
manager: angerobe
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rloutlaw
ms.openlocfilehash: 651d7737258f1b1b17c8392a09882388ddf95635
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42819636"
---
# <a name="review-security-center-compliance-results-using-the-azure-rest-apis"></a>Überprüfen von Security Center-Konformitätsergebnissen mithilfe der Azure-REST-APIs

In diesem Artikel erfahren Sie, wie Sie die Informationen zur Einhaltung der Sicherheitsvorschriften für eine Liste von Abonnements mit Hilfe der Azure REST-APIs abrufen können.

## <a name="review-compliance-for-each-subscription"></a>Überprüfen der Konformität für die einzelnen Abonnements

Das folgende Beispiel ruft Sicherheitsbewertungsinformationen für eine bestimmte Konformität und ein bestimmtes Abonnement unter Verwendung des Vorgangs [Konformitäten abrufen](/rest/api/securitycenter/compliances/get) ab.

```http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/compliances/{complianceName}?api-version=2017-08-01-preview
```

Der Parameter `{complianceName}` ist erforderlich und muss den Namen der bewerteten Konformität für `{subscription-id}` enthalten. Die Ausgabe enthält die Bewertungsergebnisse, z.B.:

```json
{
...
  "properties": {
    "assessmentResult": [
      {
        "segmentType": "Compliant",
        "percentage": 77.777777777777786
      }
    ],
    "resourceCount": 18,
    "assessmentTimestampUtcDate": "2018-01-01T00:00:00Z"
  }
}
```

## <a name="review-compliance-for-multiple-subscriptions"></a>Überprüfen der Konformität für mehrere Abonnements

Um Daten für mehrere Abonnements abzurufen, führen Sie den untenstehenden Aufruf aus, um zuerst eine Liste der Abonnements mit der Operation [Abonnements auflisten](/rest/api/resources/subscriptions/list) abzurufen. Damit wird [Konformitäten abrufen](/rest/api/securitycenter/compliances/get) für jede zurückgegebene Abonnement-ID aufgerufen.

Der API-Aufruf lautet:

```http
GET https://management.azure.com/subscriptions?api-version=2016-06-01
```

Damit wird eine Reihe von Werten zurückgegeben, wie z.B.: Verwenden Sie die subscriptionId-Werte im obigen Aufruf, um die Konformitätsinformationen für alle Abonnements zu überprüfen.

```json
"value": [
    {
      "id": "/subscriptions/{subscription-id}",
      "subscriptionId": "{subscription-id}",
      "displayName": "Demo subscription",
      ...
    }
```






