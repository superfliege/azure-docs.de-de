---
title: Erweitern von Warnungen aus OMS in Azure – Übersicht | Microsoft-Dokumentation
description: Übersicht über den Prozess zum Erweitern von Warnungen aus OMS in Azure-Warnungen sowie Details zu allgemeinen Kundenbedenken.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: 045a7f97d9c4d380e83325c04c209a6afcc761a7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="extend-alerts-from-oms-into-azure"></a>Erweitern von Warnungen aus OMS in Azure
Die neue Oberfläche für Warnungen umfasst nun die Warnungsanzeige übergreifend für verschiedene Dienste und Komponenten in Microsoft Azure. Die neue Oberfläche, die als **Warnungen** unter Azure Monitor im Portal zur Verfügung steht, führt an einer zentralen Stelle Aktivitätsprotokollwarnungen, Metrikwarnungen und Protokollwarnungen in Log Analytics sowie Application Insights zusammen. 

Bei einigen Benutzern erfolgt die Verwendung von Log Analytics und zugehörigen Funktionen wie Warnungen jedoch über [Microsoft Operation Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Um den Benutzern neben der Verwendung von Log Analytics eine einfache Verwaltung ihrer anderen Azure-Ressourcen zu ermöglichen, hat Microsoft systematisch sichergestellt, dass die Funktionen von OMS auch im Azure-Portal zur Verfügung stehen. In diesem Zusammenhang ermöglichen Azure-Warnungen den Benutzern bereits das Verwalten abfragebasierter Warnungen für Log Analytics. Weitere Informationen dazu finden Sie unter [Protokollwarnungen in Azure-Warnungen](monitor-alerts-unified-log.md). Auf der Benutzeroberfläche „Warnungen“ unter Azure Monitor werden in OMS erstellte Warnungen bereits unter dem entsprechenden Log Analytics-Arbeitsbereich aufgeführt. Doch zum Bearbeiten oder Ändern solcher in OMS erstellten Warnungen muss der Benutzer Azure verlassen und OMS verwenden. Anschließend muss er zu Azure zurückkehren, falls andere Dienste verwaltet werden müssen. Um dies zu vereinfachen, ermöglicht Microsoft den Benutzern jetzt das Erweitern ihrer Warnungen aus OMS in Azure.

## <a name="benefits-of-extending-your-alerts"></a>Vorteile des Erweiterns von Warnungen
Neben dem Vorteil, dass nicht mehr aus dem Azure-Portal hinaus navigiert werden muss, bietet das Erweitern von Warnungen aus OMS in Azure weitere wichtige Vorteile.

- Im Gegensatz zu OMS, wo nur 250 Warnungen erstellt und angezeigt werden können, weisen Azure-Warnungen diese Einschränkung nicht auf.
- Unter den Azure-Warnungen können alle Warnungstypen verwaltet, aufgelistet und angezeigt werden, nicht nur Log Analytics-Warnungen, wie es bei OMS der Fall ist.
- Azure-Warnungen verwenden [Aktionsgruppen](monitoring-action-groups.md), die es Ihnen ermöglichen, für jede Warnung über mehr als eine Aktion zu verfügen, einschließlich SMS, Sprachanruf, Automation-Runbook, Webhook, ITSM-Connector und mehr. In OMS hingegen sind Warnungen sowohl in Hinsicht auf die Anzahl als auch den Typ möglicher Aktionen beschränkt.

## <a name="process-of-extending-your-alerts"></a>Prozess zum Erweitern von Warnungen
Der Prozess zum Erweitern von Warnungen aus OMS in Azure beinhaltet **keinerlei** Änderung Ihrer Warnungsdefinition, -abfrage oder -konfiguration. Die einzige Änderung besteht darin, dass in Azure alle Aktionen (z.B. E-Mail-Benachrichtigung, Webhookaufruf, Ausführen des Automation-Runbooks oder Herstellen einer Verbindung mit dem ITSM Tool) über die Aktionsgruppe erfolgen. Wenn also Ihren Warnungen entsprechende Aktionsgruppen zugeordnet sind, werden diese in Azure erweitert.

Da der Prozess der Erweiterung weder destruktiv ist noch Unterbrechungen verursacht, führt Microsoft ab dem **23. April 2018** eine automatische Erweiterung der in OMS (Operations Management Suite) erstellten Warnungen in Azure-Warnungen durch. Ab diesem Tag beginnt Microsoft mit der Planung für das Erweitern von Warnungen in Azure und ermöglicht nach und nach die Verwaltung aller in OMS vorhandenen Warnungen über das Azure-Portal. 

Wenn für Warnungen in einem OMS-Arbeitsbereich die Erweiterung in Azure geplant ist, sind diese weiterhin funktionsfähig und beeinträchtigen **in keiner Weise** die Überwachung. Wenn Warnungen für die Erweiterung geplant sind, stehen diese möglicherweise vorübergehend nicht zur Änderung/Bearbeitung zur Verfügung, doch können innerhalb dieser kurzen Zeit weiterhin neue Azure-Warnungen erstellt werden. Falls in diesem kurzen Zeitraum eine Bearbeitung oder Erstellung von Warnungen aus OMS erfolgt, haben die Benutzer die Möglichkeit, in Azure Log Analytics oder Azure-Warnungen fortzufahren.

 ![Während des geplanten Zeitraums werden Benutzeraktionen für Warnungen an Azure umgeleitet](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Für das Erweitern von Warnungen aus OMS (Operations Management Suite) in Azure fallen keine Gebühren an, und die Nutzung von Azure-Warnungen für abfragebasierte Log Analytics-Warnungen wird nicht in Rechnung gestellt, sofern dies im Rahmen der in der [Preisrichtlinie für Azure Monitor](https://azure.microsoft.com/en-us/pricing/details/monitor/) angegebenen Grenzen und Bedingungen geschieht.  

Benutzer können die Vorteile der Erweiterung von Warnungen bereits vor diesem Datum nutzen, indem sie sich freiwillig für die Verwaltung ihrer Warnungen in Azure entscheiden.

### <a name="how-to-voluntarily-extending-your-alerts"></a>Freiwilliges Erweitern von Warnungen
Um OMS-Benutzern einen problemlosen Übergang in Azure-Warnungen zu ermöglichen, hat Microsoft Tools für eine Erweiterung der Warnungen erstellt. Microsoft OMS-Kunden (Operations Management Suite) können ihre Warnungen entweder über einen Assistenten im OMS-Portal (oder) anhand einer programmgesteuerten Vorgehensweise mithilfe einer neuen API in Azure erweitern. Weitere Informationen finden Sie unter [Erweitern von Warnungen in Azure mithilfe von OMS-Portal und API](monitoring-alerts-extend-tool.md).


## <a name="usage-after-extending-your-alerts"></a>Verwendung nach dem Erweitern von Warnungen
Wie bereits erwähnt, werden in Microsoft OMS erstellte Warnungen in Azure-Warnungen erweitert und können anschließend aus Azure verwaltet werden. Warnungen werden weiterhin im OMS-Portal im Abschnitt für die Warnungseinstellung aufgeführt. Dies ist im Folgenden dargestellt:

 ![OMS-Portal mit einer Auflistung von Warnungen nach dem Erweitern in Azure](./media/monitor-alerts-extend/PostExtendList.png)

Für jeden Vorgang, der im OMS-Portal für Warnungen ausgeführt wird (z.B. Bearbeiten oder Erstellen), werden die Benutzer auf transparente Weise zu Azure-Warnungen weitergeleitet. Das Erstellen von Warnungen erfolgt wie zuvor über die vorhandene [Log Analytics-API](../log-analytics/log-analytics-api-alerts.md) mit nur einer geringfügigen Änderung, dass nach dem Erweitern von Warnungen in Azure Aktionsgruppen im Zeitplan zugeordnet werden müssen.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die Tools für das [Initiieren der Erweiterung von Warnungen aus OMS in Azure](monitoring-alerts-extend-tool.md).
* Erfahren Sie mehr über die neue [Benutzeroberfläche „Azure-Warnungen“](monitoring-overview-unified-alerts.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen](monitor-alerts-unified-log.md).
