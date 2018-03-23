---
title: Regionsverwaltung in Azure Stack | Microsoft-Dokumentation
description: Übersicht über die Regionsverwaltung in Azure Stack
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: brenduns
ms.reviewer: efemmano
ms.openlocfilehash: 0a19490ce276eec9a46bc0ea4343e3449a9dc93c
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="region-management-in-azure-stack"></a>Regionsverwaltung in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack verwendet Regionen. Diese Regionen sind logische Entitäten, die aus Hardwareressourcen bestehen, aus denen wiederum die Azure Stack-Infrastruktur besteht. In der Regionsverwaltung finden Sie alle Ressourcen, die zum erfolgreichen Nutzen des Lebenszyklus der Azure Stack-Infrastruktur erforderlich sind.

Eine integrierte (als *Azure Stack-Cloud* bezeichnete) Systembereitstellung bildet eine einzelne Region. Jedes Azure Stack Development Kit hat eine einzelne als **lokal** benannte Region. Wenn Sie ein zweites integriertes Azure Stack-System bereitstellen oder eine andere Instanz des Development Kits auf separater Hardware einrichten, ist diese Azure Stack-Cloud eine andere Region.

## <a name="information-available-through-the-region-management-tile"></a>Informationen, die über die Kachel „Regionsverwaltung“ verfügbar sind
Azure Stack verfügt über einige Funktionen der Regionsverwaltung, die in der Kachel **Regionsverwaltung** verfügbar sind. Diese Kachel steht Azure Stack-Operatoren im Administratorportal im Standarddashboard zur Verfügung. Über diese Kachel können Sie Ihre Azure Stack-Region und deren regionsspezifische Komponenten überwachen und aktualisieren.

 ![Die Regionsverwaltungskachel](media/azure-stack-manage-region/image1.png)

 Wenn Sie auf der Kachel „Regionsverwaltung“ auf eine Region klicken, können Sie folgende Informationen abrufen:

  ![Beschreibung der Bereiche auf dem Blatt „Regionsverwaltung“](media/azure-stack-manage-region/image2.png)

1. **Das Ressourcenmenü**. Hier können Sie auf spezifische Infrastrukturverwaltungsbereiche zugreifen und Benutzerressourcen anzeigen und verwalten. Zu solchen Ressourcen zählen z.B. Speicherkonten und virtuelle Netzwerke.

2. **Warnungen**. Auf dieser Kachel werden systemweite Warnungen aufgelistet sowie genauere Angaben zu diesen Warnungen.

3. **Updates**. Auf dieser Kachel werden die aktuelle Version Ihrer Azure Stack-Infrastruktur, verfügbare Updates und der Updateverlauf angezeigt. Sie können auch Ihr integriertes System aktualisieren.

4. **Ressourcenanbieter**. Unter „Ressourcenanbieter“ verwalten Sie die Benutzerfunktionen, die von den Komponenten geboten werden, die zur Ausführung von Azure Stack erforderlich sind. Jeder Ressourcenanbieter hat eine eigene Administratorerfahrung. Diese Erfahrung beinhaltet Warnungen für spezifische Anbieter, Metriken und andere Verwaltungsfunktionen, die ressourcenbetreiberspezifisch sind.

5. **Infrastrukturrollen**. Infrastrukturrollen sind für das Ausführen von Azure Stack erforderliche Komponenten. Es werden nur die Infrastrukturrollen aufgelistet, die Warnungen ausgeben. Wenn Sie auf eine Rolle klicken, können Sie die Warnungen dieser Rolle anzeigen sowie die Rolleninstanzen, auf denen die Rolle ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte
[Überwachen von Integrität und Warnungen in Azure Stack](azure-stack-monitor-health.md)

[Verwalten von Updates in Azure Stack](azure-stack-updates.md)
