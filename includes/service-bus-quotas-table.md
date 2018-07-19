---
title: Includedatei
description: Includedatei
services: service-bus-messaging
author: sethmanheim
ms.service: service-bus-messaging
ms.topic: include
ms.date: 06/04/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: 0ff1e31e52c7db5c41f92cb9e4cb1a17f28dea6f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755865"
---
In der folgenden Tabelle sind die Kontingentinformationen für das Service Bus Messaging aufgeführt. Informationen zu Preisen und anderen Kontingenten für Service Bus finden Sie in der Übersicht der [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/) .

| Namen des Kontingents | Bereich | Notizen | Wert |
| --- | --- | --- | --- | --- |
| Maximale Anzahl von Basic-/Standard-Namespaces pro Azure-Abonnement |Namespace |Weitere Anforderungen für zusätzliche Basic-/Standard-Namespaces werden vom Portal abgelehnt. |100|
| Maximale Anzahl von Premium-Namespaces pro Azure-Abonnement |Namespace |Weitere Anforderungen für zusätzliche Premium-Namespaces werden vom Portal abgelehnt. |10 |
| Warteschlangen-/Themengröße |Entität |Bei der Erstellung der Warteschlange/des Themas definiert. <br/><br/> Nachfolgende eingehende Nachrichten werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |1, 2, 3, 4 oder 5 GB.<br /><br />In der Premium-SKU, aber wenn in der Standard-SKU [Partitionierung](/azure/service-bus-messaging/service-bus-partitioning) aktiviert ist, beträgt die maximale Größe von Warteschlangen bzw. Themen 80 GB. |
| Maximale Anzahl gleichzeitiger Verbindungen in einem Namespace |Namespace |Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. REST-Vorgänge werden nicht zu den gleichzeitigen TCP-Verbindungen hinzugezählt. |NetMessaging: 1.000<br /><br />AMQP: 5.000 |
| Anzahl der gleichzeitigen Empfangsanforderungen für eine Warteschlangen-/Themen-/Abonnement-Entität |Entität |Nachfolgende Empfangsanforderungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. Dieses Kontingent gilt für alle gleichzeitigen Empfangsvorgänge über alle Abonnements eines Themas hinweg. |5.000 |
| Anzahl von Themen/Warteschlangen pro Namespace |Namespace |Nachfolgende Anforderungen zum Erstellen eines neuen Themas bzw. einer neuen Warteschlange im Namespace werden abgelehnt. Als Ergebnis wird bei der Konfiguration über das [Azure-Portal][Azure portal] eine Fehlermeldung generiert. Bei einem Aufruf über die Verwaltungs-API wird vom aufrufenden Code eine Ausnahme empfangen. |10.000<br /><br />Die Gesamtzahl von Themen plus Warteschlangen in einem Namespace muss kleiner oder gleich 10.000 sein. |
| Anzahl von [partitionierten Themen/Warteschlangen](/azure/service-bus-messaging/service-bus-partitioning) pro Namespace |Namespace |Nachfolgende Anforderungen zum Erstellen eines neuen partitionierten Themas bzw. einer neuen Warteschlange im Namespace werden abgelehnt. Als Ergebnis wird bei der Konfiguration über das [Azure-Portal][Azure portal] eine Fehlermeldung generiert. Bei einem Aufruf über die Verwaltungs-API wird vom aufrufenden Code eine **QuotaExceededException**-Ausnahme empfangen. |Basic- und Standard-Tarif – 100<br/><br/>Partitionierte Entitäten werden im [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md)-Tarif nicht unterstützt.<br/><br />Jede partitionierte Warteschlange bzw. jedes partitionierte Thema wird in das zulässige Kontingent von 10.000 Entitäten pro Namespace eingerechnet. |
| Maximale Größe eines Messagingentitätspfads: Warteschlange oder Thema |Entität |- |260 Zeichen |
| Maximale Größe eines Messagingentitätsnamens: Namespace, Abonnement oder Abonnementregel |Entität |- |50 Zeichen |
| Maximale Größe der [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) einer Nachricht | Entität |- | 128 |
| Nachrichtengröße für eine Warteschlangen-/Themen-/Abonnemententität |Entität |Eingehende Nachrichten, die diese Kontingente überschreiten, werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |Maximale Nachrichtengröße: 256 KB ([Standard-Tarif](../articles/service-bus-messaging/service-bus-premium-messaging.md))/1 MB ([Premium-Tarif](../articles/service-bus-messaging/service-bus-premium-messaging.md)). <br /><br />Aufgrund des Mehraufwands für das System ist dieser Grenzwert kleiner als diese Werte.<br /><br />Maximale Headergröße: 64 KB<br /><br />Maximale Anzahl der Headereigenschaften im Eigenschaftenbehälter: **Byte/Int.MaxValue**<br /><br />Maximale Größe der Eigenschaft im Eigenschaftenbehälter: keine ausdrückliche Beschränkung. Beschränkung nach maximaler Headergröße. |
| Nachrichteneigenschaftsgröße für eine Warteschlangen-/Themen-/Abonnemententität |Entität |Eine **SerializationException**-Ausnahme wird generiert. |Die maximale Nachrichteneigenschaftengröße beträgt für jede Eigenschaft 32 K. Die kumulative Größe aller Eigenschaften darf 64 K nicht überschreiten. Dieser Grenzwert gilt für den gesamten Header des [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)-Elements, das sowohl über Benutzereigenschaften als auch über Systemeigenschaften verfügt (z.B. [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid)). |
| Anzahl von Abonnements pro Thema |Entität |Nachfolgende Anforderungen zum Erstellen weiterer Abonnements für das Thema werden abgelehnt. Als Ergebnis wird bei der Konfiguration über das Portal eine Fehlermeldung angezeigt. Bei einem Aufruf über die Verwaltungs-API wird vom aufrufenden Code eine Ausnahme empfangen. |2.000 |
| Anzahl von SQL-Filtern pro Thema |Entität |Nachfolgende Anforderungen für die Erstellung weiterer Filter für das Thema werden abgelehnt, und der aufrufende Code empfängt eine Ausnahme. |2.000 |
| Anzahl von Korrelationsfiltern pro Thema |Entität |Nachfolgende Anforderungen für die Erstellung weiterer Filter für das Thema werden abgelehnt, und der aufrufende Code empfängt eine Ausnahme. |100.000 |
| Größe der SQL-Filter/-Aktionen |Namespace |Nachfolgende Anforderungen für die Erstellung weiterer Filter werden abgelehnt, und der aufrufende Code empfängt eine Ausnahme. |Maximale Länge der Filterbedingungszeichenfolge: 1024 (1 K)<br /><br />Maximale Länge der Regelaktionszeichenfolge: 1024 (1 K)<br /><br />Maximale Anzahl von Ausdrücken pro Regelaktion: 32. |
| Anzahl von [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)-Regeln pro Namespace, Warteschlange oder Thema |Entität, Namespace |Nachfolgende Anforderungen für die Erstellung weiterer Regeln werden abgelehnt, und der aufrufende Code empfängt eine Ausnahme. |Maximale Anzahl von Regeln: 12. <br /><br /> Regeln, die für einen Service Bus-Namespace konfiguriert werden, gelten für alle Warteschlangen und Themen im jeweiligen Namespace. |
| Anzahl der Nachrichten pro Transaktion | Transaktion | Zusätzliche eingehende Nachrichten werden abgelehnt, und eine Ausnahme („Mehr als 100 Nachrichten dürfen nicht in einer einzelnen Transaktion gesendet werden“) wird durch den aufrufenden Code empfangen. | 100 <br /><br /> Für **Send()**- und **SendAsync()**-Vorgänge. |

[Azure portal]: https://portal.azure.com