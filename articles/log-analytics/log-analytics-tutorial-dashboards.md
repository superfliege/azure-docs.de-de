---
title: Erstellen und Freigeben von Dashboards von Azure Log Analytics-Daten | Microsoft-Dokumentation
description: "In diesem Tutorial wird beschrieben, wie in Log Analytics-Dashboards alle gespeicherten Protokollsuchvorgänge visualisiert werden können, um Ihnen eine zentrale Übersicht über Ihre Umgebung zu ermöglichen."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: b065269a27ad3764399802b4a2bc3076b7349623
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Erstellen und Freigeben von Dashboards von Log Analytics-Daten

Log Analytics-Dashboards können alle Ihre gespeicherten Protokollsuchen visualisieren und Ihnen so die Möglichkeit geben, IT-Betriebsdaten in der Organisation zu suchen, korrelieren und freizugeben.  Dieses Tutorial behandelt das Erstellen einer Protokollsuche, die ein freigegebenes Dashboard unterstützt, auf das Ihr IT-Betriebssupportteam zugreift.  Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines freigegebenen Dashboards im Azure-Portal
> * Visualisieren einer Leistungsprotokollsuche 
> * Hinzufügen einer Protokollsuche zu einem freigegebenen Dashboard 
> * Anpassen einer Kachel in einem freigegebenen Dashboard

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein, der [mit dem Log Analytics-Arbeitsbereich verbunden](log-analytics-quick-collect-azurevm.md) ist.  
 
## <a name="log-in-to-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) im Azure-Portal an. 

## <a name="create-a-shared-dashboard"></a>Erstellen eines freigegebenen Dashboards

Nachdem Sie sich beim [Microsoft Azure-Portal](../azure-portal/azure-portal-dashboards.md) angemeldet haben, wird Ihnen als Erstes ein Dashboard angezeigt.<br> ![Dashboard des Azure-Portals](media/log-analytics-tutorial-dashboards/log-analytics-portal-dashboard.png)

Hier können Sie für all Ihre Azure-Ressourcen die Betriebsdaten zusammenstellen, die für die IT-Abteilung am wichtigsten sind, einschließlich Telemetriedaten aus Azure Log Analytics.  Bevor wir die Visualisierung einer Protokollsuche durchführen, erstellen wir zuerst ein Dashboard und geben es frei.  Dann ist dies schon erledigt, bevor wir unser Leistungsprotokollsuche-Beispiel durchführen, denn dies liefert uns ein Liniendiagramm und fügt es dem Dashboard hinzu.  

Wählen Sie zum Erstellen eines Dashboards die Schaltfläche **Neues Dashboard** neben dem Namen des aktuellen Dashboards aus.<br> ![Erstellen eines neuen Dashboards im Azure-Portal](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-01.png)

Bei dieser Aktion wird ein neues, leeres, privates Dashboard erstellt. Außerdem wird der Anpassungsmodus aktiviert, in dem Sie dem Dashboard einen Namen geben und Kacheln hinzufügen oder neu anordnen können. Bearbeiten Sie den Namen des Dashboards, geben Sie *Beispieldashboard* für dieses Tutorial ein, und wählen Sie dann **Anpassung abgeschlossen**.<br><br> ![Speichern des benutzerdefinierten Azure-Dashboards](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-02.png)

Wenn Sie ein Dashboard erstellen, ist es standardmäßig ein privates Dashboard. Dies bedeutet, dass Sie die einzige Person sind, die es anzeigen kann. Verwenden Sie die Schaltfläche **Freigeben**, die neben den anderen Dashboardbefehlen angezeigt wird, um es auch für andere Benutzer sichtbar zu machen.<br> ![Freigeben eines neuen Dashboards im Azure-Portal](media/log-analytics-tutorial-dashboards/log-analytics-share-dashboard.png) 

Sie werden aufgefordert, für die Veröffentlichung des Dashboards ein Abonnement und eine Ressourcengruppe auszuwählen. Während der Veröffentlichung über das Portal werden Sie der Einfachheit halber zu einem Muster geführt, bei dem Sie Dashboards in einer Ressourcengruppe mit dem Namen **Dashboards**anordnen.  Überprüfen Sie das ausgewählte Abonnement, und klicken Sie dann auf **Veröffentlichen**.  Der Zugriff auf die im Dashboard angezeigten Informationen wird mit [Azure Resource Based Access Control](../active-directory/role-based-access-control-configure.md) gesteuert.   

## <a name="visualize-a-log-search"></a>Visualisieren einer Protokollsuche

Sie können im Azure-Portal im Portal für die Protokollsuche einfache Abfragen in einer einzelnen Zeile erstellen. Das Portal für die Protokollsuche kann verwendet werden, ohne ein externes Portal starten zu müssen. Zudem können Sie damit eine Vielzahl von Funktionen mit Protokollsuchen durchführen, einschließlich der Erstellung von Warnungsregeln, der Erstellung von Computergruppen und des Exports der Abfrageergebnisse. 

Das [Advanced Analytics-Portal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal) ist ein dediziertes Portal mit erweiterten Funktionen, die nicht im Portal für die Protokollsuche verfügbar sind. Zu den Funktionen zählen die Möglichkeiten, eine Abfrage in mehreren Zeilen zu bearbeiten, Codes selektiv auszuführen, kontextabhängige IntelliSense-Funktionen auszuführen und intelligente Analysen durchzuführen. Im Advanced Analytics-Portal erstellen Sie eine Leistungsansicht in grafischer Form, speichern sie für eine zukünftige Suche und heften sie an das zuvor erstellte freigegebene Dashboard.   

Das Advanced Analytics-Portal wird über einen Link im Portal für die Protokollsuche gestartet.<br> ![Starten des Advanced Analytics-Portals](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-01.png)

Geben Sie im Analytics-Portal die folgende Abfrage ein, um nur Prozessorauslastungs-Datensätze für Windows- und Linux-Computer gruppiert nach „Computer“ und TimeGenerated in einem visuellen Diagramm angezeigt zurückzugeben:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Speichern Sie die Abfrage durch Auswählen der Schaltfläche **Abfrage speichern** in der oberen rechten Ecke.<br> ![Speichern der Abfrage aus dem Advanced Analytics-Portal](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-02.png)<br><br> Geben Sie in der Systemsteuerung **Abfrage speichern** einen Namen wie z.B. *Azure-VMs – Prozessorauslastung* ein, und klicken Sie dann auf **Speichern**.  Auf diese Weise können Sie eine Bibliothek für allgemeine Abfragen zum Suchen erstellen, oder sie ändern, ohne sie vollständig neu schreiben zu müssen.  Heften Sie die Abfrage schließlich mit Auswahl der Schaltfläche **Diagramm an Ihr Azure-Dashboard heften** in der mittleren rechten Ecke der Seite an das zuvor erstellte freigegebene Dashboard.  

Jetzt haben Sie eine Abfrage an das Dashboard geheftet und werden feststellen, dass sie mit einem allgemeinen Titel und darunter mit einem Kommentar versehen ist.<br> ![Beispiel eines Azure-Dashboards](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  Sie sollten sie mit einem aussagekräftigeren Namen umbenennen, der für die Benutzer, die sie anzeigen, leichter verständlich ist.  Klicken Sie mit der rechten Maustaste auf die Kachel, und wählen Sie **Kachel bearbeiten** .  Klicken Sie nach der Anpassung von Titel und Untertitel der Kachel auf **Aktualisieren**.  Ein Banner wird angezeigt, in dem Sie gefragt werden, ob Sie die Änderungen veröffentlichen oder verwerfen möchten.  Klicken Sie auf **Änderungen veröffentlichen**, und schließen Sie dann den Steuerbereich **Kachel bearbeiten**.  

![Abgeschlossene Konfiguration des Beispieldashboards](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, ein Dashboard im Azure-Portal zu erstellen und ihm eine Protokollsuche hinzuzufügen.  Fahren Sie mit dem nächsten Tutorial fort, um die unterschiedlichen Reaktionen kennen zu lernen, die Sie auf der Basis von Protokollsuchergebnissen implementieren können.  

> [!div class="nextstepaction"]
> [Reagieren auf Ereignisse mit Log Analytics-Warnungen](log-analytics-tutorial-response.md)
