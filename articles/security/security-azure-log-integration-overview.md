---
title: Integrieren von Protokollen aus Azure-Ressourcen in Ihre SIEM-Systeme | Microsoft-Dokumentation
description: Sie erhalten Informationen zur Azure-Protokollintegration, zu den wichtigsten Funktionen und dazu, wie dieses Feature funktioniert.
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
ms.openlocfilehash: e427e2f7dafb6db9bc5c15d841fbbf83a02fc0e1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Einführung in die Microsoft Azure-Protokollintegration

Mit Azure Log Integration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren, falls Ihr SIEM-Anbieter noch keinen Connector für [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) anbietet.

Die bevorzugte Methode für die Integration von Azure-Protokollen ist der Azure Monitor-Connector Ihres SIEM-Anbieters unter Verwendung dieser [Anweisungen](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). Wenn jedoch der SIEM-Anbieter keinen Connector für Azure Monitor anbietet, können Sie Azure Log Integration möglicherweise als eine temporäre Lösung verwenden (sofern Ihr SIEM von Azure Log Integration unterstützt wird), bis ein solcher Connector verfügbar ist.

>[!IMPORTANT]
>Wenn Ihr Hauptinteresse das Erfassen von Protokollen für virtuelle Computer ist, wird dies von den Lösungen der meisten SIEM-Anbieter abgedeckt. Der Connector des SIEM-Anbieters sollte immer die bevorzugte Alternative sein.

Die Azure-Protokollintegration erfasst Windows-Ereignisse aus Protokollen der Windows-Ereignisanzeige, aus [Azure-Aktivitätsprotokollen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), aus [Azure Security Center-Warnungen](../security-center/security-center-intro.md) und aus [Azure-Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) von Azure-Ressourcen. Diese Integration unterstützt Ihre SIEM-Lösung beim Bereitstellen eines einheitlichen Dashboards für alle Ihre Assets (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

>[!NOTE]
Zu diesem Zeitpunkt sind die einzigen unterstützten Clouds Azure Commercial und Azure Government. Andere Clouds werden nicht unterstützt.

![Azure-Protokollintegration][1]

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle kann ich integrieren?

Azure führt für jeden Azure-Dienst eine umfassende Protokollierung durch. Diese Protokolle stellen drei Arten von Protokollen dar:

* **Steuerungs-/Verwaltungsprotokolle** ermöglichen Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Ein Beispiel für diesen Protokolltyp sind Azure-Aktivitätsprotokolle.
* **Datenebenenprotokolle** ermöglichen Einblicke in die Ereignisse, die bei Verwendung einer Azure-Ressource auftreten. Ein Beispiel für diesen Protokolltyp sind die Kanäle **System**, **Sicherheit** und **Anwendung** der Windows-Ereignisanzeige auf einem virtuellen Windows-Computer. Ein weiteres Beispiel ist das über Azure Monitor konfigurierte Diagnoseprotokoll.
* **Verarbeitete Ereignisse** bieten analysierte Ereignis- und Benachrichtigungsinformationen, die in Ihrem Auftrag verarbeitet werden. Ein Beispiel für diese Art von Ereignis sind Azure Security Center-Warnungen, bei denen Azure Security Center Ihr Abonnement verarbeitet und analysiert hat, um Warnungen bereitzustellen, die für Ihren aktuellen Sicherheitsstatus relevant sind.

Die Azure-Protokollintegration unterstützt ArcSight, QRadar und Splunk. Erkundigen Sie sich in jedem Fall bei Ihrem SIEM-Anbieter nach einem nativen Connector. Sie sollten Azure Log Integration nicht verwenden, wenn native Connectors verfügbar sind.

Wenn keine anderen Optionen zur Verfügung stehen, kann Azure Log Integration in Betracht gezogen werden. Die folgende Tabelle enthält unsere Empfehlungen.

|**SIEM** | **Kunden, die Log Integrator bereits verwenden** | **Kunden, die SIEM-Integrationsoptionen untersuchen**|
|---------|--------------------------|-------------------------------------------|
|**SPLUNK** | Migration zum [Azure Monitor-Add-On für Splunk](https://splunkbase.splunk.com/app/3534/) starten | [SPLUNK-Connector](https://splunkbase.splunk.com/app/3534/) verwenden |
|**IBM QRADAR** | Zum QRadar-Connector, der am Ende von „http://aka.ms/azmoneventhub“ dokumentiert ist, migrieren oder ihn verwenden | Den QRadar-Connector, der am Ende von „http://aka.ms/azmoneventhub“ dokumentiert ist, verwenden  |
|**ARCSIGHT** | Weiterhin den Log Integrator verwenden, bis ein Connector verfügbar ist, und dann zur Connector-basierten Lösung migrieren  | Azure Log Analytics als Alternative in Betracht ziehen. Führen Sie das Onboarding für Azure Log Integration nur aus, wenn Sie bereit sind, die Migration durchzuführen, sobald der Connector verfügbar ist. |

>[!NOTE]
>Während die Azure-Protokollintegration eine kostenlose Lösung darstellt, entstehen durch Speichern der Protokolldateiinformationen entsprechende Azure-Speicherkosten.

Wenn Sie Unterstützung benötigen, stellen Sie eine [Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md). Wählen Sie zu diesem Zweck **Protokollintegration** als Dienst aus, für den Sie Support anfordern.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde die Azure-Protokollintegration vorgestellt. Weitere Informationen zur Azure-Protokollintegration und den unterstützten Protokolltypen finden Sie hier:

* [Erste Schritte mit der Azure-Protokollintegration:](security-azure-log-integration-get-started.md) In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure-WAD-Speicher sowie Azure-Aktivitätsprotokolle, Azure Security Center-Warnungen und Azure Active Directory-Überwachungsprotokolle integrieren.
* [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
* [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
