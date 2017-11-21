---
title: Konfigurieren von Azure Service Health-Warnungen in PagerDuty | Microsoft-Dokumentation
description: Erhalten Sie personalisierte Benachrichtigungen zu Service Health-Ereignissen an Ihre PagerDuty-Instanz.
author: shawntabrizi
manager: scotthit
editor: 
services: service-health
documentationcenter: service-health
ms.assetid: 
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 9edcb727b9f0af348cacd5533523c4f2e8214703
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="configure-service-health-alerts-with-pagerduty"></a>Konfigurieren von Service Health-Warnungen mit PagerDuty

In diesem Artikel erfahren Sie, wie Sie Azure Service Health-Benachrichtigungen über PagerDuty unter Verwendung eines Webhook einrichten. Mithilfe des benutzerdefinierten Microsoft Azure-Integrationstyps von [PagerDuty](https://www.pagerduty.com/) können Sie mühelos Service Health-Warnungen zu neuen oder vorhandenen PagerDuty-Diensten hinzufügen.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>Erstellen einer URL für die Service Health-Integration in PagerDuty
1.  Stellen Sie sicher, dass Sie Ihr [PagerDuty](https://www.pagerduty.com/)-Konto registriert haben und angemeldet sind.

2.  Navigieren Sie zum Abschnitt **Services** (Dienste) in PagerDuty.

    ![Der Abschnitt „Services“ in PagerDuty](./media/webhook-alerts/pagerduty-services-section.png)

3.  Wählen Sie **Add New Service** (Neuen Dienst hinzufügen) aus, oder öffnen Sie einen vorhandenen Dienst, den Sie eingerichtet haben.

4.  Wählen Sie unter **Integration Settings** (Integrationseinstellungen) folgendes Einstellungen aus:

    a. **Integration Type**: Microsoft Azure

    b. **Integration Name**: \<Name\>

    ![„Integration Settings“ in PagerDuty](./media/webhook-alerts/pagerduty-integration-settings.png)

5.  Füllen Sie alle weiteren Pflichtfelder aus, und wählen Sie **Add** (Hinzufügen) aus.

6.  Öffnen Sie diese neue Integration und kopieren und speichern Sie die Integrations-URL (**Integration URL**).

    ![„Integration URL“ in PagerDuty](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Erstellen einer Warnung mithilfe von PagerDuty im Azure-Portal
### <a name="for-a-new-action-group"></a>Für eine neue Aktionsgruppe:
1. Befolgen Sie die Schritte 1 bis 8 in [Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit dem Azure-Portal](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

2. Definieren Sie in der Liste der **Aktionen** Folgendes:

    a. **Action Type:** *Webhook*

    b. **Details:** Die zuvor gespeicherte PagerDuty-**Integrations-URL**.

    c. **Name:** Name, Alias oder Bezeichner des Webhook.

3. Wählen Sie **Save** (Speichern) aus, wenn das Erstellen der Warnung abgeschlossen ist.

### <a name="for-an-existing-action-group"></a>Für eine vorhandene Aktionsgruppe:
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Überwachen** aus.

2. Wählen Sie im Abschnitt **Einstellungen** die Option **Aktionsgruppen** aus.

3. Suchen und markieren Sie die Aktionsgruppe, die Sie bearbeiten möchten.

4. Fügen Sie Folgendes zur Liste der **Aktionen** hinzu:

    a. **Action Type:** *Webhook*

    b. **Details:** Die zuvor gespeicherte PagerDuty-**Integrations-URL**.

    c. **Name:** Name, Alias oder Bezeichner des Webhook.

5. Wählen Sie **Save** (Speichern) aus, wenn Sie mit dem Aktualisieren der Aktionsgruppe fertig sind.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen der Webhookintegration über eine HTTP POST-Anforderung
1. Erstellen Sie die Service Health-Nutzlast, die Sie senden möchten. Eine Service Health-Beispielwebhook-Nutzlast finden Sie unter [Webhooks für Azure-Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Erstellen Sie eine HTTP POST-Anforderung, indem Sie wie folgt vorgehen:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. Sie sollten `202 Accepted` mit einer Meldung erhalten, die die „Ereignis-ID“ enthält.

4. Wechseln Sie zu [PagerDuty](https://www.pagerduty.com/), um zu überprüfen, ob Ihre Integration erfolgreich eingerichtet wurde.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Weitere Informationen zu [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md).