---
title: Berechtigte Zuweisungen und Ressourcensichtbarkeit in PIM – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie Mitgliedern in Azure AD Privileged Identity Management (PIM) die Berechtigung für Azure-Ressourcenrollen zuweisen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: fb52bc92c86261831d0e8d8e9e863a4863fe8fb9
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666888"
---
# <a name="eligible-assignments-and-resource-visibility-in-pim"></a>Zugewiesene Berechtigungen und Ressourcensichtbarkeit in PIM

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

## <a name="azure-resource-role-approval-workflow"></a>Genehmigungsworkflow für Azure-Ressourcenrolle

Über den Genehmigungsworkflow in PIM für Azure-Ressourcenrollen können Administratoren den Zugriff auf wichtige Ressourcen weiter schützen oder einschränken. Das heißt, Administratoren können für die Aktivierung von Rollenzuweisungen die Notwendigkeit einer Genehmigung festlegen.

Das Konzept der Ressourcenhierarchie gilt nur für Azure-Ressourcenrollen. Diese Hierarchie ermöglicht die Vererbung von Rollenzuweisungen von einem übergeordneten Ressourcenobjekt bis zu allen untergeordneten Ressourcen des übergeordneten Containers. 

Beispielsweise nutzt Bob als Ressourcenadministrator PIM, um Alice im Contoso-Abonnement der Rolle „Besitzer“ als berechtigtes Mitglied zuzuweisen. Durch diese Zuweisung ist Alice eine berechtigte Besitzerin aller Ressourcengruppencontainer im Contoso-Abonnement. Alice ist zudem eine berechtigte Besitzerin aller Ressourcen (z.B. virtuelle Computer) in den einzelnen Ressourcengruppen des Abonnements.

Angenommen, das Contoso-Abonnement enthält drei Ressourcengruppen: Fabrikam Test, Fabrikam Dev und Fabrikam Prod. Jede dieser Ressourcengruppen enthält einen einzelnen virtuellen Computer.

PIM-Einstellungen werden für jede Rolle einer Ressource konfiguriert. Im Gegensatz zu Zuweisungen werden diese Einstellungen nicht vererbt und gelten nur für die Ressourcenrolle.

Fortführung des Beispiels: Bob nutzt PIM, um für alle Mitglieder der Rolle „Besitzer“ im Contoso-Abonnement die Durchführung des Genehmigungsvorgangs zu erzwingen, bevor die Aktivierung erfolgen kann. Außerdem erzwingt Bob die Genehmigung für Mitglieder der Rolle „Besitzer“ für diese Ressource, um die Ressourcen in der Ressourcengruppe „Fabrikam Prod“ zu schützen. Für die Rollen „Besitzer“ in „Fabrikam Test“ und „Fabrikam Dev“ ist keine Genehmigung für die Aktivierung erforderlich.

Wenn Alice die Aktivierung ihrer Rolle „Besitzer“ für das Contoso-Abonnement anfordert, muss die Anforderung von einer Person genehmigt (bzw. abgelehnt) werden, bevor die Rolle für sie aktiviert wird. Wenn Alice den [Umfang der Aktivierung](pim-resource-roles-activate-your-roles.md) auf die Ressourcengruppe „Fabrikam Prod“ ausweiten möchte, muss diese Anforderung ebenfalls von einer Person genehmigt oder abgelehnt werden. Wenn Alice den Umfang der Aktivierung aber auf „Fabrikam Test“ und/oder „Fabrikam Dev“ ausweiten möchte, ist keine Genehmigung erforderlich.

Der Genehmigungsworkflow ist möglicherweise nicht für alle Mitglieder einer Rolle erforderlich. Stellen Sie sich ein Szenario vor, bei dem eine Organisation auf mehrere Vertragspartner zurückgreift, die Unterstützung bei der Entwicklung einer Anwendung leisten, die in einem Azure-Abonnement ausgeführt wird. Als Ressourcenadministrator möchten Sie, dass Mitarbeiter über eine Zugriffsberechtigung verfügen, ohne dass eine Genehmigung erforderlich ist, während für die Vertragspartner eine Genehmigung angefordert werden muss. Zum Konfigurieren des Genehmigungsworkflows nur für die Vertragspartner können Sie eine benutzerdefinierte Rolle mit den gleichen Berechtigungen wie für die Rolle erstellen, die den Mitarbeitern zugewiesen ist. Sie können eine Genehmigung der Aktivierung dieser benutzerdefinierten Rolle erzwingen. [Hier finden Sie weitere Informationen zu benutzerdefinierten Rollen](pim-resource-roles-custom-role-policy.md).

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
