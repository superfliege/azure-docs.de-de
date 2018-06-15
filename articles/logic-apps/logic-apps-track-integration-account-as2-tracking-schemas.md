---
title: AS2-Nachverfolgungsschemas für die B2B-Überwachung – Azure Logic Apps | Microsoft-Dokumentation
description: Verwenden Sie AS2-Nachverfolgungsschemas, um B2B-Nachrichten von Transaktionen in Ihrem Azure-Integrationskonto zu überwachen.
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 48e39fd20716e962c4a3e367fdff18e0b4fba32d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300880"
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-to-monitor-success-errors-and-message-properties"></a>Starten oder Aktivieren der Nachverfolgung von AS2-Nachrichten und MDNs für die Überwachung von Erfolgs-, Fehler- und Nachrichteneigenschaften
Sie können diese AS2-Nachverfolgungsschemas in Ihrem Azure-Integrationskonto als Unterstützung bei der Überwachung von B2B-Transaktionen (Business-to-Buiness) verwenden:

* AS2-Nachrichten-Nachverfolgungsschema
* AS2-MDN-Nachverfolgungsschema

## <a name="as2-message-tracking-schema"></a>AS2-Nachrichten-Nachverfolgungsschema
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Eigenschaft | Typ | BESCHREIBUNG |
| --- | --- | --- |
| senderPartnerName | Zeichenfolge | Name des Absenderpartners der AS2-Nachricht (Optional) |
| receiverPartnerName | Zeichenfolge | Name des Empfängerpartners der AS2-Nachricht (Optional) |
| as2To | Zeichenfolge | Name des Empfängers der AS2-Nachricht aus den Headern der AS2-Nachricht (Erforderlich) |
| as2From | Zeichenfolge | Name des Absenders der AS2-Nachricht aus den Headern der AS2-Nachricht (Erforderlich) |
| agreementName | Zeichenfolge | Name der AS2-Vereinbarung, nach der Nachrichten aufgelöst werden (Optional) |
| direction | Zeichenfolge | Richtung des Nachrichtenflusses (Empfangen oder Senden) (Erforderlich) |
| messageId | Zeichenfolge | ID der AS2-Nachricht aus den Headern der AS2-Nachricht (Optional) |
| dispositionType |Zeichenfolge | MDN-Dispositionstypwert (Message Disposition Notification) (Optional) |
| fileName | Zeichenfolge | Dateiname aus dem Header der AS2-Nachricht (Optional) |
| isMessageFailed |Boolescher Wert | Gibt an, ob die AS2-Nachricht fehlgeschlagen ist. (Erforderlich) |
| isMessageSigned | Boolescher Wert | Gibt an, ob die AS2-Nachricht signiert wurde. (Erforderlich) |
| isMessageEncrypted | Boolescher Wert | Gibt an, ob die AS2-Nachricht verschlüsselt wurde. (Erforderlich) |
| isMessageCompressed |Boolescher Wert | Gibt an, ob die AS2-Nachricht komprimiert wurde. (Erforderlich) |
| correlationMessageId | Zeichenfolge | ID der AS2-Nachricht zum Korrelieren von Nachrichten mit MDNs (Optional) |
| incomingHeaders |Wörterbuch von JToken | Headerdetails von eingehenden AS2-Nachrichten (Optional) |
| outgoingHeaders |Wörterbuch von JToken | Headerdetails von ausgehenden AS2-Nachrichten (Optional) |
| isNrrEnabled | Boolescher Wert | Verwenden Sie den Standardwert, wenn der Wert unbekannt ist. (Erforderlich) |
| isMdnExpected | Boolean | Verwenden Sie den Standardwert, wenn der Wert unbekannt ist. (Erforderlich) |
| mdnType | Enum | Zulässige Werte sind **NotConfigured**, **Sync** und **Async**. (Erforderlich) |

## <a name="as2-mdn-tracking-schema"></a>AS2-MDN-Nachverfolgungsschema
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Eigenschaft | Typ | BESCHREIBUNG |
| --- | --- | --- |
| senderPartnerName | Zeichenfolge | Name des Absenderpartners der AS2-Nachricht (Optional) |
| receiverPartnerName | Zeichenfolge | Name des Empfängerpartners der AS2-Nachricht (Optional) |
| as2To | Zeichenfolge | Name des Partners, der die AS2-Nachricht erhält (Erforderlich) |
| as2From | Zeichenfolge | Name des Partners, der die AS2-Nachricht sendet (Erforderlich) |
| agreementName | Zeichenfolge | Name der AS2-Vereinbarung, nach der Nachrichten aufgelöst werden (Optional) |
| direction |Zeichenfolge | Richtung des Nachrichtenflusses (Empfangen oder Senden) (Erforderlich) |
| messageId | Zeichenfolge | ID der AS2-Nachricht (Optional) |
| originalMessageId |Zeichenfolge | ID der ursprünglichen AS2-Nachricht (Optional) |
| dispositionType | Zeichenfolge | MDN-Dispositionstypwert (Optional) |
| isMessageFailed |Boolescher Wert | Gibt an, ob die AS2-Nachricht fehlgeschlagen ist. (Erforderlich) |
| isMessageSigned |Boolescher Wert | Gibt an, ob die AS2-Nachricht signiert wurde. (Erforderlich) |
| isNrrEnabled | Boolescher Wert | Verwenden Sie den Standardwert, wenn der Wert unbekannt ist. (Erforderlich) |
| statusCode | Enum | Zulässige Werte sind **Accepted**, **Rejected** und **AcceptedWithErrors**. (Erforderlich) |
| micVerificationStatus | Enum | Zulässige Werte sind **NotApplicable**, **Succeeded** und **Failed**. (Erforderlich) |
| correlationMessageId | Zeichenfolge | Korrelations-ID. Die ursprüngliche Nachrichten-ID (die Nachrichten-ID der Nachricht, für die MDN konfiguriert ist). (Optional) |
| incomingHeaders | Wörterbuch von JToken | Gibt Headerdetails von eingehenden Nachrichten an. (Optional) |
| outgoingHeaders |Wörterbuch von JToken | Gibt Headerdetails von ausgehenden Nachrichten an. (Optional) |

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)    
* Informieren Sie sich über das [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md).   
* Informieren Sie sich über das [benutzerdefinierte B2B-Nachverfolgungsschema](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Informieren Sie sich über das [X12-Nachverfolgungsschema](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Informieren Sie sich über das [Nachverfolgen von B2B-Nachrichten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
