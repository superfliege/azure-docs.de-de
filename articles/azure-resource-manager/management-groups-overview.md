---
title: Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen | Microsoft-Dokumentation
description: Informationen zu Verwaltungsgruppen und deren Verwendung.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2018
ms.author: rithorn
ms.openlocfilehash: 31e71f153c7bbf76b0f06f8f17a74c43cc1b1c81
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen 

Wenn Ihre Organisation über viele Abonnements verfügt, benötigen Sie möglicherweise eine Möglichkeit zur effizienten Verwaltung von Zugriff, Richtlinien und Konformität für diese Abonnements. Azure-Verwaltungsgruppen stellen einen abonnementübergreifenden Bereich dar. Sie organisieren Abonnements in Containern, die als „Verwaltungsgruppen“ bezeichnet werden, und wenden Ihre Governancebedingungen auf die Verwaltungsgruppen an. Alle Abonnements in einer Verwaltungsgruppe erben automatisch die auf die Verwaltungsgruppe angewendeten Bedingungen. Verwaltungsgruppen ermöglichen Ihnen – unabhängig von den Arten Ihrer Abonnements – die unternehmenstaugliche Verwaltung in großem Umfang.

Das Feature für Verwaltungsgruppen ist in einer öffentlichen Vorschauversion verfügbar. Um mit der Verwendung von Verwaltungsgruppen zu beginnen, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und suchen Sie nach **Verwaltungsgruppen** im Abschnitt **Alle Dienste**. 

Beispielsweise können Sie eine Richtlinie auf eine Verwaltungsgruppe anwenden, die die verfügbaren Regionen zum Erstellen virtueller Computer beschränkt. Diese Richtlinie wird auf alle Verwaltungsgruppen, Abonnements und Ressourcen in dieser Verwaltungsgruppe angewendet, sodass virtuelle Computer nur in dieser Region erstellt werden können.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarchie von Verwaltungsgruppen und Abonnements 

Sie können eine flexible Struktur von Verwaltungsgruppen und Abonnements aufbauen, um Ihre Ressourcen für die einheitliche Richtlinien- und Zugriffsverwaltung in einer Hierarchie zu organisieren. Das folgende Diagramm zeigt eine Beispielhierarchie, die aus Verwaltungsgruppen und Abonnements besteht, die nach Abteilung organisiert sind.    

![Struktur](media/management-groups/MG_overview.png)

Indem Sie eine Hierarchie erstellen, die nach Abteilungen gruppiert ist, können Sie Rollen der [rollenbasierten Zugriffssteuerung (RBAC) in Azure](../role-based-access-control/overview.md) zuweisen, die entsprechend den Abteilungen in dieser Verwaltungsgruppe *erben*. Mithilfe von Verwaltungsgruppen können Sie die Workload und das Risiko von Fehlern verringern, indem Sie die Rolle nur einmal zuweisen müssen. 

### <a name="important-facts-about-management-groups"></a>Wichtige Fakten zu Verwaltungsgruppen
- 10.000 Verwaltungsgruppen können in einem einzigen Verzeichnis unterstützt werden. 
- Eine Verwaltungsgruppenstruktur kann bis zu sechs Ebenen unterstützen.
    - Dieser Grenzwert schließt nicht die Stammebene oder die Abonnementebene ein.
- Jede Verwaltungsgruppe kann nur ein übergeordnetes Element unterstützen.
- Jede Verwaltungsgruppe kann mehrere untergeordnete Elemente besitzen. 

### <a name="preview-subscription-visibility-limitation"></a>Eingeschränkte Sichtbarkeit von Abonnements in der Vorschauversion 
Zurzeit weist die Vorschauversion die Einschränkung auf, dass keine Abonnements angezeigt werden können, deren Zugriff Sie geerbt haben. Der Zugriff wird an das Abonnement vererbt, der Azure Resource Manager kann jedoch vererbte Zugriffe noch nicht berücksichtigen.  

Wenn Sie Informationen zum Abonnement mithilfe der REST-API abrufen, werden Details zurückgegeben, da Sie zwar Zugriff auf die Abonnements haben, diese jedoch weder im Azure-Portal noch in Azure PowerShell angezeigt werden. 

An der Lösung dieses Problem wird zurzeit gearbeitet. Noch vor der Ankündigung der allgemeinen Verfügbarkeit von Verwaltungsgruppen wird eine Lösung bereitgestellt.  

### <a name="cloud-solution-providercsp-limitation-during-preview"></a>CSP-Einschränkung (Cloud Solution Provider) in der Vorschauversion 
Es gibt eine aktuelle Einschränkung für CSP-Partner (Cloud Solution Provider), die dazu führt, dass sie ihre Kundenverwaltungsgruppen nicht im Kundenverzeichnis erstellen oder verwalten können.  
An der Lösung dieses Problem wird zurzeit gearbeitet. Noch vor der Ankündigung der allgemeinen Verfügbarkeit von Verwaltungsgruppen wird eine Lösung bereitgestellt.


## <a name="root-management-group-for-each-directory"></a>Stammverwaltungsgruppe für jedes Verzeichnis

Jedes Verzeichnis erhält eine einzelne Verwaltungsgruppe auf oberster Ebene, die als Stammverwaltungsgruppe bezeichnet wird. Die Stammverwaltungsgruppe ist in die Hierarchie integriert, sodass ihr alle Verwaltungsgruppen und Abonnements untergeordnet sind. Diese Stammverwaltungsgruppe ermöglicht das Anwenden von globalen Richtlinien und RBAC-Zuweisungen auf Verzeichnisebene. Der [Verzeichnisadministrator muss seine eigenen Rechte erhöhen](../role-based-access-control/elevate-access-global-admin.md), um der erste Besitzer dieser Gruppe zu sein. Sobald der Administrator der Besitzer der Gruppe ist, kann er jede RBAC-Rolle anderen Benutzern oder Gruppen des Verzeichnisses zuweisen, um die Hierarchie zu verwalten.  

### <a name="important-facts-about-the-root-management-group"></a>Wichtige Fakten zur Stammverwaltungsgruppe
- Für den Namen und die ID der Stammverwaltungsgruppe wird standardmäßig die Azure Active Directory-ID verwendet. Der Anzeigename kann jederzeit aktualisiert werden, damit er im Azure-Portal entsprechend angezeigt wird. 
- Alle Abonnements und Verwaltungsgruppen können der Stammverwaltungsgruppe im Verzeichnis untergeordnet sein.  
    - Es wird bei der globalen Verwaltung empfohlen, dass alle Elemente im Verzeichnis der Stammverwaltungsgruppe untergeordnet sind.  
    - Während der Public Preview sind nicht alle Abonnements im Verzeichnis automatisch untergeordnete Elemente des Stamms.   
    - Während der Public Preview werden neue Abonnements nicht automatisch der Stammverwaltungsgruppe unterstellt. 
- Die Stammverwaltungsgruppe kann im Gegensatz zu anderen Verwaltungsgruppen nicht verschoben oder gelöscht werden. 
  
## <a name="management-group-access"></a>Zugriff auf die Verwaltungsgruppe

Azure-Verwaltungsgruppen unterstützen die [rollenbasierte Zugriffssteuerung (RBAC) in Azure](../role-based-access-control/overview.md) für alle Ressourcenzugriffe und Rollendefinitionen. Diese Berechtigungen werden an untergeordnete Ressourcen in der Hierarchie vererbt.   

Auch wenn einer Verwaltungsgruppe alle [integrierten RBAC-Rollen](../role-based-access-control/overview.md#built-in-roles) zugewiesen werden können, werden im Allgemeinen vier Rollen verwendet: 
- **Besitzer** verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. 
- **Mitwirkende** können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.
- **Mitwirkende bei Ressourcenrichtlinien** können Richtlinien im Verzeichnis für die Ressourcen erstellen und verwalten.     
- **Leser** können vorhandene Azure-Ressourcen anzeigen. 


## <a name="next-steps"></a>Nächste Schritte 
Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links: 
- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](management-groups-create.md)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](management-groups-manage.md)
- [Installieren des Azure PowerShell-Moduls](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Spezifikationen zur REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

