---
title: Durchführen einer Zugriffsüberprüfung in Privileged Identity Management für Azure-Ressourcen | Microsoft-Dokumentation
description: In diesem Dokument wird beschrieben, wie Sie in PIM eine Überprüfung für Azure-Ressourcen durchführen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: f1bcf114b997c3056016b84cafc28253ea1af28e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="resource-dashboards"></a>Ressourcendashboards

Das Dashboard mit der Administratoransicht verfügt über vier Hauptkomponenten. Eine grafische Darstellung der Ressourcenrollenaktivierungen der letzten sieben Tage. Diese Daten beziehen sich auf die ausgewählte Ressource und geben Aufschluss über Aktivierungen für die gängigsten Rollen (Besitzer, Mitwirkender, Benutzerzugriffsadministrator) sowie für alle Rollen zusammen.

Rechts neben dem Aktivierungsdiagramm befinden sich zwei Diagramme mit der Verteilung von Rollenzuweisungen nach Zuweisungstyp (für Benutzer und Gruppen). Wenn Sie auf ein Segment des Diagramms klicken, ändert sich der Wert in einen Prozentsatz (oder umgekehrt).

![](media/azure-pim-resource-rbac/rbac-overview-top.png)

Unterhalb der Diagramme werden die Anzahl von Benutzern und Gruppen mit neuen Rollenzuweisungen in den letzten 30 Tagen (links) sowie eine absteigend nach Gesamtzuweisungen sortierte Liste mit Rollen angezeigt.

![](media/azure-pim-resource-rbac/role-settings.png)
