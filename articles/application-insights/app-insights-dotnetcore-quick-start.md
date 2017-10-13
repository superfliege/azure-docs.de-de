---
title: Schnellstart mit Azure Application Insights | Microsoft-Dokumentation
description: "Dieser Artikel bietet Informationen zum schnellen Einrichten einer ASP.NET Core-Web-App für die Überwachung mit Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/15/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 558c3c65a811a228f9d8ecae7ce41798ac8178c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>Starten der Überwachung Ihrer ASP.NET Core-Webanwendung

Mit Azure Application Insights können Sie die Verfügbarkeit, Leistung und Nutzung Ihrer Webanwendung ganz einfach überwachen. Sie können auch Fehler in Ihrer Anwendung schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese Fehler von Benutzern gemeldet werden. 

Diese Schnellstartanleitung führt Sie durch die notwendigen Schritte, um das Application Insights SDK zu einer vorhandenen ASP.NET Core-Webanwendung hinzuzufügen. 

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
  - ASP.NET und Webentwicklung
  - Azure-Entwicklung
- [Installieren Sie das .NET Core 2.0 SDK](https://www.microsoft.com/net/core).
- Sie benötigen ein Azure-Abonnement und eine vorhandene .NET Core-Webanwendung.

Wenn Sie noch nicht über eine ASP.NET Core-Webanwendung verfügen, können Sie mithilfe der Anleitung unter [Erstellen einer ASP.NET Core-Web-App](https://docs.microsoft.com/en-us/aspnet/core/tutorials/publish-to-azure-webapp-using-vs) eine erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

Application Insights kann Telemetriedaten von jeder mit dem Internet verbundenen Anwendung erfassen, unabhängig davon, ob die Anwendung lokal oder in der Cloud ausgeführt wird. Geben Sie folgendermaßen vor, um diese Daten anzuzeigen.

1. Wählen Sie **Neu** > **Überwachung + Verwaltung** > **Application Insights** aus.

   ![Hinzufügen einer Application Insights-Ressource](./media/app-insights-dotnetcore-quick-start/0001-dc.png)

    Es wird ein Dialogfeld für die Konfiguration geöffnet. Füllen Sie die Eingabefelder anhand der Informationen in der unten stehenden Tabelle aus.

    | Einstellungen        |  Wert           | Beschreibung  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Anwendungstyp** | ASP.NET-Webanwendung | Der Typ der zu überwachenden App. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. |
   | **Standort** | USA (Ost) | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts, in dem Ihre App gehostet wird. |

2. Klicken Sie auf **Erstellen**.

## <a name="configure-app-insights-sdk"></a>Konfigurieren des Application Insights SDK

1. Öffnen Sie das **Projekt** Ihrer ASP.NET Core-Web-App in Visual Studio. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Namen Ihrer App, und wählen Sie **Hinzufügen** > **Application Insights-Telemetrie** aus.

    ![Hinzufügen von Application Insights-Telemetrie](./media/app-insights-dotnetcore-quick-start/0001.png)

2. Klicken Sie auf die Schaltfläche **Kostenlos starten**, und wählen Sie die **vorhandene Ressource** aus, die Sie im Azure-Portal erstellt haben. Klicken Sie dann auf **Registrieren**.

3. Wählen Sie **Debuggen** > **Ohne Debuggen starten**, oder drücken Sie STRG+F5, um Ihre App zu starten.

> [!NOTE]
> Es dauert ca. 3-5 Minuten, bis die ersten Daten im Portal angezeigt werden. Wenn es sich um eine Test-App mit geringem Datenverkehr handelt, denken Sie daran, dass die meisten Metriken nur erfasst werden, wenn aktive Anforderungen oder Vorgänge vorhanden sind.

## <a name="start-monitoring-in-the-azure-portal"></a>Starten der Überwachung im Azure-Portal

1. Sie können jetzt die Seite **Übersicht** für Application Insights im Azure-Portal erneut öffnen, indem Sie **Projekt** > **Application Insights** > **Application Insights-Portal öffnen** auswählen. Hier können Sie Details zu Ihrer aktuell ausgeführten Anwendung anzeigen.

   ![Application Insights – Menü „Übersicht“](./media/app-insights-dotnetcore-quick-start/004-Black.png)

2. Klicken Sie auf **App-Zuordnung**, um ein visuelles Layout der Abhängigkeitsbeziehungen zwischen den Komponenten Ihrer Anwendung zu erhalten. Jede Komponente zeigt KPIs wie z.B. Last, Leistung, Fehler und Warnungen an.

   ![Anwendungszuordnung](./media/app-insights-dotnetcore-quick-start/0002-dc.png)

3. Klicken Sie auf die Schaltfläche für die **App-Analyse** ![Schaltfläche „Anwendungszuordnung“](./media/app-insights-dotnetcore-quick-start/006.png).  Dadurch wird die **Application Insights-Analyse** geöffnet, die eine erweiterte Abfragesprache zum Analysieren aller Daten bereitstellt, die von Application Insights gesammelt werden. In diesem Fall wird eine Abfrage für Sie generiert, die die Anzahl von Anforderungen als Diagramm darstellt. Sie können selbst Abfragen zum Analysieren anderer Daten schreiben.

   ![Analysediagramm der Benutzeranforderungen in einem bestimmten Zeitraum](./media/app-insights-dotnetcore-quick-start/0007-dc.png)

4. Kehren Sie zur Seite **Übersicht** zurück, und untersuchen Sie die **Übersichtszeitachse für die Integrität**.  Dieses Dashboard zeigt Statistiken zur Integrität Ihrer Anwendung, einschließlich der Anzahl von eingehenden Anforderungen, der Dauer dieser Anforderungen und aller auftretenden Fehler. 

   ![Diagramm der Übersichtszeitachse für die Integrität](./media/app-insights-dotnetcore-quick-start/0008-dc.png)

   Damit das Diagramm **Ladezeit der Seitenansicht** mit **clientseitigen Telemetriedaten** aufgefüllt werden kann, fügen Sie dieses Skript zu jeder Seite hinzu, die Sie nachverfolgen möchten:

   ```HTML
   <!-- 
   To collect end-user usage analytics about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
       function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
       }({
           instrumentationKey:"<insert instrumentation key>"
       });
       
       window.appInsights=appInsights;
       appInsights.trackPageView();
   </script>
   ```

5. Klicken Sie unter der Überschrift **Untersuchen** auf **Browser**. Hier finden Sie Metriken in Zusammenhang mit der Leistung der Seiten Ihrer App. Sie können auf **Neues Diagramm hinzufügen** klicken, um zusätzliche benutzerdefinierte Ansichten zu erstellen, oder auf **Bearbeiten** klicken, um Typ, Höhe, Farbpalette, Gruppierungen und Metriken der vorhandenen Diagramme zu ändern.

   ![Servermetriken – Diagramm](./media/app-insights-dotnetcore-quick-start/009-Black.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie planen, die nachfolgenden Schnellstartanleitungen oder Tutorials durchzuarbeiten, sollten Sie die in dieser Schnellstartanleitung erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myResourceGroup**.
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Suchen nach und Diagnostizieren von Laufzeitausnahmen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-tutorial-runtime-exceptions)
