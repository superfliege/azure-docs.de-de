---
title: Blatt mit Azure Application Insights Profiler-Einstellungen | Microsoft-Dokumentation
description: Anzeigen des Profiler-Status und Starten von Profilerstellungssitzungen
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 81608dd7281ceddce7e0701535ad99e1c9e44315
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142793"
---
# <a name="configure-application-insights-profiler"></a>Konfigurieren von Application Insights Profiler

## <a name="profiler-settings-page"></a>Seite mit Profiler-Einstellungen

Die Seite mit Profiler-Einstellungen kann über die Application Insights-Seite „Leistung“ geöffnet werden, indem Sie auf die Schaltfläche **Profiler** klicken.

![Bereichseintrag „Profiler konfigurieren“][configure-profiler-entry]

Die Seite „Application Insights Profiler konfigurieren“ enthält vier Features: 
1. **Jetzt Profil erstellen**: Durch Klicken auf diese Schaltfläche werden Profilerstellungssitzungen für alle Apps gestartet, die mit dieser Instanz von Application Insights verknüpft sind.
1. **Verknüpfte Apps**: Liste der Anwendungen, die Profiler an diese Application Insights-Ressource senden.
1. **Aktuelle Sitzungen**: Wenn Sie auf **Jetzt Profil erstellen** klicken, wird hier der Status der Sitzung angezeigt.
1. **Letzte Profilerstellungssitzungen**: Zeigt Informationen zu vergangenen Profilerstellungssitzungen an.

![Profiler auf Anforderung][profiler-on-demand]

## <a name="app-service-environments-ase"></a>App Service-Umgebungen (ASE)
Abhängig von Ihrer ASE-Konfiguration ist der Aufruf zum Überprüfen des Agent-Status möglicherweise blockiert. Diese Seite informiert Sie, dass der Agent nicht ausgeführt wird, obwohl dies tatsächlich der Fall ist. Zur Sicherheit können Sie den Webauftrag in Ihrer Anwendung überprüfen. Wenn aber alle App-Einstellungen korrekt festgelegt sind und die App Insights-Websiteerweiterung für Ihre Anwendung installiert ist, wird der Profiler ausgeführt. In diesem Fall werden die aktuellen Profilerstellungssitzungen in der Liste angezeigt, sofern ausreichend Datenverkehr zu Ihrer Anwendung vorliegt.

## <a id="profileondemand"></a> Manuelles Auslösen von Profiler

Profiler kann manuell mit einem Klick auf eine Schaltfläche ausgelöst werden. Nehmen wir an, dass Sie einen Webleistungstest ausführen. Sie benötigen Ablaufverfolgungen, um besser zu verstehen, wie Ihre Web-App unter Last ausgeführt wird. Es ist von entscheidender Bedeutung, dass Sie kontrollieren können, wann Ablaufverfolgungen erfasst werden, da Sie wissen, wann der Auslastungstest ausgeführt wird, aber das Intervall für zufällige Stichproben diesen möglicherweise verpasst.
Die folgenden Schritte veranschaulichen das Szenario:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Optional) Schritt 1: Generieren Sie Datenverkehr zu Ihrer Web-App, indem Sie einen Webleistungstest starten

Wenn Ihre Web-App bereits über eingehenden Datenverkehr verfügt oder wenn Sie einfach nur manuell Datenverkehr generieren möchten, überspringen Sie diesen Abschnitt, und fahren Sie mit Schritt 2 fort.

Navigieren Sie zum Application Insights-Portal, und klicken Sie auf **Konfigurieren > Leistungstests**. Klicken Sie auf „Neu“, um einen neuen Leistungstest zu starten.

![Neuen Leistungstest erstellen][create-performance-test]

Konfigurieren Sie im Bereich **Neuer Leistungstest** die Testziel-URL. Übernehmen Sie alle Standardeinstellungen, und starten Sie die Ausführung des Auslastungstests.

![Konfigurieren von Auslastungstests][configure-performance-test]

Sie sehen, dass der neue Test zuerst in die Warteschlange eingereiht wird. Anschließend folgt der Status „In Bearbeitung“.

![Auslastungstest übermittelt und in die Warteschlange gestellt][load-test-queued]

![Ausführung des Auslastungstests ist in Bearbeitung][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Schritt 2: Starten von Profiler auf Anforderung

Sobald der Auslastungstest ausgeführt wird, können wir Profiler starten, um Ablaufverfolgungen in der Web-App zu erfassen, während diese die Last empfängt.
Navigieren Sie zum Bereich „Profiler konfigurieren“:


### <a name="step-3-view-traces"></a>Schritt 3: Anzeigen von Ablaufverfolgungen

Wenn die Profiler-Ausführung abgeschlossen ist, folgen Sie den Anweisungen zur Benachrichtigung, um zur Seite „Leistung“ zu wechseln und Ablaufverfolgungen anzuzeigen.

## <a name="troubleshooting-on-demand-profiler"></a>Problembehandlung für den bedarfsgesteuerten Profiler

Im einigen Fällen werden nach einer bedarfsgesteuerten Sitzung möglicherweise Meldungen zu Profiler-Timeoutfehlern angezeigt:

![Profiler-Timeoutfehler][profiler-timeout]

Möglicherweise gibt es zwei Gründe dafür, dass der Fehler angezeigt wird:

1. Die bedarfsgesteuerte Profiler-Sitzung war erfolgreich, aber Application Insights hat mehr Zeit zum Verarbeiten der gesammelten Daten benötigt. Wenn die Datenverarbeitung nicht innerhalb von 15 Minuten abgeschlossen wurde, zeigt das Portal eine Timeoutmeldung an. Kurze Zeit später werden jedoch Profiler-Ablaufverfolgungen angezeigt. Wenn dies geschieht, ignorieren Sie die Fehlermeldung vorerst. Wir arbeiten mit Hochdruck an der Lösung dieses Problems.

1. Ihre Web-App verfügt über eine ältere Version des Profiler-Agent, der nicht über das On-Demand-Feature verfügt. Wenn Sie das Application Insights-Profil bereits aktiviert haben, ist es wahrscheinlich, dass Sie Ihren Profiler-Agent aktualisieren müssen, um das On-Demand-Feature zu verwenden.
  
So überprüfen und installieren Sie den neuesten Profiler:

1. Wechseln Sie zu den App-Einstellungen von App Services, und prüfen Sie, ob die folgenden Einstellungen festgelegt sind:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Ersetzen Sie die Einstellung durch den richtigen Instrumentierungsschlüssel für Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 Wenn nicht alle Einstellungen festgelegt werden, wechseln Sie zum Application Insights-Aktivierungsbereich, um die neueste Websiteerweiterung zu installieren.

1. Wechseln Sie zum Application Insights-Bereich im App Services-Portal.

    ![Aktivieren von Application Insights im App Services-Portal][enable-app-insights]

1. Wenn auf der folgenden Seite eine Schaltfläche „Aktualisieren“ angezeigt wird, klicken Sie darauf, um die Application Insights-Websiteerweiterung zu aktualisieren. Anschließend wird der neueste Profiler-Agent installiert.

    ![Websiteerweiterung aktualisieren][update-site-extension]

1. Klicken Sie auf **Ändern**, um sicherzustellen, dass Profiler aktiviert ist, und anschließend auf **OK**, um die Änderungen zu speichern.

    ![Ändern und Speichern von App Insights][change-and-save-appinsights]

1. Kehren Sie zurück zur Registerkarte **App-Einstellungen** Registerkarte für den App Service, um zu überprüfen, ob die folgenden Elemente der App-Einstellungen festgelegt sind:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Ersetzen Sie den Wert durch den ordnungsgemäßen Instrumentierungsschlüssel für Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![App-Einstellungen für Profiler][app-settings-for-profiler]

1. Aktivieren Sie optional die Erweiterungsversion, und stellen Sie sicher, dass kein Update verfügbar ist.

    ![Auf Erweiterungsupdate prüfen][check-for-extension-update]

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren von Profiler und Anzeigen von Ablaufverfolgungen](app-insights-profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png