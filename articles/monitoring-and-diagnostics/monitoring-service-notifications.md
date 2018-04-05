---
title: "Was sind Azure-Dienstintegritätsbenachrichtigungen? | Microsoft-Dokumentation"
description: "Mit Dienstintegritätsbenachrichtigungen können Sie von Microsoft Azure veröffentlichte Dienstintegritätsmeldungen anzeigen."
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
ms.openlocfilehash: 4a95e9882515e6a2861292829a44847e11f39063
ms.sourcegitcommit: 09a2485ce249c3ec8204615ab759e3b58c81d8cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal

Dienstintegritätsbenachrichtigungen werden von Azure veröffentlicht und enthalten Informationen zu den Ressourcen in Ihrem Abonnement. Diese Benachrichtigungen sind eine Unterklasse von Aktivitätsprotokollereignissen und auch im Aktivitätsprotokoll zu finden. Dienstintegritätsbenachrichtigungen können je nach Klasse informativ oder handlungsrelevant sein.

Es gibt verschiedene Klassen von Dienstintegritätsbenachrichtigungen:  

- **Aktion erforderlich:** Möglicherweise wurde von Azure etwas Ungewöhnliches in Ihrem Konto festgestellt. Das Azure-Team arbeitet mit Ihnen zusammen, um das Problem zu beheben. Azure sendet Ihnen eine Benachrichtigung, in der beschrieben wird, welche Maßnahmen Sie ergreifen müssen oder wie Sie sich an das Azure-Engineeringteam oder den Azure-Support wenden.  
- **Unterstützte Wiederherstellung:** Ein Ereignis ist aufgetreten, und das Engineeringteam hat sich davon überzeugt, dass weiterhin Beeinträchtigungen vorliegen. Das Azure-Engineeringteam muss direkt mit Ihnen zusammenarbeiten, um Ihre Dienste vollständig wiederherzustellen.  
- **Incident:** Ein Ereignis, das den Dienst beeinträchtigt, beeinflusst derzeit eine oder mehrere Ressourcen in Ihrem Abonnement.  
- **Wartung:** Eine geplante Wartungsaktivität, die eine oder mehrere Ressourcen in Ihrem Abonnement beeinträchtigen kann.  
- **Information:** Potenzielle Optimierungen, mit denen Sie Ihre Ressourcennutzung verbessern können. 
- **Sicherheit:** Dringende sicherheitsbezogene Informationen zu Ihren auf Azure ausgeführten Lösungen.

Jede Dienstintegritätsbenachrichtigung schließt Details zum Umfang und zu den Auswirkungen auf Ihre Ressourcen ein. Zu den Details gehören:

Eigenschaftenname | BESCHREIBUNG
-------- | -----------
channels | Einer der folgenden Werte: **Admin** oder **Vorgang**.
correlationId | Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die zur gleichen Aktion gehören, weisen normalerweise den gleichen correlationId-Wert auf.
eventDataId | Der eindeutige Bezeichner eines Ereignisses.
eventName | Der Titel eines Ereignisses.
level | Das Level eines Ereignisses. Einer der folgenden Werte: **Kritisch**, **Fehler**, **Warnung** oder **Information**.
resourceProviderName | Der Name des Ressourcenanbieters für die betroffene Ressource.
resourceType| Der Ressourcentyp der betroffenen Ressource.
subStatus | Üblicherweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch weitere Zeichenfolgen zur Beschreibung eines untergeordneten Status enthalten. Beispiele: „OK“ (HTTP-Statuscode: 200), „Erstellt“ (HTTP-Statuscode: 201), „Akzeptiert“ (HTTP-Statuscode: 202), „Kein Inhalt“ (HTTP-Statuscode: 204), „Ungültige Anforderung“ (HTTP-Statuscode: 400), „Nicht gefunden“ (HTTP-Statuscode: 404), „Konflikt“ (HTTP-Statuscode: 409), „Interner Serverfehler“ (HTTP-Statuscode: 500), „Dienst nicht verfügbar“ (HTTP-Statuscode: 503) und „Gatewaytimeout“ (HTTP-Statuscode: 504).
eventTimestamp | Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat.
submissionTimestamp | Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war.
subscriptionId | Das Azure-Abonnement, in dem dieses Ereignis protokolliert wurde.
status | Zeichenfolge, die den Status des Vorgangs beschreibt. Einige häufig verwendete Werte: **Gestartet**, **In Bearbeitung**, **Erfolgreich**, **Fehler**, **Aktiv** und **Aufgelöst**.
operationName | Der Name des Vorgangs.
category | Diese Eigenschaft lautet immer **ServiceHealth**.
Ressourcen-ID | Die Ressourcen-ID der betroffenen Ressource.
Properties.title | Der lokalisierte Titel für diese Kommunikation. Englisch ist der Standard.
Properties.communication | Die lokalisierten Details der Kommunikation mit HTML-Markup. Englisch ist der Standard.
Properties.incidentType | Einer der folgenden Werte **AssistedRecovery**, **ActionRequired**, **Information**, **Incident**, **Wartung** oder **Sicherheit**.
Properties.trackingId | Der Incident, dem dieses Ereignis zugeordnet ist. Verwenden Sie diese Option, um die Ereignisse im Zusammenhang mit einem Vorfall zu korrelieren.
Properties.impactedServices | Ein JSON-Blob mit Escapezeichen, das die vom Incident betroffenen Dienste und Regionen beschreibt. Die Eigenschaft umfasst eine Liste der Dienste, von denen jeder über einen **ServiceName** verfügt, sowie eine Liste der betroffenen Regionen, von denen jede einen **RegionName** aufweist.
Properties.defaultLanguageTitle | Die Kommunikation auf Englisch.
Properties.defaultLanguageContent | Die Kommunikation auf Englisch, entweder als HTML-Markup oder Nur-Text.
Properties.stage | Die möglichen Werte für **AssistedRecovery**, **ActionRequired**, **Information**, **Incident** und **Sicherheit** sind **Aktiv** oder **Aufgelöst**. Für **Wartung** sind folgende Werte möglich: **Aktiv**, **Geplant**, **InProgress**, **Abgebrochen**, **Neu geplant**, **Aufgelöst** oder **Abgeschlossen**.
Properties.communicationId | Die Kommunikation, der dieses Ereignis ist zugeordnet ist.


## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Anzeigen Ihrer Dienstintegritätsbenachrichtigungen im Azure-Portal
1.  Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Überwachen** aus.

    ![Screenshot des Menüs im Azure-Portal mit ausgewählter Überwachungsoption](./media/monitoring-service-notifications/home-monitor.png)

    Azure Monitor fasst alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Wählen Sie **Warnungen**.

    ![Screenshot des Überwachungsaktivitätsprotokolls mit ausgewählten Warnungen](./media/monitoring-service-notifications/service-health-summary.png)
4. Wählen Sie **+ Aktivitätsprotokollwarnung hinzufügen** aus, und richten Sie eine Warnung ein, um sicherzustellen, dass Sie zukünftige Dienstbenachrichtigungen erhalten. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Nächste Schritte
Erhalten Sie [Warnungsbenachrichtigungen, wenn eine Dienstintegritätsbenachrichtigung](monitoring-activity-log-alerts-on-service-notifications.md) gesendet wird.  
Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts.md).
