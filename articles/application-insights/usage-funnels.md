---
title: Azure Application Insights-Trichter
description: "In diesem Artikel wird erläutert, wie Sie Trichter dazu verwenden können, sich über die Interaktionen der Kunden mit Ihrer Anwendung zu informieren."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: mbullwin
ms.openlocfilehash: 0396c59d9d95ab71f0af04029d87afbb6e47dc35
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Informieren Sie sich mit den Application Insights-Trichtern darüber, wie die Kunden Ihre Anwendung verwenden

Es ist für Ihr Unternehmen von essenzieller Bedeutung, ein Gefühl für die Kundenerfahrung zu entwickeln. Wenn in Ihrer Anwendung mehrere Schritte durchlaufen werden, müssen Sie wissen, ob die meisten Kunden den gesamten Prozess durchlaufen oder diesen an einem bestimmten Punkt beenden. Der Fortschritt beim Ausführen einer Reihe von Schritten in einer Webanwendung wird als „Trichter“ bezeichnet. Sie können die Application Insights-Trichter dazu verwenden, Erkenntnisse über Ihre Benutzer zu erhalten und die schrittweisen Konvertierungsraten zu überwachen. 

## <a name="create-your-funnel"></a>Erstellen Ihres Trichters
Bevor Sie Ihren Trichter erstellen, müssen Sie eine Entscheidung darüber treffen, welche Frage Sie mit dem Trichter beantworten möchten. Beispiel: Sie möchten wissen, wie viele Benutzer die Homepage oder ein Kundenprofil anzeigen und ein Ticket erstellen. In diesem Beispiel möchten die Besitzer des Unternehmens Fabrikam Fiber den Prozentsatz der Kunden erfahren, die ein Kundenticket erstellt haben.

Im Folgenden werden die Schritte zum Erstellen ihres Trichters beschrieben.

1. Klicken Sie im Trichtertool auf die Schaltfläche „Neu“.
1. Wählen Sie als **Zeitraum** die Option „Letzte 90 Tage“ aus der Dropdownliste aus. Wählen Sie entweder „Meine Trichter“ oder „Freigegebene Trichter“ aus.
1. Wählen Sie das Ereignis **Index** aus der Dropdownliste aus **Schritt 1** aus. 
1. Wählen Sie das Ereignis **Kunde** aus der Dropdownliste aus **Schritt 2** aus.
1. Wählen Sie das Ereignis **Erstellen** aus der Dropdownliste aus **Schritt 3** aus.
1. Fügen Sie einen Namen zu dem Trichter hinzu, und klicken Sie auf **Speichern**.

Die folgende Abbildung zeigt die vom Trichtertool generierten Daten. Hier können die Besitzer von Fabrikam erkennen, dass während der letzten 90 Tage 54,3 Prozent Ihrer Kunden, die die Homepage aufgerufen haben, ein Kundenticket erstellt haben. Sie sehen außerdem, dass 2.700 Kunden über die Homepage zum Index navigiert sind, was auf ein Aktualisierungsproblem hindeuten könnte.


![Trichtertool mit Daten](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnel-features"></a>Trichterfunktionen
1. Wenn für Ihre App Stichproben erstellt werden, wird ein entsprechendes Banner angezeigt. Wenn Sie auf das Banner klicken, wird ein Kontextbereich mit Anweisungen zum Deaktivieren der Stichprobenentnahme angezeigt. 
2. Sie können den Trichter nach [Power BI](app-insights-export-power-bi.md) exportieren.
3. Klicken Sie auf einen Schritt, um rechts ausführlichere Informationen anzuzeigen. 
4. Die Verlaufsdaten zur Konvertierung zeigen die Konvertierung in den letzten 90 Tagen. 
5. Verstehen Sie Ihre Benutzer besser, indem Sie über Trichter auf das Benutzertool zugreifen. In jedem Schritt stehen ausgewählte Benutzerfilter zur Verfügung. 

## <a name="next-steps"></a>Nächste Schritte
  * [Nutzungsübersicht](app-insights-usage-overview.md)
  * [Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights](app-insights-usage-segmentation.md)
  * [Aufbewahrung](app-insights-usage-retention.md)
  * [Arbeitsmappen](app-insights-usage-workbooks.md)
  * [Hinzufügen von Benutzerkontext](app-insights-usage-send-user-context.md)
  * [Exportieren nach Power BI](app-insights-export-power-bi.md)

