---
title: Privileged Identity Management für Azure-Ressourcen – Berechtigte Zuweisungen und Ressourcensichtbarkeit | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Mitglieder für Ressourcenrollen als „Berechtigt“ zuweisen.
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
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 273b06c91d68a764fe814374c0eca6ed1698cc2e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="eligible-assignments-and-resource-visibility"></a>Berechtigte Zuweisungen und Ressourcensichtbarkeit

PIM für Azure-Ressourcenrollen ermöglicht für Organisationen mit wichtigen Azure-Ressourcen eine verbesserte Sicherheit. Mit PIM haben Ressourcenadministratoren die Möglichkeit zum Zuweisen von Mitgliedern als „Berechtigt“ für Ressourcenrollen. Unten erhalten Sie weitere Informationen zu den verschiedenen Zuweisungstypen und Status für Azure-Ressourcenrollen. 

## <a name="assignment-types"></a>Zuweisungstypen

PIM für Azure-Ressourcen verfügt über zwei unterschiedliche Zuweisungstypen:

- Berechtigt
- Aktiv

Für berechtigte Zuweisungen muss das Mitglied der Rolle eine Aktion durchführen, um die Rolle verwenden zu können. Beispiele für diese Aktionen sind eine erfolgreiche Multi-Factor Authentication-Überprüfung, die Angabe einer Begründung und das Anfordern einer Genehmigung von den angegebenen genehmigenden Personen.

Für aktive Zuweisungen ist es nicht erforderlich, dass das Mitglied eine Aktion durchführt, um die Rolle nutzen zu können. Für Mitglieder, die als „Aktiv“ zugewiesen sind, sind die Berechtigungen jederzeit über die Rolle verfügbar.

## <a name="assignment-duration"></a>Zuweisungsdauer

Ressourcenadministratoren können für jeden Zuweisungstyp eine von zwei Optionen auswählen, wenn die PIM-Einstellungen für eine Rolle konfiguriert werden. Diese Optionen werden zur maximalen Standarddauer, wenn ein Mitglied der Rolle in PIM zugewiesen wird.

- Allow permanent eligible assignment (Dauerhafte berechtigte Zuweisung zulassen)
- Allow permanent active assignment (Dauerhafte aktive Zuweisung zulassen)

oder

- Expire eligible assignments after (Berechtigte Zuweisungen beenden nach)
- Expire active assignments after (Aktive Zuweisungen beenden nach)

Wenn ein Ressourcenadministrator die Option „Allow permanent eligible assignment“ (Dauerhafte berechtigte Zuweisung zulassen) bzw. „Allow permanent active assignment“ (Dauerhafte aktive Zuweisung zulassen) wählt, können alle Administratoren, die der Ressource Mitglieder zuweisen, dauerhafte Mitgliedschaften zuweisen.

Die Auswahl von „Expire eligible assignments after“ (Berechtigte Zuweisungen beenden nach) bzw. „Expire active assignments after“ (Aktive Zuweisungen beenden nach) ermöglicht die Kontrolle über den Zuweisungslebenszyklus, indem erzwungen wird, dass alle Zuweisungen über ein angegebenes Start- und Enddatum verfügen.

>[!NOTE] 
>Alle Zuweisungen mit einem angegebenen Enddatum können von Ressourcenadministratoren erneuert werden, und Mitglieder können Self-Service-Anforderungen initiieren, um [Zuweisungen zu verlängern oder zu erneuern](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Zuweisungsstatus

PIM für Azure-Ressourcen verfügt über zwei unterschiedliche Zuweisungsstatus, die in PIM in den Ansichten „Meine Rollen“, „Rollen“ und „Mitglieder“ auf der Registerkarte „Meine Rollen“ angezeigt werden. Folgende Status sind möglich:

- Zugewiesen
- Aktiviert

Beim Anzeigen einer Mitgliedschaft, die unter „Aktive Rollen“ aufgeführt ist, können Sie in der Spalte mit dem STATUS zwischen Benutzern unterscheiden, die als aktiv zugewiesen sind, und Benutzern, für die eine berechtigte Zuweisung aktiviert wurde und die derzeit aktiv sind.

## <a name="next-steps"></a>Nächste Schritte

[Zuweisen von Rollen in PIM](pim-resource-roles-assign-roles.md)

