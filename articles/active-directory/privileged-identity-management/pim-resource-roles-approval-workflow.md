---
title: Genehmigungsworkflow für Azure-Ressourcenrollen in Privileged Identity Management| Microsoft-Dokumentation
description: In diesem Artikel wird der Prozess des Genehmigungsworkflows für Azure-Ressourcen beschrieben.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 42b0a8f94ff09b308a579b962bc99c4796c73c2e
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443034"
---
# <a name="approval-workflow-for-azure-resource-roles-in-privileged-identity-management"></a>Genehmigungsworkflow für Azure-Ressourcenrollen in Privileged Identity Management

Über den Genehmigungsworkflow in Privileged Identity Management (PIM) für Azure-Ressourcenrollen können Administratoren den Zugriff auf wichtige Ressourcen weiter schützen oder einschränken. Das heißt, Administratoren können für die Aktivierung von Rollenzuweisungen die Notwendigkeit einer Genehmigung festlegen. 

Das Konzept der Ressourcenhierarchie gilt nur für Azure-Ressourcenrollen. Diese Hierarchie ermöglicht die Vererbung von Rollenzuweisungen von einem übergeordneten Ressourcenobjekt bis zu allen untergeordneten Ressourcen des übergeordneten Containers. 

Beispielsweise nutzt Bob als Ressourcenadministrator PIM, um Alice im Contoso-Abonnement der Rolle „Besitzer“ als berechtigtes Mitglied zuzuweisen. Durch diese Zuweisung ist Alice eine berechtigte Besitzerin aller Ressourcengruppencontainer im Contoso-Abonnement. Alice ist zudem eine berechtigte Besitzerin aller Ressourcen (z.B. virtuelle Computer) in den einzelnen Ressourcengruppen des Abonnements. 

Angenommen, das Contoso-Abonnement enthält drei Ressourcengruppen: Fabrikam Test, Fabrikam Dev und Fabrikam Prod. Jede dieser Ressourcengruppen enthält einen einzelnen virtuellen Computer.

PIM-Einstellungen werden für jede Rolle einer Ressource konfiguriert. Im Gegensatz zu Zuweisungen werden diese Einstellungen nicht vererbt und gelten nur für die Ressourcenrolle. [Hier finden Sie weitere Informationen zu berechtigten Zuweisungen und zur Ressourcensichtbarkeit](pim-resource-roles-eligible-visibility.md).

Fortführung des Beispiels: Bob nutzt PIM, um für alle Mitglieder der Rolle „Besitzer“ im Contoso-Abonnement die Durchführung des Genehmigungsvorgangs zu erzwingen, bevor die Aktivierung erfolgen kann. Außerdem erzwingt Bob die Genehmigung für Mitglieder der Rolle „Besitzer“ für diese Ressource, um die Ressourcen in der Ressourcengruppe „Fabrikam Prod“ zu schützen. Für die Rollen „Besitzer“ in „Fabrikam Test“ und „Fabrikam Dev“ ist keine Genehmigung für die Aktivierung erforderlich.

Wenn Alice die Aktivierung ihrer Rolle „Besitzer“ für das Contoso-Abonnement anfordert, muss die Anforderung von einer Person genehmigt (bzw. abgelehnt) werden, bevor die Rolle für sie aktiviert wird. Wenn Alice den [Umfang der Aktivierung](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) auf die Ressourcengruppe „Fabrikam Prod“ ausweiten möchte, muss diese Anforderung ebenfalls von einer Person genehmigt oder abgelehnt werden. Wenn Alice den Umfang der Aktivierung aber auf „Fabrikam Test“ und/oder „Fabrikam Dev“ ausweiten möchte, ist keine Genehmigung erforderlich.

Der Genehmigungsworkflow ist möglicherweise nicht für alle Mitglieder einer Rolle erforderlich. Stellen Sie sich ein Szenario vor, bei dem eine Organisation auf mehrere Vertragspartner zurückgreift, die Unterstützung bei der Entwicklung einer Anwendung leisten, die in einem Azure-Abonnement ausgeführt wird. Als Ressourcenadministrator möchten Sie, dass Mitarbeiter über eine Zugriffsberechtigung verfügen, ohne dass eine Genehmigung erforderlich ist, während für die Vertragspartner eine Genehmigung angefordert werden muss. Zum Konfigurieren des Genehmigungsworkflows nur für die Vertragspartner können Sie eine benutzerdefinierte Rolle mit den gleichen Berechtigungen wie für die Rolle erstellen, die den Mitarbeitern zugewiesen ist. Sie können eine Genehmigung der Aktivierung dieser benutzerdefinierten Rolle erzwingen. [Hier finden Sie weitere Informationen zu benutzerdefinierten Rollen](pim-resource-roles-custom-role-policy.md).

Führen Sie die folgenden Schritte aus, um den Genehmigungsworkflow zu konfigurieren und anzugeben, wer Anforderungen genehmigen oder ablehnen kann.

## <a name="require-approval-to-activate"></a>Erzwingen der Genehmigung für die Aktivierung

1. Browsen Sie im Azure-Portal zu PIM, und wählen Sie in der Liste eine Ressource aus.

   ![Bereich „Azure-Ressourcen“ mit einer ausgewählten Ressource](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. Wählen Sie im linken Bereich die Option **Rolleneinstellungen** aus.

3. Suchen Sie nach einer Rolle, und wählen Sie sie aus. Wählen Sie dann **Bearbeiten** aus, um die Einstellungen zu ändern.

   ![Schaltfläche „Bearbeiten“ für die Rolle „Operator“](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. Aktivieren Sie im Abschnitt **Aktivierung** das Kontrollkästchen **Genehmigung zum Aktivieren anfordern**.

   ![Abschnitt „Aktivierung“ der Rolleneinstellungen](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Angeben von genehmigenden Personen

Klicken Sie auf **Genehmigende Personen auswählen**, um den Bereich **Benutzer oder Gruppe auswählen** zu öffnen.

>[!NOTE]
>Sie müssen mindestens einen Benutzer oder eine Gruppe auswählen, um die Einstellung zu aktualisieren. Für genehmigende Personen gibt es keine Standardeinstellung.

Ressourcenadministratoren können der Liste mit den genehmigenden Personen eine beliebige Kombination von Benutzern und Gruppen hinzufügen. 

![Bereich „Benutzer oder Gruppe auswählen“ mit einem ausgewählten Benutzer](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Anfordern der Genehmigung für die Aktivierung

Das Anfordern der Genehmigung hat keine Auswirkungen auf das Verfahren, das ein Mitglied für die Aktivierung einhalten muss. [Sehen Sie sich die Schritte zum Aktivieren einer Rolle an](pim-resource-roles-activate-your-roles.md).

Wenn ein Mitglied die Aktivierung einer Rolle angefordert hat, für die eine Genehmigung erforderlich ist, und die Rolle nicht mehr benötigt wird, kann das Mitglied die Anforderung in PIM stornieren.

Navigieren Sie zum Stornieren zu PIM, und wählen Sie **Eigene Anforderungen** aus. Suchen Sie nach der Anforderung, und wählen Sie **Abbrechen** aus.

![Bereich „Eigene Anforderungen“](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Genehmigen oder Ablehnen einer Anforderung

Sie müssen ein Mitglied der Liste mit den genehmigenden Personen sein, um eine Anforderung genehmigen oder ablehnen zu können. 

1. Wählen Sie in PIM im linken Menü auf der Registerkarte die Option **Anforderungen genehmigen** aus, und suchen Sie nach der Anforderung.

   ![Bereich „Anforderungen genehmigen“](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Wählen Sie die Anforderung aus, geben Sie eine Begründung für die Entscheidung an, und wählen Sie **Genehmigen** oder **Ablehnen** aus. Die Anforderung wird dann abgeschlossen.

   ![Ausgewählte Anforderung mit detaillierten Informationen](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Workflowbenachrichtigungen

Einige Fakten zu Workflowbenachrichtigungen:

- Alle Mitglieder der Liste mit den genehmigenden Personen werden per E-Mail benachrichtigt, wenn für die Anforderung einer Rolle die Überprüfung aussteht. E-Mail-Benachrichtigungen enthalten einen direkten Link zur Anforderung, damit diese von der genehmigenden Person genehmigt oder abgelehnt werden kann.
- Anforderungen werden von dem Mitglied bearbeitet, das in der Liste der genehmigenden Personen an erster Stelle steht. 
- Wenn eine genehmigende Person auf die Anforderung reagiert, werden alle Mitglieder der Liste über die Aktion informiert. 
- Ressourcenadministratoren werden benachrichtigt, wenn ein genehmigtes Mitglied in seiner Rolle aktiv wird. 

>[!Note]
>Wenn ein Ressourcenadministrator der Meinung ist, dass ein genehmigtes Mitglied nicht aktiv sein sollte, kann er die aktive Rollenzuweisung in PIM entfernen. Ressourcenadministratoren werden zwar nur über ausstehende Anforderungen benachrichtigt, wenn sie Mitglieder der Liste mit den genehmigenden Personen sind, aber sie können ausstehende Anforderungen aller Benutzer anzeigen und abbrechen, indem sie diese in PIM aufrufen. 

## <a name="next-steps"></a>Nächste Schritte

[Use custom roles to target PIM settings](pim-resource-roles-custom-role-policy.md) (Verwenden von benutzerdefinierten Rollen für PIM-Einstellungen)
