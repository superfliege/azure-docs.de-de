---
title: Ratenlimits für SMS, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhooks | Microsoft-Dokumentation
description: Es wird erläutert, wie Azure die Anzahl der möglichen SMS-, E-Mail-, Azure App-Push- oder Webhookbenachrichtigungen aus einer Aktionsgruppe beschränkt.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/12/2018
ms.author: dukek
ms.openlocfilehash: 9216a64dbd8201ff09ea5c9283b4db465682a3bd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge
Das Ratenlimit führt zu einer Unterbrechung der Benachrichtigungen, wenn zu viele an eine bestimmte Telefonnummer, E-Mail-Adresse oder an ein Gerät gesendet werden. Das Ratenlimit stellt sicher, dass Warnungen verwaltbar und verfolgbar sind.

Folgende Ratenlimits gelten:

 - **SMS**: Nicht mehr als 1 SMS alle 5 Minuten.
 - **Sprache**: nicht mehr als 1 Telefonanruf alle 5 Minuten.
 - **E-Mail**: Nicht mehr als 100 E-Mails in einer Stunde.
 
 Für andere Aktionen gelten keine Ratenlimits.

## <a name="rate-limit-rules"></a>Regeln für Ratenlimits
- Für eine bestimmte Telefonnummer oder E-Mail-Adresse wird die Rate begrenzt, wenn sie mehr Nachrichten erhält, als der Schwellenwert zulässt.
- Eine Telefonnummer oder E-Mail-Adresse kann Mitglied von Aktionsgruppen mehrerer Abonnements sein. Das Ratenlimit gilt für alle Abonnements. Es wird wirksam, sobald der Schwellenwert erreicht wird, auch wenn Nachrichten aus mehreren Abonnements gesendet werden.
- Wenn für eine E-Mail-Adresse ein Ratenlimit gilt, wird eine weitere Benachrichtigung gesendet, um über das Ratenlimit zu informieren. Per E-Mail wird mitgeteilt, wann das Ratenlimit abläuft.

## <a name="next-steps"></a>Nächste Schritte ##
* Erfahren Sie mehr über das [SMS-Warnungsverhalten in Aktionsgruppen](monitoring-sms-alert-behavior.md).
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).
