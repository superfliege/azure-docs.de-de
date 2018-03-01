---
title: SMS-Warnungsverhalten in Aktionsgruppen | Microsoft-Dokumentation
description: "SMS-Nachrichtenformat und Antworten auf SMS-Nachrichten zum Kündigen des Abonnements, erneuten Abonnieren oder Anfordern von Hilfe."
author: dkamstra
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: dukek
ms.openlocfilehash: ce6908de0f6bcc30d1ee846fe92171a0cb589cbb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS-Warnungsverhalten in Aktionsgruppen
## <a name="overview"></a>Übersicht ##
Mit Aktionsgruppen können Sie eine Aktionsliste konfigurieren. Diese Gruppen werden beim Definieren von Warnungen genutzt, um sicherzustellen, dass eine bestimmte Aktionsgruppe benachrichtigt wird, wenn die Warnung ausgelöst wird. Eine der unterstützten Aktionen ist SMS – SMS-Benachrichtigungen unterstützen die bidirektionale Kommunikation. Ein Benutzer kann auf eine SMS antworten:

- **Abonnieren von Warnungen kündigen:** Ein Benutzer kann das Abonnieren aller SMS-Warnungen für alle Aktionsgruppen oder eine einzelne Aktionsgruppe kündigen.
- **Warnungen neu abonnieren:** Ein Benutzer kann alle SMS-Warnungen für alle Aktionsgruppen oder eine einzelne Aktionsgruppe neu abonnieren.  
- **Hilfe anfordern:** Ein Benutzer kann weitere Informationen per SMS anfordern. Er wird zu diesem Artikel weitergeleitet.

Dieser Artikel behandelt das Verhalten der SMS-Warnungen und die Antwortaktionen, die der Benutzer durchführen kann, basierend auf dem Gebietsschema des Benutzers:

## <a name="receiving-an-sms-alert"></a>Empfangen einer SMS-Warnung
Ein SMS-Empfänger, der als Teil einer Aktionsgruppe konfiguriert ist, erhält eine SMS, wenn eine Warnung ausgelöst wird. Die SMS enthält die folgenden Informationen:
* Kurzname der Aktionsgruppe, an die diese Warnung gesendet wurde.
- Titel der Warnung.

| REPLY | BESCHREIBUNG |
| ----- | ----------- |
| DISABLE <Action Group Short name> | Deaktiviert weitere SMS-Benachrichtigungen von der Aktionsgruppe |
| ENABLE <Action Group Short name> | Aktiviert SMS-Benachrichtigungen von der Aktionsgruppe wieder |
| STOP | Deaktiviert weitere SMS-Benachrichtigungen von allen Aktionsgruppen |
| START | Aktiviert SMS-Benachrichtigungen von ALLEN Aktionsgruppen wieder |
| HELP | Der Benutzer erhält eine Antwort mit einem Link zu diesem Artikel. |

>[!NOTE]
>Wenn ein Benutzer das Abonnement aller SMS-Warnungen gekündigt hat, aber dann einer neuen Aktionsgruppe hinzugefügt wird, EMPFÄNGT er SMS-Warnungen für diese neue Aktionsgruppe, doch die Kündigung des Abonnements für alle vorherigen Aktionsgruppen bleibt bestehen.

## <a name="next-steps"></a>Nächste Schritte
Verschaffen Sie sich einen [Überblick über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie gewarnt werden können.  
Erfahren Sie mehr über die [SMS-Rateneinschränkung](monitoring-alerts-rate-limiting.md).  
Erfahren Sie mehr über [Aktionsgruppen](monitoring-action-groups.md).
