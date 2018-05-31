---
title: Auslösen komplexer Aktionen mit Azure Monitor-Warnungen und Aktionsgruppen | Microsoft Docs
description: Erfahren Sie, wie eine Aktion einer Logik-App zum Verarbeiten von Azure Monitor-Warnungen erstellt wird.
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
ms.date: 04/30/2018
ms.author: dukek
ms.openlocfilehash: 0020f1475d52d01897320062edbd3a66c8acf751
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33887450"
---
# <a name="create-a-logic-app-action"></a>Erstellen einer Logik-App-Aktion
## <a name="overview"></a>Übersicht ##
Wenn eine Azure Monitor-Warnung auslöst wird, wird eine [Aktionsgruppe](monitoring-action-groups.md) aufgerufen. Aktionsgruppen ermöglichen Ihnen das Auslösen von mindestens einer Aktion, um Personen über die Warnung zu benachrichtigen und diese sogar zu beseitigen.

Dieser Artikel zeigt, wie eine Logik-App eingerichtet und ausgelöst wird, um eine Konversation in Microsoft Teams zu erstellen, wenn eine Warnung ausgelöst wird.

Der allgemeine Vorgang besteht aus den folgenden Schritten:

-   Erstellen der Logik-App für den jeweiligen Warnungstyp

-   Importieren des Schemas für den jeweiligen Warnungstyp in die Logik-App

-   Definieren des Verhaltens der Logik-App

-   Kopieren des HTTP-Endpunkts der Logik-App in eine Azure-Aktionsgruppe

Der Vorgang ist ähnlich, wenn die Logik-App eine andere Aktion ausführen soll.

## <a name="create-an-activity-log-alert--administrative"></a>Erstellen einer Aktivitätsprotokollwarnung: Verwaltung

1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Ressource erstellen**.

2.  Suchen Sie nach **Logik-App**, und wählen Sie diese Option aus. Klicken Sie auf die Schaltfläche **Erstellen** .

3.  Benennen Sie die Logik-App, wählen Sie eine Ressourcengruppe aus usw.

    ![Dialogfeld „Erstellen“ der Logik-App](media/monitoring-action-groups/create-logic-app-dialog.png "Dialogfeld „Erstellen“ der Logik-App")

4.  Klicken Sie auf die Schaltfläche „Erstellen“, um die Logik-App zu erstellen. Nachdem die Logik-App erstellt wurde, wird ein Popupfenster angezeigt. Klicken Sie auf die Schaltfläche „Ressource starten“, um den Logik-App-Designer zu öffnen.

5.  Wählen Sie **Beim Empfang einer HTTP-Anforderung** als Trigger aus.

    ![Logik-App-Trigger](media/monitoring-action-groups/logic-app-triggers.png "Logik-App-Trigger")

6.  Wählen Sie **Bearbeiten** aus, um den HTTP-Anforderungstrigger zu ändern.

    ![Form des HTTP-Anforderungstriggers](media/monitoring-action-groups/http-request-trigger-shape.png "Form des HTTP-Anforderungstriggers")

7.  Wählen Sie **Beispielnutzlast zum Generieren eines Schemas verwenden** aus.

    ![Schaltfläche „Beispielnutzlast verwenden“](media/monitoring-action-groups/use-sample-payload-button.png "Schaltfläche „Beispielnutzlast verwenden“")

8.  Kopieren Sie das folgende Beispielschema, und fügen Sie es in das Dialogfeld ein.

    ```json
        {
            "schemaId": "Microsoft.Insights/activityLogs",
            "data": {
                "status": "Activated",
                "context": {
                "activityLog": {
                    "authorization": {
                    "action": "microsoft.insights/activityLogAlerts/write",
                    "scope": "/subscriptions/…"
                    },
                    "channels": "Operation",
                    "claims": "…",
                    "caller": "logicappdemo@contoso.com",
                    "correlationId": "91ad2bac-1afa-4932-a2ce-2f8efd6765a3",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-04-03T22:33:11.762469+00:00",
                    "eventDataId": "ec74c4a2-d7ae-48c3-a4d0-2684a1611ca0",
                    "level": "Informational",
                    "operationName": "microsoft.insights/activityLogAlerts/write",
                    "operationId": "61f59fc8-1442-4c74-9f5f-937392a9723c",
                    "resourceId": "/subscriptions/…",
                    "resourceGroupName": "LOGICAPP-DEMO",
                    "resourceProviderName": "microsoft.insights",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:33:36.1068742+00:00",
                    "resourceType": "microsoft.insights/activityLogAlerts"
                }
                },
                "properties": {}
            }
        }
    ```

9. Der Logik-App-Designer zeigt einen Hinweis an, der Sie daran erinnert, dass in der an die Logik-App gesendeten Anforderung der Content-Type-Header auf „application/json“ festgelegt werden muss. Schließen Sie dieses Dialogfeld. Die Azure Monitor-Warnung führt dies ordnungsgemäß aus.

    ![Content-Type-Header](media/monitoring-action-groups/content-type-header.png "Content-Type-Header")

10. Wählen Sie **+ Neuer Schritt** und anschließend **Aktion hinzufügen** aus.

    ![Aktion hinzufügen](media/monitoring-action-groups/add-action.png "Aktion hinzufügen")

11. Suchen Sie nach dem Microsoft Teams-Connector, und wählen ihn aus. Wählen Sie die Aktion **Microsoft Teams: Nachricht veröffentlichen** aus.

    ![Microsoft Teams-Aktionen](media/monitoring-action-groups/microsoft-teams-actions.png "Microsoft Teams-Aktionen")

12. Konfigurieren Sie die Microsoft Teams-Aktion. Der Logik-Apps-Designer fordert Sie zur Authentifizierung mit Ihrem Office 365-Konto auf. Wählen Sie die **Team-ID** und die **Kanal-ID** zum Senden der Nachricht aus.

13. Konfigurieren Sie die **Nachricht** mithilfe einer Kombination aus statischem Text und Verweisen auf die \<Felder\> im dynamischen Inhalt. Schneiden Sie den folgenden Text aus, und fügen Sie ihn in das Feld „Nachricht“ ein.

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Suchen Sie dann nach den \<Feldern\> mit dynamischen Inhaltstags mit dem gleichen Namen, und ersetzen Sie diese.

    **[HINWEIS]**: Es gibt zwei dynamische Felder mit dem Namen **Status**. Fügen Sie der Nachricht beide Statusfelder hinzu. Verwenden Sie das Feld im activityLog-Eigenschaftenbehälter, und löschen Sie das andere Feld. Wenn Sie Ihre Maus über dem Feld **Status** positionieren, wird der vollqualifizierte Feldverweis wie im Screenshot gezeigt angezeigt.

    ![Teams-Aktion: Nachricht veröffentlichen](media/monitoring-action-groups/teams-action-post-message.png "Teams-Aktion: Nachricht veröffentlichen")

14. Speichern Sie Ihre Logik-App, indem Sie auf die Schaltfläche „Speichern“ oben im Designer klicken.

15. Klicken Sie auf die Form „HTTP-Anforderung“, um sie zu erweitern. Kopieren Sie die HTTP POST-URL.

    ![HTTP POST-URL](media/monitoring-action-groups/http-post-url.png "HTTP POST-URL")

16. Öffnen Sie Ihre vorhandene Aktionsgruppe, und fügen Sie eine Aktion hinzu, um auf die Logik-App zu verweisen. Wenn noch keine Aktionsgruppe vorhanden ist, finden Sie unter <https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups> weitere Informationen zum Erstellen einer Aktionsgruppe. Vergessen Sie nicht, Ihre Änderungen zu speichern.

    ![Aktionsgruppe aktualisieren](media/monitoring-action-groups/update-action-group.png "Aktionsgruppe aktualisieren")

Das nächste Mal, wenn Ihre Aktionsgruppe durch eine Warnung aufgerufen wird, wird Ihre Logik-App aufgerufen.

## <a name="create-a-service-health-alert"></a>Erstellen einer Service Health-Warnung

Service Health-Einträge sind Teil des Aktivitätsprotokolls. Daher ist der Vorgang mit den folgenden Änderungen sehr ähnlich.

1.  Die Schritte 1 bis 7 sind identisch.
2.  Verwenden Sie das folgende Beispielschema für den HTTP-Trigger in Schritt 8.

    ```json
    {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "channels": "Admin",
                    "correlationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "description": "…",
                    "eventSource": "ServiceHealth",
                    "eventTimestamp": "2018-04-03T22:44:43.7467716+00:00",
                    "eventDataId": "9ce152f5-d435-ee31-2dce-104228486a6d",
                    "level": "Informational",
                    "operationName": "Microsoft.ServiceHealth/incident/action",
                    "operationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "properties": {
                        "title": "…",
                        "service": "…",
                        "region": "Global",
                        "communication": "…",
                        "incidentType": "Incident",
                        "trackingId": "…",
                        "impactStartTime": "2018-03-22T21:40:00.0000000Z",
                        "impactMitigationTime": "2018-03-22T21:41:00.0000000Z",
                        "impactedServices": "[{"ImpactedRegions"}]",
                        "defaultLanguageTitle": "…",
                        "defaultLanguageContent": "…",
                        "stage": "Active",
                        "communicationId": "11000001466525",
                        "version": "0.1.1"
                    },
                    "status": "Active",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:44:50.8013523+00:00"
                }
            },
            "properties": {}
        }
    }
    ```

3.  Die Schritte 9 bis 10 sind identisch.
4.  Verwenden Sie ab Schritt 11 die unten beschriebene Vorgehensweise.
5.  Klicken Sie auf die Schaltfläche **+ Neuer Schritt**, und wählen Sie **Bedingung hinzufügen** aus. Legen Sie die folgenden Bedingungen fest, um sicherzustellen, dass die Logik-App nur ausgeführt wird, wenn die Eingabedaten diesen Werten entsprechen.
    - schemaId == Microsoft.Insights/activityLogs
    - eventSource == ServiceHealth
    - version == 0.1.1

    ![„Bedingung Service Health-Nutzlast“](media/monitoring-action-groups/service-health-payload-condition.png "Bedingung Service Health-Nutzlast")

6. Fügen Sie in der Bedingung **if true** die Microsoft Teams-Aktion mit den Schritten 11 bis 13 aus dem vorherigen Beispiel hinzu.

7. Definieren Sie die Nachricht unter Verwendung einer Kombination von HTML und [dynamischem Inhalt]. Kopieren Sie den Inhalt unten, und fügen Sie ihn in das Nachrichtenfeld ein. Ersetzen Sie die Felder [IncidentType], [TrackingID], [title] und [communication] durch dynamische Inhaltstags mit dem gleichen Namen.

    ```html
    <p>
    <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
    <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
    <strong>Title:</strong>&nbsp;[title]</p>
    <p>
    <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard</a>
    </p>
    <p>[communication]</p>
    ```

    ![„Veröffentlichungsaktion von Service Health, wenn Bedingung TRUE ist“](media/monitoring-action-groups/service-health-true-condition-post-action.png "Veröffentlichungsaktion von Service Health, wenn Bedingung TRUE ist")

8. Geben Sie für die Bedingung **If false** eine sinnvolle Nachricht an.

    ```html
    <p><strong>Service Health Alert</strong></p>
    <p><b>Unrecognized alert schema</b></p>
    <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard\</a></p>
    ```

    ![„Veröffentlichungsaktion von Service Health, wenn Bedingung FALSE ist“](media/monitoring-action-groups/service-health-false-condition-post-action.png "Veröffentlichungsaktion von Service Health, wenn Bedingung FALSE ist")

9.  Führen Sie die Schritte 15 bis 16 des vorherigen Beispiels zum Speichern Ihrer Logik-App und Aktualisieren Ihrer Aktionsgruppe aus.

## <a name="metric-alert"></a>Metrikwarnung

1.  Die Schritte 1 bis 7 sind mit dem ersten Beispiel identisch.
2.  Verwenden Sie das folgende Beispielschema für den HTTP-Trigger in Schritt 8.

    ```json
    {
    "schemaId": "AzureMonitorMetricAlert",
    "data": {
        "version": "2.0",
        "status": "Activated",
        "context": {
        "timestamp": "2018-04-09T19:00:07.7461615Z",
        "id": "…",
        "name": "TEST-VM CPU Utilization",
        "description": "",
        "conditionType": "SingleResourceMultipleMetricCriteria",
        "condition": {
            "windowSize": "PT15M",
            "allOf": [
                {
                    "metricName": "Percentage CPU",
                    "dimensions": [
                    {
                        "name": "ResourceId",
                        "value": "d92fc5cb-06cf-4309-8c9a-538eea6a17a6"
                    }
                ],
                "operator": "GreaterThan",
                "threshold": "5",
                "timeAggregation": "PT15M",
                "metricValue": 1.0
            }
            ]
        },
        "subscriptionId": "…",
        "resourceGroupName": "TEST",
        "resourceName": "test-vm",
        "resourceType": "Microsoft.Compute/virtualMachines",
        "resourceId": "…",
        "portalLink": "…"
        },
        "properties": {}
    }
    }
    ```

3.  Die Schritte 9 bis 10 sind identisch.
4.  Verwenden Sie ab Schritt 11 die unten beschriebene Vorgehensweise.
5.  Klicken Sie auf die Schaltfläche **+ Neuer Schritt**, und wählen Sie **Bedingung hinzufügen** aus. Legen Sie die folgenden Bedingungen fest, um sicherzustellen, dass die Logik-App nur ausgeführt wird, wenn die Eingabedaten diesen Werten entsprechen.

    - schemaId == AzureMonitorMetricAlert
    - version == 2.0

    ![„Bedingung Metrikwarnungnutzlast“](media/monitoring-action-groups/metric-alert-payload-condition.png "Bedingung Metrikwarnungnutzlast")

6.  In der Bedingung **if true** fügen wir eine **For each**-Form und die Microsoft Teams-Aktion hinzu und definieren die Nachricht unter Verwendung einer Kombination aus HTML und [dynamischem Inhalt].

    ![„Veröffentlichungsaktion Metrikwarnung, wenn Bedingung TRUE ist“](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "Veröffentlichungsaktion Metrikwarnung, wenn Bedingung TRUE ist")

7.  Definieren Sie in der **If false**-Form eine Microsoft Teams-Aktion, die kommuniziert, dass die Metrikwarnung nicht mit den Erwartungen der Logik-App übereinstimmt, und schließen Sie die JSON-Nutzlast ein. Beachten Sie, wie wir auf den dynamischen triggerBody-Inhalt im json()-Ausdruck verweisen.

    ![„Veröffentlichungsaktion Metrikwarnung, wenn Bedingung FALSE ist“](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "Veröffentlichungsaktion Metrikwarnung, wenn Bedingung FALSE ist")

8.  Führen Sie die Schritte 15 bis 16 des ersten Beispiels zum Speichern Ihrer Logik-App und Aktualisieren Ihrer Aktionsgruppe aus.

## <a name="next-steps"></a>Nächste Schritte ##
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).
* Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)