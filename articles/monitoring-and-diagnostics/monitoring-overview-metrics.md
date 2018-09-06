---
title: Überblick über Metriken in Microsoft Azure
description: Überblick über Metriken und ihre Verwendung in Microsoft Azure
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d61ac48aa7c51bc4b215a7d56b1bbedfdc613f9f
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287555"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Überblick über Metriken in Microsoft Azure
In diesem Artikel wird beschrieben, was Metriken in Microsoft Azure sind, welche Vorteile sie haben und wie Sie mit deren Nutzung beginnen.  

## <a name="what-are-metrics"></a>Was sind Metriken?
Mit Azure Monitor können Sie Telemetriedaten verwenden, um sich einen Überblick über Leistung und Integrität Ihrer Workloads in Azure zu verschaffen. Die wichtigsten Typen von Telemetriedaten sind Metriken (auch Leistungsindikatoren genannt), die von den meisten Azure-Ressourcen ausgegeben werden. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese Metriken für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen.

## <a name="what-are-the-characteristics-of-metrics"></a>Was sind die Merkmale von Metriken?
Metriken weisen folgende Merkmale auf:

* Alle Metriken haben **eine Frequenz von einer Minute** (sofern nicht in der Definition einer Metrik anders angegeben). Jede Minute erhalten Sie von Ihrer Ressource einen Metrikwert, der Ihnen einen Überblick über den Zustand und die Integrität Ihrer Ressource nahezu in Echtzeit bietet.
* Metriken sind **sofort verfügbar**. Sie müssen sie nicht aktivieren oder zusätzliche Diagnosen einrichten.
* Sie können für jede Metrik auf einen **Verlauf von 93 Tagen** zugreifen. Sie können schnell den aktuellen und monatlichen Trend für die Leistung oder Integrität Ihrer Ressource überblicken.
* Einige Metriken können über Attribute für Name/Wert-Paare verfügen, die als **Dimensionen** bezeichnet werden. Dadurch können Sie eine Metrik auf sinnvollere Weise weiter segmentieren und untersuchen.

## <a name="what-can-you-do-with-metrics"></a>Wozu kann ich Metriken nutzen?
Mithilfe von Metriken können Sie die folgenden Aufgaben ausführen:


- Konfigurieren einer **Warnungsregel für eine Metrik, die eine Benachrichtigung sendet oder eine automatisierte Aktion ausführt**, sobald der Metrikwert einen von Ihnen festgelegten Schwellenwert überschreitet. Aktionen werden durch [Aktionsgruppen](monitoring-action-groups.md) gesteuert. Zu den Beispielen für Aktionen gehören E-Mail-, Telefon- und SMS-Benachrichtigungen, das Aufrufen eines Webhooks, das Starten eines Runbooks und mehr. **Automatisch skalieren** ist eine besondere automatisierte Aktion, die es Ihnen ermöglicht, Ihre Ressourcengruppe zur Verarbeitung von Lasten hoch- und herunterzuskalieren und zugleich die Kosten ohne Last gering zu halten. Sie können eine Regel für die automatische Skalierung konfigurieren, die das horizontale Hoch- und Herunterskalieren auslöst, sobald eine Metrik einen bestimmten Schwellenwert überschreitet.
- **Leiten** Sie alle Metriken an *Application Insights* oder *Log Analytics* weiter, um unmittelbare Analysen, Suchen und benutzerdefinierte Warnungen für von Ihren Ressourcen gesendete Metrikdaten zu ermöglichen. Sie können auch Metriken an einen *Event Hub* streamen, wodurch Sie sie dann an Azure Stream Analytics oder benutzerdefinierte Apps weiterleiten können, um Analysen nahezu in Echtzeit durchführen zu können. Event Hub-Streaming wird mit Diagnoseeinstellungen eingerichtet.
- **Archivieren** des Leistungs- oder Integritätsverlaufs Ihrer Ressourcen zu Kompatibilitäts-/Überwachungszwecken oder zur Offline-Berichterstellung.  Sie können Ihre Metriken an Azure Blob Storage weiterleiten, wenn Sie die Diagnoseeinstellungen für Ihre Ressource konfigurieren.
- Verwenden Sie das **Azure-Portal**, um alle Metriken zu entdecken, anzuzeigen und darauf zuzugreifen, wenn Sie eine Ressource auswählen und die Metriken in einem Diagramm darstellen. Sie können die Leistung Ihrer Ressourcen (z.B. virtuelle Computer, Websites oder Logik-Apps) nachverfolgen, indem Sie das betreffende Diagramm an Ihr Dashboard anheften.  
- **Ausführen fortgeschrittener Analysen** oder Erstellen von Berichten zu Leistungs- oder Nutzungstrends für Ihre Ressource.
- **Abfragen** von Metriken mithilfe von PowerShell-Cmdlets oder der plattformübergreifenden REST-API.
- **Nutzen** von Metriken über die neuen Azure Monitor-REST-APIs.

  ![Routing von Metriken in Azure Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Zugreifen auf Metriken über das Portal
Es folgt eine kurze exemplarische Vorgehensweise zum Erstellen eines Metrikdiagramm mithilfe des Azure-Portals.

### <a name="to-view-metrics-after-creating-a-resource"></a>So zeigen Sie Metriken nach Erstellen einer Ressource an
1. Öffnen Sie das Azure-Portal.
2. Erstellen Sie eine Azure App Service-Website.
3. Wechseln Sie nach dem Erstellen einer Website zum Blatt **Übersicht** der Website.
4. Auf der Kachel **Überwachung** können Sie neue Metriken anzeigen. Sie können die Kachel bearbeiten und weitere Metriken auswählen.

   ![Metriken für eine Ressource in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>So greifen Sie zentral auf alle Metriken zu
1. Öffnen Sie das Azure-Portal.
2. Navigieren Sie zur neuen Registerkarte **Überwachen**, und wählen Sie darunter die Option **Metriken** aus.
3. Wählen Sie in der Dropdownliste Ihr Abonnement, Ihre Ressourcengruppe und den Namen der Ressource aus.
4. Sehen Sie sich die Liste mit den verfügbaren Metriken an. Wählen Sie die Metrik aus, die Sie interessiert, und stellen Sie sie dar.
5. Durch Klicken auf den Stift in der rechten oberen Ecke können Sie sie an das Dashboard heften.

   ![Zugreifen auf alle Metriken von einer zentralen Stelle in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Sie können auf Metriken auf Hostebene von VMs (basierend auf Azure Resource Manager) und VM-Skalierungsgruppen aus zugreifen, ohne eine zusätzliche Diagnose einzurichten. Diese neuen Metriken auf Hostebene sind für Windows- und Linux-Instanzen verfügbar. Diese Metriken sind nicht zu verwechseln mit Metriken auf Gastbetriebssystemebene, auf die Sie zugreifen können, wenn Sie die Azure-Diagnose auf Ihren virtuellen Computern oder in Ihren VM-Skalierungsgruppen eingeschaltet haben. Weitere Informationen zum Konfigurieren der Diagnose finden Sie unter [Was ist Microsoft Azure-Diagnose?](../azure-diagnostics.md).
>
>

Azure Monitor verfügt über eine neue Darstellung der Metriken in Diagrammen, die in der Vorschauversion verfügbar ist. Dadurch wird es Benutzern ermöglicht, Metriken aus mehreren Ressourcen in einem Diagramm zu überlagern. Die Benutzer können mehrdimensionale Metriken ebenfalls darstellen, segmentieren und filtern, indem Sie die neuen Diagramme für Metriken verwenden. Klicken Sie [hier](https://aka.ms/azuremonitor/new-metrics-charts), um weitere Informationen zu erhalten.

## <a name="access-metrics-via-the-rest-api"></a>Zugreifen auf Metriken über die REST-API
Auf Azure-Metriken kann über Azure Monitor-APIs zugegriffen werden. Es gibt zwei APIs, mit denen Sie Metriken ermitteln und darauf zugreifen können:

* Verwenden Sie die [Azure Monitor-REST-API für Metrikdefinitionen](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) für den Zugriff auf die Liste von Metriken und Dimensionen, die für einen Dienst verfügbar sind.
* Verwenden Sie die [Azure Monitor-REST-API für Metriken](https://docs.microsoft.com/rest/api/monitor/metrics), um die tatsächlichen Metrikdaten zu segmentieren, zu filtern und auf diese zuzugreifen.

> [!NOTE]
> In diesem Artikel geht es um die Metriken, die über die [neue API für Metriken](https://docs.microsoft.com/rest/api/monitor/) für Azure-Ressourcen abgerufen werden können. Die API-Version für die neuen Metrikdefinitionen und Metrik-APIs lautet 2018-01-01. Auf die Legacy-Metrikdefinitionen und -Metriken kann über die API-Version 2014-04-01 zugegriffen werden.
>
>

Eine ausführliche exemplarische Vorgehensweise unter Verwendung der Azure Monitor-REST-APIs finden Sie unter [Exemplarische Vorgehensweise zur Azure Monitor-REST-API](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Exportieren von Metriken
Die Exportoptionen für Metriken können Sie auf der Registerkarte **Monitor** auf dem Blatt **Diagnoseprotokolle** anzeigen. Sie können Metriken (und Diagnoseprotokolle) zum Weiterleiten an Blob Storage, Azure Event Hubs oder Log Analytics für die Anwendungsfälle auswählen, die zuvor in diesem Artikel erwähnt wurden.

 ![Exportoptionen für Metriken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

Dies können Sie über Resource Manager-Vorlagen, [PowerShell](insights-powershell-samples.md), die [Azure CLI](insights-cli-samples.md) oder [REST-APIs](https://msdn.microsoft.com/library/dn931943.aspx) konfigurieren.

> [!NOTE]
> Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
>
> *Beispiel:* Die Metrik „Eingehende Nachrichten“ eines Event Hubs kann auf einer warteschlangenspezifischen Ebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.
>
>

## <a name="take-action-on-metrics"></a>Ausführen von Aktionen für Metriken
Um Benachrichtigungen zu empfangen oder aufgrund von Metrikdaten automatisierte Aktionen auszuführen, können Sie Warnungsregeln oder Einstellungen für die automatische Skalierung konfigurieren.

### <a name="configure-alert-rules"></a>Konfigurieren von Warnungsregeln
Sie können Warnungsregeln zu Metriken konfigurieren. Diese Warnungsregeln können überprüfen, ob eine Metrik einen bestimmten Schwellenwert überschritten hat. Es gibt zwei Funktionen für Metrikwarnungen, die von Azure Monitor bereitgestellt werden.

Metrikwarnungen: Diese können Sie per E-Mail benachrichtigen oder einen Webhook auslösen, der zum Ausführen eines benutzerdefinierten Skripts genutzt werden kann. Außerdem können Sie mit dem Webhook Integrationen von Drittanbieterprodukten konfigurieren.

 ![Metriken und Warnungsregeln in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

Neuere Metrikwarnungen können mehrere Metriken und Schwellenwerte für eine Ressource überwachen und Sie dann über eine [Aktionsgruppe](monitoring-action-groups.md) benachrichtigen. Weitere Informationen zu neueren Warnungen finden Sie [hier](https://aka.ms/azuremonitor/near-real-time-alerts).


### <a name="autoscale-your-azure-resources"></a>Automatische Skalierung Ihrer Azure-Ressourcen
Einige Azure-Ressourcen unterstützen das horizontale Hoch- oder Herunterskalieren mehrerer Instanzen, um Ihre Workloads zu bewältigen. Die automatische Skalierungsfunktion steht für App Services (Web-Apps), VM-Skalierungsgruppen und klassische Azure Cloud Services zur Verfügung. Sie können Regeln zur automatischen Skalierung konfigurieren, damit horizontal hoch- oder herunterskaliert wird, sobald eine bestimmte, für Ihre Workload relevante Metrik einen von Ihnen festgelegten Schwellenwert überschreitet. Weitere Informationen finden Sie in der [Übersicht über die automatische Skalierung](monitoring-overview-autoscale.md).

 ![Metriken und automatische Skalierung in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Informationen zu unterstützten Diensten und Metriken
Eine ausführliche Liste aller unterstützter Dienste mit den entsprechenden Metriken finden Sie unter [Azure Monitor-Metriken – unterstütze Metriken pro Ressourcentyp](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie auch über die Links in diesem Artikel. Außerdem erfahren Sie etwas zu folgenden Themen:  

* [Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md)
* [Erstellen von Warnungsregeln](insights-alerts-portal.md)
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)
