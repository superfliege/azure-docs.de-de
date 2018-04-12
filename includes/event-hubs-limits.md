---
title: Includedatei
description: Includedatei
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 02/26/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: ab4c5b98ed9f6fcc8c271797db2d81dcc7ec4449
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
In der folgenden Tabelle sind die Kontingente und Grenzwerte aufgelistet, die für [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) gelten. Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

| Begrenzung | Umfang | Notizen | Wert |
| --- | --- | --- | --- | --- |
| Anzahl von Event Hubs pro Namespace |Namespace |Nachfolgende Anforderungen für die Erstellung eines neuen Event Hub werden zurückgewiesen. |10 |
| Anzahl von Partitionen pro Event Hub |Entität |- |32 |
| Anzahl von Consumergruppen pro Event Hub |Entität |- |20 |
| Anzahl von AMQP-Verbindungen pro Namespace |Namespace |Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |5.000 |
| Maximale Größe des Event Hubs-Ereignisses|Entität |- |256 KB |
| Maximale Größe eines Event Hub-Namens |Entität |- |50 Zeichen |
| Anzahl nicht epochenbezogener Empfänger pro Consumergruppe |Entität |- |5 |
| Maximale Aufbewahrungsdauer von Ereignisdaten |Entität |- |1–7 Tage |
| Maximale Durchsatzeinheiten |Namespace |Bei einer Überschreitung des Grenzwerts für Durchsatzeinheiten werden Ihre Daten gedrosselt, und eine **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**-Ausnahme wird ausgelöst. Sie können im Tarif „Standard“ eine höhere Anzahl von Durchsatzeinheiten anfordern, indem Sie eine [Supportanfrage](/azure/azure-supportability/how-to-create-azure-support-request) erstellen. [Zusätzliche Durchsatzeinheiten](../articles/event-hubs/event-hubs-auto-inflate.md) sind für einen festgelegten Kaufpreis in 20er-Blöcken verfügbar. |20 |
| Anzahl von Autorisierungsregeln pro Namespace |Namespace|Nachfolgende Anforderungen zur Erstellung von Autorisierungsregeln werden abgelehnt.|12 |
