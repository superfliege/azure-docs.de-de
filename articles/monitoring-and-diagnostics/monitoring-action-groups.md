---
title: Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
description: Erfahren Sie, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 1b66f40594d0dd578e2e5680fb0d5bb0f82f1241
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311805"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
## <a name="overview"></a>Übersicht ##
Eine Aktionsgruppe ist eine Sammlung von Benachrichtigungseinstellungen, die vom Benutzer definiert wurden. Warnungen von Azure Monitor und Service Health sind beim Auslösen der Warnung für die Verwendung einer bestimmten Aktionsgruppe konfiguriert. Verschiedene Warnungen können je nach den Bedürfnissen des Benutzers die gleiche Aktionsgruppe oder verschiedene Aktionsgruppen verwenden.

In diesem Artikel wird beschrieben, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.

Jede Aktion besteht aus den folgenden Eigenschaften:

* **Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe.  
* **Aktionstyp**: die auszuführende Aktion. Beispiele sind das Senden eines Sprachanrufs, SMS, E-Mail oder das Auslösen verschiedener Arten von automatisierten Aktionen. Siehe „Typen“ weiter unten in diesem Artikel. 
* **Details**: die entsprechenden Details, die je nach *Aktionstyp* variieren können. 

Weitere Informationen zum Verwenden von Azure Resource Manager-Vorlagen zur Konfigurierung von Aktionsgruppen finden Sie unter [Aktionsgruppen-Resource Manager-Vorlagen](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Erstellen einer Aktionsgruppe mit dem Azure-Portal ##
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Überwachen**. Das Blatt **Überwachen** konsolidiert alle Ihre Überwachungseinstellungen und Daten in einer Ansicht.

    ![Der Dienst „Überwachen“](./media/monitoring-action-groups/home-monitor.png)
1. Wählen Sie **Warnungen** und dann **Aktionsgruppen verwalten** aus.

    ![Schaltfläche „Aktionsgruppen verwalten“](./media/monitoring-action-groups/manage-action-groups.png)
1. Wählen Sie **Aktionsgruppe hinzufügen**, und füllen Sie die Felder aus.

    ![Der Befehl „Aktionsgruppe hinzufügen“](./media/monitoring-action-groups/add-action-group.png)
1. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

      ![Das Dialogfeld „Aktionsgruppe hinzufügen“](./media/monitoring-action-groups/action-group-define.png)

1. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingetragen. In diesem Abonnement wird die Aktionsgruppe gespeichert.

1. Wählen Sie die **Ressourcengruppe**, in der die Aktionsgruppe gespeichert wird.

1. Definieren Sie eine Liste von Aktionen, indem Sie für jede Aktion Folgendes angeben:

    a. **Name**: Geben Sie einen eindeutigen Bezeichner für diese Aktion an.

    b. **Aktionstyp**: Wählen Sie E-Mail/SMS/Push/Sprachanruf, Logik-App, Webhook, ITSM oder Automation-Runbook aus.

    c. **Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse, einen Webhook-URI, eine Azure-App, eine ITSM-Verbindung oder ein Automation-Runbook an. Legen Sie für die ITSM-Aktion darüber hinaus **Arbeitselement** und andere Felder fest, die Ihr ITSM-Tool benötigt.

1. Wählen Sie **OK**, um die Aktionsgruppe zu erstellen.

## <a name="manage-your-action-groups"></a>Verwalten von Aktionsgruppen ##
Nachdem Sie eine Aktionsgruppe erstellt haben, wird diese im Abschnitt **Aktionsgruppen** des Diensts **Überwachen** angezeigt. Wählen Sie die Aktionsgruppe, die Sie verwalten möchten, um Folgendes zu tun:

* Fügen Sie Aktionen hinzu, bearbeiten oder entfernen Sie diese.
* Löschen der Aktionsgruppe.

## <a name="action-specific-information"></a>Aktionsspezifische Informationen
**Azure-App-Push**: eine Aktionsgruppe kann bis zu zehn Azure-App-Aktionen enthalten. Derzeit werden für Azure-App-Aktionen nur ServiceHealth-Warnungen unterstützt. Alle anderen Warnungen werden ignoriert. Weitere Informationen finden Sie unter [Konfigurieren von Warnungen, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).

**E-Mail**: E-Mails werden von den folgenden E-Mail-Adressen gesendet. Achten Sie darauf, dass Ihre E-Mail-Filterung ordnungsgemäß konfiguriert ist.
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

Eine Aktionsgruppe kann bis zu 1.000 E-Mail-Aktionen enthalten. Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./monitoring-alerts-rate-limiting.md).

**ITSM**: Eine Aktionsgruppe kann bis zu zehn ITSM-Aktionen enthalten. Für eine ITSM-Aktion ist eine ITSM-Verbindung erforderlich. Informieren Sie sich, wie Sie [eine ITSM-Verbindung erstellen](../log-analytics/log-analytics-itsmc-overview.md).

**Logik-App**: Eine Aktionsgruppe kann bis zu zehn Logik-App-Aktionen enthalten.

**Funktions-App:** Die für Funktions-Apps als Aktionen konfigurierten Funktionsschlüssel werden über die Functions-API gelesen, für die derzeit Funktions-Apps der Version 2 erforderlich sind, um „files“ für die App-Einstellung „AzureWebJobsSecretStorageType“ zu konfigurieren. Weitere Informationen finden Sie unter [Changes to Key Management in Functions V2]( https://aka.ms/funcsecrets) (Änderungen der Schlüsselverwaltung in Functions V2).

**Runbook**: Eine Aktionsgruppe kann bis zu zehn Runbookaktionen enthalten. Informationen zu den Einschränkungen für Runbook-Nutzlasten, die für Protokollwarnungen über Azure-Aktionsgruppen ausgelöst werden, finden Sie unter [Azure-Abonnementdienstgrenzen](../azure-subscription-service-limits.md).

**SMS**: Eine Aktionsgruppe kann bis zu zehn SMS-Aktionen enthalten. Weitere Informationen finden Sie in den Artikeln [Informationen zu Ratenbegrenzungen](./monitoring-alerts-rate-limiting.md) und [Verhalten von SMS-Benachrichtigungen](monitoring-sms-alert-behavior.md).

**Sprachanruf**: Eine Aktionsgruppe kann bis zu zehn Sprachanrufaktionen enthalten.</dd>
Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./monitoring-alerts-rate-limiting.md).</dd>

**Webhook**: Eine Aktionsgruppe kann bis zu zehn Webhookaktionen enthalten. Wiederholungslogik – Die Timeoutperiode für eine Antwort beträgt 10 Sekunden. Es wird maximal zweimal versucht, den Webhookaufruf durchzuführen, wenn die HTTP-Statuscodes 408, 429, 503 oder 504 zurückgegeben werden oder der HTTP-Endpunkt nicht reagiert. Der erste Wiederholungsversuch erfolgt nach 10 Sekunden. Der zweite und letzte Wiederholungsversuch erfolgt nach 100 Sekunden.

Quell-IP-Adressbereiche
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Um Updates über Änderungen an diesen IP-Adressen zu erhalten, empfehlen wir Ihnen, eine [Service Health-Warnung](./monitoring-service-notifications.md) zu konfigurieren, die eine Überwachung auf Informationsbenachrichtigungen des Aktionsgruppendiensts ausführt.


## <a name="next-steps"></a>Nächste Schritte ##
* Erfahren Sie mehr über das [SMS-Warnungsverhalten in Aktionsgruppen](monitoring-sms-alert-behavior.md).  
* Erweitern Sie Ihr [Verständnis des Webhookschemas für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).  
* Erfahren Sie mehr über den [ITSM-Connector](../log-analytics/log-analytics-itsmc-overview.md).
* Weitere Informationen zu [Ratenlimits](monitoring-alerts-rate-limiting.md) für Warnungen.
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).
