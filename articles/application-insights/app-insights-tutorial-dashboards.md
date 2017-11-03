---
title: Erstellen benutzerdefinierter Dashboards in Azure Application Insights | Microsoft-Dokumentation
description: Tutorial zum Erstellen benutzerdefinierter KPI-Dashboards mithilfe von Azure Application Insights
keywords: 
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0d2f98ca2fb39289b2916ddd24590924856507d6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Erstellen benutzerdefinierter KPI-Dashboards mithilfe von Azure Application Insights

Sie können mehrere Dashboards im Azure-Portal erstellen, wobei jedes Kacheln enthält, die Daten aus mehreren Azure-Ressourcen in verschiedenen Ressourcengruppen und Abonnements visualisieren.  Außerdem lassen sich verschiedene Diagramme und Ansichten aus Azure Application Insights anheften, um benutzerdefinierte Dashboards zu erstellen, mit denen Sie einen vollständigen Überblick über die Integrität und Leistung der Anwendung erhalten.  Dieses Tutorial führt Sie durch die Erstellung eines benutzerdefinierten Dashboards, das mehrere Daten- und Visualisierungstypen aus Azure Application Insights enthält.  Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Dashboards in Azure
> * Hinzufügen einer Kachel aus dem Kachelkatalog
> * Hinzufügen von Standardmetriken in Application Insights zum Dashboard 
> * Hinzufügen eines benutzerdefinierten Metrikdiagramms in Application Insights zum Dashboard
> * Hinzufügen der Ergebnisse einer Analytics-Abfrage zum Dashboard 



## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Stellen Sie eine .NET-Anwendung in Azure bereit, und [aktivieren Sie das Application Insights SDK](app-insights-asp-net.md). 

## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) im Azure-Portal an.

## <a name="create-a-new-dashboard"></a>Erstellen eines neuen Dashboards
Ein einzelnes Dashboard kann Ressourcen aus mehreren Anwendungen, Ressourcengruppen und Abonnements enthalten.  Starten Sie das Tutorial, indem Sie ein neues Dashboard für Ihre Anwendung erstellen.  

2.  Klicken Sie auf dem Hauptbildschirm des Portals auf **Neues Dashboard**.

    ![Neues Dashboard](media/app-insights-tutorial-dashboards/new-dashboard.png)

3. Geben Sie einen Namen für das Dashboard ein.
4. Werfen Sie einen Blick auf den **Kachelkatalog**. Dort finden Sie verschiedene Kacheln, die Sie zum Dashboard hinzufügen können.  Sie können aber nicht nur Kacheln aus dem Katalog hinzufügen. Sie haben außerdem die Möglichkeit, Diagramme und andere Ansichten direkt aus Application Insights an das Dashboard anzuheften.
5. Suchen Sie die Kachel **Markdown**, und ziehen Sie sie in Ihr Dashboard.  Diese Kachel ermöglicht es Ihnen, in Markdown formatierten Text hinzuzufügen. Diese Funktion eignet sich ideal, um Ihrem Dashboard beschreibenden Text hinzuzufügen.
6. Fügen Sie den Eigenschaften der Kachel Text hinzu, und passen Sie deren Größe im Dashboardbereich an.
    
    ![Bearbeiten der Markdown-Kachel](media/app-insights-tutorial-dashboards/edit-markdown.png)

6. Klicken Sie oben im Bildschirm auf **Anpassung abgeschlossen**, um den Bearbeitungsmodus für die Kachel zu beenden, und klicken Sie dann auf **Änderungen veröffentlichen**, um sie zu speichern.

    ![Dashboard mit Markdown-Kachel](media/app-insights-tutorial-dashboards/dashboard-01.png)


## <a name="add-health-overview"></a>Hinzufügen der Integritätsübersicht
Ein Dashboard, das nur statischen Text enthält, ist nicht besonders interessant. Fügen Sie daher eine Kachel aus Application Insights hinzu, um Informationen zu Ihrer Anwendung darzustellen.  Application Insights-Kacheln lassen sich aus dem Kachelkatalog hinzufügen oder direkt von Application Insights-Bildschirmen anheften.  Dadurch können Sie Diagramme und Ansichten konfigurieren, mit denen Sie bereits vertraut sind, bevor Sie diese an Ihr Dashboard anheften.  Fügen Sie zuerst die Standardübersicht über die Integrität Ihrer Anwendung hinzu.  Dies erfordert keine Konfiguration und ermöglicht minimale Anpassungen im Dashboard.


1. Klicken Sie im Azure-Menü auf **Application Insights**, und wählen Sie dann die Anwendung aus.
2. Rufen Sie in der **Übersichtszeitachse** das Kontextmenü auf, und klicken Sie auf **An Dashboard anheften**.  Dadurch wird die Kachel dem letzten Dashboard hinzugefügt, das Sie aufgerufen haben.  

    ![Übersichtszeitachse anheften](media/app-insights-tutorial-dashboards/pin-overview-timeline.png)
 
3. Klicken Sie oben im Bildschirm auf **Dashboard anzeigen**, um zum Dashboard zurückzukehren.
4. Die Übersichtszeitachse wurde Ihrem Dashboard hinzugefügt.  Klicken Sie darauf, ziehen Sie sie an die gewünschte Position, und klicken Sie dann auf **Anpassung abgeschlossen** und **Änderungen veröffentlichen**.  Ihr Dashboard verfügt jetzt über eine Kachel mit hilfreichen Informationen.

    ![Dashboard mit einer Übersichtszeitachse](media/app-insights-tutorial-dashboards/dashboard-02.png)



## <a name="add-custom-metric-chart"></a>Hinzufügen eines benutzerdefinierten Metrikdiagramms
Im Bereich **Metriken** können Sie eine Metrik grafisch darstellen, die von Application Insights über einen Zeitraum hinweg mit optionalen Filtern und Gruppierungen erfasst wurde.  Wie alle anderen Elemente in Application Insights auch, können Sie dieses Diagramm dem Dashboard hinzufügen.  Dies erfordert aber kleine Anpassungen.

1. Klicken Sie im Azure-Menü auf **Application Insights**, und wählen Sie dann die Anwendung aus.
1. Klicken Sie auf **Metriken**.  
2. Ein leeres Diagramm wurde bereits erstellt, und Sie werden aufgefordert, eine Metrik hinzuzufügen.  Fügen Sie dem Diagramm eine Metrik hinzu sowie optional einen Filter und eine Gruppierung.  Im folgenden Beispiel wird die Anzahl der Serveranforderungen gruppiert nach Erfolg dargestellt.  Dies bietet einen aktuellen Überblick über erfolgreiche und fehlgeschlagene Anforderungen.

    ![Hinzufügen von Metriken](media/app-insights-tutorial-dashboards/metrics-chart.png)

4. Rufen Sie das Kontextmenü für das Diagramm auf, und klicken Sie auf **An Dashboard anheften**.  Dadurch wird die Ansicht dem letzten Dashboard hinzugefügt, mit dem Sie gearbeitet haben.

    ![PIN-Metrikdiagramm](media/app-insights-tutorial-dashboards/pin-metrics-chart.png)

3. Klicken Sie oben im Bildschirm auf **Dashboard anzeigen**, um zum Dashboard zurückzukehren.

4. Die Übersichtszeitachse wurde Ihrem Dashboard hinzugefügt.  Klicken Sie darauf, ziehen Sie sie an die gewünschte Position, und klicken Sie dann auf **Anpassung abgeschlossen** und **Änderungen veröffentlichen**. 

    ![Dashboard mit Metriken](media/app-insights-tutorial-dashboards/dashboard-03.png)


## <a name="metrics-explorer"></a>Metrik-Explorer
Der **Metrik-Explorer** ähnelt Metriken, obwohl er deutlich größere Anpassung ermöglicht, wenn er dem Dashboard hinzugefügt wird.  Welche Sie verwenden, um die Metriken grafisch darzustellen, hängt von Ihren Anforderungen ab.

1. Klicken Sie im Azure-Menü auf **Application Insights**, und wählen Sie dann die Anwendung aus.
1. Klicken Sie auf **Metrik-Explorer**. 
2. Klicken Sie auf diese Option, um das Diagramm zu bearbeiten, und wählen Sie mindestens eine Metrik und optional eine detaillierte Konfiguration aus.  Im Beispiel wird ein Liniendiagramm gezeigt, das die durchschnittliche Seitenantwortzeit nachverfolgt.
3. Klicken Sie oben rechts auf das Reißzweckensymbol, um das Diagramm Ihrem Dashboard hinzuzufügen, und ziehen Sie es dann an die gewünschte Position.

    ![Metrik-Explorer](media/app-insights-tutorial-dashboards/metrics-explorer.png)

4. Die Kachel „Metrik-Explorer“ ermöglicht eine stärkere Anpassung, wenn sie dem Dashboard hinzugefügt wird.  Klicken Sie mit der rechten Maustaste auf die Kachel, und wählen Sie **Kachel bearbeiten** aus, um einen benutzerdefinierten Titel hinzuzufügen.  Nehmen Sie bei Bedarf weitere Anpassungen vor.

    ![Dashboard mit Metrik-Explorer](media/app-insights-tutorial-dashboards/dashboard-04a.png)

5. Das Metrik-Explorer-Diagramm wurde nun Ihrem Dashboard hinzugefügt.

    ![Dashboard mit Metrik-Explorer](media/app-insights-tutorial-dashboards/dashboard-04.png)

## <a name="add-analytics-query"></a>Exportieren einer Analytics-Abfrage
Azure Application Insights Analytics bietet eine umfangreiche Abfragesprache, mit der Sie alle von Application Insights erfassten Daten analysieren können.  Sie können Ihrem Dashboard die Ausgabe einer Analytics-Abfrage genauso wie Diagramme und andere Ansichten hinzufügen.   

Da es sich bei Azure Applications Insights Analytics um einen separaten Dienst handelt, müssen Sie Ihr Dashboard dafür freigeben, damit eine Analytics-Abfrage eingeschlossen werden kann.  Wenn Sie ein Azure-Dashboard freigeben, können Sie es als Azure-Ressource veröffentlichen, wodurch es für andere Benutzer und Ressourcen verfügbar wird.  

1. Klicken Sie dazu oben im Dashboard auf **Freigeben**.

    ![Veröffentlichen des Dashboards](media/app-insights-tutorial-dashboards/publish-dashboard.png)

2. Behalten Sie für **Dashboardname** den ursprünglichen bei, und legen Sie für **Abonnementname** denselben wie für das Dashboard fest.  Klicken Sie auf **Veröffentlichen**.  Das Dashboard ist jetzt für andere Abonnements und Dienste verfügbar.  Sie können optional bestimmte Benutzer definieren, die Zugriff auf das Dashboard haben sollen.
1. Klicken Sie im Azure-Menü auf **Application Insights**, und wählen Sie dann die Anwendung aus.
2. Klicken Sie oben im Bildschirm auf **Analytics**, um das Analytics-Portal zu öffnen.

    ![Starten von Analytics](media/app-insights-tutorial-dashboards/start-analytics.png)

3. Geben Sie die folgende Abfrage ein, die die zehn am häufigsten angeforderten Seiten und die Anzahl der Anforderungen zurückgibt:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Klicken Sie auf **Go** (Los), um die Ergebnisse der Abfrage zu überprüfen.
5. Klicken Sie auf das Reißzweckensymbol, und wählen Sie den Namen des Dashboards aus.  Im Gegensatz zu den vorherigen Schritten, in denen das zuletzt verwendete Dashboard genutzt wurde, müssen Sie für diese Option ein Dashboard auswählen, weil die Analytics-Konsole ein separater Dienst ist und aus allen verfügbaren, freigegebenen Dashboards eine Auswahl treffen muss.

    ![Anheften der Analytics-Abfrage](media/app-insights-tutorial-dashboards/analytics-pin.png)

5. Fügen Sie eine weitere Abfrage hinzu, bevor Sie zum Dashboard zurückkehren. Rendern Sie sie dieses Mal jedoch als Diagramm, um die verschiedenen Möglichkeiten der Visualisierung von Analytics-Abfragen in einem Dashboard kennenzulernen.  Beginnen Sie mit der folgenden Abfrage, die die zehn häufigsten Vorgänge mit den meisten Ausnahmen zusammenfasst.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. Klicken Sie auf **Diagramm**, und ändern Sie es in ein **Ringdiagramm**, um die Ausgabe anzuzeigen.

    ![Analytics-Diagramm](media/app-insights-tutorial-dashboards/analytics-chart.png)

6. Klicken Sie auf das Reißzweckensymbol, um das Diagramm an Ihr Dashboard anzuheften, und klicken Sie dieses Mal auf den Link, um zum Dashboard zurückzukehren.
4. Die Ergebnisse der Abfragen werden Ihrem Dashboard jetzt in dem Format hinzugefügt, das Sie ausgewählt haben.  Klicken Sie auf jedes, ziehen Sie es an die gewünschte Position, und klicken Sie dann auf **Bearbeitung abgeschlossen**.
5. Klicken Sie mit der rechten Maustaste auf jede der Kacheln, und wählen Sie **Titel bearbeiten** aus, um ihnen einen aussagekräftigen Namen zu geben.

    ![Dashboard mit Analytics](media/app-insights-tutorial-dashboards/dashboard-05.png)

5. Klicken Sie auf **Änderungen veröffentlichen**, um die Änderungen an Ihrem Dashboard zu committen, das jetzt eine Vielzahl von Diagrammen und Visualisierungen aus Application Insights enthält.


## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt wissen, wie Sie benutzerdefinierte Dashboards erstellen, können Sie einen Blick auf die übrige Application Insights-Dokumentation einschließlich einer Fallstudie werfen.

> [!div class="nextstepaction"]
> [Eingehende Diagnose](app-insights-devops.md)