---
title: Near Real-Time Metric Alerts in Azure Monitor | Microsoft Docs
description: "Near Real-Time Metric Alerts ermöglichen Ihnen die minütliche Überwachung von Azure-Ressourcemetriken."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: snmuvva
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: aeeb6c2fb87e6c19991ef243ee7230f4e8f4e251
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="near-real-time-metric-alerts-preview"></a>Near Real-Time Metric Alerts (Preview)
Azure Monitor unterstützt jetzt einen neuen Typ von Metrikwarnungen, die als „Near Real-Time Metric Alerts (Preview)“ bezeichnet werden. Dieses Feature ist zurzeit als öffentliche Preview verfügbar.
Diese Warnungen unterscheiden sich in verschiedener Hinsicht von regulären Metrikwarnungen.

- **Verbesserte Latenz**: Near Real-Time Metric Alerts können Änderungen an Metrikwerten innerhalb einer Minute überwachen.
- **Mehr Kontrolle über Metrikbedingungen**: Near Real-Time Metric Alerts ermöglichen Benutzern, umfangreichere Warnungsregeln zu definieren. Die Warnungen unterstützen nun die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
- **Kombinierte Überwachung mehrerer Metriken**: Near Real-Time Metric Alerts können mehrere Metriken (zurzeit zwei Metriken) mit einer einzigen Regel überwachen. Die Warnung wird ausgelöst, wenn beide Metriken ihre jeweiligen Schwellenwerte für den angegebenen Zeitraum verletzen.
- **Modulares Benachrichtigungssystem**: Near Real-Time Metric Alerts verwenden [Aktionsgruppen](monitoring-action-groups.md). Diese Funktion ermöglicht Benutzern das Erstellen von Aktionen auf modulare Weise sowie die Wiederverwendung für viele Warnungsregeln.

> [!NOTE]
> Das Feature Real-Time Metric Alerts ist zurzeit als öffentliche Preview verfügbar. Die Funktionen und die Benutzeroberfläche können jederzeit geändert werden.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Für welche Ressourcen kann ich Real-Time Metric Alerts erstellen?
Vollständige Liste der Ressourcentypen, die von Real-Time Metric Alerts unterstützt werden:

* Microsoft.ApiManagement/service
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Sql/servers/elasticpools
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.Timeseriesinsights
* Microsoft.CognitiveServices/accounts


## <a name="create-a-near-real-time-metric-alert"></a>Erstellen eines Real-Time Metric Alerts
Zurzeit können Real-Time Metric Alerts nur über das Azure Portal erstellt werden. Unterstützung für die Konfiguration von Near Real-Time Metric Alerts über PowerShell, die Befehlszeilenschnittstelle (CLI) und die REST-API von Azure Monitor wird bald verfügbar sein.

1. Suchen Sie im [Portal](https://portal.azure.com/)die Ressource, die Sie überwachen möchten, und wählen Sie sie aus. Diese Ressource sollte einer der im [vorherigen Abschnitt](#what-resources-can-i-create-near-real-time-metric-alerts-for) aufgeführten Ressourcentypen sein. Sie können die gleiche Aktion auch für alle unterstützten Ressourcentypen zentral über „Überwachen“ > „Warnungen“ ausführen.

2. Wählen Sie im Abschnitt ÜBERWACHUNG **Warnungen** oder **Warnungsregeln** aus. Text und Symbol können je nach Ressource geringfügig variieren.
   ![Überwachung](./media/insights-alerts-portal/AlertRulesButton.png)

3. Klicken Sie auf den Befehl **Near Real-Time Metrics Alert (Preview) hinzufügen**. Wenn der Befehl abgeblendet ist, stellen Sie sicher, dass die Ressource im Filter ausgewählt ist.

    ![Schaltfläche „Near Real-Time Metrics Alert hinzufügen“](./media/monitoring-near-real-time-metric-alerts/AddNRTAlertButton.png)

4. **Benennen** Sie Ihre Warnungsregel, und wählen Sie eine **Beschreibung** aus, die auch in Benachrichtigungs-E-Mails angezeigt wird.
5. Wählen Sie die **Metrik** aus, die Sie überwachen möchten, und wählen Sie dann je einen Wert für die **Bedingung**, die **Zeitaggregation** und den **Schwellenwert** für die Metrik aus. Wählen Sie optional eine weitere **Metrik** aus, die Sie überwachen möchten, und wählen Sie dann je einen Wert für die **Bedingung**, die **Zeitaggregation** und den **Schwellenwert** für die zweite Metrik aus. 

    ![Hinzufügen von Near Real-Time Metrics Alert1](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert1.png) ![Hinzufügen von Near Real-Time Metrics Alert2](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert2.png)
6. Wählen Sie den **Zeitraum** aus, für den die Metrikregeln erfüllt sein müssen, bevor die Warnung ausgelöst wird. Wenn Sie z. B. den Zeitraum „Innerhalb der letzten fünf Minuten“ auswählen und die Warnung nach einer CPU-Auslastung von über 80 % (und NetworkIn über 500 MB) sucht, wird die Warnung ausgelöst, wenn die CPU-Auslastung 5 Minuten lang durchgängig über 80 % lag. Nachdem der erste Trigger ausgelöst wurde, erfolgt ein erneutes Auslösen, wenn die CPU-Auslastung 5 Minuten unter 80% bleibt. Die Warnung wird gemäß der **Auswertungshäufigkeit** ausgewertet.


6. Wählen Sie einen entsprechenden **Schweregrad** aus der Dropdownliste aus.

7. Geben Sie an, ob Sie eine neue oder eine vorhandene **Aktionsgruppe** verwenden möchten.

8. Wenn Sie eine **neue** Aktionsgruppe erstellen möchten, geben Sie der Aktionsgruppe einen Namen und einen Kurznamen, geben Sie Aktionen (SMS, E-Mail, Webhook) an, und geben Sie die entsprechenden Details ein.


8. Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.   

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="managing-near-real-time-metric-alerts"></a>Verwalten von Near Real-Time Metric Alerts
Nachdem Sie eine Warnung erstellt haben, können Sie sie auswählen und:

* ein Diagramm einblenden, das den Schwellenwert der Metrik und die tatsächlichen Werte vom Vortag zeigt.
* bearbeiten oder löschen.
* sie **deaktivieren** oder **aktivieren**, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.




