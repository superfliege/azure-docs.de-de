---
title: Empfangen von Aktivitätsprotokollwarnungen zu Azure-Dienstbenachrichtigungen
description: Lassen Sie sich per SMS, E-Mail oder Webhook benachrichtigen, wenn Ereignisse beim Azure-Dienst eintreten.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.component: alerts
ms.openlocfilehash: 1fcbbce87c822c49882f8e42a1eaccfd389ee310
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852927"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen
## <a name="overview"></a>Übersicht
Dieser Artikel zeigt, wie Sie Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen im Azure-Portal einrichten.  

Sie können eine Warnung erhalten, wenn Azure Benachrichtigungen zur Dienstintegrität an Ihr Azure-Abonnement sendet. Sie können die Warnung konfigurieren auf der Grundlage von:

- Der Klasse der Dienstintegritätsbenachrichtigung (Dientsprobleme, Geplante Wartung, Integritätsempfehlungen).
- Dem betroffenen Abonnement.
- Den betroffenen Diensten.
- Den betroffenen Regionen.

> [!NOTE]
> Dienstintegritätsbenachrichtigungen senden keine Warnung bezüglich Ereignissen der Ressourcenintegrität.

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:

- Wählen Sie eine vorhandene Aktionsgruppe aus.
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md).

Informationen zum Konfigurieren von Dienstintegritätsbenachrichtigungs-Warnungen mithilfe von Azure Resource Manager-Vorlagen siehe [Resource Manager-Vorlagen](alert-activity-log.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit dem Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Dienstintegrität** aus.

    ![Der Dienst „Dienstintegrität“](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

1. Wählen Sie im Abschnitt **Warnungen** die Option **Integritätswarnungen** aus.

    ![Die Registerkarte „Integritätswarnungen“](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

1. Wählen Sie **Dienstintegritätswarnung erstellen** aus, und füllen Sie die Felder aus.

    ![Der Befehl „Dienstintegritätswarnung erstellen“](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

1. Wählen Sie das **Abonnement**, die **Dienste** und die **Regionen** für Benachrichtigungen aus.

    ![Das Dialogfeld „Aktivitätsprotokollwarnung hinzufügen“](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Dieses Abonnement wird verwendet, um die Aktivitätsprotokollwarnung zu speichern. Die Warnungsressource wird für dieses Abonnement bereitgestellt und überwacht hierfür Ereignisse im Aktivitätsprotokoll.

1. Wählen Sie die **Ereignistypen** für Benachrichtigungen aus: *Dienstproblem*, *Geplante Wartung* und *Integritätsempfehlungen* 

1. Definieren Sie die Warnungsdetails durch Eingeben eines **Namens für die Warnungsregel** und einer **Beschreibung**.

1. Wählen Sie die **Ressourcengruppe** aus, in der die Warnung gespeichert werden soll.

1. Erstellen Sie eine neue Aktionsgruppe, indem Sie **Neue Aktionsgruppe** auswählen. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Auf den kurzen Namen wird in den Benachrichtigungen verwiesen, die gesendet werden, wenn diese Warnung ausgelöst wird.

    ![Erstellen einer neuen Aktionsgruppe](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

1. Definieren Sie eine Liste von Empfängern, indem Sie folgende Daten der Empfänger angeben:

    a. **Name:** Geben Sie Name, Alias oder Bezeichner des Empfängers ein.

    b. **Aktionstyp**: Wählen Sie „SMS“, „E-Mail“, „Webhook“ „Azure-App“ usw. aus.

    c. **Details**: Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, eine E-Mail-Adresse, einen Webhook-URI usw. ein.

1. Wählen Sie **OK** aus, um die Aktionsgruppe zu erstellen, und dann **Warnungsregel erstellen**, um die Warnung abzuschließen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](../service-health/service-health-alert-webhook-guide.md). Informationen über das Webhookschema für Aktivitätsprotokollwarnungen finden Sie unter [Webhook für Azure-Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine vorhandene Aktionsgruppe mit dem Azure-Portal

1. Führen Sie die Schritte 1 bis 7 im vorherigen Abschnitt aus, um Ihre Dienstintegritätsbenachrichtigung zu erstellen. 

1. Klicken Sie unter **Aktionsgruppe definieren** auf die Schaltfläche **Aktionsgruppe auswählen**. Wählen Sie die entsprechende Aktionsgruppe aus.

1. Wählen Sie **Hinzufügen** aus, um die Aktionsgruppe hinzuzufügen, und dann **Warnungsregel erstellen**, um die Warnung abzuschließen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-resource-manager-templates"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit den Azure Resource Manager-Vorlagen

Nachfolgend sehen Sie ein Beispiel, das eine Aktionsgruppe mit einem E-Mail-Ziel erstellt und alle Dienstintegritätsbenachrichtigungen für das Zielabonnement aktiviert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
    },
    "resources": [
        {
            "comments": "Action Group",
            "type": "microsoft.insights/actionGroups",
            "name": "[parameters('actionGroups_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "groupShortName": "[parameters('actionGroups_name')]",
                "enabled": true,
                "emailReceivers": [
                    {
                        "name": "[parameters('actionGroups_name')]",
                        "emailAddress": "[parameters('emailAddress')]"
                    }
                ],
                "smsReceivers": [],
                "webhookReceivers": []
            },
            "dependsOn": []
        },
        {
            "comments": "Service Health Activity Log Alert",
            "type": "microsoft.insights/activityLogAlerts",
            "name": "[parameters('activityLogAlerts_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "scopes": [
                    "[variables('alertScope')]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
                            "webhookProperties": {}
                        }
                    ]
                },
                "enabled": true,
                "description": ""
            },
            "dependsOn": [
                "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
            ]
        }
    ]
}
```

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen

Nachdem Sie eine Warnung erstellt haben, wird sie unter **Überwachen** im Abschnitt **Warnungen** angezeigt. Wählen Sie die Warnung, die Sie verwalten möchten, um Folgendes zu tun:

* Sie bearbeiten.
* Sie löschen.
* Deaktivieren oder aktivieren Sie sie, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](../service-health/service-health-alert-webhook-guide.md).
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).
- Erfahren Sie mehr über [Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks](monitoring-alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).
- Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können. 
- Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md).
