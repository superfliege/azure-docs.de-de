---
title: Profilerstellung für ASP.NET Core-Azure Linux-Web-Apps mit Application Insights Profiler | Microsoft-Dokumentation
description: Übersicht über das Konzept und schrittweises Tutorial zu dessen Aktivierung
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 63a7ceacffe1ee33227d3a8272dda7de7b3b1135
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profilerstellung für ASP.NET Core-Azure Linux-Web-Apps mit Application Insights Profiler

Diese Funktion steht derzeit als Vorschau zur Verfügung.

Ermitteln Sie durch Verwendung von [Application Insights](app-insights-overview.md) den Zeitaufwand für die einzelnen Methoden in Ihrer Live-Webanwendung. Der Profiler ist jetzt für ASP.NET Core-Web-Apps verfügbar, die in Linux auf App Services gehostet werden. Dieser Leitfaden enthält eine schrittweise Anleitung zum Erfassen von Profilerablaufverfolgungen für ASP.NET Core-Linux-Web-Apps.

Nach Abschluss dieser exemplarischen Vorgehensweise erfasst Ihre App Profilerablaufverfolgungen ähnlich dem folgenden Screenshot. In diesem Beispiel weist die Profilerablaufverfolgung nach, dass eine bestimmte Webanforderung langsam ist, da die meiste Zeit für das Warten aufgewendet wird. Dem langsamsten Pfad im Code, der die App ausgebremst hat, wird das Flammensymbol vorangestellt. Dieses Beispiel zeigt, dass die `About`-Methode in `HomeController` die Web-App durch den Aufruf von `Thread.Sleep` verlangsamt hat.

![Profilerablaufverfolgungen](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="pre-requisites"></a>Voraussetzungen
Die folgenden Anweisungen werden auf alle Windows-, Linux- und Mac-Entwicklungsumgebungen angewendet:

* Installieren Sie [.NET Core SDK 2.1.2 oder höher](https://www.microsoft.com/net/download/windows/build)
* Installieren Sie Git nach den Anweisungen unter [Getting Started – Installing Git (Erste Schritte – Installieren von Git)](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="setup-project-locally"></a>Lokales Einrichten des Projekts

1. Öffnen Sie eine Eingabeaufforderung auf dem Computer. Die folgenden Anweisungen gelten für alle Windows-, Linux- und Mac-Entwicklungsumgebungen.

2. Erstellen Sie eine ASP.NET Core-MVC-Webanwendung.
    ```
    dotnet new mvc -n LinuxProfilerTest
    ```
3. Ändern Sie das Verzeichnis im Eingabeaufforderungsfenster in den Stammordner des Projekts.

4. Fügen Sie das NuGet-Paket für das Erfassen von Profilerablaufverfolgungen hinzu.
    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```
5. Fügen Sie eine Codezeile zur Verzögerung um ein paar Sekunden nach dem Zufallsprinzip in „HomeController.cs“ hinzu.

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```
6. Speichern Sie die Änderungen, und committen Sie sie an das lokale Repository.

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-azure-app-service-for-hosting-your-project"></a>Erstellen von Azure App Service zum Hosten Ihres Projekts
1. Erstellen Sie eine App Services-Linux-Umgebung.

    ![Linux-App Services erstellen](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Erstellen Sie Anmeldeinformationen für die Bereitstellung. Notieren Sie sich Ihr Kennwort, da Sie dies später beim Bereitstellen Ihrer App benötigen.

    ![Anmeldeinformationen für die Bereitstellung erstellen](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Wählen Sie die Bereitstellungsoption aus. Richten Sie gemäß der Anweisungen im Azure-Portal ein lokales Git-Repository in der Web-App ein. Ein Git-Repository wird automatisch erstellt.

    ![Git-Repository einrichten](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Weitere Bereitstellungsoptionen finden Sie [hier](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Bereitstellen Ihres Projekts

1. Navigieren Sie an der Eingabeaufforderung zum Stammordner des Projekts. Fügen Sie das Git-Remote-Repository hinzu, sodass es auf das auf den App Services weist:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```
    * Verwenden Sie den „Benutzernamen“ aus dem Schritt des Erstellens von Anmeldeinformationen für die Bereitstellung.
    * Verwenden Sie den „App-Namen“ aus dem Schritt der App Services-Erstellung.

2. Stellen Sie das Projekt durch Pushen der Änderungen in Azure bereit.

    ```
    git push azure master
    ```
Eine Ausgabe ähnlich der folgenden wird angezeigt:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Hinzufügen von Application Insights zum Überwachen Ihrer Web-Apps
1. [Erstellen Sie eine Application Insights-Ressource](./app-insights-create-new-resource.md).
2. Kopieren Sie den iKey der Application Insights-Ressource, und legen Sie die folgenden Einstellungen in Ihren App Services fest.

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![App-Einstellungen einrichten](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Beim Ändern der App-Einstellungen wird die Website automatisch neu gestartet. Sobald die neuen Einstellungen angewendet werden, startet der Profiler sofort für 2 Minuten. Er wird dann pro Stunde 2 Minuten lang ausgeführt.

3. Generieren Sie etwas Datenverkehr zu Ihrer Website. Sie können die ```About```-Seite Ihrer Website mehrmals aktualisieren.

4. Warten Sie 2 bis 5 Minuten, damit die Ereignisse in Application Insights aggregiert werden können.

5. Navigieren Sie zum Application Insights-Leistungsbereich im Azure-Portal. Sie sehen, dass Profilerablaufverfolgungen in der unteren rechten Ecke verfügbar sind.

    ![Ablaufverfolgungen anzeigen](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="enable-button-in-profiler-configuration-pane-does-not-work"></a>Fehler beim Verwenden der Schaltfläche „Aktivieren“ im Bereich zur Profiler-Konfiguration
**Wenn Sie Ihre App mit App Services unter Linux hosten, müssen Sie Profiler nicht erneut im App Insights-Portal im Bereich „Leistung“ aktivieren. Für die Aktivierung von Profiler muss lediglich das NuGet-Paket im Projekt einbezogen und App Insights iKey in den App-Einstellungen festgelegt werden**.

Wenn Sie dem Aktivierungsworkflow [App Insights-Profiler für Windows](./app-insights-profiler.md) folgend im Bereich zur Profiler-Konfiguration auf **Aktivieren** klicken, wird eine Fehlermeldung angezeigt, da durch Betätigen der Schaltfläche versucht wird, die Windows-Version des Profiler-Agents in der Linux-Umgebung zu installieren.

Wir arbeiten an der Lösung dieses aktivierungsbezogenen Problems.

![Sie müssen Profiler nicht erneut im Leistungsbereich aktivieren, damit Profiler in App Services unter Linux funktioniert.](./media/app-insights-profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Nächste Schritte
Gehen Sie bei Verwendung benutzerdefinierter, von App Services gehosteter Container gemäß der Anleitung unter [Enable Service Profiler for containerized ASP.NET Core application](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) (Aktivieren von Service Profiler für die containerbasierte ASP.NET Core-Anwendung) vor, um App Insights Profiler zu aktivieren.

Richten Sie Probleme oder Vorschläge an unser Github-Repository: [ApplicationInsights-Profiler-AspNetCore: Issues (ApplicationInsights-Profiler-AspNetCore: Probleme)](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
