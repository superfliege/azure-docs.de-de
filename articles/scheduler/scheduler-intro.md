---
title: Was ist Azure Scheduler? | Microsoft Docs
description: "Azure Scheduler können Sie deklarativ zu beschreiben, Aktionen, die in der Cloud ausgeführt. Anschließend plant und diese Aktionen automatisch ausgeführt."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: a3bf1aacd6978499d7ef77cbcb451a06b857ac38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-scheduler"></a>Was ist Azure Scheduler?
Azure Scheduler können Sie deklarativ zu beschreiben, Aktionen, die in der Cloud ausgeführt. Anschließend plant und diese Aktionen automatisch ausgeführt.  Scheduler geschieht mithilfe [Azure-Portal](scheduler-get-started-portal.md), Code [REST-API](https://msdn.microsoft.com/library/mt629143.aspx), oder über Azure PowerShell.

Planer erstellt, verwaltet und geplante Arbeit aufruft.  Scheduler arbeitsauslastungen hosten oder Ausführen von Code nicht. Er nur *ruft* Code gehostet an anderer Stelle – in Azure, lokal oder bei einem anderen Anbieter. Sie ruft über HTTP, HTTPS, eine Speicherwarteschlange, Servicebus-Warteschlange oder ein Servicebus-Thema.

Scheduler-Zeitpläne [Aufträge](scheduler-concepts-terms.md), protokolliert den Verlauf der Auftragsausführungsergebnisse, dass eine kann überprüfen, und deterministisch und zuverlässig plant arbeitsauslastungen ausgeführt werden. Azure WebJobs (Teil der Web-Apps-Funktion in Azure App Service) und anderen Azure-Planungsfunktionen verwenden Scheduler im Hintergrund. Die [Scheduler-REST-API](https://msdn.microsoft.com/library/mt629143.aspx) hilft bei die Kommunikation für diese Aktionen zu verwalten. Daher HPC Scheduler unterstützt [komplexe Zeitpläne und erweiterte Wiederholung](scheduler-advanced-complexity.md) einfach.

Es gibt mehrere Szenarien, die für die Verwendung eines Planers geeignet. Beispiel:

* *Wiederkehrende Anwendungsaktionen:* Sammeln von Daten in regelmäßigen Abständen von Twitter zu einem Feed.
* *Tägliche Wartung:* tägliche pflegen der Protokolle, Erstellen von Sicherungen und andere Wartungsaufgaben. Beispielsweise kann ein Administrator wählen Sie zum Sichern der Datenbank um 1:00 Uhr täglich für die nächsten neun Monate.

Scheduler können Sie erstellen, aktualisieren, löschen, anzeigen und Verwalten von Aufträgen und [Auftrag Sammlungen](scheduler-concepts-terms.md) programmgesteuert, durch die Verwendung von Skripts, und klicken Sie im Portal.

## <a name="see-also"></a>Weitere Informationen:
 [Azure Scheduler-Konzepte,-Terminologie und-Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Gewusst wie: Erstellen von komplexen Zeitpläne und erweiterte Wiederholung mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler-REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Referenz zur Azure Scheduler-PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler – hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler – Einschränkungen, Standardwerte und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung mit Azure Scheduler](scheduler-outbound-authentication.md)

