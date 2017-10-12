---
title: "Ressourcennutzungs-API für Anbieter | Microsoft-Dokumentation"
description: "Referenz für die Ressourcennutzungs-API, die Azure Stack-Nutzungsinformationen abruft"
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: b6055923-b6a6-45f0-8979-225b713150ae
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: alfredop
ms.openlocfilehash: c54dca9d734cf909cf20d5235a90b9b46f0af11c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="provider-resource-usage-api"></a>Ressourcennutzungs-API für Anbieter
Der Begriff „Anbieter“ bezieht sich auf den Dienstadministrator und auf alle delegierten Anbieter. Azure Stack-Betreiber und delegierte Anbieter können die Nutzungs-API für Anbieter verwenden, um einen Einblick in die Nutzung ihrer direkten Mandanten zu erhalten. A0 kann beispielsweise die Anbieter-API aufrufen, um die Nutzungsinformationen zur direkten Nutzung von A1 und A2 abzurufen, und A1 kann die Nutzungsinformationen von A3 und A4 abrufen.

![Konzeptuelles Modell der Anbieterhierarchie](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>API-Aufrufreferenz
### <a name="request"></a>Anforderung
Die Anforderung ruft detaillierte Nutzungsinformationen für das angeforderte Abonnement und den angeforderten Zeitraum ab. Es gibt keinen Anforderungstext.

Die Nutzungs-API ist eine Anbieter-API. Aus diesem Grund muss dem Aufrufer im Abonnement des Anbieters die Rolle des Besitzers, Mitwirkenden oder Lesers zugewiesen sein.

| **Methode** | **Anforderungs-URI** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumente
| **Argument** | **Beschreibung** |
| --- | --- |
| *armendpunkt* |Azure Resource Management-Endpunkt Ihrer Azure Stack-Umgebung. Nach Azure Stack-Konvention folgt der Name des Azure Resource Manager-Endpunkts dem Format `https://adminmanagement.{domain-name}`. Wenn der Domänenname für das Development Kit beispielsweise „local.azurestack.external“ lautet, ist der Resource Manager-Endpunkt `https://adminmanagement.local.azurestack.external`. |
| *aboId* |Abonnement-ID des Benutzers, von dem der Aufruf ausgeht |
| *gemeldeteStartzeit* |Die Startzeit der Abfrage. Der Wert für *DatumUhrzeit* sollte in UTC angegeben werden und in vollen Stunden, z.B. 13:00. Legen Sie den Wert für eine tägliche Aggregation auf Mitternacht (UTC) fest. Das Format ist ISO 8601 *mit Escapezeichen*, z.B. 2015-06-16T18%3a53%3a11%2b00%3a00Z, wobei ein Semikolon zu „%3a“ und ein Pluszeichen zu „%2b“ wird, um auch für URIs geeignet zu sein. |
| *gemeldeteEndzeit* |Die Endzeit der Abfrage. Für dieses Argument gelten die gleichen Einschränkungen wie für die *gemeldeteStartzeit*. Der Wert für *gemeldeteEndzeit* darf nicht in der Zukunft oder am aktuellen Tag liegen. Wenn dies der Fall ist, wird als Ergebnis „processing not complete“ (nicht abgeschlossen) angezeigt. |
| *aggregationsgranularität* |Optionaler Parameter, der zwei mögliche diskrete Werte hat: täglich und stündlich. Wie die Namen schon sagen, gibt der eine Wert Daten in Abständen von einem Tag zurück und der andere in Abständen von einer Stunde. Standardmäßig ist die tägliche Option aktiviert. |
| *abonnentenId* |Abonnement-ID Um gefilterte Daten zu erhalten, ist die Abonnement-ID eines direkten Mandanten des Anbieters erforderlich. Wenn kein Parameter für die Abonnement-ID angegeben ist, gibt der Aufruf Nutzungsdaten für alle direkten Mandanten des Anbieters zurück. |
| *api-version* |Die Version des Protokolls, dass für diese Anforderung verwendet wird. Dieser Wert ist auf „2015-06-01-preview“ festgelegt. |
| *fortsetzungsToken* |Das Token, das durch den letzten Aufruf des Anbieters der Nutzungs-API abgerufen wurde. Dieses Token ist erforderlich, wenn eine Antwort länger als 1000 Zeilen ist und als Textmarke für den Fortschritt fungiert. Falls nicht vorhanden, werden die Daten auf Grundlage der übergebenen Granularität vom Anfang des Tages oder der Stunde abgerufen. |

### <a name="response"></a>Antwort
GET /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00&reportedEndTime=2015-06-01T00%3a00%3a00%2b00%3a00&aggregationGranularity=Daily&subscriberId=sub1.1&api-version=1.0

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Genaue Angaben zu Antwort
| **Argument** | **Beschreibung** |
| --- | --- |
| *id* |Eindeutige ID des Nutzungsaggregats |
| *name* |Name des Nutzungsaggregats |
| *type* |Ressourcendefinition |
| *subscriptionId* |Bezeichner des Abonnements des Azure Stack-Benutzers |
| *usageStartTime* |Startzeit (UTC) des Nutzungsbuckets, zu dem dieses Nutzungsaggregat gehört |
| *usageEndTime* |Endzeit (UTC) des Nutzungsbuckets, zu dem dieses Nutzungsaggregat gehört |
| *instanceData* |Schlüssel-Wert-Paare der genaueren Angaben zu der Instanz (in neuem Format):<br> *resourceUri*: Vollqualifizierte Ressourcen-ID, die die Ressourcengruppen und den Instanznamen enthält <br> *location*: Region, in der der Dienst ausgeführt wurde <br> *tags*: Ressourcentags, die vom Benutzer angegeben werden <br> *additionalInfo*: Weitere Angaben zur genutzten Ressource, z.B. die Betriebssystemversion oder der Imagetyp |
| *quantity* |Menge der Ressourcennutzung, die in diesem Zeitraum aufgetreten ist |
| *meterId* |Eindeutige ID der Verbrauchseinheit: Eine eindeutige ID für die verwendete Ressource (auch als *ResourceID* bezeichnet) |

## <a name="next-steps"></a>Nächste Schritte
[Referenz zur Ressourcennutzungs-API für Mandanten](azure-stack-tenant-resource-usage-api.md)

[Häufig gestellte Fragen zur Nutzung](azure-stack-usage-related-faq.md)

