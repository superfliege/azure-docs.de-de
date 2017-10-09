---
title: Diagnostizieren von Laufzeitausnahmen mit Azure Application Insights | Microsoft-Dokumentation
description: Tutorial zum Suchen und Diagnostizieren von Laufzeitausnahmen in Ihrer Anwendung mithilfe von Azure Application Insights
services: application-insights
keywords: 
author: bwren
ms.author: bwren
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 58c69e9c75f3a97c04b2c5b2e3b9acfc7065c658
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Suchen und Diagnostizieren von Laufzeitausnahmen mit Azure Application Insights

Azure Application Insights erfasst Telemetriedaten Ihrer Anwendung, um Laufzeitausnahmen zu identifizieren und diagnostizieren.  Dieses Tutorial führt Sie mit Ihrer Anwendung durch diesen Vorgang.  Folgendes wird vermittelt:

> [!div class="checklist"]
> * Ändern Ihres Projekt zum Aktivieren der Ausnahmeverfolgung
> * Identifizieren von Ausnahmen für verschiedene Komponenten Ihrer Anwendung
> * Anzeigen der Details einer Ausnahme
> * Herunterladen einer Momentaufnahme der Ausnahme in Visual Studio zum Debuggen
> * Analysieren der Details fehlgeschlagener Anforderungen mithilfe der Abfragesprache
> * Erstellen eines neuen Arbeitselements zum Korrigieren des fehlerhaften Codes


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
    - ASP.NET und Webentwicklung
    - Azure-Entwicklung
- Laden Sie den [Visual Studio Snapshot Debugger](http://aka.ms/snapshotdebugger) herunter, und installieren Sie diesen.
- Stellen Sie eine .NET-Anwendung in Azure bereit, und [aktivieren Sie das Application Insights SDK](app-insights-asp-net.md). 
- Das Tutorial verfolgt die Identifikation einer Ausnahme in Ihrer Anwendung, ändern Sie also den Code in Ihrer Entwicklungs- oder Testumgebung, um eine Ausnahme zu generieren. 

## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) am Azure-Portal an.


## <a name="analyze-failures"></a>Analysieren von Fehlern
Application Insights erfasst alle Fehler in Ihrer Anwendung und ermöglicht Ihnen das Anzeigen deren Häufigkeit in verschiedenen Vorgänge, damit Sie sich auf die Fehler mit den höchsten Auswirkungen konzentrieren können.  Sie können dann die Ursache dieser Fehler identifizieren, indem Sie einen Drilldown zu deren Details ausführen.   

1. Wählen Sie **Application Insights** und anschließend Ihr Abonnement aus.  
1. Zum Öffnen des Bereichs **Fehler** wählen Sie entweder im Menü **Untersuchen** die Option **Fehler**, oder klicken Sie auf das Diagramm **Anforderungsfehler**.

    ![Anforderungsfehler](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

2. Im Bereich **Anforderungsfehler** werden für jeden Vorgang für die Anwendung die Anzahl der fehlgeschlagenen Anforderungen und die Anzahl der betroffenen Benutzer angezeigt.  Durch das Sortieren dieser Informationen nach Benutzer können Sie die Fehler identifizieren, die sich auf die meisten Benutzer auswirken.  In diesem Beispiel sollten **GET Employees/Create** und **GET Customers/Details** aufgrund der hohen Anzahl von Fehlern und betroffenen Benutzern untersucht werden.  Durch Auswählen eines Vorgangs werden im rechten Bereich weitere Informationen zu diesem Vorgang angezeigt.

    ![Bereich „Anforderungsfehler“](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

3. Reduzieren Sie das Zeitfenster, um den Zeitraum zu vergrößern, in dem die Fehlerrate eine Spitze aufweist.

    ![Fenster „Anforderungsfehler“](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

4. Klicken Sie auf **Details anzeigen** um die Details für den Vorgang anzuzeigen.  Dazu gehört ein Gantt-Diagramm, das zwei fehlgeschlagene Abhängigkeiten zeigt, deren Abschluss zusammen fast eine halbe Sekunde gedauert hat.  Durch das Analysieren von Leistungsproblemen können Sie mehr erfahren. Führen Sie dazu das Tutorial [Suchen und Diagnostizieren von Leistungsproblemen mit Azure Application Insights](app-insights-tutorial-performance.md) aus.

    ![Details zu Anforderungsfehlern](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

5. In den Vorgangsdetails wird auch eine Ausnahme vom Typ „FormatException“ angezeigt, die den Fehler anscheinend verursacht hat.  Klicken Sie auf die Ausnahme oder die Anzahl der **Top-3-Ausnahmetypen**, um die Details dazu anzuzeigen.  Sie können sehen, dass die Ursache eine ungültige Postleitzahl ist.

    ![Ausnahmedetails](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)



## <a name="identify-failing-code"></a>Fehlerhaften Code identifizieren
Der Momentaufnahmedebugger erfasst Momentaufnahmen der häufigsten Ausnahmen in Ihrer Anwendung, um Sie beim Diagnostizieren der Ursache in der Produktion zu unterstützen.  Sie können Debugmomentaufnahmen im Portal anzeigen, um die Aufrufliste anzuzeigen und die Variablen in jedem Aufruflistenrahmen zu überprüfen. Sie können den Quellcode debuggen, indem Sie die Momentaufnahme herunterladen und in Visual Studio 2017 öffnen.

1. Klicken Sie in den Eigenschaften der Ausnahme auf **Debugmomentaufnahme öffnen**.
2. Der Bereich **Momentaufnahme debuggen** Bereich wird mit der Aufrufliste für die Anforderung geöffnet.  Klicken Sie auf eine beliebige Methode, um die Werte aller lokalen Variablen zum Zeitpunkt der Anforderung anzuzeigen.  Beginnend mit der ersten Methode in diesem Beispiel sehen wir lokale Variablen, die keinen Wert aufweisen.

    ![Momentaufnahme debuggen](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. Der erste Aufruf mit gültigen Werten ist **ValidZipCode**, und wir können sehen, dass eine Postleitzahl mit Buchstaben angegeben wurde, die nicht in eine ganze Zahl übersetzt werden können.  Dies scheint der Fehler im Code zu sein, der behoben werden muss.

    ![Momentaufnahme debuggen](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Zum Herunterladen dieser Momentaufnahme in Visual Studio, wo wir den tatsächlichen Code finden, der behoben werden muss, klicken Sie auf **Momentaufnahme herunterladen**.
6. Die Momentaufnahme wird in Visual Studio geladen.
7. Jetzt können Sie eine Debugsitzung in Visual Studio ausführen, mit der die Codezeile, die die Ausnahme verursacht hat, schnell identifiziert wird.

    ![Ausnahme im Code](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Verwenden von Analysedaten
Alle von Application Insights gesammelten Daten werden in Azure Log Analytics gespeichert, die eine umfangreiche Abfragesprache bereitstellt, die Ihnen eine Vielzahl von Möglichkeiten zum Analysieren der Daten bietet.  Wir können diese Daten verwenden, um die Anforderungen zu analysieren, die die von uns untersuchte Ausnahme generiert haben. 

8. Klicken Sie auf die CodeLens-Informationen über dem Code, um von Application Insights bereitgestellte Telemetriedaten anzuzeigen.

    ![Code](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Klicken Sie auf **Auswirkungen analysieren**, um Application Insights Analytics zu öffnen.  Die Analyse enthält mehrere Abfragen, die Details über Anforderungsfehler bereitstellen, z.B. betroffene Benutzer, Browser und Regionen.<br><br>![Analyse](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Arbeitselement hinzufügen
Wenn Sie Application Insights mit einem Nachverfolgungssystem wie Visual Studio Team Services oder GitHub verbinden, können Sie direkt in Application Insights ein Arbeitselement erstellen.

1. Kehren Sie zum Bereich mit den **Ausnahmeeigenschaften** in Application Insights zurück.
2. Klicken Sie auf **Neues Arbeitselement**.
3. Der Bereich **New Work Item** wird geöffnet und enthält bereits Details zur Ausnahme.  Sie können vor dem Speichern weitere Informationen hinzufügen.

    ![Neues Arbeitselement](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit dem Identifizieren von Laufzeitausnahmen vertraut gemacht haben, können Sie mit dem nächsten Tutorial fortfahren. Dort erfahren Sie, wie Leistungsprobleme identifiziert und diagnostiziert werden.

> [!div class="nextstepaction"]
> [Identifizieren von Leistungsproblemen](app-insights-tutorial-performance.md)
