---
title: Benutzerdefinierte Nachverfolgungsschemas für die B2B-Überwachung – Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen Sie benutzerdefinierte Nachverfolgungsschemas, um B2B-Nachrichten von Transaktionen in Ihrem Azure-Integrationskonto zu überwachen.
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 94de6afcf3f9f95a3cb45207ef43e1d33b728d99
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="enable-tracking-to-monitor-your-complete-workflow-end-to-end"></a>Aktivieren der Nachverfolgung zur vollständigen End-to-End-Überwachung von Workflows
Es gibt eine integrierte Nachverfolgung, die Sie für die verschiedenen Teile des B2B-Workflows aktivieren können, z.B. für die Überwachung von AS2- oder X12-Nachrichten. Wenn Sie Workflows erstellen, die eine Logik-App, BizTalk Server, SQL Server oder andere Ebenen enthalten, dann können Sie die benutzerdefinierte Nachverfolgung aktivieren, bei der Ereignisse vom Anfang bis zum Ende des Workflows protokolliert werden. 

Dieses Thema enthält benutzerdefinierten Code, den Sie in den Ebenen außerhalb Ihrer Logik-App verwenden können. 

## <a name="custom-tracking-schema"></a>Benutzerdefiniertes Nachverfolgungsschema
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Eigenschaft | Typ | BESCHREIBUNG |
| --- | --- | --- |
| sourceType |   | Typ der Ausführungsquelle Zulässige Werte sind **Microsoft.Logic/workflows** und **custom** (benutzerdefiniert). (Erforderlich) |
| Quelle |   | Wenn der Quelltyp **Microsoft.Logic/workflows** ist, müssen die Quellinformationen diesem Schema folgen. Wenn der Quelltyp **custom** ist, ist das Schema ist ein JToken. (Erforderlich) |
| systemId | Zeichenfolge | System-ID der Logik-App (Erforderlich) |
| runId | Zeichenfolge | Ausführungs-ID der Logik-App (Erforderlich) |
| operationName | Zeichenfolge | Name des Vorgangs (z.B. Aktion oder Trigger) (Erforderlich) |
| repeatItemScopeName | Zeichenfolge | Elementnamen wiederholen, wenn sich die Aktion innerhalb einer `foreach`/`until`-Schleife befindet. (Erforderlich) |
| repeatItemIndex | Ganze Zahl  | Gibt an, ob sich die Aktion innerhalb einer `foreach`/`until`-Schleife befindet. Gibt den Index des Wiederholungselements an. (Erforderlich) |
| trackingId | Zeichenfolge | Überwachungs-ID zum Korrelieren der Nachrichten (Optional) |
| correlationId | Zeichenfolge | Korrelations-ID zum Korrelieren der Nachrichten (Optional) |
| clientRequestId | Zeichenfolge | Kann vom Client zum Korrelieren von Nachrichten ausgefüllt werden. (Optional) |
| eventLevel |   | Ebene des Ereignisses. (Erforderlich) |
| eventTime |   | Zeit des Ereignisses im UTC-Format YYYY-MM-DDTHH:MM:SS.00000Z. (Erforderlich) |
| recordType |   | Typ des Nachverfolgungsdatensatzes Der zulässige Wert ist **custom**. (Erforderlich) |
| record |   | Benutzerdefinierter Datensatztyp Das zulässige Format ist JToken. (Erforderlich) |

## <a name="b2b-protocol-tracking-schemas"></a>B2B-Protokollnachverfolgungsschemas
Informationen zu B2B-Protokollnachverfolgungsschemas finden Sie unter:
* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12-Nachverfolgungsschemas](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über das [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md).   
* Informieren Sie sich über das [Nachverfolgen von B2B-Nachrichten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Weitere Informationen zum [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
