---
title: "Intelligente Erkennung – Von Azure Application Insights erkannte niedrige Auslastung von Cloudressourcen | Microsoft Docs"
description: "Überwachen Sie Anwendungen mit Azure Application Insights auf eine niedrige Auslastung von Cloudressourcen."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 8382f6047ae222a01cc0e8d6ca9dcf5593d0dff6
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="low-utilization-of-cloud-resources-preview"></a>Niedrige Auslastung von Cloudressourcen (Vorschau)

Application Insights analysiert die CPU-Auslastung der einzelnen Rolleninstanzen in Ihrer Anwendung automatisch und erkennt Instanzen mit niedriger CPU-Auslastung. Diese Erkennung ermöglicht es Ihnen, Ihre Azure-Ressourcen zu verringern und Kosten zu senken, indem Sie die Anzahl der von jeder Rolle verwendeten Rolleninstanzen oder die Anzahl der Rollen verringern.

Diese Funktion erfordert keine spezielle Einrichtung, abgesehen von der [Konfiguration von Leistungsindikatoren](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) für Ihre App. Sie ist aktiv, wenn Ihre App genügend Telemetriedaten zu CPU-Leistungsindikatoren („Prozessorzeit (%)“) generiert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wann erhalte ich diese Art von Benachrichtigung der intelligenten Erkennung?
Eine typische Benachrichtigung tritt auf, wenn viele Ihrer Web-/Workerrolleninstanzen eine niedrige CPU-Auslastung aufweisen.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Beansprucht meine App definitiv zu viele Ressourcen?
Nein, eine Benachrichtigung bedeutet nicht zwingend, dass Ihre App definitiv zu viele Ressourcen verbraucht. Obwohl solche Muster einer niedrigen CPU-Auslastung in der Regel darauf hinweisen, dass der Ressourcenverbrauch verringert werden kann, könnte dieses Verhalten typisch für Ihre spezifische Rolle sein oder einen natürlichen geschäftliche Grund haben und ignoriert werden. Es kann beispielsweise sein, dass mehrere Instanzen für andere Ressourcen wie Arbeitsspeicher/Netzwerk und nicht für die CPU benötigt werden.

## <a name="how-do-i-fix-it"></a>Wie behebe ich das Problem?
Die Benachrichtigungen umfassen Diagnoseinformationen zur Unterstützung des Diagnoseprozesses:
1. **Selektierung.** Die Benachrichtigung gibt die Rollen in Ihrer App an, die eine niedrige CPU-Auslastung aufweisen. Dadurch können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Wie viele Rollen wiesen eine niedrige CPU-Auslastung auf, und wie viele Instanzen jeder Rolle beanspruchen die CPU in geringem Maße? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält den Prozentsatz der CPU-Auslastung, wobei die CPU-Auslastung für jede Instanz im Laufe der Zeit angezeigt wird. Zur weiteren Diagnose des Problems können Sie auch die entsprechenden Elemente und Berichte verwenden, die mit weiterführenden Informationen (z. B. Perzentilen der CPU-Auslastung) verknüpft sind.
