---
title: Integrieren von Protokollen aus Azure-Ressourcen in Ihre SIEM-Systeme | Microsoft-Dokumentation
description: Informationen zu Azure Log Integration, zu den wichtigsten Funktionen und zur Funktionsweise dieses Diensts.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6d91692a64a4d3def80990a439fe0a0898bf2f09
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-azure-log-integration"></a>Einführung in Azure Log Integration

Mit Azure Log Integration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren. Verwenden Sie Azure Log Integration nur, wenn Ihr SIEM-Anbieter keinen Connector für [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) zur Verfügung stellt.

Die bevorzugte Methode für die Integration von Azure-Protokollen ist die Verwendung des Azure Monitor-Connectors Ihres SIEM-Anbieters. Anweisungen zur Verwendung des Connectors finden Sie unter [Streamen von Überwachungsdaten an Event Hubs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). 

Wenn jedoch der SIEM-Anbieter keinen Connector für Azure Monitor anbietet, können Sie Azure Log Integration möglicherweise als temporäre Lösung verwenden, bis ein Connector verfügbar ist. Die Verwendung von Azure Log Integration ist nur möglich, wenn Azure Log Integration Ihr SIEM-System unterstützt.

> [!IMPORTANT]
> Wenn Sie hauptsächlich Protokolle für virtuelle Computer erfassen möchten, wird dies in den Lösungen der meisten SIEM-Anbieter abgedeckt. Das Verwenden des Connectors des SIEM-Anbieters ist immer die bevorzugte Alternative.

Azure Log Integration erfasst Windows-Ereignisse aus Protokollen der Windows-Ereignisanzeige, aus [Azure-Aktivitätsprotokollen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), aus [Azure Security Center-Warnungen](../security-center/security-center-intro.md) und aus [Azure-Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) von Azure-Ressourcen. Durch die Integration kann in Ihrer SIEM-Lösung ein einheitliches Dashboard für alle Ihre Objekte – lokal oder in der Cloud – bereitgestellt werden. In einem Dashboard können Sie Warnungen für Sicherheitsereignisse empfangen, aggregieren, korrelieren und analysieren.

> [!NOTE]
> Derzeit unterstützt Azure Log Integration nur Azure Commercial Clouds und Azure Government Clouds. Andere Clouds werden nicht unterstützt.

![Abbildung des Azure Log Integration-Prozesses][1]

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle kann ich integrieren?

Azure führt für jeden Azure-Dienst eine umfassende Protokollierung durch. Die Protokolle lassen sich in drei Protokolltypen unterteilen:

* **Steuerungs-/Verwaltungsprotokolle:** ermöglichen Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Ein Beispiel für diesen Protokolltyp sind Azure-Aktivitätsprotokolle.
* **Datenebenenprotokolle:** ermöglichen Einblicke in die Ereignisse, die bei Verwendung einer Azure-Ressource ausgelöst werden. Ein Beispiel für diesen Protokolltyp sind die Kanäle **System**, **Sicherheit** und **Anwendung** der Windows-Ereignisanzeige auf einem virtuellen Windows-Computer. Ein weiteres Beispiel ist die Azure-Diagnoseprotokollierung, die über Azure Monitor konfiguriert wird.
* **Verarbeitete Ereignisse:** bieten analysierte Ereignis- und Benachrichtigungsinformationen, die für Sie verarbeitet werden. Ein Beispiel für diesen Ereignistyp sind Azure Security Center-Warnungen. Azure Security Center verarbeitet und analysiert Ihr Abonnement und stellt dann Warnungen bereit, die für den aktuellen Sicherheitsstatus relevant sind.

Die Azure-Protokollintegration unterstützt ArcSight, QRadar und Splunk. Erkundigen Sie sich bei Ihrem SIEM-Anbieter nach einem nativen Connector. Verwenden Sie Azure Log Integration nicht, wenn ein nativer Connector verfügbar ist.

Wenn keine anderen Optionen zur Verfügung stehen, kann Azure Log Integration in Betracht gezogen werden. Die folgende Tabelle enthält unsere Empfehlungen:

|SIEM | Kunde verwendet bereits den Azure Log Integrator | Kunde prüft die SIEM-Integrationsoptionen|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Migration zum [Azure Monitor-Add-On für Splunk](https://splunkbase.splunk.com/app/3534/) starten | [Splunk-Connector](https://splunkbase.splunk.com/app/3534/) verwenden |
|**QRadar** | Zum QRadar-Connector, der im letzten Abschnitt von [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) dokumentiert ist, migrieren oder diesen verwenden | Den QRadar-Connector, der im letzten Abschnitt von [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) dokumentiert ist, verwenden |
|**ArcSight** | Weiterhin den Azure Log Integrator verwenden, bis ein Connector verfügbar ist. Dann zur Connector-basierten Lösung migrieren.  | Verwendung von Azure Log Analytics als Alternative in Betracht ziehen. Führen Sie das Onboarding für Azure Log Integration nur aus, wenn Sie bereit sind, die Migration durchzuführen, nachdem der Connector verfügbar ist. |

> [!NOTE]
> Obwohl Azure Log Integration eine kostenlose Lösung ist, entstehen durch Speichern der Protokolldateiinformationen entsprechende Azure-Speicherkosten.

Wenn Sie Unterstützung benötigen, können Sie eine [Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md) erstellen. Wählen Sie für diesen Dienst die Option **Protokollintegration** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird Azure Log Integration vorgestellt. Weitere Informationen zu Azure Log Integration und den unterstützten Protokolltypen finden Sie in folgenden Artikeln:

* [Erste Schritte mit Azure Log Integration](security-azure-log-integration-get-started.md). In diesem Tutorial wird die Installation von Azure Log Integration Schritt für Schritt erläutert. Zudem wird beschrieben, wie Protokolle aus dem Microsoft Azure-Diagnosespeicher (WAD-Speicher) sowie Azure-Aktivitätsprotokolle, Azure Security Center-Warnungen und Azure Active Directory-Überwachungsprotokolle integriert werden.
* [Azure Log Integration – häufig gestellte Fragen](security-azure-log-integration-faq.md). Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure Log Integration.
* Erhalten Sie weitere Informationen zum [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
