---
title: Service Map-Lösung in Azure-Demo mit individuellem Arbeitstempo | Microsoft-Dokumentation
description: Service Map ist eine Lösung in Azure, die Anwendungskomponenten auf Windows- und Linux-Systemen automatisch ermittelt und die Kommunikation zwischen Diensten abbildet. Dies ist eine Demo mit individuellem Arbeitstempo, bei der schrittweise beschrieben wird, wie Sie Service Map zum Identifizieren und Diagnostizieren eines simulierten Problems in einer Webanwendung verwenden.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: monitoring
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: b406cb2a9e96e3ba3514ed08c20483df57de9c71
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="self-paced-demo---service-map"></a>Demo mit individuellem Arbeitstempo – Service Map
Dies ist eine Demo mit individuellem Arbeitstempo, bei der schrittweise beschrieben wird, wie Sie die [Service Map-Lösung](monitoring-service-map.md) zum Identifizieren und Diagnostizieren eines simulierten Problems in einer Webanwendung verwenden. Service Map ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. Außerdem werden Daten zusammengefasst, die von anderen Diensten und Lösungen gesammelt werden, um Sie beim Analysieren der Leistung und Identifizieren von Problemen zu unterstützen. Außerdem nutzen Sie [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md), um einen Drilldown für erfasste Daten auszuführen und das zugrunde liegende Problem zu identifizieren.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Sie haben soeben eine Benachrichtigung erhalten, dass für die ACME Customer Portal-Anwendung Leistungsprobleme bestehen. Sie verfügen nur über die Information, dass diese Probleme heute um 4:00 Uhr morgens (PST) begonnen haben. Sie wissen nicht genau, von welchen Komponenten das Portal abhängig ist (mit Ausnahme einer Gruppe von Webservern). 

## <a name="components-and-features-used"></a>Verwendete Komponenten und Features
- [Service Map-Lösung](monitoring-service-map.md)
- [Log Analytics-Protokollsuchen](../log-analytics/log-analytics-log-searches.md)


## <a name="walkthrough"></a>Exemplarische Vorgehensweise

### <a name="1-connect-to-the-oms-experience-center"></a>1. Herstellen einer Verbindung mit dem OMS Experience Center
In dieser exemplarischen Vorgehensweise wird das [Operations Management Suite Experience Center](https://experience.mms.microsoft.com/) genutzt, über das eine vollständige Log Analytics-Umgebung mit Beispieldaten bereitgestellt wird. Verwenden Sie den obigen Link, geben Sie Ihre Informationen ein, und wählen Sie dann das Szenario **Insight & Analytics** aus.


### <a name="2-start-service-map"></a>2. Starten von Service Map
Starten Sie die Service Map-Lösung, indem Sie auf die Kachel **Service Map** klicken.

![Service Map-Kachel](media/monitoring-walkthrough-servicemap/tile.png)

Die Service Map-Konsole wird angezeigt. Im linken Bereich wird eine Liste mit den Computern Ihrer Umgebung angezeigt, für die der Service Map-Agent installiert wurde. In dieser Liste wählen Sie den Computer aus, den Sie anzeigen möchten.

![Computerliste](media/monitoring-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Anzeigen des Computers
Sie wissen, dass die Webserver die Namen AcmeWFE001 und AcmeWFE002 haben. Dies ist also ein guter Ort, um zu beginnen. Klicken Sie auf **AcmeWFE001**. Die Zuordnung für AcmeWFE001 und alle Abhängigkeiten wird angezeigt. Sie können anzeigen, welche Prozesse auf dem ausgewählten Computer ausgeführt werden und mit welchen externen Diensten sie kommunizieren.

![Webserver](media/monitoring-walkthrough-servicemap/web-server.png)

Da es um die Leistung der Webanwendung geht, klicken Sie auf den Prozess **AcmeAppPool (IIS App Pool)**. Die Details zu diesem Prozess werden angezeigt, und die dazugehörigen Abhängigkeiten werden hervorgehoben. 

![App-Pool](media/monitoring-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Ändern des Zeitfensters

Sie haben gehört, dass das Problem zuerst um 4:00 Uhr nachts aufgetreten ist. Daher sehen Sie sich an, was zu dieser Zeit passiert ist. Klicken Sie auf **Zeitbereich**, und ändern Sie die Uhrzeit in „4:00 PST“ (aktuelles Datum beibehalten und Anpassung an Ihre lokale Zeitzone vornehmen) und eine Dauer von 20 Minuten.

![Zeitauswahl](./media/monitoring-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Anzeigen einer Warnung

Sie können erkennen, dass für die Abhängigkeit **acmetomcat** eine Warnung angezeigt wird. Dies ist also das potenzielle Problem. Klicken Sie auf das Warnsymbol unter **acmetomcat**, um die Details der Warnung anzuzeigen. Sie können sehen, dass eine kritische CPU-Auslastung vorgelegen hat, und weitere Details dazu anzeigen. Dies ist wahrscheinlich der Grund für die schlechte Leistung. 

![Warnung](./media/monitoring-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Anzeigen der Leistung

Wir sehen uns **acmetomcat** genauer an. Klicken Sie rechts oberhalb von **acmetomcat**, und wählen Sie die Option **Serverübersicht laden**, um die Details und Abhängigkeiten für den Computer anzuzeigen. Anschließend können Sie sich eingehender über diese Leistungsindikatoren informieren, um den Verdacht zu untermauern. Wählen Sie die Registerkarte **Leistung**, um die [mit Log Analytics erfassten Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) für den Zeitbereich anzuzeigen. Sie sehen, dass für den Prozessor und Arbeitsspeicher regelmäßige Spitzenauslastungen auftreten.

![Leistung](./media/monitoring-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Anzeigen der Änderungsnachverfolgung
Wir möchten nun ermitteln, welchen Grund diese hohe Auslastung hat. Klicken Sie auf die Registerkarte **Zusammenfassung**. Sie enthält Informationen, die per Log Analytics für den Computer erfasst wurden, z.B. Fehler bei der Verbindungsherstellung, kritische Warnungen und Softwareänderungen. Abschnitte mit relevanten Informationen, die kürzlich erfasst wurden, sollten bereits erweitert sein. Sie können auch die anderen Abschnitte erweitern, um die darin enthaltenen Informationen anzuzeigen.


Erweitern Sie die Option **Änderungsnachverfolgung**, falls sie nicht bereits geöffnet ist. Darunter werden Informationen angezeigt, die mit der [Lösung für die Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md) erfasst wurden. Es sieht so aus, als ob in diesem Zeitfenster eine Softwareänderung vorgenommen wurde. Klicken Sie auf **Software**, um die Details hierzu anzuzeigen. Kurz nach 4:00 Uhr wurde dem Computer ein Sicherungsprozess hinzugefügt. Dies scheint der Grund für die übermäßige Ressourcennutzung zu sein.

![Änderungsnachverfolgung](./media/monitoring-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Anzeigen von Details in der Protokollsuche
Sie können dies weiter überprüfen, indem Sie sich die ausführlichen Leistungsinformationen ansehen, die im Log Analytics-Arbeitsbereich erfasst wurden. Klicken Sie erneut auf die Registerkarte **Warnungen** und dann auf eine der Warnungen unter **High CPU** (Hohe CPU-Auslastung). Klicken Sie auf **In Protokollsuche anzeigen**. Das Fenster „Protokollsuche“ wird geöffnet. In diesem Fenster können Sie [Protokollsuchen](../log-analytics/log-analytics-log-searches.md) für im Arbeitsbereich gespeicherte Daten durchführen. Von Service Map wurde bereits eine Abfrage eingefügt, mit der wir die gewünschte Warnung abrufen können. 

![Protokollsuche](./media/monitoring-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Öffnen der gespeicherten Suche
Sie möchten nun weitere Details zur Erfassung von Leistungsdaten erhalten, bei der diese Warnung generiert wurde, und Ihren Verdacht bestätigen, dass die Probleme von diesem Sicherungsprozess verursacht werden. Ändern Sie den Zeitbereich in **6 Stunden**. Klicken Sie anschließend auf **Favoriten**, und führen Sie einen Bildlauf nach unten zu den gespeicherten Suchen für **Service Map** durch. Sie haben diese Abfragen speziell für diese Analyse erstellt. Klicken Sie auf **Top 5 Processes by CPU for acmetomcat** (Top 5-Prozesse nach CPU für acmetomcat).

![Gespeicherte Suche](./media/monitoring-walkthrough-servicemap/saved-search.png)


Diese Abfrage gibt eine Liste mit den Top 5 der Prozesse zurück, die den Prozessor für **acmetomcat** nutzen. Sie können sich die Abfrage näher ansehen, um sich über die Abfragesprache zu informieren, die für Protokollsuchen verwendet wird. Falls Prozesse auf anderen Computern für Sie interessant sind, können Sie die Abfrage entsprechend ändern, um die entsprechenden Informationen abzurufen.

In diesem Fall ist erkennbar, dass der Sicherungsprozess ständig ca. 60% der CPU des App-Servers verbraucht. Es ist offensichtlich, dass dieser neue Prozess für das Leistungsproblem verantwortlich ist. Die Lösung sollte nun natürlich darin bestehen, diese neue Sicherungssoftware vom Anwendungsserver zu entfernen. Sie können die Konfiguration für den gewünschten Zustand (Desired State Configuration, DSC) nutzen, die von Azure Automation verwaltet wird, um anhand von Richtlinien sicherzustellen, dass dieser Prozessserver auf diesen kritischen Systemen nicht ausgeführt wird.


## <a name="summary-points"></a>Zusammenfassung
- Mit [Service Map](monitoring-service-map.md) erhalten Sie auch dann eine Übersicht über Ihre gesamte Anwendung, wenn Ihnen nicht alle dazugehörigen Server und Abhängigkeiten bekannt sind.
- Service Map bringt Daten ans Licht, die mit anderen Verwaltungslösungen erfasst werden, damit Sie Probleme mit Ihrer Anwendung und der zugrunde liegenden Infrastruktur identifizieren können.
- Mit [Protokollsuchen](../log-analytics/log-analytics-log-searches.md) können Sie einen Drilldown für bestimmte Daten durchführen, die im Log Analytics-Arbeitsbereich erfasst werden.  

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Service Map](monitoring-service-map.md)
- [Bereitstellen und Konfigurieren](monitoring-service-map-configure.md) von Service Map
- Informieren Sie sich über [Log Analytics](../log-analytics/log-analytics-overview.md). Diese Anwendung ist für Service Map erforderlich und dient zum Speichern von Betriebsdaten, die von Agents gespeichert werden.