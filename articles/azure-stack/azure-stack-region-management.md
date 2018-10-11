---
title: Regionsverwaltung in Azure Stack | Microsoft-Dokumentation
description: Übersicht über die Regionsverwaltung in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 401b81ceb7ab71528a4ad11bc7d8944b4d732933
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078863"
---
# <a name="region-management-in-azure-stack"></a>Regionsverwaltung in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack verwendet Regionen. Diese Regionen sind logische Entitäten, die Hardwareressourcen umfassen, aus denen wiederum die Azure Stack-Infrastruktur besteht. In der Regionsverwaltung finden Sie alle Ressourcen, die zum erfolgreichen Nutzen der Azure Stack-Infrastruktur erforderlich sind.

Eine integrierte (als *Azure Stack-Cloud* bezeichnete) Systembereitstellung bildet eine einzelne Region. Jedes Azure Stack Development Kit hat eine einzelne als **lokal** benannte Region. Wenn Sie ein zweites integriertes Azure Stack-System bereitstellen oder eine andere Instanz des Development Kits auf separater Hardware einrichten, ist diese Azure Stack-Cloud eine andere Region.

## <a name="information-available-through-the-region-management-tile"></a>Informationen, die über die Kachel „Regionsverwaltung“ verfügbar sind

Azure Stack verfügt über einige Funktionen der Regionsverwaltung, die in der Kachel **Regionsverwaltung** verfügbar sind. Diese Kachel steht Azure Stack-Operatoren im Administratorportal im Standarddashboard zur Verfügung. Über diese Kachel können Sie Ihre Azure Stack-Region und deren regionsspezifische Komponenten überwachen und aktualisieren.

 ![Die Regionsverwaltungskachel](media/azure-stack-manage-region/image1.png)

 Wenn Sie auf der Kachel „Regionsverwaltung“ auf eine Region klicken, können Sie folgende Informationen abrufen:

  ![Beschreibung der Bereiche auf dem Blatt „Regionsverwaltung“](media/azure-stack-manage-region/image2.png)

1. **Das Ressourcenmenü**. Hier können Sie auf spezifische Infrastrukturverwaltungsbereiche zugreifen und Benutzerressourcen anzeigen und verwalten. Zu solchen Ressourcen zählen z.B. Speicherkonten und virtuelle Netzwerke.

2. **Warnungen**. Hier werden systemweite Warnungen sowie genauere Angaben zu diesen Warnungen aufgelistet.

3. **Updates**. Hier werden die aktuelle Version Ihrer Azure Stack-Infrastruktur, verfügbare Updates und der Updateverlauf angezeigt. Sie können auch Ihr integriertes System aktualisieren.

4. **Ressourcenanbieter**. Unter „Ressourcenanbieter“ verwalten Sie die Benutzerfunktionen, die von den Komponenten geboten werden, die zur Ausführung von Azure Stack erforderlich sind. Jeder Ressourcenanbieter hat eine eigene Administratorerfahrung. Diese Erfahrung beinhaltet Warnungen für spezifische Anbieter, Metriken und andere Verwaltungsfunktionen, die ressourcenbetreiberspezifisch sind.

5. **Infrastrukturrollen**. Infrastrukturrollen sind für das Ausführen von Azure Stack erforderliche Komponenten. Es werden nur die Infrastrukturrollen aufgelistet, die Warnungen ausgeben. Wenn Sie eine Rolle auswählen, können Sie die Warnungen dieser Rolle anzeigen sowie die Rolleninstanzen, auf denen die Rolle ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen von Integrität und Warnungen in Azure Stack](azure-stack-monitor-health.md)
- [Verwalten von Updates in Azure Stack](azure-stack-updates.md)
