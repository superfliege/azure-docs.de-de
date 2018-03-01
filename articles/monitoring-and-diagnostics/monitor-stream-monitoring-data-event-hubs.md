---
title: "Streamen von Azure-Überwachungsdaten an Event Hubs | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie all Ihre Azure-Überwachungsdaten an einen Event Hub streamen, um die Daten in einem SIEM- oder Analysetool von Partnern abzurufen."
author: johnkemnetz
manager: robb
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2018
ms.author: johnkem
ms.openlocfilehash: d449be98cd59756e2bafc584e0501b8c83c594eb
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool

Azure Monitor stellt eine zentrale Pipeline für den Zugriff auf sämtliche Überwachungsdaten aus der Azure-Umgebung bereit, sodass Sie mühelos SIEM- und Überwachungstools von Partnern für die Datenverarbeitung einrichten können. In diesem Artikel wird das Einrichten der verschiedenen Schichten an Daten aus der Azure-Umgebung erläutert, die für die Erfassung durch ein externes Tool an einen einzelnen Event Hubs-Namespace oder Event Hub gesendet werden sollen.

## <a name="what-data-can-i-send-into-an-event-hub"></a>Welche Daten kann ich an einen Event Hub senden? 

In Ihrer Azure-Umgebung gibt es mehrere Schichten von Überwachungsdaten, weshalb die Zugriffsmethode für Daten aus verschiedenen Schichten geringfügig variiert. Diese Schichten lassen sich in der Regel wie folgt definieren:

- **Überwachungsdaten zur Anwendung:** Daten zur Leistung und Funktionalität des von Ihnen geschriebenen Codes, den Sie in Azure ausführen. Zu Anwendungsüberwachungsdaten zählen z.B. Leistungsablaufverfolgungen, Anwendungsprotokolle und benutzerspezifische Telemetriedaten. Anwendungsüberwachungsdaten werden in der Regel auf einer der folgenden Arten gesammelt:
  - Durch Instrumentieren des Codes mit einem SDK wie dem [Application Insights-SDK](../application-insights/app-insights-overview.md)
  - Durch Ausführen eines Überwachungs-Agents, der nach neuen Anwendungsprotokollen auf dem Computer lauscht, auf dem Ihre Anwendung ausgeführt wird (z.B. der [Azure-Diagnose-Agent für Windows](./azure-diagnostics.md) oder [Azure-Diagnose-Agent für Linux](../virtual-machines/linux/diagnostic-extension.md))
- **Überwachungsdaten zum Gast-BS:** Daten zum Betriebssystem, unter dem die Anwendung ausgeführt wird. Zu Überwachungsdaten zum Gast-BS zählen z.B. das Linux-Syslog oder Windows-Systemereignisse. Um diesen Typ von Daten zu sammeln, müssen Sie einen Agent wie den [Azure-Diagnose-Agent für Windows](./azure-diagnostics.md) oder [Azure-Diagnose-Agent für Linux](../virtual-machines/linux/diagnostic-extension.md) installieren.
- **Überwachungsdaten zu Azure-Ressourcen:** Daten zum Betrieb einer Azure-Ressource. Bei einigen Arten von Azure-Ressourcen wie virtuellen Computern gibt es ein Gast-BS und Anwendungen, die für die Überwachung innerhalb dieses Azure-Diensts vorgesehen sind. Bei anderen Azure-Ressourcen wie Netzwerksicherheitsgruppen sind Überwachungsdaten zu Ressourcen in der höchsten Datenschicht verfügbar (da weder ein Gast-BS noch eine Anwendung in diesen Ressourcen ausgeführt wird). Diese Daten können mithilfe von [Ressourcendiagnoseeinstellungen](./monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) gesammelt werden.
- **Überwachungsdaten zur Azure-Plattform:** Daten zum Betrieb und zur Verwaltung eines Azure-Abonnements oder -Mandanten sowie Daten zur Integrität und zum Betrieb von Azure selbst. Überwachungsdaten zur Plattform können beispielsweise [Aktivitätsprotokolle](./monitoring-overview-activity-logs.md) sein, einschließlich Dienstintegritätsdaten und Active Directory-Überwachungsdaten. Diese Daten können auch mithilfe von Diagnoseeinstellungen gesammelt werden.

Daten aus beliebigen Schichten können an einen Event Hub gesendet und in ein Partnertool geladen werden. In den nächsten Abschnitten wird beschrieben, wie Sie Daten aus den einzelnen Schichten zum Streamen an einen Event Hub konfigurieren können. Bei der Erläuterung der Schritte wird davon ausgegangen, dass bereits zu überwachende Ressourcen in dieser Schicht vorhanden sind.

Bevor Sie beginnen, müssen Sie [einen Event Hubs-Namespace und einen Event Hub erstellen](../event-hubs/event-hubs-create.md). Dieser Namespace und Event Hub sind das Ziel für sämtliche Ihrer Überwachungsdaten.

## <a name="how-do-i-set-up-azure-platform-monitoring-data-to-be-streamed-to-an-event-hub"></a>Wie richte ich die Überwachungsdaten zur Azure-Plattform für das Streaming an einen Event Hub ein?

Überwachungsdaten zur Azure-Plattform stammen aus zwei Hauptquellen:
1. [Azure-Aktivitätsprotokolle](./monitoring-overview-activity-logs.md), die Erstellungs-, Aktualisierungs- und Löschvorgänge im Resource Manager, Änderungen in [Azure Service Health](../service-health/service-health-overview.md) mit Auswirkungen auf Ressourcen in Ihrem Abonnement, [Ressourcenintegrität](../service-health/resource-health-overview.md)-Zustandsübergänge und diverse andere Typen von Ereignissen auf Abonnementebene umfassen. [In diesem Artikel werden alle Kategorien von Ereignissen erläutert, die im Azure-Aktivitätsprotokoll angezeigt werden](./monitoring-activity-log-schema.md).
2. [Azure Active Directory-Berichterstellung](../active-directory/active-directory-reporting-azure-portal.md), die den Verlauf der Anmeldeaktivität und des Überwachungspfads von Änderungen beinhaltet, die innerhalb eines bestimmten Mandanten vorgenommen wurden. Es ist noch nicht möglich, Azure Active Directory-Daten an einen Event Hub zu streamen.

### <a name="stream-azure-activity-log-data-into-an-event-hub"></a>Streamen von Azure-Aktivitätsprotokolldaten an einen Event Hub

Um Daten aus dem Azure-Aktivitätsprotokoll an Event Hubs-Namespaces zu senden, richten Sie ein Protokollprofil für Ihr Abonnement ein. [Befolgen Sie die Schritte in diesem Handbuch](./monitoring-stream-activity-logs-event-hubs.md) zum Einrichten eines Protokollprofils für Ihr Abonnement. Führen Sie diesen Vorgang für jedes zu überwachende Abonnement durch.

> [!TIP]
> Derzeit können Sie mithilfe eines Protokollprofils nur einen Event Hubs-Namespace auswählen, in dem ein Event Hub mit dem Namen „insights-operational-logs“ erstellt wird. Es ist noch nicht möglich, einen eigenen Event Hub-Namen in einem Protokollprofil anzugeben.

## <a name="how-do-i-set-up-azure-resource-monitoring-data-to-be-streamed-to-an-event-hub"></a>Wie richte ich die Überwachungsdaten zu Azure-Ressourcen für das Streaming an einen Event Hub ein?

Azure-Ressourcen geben zwei Arten von Überwachungsdaten aus:
1. [Ressourcendiagnoseprotokolle](./monitoring-overview-of-diagnostic-logs.md)
2. [Metriken](monitoring-overview-metrics.md)

Beide Arten von Daten werden mithilfe einer Ressourcendiagnoseeinstellung an einen Event Hub gesendet. [Befolgen Sie die Schritte in diesem Handbuch](./monitoring-stream-diagnostic-logs-to-event-hubs.md) zum Einrichten einer Ressourcendiagnoseeinstellung für eine bestimmte Ressource. Legen Sie für jede Ressource, für die Protokolle gesammelt werden sollen, eine Ressourcendiagnoseeinstellung fest.

> [!TIP]
> Mit Azure Policy können Sie [durch den DeployIfNotExists-Effekt in der Richtlinienregel](../azure-policy/policy-definition.md#policy-rule) sicherstellen, dass alle Ressourcen innerhalb eines bestimmten Bereichs stets mit einer Diagnoseeinstellung eingerichtet werden. DeployIfNotExists wird derzeit nur für integrierte Richtlinien unterstützt.

## <a name="how-do-i-set-up-guest-os-monitoring-data-to-be-streamed-to-an-event-hub"></a>Wie richte ich die Überwachungsdaten zu Gast-BS für das Streaming an einen Event Hub ein?

Sie müssen einen Agent zum Senden von Überwachungsdaten zu Gast-BS an einen Event Hub installieren. Unter Windows oder Linux geben Sie die an den Event Hub zu sendenden Daten sowie den Event Hub an, an den die Daten in einer Konfigurationsdatei gesendet werden sollen, und übergeben die Konfigurationsdatei an den Agent, der auf der VM ausgeführt wird.

### <a name="stream-linux-data-to-an-event-hub"></a>Streamen von Linux-Daten an einen Event Hub

Mit dem [Azure-Diagnose-Agent für Linux](../virtual-machines/linux/diagnostic-extension.md) können Überwachungsdaten von einem Linux-Computer an einen Event Hub gesendet werden. Hierzu wird der Event Hub als Senke in den geschützten JSON-Einstellungen Ihrer LAD-Konfigurationsdatei. [In diesem Artikel finden Sie weitere Informationen zum Hinzufügen der Event Hub-Senke zu Ihrem Azure-Diagnose-Agent für Linux](../virtual-machines/linux/diagnostic-extension.md#protected-settings).

> [!NOTE]
> Das Streaming von Überwachungsdaten zu Gast-BS an einen Event Hub kann nicht im Portal eingerichtet werden. Sie müssen die Konfigurationsdatei stattdessen manuell bearbeiten.

### <a name="stream-windows-data-to-an-event-hub"></a>Streamen von Windows-Daten an einen Event Hub

Mit dem [Azure-Diagnose-Agent für Windows](./azure-diagnostics.md) können Überwachungsdaten von einem Windows-Computer an einen Event Hub gesendet werden. Hierzu wird der Event Hub im privateConfig-Abschnitt der WAD-Konfigurationsdatei als Senke hinzugefügt. [In diesem Artikel finden Sie weitere Informationen zum Hinzufügen der Event Hub-Senke zu Ihrem Azure-Diagnose-Agent für Windows](./azure-diagnostics-streaming-event-hubs.md).

> [!NOTE]
> Das Streaming von Überwachungsdaten zu Gast-BS an einen Event Hub kann nicht im Portal eingerichtet werden. Sie müssen die Konfigurationsdatei stattdessen manuell bearbeiten.

## <a name="how-do-i-set-up-application-monitoring-data-to-be-streamed-to-event-hub"></a>Wie richte ich die Überwachungsdaten zu Anwendungen für das Streaming an einen Event Hub ein?

Für Überwachungsdaten zu Anwendungen muss Ihr Code mit einem SDK instrumentiert sein. Deshalb gibt es keine allgemeine Lösung, um Überwachungsdaten zu Anwendungen an einen Event Hub in Azure weiterzuleiten. Allerdings ist [Azure Application Insights](../application-insights/app-insights-overview.md) ein Dienst, der zum Sammeln von Azure-Daten auf Anwendungsebene verwendet werden kann. Wenn Sie Application Insights verwenden, können Sie Überwachungsdaten folgendermaßen an einen Event Hub streamen:

1. [Richten Sie den fortlaufenden Export](../application-insights/app-insights-export-telemetry.md) der Application Insights-Daten in ein Speicherkonto ein.

2. Richten Sie eine per Timer ausgelöste Logik-App ein, die [Daten aus Blob Storage abruft](../connectors/connectors-create-api-azureblobstorage.md#use-an-action) und [mithilfe von Push als Nachricht an den Event Hub überträgt](../connectors/connectors-create-api-azure-event-hubs.md#send-events-to-your-event-hub-from-your-logic-app).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>Wie kann ich die an meinen Event Hub gesendeten Überwachungsdaten nutzen?

Die Weiterleitung Ihrer Überwachungsdaten an einen Event Hub mit Azure Monitor ermöglicht eine einfache Integration in SIEM- und Überwachungstools von Partnern. Die meisten Tools benötigen die Event Hub-Verbindungszeichenfolge und bestimmte Berechtigungen für Ihr Azure-Abonnement zum Lesen von Daten aus dem Event Hub. Im Folgenden wird eine nicht erschöpfende Liste von Tools mit der Azure Monitor-Integration aufgeführt:

* **IBM QRadar**: Das DSM und Event Hub-Protokoll von Microsoft Azure sind zum Download auf der [Website des IBM-Supports](http://www.ibm.com/support) erhältlich. Weitere Informationen zur Integration in Azure finden Sie [hier](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk**: Abhängig vom Splunk-Setup gibt es zwei Ansätze:
    1. [Das Azure Monitor-Add-On für Splunk](https://splunkbase.splunk.com/app/3534/) steht in der Splunkbase und in einem Open Source-Projekt zur Verfügung. Die Dokumentation finden Sie [hier](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
    2. Wenn Sie in Ihrer Splunk-Instanz ein Add-On nicht installieren können (etwa bei Verwendung eines Proxys oder bei Ausführung in Splunk Cloud), können Sie diese Ereignisse an die HTTP-Ereignissammlung von Splunk weiterleiten. Verwenden Sie dazu [diese Funktion, die durch neue Nachrichten im Event Hub ausgelöst wird](https://github.com/sebastus/AzureFunctionForSplunkVS).
* **SumoLogic**: Anweisungen zum Einrichten von SumoLogic für die Nutzung von Daten aus einem Event Hub sind [hier](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub) verfügbar.

## <a name="next-steps"></a>Nächste Schritte
* [Archivieren Sie das Aktivitätsprotokoll mithilfe eines Speicherkontos.](monitoring-archive-activity-log.md)
* [Lesen Sie die Übersicht über das Azure-Aktivitätsprotokoll.](monitoring-overview-activity-logs.md)
* [Richten Sie eine Warnung ein, die auf einem Aktivitätsprotokollereignis basiert.](insights-auditlog-to-webhook-email.md)

