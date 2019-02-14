---
title: Profilerstellung für ASP.NET Core-Azure Linux-Web-Apps mit Application Insights Profiler | Microsoft-Dokumentation
description: Eine konzeptionelle Übersicht und ein schrittweises Tutorial zur Verwendung von Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.reviewer: mbullwin
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: cweining
ms.openlocfilehash: 5787db7e2b726a10891fcabb0b215399d0d4e0ae
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884306"
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profilerstellung für ASP.NET Core-Azure Linux-Web-Apps mit Application Insights Profiler

Diese Funktion steht derzeit als Vorschau zur Verfügung.

Ermitteln Sie durch Verwendung von [Application Insights](../../azure-monitor/app/app-insights-overview.md) den Zeitaufwand für die einzelnen Methoden in Ihrer Live-Webanwendung. Application Insights Profiler ist jetzt für ASP.NET Core-Web-Apps verfügbar, die in Linux auf Azure App Service gehostet werden. Dieser Leitfaden enthält eine schrittweise Anleitung zum Erfassen von Profiler-Ablaufverfolgungen für ASP.NET Core-Linux-Web-Apps.

Nach Abschluss dieser exemplarischen Vorgehensweise kann Ihre App Profiler-Ablaufverfolgungen wie die Ablaufverfolgungsdaten erfassen, die in der Abbildung dargestellt sind. In diesem Beispiel weist die Profiler-Ablaufverfolgung nach, dass eine bestimmte Webanforderung aufgrund der Wartezeit langsam ist. Der *langsamste Pfad* im Code, der die App verlangsamt, wird durch ein Flammensymbol gekennzeichnet. Die **About**-Methode im Abschnitt **HomeController** verlangsamt die Web-App, da die Methode die **Thread.Sleep**-Funktion aufruft.

![Profilerablaufverfolgungen](./media/profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Voraussetzungen
Die folgenden Anweisungen werden auf alle Windows-, Linux- und Mac-Entwicklungsumgebungen angewendet:

* Installieren Sie [.NET Core SDK 2.1.2 oder höher](https://dotnet.microsoft.com/download/archives).
* Installieren Sie Git nach den Anweisungen unter [Getting Started – Installing Git (Erste Schritte – Installieren von Git)](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## <a name="set-up-the-project-locally"></a>Lokales Einrichten des Projekts

1. Öffnen Sie ein Eingabeaufforderungsfenster auf dem Computer. Die folgenden Anweisungen werden auf alle Windows-, Linux- und Mac-Entwicklungsumgebungen angewendet.

2. Erstellen Sie eine ASP.NET Core-MVC-Webanwendung:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

3. Ändern Sie das Arbeitsverzeichnis in den Stammordner des Projekts.

4. Fügen Sie das NuGet-Paket hinzu, um die Profiler-Ablaufverfolgungen zu erfassen:

    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

5. Fügen Sie im Abschnitt **HomeController.cs** eine Codezeile zur Verzögerung um ein paar Sekunden nach dem Zufallsprinzip hinzu:

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

6. Speichern Sie die Änderungen, und committen Sie sie an das lokale Repository:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>Erstellen der Linux-Web-App zum Hosten Ihres Projekts

1. Erstellen Sie die Web-App-Umgebung mithilfe von App Service unter Linux:

    ![Erstellen der Linux-Web-App](./media/profiler-aspnetcore-linux/create-linux-appservice.png)

2. Erstellen Sie die Anmeldeinformationen für die Bereitstellung:

    > [!NOTE]
    > Notieren Sie Ihr Kennwort zur späteren Verwendung, wenn Sie Ihre Web-App bereitstellen.

    ![Erstellen der Anmeldeinformationen für die Bereitstellung](./media/profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Wählen Sie die Bereitstellungsoptionen aus. Richten Sie gemäß der Anweisungen im Azure-Portal ein lokales Git-Repository in der Web-App ein. Ein Git-Repository wird automatisch erstellt.

    ![Git-Repository einrichten](./media/profiler-aspnetcore-linux/setup-git-repo.png)

Weitere Bereitstellungsoptionen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Bereitstellen des Projekts

1. Navigieren Sie im Eingabeaufforderungsfenster zum Stammordner für Ihr Projekt. Fügen Sie ein Git-Remote-Repository hinzu, sodass es auf das Repository auf App Service weist:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Verwenden Sie den **username (Benutzernamen)**, den Sie verwendet haben, um die Anmeldeinformationen für die Bereitstellung zu erstellen.
    * Verwenden Sie den **app-name (App-Namen)**, den Sie verwendet haben, um die Web-App mithilfe von App Service unter Linux zu erstellen.

2. Stellen Sie das Projekt durch Pushen der Änderungen in Azure bereit:

    ```
    git push azure master
    ```

Eine Ausgabe ähnlich des folgenden Beispiels sollte angezeigt werden:

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

1. [Erstellen Sie eine Application Insights-Ressource](./../../azure-monitor/app/create-new-resource.md ).

2. Kopieren Sie den Wert **iKey** der Application Insights-Ressource, und legen Sie die folgenden Einstellungen in Ihren Web-Apps fest:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Konfigurieren der App-Einstellungen](./media/profiler-aspnetcore-linux/set-appsettings.png)

    Wenn die App-Einstellungen geändert werden, wird die Website automatisch neu gestartet. Sobald die neuen Einstellungen angewendet werden, wird der Profiler sofort für zwei Minuten ausgeführt. Anschließend wird der Profiler einmal pro Stunde für zwei Minuten ausgeführt.

3. Generieren Sie etwas Datenverkehr zu Ihrer Website. Sie können Datenverkehr generieren, indem Sie die **Info**-Seite der Website mehrmals aktualisieren.

4. Warten Sie zwei bis fünf Minuten, bis die Ereignisse auf Application Insights summiert sind.

5. Navigieren Sie zum Application Insights-**Leistungsbereich** im Azure-Portal. Sie können die Profiler-Ablaufverfolgungen unten rechts im Bereich anzeigen.

    ![Profiler-Ablaufverfolgungen anzeigen](./media/profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="the-enable-action-in-the-profiler-configuration-pane-doesnt-work"></a>Die Aktion „Aktivieren“ im Bereich „Profilerkonfiguration“ funktioniert nicht

> [!NOTE]
> Wenn Sie Ihre App mithilfe von App Service unter Linux hosten, müssen Sie den Profiler nicht im Application Insights-Portal im Bereich **Leistung** reaktivieren. Sie können das NuGet-Paket in Ihr Projekt einschließen und den **iKey**-Wert von Application Insights in Ihren Web-App-Einstellungen festlegen, um den Profiler zu aktivieren.

Wenn Sie den Aktivierungsworkflow für [Application Insights Profiler für Windows](./profiler.md) befolgen und **Aktivieren** im Bereich **Profiler konfigurieren** auswählen, erhalten Sie eine Fehlermeldung. Die Aktivierung versucht, die Windows-Version des Profiler-Agents für die Linux-Umgebung zu installieren.

Wir arbeiten an einer Lösung dieses Problems.

![Versuchen Sie nicht, den Profiler im Bereich „Leistung“ zu reaktivieren](./media/profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Nächste Schritte
Gehen Sie bei Verwendung benutzerdefinierter, von Azure App Service gehosteter Container gemäß der Anleitung unter [Enable Service Profiler for containerized ASP.NET Core application (Aktivieren von Service Profiler für die containerbasierte ASP.NET Core-Anwendung)](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) vor, um Application Insights Profiler zu aktivieren.

Senden Sie Berichte über Probleme oder Vorschläge an das GitHub-Repository für Application Insights: [ApplicationInsights-Profiler-AspNetCore: Issues](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues) (ApplicationInsights-Profiler-AspNetCore: Probleme)
