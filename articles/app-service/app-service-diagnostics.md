---
title: "Übersicht über die Azure App Service-Diagnose | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mit der App Service-Diagnose Probleme bei einer Web-App beheben können."
keywords: "App Service, Azure App Service, Diagnose, Unterstützung, Web-App, Problembehandlung, Selbsthilfe"
services: app-service
documentationcenter: 
author: jen7714
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.openlocfilehash: f027e7fbc5866a85e7f55460192a1c99a71e368e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="azure-app-service-diagnostics-overview"></a>Übersicht über die Azure App Service-Diagnose 

Wenn Sie eine Webanwendung ausführen, möchten Sie auf möglicherweise auftretende Probleme vorbereitet sein – von Fehlern mit dem Code 500 bis zu Benachrichtigungen durch Benutzer, dass die Website ausgefallen ist. Die App Service-Diagnose ist eine intelligente und interaktive Komponente, mit der Sie Probleme bei Ihrer Web-App untersuchen können, ohne dass eine Konfiguration erforderlich ist. Falls bei Ihrer Web-App Probleme auftreten, zeigt die App Service-Diagnose den Fehler auf, damit Sie die richtigen Informationen finden, um das Problem einfacher und schneller behandeln und lösen zu können. 
 
Diese Komponente ist besonders hilfreich, wenn innerhalb der letzten 24 Stunden Probleme bei der Web-App aufgetreten sind, jedoch stehen Ihnen alle Diagnosediagramme jederzeit für die Analyse zur Verfügung. Die rechte Spalte enthält zusätzliche Problembehandlungstools und Links zu hilfreicher Dokumentation und nützlichen Foren.

![Startseite](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Integritätsüberprüfung

Wenn Sie nicht wissen, welches Problem in der Web-App auftritt oder mit welchen Problembehandlungsschritten Sie beginnen sollten, empfiehlt sich zunächst das Ausführen einer Integritätsüberprüfung. Bei der Integritätsüberprüfung werden Ihre Webanwendungen analysiert, um Ihnen eine schnelle, interaktive Übersicht zu bieten. In dieser werden ordnungsgemäße und problembehaftete Elemente angegeben, und Sie erhalten Hinweise auf Bereiche, die Sie zum Untersuchen des Problems überprüfen sollten. Die intelligente und interaktive Benutzeroberfläche leitet Sie durch den Prozess der Problembehandlung.  

![Integritätsüberprüfung](./media/app-service-diagnostics/HealthCheckup2.png)

Wenn innerhalb der letzten 24 Stunden ein Problem einer bestimmten Kategorie erkannt wurde, können Sie den vollständigen Diagnosebericht anzeigen. Möglicherweise werden Sie von der App Service-Diagnose aufgefordert, weitere Hinweise zur Fehlerbehandlung und die nächsten Schritte anzuzeigen, um eine umfassendere Anleitung zu erhalten.

![Problembehandlung und nächste Schritte](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Kachelverknüpfungen

Wenn Sie genau wissen, welche Fehlerbehandlungsinformationen Sie suchen, gelangen Sie mit den Kachelverknüpfungen direkt zum vollständigen Diagnosebericht der relevanten Problemkategorie. Die Kachelverknüpfungen bieten direkteren Zugriff auf die Diagnosemetriken als die Integritätsüberprüfung, jedoch weniger Anleitung hierzu.  

![Kachelverknüpfungen](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Diagnosebericht

Wenn Sie nach dem Ausführen einer [Integritätsüberprüfung](#health-checkup) weitere Informationen wünschen oder auf eine der [Kachelverknüpfungen](#tile-shortcuts) geklickt haben, werden im vollständigen Diagnosebericht relevante Metrikdiagramme für die letzten 24 Stunden angezeigt. Wenn es bei Ihrer App zu Downtime kommt, wird dies durch einen orangen Balken unter der Zeitachse dargestellt. Sie können eine der Downtimes auswählen, um Analysen der Ausfallzeit und Lösungsvorschläge zu erhalten. 

![Diagnosebericht](./media/app-service-diagnostics/DiagnosticReport5.png)

## <a name="open-app-service-diagnostics"></a>Öffnen der App Service-Diagnose

Navigieren Sie für den Zugriff auf die App Service-Diagnose zu Ihrer App Service-Web-App im [Azure-Portal](https://portal.azure.com). 

Klicken Sie im linken Navigationsbereich auf **Diagnose und Problembehandlung**.