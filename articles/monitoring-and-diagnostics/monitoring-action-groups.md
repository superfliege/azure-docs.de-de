---
title: Erstellen und Verwalten von Aktionsgruppen im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: dukek
ms.openlocfilehash: a1f163acea4e1965ab90b32e23e502b13f8908be
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
## <a name="overview"></a>Übersicht ##
In diesem Artikel wird beschrieben, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.

Sie können eine Liste von Aktionen mit Aktionsgruppen konfigurieren. Diese Gruppen können Sie dann bei jeder Warnung wiederverwenden, die Sie definieren, um sicherzustellen, dass jedes Mal, wenn eine Warnung ausgelöst wird, die gleichen Aktionen ausgeführt werden.

Jede Aktion besteht aus den folgenden Eigenschaften:

* **Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe.  
* **Aktionstyp**: einen Sprachanruf oder eine SMS senden, eine E-Mail senden, einen Webhook aufrufen, Daten an ein ITSM-Tool senden, eine Logik-App aufrufen, eine Pushbenachrichtigung an die Azure-App senden oder ein Automation-Runbook ausführen.
* **Details**: Die entsprechende Telefonnummer oder E-Mail-Adresse, die entsprechenden Details zur ITSM-Verbindung oder der entsprechende Webhook-URI.

Weitere Informationen zum Verwenden von Azure Resource Manager-Vorlagen zur Konfigurierung von Aktionsgruppen finden Sie unter [Aktionsgruppen-Resource Manager-Vorlagen](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Erstellen einer Aktionsgruppe mit dem Azure-Portal ##
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Überwachen**. Das Blatt **Überwachen** konsolidiert alle Ihre Überwachungseinstellungen und Daten in einer Ansicht.

    ![Der Dienst „Überwachen“](./media/monitoring-action-groups/home-monitor.png)
2. Wählen Sie im Abschnitt **Einstellungen** die Option **Aktionsgruppen** aus.

    ![Registerkarte „Aktionsgruppen“](./media/monitoring-action-groups/action-groups-blade.png)
3. Wählen Sie **Aktionsgruppe hinzufügen**, und füllen Sie die Felder aus.

    ![Der Befehl „Aktionsgruppe hinzufügen“](./media/monitoring-action-groups/add-action-group.png)
4. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

      ![Das Dialogfeld „Aktionsgruppe hinzufügen“](./media/monitoring-action-groups/action-group-define.png)

5. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingetragen. In diesem Abonnement wird die Aktionsgruppe gespeichert.

6. Wählen Sie die **Ressourcengruppe**, in der die Aktionsgruppe gespeichert wird.

7. Definieren Sie eine Liste von Aktionen, indem Sie für jede Aktion Folgendes angeben:

    a. **Name**: Geben Sie einen eindeutigen Bezeichner für diese Aktion an.

    b. **Aktionstyp**: Wählen Sie E-Mail/SMS/Push/Sprachanruf, Logik-App, Webhook, ITSM oder Automation-Runbook aus.

    c. **Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse, einen Webhook-URI, eine Azure-App, eine ITSM-Verbindung oder ein Automation-Runbook an. Legen Sie für die ITSM-Aktion darüber hinaus **Arbeitselement** und andere Felder fest, die Ihr ITSM-Tool benötigt.

8. Wählen Sie **OK**, um die Aktionsgruppe zu erstellen.

## <a name="action-specific-information"></a>Aktionsspezifische Informationen
<dl>
<dt>Azure-App-Push</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn Azure-App-Aktionen enthalten.</dd>
<dd>Derzeit werden für Azure-App-Aktionen nur ServiceHealth-Warnungen unterstützt. Alle anderen Warnungen werden ignoriert. Weitere Informationen finden Sie unter [Konfigurieren von Warnungen, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).</dd>

<dt>
E-Mail</dt>
<dd>Eine Aktionsgruppe kann bis zu 50 E-Mail-Aktionen enthalten.</dd>
<dd>Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./monitoring-alerts-rate-limiting.md).</dd>

<dt>ITSM</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn ITSM-Aktionen enthalten.</dd>
<dd>Für eine ITSM-Aktion muss eine ITSM-Verbindung hergestellt werden. Informieren Sie sich, wie Sie [eine ITSM-Verbindung erstellen](../log-analytics/log-analytics-itsmc-overview.md).</dd>

<dt>
Logik-App</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn Logik-App-Aktionen enthalten.</dd>

<dt>Runbook</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn Runbookaktionen enthalten.</dd>

<dt>SMS</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn SMS-Aktionen enthalten.</dd>
<dd>Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./monitoring-alerts-rate-limiting.md).</dd>
<dd>Weitere Informationen finden Sie im Artikel [SMS-Warnungsverhalten in Aktionsgruppen](monitoring-sms-alert-behavior.md).</dd>

<dt>
Sprachanruf</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn Sprachanrufaktionen enthalten.</dd>
<dd>Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./monitoring-alerts-rate-limiting.md).</dd>

<dt>Webhook</dt>
<dd>Eine Aktionsgruppe kann bis zu zehn Webhookaktionen enthalten.
<dd>Wiederholungslogik: Es wird maximal zweimal versucht, den Webhookaufruf durchzuführen, wenn die HTTP-Statuscodes 408, 429, 503 oder 504 zurückgegeben werden oder der HTTP-Endpunkt nicht reagiert. Der erste Wiederholungsversuch erfolgt nach 10 Sekunden. Der zweite und letzte Wiederholungsversuch erfolgt nach 100 Sekunden.</dd>
</dl>

## <a name="manage-your-action-groups"></a>Verwalten von Aktionsgruppen ##
Nachdem Sie eine Aktionsgruppe erstellt haben, wird diese im Abschnitt **Aktionsgruppen** des Diensts **Überwachen** angezeigt. Wählen Sie die Aktionsgruppe, die Sie verwalten möchten, um Folgendes zu tun:

* Fügen Sie Aktionen hinzu, bearbeiten oder entfernen Sie diese.
* Löschen der Aktionsgruppe.

## <a name="next-steps"></a>Nächste Schritte ##
* Erfahren Sie mehr über das [SMS-Warnungsverhalten in Aktionsgruppen](monitoring-sms-alert-behavior.md).  
* Erweitern Sie Ihr [Verständnis des Webhookschemas für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).  
* Erfahren Sie mehr über den [ITSM-Connector](../log-analytics/log-analytics-itsmc-overview.md).
* Weitere Informationen zu [Ratenlimits](monitoring-alerts-rate-limiting.md) für Warnungen.
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).
