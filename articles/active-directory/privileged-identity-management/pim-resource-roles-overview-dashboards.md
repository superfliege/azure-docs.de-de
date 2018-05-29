---
title: 'Übersicht: Durchführen einer Zugriffsüberprüfung in Privileged Identity Management für Azure-Ressourcen | Microsoft-Dokumentation'
description: In diesem Dokument wird beschrieben, wie Sie in PIM eine Zugriffsüberprüfung für Azure-Ressourcen durchführen.
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
ms.openlocfilehash: 61be9873cac462c096599680a6e071e104f3a54c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33200619"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Verwenden eines Ressourcendashboards für die Zugriffsüberprüfung

Sie können ein Ressourcendashboard verwenden, um eine Zugriffsüberprüfung in Privileged Identity Management (PIM) für Azure-Ressourcen durchzuführen. Das Dashboard mit der Administratoransicht besteht aus drei Hauptkomponenten:

- eine grafische Darstellung der Ressourcenrollenaktivierungen
- zwei Diagramme, die die Verteilung von Rollenzuweisungen nach Zuweisungstyp darstellen
- ein Datenbereich, der sich auf neue Rollenzuweisungen bezieht

![Screenshot der Dashboardadministratoransicht mit Diagrammen](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Screenshot der Dashboardadministratoransicht mit Datenlisten](media/azure-pim-resource-rbac/role-settings.png)

Die grafische Darstellung der Ressourcenrollenaktivierungen deckt die letzten sieben Tage ab. Diese Daten beziehen sich auf die ausgewählte Ressource und geben Aufschluss über Aktivierungen für die gängigsten Rollen (Besitzer, Mitwirkender, Benutzerzugriffsadministrator) sowie für alle Rollen zusammen.

Rechts neben dem Aktivierungsdiagramm befinden sich zwei Diagramme mit der Verteilung von Rollenzuweisungen nach Zuweisungstyp (für Benutzer und Gruppen). Wenn Sie auf ein Segment des Diagramms klicken, ändert sich der Wert in einen Prozentsatz (oder umgekehrt).

Unterhalb der Diagramme werden die Anzahl von Benutzern und Gruppen mit neuen Rollenzuweisungen in den letzten 30 Tagen sowie eine (in absteigender Reihenfolge) nach Gesamtzuweisungen sortierte Liste mit Rollen angezeigt.


