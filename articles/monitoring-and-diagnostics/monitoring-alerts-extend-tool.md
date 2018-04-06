---
title: Initiieren der Erweiterung von Warnungen aus OMS in Azure | Microsoft Docs
description: Die Tools und die API, mit denen Warnungen aus OMS in Azure-Warnungen erweitert werden, können von Kunden freiwillig ausgeführt werden.
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
ms.openlocfilehash: 76b7481223566f16a5da8c08d9d76f2bdb6b542a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="initiate-extending-alerts-from-oms-into-azure"></a>Initiieren der Erweiterung von Warnungen aus OMS in Azure
Ab **23. April 2018** werden die Warnungen aller Kunden, die in [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) konfiguriert sind, in Azure erweitert. Warnungen, die in Azure erweitert werden, verhalten sich auf die gleiche Weise wie in OMS. Die Überwachungsfunktionen bleiben intakt. Das Erweitern von Warnungen aus OMS in Azure bietet zahlreiche Vorteile. Weitere Informationen zu den Vorteilen und dem Vorgang der Erweiterung von Warnungen aus OMS in Azure finden Sie unter [Erweitern von Warnungen aus OMS in Azure](monitoring-alerts-extend.md).

Kunden, die ihre Warnungen sofort aus OMS in Azure verschieben möchten, können dies über eine der angegebenen Optionen erreichen.

## <a name="option-1---using-oms-portal"></a>Option 1: Verwenden des OMS-Portals
Um die Erweiterung von Warnungen aus OMS in Azure freiwillig zu initiieren, führen Sie die unten aufgeführten Schritte aus.

1. Navigieren Sie auf der Seite „Übersicht“ der Operations Management Suite (OMS) zum Abschnitt „Einstellungen“ und dann zu „Warnungen“. Klicken Sie wie in der Abbildung unten gezeigt auf die Schaltfläche „In Azure erweitern“.

    ![Seite „OMS-Warnungseinstellungen“ mit Option „Erweitern“](./media/monitor-alerts-extend/ExtendInto.png)

2. Nach dem Klicken auf die Schaltfläche wird ein Assistent mit drei Schritten angezeigt. Dabei stellt der erste Schritt die Details des Vorgangs bereit. Klicken Sie zum Fortfahren auf „Weiter“.

    ![Erweitern von Warnungen aus OMS in Azure: Schritt 1](./media/monitor-alerts-extend/ExtendStep1.png)

3. Im zweiten Schritt zeigt das System eine Zusammenfassung der vorgeschlagenen Änderung an, indem es die entsprechenden [Aktionsgruppen](monitoring-action-groups.md) für die Warnungen in OMS auflistet. Wenn ähnliche Aktionen über mehrere Warnungen hinweg auftreten, schlägt das System vor, allen eine einzige Aktionsgruppe zuzuordnen.  Die vorgeschlagene Aktionsgruppe folgt dieser Namenskonvention: *ArbeitsbereichName_AG_#Zahl*. Klicken Sie auf „Weiter“, um den Vorgang fortzusetzen.
Unten sehen Sie einen Beispielbildschirm.

    ![Erweitern von Warnungen aus OMS in Azure: Schritt 2](./media/monitor-alerts-extend/ExtendStep2.png)


4. Im letzten Schritt des Assistenten können Sie OMS auffordern, alle Ihre Warnungen in Azure zu erweitern, indem neue Aktionsgruppen erstellt und mit Warnungen verknüpft werden, wie im Screenshot weiter oben gezeigt. Zu Fortfahren klicken Sie auf „Fertig stellen“ und bestätigen an der Eingabeaufforderung, dass der Vorgang initiiert werden soll. Optional können Kunden auch E-Mail-Adressen angeben, an die OMS einen Bericht über den Abschluss der Verarbeitung senden soll.

    ![Erweitern von Warnungen aus OMS in Azure: Schritt 3](./media/monitor-alerts-extend/ExtendStep3.png)

5. Sobald der Assistent beendet wurde, wird die Steuerung zurück an die Seite mit den Warnungseinstellungen gegeben, und die Option „In Azure erweitern“ wird entfernt. Im Hintergrund plant OMS, dass Warnungen aus OMS in Azure erweitert werden. Dies kann einige Zeit in Anspruch nehmen, und wenn der Vorgang beginnt, stehen Warnungen in OMS für eine kurze Zeit nicht zur Änderung zur Verfügung. Der aktuelle Status wird über das Banner angezeigt, und wenn in Schritt 4 E-Mail-Adressen angegeben wurden, werden diese informiert, wenn der Hintergrundprozess alle Warnungen erfolgreich in Azure erweitert hat. 

6. Warnungen werden weiterhin in OMS aufgelistet, auch nachdem sie erfolgreich in Azure erweitert wurden.

    ![Nach dem Erweitern von Warnungen aus OMS in Azure](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>Option 2: Verwenden der API
Für Kunden, die den Vorgang des Erweiterns von Warnungen aus OMS in Azure programmgesteuert steuern oder automatisieren möchten, hat Microsoft eine neue AlertsVersion-API unter Log Analytics bereitgestellt.

Die AlertsVersion-API aus Log Analytics ist RESTful. Der Zugriff darauf erfolgt über die Azure Resource Manager-REST-API. In diesem Dokument finden Sie Beispiele, in denen über eine PowerShell-Befehlszeile mit [ARMClient](https://github.com/projectkudu/ARMClient) auf die API zugegriffen wird. Dies ist ein Open Source-Befehlszeilentool, mit dem das Aufrufen der Azure Resource Manager-API vereinfacht wird. Die Verwendung von ARMClient und PowerShell ist eine von vielen Möglichkeiten, auf die API zuzugreifen. Die API gibt Ergebnisse im JSON-Format aus, die programmgesteuert auf viele verschiedene Arten verwendet werden können.

Durch die Verwendung von GET mit der API können Sie im Ergebnis die Zusammenfassung der vorgeschlagenen Änderung als Liste geeigneter [Aktionsgruppen](monitoring-action-groups.md) für die Warnungen in OMS im JSON-Format erhalten. Wenn ähnliche Aktionen über mehrere Warnungen hinweg auftreten, schlägt das System vor, eine Aktionsgruppe zu erstellen und allen eine einzige Aktionsgruppe zuzuordnen.  Die vorgeschlagene Aktionsgruppe folgt dieser Namenskonvention: *ArbeitsbereichName_AG_#Zahl*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> Der Aufruf von GET für der API führt nicht dazu, dass Warnungen aus OMS in Azure erweitert werden. Er bietet nur als Ergebnis die Zusammenfassung der vorgeschlagenen Änderungen. Um zu bestätigen, dass diese Änderungen zum Erweitern von Warnungen in Azure vorgenommen werden sollen, muss ein POST-Aufruf der API erfolgen.

Wenn der GET-Aufruf der API erfolgreich ist, wird zusammen mit einer 200 OK-Antwort eine JSON-Liste der Warnungen und vorgeschlagenen Aktionsgruppen bereitgestellt. Die Abbildung unten zeigt eine Beispielantwort:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
Falls es keine Warnungen im angegebenen Arbeitsbereich gibt, wird zusammen mit der 200 OK-Antwort für den GET-Vorgang folgender JSON-Code zurückgegeben:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Wenn alle Warnungen im angegebenen Arbeitsbereich bereits in Azure erweitert wurden, würde die Antwort auf den GET-Aufruf folgendermaßen lauten:
```json
{
    "version": 2
}
```

Um die Planung der Erweiterung von Warnungen aus OMS in Azure zu starten, initiieren Sie einen POST-Aufruf der API. Durch diesen Aufruf/Befehl wird die Absicht des Benutzers bestätigt, seine Warnungen aus OMS in Azure zu erweitern und die Änderungen auszuführen, die als Antwort auf den GET-Aufruf der API angegeben werden. Optional kann der Benutzer eine Liste von E-Mail-Adressen angeben, an die OMS einen Bericht sendet, wenn der geplante Hintergrundprozess der Erweiterung der Warnungen aus OMS in Azure erfolgreich abgeschlossen wurde.

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Das Ergebnis der Erweiterung von OMS-Warnungen in Azure kann aufgrund von Änderungen im System von der von GET bereitgestellten Zusammenfassung abweichen. Nach der Planung sind Warnungen in OMS vorübergehend nicht mehr für die Bearbeitung/Änderung verfügbar, während neue Warnungen weiterhin erstellt werden können. 

Wenn der POST-Aufruf erfolgreich ist, wird eine 200 OK-Antwort zusammen mit der folgenden Angabe zurückgegeben:
```json
{
    "version": 2
}
```
Diese Antwort gibt an, dass die Warnungen in Azure erweitert wurden, wie „version 2“ besagt. Diese Versionsangabe dient nur zur Überprüfung, ob Warnungen in Azure erweitert wurden, und besitzt keinen Einfluss auf die Verwendung mit der [Log Analytics-Such-API](../log-analytics/log-analytics-api-alerts.md). Sobald die Warnungen erfolgreich in Azure erweitert wurden, erhalten alle Benutzer, die Administrator- und Mitwirkendenrollen im Arbeitsbereich zugeordnet sind, eine E-Mail mit Details zu den vorgenommenen Änderungen.


Wenn die Erweiterung aller Warnungen im angegebenen Arbeitsbereich in Azure bereits geplant wurde, ist die Antwort auf POST schließlich 403 Forbidden.


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die neue [Benutzeroberfläche „Azure-Warnungen“](monitoring-overview-unified-alerts.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen](monitor-alerts-unified-log.md).
