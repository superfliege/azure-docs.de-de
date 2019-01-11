---
title: Profilerstellung für Live-Azure App Service-Apps mit Application Insights | Microsoft-Dokumentation
description: Profilerstellung für Live-Apps in Azure App Service mit Application Insights Profiler
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: daba72639e190bb255dd515237d25c20df433901
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075021"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Profilerstellung für Live-Azure App Service-Apps mit Application Insights

Profiler kann momentan für ASP.NET- und ASP.NET Core-Apps verwendet werden, die unter Azure App Service ausgeführt werden. Zur Verwendung von Profiler ist der Diensttarif „Basic“ oder höher erforderlich. Profiler unter Linux lässt sich derzeit nur mit [dieser Methode](profiler-aspnetcore-linux.md) aktivieren.

## <a id="installation"></a> Aktivieren von Profiler für Ihre App
Befolgen Sie die unten angegebene Anleitung, um Profiler für eine App zu aktivieren. Wenn Sie eine andere Art von Azure-Dienst ausführen, helfen Ihnen diese Anleitungen zum Aktivieren von Profiler auf anderen unterstützten Plattformen weiter:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md ?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-Anwendungen](../../azure-monitor/app/profiler-servicefabric.md ?toc=/azure/azure-monitor/toc.json)
* [Virtuelle Computer](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler ist als Teil der App Services-Runtime zwar bereits vorinstalliert, muss aber noch aktiviert werden, um Profile für Ihre App Service-Apps zu erhalten. Führen Sie die unten angegebenen Schritte zum Aktivieren von Profiler aus, nachdem Sie eine App bereitgestellt haben (auch wenn Sie das Application Insights SDK in den Quellcode eingefügt haben).

1. Wechseln Sie zum Bereich **App Services** im Azure-Portal.
2. Navigieren Sie zum Bereich **Einstellungen > Application Insights**.

   ![Aktivieren von App Insights im App Services-Portal](./media/profiler/AppInsights-AppServices.png)

3. Befolgen Sie die Anweisungen im Bereich zum Erstellen einer neuen Ressource, oder wählen Sie eine vorhandene App Insights-Ressource aus, um Ihre App zu überwachen. Vergewissern Sie sich außerdem, dass der Profiler auf **Ein** festgelegt ist.

   ![Hinzufügen der App Insights-Websiteerweiterung][Enablement UI]

4. Der Profiler wurde mithilfe einer App Services-App-Einstellung aktiviert.

    ![App-Einstellung für Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Deaktivieren von Profiler

Wenn Sie Profiler für eine einzelne Instanz einer App beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur App-Ressource. Wenn Sie Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

![Deaktivieren von Profiler für einen Webauftrag][disable-profiler-webjob]

Es wird empfohlen, Profiler für alle Apps zu aktivieren, um Leistungsprobleme möglichst schnell zu ermitteln.

Wenn Sie Änderungen an Ihrer Webanwendung mithilfe von WebDeploy bereitstellen, sollten Sie darauf achten, dass der Ordner „App_Data“ bei der Bereitstellung nicht gelöscht wird. Andernfalls werden die Dateien der Profiler-Erweiterung gelöscht, wenn Sie die Webanwendung das nächste Mal in Azure bereitstellen.



## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
