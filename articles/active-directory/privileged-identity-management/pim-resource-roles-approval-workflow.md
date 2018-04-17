---
title: Privileged Identity Management – Genehmigungsworkflow für Azure-Ressourcenrollen | Microsoft-Dokumentation
description: Es wird der Prozess des Genehmigungsworkflows für Azure-Ressourcen beschrieben.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c02d595d75b2d63558896054c185102ebb23cc9e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---approve"></a>Privileged Identity Management – Ressourcenrollen – Genehmigen

Über den Genehmigungsworkflow in PIM für Azure-Ressourcenrollen können Administratoren den Zugriff auf wichtige Ressourcen weiter schützen oder einschränken, indem für die Aktivierung der Rollenzuweisungen eine Genehmigung erzwungen wird. Ein spezielles Konzept von Azure-Ressourcenrollen ist die Ressourcenhierarchie. Diese Hierarchie ermöglicht die Vererbung von Rollenzuweisungen von einem übergeordneten Ressourcenobjekt bis hinunter zu allen untergeordneten Ressourcen des übergeordneten Containers. 

Beispielsweise nutzt Bob als Ressourcenadministrator PIM, um Alice im Contoso-Abonnement der Rolle „Besitzer“ als berechtigtes Mitglied zuzuweisen. Durch diese Zuweisung ist Alice auch eine berechtigte Besitzerin aller Ressourcengruppencontainer im Contoso-Abonnement sowie aller Ressourcen(z.B. virtuelle Computer) in den einzelnen Ressourcengruppen des Abonnements. Angenommen, das Contoso-Abonnement enthält drei Ressourcengruppen: Fabrikam Test, Fabrikam Dev und Fabrikam Prod. Jede dieser Ressourcengruppen enthält einen einzelnen virtuellen Computer.

PIM-Einstellungen werden für jede Rolle einer Ressource konfiguriert, und im Gegensatz zu Zuweisungen werden diese Einstellungen nicht vererbt und gelten nur für die Ressourcenrolle. [Hier finden Sie weitere Informationen zu berechtigten Zuweisungen und zur Ressourcensichtbarkeit.](pim-resource-roles-eligible-visibility.md)

Im obigen Beispiel nutzt Bob PIM, um für alle Mitglieder der Rolle „Besitzer“ im Contoso-Abonnement die Durchführung des Genehmigungsvorgangs zu erzwingen, bevor die Aktivierung erfolgen kann. Außerdem erzwingt Bob die Genehmigung für Mitglieder der Rolle „Besitzer“ für diese Ressource, um die in der Ressourcengruppe „Fabrikam Prod“ enthaltenen Ressourcen zu schützen. Für die Besitzerrollen in „Fabrikam Test“ und „Fabrikam Dev“ ist keine Genehmigung für die Aktivierung erforderlich.

Wenn Alice die Aktivierung Ihrer Rolle „Besitzer“ für das Contoso-Abonnement anfordert, muss die Anforderung von einer Person genehmigt (bzw. abgelehnt) werden, bevor die Rolle für sie aktiviert wird. Falls Alice den [Umfang der Aktivierung](pim-resource-roles-activate-your-roles.md#just-enough-administration) auf die Ressourcengruppe „Fabrikam Prod“ ausweiten möchte, muss diese Anforderung ebenfalls von einer Person genehmigt oder abgelehnt werden. Wenn Alice den Umfang der Aktivierung aber auf „Fabrikam Test“ und/oder „Fabrikam Dev“ ausweiten möchte, ist keine Genehmigung erforderlich.

Der Genehmigungsworkflow ist ggf. nicht für alle Mitglieder einer Rolle erforderlich. Stellen Sie sich ein Szenario vor, bei dem eine Organisation mehrere Vertragspartner nutzt, um Unterstützung bei der Entwicklung einer Anwendung zu leisten, die in einem Azure-Abonnement ausgeführt wird. Als Ressourcenadministrator möchten Sie, dass Mitarbeiter über eine Zugriffsberechtigung verfügen, ohne dass eine Genehmigung erforderlich ist, während für die Vertragspartner eine Genehmigung angefordert werden muss. Zum Konfigurieren des Genehmigungsworkflows nur für die Vertragspartner können Sie eine benutzerdefinierte Rolle mit den gleichen Berechtigungen wie für die Rolle erstellen, die den Mitarbeitern zugewiesen ist, und eine Genehmigung der Aktivierung dieser benutzerdefinierten Rolle erzwingen. [Hier finden Sie weitere Informationen zu benutzerdefinierten Rollen](pim-resource-roles-custom-role-policy.md).

Führen Sie unten angegebenen Schritte aus, um den Genehmigungsworkflow zu konfigurieren und anzugeben, wer Anforderungen genehmigen oder ablehnen kann.

## <a name="require-approval-to-activate"></a>Erzwingen der Genehmigung für die Aktivierung

Navigieren Sie im Azure-Portal zu PIM, und wählen Sie in der Liste eine Ressource aus.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

Wählen Sie im linken Navigationsmenü die Option **Rolleneinstellungen**.

Suchen Sie nach einer Rolle, und wählen Sie sie aus. Klicken Sie dann auf **Bearbeiten**, um die Einstellungen zu ändern.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

Aktivieren Sie im Abschnitt „Aktivierung“ das Kontrollkästchen **Genehmigung zum Aktivieren anfordern**.

![](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Angeben von genehmigenden Personen

Klicken Sie auf **Genehmigende Personen auswählen**, um das Auswahlfenster zu öffnen.

>[!NOTE]
>Sie müssen mindestens einen Benutzer oder eine Gruppe auswählen, um die Einstellung zu aktualisieren. Für genehmigende Personen gibt es keine Standardeinstellung.

Ressourcenadministratoren können der Liste mit den genehmigenden Personen eine beliebige Kombination von Benutzern und Gruppen hinzufügen. 

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Anfordern der Genehmigung für die Aktivierung

Das Anfordern der Genehmigung hat keinerlei Auswirkungen auf das Verfahren, das ein Mitglied für die Aktivierung einhalten muss. [Sehen Sie sich die Schritte zum Aktivieren einer Rolle an](pim-resource-roles-activate-your-roles.md).

Wenn ein Mitglied die Aktivierung einer Rolle angefordert hat, für die eine Genehmigung erforderlich ist, und die Rolle nicht mehr benötigt wird, kann das Mitglied die Anforderung in PIM stornieren.

Navigieren Sie zum Stornieren zu PIM, und wählen Sie „Eigene Anforderungen“. Suchen Sie nach der Anforderung, und klicken Sie auf „Abbrechen“.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Genehmigen oder Ablehnen einer Anforderung

Sie müssen ein Mitglied der Liste mit den genehmigenden Personen sein, um eine Anforderung genehmigen oder ablehnen zu können. Wählen Sie in PIM im linken Navigationsmenü auf der Registerkarte die Option „Anforderungen genehmigen“, und suchen Sie nach der Anforderung.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

Wählen Sie die Anforderung aus, geben Sie eine Begründung für die Entscheidung an, und wählen Sie „Genehmigen“ oder „Ablehnen“, um die Anforderung abzuschließen.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Workflowbenachrichtigungen

- Alle Mitglieder der Liste mit den genehmigenden Personen werden per E-Mail benachrichtigt, wenn für die Anforderung einer Rolle die Überprüfung aussteht. E-Mail-Benachrichtigungen enthalten einen direkten Link zur Anforderung, damit diese von der genehmigenden Person genehmigt oder abgelehnt werden kann.
- Anforderungen werden von dem Mitglied bearbeitet, das in der Liste der genehmigenden Personen an erster Stelle steht. 
- Wenn eine genehmigende Person auf die Anforderung reagiert, werden alle Mitglieder der Liste über die Aktion informiert. 
- Ressourcenadministratoren werden benachrichtigt, wenn ein genehmigtes Mitglied in seiner Rolle aktiv wird. 

>[!Note]
>Wenn ein Ressourcenadministrator der Meinung ist, dass das genehmigte Mitglied nicht aktiv sein sollte, kann er die aktive Rollenzuweisung in PIM entfernen. Ressourcenadministratoren werden zwar nur über ausstehende Anforderungen benachrichtigt, wenn sie Mitglieder der Liste mit den genehmigenden Personen sind, aber sie können ausstehende Anforderungen aller Benutzer anzeigen und abbrechen, indem sie diese in PIM aufrufen. 

## <a name="next-steps"></a>Nächste Schritte

[Use custom roles to target PIM settings](pim-resource-roles-custom-role-policy.md) (Verwenden von benutzerdefinierten Rollen für PIM-Einstellungen)
