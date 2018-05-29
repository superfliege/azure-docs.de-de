---
title: Berechtigte Zuweisungen und Ressourcensichtbarkeit für Azure in Privileged Identity Management | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie bei Verwendung von PIM Mitglieder für Ressourcenrollen als „Berechtigt“ zuweisen.
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
ms.openlocfilehash: 4804d930a98192d64245784058920eeba7d30212
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32149985"
---
# <a name="eligible-assignments-and-resource-visibility-with-privileged-identity-management"></a>Berechtigte Zuweisungen und Ressourcensichtbarkeit mit Privileged Identity Management

Privileged Identity Management (PIM) für Azure-Ressourcenrollen ermöglicht eine verbesserte Sicherheit für Organisationen, die über wichtige Azure-Ressourcen verfügen. Mit PIM können Ressourcenadministratoren Mitglieder als „Berechtigt“ für Ressourcenrollen zuweisen. In den folgenden Abschnitten erhalten Sie weitere Informationen zu den verschiedenen Zuweisungstypen und Zuweisungsstatus für Azure-Ressourcenrollen. 

## <a name="assignment-types"></a>Zuweisungstypen

PIM für Azure-Ressourcen verfügt über zwei unterschiedliche Zuweisungstypen:

- Berechtigt
- Aktiv

Für berechtigte Zuweisungen muss das Mitglied der Rolle eine Aktion durchführen, um die Rolle verwenden zu können. Beispiele für Aktionen sind eine erfolgreiche Multi-Factor Authentication-Überprüfung, die Angabe einer geschäftlichen Begründung oder das Anfordern einer Genehmigung von den angegebenen genehmigenden Personen.

Für aktive Zuweisungen ist es nicht erforderlich, dass das Mitglied eine Aktion durchführt, um die Rolle nutzen zu können. Für Mitglieder, die als „Aktiv“ zugewiesen sind, sind die Berechtigungen immer der Rolle zugewiesen.

## <a name="assignment-duration"></a>Zuweisungsdauer

Ressourcenadministratoren können für jeden Zuweisungstyp eine von zwei Optionen auswählen, wenn sie PIM-Einstellungen für eine Rolle konfigurieren. Diese Optionen werden zur maximalen Standarddauer, wenn ein Mitglied der Rolle in PIM zugewiesen wird. 

Ein Administrator kann einen dieser Zuweisungstypen auswählen:

- Allow permanent eligible assignment (Dauerhafte berechtigte Zuweisung zulassen)
- Allow permanent active assignment (Dauerhafte aktive Zuweisung zulassen)

Oder ein Administrator kann einen dieser Zuweisungstypen auswählen:

- Expire eligible assignments after (Berechtigte Zuweisungen beenden nach)
- Expire active assignments after (Aktive Zuweisungen beenden nach)

Wenn ein Ressourcenadministrator die Option **Allow permanent eligible assignment** (Dauerhafte berechtigte Zuweisung zulassen) oder **Allow permanent active assignment** (Dauerhafte aktive Zuweisung zulassen) auswählt, können alle Administratoren, die der Ressource Mitglieder zuweisen, dauerhafte Mitgliedschaften zuweisen.

Wenn ein Ressourcenadministrator **Expire eligible assignments after** (Berechtigte Zuweisungen beenden nach) oder **Expire active assignments after** (Aktive Zuweisungen beenden nach) auswählt, steuert er den Zuweisungslebenszyklus, indem erzwungen wird, dass alle Zuweisungen über ein angegebenes Start- und Enddatum verfügen.

> [!NOTE] 
> Alle Zuweisungen mit einem angegebenen Enddatum können von Ressourcenadministratoren erneuert werden. Zudem können Mitglieder Self-Service-Anforderungen initiieren, um [Zuweisungen zu verlängern oder zu erneuern](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Zuweisungsstatus

PIM für Azure-Ressourcen verfügt über zwei unterschiedliche Zuweisungsstatus, die in PIM in den Ansichten **My roles** (Meine Rollen), **Rollen** und **Mitglieder** auf der Registerkarte **Aktive Rollen** angezeigt werden. Folgende Status sind möglich:

- Zugewiesen
- Aktiviert

Beim Anzeigen einer Mitgliedschaft, die unter **Aktive Rollen** aufgeführt ist, können Sie anhand des Werts in der Spalte **STATUS** zwischen Benutzern unterscheiden, die als aktiv **zugewiesen** sind, und Benutzern, für die eine berechtigte Zuweisung **aktiviert** wurde und die derzeit aktiv sind.

## <a name="next-steps"></a>Nächste Schritte

[Zuweisen von Rollen in Privileged Identity Management](pim-resource-roles-assign-roles.md)
