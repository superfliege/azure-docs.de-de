---
title: "Was sind Dienstintegritätsbenachrichtigungen? | Microsoft-Dokumentation"
description: "Mit Dienstintegritätsbenachrichtigungen können Sie Dienstintegritätsmeldungen von Microsoft Azure anzeigen."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: efdd42d244710b27fc33154b708cfbe40312e3b0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="service-health-notifications"></a>Dienstintegritätsbenachrichtigungen
## <a name="overview"></a>Übersicht

Dieser Artikel zeigt Ihnen, wie Sie Dienstintegritätsbenachrichtigungen im Azure-Portal anzeigen.

Mit Dienstintegritätsbenachrichtigungen können Sie Dienstintegritätsmeldungen des Azure-Teams anzeigen, die sich auf die Ressourcen unter Ihrem Abonnement auswirken könnten. Diese Benachrichtigungen sind eine Unterklasse von Aktivitätsprotokollereignissen und auch im Aktivitätsprotokoll zu finden. Dienstintegritätsbenachrichtigungen können je nach Klasse informativ oder handlungsrelevant sein.

Es gibt fünf Klassen von Dienstintegritätsbenachrichtigungen:  

- **Aktion erforderlich:** Von Zeit zu Zeit stellt Azure möglicherweise fest, dass auf Ihrem Konto etwas Ungewöhnliches stattfindet. Dies muss Azure dann eventuell mit Ihnen gemeinsam klären. Azure sendet Ihnen in diesem Fall eine Benachrichtigung, in der Ihnen ausführlich mitgeteilt wird, welche Maßnahmen Sie ergreifen müssen oder wie Sie sich an Azure-Engineering oder den Support wenden.  
- **Unterstützte Wiederherstellung:** Ein Ereignis ist aufgetreten, und Entwickler haben sich davon überzeugt, dass Sie weiterhin Beeinträchtigungen unterliegen. Das Azure-Engineeringteam muss direkt mit Ihnen zusammenarbeiten, um Ihre Dienste vollständig wiederherzustellen.  
- **Incident:** Ein Ereignis, das Dienste beeinträchtigt, beeinflusst derzeit eine oder mehrere Ressourcen in Ihrem Abonnement.  
- **Wartung:** Diese Benachrichtigung informiert Sie über eine geplante Wartungsaktivität, die eine oder mehrere Ressourcen in Ihrem Abonnement beeinträchtigen kann.  
- **Informationen:** Von Zeit zu Zeit sendet Azure Ihnen möglicherweise Benachrichtigungen, um Sie über potenzielle Optimierungen zu informieren, die Ihre Ressourcenverwendung verbessern könnten.  
- **Sicherheit:** Dringende sicherheitsbezogene Informationen bezüglich Ihrer auf Azure ausgeführten Lösung(en).

Jede Dienstintegritätsbenachrichtigung schließt Details zum Umfang und zu den Auswirkungen auf Ihre Ressourcen ein. Zu den Details gehören:

Eigenschaftenname | Beschreibung
-------- | -----------
channels | Einer der folgenden Werte: „Admin“, „Operation“
correlationId | Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die zur gleichen Uber-Aktion gehören, haben normalerweise die gleiche correlationId.
eventDataId | Eindeutiger Bezeichner eines Ereignisses.
eventName | Der Titel des Ereignisses.
Einstellung | Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose
resourceProviderName | Name des Ressourcenanbieters für die betroffene Ressource.
resourceType| Der Ressourcentyp der betroffenen Ressource.
subStatus | Normalerweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch andere Zeichenfolgen zum Beschreiben eines Unterstatus enthalten, z.B. diese häufigen Werte: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504).
eventTimestamp | Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat.
submissionTimestamp |   Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war.
subscriptionId | Das Azure-Abonnement, in dem dieses Ereignis protokolliert wurde.
status | Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved.
operationName | Name des Vorgangs.
category | „ServiceHealth“
resourceId | Ressourcen-ID der betroffenen Ressource.
Properties.title | Der lokalisierte Titel für diese Kommunikation. Englisch ist die Standardsprache.
Properties.communication | Die lokalisierten Details der Kommunikation mit HTML-Markup. Englisch ist der Standard.
Properties.incidentType | Mögliche Werte: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security
Properties.trackingId | Identifiziert den Incident, dem dieses Ereignis zugeordnet ist. Verwenden Sie diese Option, um die Ereignisse im Zusammenhang mit einem Vorfall zu korrelieren.
Properties.impactedServices | Ein JSON-Blob mit Escapezeichen, das die Dienste und Regionen beschreibt, die vom Incident betroffen sind. Eine Liste der Dienste, von denen jeder über einen ServiceName und eine Liste von ImpactedRegions verfügt, von denen jede einen RegionName hat.
Properties.defaultLanguageTitle | Die Kommunikation in Englisch.
Properties.defaultLanguageContent | Die Kommunikation in Englisch, entweder als HTML-Markup oder Nur-Text.
Properties.stage | Mögliche Werte für AssistedRecovery, ActionRequired, Information, Incident, Security: Active, Resolved. Für die Wartung lauten sie: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete.
Properties.communicationId | Die Kommunikation dieses Ereignisses ist zugeordnet.


## <a name="viewing-your-service-health-notifications-in-the-azure-portal"></a>Anzeigen Ihrer Dienstintegritätsbenachrichtigungen im Azure-Portal
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-service-notifications/home-monitor.png)
2.  Klicken Sie auf die Option **Überwachen**, um die Oberfläche „Überwachen“ zu öffnen. Azure Monitor fasst alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie nun auf den Abschnitt **Warnungen**.

    ![Überwachen](./media/monitoring-service-notifications/service-health-summary.png)
4. Klicken Sie auf **+ Aktivitätsprotokollwarnung hinzufügen**, und konfigurieren Sie eine Warnung, um sicherzustellen, dass Sie zukünftige Dienstbenachrichtigungen erhalten. Weitere Informationen zum Konfigurieren von Warnungen für Dienstbenachrichtigungen finden Sie auf der [Seite für Aktivitätsprotokollwarnungen und Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Nächste Schritte:
Empfangen von [Warnungsbenachrichtigungen, wenn eine Dienstintegritätsbenachrichtigung](monitoring-activity-log-alerts-on-service-notifications.md) gesendet wird  
Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts.md).
