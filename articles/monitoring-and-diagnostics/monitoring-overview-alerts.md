---
title: Überblick über klassische Warnungen in Microsoft Azure und Microsoft Azure Monitor| Microsoft-Dokumentation
description: Mit Warnungen können Sie Metriken, Ereignisse oder Protokolle für Ihre Azure-Ressourcen überwachen. Lassen Sie sich benachrichtigen, wenn eine von Ihnen festgelegte Bedingung erfüllt ist.
author: rboucher
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: robb
ms.openlocfilehash: 039ab7226b4ea7e011e1c0cbb4cac528b5237483
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204004"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Was sind klassische Warnungen in Microsoft Azure?

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie ältere klassische Metrikwarnungen erstellen. Azure Monitor unterstützt nun [neuere Metrikwarnungen, die nahezu in Echtzeit stattfinden, und eine neue Oberfläche für Warnungen](monitoring-overview-unified-alerts.md). 
>

Mit Warnungen können Sie Bedingungen für Daten konfigurieren und benachrichtigt werden, wenn die Bedingungen Übereinstimmungen mit den aktuellen Überwachungsdaten ergeben.


## <a name="alerts-on-azure-monitor-data"></a>Warnungen zu Azure Monitor-Daten
Es sind zwei Arten von klassischen Warnungen verfügbar – Metrikwarnungen und Aktivitätsprotokollwarnungen.

* **Klassische Metrikwarnungen**: Diese Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert überschreitet. Die Warnung generiert eine Benachrichtigung, wenn die Warnung den Status „Aktiviert“ (nach Überschreitung des Schwellenwerts und Erfüllung der Warnungsbedingung) oder „Gelöst“ (nach der erneuten Überschreitung des Schwellenwerts und Nichterfüllung der Bedingung) hat. Neuere Metrikwarnungen finden Sie im Folgenden.

* **Klassische Aktivitätsprotokollwarnungen**: Eine Streamingprotokollwarnung, die ausgelöst wird, wenn ein Aktivitätsprotokollereignis generiert wird, das von Ihnen zugewiesene Filterkriterien erfüllt. Diese Warnungen haben nur einen Status: „Aktiviert“. Dies liegt daran, dass die Warnungs-Engine die Filterkriterien einfach auf ein neues Ereignis anwendet. Diese Warnungen können zum Empfangen von Benachrichtigungen für den Fall verwendet werden, dass ein neuer Service Health-Vorfall eintritt oder ein Benutzer oder eine Anwendung in Ihrem Abonnement einen Vorgang durchführt, z.B. „Virtuellen Computer löschen“.

Für Diagnoseprotokolldaten, die über Azure Monitor verfügbar sind, empfehlen wir das Weiterleiten der Daten in Log Analytics (ehemals OMS) und das Verwenden einer Log Analytics-Abfragewarnung. Log Analytics verwendet jetzt die [neue Warnmethode](monitoring-overview-unified-alerts.md) 

Das folgende Diagramm enthält eine Zusammenfassung der Quellen von Daten in Azure Monitor und des Konzepts zum Erzeugen von Warnungen aus diesen Daten.

![Warnungen im Überblick](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Taxonomie von Azure Monitor-Warnungen (klassisch)
In Azure werden die folgenden Begriffe verwendet, um klassische Warnungen und ihre Funktionen zu beschreiben:
* **Warnung**: Eine Definition mit Kriterien (mindestens eine Regel oder Bedingung), die aktiviert wird, wenn die Kriterien erfüllt sind.
* **Aktiv**: Der Status, wenn die für eine klassische Warnung definierten Kriterien erfüllt sind.
* **Gelöst**: Der Status, wenn die für eine klassische Warnung definierten Kriterien nicht mehr erfüllt sind, nachdem dies zuvor der Fall war.
* **Benachrichtigung**: Die Aktion, die durchgeführt wird, wenn eine klassische Warnung aktiv wird.
* **Aktion**: Ein bestimmte Kontaktaufnahme, die an den Empfänger einer Benachrichtigung gesendet wird (z.B. E-Mail an eine Adresse oder das Posten an eine Webhook-URL). Benachrichtigungen können in der Regel mehrere Aktionen auslösen.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Wie erhalte ich eine Benachrichtigung von einer klassischen Azure Monitor-Warnung?
Bisher wurden für Azure-Warnungen unterschiedlicher Dienste die eigenen integrierten Benachrichtigungsmethoden verwendet. 

In Azure Monitor gibt es jetzt eine wiederverwendbare Benachrichtigungsgruppierung mit der Bezeichnung *Aktionsgruppen*. Mit Aktionsgruppen wird eine Reihe von Empfängern für eine Benachrichtigung angegeben, und Jedes Mal, wenn eine Warnung aktiviert wird, die auf die Aktionsgruppe verweist, erhalten alle Empfänger eine Benachrichtigung. Auf diese Weise ist es möglich, eine Gruppierung von Empfängern (z.B. Ihre Liste mit Technikern in Bereitschaft) für viele Warnungsobjekte wiederzuverwenden. Für die Aktionsgruppen werden Benachrichtigungen unterstützt, indem zusätzlich zu E-Mail-Adressen, SMS-Telefonnummern und einer Reihe anderer Aktionen eine Webhook-URL bereitgestellt wird.  Weitere Informationen finden Sie unter [Aktionsgruppen](monitoring-action-groups.md). 

Ältere klassische Aktivitätsprotokollwarnungen verwenden Aktionsgruppen.

Für die älteren Metrikwarnungen werden jedoch keine Aktionsgruppen verwendet. Sie können stattdessen die folgenden Aktionen konfigurieren: 
* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator, an Co-Administratoren oder an zusätzliche von Ihnen festgelegte E-Mail-Adressen
* Aufrufen eines Webhooks, um zusätzliche automatisierte Aktionen auszuführen

Webhooks ermöglichen die Automatisierung und Fehlerbehebung, z.B. mit folgenden Komponenten:
    - Azure Automation-Runbook
    - Azure Function
    - Azure-Logik-App
    - ein Drittanbieter-Dienst

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über Warnregeln und ihre Konfiguration erhalten Sie hier:

* Weitere Informationen zu [Metriken](monitoring-overview-metrics.md)
* Konfigurieren von [klassischen Metrikwarnungen über das Azure-Portal](insights-alerts-portal.md)
* Konfigurieren von [klassischen Metrikwarnungen mit PowerShell](insights-alerts-powershell.md)
* Konfigurieren von [klassischen Metrikwarnungen über die Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
* Konfigurieren von [klassischen Metrikwarnungen mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Weitere Informationen zu [Aktivitätsprotokollen](monitoring-overview-activity-logs.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über das Azure-Portal](monitoring-activity-log-alerts.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über den Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
* Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)
* Konfigurieren [neuerer Warnungen](monitor-alerts-unified-usage.md)
