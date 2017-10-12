---
title: "Überwachen von Integrität und Warnungen in Azure Stack | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Integrität und Warnungen in Azure Stack überwacht werden."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 69901c7b-4673-4bd8-acf2-8c6bdd9d1546
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: b2ba7ec922341464ea7160d08e475999c941c42a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Überwachen von Integrität und Warnungen in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack umfasst Funktionen zur Infrastrukturüberwachung, mit denen Sie Informationen zu Integrität und Warnungen für eine Azure Stack-Region anzeigen können. Auf der Kachel **Regionsverwaltung**, die standardmäßig im Administratorportal für das Standardanbieterabonnement angeheftet ist, werden alle bereitgestellten Regionen von Azure Stack aufgelistet. Die Kachel enthält die Anzahl aktiver kritischer Warnungen und Warnbenachrichtigungen für jede Region und ist Ihr Einstiegspunkt in die Integritäts- und Warnfunktion von Azure Stack.

 ![Kachel „Regionsverwaltung“](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>Grundlegendes zur Integrität in Azure Stack

 Die Integrität und Warnungen werden vom Integritätsressourcenanbieter verwaltet. Azure Stack-Infrastrukturkomponenten werden bei der Bereitstellung und Konfiguration von Azure Stack beim Integritätsressourcenanbieter registriert. Nach der Registrierung können die Integrität und Warnungen für jede Komponente angezeigt werden. Die Integrität in Azure Stack basiert auf einem einfachen Konzept. Wenn Warnungen für eine registrierte Instanz einer Komponente vorhanden sind, gibt der Integritätsstatus dieser Komponente den schlechtesten aktiven Warnungsschweregrad an, nämlich „Warnung“ oder „Kritisch“.
 
 ## <a name="view-and-manage-component-health-state"></a>Anzeigen und Verwalten des Integritätsstatus von Komponenten
 
 Als Azure Stack-Betreiber können Sie den Integritätsstatus der Komponenten im Administratorportal und über die Rest-API und PowerShell anzeigen.
 
Um den Integritätsstatus im Portal anzuzeigen, klicken Sie auf die Region, die in der Kachel **Regionsverwaltung** angezeigt werden soll. Sie können den Integritätsstatus der Infrastrukturrollen und der Ressourcenanbieter anzeigen. Der Computeressourcenanbieter meldet in dieser Version nicht den Integritätsstatus.

![Liste von Infrastrukturrollen](media/azure-stack-monitor-health/image2.png)

Um ausführlichere Informationen anzuzeigen, klicken Sie auf einen Ressourcenanbieter oder eine Infrastrukturrolle.

> [!WARNING]
>Wenn Sie auf eine Infrastrukturrolle und dann auf die Rolleninstanz klicken, werden Optionen zum Starten, Neustarten oder Herunterfahren angezeigt. Verwenden Sie diese Aktionen nicht beim Anwenden von Updates auf ein integriertes System. Verwenden Sie diese Optionen außerdem **nicht** in einer Azure Stack Development Kit-Umgebung. Diese Optionen sind nur für eine Umgebung mit integrierten Systemen konzipiert, in denen es mehr als eine Rolleninstanz pro Infrastrukturrolle gibt. Der Neustart einer Rolleninstanz (insbesondere „AzS-Xrp01“) im Development Kit führt zur Instabilität des Systems. Um Unterstützung bei der Problembehandlung zu erhalten, veröffentlichen Sie Ihr Problem im [Azure Stack-Forum](https://aka.ms/azurestackforum).
>
 
## <a name="view-alerts"></a>Anzeigen von Warnungen

Die Liste der aktiven Warnungen für jede Azure Stack-Region kann direkt über das Blatt **Regionsverwaltung** abgerufen werden. Die erste Kachel in der Standardkonfiguration ist die Kachel **Warnungen**, in der eine Zusammenfassung der Benachrichtigungen mit den Schweregraden „Kritisch“ und „Warnung“ für die Region angezeigt werden. Sie können die Kachel „Warnungen“ wie alle anderen Kacheln auf diesem Blatt für den Schnellzugriff an das Dashboard anheften.   

![Kachel „Warnungen“ mit einer Warnung](media/azure-stack-monitor-health/image3.png)

Durch Auswahl des oberen Teils der Kachel **Warnungen** navigieren Sie zur Liste aller aktiven Warnungen für die Region. Wenn Sie in der Kachel das Zeilenelement **Kritisch** oder **Warnung** auswählen, navigieren Sie zu einer gefilterten Liste von Warnungen („Kritisch“ oder „Warnung“). 

![Nach „Warnung“ gefilterte Benachrichtigungen](media/azure-stack-monitor-health/image4.png)
  
Auf dem Blatt **Warnungen** können Benachrichtigungen nach Status („Aktiv“ oder „Geschlossen“) oder Schweregrad („Kritisch“ oder „Warnung“) gefiltert werden. Die Standardansicht zeigt alle aktiven Warnungen an. Alle geschlossene Warnungen werden nach sieben Tagen aus dem System entfernt.

![Filterbereich zum Filtern nach dem Status „Kritisch“ oder „Warnung“](media/azure-stack-monitor-health/image5.png)

Die Aktion **API anzeigen** zeigt die zum Generieren der Listenansicht verwendete REST-API an. Anhand dieser Aktion können Sie sich schnell mit der REST-API-Syntax vertraut machen, die Sie zum Abfragen von Warnungen verwenden können. Sie können diese API bei der Automatisierung oder Integration in Ihre vorhandenen Rechenzentrumslösungen für die Überwachung, Berichterstattung und Ticketausstellung verwenden. 

![Option „API anzeigen“ mit Anzeige der REST-API](media/azure-stack-monitor-health/image6.png)

Sie können auf eine bestimmte Warnung klicken, um die Warnungsdetails anzuzeigen. In den Warnungsdetails werden alle Felder angezeigt, die der Warnung zugeordnet sind. Zudem ermöglichen sie eine schnelle Navigation zur betroffenen Komponente und Quelle der Warnung. Beispielsweise tritt die folgende Warnung auf, wenn eine der Infrastrukturrolleninstanzen offline geschaltet wird oder nicht aufrufbar ist.  

![Blatt „Warnungsdetails“](media/azure-stack-monitor-health/image7.png)

Nachdem die Infrastrukturrolleninstanz wieder online geschaltet wurde, wird diese Warnung automatisch geschlossen. Nicht alle, aber viele Warnungen werden automatisch geschlossen, wenn das zugrunde liegende Problem behoben wird. Es wird empfohlen, nach dem Ausführen der Schritte zur Behebung auf **Close Alert** (Warnung schließen) zu klicken. Wenn das Problem weiterhin auftritt, erstellt Azure Stack eine neue Warnung. Wenn Sie das Problem behoben haben, bleibt die Warnung geschlossen und erfordert keine weitere Aktion.

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von Updates in Azure Stack](azure-stack-updates.md)

[Regionsverwaltung in Azure Stack](azure-stack-region-management.md)
