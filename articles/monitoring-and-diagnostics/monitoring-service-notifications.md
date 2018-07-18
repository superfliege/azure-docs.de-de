---
title: Was sind Azure-Dienstintegritätsbenachrichtigungen?
description: Mit Dienstintegritätsbenachrichtigungen können Sie von Microsoft Azure veröffentlichte Dienstintegritätsmeldungen anzeigen.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2017
ms.author: dukek
ms.component: activitylog
ms.openlocfilehash: d6a87b17041c4ce6cf41da863354ef5a2a37141c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264441"
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
level | Die Stufe eines Ereignisses
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
Properties.incidentType | Einer der folgenden Werte: **ActionRequired**, **Information**, **Incident**, **Maintenance** oder **Security**.
Properties.trackingId | Der Incident, dem dieses Ereignis zugeordnet ist. Verwenden Sie diese Option, um die Ereignisse im Zusammenhang mit einem Vorfall zu korrelieren.
Properties.impactedServices | Ein JSON-Blob mit Escapezeichen, das die vom Incident betroffenen Dienste und Regionen beschreibt. Die Eigenschaft umfasst eine Liste der Dienste, von denen jeder über einen **ServiceName** verfügt, sowie eine Liste der betroffenen Regionen, von denen jede einen **RegionName** aufweist.
Properties.defaultLanguageTitle | Die Kommunikation auf Englisch.
Properties.defaultLanguageContent | Die Kommunikation auf Englisch, entweder als HTML-Markup oder Nur-Text.
Properties.stage | Die möglichen Werte für **Incident** und **Security** sind **Active,** **Resolved** oder **RCA**. Der einzige Wert für **ActionRequired** oder **Information** ist **Active**. Für **Maintenance** sind folgende Werte möglich: **Active**, **Planned**, **InProgress**, **Canceled**, **Rescheduled**, **Resolved** oder **Complete**.
Properties.communicationId | Die Kommunikation, der dieses Ereignis ist zugeordnet ist.

### <a name="details-on-service-health-level-information"></a>Details zu Informationen zur Dienstintegritätsebene
  <ul>
    <li><b>Aktion erforderlich</b> (properties.incidentType == ActionRequired) <dl>
            <dt>Zur Information</dt>
            <dd>Aktion des Administrators erforderlich, um Auswirkungen auf bestehende Dienste zu vermeiden</dd>
        </dl>
    </li>
    <li><b>Wartung</b> (properties.incidentType == Maintenance) <dl>
            <dt>Warnung</dt>
            <dd>Notfallwartung<dd>
            <dt>Zur Information</dt>
            <dd>Standardmäßig geplante Wartung</dd>
        </dl>
    </li>
    <li><b>Informationen</b> (properties.incidentType == Information) <dl>
            <dt>Zur Information</dt>
            <dd>Administrator muss ggf. eingreifen, um Auswirkungen auf bestehende Dienste zu vermeiden</dd>
        </dl>
    </li>
    <li><b>Sicherheit</b> (properties.incidentType == Security) <dl>
            <dt>Fehler</dt>
            <dd>Weit verbreitete Probleme beim Zugriff auf mehrere Dienste in verschiedenen Regionen wirken sich auf eine Vielzahl von Kunden aus.</dd>
            <dt>Warnung</dt>
            <dd>Probleme beim Zugriff auf bestimmte Dienste und/oder bestimmte Regionen wirken sich auf eine Teilmenge von Kunden aus.</dd>
            <dt>Zur Information</dt>
            <dd>Probleme, die sich auf den Verwaltungsbetrieb und/oder die Latenz auswirken, ohne die Verfügbarkeit von Diensten zu beeinträchtigen.</dd>
        </dl>
    </li>
    <li><b>Dienstprobleme</b> (properties.incidentType == Incident) <dl>
            <dt>Fehler</dt>
            <dd>Weit verbreitete Probleme beim Zugriff auf mehrere Dienste in verschiedenen Regionen wirken sich auf eine Vielzahl von Kunden aus.</dd>
            <dt>Warnung</dt>
            <dd>Probleme beim Zugriff auf bestimmte Dienste und/oder bestimmte Regionen wirken sich auf eine Teilmenge von Kunden aus.</dd>
            <dt>Zur Information</dt>
            <dd>Probleme, die sich auf den Verwaltungsbetrieb und/oder die Latenz auswirken, ohne die Verfügbarkeit von Diensten zu beeinträchtigen.</dd>
        </dl>
    </li>
  </ul>

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
