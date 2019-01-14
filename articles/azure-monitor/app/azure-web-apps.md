---
title: Überwachen der Leistung von Azure App Service | Microsoft-Dokumentation
description: Überwachung der Anwendungsleistung für Azure-Web-Apps. Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: mbullwin
ms.openlocfilehash: a5aea15c2563a81f4e915baa1b332beda64a1a70
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53970966"
---
# <a name="monitor-azure-app-service-performance"></a>Überwachen der Leistung von Azure App Service
Im [Azure-Portal](https://portal.azure.com) können Sie eine Anwendungsleistungsüberwachung für Ihre Web-Apps, mobilen Back-Ends und API-Apps in [Azure App Service](../../app-service/overview.md) einrichten. [Azure Application Insights](../../application-insights/app-insights-overview.md) instrumentiert Ihre App, um Telemetriedaten über ihre Aktivitäten an den Application Insights-Dienst zu senden, wo sie gespeichert und analysiert werden. Hier können Sie dann mithilfe von Metrikdiagrammen und Suchtools Probleme diagnostizieren, die Leistung verbessern und die Nutzung untersuchen.

## <a name="run-time-or-build-time"></a>Laufzeit oder Buildzeit
Zum Konfigurieren der Überwachung kann die App auf zwei Arten instrumentiert werden:

* **Laufzeit:** Sie können eine Erweiterung für die Leistungsüberwachung auswählen, wenn Ihre Web-App bereits live geschaltet wurde. Die App muss dazu nicht neu erstellt oder erneut installiert werden. Sie erhalten einen Standardsatz von Paketen zur Überwachung von Antwortzeiten, Erfolgsraten, Ausnahmen, Abhängigkeiten und Ähnlichem. 
* **Buildzeit:** Sie können ein Paket in der App installieren, die Sie gerade entwickeln. Diese Option ist wesentlich flexibler. Neben den gleichen Standardpaketen können Sie auch Code schreiben, um die Telemetrie anzupassen oder Ihre eigenen Telemetriedaten zu senden. Sie können bestimmte Aktivitäten protokollieren oder Ereignisse gemäß der Semantik Ihrer App-Domäne erfassen. 

## <a name="run-time-instrumentation-with-application-insights"></a>Laufzeitinstrumentierung mit Application Insights
Wenn Sie bereits eine Web-App in Azure ausführen, verfügen Sie schon über eine Art der Überwachung, und zwar von Anforderungs- und Fehlerraten. Fügen Sie Application Insights hinzu, um mehr zu erhalten – beispielsweise Reaktionszeiten, Überwachung von Abhängigkeitsaufrufen, intelligente Erkennung und leistungsstarke Log Analytics-Abfragesprache. 

1. In der Azure-Systemsteuerung für Ihre Web-App können Sie **Application Insights auswählen**.

    ![Auswählen von „Application Insights“ unter „Einstellungen“](./media/azure-web-apps/settings-app-insights.png)

   * Wählen Sie die Option zum Erstellen einer neuen Ressource, sofern Sie nicht bereits eine Application Insights-Ressource für diese Anwendung eingerichtet haben. 

    > [!NOTE]
    > Beim Klicken auf **OK** zum Erstellen der neuen Ressource wird die Aufforderung **Überwachungseinstellungen anwenden** angezeigt. Wenn Sie **Weiter** wählen, wird Ihre neue Application Insights-Ressource mit Ihrer Web-App verknüpft, und außerdem wird **ein Neustart Ihrer Web-App ausgelöst**. 

    ![Instrumentieren Ihrer Web-App](./media/azure-web-apps/create-resource.png)

2. Nach Angabe der zu verwendenden Ressource können Sie plattformspezifisch auswählen, wie Application Insights Daten für Ihre Anwendung erfassen soll.

    ![Auswählen plattformspezifischer Optionen](./media/azure-web-apps/choose-options.png)

3. **Instrumentieren Sie Ihre Web-App**, nachdem Application Insights installiert wurde.

   **Aktivieren Sie die clientseitige Überwachung** für Seitenansicht und Benutzertelemetrie.

   * Wählen Sie „Einstellungen“ > „Anwendungseinstellungen“ aus.
   * Fügen Sie unter „App-Einstellungen“ ein neues Schlüssel-Wert-Paar hinzu:

    Schlüssel: `APPINSIGHTS_JAVASCRIPT_ENABLED`

    Wert: `true`
   * **Speichern** Sie die Einstellungen, und **starten Sie die App neu**.
4. Erkunden Sie die Überwachungsdaten Ihrer App, indem Sie **Einstellungen** > **Application Insights** > **Weitere Informationen in Application Insights anzeigen** wählen.

Später können Sie die App bei Bedarf mit Application Insights erstellen.

*Wie entferne ich Application Insights oder stelle auf das Senden an eine andere Ressource um?*

* Öffnen Sie in Azure das Steuerungsblatt der Web-App und anschließend **Application Insights** (unter „Einstellungen“). Sie können Application Insights deaktivieren, indem Sie im oberen Bereich auf **Deaktivieren** klicken. Alternativ können Sie im Abschnitt **Change your resource** (Ressource ändern) eine neue Ressource auswählen.

## <a name="build-the-app-with-application-insights"></a>Erstellen der App mit Application Insights
Application Insights kann durch Installieren eines SDK in Ihrer App eine detailliertere Telemetrie bereitstellen. Beispielsweise können Sie Ablaufverfolgungsprotokolle erfassen, [benutzerdefinierte Telemetriedaten schreiben](../../azure-monitor/app/api-custom-events-metrics.md) und ausführlichere Berichte zu Ausnahmen erhalten.

1. Konfigurieren Sie in **Visual Studio** (2013 Update 2 oder höher) Application Insights für Ihr Projekt.

    Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie **Hinzufügen > Application Insights** oder **Projekt** > **Application Insights** > **Application Insights konfigurieren**.

    ![Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie „"Application Insights hinzufügen“ oder „Application Insights konfigurieren“.](./media/azure-web-apps/03-add.png)

    Wenn Sie zur Anmeldung aufgefordert werden, verwenden Sie die Anmeldeinformationen für Ihr Azure-Konto.

    Der Vorgang hat zwei Auswirkungen:

   1. Es wird eine Application Insights-Ressource in Azure erstellt, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.
   2. Das Application Insights-NuGet-Paket wird dem Code hinzugefügt (sofern noch nicht vorhanden) und so konfiguriert, dass Telemetriedaten an die Azure-Ressource gesendet werden.
2. **Testen Sie die Telemetrie**, indem Sie die App auf Ihrem Entwicklungscomputer (F5) ausführen.
3. **Veröffentlichen Sie die App** wie gewohnt in Azure. 

*Wie führe ich die Umstellung durch, damit Daten an eine andere Application Insights-Ressource gesendet werden?*

* Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights konfigurieren** und dann die gewünschte Ressource aus. Sie erhalten die Möglichkeit, eine neue Ressource zu erstellen. Führen Sie die Neuerstellung und dann die erneute Bereitstellung durch.

## <a name="more-telemetry"></a>Mehr Telemetrie

* [Ladedaten für Webseiten](../../azure-monitor/app/javascript.md)
* [Benutzerdefinierte Telemetrie](../../azure-monitor/app/api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nächste Schritte
* [Ausführen des Profilers in Ihrer Live-App](../../application-insights/app-insights-profiler.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): Überwachen Sie Azure Functions mit Application Insights
* [Ermöglichen des Sendens von Azure-Diagnosedaten an Application Insights](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Überwachen von Dienstintegritätsmetriken](../../azure-monitor/platform/data-collection.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Empfangen von Warnbenachrichtigungen](../../azure-monitor/platform/alerts-overview.md) , wenn ein Vorgangsereignis auftritt oder Metriken einen Schwellenwert überschreiten.
* Verwenden von [Application Insights für JavaScript-Apps und Webseiten](../../azure-monitor/app/javascript.md), um Clienttelemetriedaten von den Browsern zu erhalten, mit denen auf eine Webseite zugegriffen wird
* [Einrichten von Verfügbarkeitswebtests](../../azure-monitor/app/monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihre Website nicht verfügbar ist

