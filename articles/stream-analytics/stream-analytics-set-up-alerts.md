---
title: Einrichten von Überwachungswarnungen für Azure Stream Analytics-Aufträge
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung und Warnungen für Azure Stream Analytics-Aufträge mithilfe des Azure-Portals einrichten.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: fb69f1b52e5b17dd6794c896969c29003a929c9b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-set-up-alerts-for-azure-stream-analytics-jobs"></a>Tutorial: Einrichten von Warnungen für Azure Stream Analytics-Aufträge
Sie können Warnungen einrichten, die eine Warnung auslösen, wenn eine Metrik eine von Ihnen angegebene Bedingung erreicht. Beispielsweise können Sie eine Warnung für eine Bedingung wie die folgende einrichten:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Regeln können für Metriken über das Portal eingerichtet oder [programmgesteuert](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) über Vorgangsprotokolldaten konfiguriert werden.

## <a name="set-up-alerts-in-the-azure-portal"></a>Einrichten von Warnungen im Azure-Portal
1. Öffnen Sie im Azure-Portal den Stream Analytics-Auftrag, für den Sie eine Warnung erstellen möchten. 

2. Klicken Sie auf dem Blatt **Auftrag** auf den Abschnitt **Überwachung**.  

3. Klicken Sie auf dem Blatt **Metrik** auf den Befehl **Warnung hinzufügen**.

      ![Azure-Portal, „Einrichtung“](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Geben Sie einen Namen und eine Beschreibung ein.

5. Verwenden Sie die Selektoren, um die Bedingung zu definieren, unter der die Warnung gesendet wird.

6. Geben Sie an, wohin die Warnung gesendet werden soll.

      ![Einrichten einer Warnung für einen Azure Stream Analytics-Auftrag](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Weitere Informationen zum Konfigurieren von Warnungen im Azure-Portal finden Sie unter [Empfangen von Warnbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

