---
title: Behandeln von Problemen mit Azure Application Insights Profiler | Microsoft-Dokumentation
description: Diese Seite enthält Schritte zur Problembehandlung sowie Informationen, um Entwickler bei der Aktivierung oder Verwendung von Application Insights Profiler zu unterstützen.
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
ms.openlocfilehash: 28de0f8bdcaa730c5beea0c630d4e86e15642809
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142851"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Behandeln von Problemen mit dem Aktivieren oder Anzeigen von Application Insights Profiler

## <a id="troubleshooting"></a>Allgemeine Problembehandlung

### <a name="profiles-are-only-uploaded-if-there-are-requests-to-your-application-while-the-profiler-is-running"></a>Profile werden nur bei vorhandenen Anforderungen für die Anwendung während der Profiler-Ausführung hochgeladen
Application Insights Profiler sammelt Profiler-Daten jede Stunde zwei Minuten lang oder beim Klicken auf die Schaltfläche [**Profil jetzt erstellen**](app-insights-profiler-settings.md?toc=/azure/azure-monitor/toc.json) auf der Seite **Konfigurieren von Application Insights Profiler**. Die Profilerstellungsdaten werden jedoch nur hochgeladen, wenn sie an eine Anforderung angefügt werden können, die während der Profiler-Ausführung aufgetreten ist. 

Der Profiler schreibt Meldungen zur Ablaufverfolgung und benutzerdefinierten Ereignissen in Ihre Application Insights-Ressource. Sie können anhand dieser Ereignisse feststellen, wie der Profiler ausgeführt wird. Wenn der Profiler Ablaufverfolgungen ausführen und erfassen soll, aber nicht auf der Seite „Leistung“ angezeigt wird, können Sie überprüfen, wie der Profiler ausgeführt wird:

1. Suchen Sie nach Meldungen zur Ablaufverfolgung, die vom Profiler an Ihre Application Insights-Ressource gesendet wurden. Anhand dieser Suchzeichenfolge können Sie die relevanten Daten suchen:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Im folgenden Screenshot sehen Sie ein Beispiel von zwei Suchvorgängen aus zwei verschiedenen KI-Ressourcen. 
    
    * Das Beispiel auf der linken Seite stammt von einer Anwendung, die keine Anforderungen abruft, während der Profiler ausgeführt wird. Sie können die Meldung sehen, die besagt, dass der Upload aufgrund von Inaktivität abgebrochen wurde. 

    * Im Beispiel auf der rechten Seite sehen Sie, dass der Profiler gestartet wurde und benutzerdefinierte Ereignisse gesendet hat, als Anforderungen während der Profiler-Ausführung erkannt wurden. Wenn das benutzerdefinierte ServiceProfilerSample-Ereignis angezeigt wird, bedeutet dies, dass der Profiler eine Ablaufverfolgung an eine Anforderung angefügt hat. Die Ablaufverfolgung können Sie auf der Seite zur Application Insights-Leistung anzeigen.

    * Wenn überhaupt keine Telemetriedaten angezeigt werden, wird der Profiler nicht ausgeführt. Wir empfehlen Ihnen, die Abschnitte zur Problembehandlung für Ihren spezifischen App-Typ in diesem Dokument zu lesen.  

     ![Suchen nach Profiler-Telemetriedaten][profiler-search-telemetry]

1. Wenn während des Zeitraums, in dem der Profiler ausgeführt wurde, Anforderungen gesendet wurden, stellen Sie sicher, dass diese Anforderungen von der Komponente Ihrer Anwendung verarbeitet werden, die den Profiler aktiviert hat. Manchmal bestehen Anwendungen aus mehreren Komponenten, aber der Profiler ist nur für einige dieser Komponenten aktiviert. Die Seite „Application Insights-Profiler konfigurieren“ zeigt die Komponenten an, die Ablaufverfolgungen hochgeladen haben.

### <a name="net-core-21-bug"></a>**Fehler in .NET Core 2.1** 
Ein Fehler im Profiler-Agent verhindert das Hochladen von Ablaufverfolgungen von Anwendungen, die unter ASP.NET Core 2.1 ausgeführt werden. Wir arbeiten an einer Lösung und werden sie bald vorstellen. Die Behebung dieses Fehlers wird Ende Oktober bereitgestellt.

### <a name="other-things-to-check"></a>**Überprüfen Sie Folgendes:**
* Ihre App wird unter .NET Framework 4.6 ausgeführt.
* Wenn Ihre Web-App eine ASP.NET Core-Anwendung ist, muss sie mindestens ASP.NET Core 2.0 ausführen.
* Falls die Daten, die Sie anzeigen möchten, bereits mehrere Wochen alt sind, schränken Sie Ihren Zeitfilter ein, und versuchen Sie es noch mal. Ablaufverfolgungen werden nach sieben Tagen gelöscht.
* Stellen Sie sicher, dass der Zugriff auf https://gateway.azureserviceprofiler.net nicht durch Proxys oder durch eine Firewall blockiert wird.

### <a id="double-counting"></a>**Doppelte Erfassung in parallelen Threads**

Manchmal übersteigt die Gesamtzeitmetrik im Stapel-Viewer die Dauer der Anforderung.

Diese Situation kann eintreten, wenn einer Anforderung mehrere Threads zugeordnet sind, die parallel ausgeführt werden. In diesem Fall übersteigt die Gesamtzeit der Threads die verstrichene Zeit. Ein Thread wartet möglicherweise auf den Abschluss eines anderen Threads. Der Viewer versucht, solche Fälle zu erkennen und die nicht relevanten Wartevorgänge zu ignorieren. Im Zweifelsfall werden jedoch eher zu viele Informationen angezeigt, um zu vermeiden, dass womöglich entscheidende Informationen ausgelassen werden.

Wenn Ihnen in Ihren Ablaufverfolgungen parallele Threads auffallen, bestimmen Sie die wartenden Threads, um den kritischen Pfad für die Anforderung ermitteln zu können. In den meisten Fällen wartet der Thread, der schnell in einen Wartezustand wechselt, einfach auf den Abschluss der anderen Threads. Konzentrieren Sie sich auf die anderen Threads, und ignorieren Sie die Zeit in den wartenden Threads.

### <a name="error-report-in-the-profiling-viewer"></a>**Fehlerbericht im Profilerstellungs-Viewer**
Senden Sie ein Supportticket über das Portal. Geben Sie dabei die Korrelations-ID aus der Fehlermeldung an.

## <a name="troubleshooting-profiler-on-app-services"></a>Problembehandlung für den Profiler in App Services
### <a name="for-the-profiler-to-work-properly"></a>**Damit der Profiler ordnungsgemäß funktioniert, müssen folgende Voraussetzungen erfüllt sein:**
* Für die Web-App muss mindestens ein App Service-Plan mit Basic-Tarif verwendet werden.
* Für Ihre Web-App muss die Application Insights-Erweiterung für App Services (2.6.5) installiert sein.
* Für Ihre Web-App muss die App-Einstellung **APPINSIGHTS_INSTRUMENTATIONKEY** mit dem gleichen Instrumentierungsschlüssel konfiguriert sein, der auch vom Application Insights SDK verwendet wird.
* Für Ihre Web-App muss die App-Einstellung **APPINSIGHTS_PROFILERFEATURE_VERSION** definiert und auf „1.0.0“ festgelegt sein.
* Der Webauftrag **ApplicationInsightsProfiler2** muss ausgeführt werden. Sie können den Webauftrag überprüfen, indem Sie zu [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/) wechseln und im Menü „Tools“ das **WebJobs-Dashboard** öffnen. Wie Sie in den Screenshots unten sehen können, können Sie durch Klicken auf den Link „ApplicationInsightsProfiler2“ die Details des Webauftrags einschließlich des Protokolls anzeigen.

    Auf diesen Link müssen Sie klicken, um die Details zum Webauftrag anzuzeigen: 

    ![profiler-webjob]

    Hier sehen Sie die Seite, auf der die Details angezeigt werden. Sie können das Protokoll herunterladen und an Ihr Team senden, wenn Sie selbst nicht herausfinden, warum der Profiler nicht funktioniert.
    
    ![profiler-webjob-log]

### <a name="manual-installation"></a>**Manuelle Installation**

Wenn Sie Profiler konfigurieren, werden an den Einstellungen der Web-App Aktualisierungen vorgenommen. Sie können die Updates manuell anwenden, wenn dies für Ihre Umgebung erforderlich ist. Beispiel: Ihre Anwendung wird in einer Web-Apps-Umgebung für PowerApps ausgeführt.

1. Öffnen Sie im **Steuerungsbereich der Web-App** die Option **Einstellungen**.
1. Legen Sie die **.NET Framework-Version** auf **v4.6** fest.
1. Legen Sie **Immer bereit** auf **Ein** fest.
1. Fügen Sie die App-Einstellung **APPINSIGHTS_INSTRUMENTATIONKEY** hinzu, und legen Sie den Wert auf den Instrumentierungsschlüssel des SDK fest.
1. Fügen Sie die App-Einstellung **APPINSIGHTS_PROFILERFEATURE_VERSION** hinzu, und legen Sie den Wert auf „1.0.0“ fest.
1. Öffnen Sie **Erweiterte Tools**.
1. Wählen Sie **Los** aus, um die Kudu-Website zu öffnen.
1. Wählen Sie auf der Kudu-Website **Site extensions** (Websiteerweiterungen) aus.
1. Installieren Sie **Application Insights** aus dem Katalog der Azure-Web-Apps.
1. Starten Sie die Web-App neu.

### <a name="too-many-active-profiling-sessions"></a>**Zu viele aktive Profilerstellungssitzungen**

Zurzeit können Sie Profiler für maximal vier Azure Web-Apps und Bereitstellungsslots aktivieren, die im selben Serviceplan ausgeführt werden. Wenn der Profiler-Webauftrag zu viele aktive Profilerstellungssitzungen meldet, verschieben Sie einige Web-Apps in einen anderen Serviceplan.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Bereitstellungsfehler: Verzeichnis nicht leer 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

Wenn Sie Ihre Web-App erneut für eine Web-Apps-Ressource bereitstellen und Profiler aktiviert ist, wird möglicherweise eine Meldung wie die folgende angezeigt:

*Verzeichnis nicht leer „D:\\home\\site\\wwwroot\\App_Data\\jobs“*

Dieser Fehler tritt auf, wenn Sie Web Deploy über Skripts oder die Azure DevOps-Bereitstellungspipeline ausführen. Als Lösung fügen Sie die folgenden zusätzlichen Bereitstellungsparameter zum Web Deploy-Task hinzu:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Diese Parameter löschen den Ordner, der von Application Insights Profiler verwendet wird, und heben die Sperre der erneuten Bereitstellung auf. Sie haben keine Auswirkungen auf die Profiler-Instanz, die gerade ausgeführt wird.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Wie kann ich feststellen, ob Application Insights Profiler ausgeführt wird?

Profiler wird als fortlaufender Webauftrag in der Web-App ausgeführt. Sie können die Web-App-Ressource im [Azure-Portal](https://portal.azure.com) öffnen. Überprüfen Sie im Bereich **WebJobs** den Status von **ApplicationInsightsProfiler**. Wenn er nicht ausgeführt wird, öffnen Sie **Protokolle**, um weitere Informationen zu erhalten.

## <a name="troubleshooting-problems-with-profiler-and-wad"></a>Behandeln von Problemen mit Profiler und WAD

Um zu ermitteln, ob der Profiler von WAD ordnungsgemäß konfiguriert wurde, müssen Sie drei Dinge überprüfen. Erstens müssen Sie überprüfen, ob die bereitgestellten Inhalte der WAD-Konfiguration Ihren Erwartungen entsprechen. Zweitens müssen Sie überprüfen, ob WAD in der Profiler-Befehlszeile den richtigen iKey übergibt. Drittens können Sie die Profiler-Protokolldatei überprüfen, um festzustellen, ob der Profiler ausgeführt wurde, aber einen Fehler erhält. 

Um die Einstellungen zu überprüfen, die zum Konfigurieren von WAD verwendet wurden, müssen Sie sich beim virtuellen Computer anmelden und die Protokoll in diesem Speicherort öffnen: 
```
c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.logs  
```
Suchen Sie in dieser Datei nach der Zeichenfolge „WadCfg“, um die Einstellungen zu finden, die zum Konfigurieren von WAD an den virtuellen Computer übergeben wurden. Sie können überprüfen, ob der von der Profiler-Senke verwendete iKey richtig ist.

Sie können auch die Befehlszeile überprüfen, die zum Starten des Profilers verwendet wird. Die folgende Datei enthält die Argumente, die zum Starten des Profilers verwendet werden.
```
D:\ProgramData\ApplicationInsightsProfiler\config.json
```
Überprüfen Sie, ob der iKey in der Profiler-Befehlszeile richtig ist. 

Überprüfen Sie die Protokolldatei des Profilers, die Sie im Pfad in der oben genannten config.json-Datei finden. Die Datei zeigt Debuginformationen an, die die vom Profiler verwendeten Einstellungen sowie Status- und Fehlermeldungen aus dem Profiler angeben. Wenn der Profiler ausgeführt wird, während Ihre Anwendung Anforderungen empfängt, wird die Meldung „Aktivität aus iKey erkannt“ angezeigt. Wenn die Ablaufverfolgung hochgeladen wird, wird die Meldung angezeigt, dass mit dem Hochladen der Ablaufverfolgung begonnen wurde. 


[profiler-search-telemetry]:./media/app-insights-profiler/Profiler-Search-Telemetry.png
[profiler-webjob]:./media/app-insights-profiler/Profiler-webjob.png
[profiler-webjob-log]:./media/app-insights-profiler/Profiler-webjob-log.png








