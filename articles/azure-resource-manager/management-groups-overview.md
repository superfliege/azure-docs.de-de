---
title: Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen | Microsoft Docs
description: Informationen zu Verwaltungsgruppen und deren Verwendung.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/09/2018
ms.author: rithorn
ms.openlocfilehash: c8152a6c12c776806d9a17c5e434d825d6c91165
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38466642"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen

Wenn Ihre Organisation über viele Abonnements verfügt, benötigen Sie möglicherweise eine Möglichkeit zur effizienten Verwaltung von Zugriff, Richtlinien und Konformität für diese Abonnements. Azure-Verwaltungsgruppen stellen einen abonnementübergreifenden Bereich dar. Sie organisieren Abonnements in Containern, die als „Verwaltungsgruppen“ bezeichnet werden, und wenden Ihre Governancebedingungen auf die Verwaltungsgruppen an. Alle Abonnements in einer Verwaltungsgruppe erben automatisch die auf die Verwaltungsgruppe angewendeten Bedingungen. Verwaltungsgruppen ermöglichen Ihnen – unabhängig von den Arten Ihrer Abonnements – die unternehmenstaugliche Verwaltung in großem Umfang.

Das Feature für Verwaltungsgruppen ist in einer öffentlichen Vorschauversion verfügbar. Um mit der Verwendung von Verwaltungsgruppen zu beginnen, melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und suchen Sie im Abschnitt **Alle Dienste** nach **Verwaltungsgruppen**.

Beispielsweise können Sie eine Richtlinie auf eine Verwaltungsgruppe anwenden, die die verfügbaren Regionen zum Erstellen virtueller Computer einschränkt. Diese Richtlinie wird auf alle Verwaltungsgruppen, Abonnements und Ressourcen in dieser Verwaltungsgruppe angewendet, sodass virtuelle Computer nur in dieser Region erstellt werden können.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarchie von Verwaltungsgruppen und Abonnements

Sie können eine flexible Struktur von Verwaltungsgruppen und Abonnements aufbauen, um Ihre Ressourcen für die einheitliche Richtlinien- und Zugriffsverwaltung in einer Hierarchie zu organisieren.
Das folgende Diagramm zeigt eine Beispielhierarchie, die aus Verwaltungsgruppen und Abonnements besteht, die nach Abteilung organisiert sind.

![Struktur](media/management-groups/MG_overview.png)

Indem Sie eine Hierarchie erstellen, die nach Abteilungen gruppiert ist, können Sie Rollen der [rollenbasierten Zugriffssteuerung (RBAC) in Azure](../role-based-access-control/overview.md) zuweisen, die an die Abteilungen unter dieser Verwaltungsgruppe *vererbt* werden. Mithilfe von Verwaltungsgruppen können Sie die Workload und das Risiko von Fehlern verringern, indem Sie die Rolle nur einmal zuweisen müssen.

### <a name="important-facts-about-management-groups"></a>Wichtige Fakten zu Verwaltungsgruppen

- 10.000 Verwaltungsgruppen können in einem einzigen Verzeichnis unterstützt werden.
- Eine Verwaltungsgruppenstruktur kann bis zu sechs Ebenen unterstützen.
  - Dieser Grenzwert schließt nicht die Stammebene oder die Abonnementebene ein.
- Jede Verwaltungsgruppe und jedes Abonnement kann nur ein übergeordnetes Element unterstützen.
- Jede Verwaltungsgruppe kann mehrere untergeordnete Elemente besitzen.
- Alle Abonnements und Verwaltungsgruppen sind in einer einzelnen Hierarchie in jedem Verzeichnis enthalten. Weitere Informationen zu den Ausnahmen während der Vorschauphase finden Sie unter [Wichtige Fakten zur Stammverwaltungsgruppe](#important-facts-about-the-root-management-group).

### <a name="preview-subscription-visibility-limitation"></a>Eingeschränkte Sichtbarkeit von Abonnements in der Vorschauversion

Zurzeit weist die Vorschauversion die Einschränkung auf, dass keine Abonnements angezeigt werden können, deren Zugriff Sie geerbt haben. Der Zugriff wird an das Abonnement vererbt, der Azure Resource Manager kann jedoch vererbte Zugriffe noch nicht berücksichtigen.  

Wenn Sie Informationen zum Abonnement mithilfe der REST-API abrufen, werden Details zurückgegeben, da Sie zwar Zugriff auf die Abonnements haben, diese jedoch weder im Azure-Portal noch in Azure PowerShell angezeigt werden.

An der Lösung dieses Problem wird zurzeit gearbeitet. Noch vor der Ankündigung der allgemeinen Verfügbarkeit von Verwaltungsgruppen wird eine Lösung bereitgestellt.  

### <a name="cloud-solution-provider-csp-limitation-during-preview"></a>CSP-Einschränkung (Cloud Solution Provider) in der Vorschauversion

Es gibt eine aktuelle Einschränkung für CSP-Partner (Cloud Solution Provider), die dazu führt, dass diese ihre Kundenverwaltungsgruppen nicht im Kundenverzeichnis erstellen oder verwalten können.  
An der Lösung dieses Problem wird zurzeit gearbeitet. Noch vor der Ankündigung der allgemeinen Verfügbarkeit von Verwaltungsgruppen wird eine Lösung bereitgestellt.

## <a name="root-management-group-for-each-directory"></a>Stammverwaltungsgruppe für jedes Verzeichnis

Jedes Verzeichnis erhält eine einzelne Verwaltungsgruppe auf oberster Ebene, die als Stammverwaltungsgruppe bezeichnet wird. Die Stammverwaltungsgruppe ist in die Hierarchie integriert, sodass ihr alle Verwaltungsgruppen und Abonnements untergeordnet sind. Diese Stammverwaltungsgruppe ermöglicht das Anwenden von globalen Richtlinien und RBAC-Zuweisungen auf Verzeichnisebene. Der [Verzeichnisadministrator muss seine eigenen Rechte erhöhen](../role-based-access-control/elevate-access-global-admin.md), um der erste Besitzer dieser Gruppe zu sein. Sobald der Administrator der Besitzer der Gruppe ist, kann er jede RBAC-Rolle anderen Benutzern oder Gruppen des Verzeichnisses zuweisen, um die Hierarchie zu verwalten.  

### <a name="important-facts-about-the-root-management-group"></a>Wichtige Fakten zur Stammverwaltungsgruppe

- Der Name und die ID der Stammverwaltungsgruppe werden standardmäßig vergeben. Der Anzeigename kann jederzeit aktualisiert werden, damit er im Azure-Portal entsprechend angezeigt wird.
  - Der Name lautet „Mandantenstammgruppe“.
  - Die ID ist die Azure Active Directory-ID.
- Die Stammverwaltungsgruppe kann im Gegensatz zu anderen Verwaltungsgruppen nicht verschoben oder gelöscht werden.  
- Alle Abonnements und Verwaltungsgruppen sind der Stammverwaltungsgruppe im Verzeichnis untergeordnet.
  - Alle Ressourcen im Verzeichnis sind der Stammverwaltungsgruppe für die globale Verwaltung untergeordnet.
  - Neue Abonnements werden bei ihrer Erstellung standardmäßig der Stammverwaltungsgruppe zugeordnet.
- Alle Azure-Kunden können die Stammverwaltungsgruppe sehen, aber nicht alle Kunden besitzen Zugriff, um diese Stammverwaltungsgruppe zu verwalten.
  - Jeder Benutzer, der Zugriff auf ein Abonnement besitzt, kann den Kontext erkennen, in dem sich das Abonnement in der Hierarchie befindet.  
  - Niemand erhält Standardzugriff auf die Stammverwaltungsgruppe. Globale Verzeichnisadministratoren sind die einzigen Benutzer, die ihre Rechte erhöhen können, um Zugriff zu erlangen.  Sobald sie über Zugriff verfügen, können die Verzeichnisadministratoren eine beliebige RBAC-Rolle anderen Benutzern zum Verwalten zuweisen.  

>[!NOTE]
>Wenn Ihr Verzeichnis mit der Verwendung des Verwaltungsgruppendiensts vor dem 25.06.2018 begonnen hat, ist Ihr Verzeichnis ggf. nicht mit allen Abonnements in der Hierarchie eingerichtet. Das Verwaltungsgruppenteam aktualisiert im Juli 2018 rückwirkend jedes Verzeichnis, das mit der Verwendung von Verwaltungsgruppen in der öffentlichen Vorschauversion vor diesem Datum begonnen hat. Alle Abonnements in den Verzeichnissen werden zuvor zu untergeordneten Elementen in der Stammverwaltungsgruppe erklärt.  
>
>Wenn Sie Fragen zu diesem rückwirkenden Vorgang haben, wenden Sie sich an: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Erstmalige Einrichtung von Verwaltungsgruppen

Wenn ein Benutzer mit der Verwendung von Verwaltungsgruppen beginnt, findet ein erster Einrichtungsvorgang statt. Der erste Schritt besteht darin, dass die Stammverwaltungsgruppe im Verzeichnis erstellt wird. Sobald diese Gruppe erstellt wurde, werden alle Abonnements, die im Verzeichnis vorhanden sind, zu untergeordneten Elementen der Stammverwaltungsgruppe erklärt.  Der Grund für diesen Vorgang besteht im Sicherstellen, dass nur eine Verwaltungsgruppenhierarchie in einem Verzeichnis vorhanden ist.  Die einzige Hierarchie innerhalb des Verzeichnisses kann administrative Kunden das Anwenden von globalem Zugriff und globalen Richtlinien ermöglichen, die andere Kunden im Verzeichnis nicht umgehen können. Alles, was dem Stamm zugewiesen wird, gilt für alle Verwaltungsgruppen, Abonnements, Ressourcengruppen und Ressourcen innerhalb des Verzeichnisses, weil eine Hierarchie im Verzeichnis vorhanden ist.  

> [!IMPORTANT]
> Jede Zuweisung von Benutzerzugriff oder Richtlinienzuweisung für die Stammverwaltungsgruppe **gilt für alle Ressourcen im Verzeichnis**. Aus diesem Grund sollten alle Kunden die Notwendigkeit untersuchen, Elemente in diesem Bereich zu definieren.  Benutzerzugriff und Richtlinienzuweisungen sollten nur in diesem Bereich ein „Muss“ sein.  
  
## <a name="management-group-access"></a>Zugriff auf die Verwaltungsgruppe

Azure-Verwaltungsgruppen unterstützen die [rollenbasierte Zugriffssteuerung (RBAC) in Azure](../role-based-access-control/overview.md) für alle Ressourcenzugriffe und Rollendefinitionen. Diese Berechtigungen werden an untergeordnete Ressourcen in der Hierarchie vererbt. Eine beliebige integrierte RBAC-Rolle kann einer Verwaltungsgruppe zugewiesen werden, die in der Hierarchie an die Ressourcen vererbt wird.  Beispielsweise kann die RBAC-Rolle „VM-Mitwirkender“ einer Verwaltungsgruppe zugewiesen werden. Diese Rolle verfügt über keine Aktion für die Verwaltungsgruppe, wird jedoch an alle virtuellen Computer unter dieser Verwaltungsgruppe vererbt.  

Die folgende Abbildung zeigt die Liste der Rollen und die unterstützten Aktionen für Verwaltungsgruppen.

| Name der RBAC-Rolle             | Erstellen | Umbenennen | Move | Löschen | Zuweisen des Zugriffs | Zuweisen der Richtlinie | Lesen  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Owner (Besitzer)                       | X      | X      | X    | X      | X             |               | X     |
|Mitwirkender                 | X      | X      | X    | X      |               |               | X     |
|Leser                      |        |        |      |        |               |               | X     |
|Ressourcenrichtlinienmitwirkender |        |        |      |        |               | X             |       |
|Benutzerzugriffsadministrator   |        |        |      |        | X             |               |       |

### <a name="custom-rbac-role-definition-and-assignment"></a>Definition und Zuweisung der benutzerdefinierten RBAC-Rolle

Benutzerdefinierte RBAC-Rollen werden zurzeit für Verwaltungsgruppen nicht unterstützt.  Weitere Informationen zum Status dieses Elements finden Sie im [Feedbackforum zu Verwaltungsgruppen](https://aka.ms/mgfeedback).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links:

- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](management-groups-create.md)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](management-groups-manage.md)
- [Installieren des Azure PowerShell-Moduls](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Spezifikationen zur REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
