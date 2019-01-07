---
title: Beheben der Fehler „502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“ – Azure App Service | Microsoft-Dokumentation
description: Behebung der Fehler „502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“ in Ihrer Web-App, die im Azure App Service gehostet wird.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: „502 Ungültiges Gateway“, „503 Dienst nicht verfügbar“, Fehler 503, Fehler 502
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 71842f9dbc8d0454da1847c956dea3b063208836
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53388477"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Problembehandlung bei HTTP-Fehler „502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“ in Ihren Azure-Web-Apps
„502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“ sind häufige Fehler in Ihrer Web-App, die im [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714)gehostet wird. Dieser Artikel hilft Ihnen, diese Fehler zu beheben.

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Ihre Frage im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/)stellen, um dort Hilfe von Azure-Experten zu erhalten. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und klicken Sie auf **Support erhalten**.

## <a name="symptom"></a>Symptom
Beim Aufrufen der Web-App wird ein HTTP-Fehler „502 Ungültiges Gateway“ oder „503 Dienst nicht verfügbar“ zurückgegeben.

## <a name="cause"></a>Ursache
Diese Fehler sind häufig auf Probleme auf der Anwendungsebene zurückzuführen, z. B.:

* Anforderungen, die sehr lange dauern
* Eine Anwendung mit hoher Speicher-/CPU-Auslastung
* Eine Anwendung, die aufgrund einer Ausnahme abstürzt

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Schritte zur Behebung der Fehler „502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“
Die Problembehandlung lässt sich in diesem Fall in drei unterschiedliche Aufgaben unterteilen, die nacheinander ausgeführt werden:

1. [Beobachten und Überwachen des Anwendungsverhaltens](#observe)
2. [Sammeln von Daten](#collect)
3. [Minimieren der Auswirkungen des Problems](#mitigate)

[App Service-Web-Apps](app-service-web-overview.md) bietet Ihnen bei jedem Schritt verschiedene Optionen.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Beobachten und Überwachen des Anwendungsverhaltens
#### <a name="track-service-health"></a>Nachverfolgen der Dienstintegrität
Microsoft Azure informiert jeweils aktuell über Dienstunterbrechungen oder Leistungsbeeinträchtigungen. Sie können die Integrität des Diensts im [Azure-Portal](https://portal.azure.com/)nachverfolgen. Weitere Informationen finden Sie unter [Nachverfolgen der Dienstintegrität](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Überwachen Ihrer Web-App
Durch das Überwachen der Web-App können Sie herausfinden, ob in Ihrer Anwendung Probleme vorliegen. Klicken Sie auf dem Blatt Ihrer Web-App auf die Kachel **Anforderungen und Fehler** . Auf dem Blatt **Metrik** werden alle Metriken angezeigt, die Sie hinzufügen können.

Folgende Metriken können Sie z. B. für Ihre Web-App überwachen:

* Durchschnittlicher Arbeitssatz für Arbeitsspeicher
* Durchschnittliche Reaktionszeit
* CPU-Zeit
* Arbeitssatz für Arbeitsspeicher
* Requests

![Überwachen der Web-App bei Behebung der HTTP-Fehler „502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Weitere Informationen finden Sie unter

* [Überwachen von Web-Apps in Azure App Service](web-sites-monitor.md)
* [Empfangen von Warnbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Sammeln von Daten
#### <a name="use-the-diagnostics-tool"></a>Verwenden des Diagnosetools
App Service stellt eine intelligente und interaktive Komponente bereit, mit der Sie Probleme bei Ihrer Web-App untersuchen können, ohne dass eine Konfiguration erforderlich ist. Wenn bei Ihrer Web-App Probleme auftreten, zeigt das Diagnosetool den Fehler auf, damit Sie die richtigen Informationen finden, um das Problem einfacher und schneller behandeln und lösen zu können.

Navigieren Sie für den Zugriff auf die App Service-Diagnose zu Ihrer App Service-App oder App Service-Umgebung im [Azure-Portal](https://portal.azure.com). Klicken Sie im linken Navigationsbereich auf **Diagnose und Problembehandlung**.

#### <a name="use-the-kudu-debug-console"></a>Verwenden der Kudu-Debugkonsole
Web-Apps enthält eine Debugkonsole, die Sie zum Debuggen, Untersuchen und Hochladen von Dateien verwenden können, sowie JSON-Endpunkte zum Abrufen von Informationen über Ihre Umgebung. Dieses Feature wird als *Kudu-Konsole* oder *SCM-Dashboard* für Ihre Web-App bezeichnet.

Sie können über den Link **https://&lt;Name_Ihrer_App>.scm.azurewebsites.net/** auf das Dashboard zugreifen.

Die Kudu-Konsole bietet z. B. folgende Funktionen:

* Umgebungseinstellungen für Ihre Anwendung
* Protokolldatenstrom
* Speicherabbild zu Diagnosezwecken
* Debugkonsole zum Ausführen von Powershell-Cmdlets und grundlegenden DOS-Befehlen

Eine weitere nützliche Funktion von Kudu ist die Möglichkeit, mithilfe von Kudu und dem SysInternals-Tool „Procdump“ Speicherabbilddateien zu erstellen, wenn Ihre Anwendung Ausnahmen (erste Chance) auslöst. Bei diesen Speicherabbilddateien handelt es sich um Momentaufnahmen des Prozesses, die beim Beheben komplizierterer Probleme mit Ihrer Web-App hilfreich sein können.

Weitere Informationen zu den verfügbaren Funktionen in Kudu finden Sie unter [Online-Tools für Azure-Websites, die Sie kennen sollten](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Minimieren der Auswirkungen des Problems
#### <a name="scale-the-web-app"></a>Skalieren der Web-App
In Azure App Service können Sie zum Steigern der Leistung und des Durchsatzes die Skalierung anpassen, mit der Sie Ihre Anwendung ausführen. Zum Hochskalieren einer Web-App müssen Sie zwei zusammenhängende Schritte ausführen: Für den App Service-Plan muss ein höherer Tarif verwendet und nach dem Tarifwechsel müssen bestimmte Einstellungen konfiguriert werden.

Weitere Informationen zur Skalierung finden Sie unter [Skalieren einer Web-App in Azure App Service](web-sites-scale.md).

Außerdem haben Sie die Möglichkeit, Ihre Anwendung auf mehreren Instanzen auszuführen. Dies erhöht nicht nur die Verarbeitungskapazität, sondern bietet auch ein gewisses Maß an Fehlertoleranz. Wenn der Prozess in einer Instanz ausfällt, kann die andere Instanz weiterhin Anfragen verarbeiten.

Sie können die Skalierung auf „Manuell“ oder „Automatisch“ festlegen.

#### <a name="use-autoheal"></a>Verwenden von „AutoHeal“
„AutoHeal“ startet den Arbeitsprozess für Ihre App basierend auf von Ihnen ausgewählten Einstellungen neu (z. B. Konfigurationsänderungen, Anforderungen, speicherbasierte Grenzwerte oder zum Ausführen einer Anforderung benötigte Zeit). In den meisten Fällen ist das Neustarten des Prozesses die schnellste Methode zum Beheben eines Problems. Sie können die Web-App zwar immer direkt im Azure-Portal neu starten, „AutoHeal“ führt diesen Schritt jedoch automatisch für Sie aus. Sie müssen lediglich einige Trigger in der Stammdatei „web.config“ Ihrer Web-App hinzufügen. Beachten Sie, dass diese Einstellungen immer auf die gleiche Weise angewendet werden, auch dann, wenn es sich bei Ihrer Anwendung nicht um eine .Net-Anwendung handelt.

Weitere Informationen finden Sie unter [Automatische Reparatur von Azure-Websites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Neustarten der Web-App
Dies ist oft die einfachste Methode zum Beheben einmaliger Probleme. Im [Azure-Portal](https://portal.azure.com/)stehen auf dem Blatt Ihrer Web-App Optionen zum Beenden oder Neustarten der App zur Verfügung.

 ![Neustart der App zur Behebung der HTTP-Fehler „502 Ungültiges Gateway“ und „503 Dienst nicht verfügbar“](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Sie können Ihre Web-App auch mit Azure PowerShell verwalten. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).

