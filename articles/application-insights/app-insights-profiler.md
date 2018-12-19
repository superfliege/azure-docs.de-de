---
title: Profilerstellung für Live-Azure-Web-Apps mit Application Insights | Microsoft-Dokumentation
description: Profilerstellung für Live-Web-Apps in Azure mit Application Insights Profiler
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
ms.openlocfilehash: 4d957c26bd4e4ae278c0909c9df1476b02954b86
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138006"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profilerstellung für Live-Azure-Web-Apps mit Application Insights

Profiler kann derzeit für ASP.NET- und ASP.NET Core-Web-Apps verwendet werden, die unter Web-Apps ausgeführt werden. Zur Verwendung von Profiler ist der Diensttarif „Basic“ oder höher erforderlich. Profiler unter Linux lässt sich derzeit nur mit [dieser Methode](app-insights-profiler-aspnetcore-linux.md) aktivieren.

## <a id="installation"></a> Aktivieren von Profiler für Ihre Web-Apps
Befolgen Sie die unten angegebene Anleitung, um Profiler für eine Web-App zu aktivieren. Wenn Sie eine andere Art von Azure-Dienst ausführen, helfen Ihnen diese Anleitungen zum Aktivieren von Profiler auf anderen unterstützten Plattformen weiter:
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-Anwendungen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuelle Computer](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler ist als Teil der App Services-Runtime zwar bereits vorinstalliert, muss aber noch aktiviert werden, um Profile für Ihre Azure-Web-Apps zu erhalten. Führen Sie die unten angegebenen Schritte zum Aktivieren von Profiler aus, nachdem Sie eine Web-App bereitgestellt haben (auch wenn Sie das Application Insights SDK in den Quellcode eingefügt haben).

1. Wechseln Sie zum Bereich **App Services** im Azure-Portal.
1. Navigieren Sie zum Bereich **Einstellungen > Application Insights**.

   ![Aktivieren von App Insights im App Services-Portal](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Befolgen Sie entweder die Anweisungen im Bereich zum Erstellen einer neuen Ressource, oder wählen Sie eine vorhandene App Insights-Ressource zum Überwachen Ihrer Webs-App aus. Vergewissern Sie sich außerdem, dass der Profiler auf **Ein** festgelegt ist.

   ![Hinzufügen der App Insights-Websiteerweiterung][Enablement UI]

1. Der Profiler wurde mithilfe einer App Services-App-Einstellung aktiviert.

    ![App-Einstellung für Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Deaktivieren von Profiler

Wenn Sie Profiler für eine einzelne Instanz einer Web-App beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur Web-Apps-Ressource. Wenn Sie Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

![Deaktivieren von Profiler für einen Webauftrag][disable-profiler-webjob]

Es wird empfohlen, Profiler für alle Web-Apps zu aktivieren, um Leistungsprobleme möglichst schnell zu ermitteln.

Wenn Sie Änderungen an Ihrer Webanwendung mithilfe von WebDeploy bereitstellen, sollten Sie darauf achten, dass der Ordner „App_Data“ bei der Bereitstellung nicht gelöscht wird. Andernfalls werden die Dateien der Profiler-Erweiterung gelöscht, wenn Sie die Webanwendung das nächste Mal in Azure bereitstellen.



## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png

