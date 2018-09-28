---
title: Übersicht über Azure Cost Management | Microsoft-Dokumentation
description: Azure Cost Management ist eine Lösung für die Kostenverwaltung, mit der Sie die Azure-Ausgaben überwachen und steuern und die Ressourcennutzung optimieren können.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/12/2018
ms.topic: overview
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 10fef55ef09e4be427fbd36c730528a993dd64e2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960860"
---
# <a name="what-is-azure-cost-management"></a>Was ist Azure-Kostenverwaltung?

Die Kostenverwaltung ist der Prozess, bei dem Sie die Kosten Ihrer Geschäftstätigkeit effektiv planen und steuern. Kostenverwaltungsaufgaben werden im Allgemeinen durch die Finanz-, Management- und App-Teams ausgeführt. Azure Cost Management ermöglicht Organisationen mit Microsoft Enterprise Agreements (EA) das kostenbewusste Planen. Zudem können damit Kosten effektiv analysiert werden, und es können Maßnahmen zum Optimieren der Cloudausgaben ergriffen werden. Wenn Sie sich weiter über den Kostenverwaltungsansatz von Organisationen informieren möchten, sehen Sie den Artikel [Azure Cost Management – bewährte Methoden](cost-mgt-best-practices.md) ein.

Die Abrechnung steht zwar in enger Beziehung zur Kostenverwaltung, ist jedoch nicht identisch mit dieser. Bei der Abrechnung werden Kunden Waren oder Dienstleistungen in Rechnungen gestellt, zudem werden Geschäftsbeziehungen verwaltet.  Für Abrechnungsaufgaben sind in der Regel die Beschaffungs- und Finanzteams zuständig.

Cost Management zeigt die Kosten und Verwendungsmuster der Organisation mit erweiterten Analysen. Berichte in Cost Management zeigen Azure-Kosten und -Nutzung, reservierte Instanzen sowie Nutzung des Azure-Hybridvorteils. Die Berichte liefern eine Übersicht Ihrer internen und externen Kosten. Anhand der Berichte verstehen Sie Ihre Ausgaben und Ihre Ressourcennutzung, und Sie können außergewöhnliche Ausgaben ermitteln. Vorhersageanalysen sind ebenfalls verfügbar. Cost Management zeigt anhand von Azure-Verwaltungsgruppen, Budgets und Empfehlungen deutlich die Struktur Ihrer Ausgaben sowie Möglichkeiten der Kostensenkung auf.

Über das Azure-Portal oder mithilfe diverser APIs können Sie Exportvorgänge automatisieren, um Kostendaten in externe Systeme und Prozesse zu integrieren. Automatisierter Abrechnungsdatenexport und geplante Berichte sind ebenfalls verfügbar.

## <a name="plan-and-control-expenses"></a>Ausgaben planen und steuern

Cost Management unterstützt Sie wie folgt beim Planen und Steuern Ihrer Kosten: Kostenanalyse, Budgets, Empfehlungen und Exportieren von Kostenverwaltungsdaten.

Mithilfe der Kostenanalyse ermitteln Sie für Ihre Organisation anfallende Kosten und analysieren diese. Durch die Anzeige aggregierter Kosten nach Organisationen können Sie Kostenstellen und Ausgabentrends nachvollziehen. Mithilfe der akkumulierten Kosten lassen sich zudem unter Berücksichtigung von Budgets Kostenschätzungen für monatliche, vierteljährliche oder jährliche Zeiträume erstellen.

Budgets erleichtern Ihnen das Planen und Realisieren finanzieller Zuständigkeiten in Ihrer Organisation. Mit ihnen kann verhindert werden, dass Kostenschwellenwerte und -limits überschritten werden. Anhand von Budgets können Sie auch andere über ihre Ausgaben informieren, um Kosten proaktiv zu steuern. Gemeinsam können Sie die Ausgabenentwicklung über einen Zeitraum untersuchen.

Empfehlungen zeigen Möglichkeiten der Optimierung und Steigerung der Effizienz auf, indem ungenutzte oder nicht ausreichend genutzte Ressourcen ermittelt werden. Auch kostengünstigere Möglichkeiten der Ressourcennutzung können vorgeschlagen werden. Wenn Sie aufgrund der Empfehlungen Maßnahmen ergreifen, ändern Sie Ihre Ressourcennutzung und können damit Geld zu sparen. Bevor Sie tätig werden, untersuchen Sie zunächst die Empfehlungen zur Kostenoptimierung, um mögliche ineffiziente Nutzungen aufzudecken. Anschließend befolgen Sie die Empfehlung und ändern Ihre Azure-Ressource so, dass eine kostengünstigere Option genutzt wird. Schließlich prüfen Sie die Ergebnisse, um sicherzustellen, dass die vorgenommene Änderung gewinnbringend ist.

Wenn Sie Kostenverwaltungsdaten mithilfe externer Systeme abrufen und prüfen, können Sie die Daten auf einfache Weise aus Azure exportieren. Sie können einen täglichen geplanten Export im CSV-Format einrichten und die Datendateien im Azure-Speicher speichern. Anschließend können Sie aus Ihrem externen System auf die Daten zugreifen.

## <a name="consider-cloudyn"></a>Verwendung von Cloudyn erwägen

[Cloudyn](overview.md) ist ein Azure-Dienst in Verbindung mit Cost Management. Mit Cloudyn können Sie die Cloudnutzung sowie die Ausgaben für Ihre Azure-Ressourcen nachverfolgen. Auch andere Cloudanbieter werden unterstützt, einschließlich von AWS und Google. Leicht verständliche Dashboardberichte unterstützen Sie bei der Kostenzuteilung sowie bei Showbacks und verbrauchsbasierten Kostenzuteilungen. Derzeit bietet Cost Management keine Unterstützung rückblickender/verbrauchsbasierter Kostenzuteilung oder anderer Clouddienstanbieter. Cloudyn hingegen ist eine Option, die eine solche Unterstützung _bietet_. Derzeit unterstützt Cost Management ausschließlich Azure EA-Konten. Zwar werden keine individuellen Konten, Konten mit nutzungsbasierter Bezahlung oder Microsoft Cloud-Dienstanbieter-Konten unterstützt, bei Cloudyn ist dies jedoch der Fall. Wenn Sie über ein solches Konto verfügen, können Sie Ihre Kosten mithilfe von Cloudyn verwalten.

## <a name="additional-azure-tools"></a>Zusätzliche Azure-Tools

Azure bietet weitere Tools, die im Azure Cost Management-Funktionsumfang nicht enthalten sind. Sie können dennoch eine wichtige Rolle im Kostenverwaltungsprozess spielen. Weitere Informationen zu diesen Tools finden Sie unter den folgenden Links.

- [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/): Mit diesem Tool können Sie Ihre im Voraus anfallenden Cloudkosten schätzen.
- [Azure Migrate](../migrate/migrate-overview.md): Bewerten Sie die aktuelle Arbeitsauslastung Ihrer Rechenzentren, und stellen Sie fest, welche Anforderungen eine Azure-Austauschlösung erfüllen muss.
- [Azure Advisor](../advisor/advisor-overview.md): Identifizieren Sie nicht genutzte virtuelle Computer, und erhalten Sie Empfehlungen zum Kauf von reservierten Azure-VM-Instanzen.
- [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/): Nutzen Sie Ihre aktuellen Lizenzen für lokale Windows Server oder SQL Server für VMs in Azure, um Kosten zu sparen.


## <a name="next-steps"></a>Nächste Schritte

Da Sie sich nun mit Cost Management vertraut gemacht haben, können Sie im nächsten Schritt mit der Nutzung des Diensts beginnen.

- Steigen Sie in Azure Cost Management ein, um [Kosten zu analysieren](quick-acm-cost-analysis.md).
- Weitere Informationen finden Sie auch in [Azure Cost Management – bewährte Methoden](cost-mgt-best-practices.md).
