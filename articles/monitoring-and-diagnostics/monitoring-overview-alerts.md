---
title: Überblick über klassische Warnungen in Microsoft Azure und Microsoft Azure Monitor
description: Mit Warnungen können Sie Metriken, Ereignisse oder Protokolle für Ihre Azure-Ressourcen überwachen. Lassen Sie sich benachrichtigen, wenn eine von Ihnen festgelegte Bedingung erfüllt ist.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: a0abcbdaa7e998413efb717be6e0addc5607ec5c
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114010"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Was sind klassische Warnungen in Microsoft Azure?

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie ältere klassische Metrikwarnungen erstellen. Azure Monitor unterstützt nun [neuere Metrikwarnungen, die nahezu in Echtzeit stattfinden, und eine neue Oberfläche für Warnungen](monitoring-overview-unified-alerts.md). 
>

Durch die Verwendung von Warnungen können Sie Bedingungen für Daten konfigurieren und benachrichtigt werden, wenn die Bedingungen Übereinstimmungen mit den aktuellen Überwachungsdaten ergeben.


## <a name="alerts-on-azure-monitor-data"></a>Warnungen zu Azure Monitor-Daten
Es sind zwei Arten von klassischen Warnungen verfügbar: Metrikwarnungen und Aktivitätsprotokollwarnungen.

* **Klassische Metrikwarnungen**: Diese Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert überschreitet. Die Warnung generiert eine Benachrichtigung, wenn sie „aktiviert“ ist (wenn der Schwellenwert überschritten wird und die Warnbedingung erfüllt ist). Es wird außerdem eine Benachrichtigung generiert, wenn die Warnung „gelöst“ ist (wenn der Schwellenwert erneut überschritten wird und die Warnbedingung nicht mehr erfüllt ist). 

* **Klassische Aktivitätsprotokollwarnungen**: Diese Streamingprotokollwarnung wird ausgelöst, wenn ein Aktivitätsprotokollereignis generiert wird, das von Ihnen zugewiesene Filterkriterien erfüllt. Diese Warnungen haben nur einen Status: „Aktiviert“. Dies liegt daran, dass die Warnungs-Engine die Filterkriterien einfach auf ein neues Ereignis anwendet. Diese Warnungen können Sie darüber informieren, dass ein neuer Service Health-Vorfall eintritt oder ein Benutzer oder eine Anwendung in Ihrem Abonnement einen Vorgang durchführt, z.B. „Virtuellen Computer löschen“.

Um Diagnoseprotokolldaten zu erhalten, die über Azure Monitor verfügbar sind, leiten Sie die Daten in Log Analytics (ehemals Operations Management Suite) weiter und verwenden Sie eine Log Analytics-Abfragewarnung. Log Analytics verwendet jetzt die [neue Warnmethode](monitoring-overview-unified-alerts.md). 

Das folgende Diagramm enthält eine Zusammenfassung der Quellen von Daten in Azure Monitor und erläutert, wie Sie Warnungen aus diesen Daten erzeugen können.

![Warnungen im Überblick](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Taxonomie von Azure Monitor-Warnungen (klassisch)
In Azure werden die folgenden Begriffe verwendet, um klassische Warnungen und ihre Funktionen zu beschreiben:
* **Warnung**: Die Definition von Kriterien (mindestens eine Regel oder Bedingung), die aktiviert wird, wenn die Kriterien erfüllt sind.
* **Aktiv**: Der Status, der auftritt, wenn die für eine klassische Warnung definierten Kriterien erfüllt sind.
* **Gelöst**: Der Status, der auftritt, wenn die für eine klassische Warnung definierten Kriterien nicht mehr erfüllt sind, nachdem dies zuvor der Fall war.
* **Benachrichtigung**: Die Aktion, die durchgeführt wird, wenn eine klassische Warnung aktiv wird.
* **Aktion**: Der spezifische Aufruf, der an den Empfänger einer Benachrichtigung gesendet wird (z.B. eine E-Mail oder eine Nachricht an eine Webhook-URL). Benachrichtigungen können in der Regel mehrere Aktionen auslösen.

## <a name="how-do-i-receive-notifications-from-an-azure-monitor-classic-alert"></a>Wie erhalte ich Benachrichtigungen von einer klassischen Azure Monitor-Warnung?
Bisher wurden für Azure-Warnungen unterschiedlicher Dienste die eigenen integrierten Benachrichtigungsmethoden verwendet. 

In Azure Monitor gibt es eine wiederverwendbare Benachrichtigungsgruppierung mit der Bezeichnung *Aktionsgruppen*. Aktionsgruppen geben einen Reihe von Empfängern für eine Benachrichtigung an. Wenn eine Warnung aktiviert wird, die auf die Aktionsgruppe verweist, erhalten alle Empfänger diese Benachrichtigung. Durch diese Funktionalität ist es möglich, eine Gruppierung von Empfängern (z.B. Ihre Liste mit Technikern in Bereitschaft) für viele Warnungsobjekte wiederzuverwenden. Aktionsgruppen unterstützen Benachrichtigungen über verschiedene Methoden. Diese Methoden können Nachrichten an eine Webhook-URL, das Versenden von E-Mails, SMS-Nachrichten und eine Reihe anderer Aktionen umfassen. Weitere Informationen erhalten Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md). 

Ältere klassische Aktivitätsprotokollwarnungen verwenden Aktionsgruppen.

Für die älteren Metrikwarnungen werden jedoch keine Aktionsgruppen verwendet. Sie können stattdessen die folgenden Aktionen konfigurieren: 
* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator, an Co-Administratoren oder an zusätzliche von Ihnen festgelegte E-Mail-Adressen
* Aufrufen eines Webhooks, um zusätzliche automatisierte Aktionen auszuführen

Webhooks ermöglichen die Automatisierung und Fehlerbehebung, z.B. mit folgenden Diensten:
- Azure Automation-Runbook
- Azure-Funktionen
- Azure-Logik-App
- Ein Drittanbieterdienst

## <a name="next-steps"></a>Nächste Schritte
Informationen über Warnregeln und deren Konfiguration finden Sie in der folgenden Dokumentation:

* Weitere Informationen zu [Metriken](monitoring-overview-metrics.md)
* Konfigurieren von [klassischen Metrikwarnungen über das Azure-Portal](insights-alerts-portal.md)
* Konfigurieren von [klassischen Metrikwarnungen mit PowerShell](insights-alerts-powershell.md)
* Konfigurieren von [klassischen Metrikwarnungen über die Azure-Befehlszeilenschnittstelle](insights-alerts-command-line-interface.md)
* Konfigurieren von [klassischen Metrikwarnungen über die Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Weitere Informationen zu [Aktivitätsprotokollen](monitoring-overview-activity-logs.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über das Azure-Portal](monitoring-activity-log-alerts.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen mit Ressourcen-Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
* Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)
* Konfigurieren [neuerer Warnungen](monitor-alerts-unified-usage.md)
