---
title: Übersicht über die Azure App Service-Diagnose | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit der App Service-Diagnose Probleme bei einer Web-App beheben können.
keywords: App Service, Azure App Service, Diagnose, Unterstützung, Web-App, Problembehandlung, Selbsthilfe
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.openlocfilehash: 50e0e9f5edc18aac42ee80e232f70e09736124bc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33761886"
---
# <a name="azure-app-service-diagnostics-overview"></a>Übersicht über die Azure App Service-Diagnose 

Wenn Sie eine Webanwendung ausführen, möchten Sie auf möglicherweise auftretende Probleme vorbereitet sein – von Fehlern mit dem Code 500 bis zu Benachrichtigungen durch Benutzer, dass die Website ausgefallen ist. Die App Service-Diagnose ist eine intelligente und interaktive Komponente, mit der Sie Probleme bei Ihrer Web-App untersuchen können, ohne dass eine Konfiguration erforderlich ist. Falls bei Ihrer Web-App Probleme auftreten, zeigt die App Service-Diagnose den Fehler auf, damit Sie die richtigen Informationen finden, um das Problem einfacher und schneller behandeln und lösen zu können. 
 
Diese Komponente ist besonders hilfreich, wenn innerhalb der letzten 24 Stunden Probleme bei der Web-App aufgetreten sind, jedoch stehen Ihnen alle Diagnosediagramme jederzeit für die Analyse zur Verfügung. Die rechte Spalte enthält zusätzliche Problembehandlungstools und Links zu hilfreicher Dokumentation und nützlichen Foren.

Die App Service-Diagnose funktioniert nicht nur bei Ihrer App unter Windows, sondern auch bei Apps für [Linux-Container](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro), die [App Service-Umgebung](https://docs.microsoft.com/en-us/azure/app-service/environment/intro) und [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview). 

## <a name="open-app-service-diagnostics"></a>Öffnen der App Service-Diagnose

Navigieren Sie für den Zugriff auf die App Service-Diagnose zu Ihrer App Service-App oder App Service-Umgebung im [Azure-Portal](https://portal.azure.com). Klicken Sie im linken Navigationsbereich auf **Diagnose und Problembehandlung**. 

Navigieren Sie für Azure Functions zu Ihrer Funktionen-App, klicken Sie im oberen Navigationsbereich auf **Plattformfeatures**, und wählen Sie **Diagnose und Problembehandlung** im Abschnitt **Überwachung** aus. 

![Startseite](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Integritätsüberprüfung

Wenn Sie nicht wissen, welches Problem in der Web-App auftritt oder mit welchen Problembehandlungsschritten Sie beginnen sollten, empfiehlt sich zunächst das Ausführen einer Integritätsüberprüfung. Bei der Integritätsüberprüfung werden Ihre Webanwendungen analysiert, um Ihnen eine schnelle, interaktive Übersicht zu bieten. In dieser werden ordnungsgemäße und problembehaftete Elemente angegeben, und Sie erhalten Hinweise auf Bereiche, die Sie zum Untersuchen des Problems überprüfen sollten. Die intelligente und interaktive Benutzeroberfläche leitet Sie durch den Prozess der Problembehandlung.  

![Integritätsüberprüfung](./media/app-service-diagnostics/HealthCheckup2.png)

Wenn innerhalb der letzten 24 Stunden ein Problem einer bestimmten Kategorie erkannt wurde, können Sie den vollständigen Diagnosebericht anzeigen. Möglicherweise werden Sie von der App Service-Diagnose aufgefordert, weitere Hinweise zur Fehlerbehandlung und die nächsten Schritte anzuzeigen, um eine umfassendere Anleitung zu erhalten.

![Problembehandlung und nächste Schritte](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Kachelverknüpfungen

Wenn Sie genau wissen, welche Fehlerbehandlungsinformationen Sie suchen, gelangen Sie mit den Kachelverknüpfungen direkt zum vollständigen Diagnosebericht der relevanten Problemkategorie. Die Kachelverknüpfungen bieten direkteren Zugriff auf die Diagnosemetriken als die Integritätsüberprüfung, jedoch weniger Anleitung hierzu. Als Teil der Kachelverknüpfungen finden Sie dort auch die **Diagnosetools**, fortschrittlichere Tools, mit denen Sie u.a. Probleme im Zusammenhang mit Anwendungscode, Verlangsamung und Verbindungszeichenfolgen untersuchen können. 

![Kachelverknüpfungen](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Diagnosebericht

Wenn Sie nach dem Ausführen einer [Integritätsüberprüfung](#health-checkup) weitere Informationen wünschen oder auf eine der [Kachelverknüpfungen](#tile-shortcuts) geklickt haben, werden im vollständigen Diagnosebericht relevante Metrikdiagramme für die letzten 24 Stunden angezeigt. Wenn es bei Ihrer App zu Downtime kommt, wird dies durch einen orangen Balken unter der Zeitachse dargestellt. Sie können zur Auswahl der Ausfallzeit einen der orangefarbenen Balken auswählen, um Beobachtungen zu dieser Ausfallzeit und die vorgeschlagenen Schritte zur Problembehandlung anzuzeigen. 

![Diagnosebericht](./media/app-service-diagnostics/DiagnosticReport5.png)


## <a name="investigating-application-code-issues"></a>Untersuchen von Problemen mit Anwendungscode

Da viele App-Probleme auf Ihren Anwendungscode zurückzuführen sind, ist die App Service-Diagnose in [Application Insights](https://azure.microsoft.com/services/application-insights/) integriert, um Ausnahmen und Probleme mit Abhängigkeiten zur entsprechend ausgewählten Ausfallzeiten hervorzuheben. Application Insights muss separat aktiviert werden. 

Wählen Sie zum Anzeigen von Application Insights-Ausnahmen und -Abhängigkeiten die Kachelverknüpfung **Web-App ausgefallen** oder **Web-App langsam** aus. 

![Application Insights](./media/app-service-diagnostics/AppInsights6.png)

