---
title: Erstellen von benutzerdefinierten Rollen für die rollenbasierte Zugriffssteuerung und Zuweisen der Rollen zu internen und externen Benutzern in Azure | Microsoft-Dokumentation
description: Zuweisen von benutzerdefinierten, mithilfe von PowerShell und der Befehlszeilenschnittstelle erstellten RBAC-Rollen zu internen und externen Benutzern
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: kgremban
ms.assetid: ''
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: b60b30e3a5a4f5adec4fbef8c4e981ad034a7f6c
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/11/2018
---
# <a name="intro-on-role-based-access-control"></a>Einführung in die rollenbasierte Zugriffssteuerung

Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) ist ein nur im Azure-Portal verfügbares Feature, mit dem Besitzer eines Abonnements Benutzern, die bestimmte Ressourcenbereiche in ihrer Umgebung verwalten können, differenzierte Rollen zuweisen.

RBAC ermöglicht eine bessere Sicherheitsverwaltung für große Organisationen sowie für kleine und mittelständische Unternehmen, die mit externen Projektmitarbeitern, Lieferanten oder Freiberuflern arbeiten und für diese Zugriff auf bestimmte Ressourcen in der Umgebung benötigen, nicht aber auf die gesamte Infrastruktur oder abrechnungsrelevante Bereiche. Die rollenbasierte Zugriffssteuerung bietet Flexibilität: Sie können ein Azure-Abonnement besitzen, das vom Administratorkonto (Dienstadministratorrolle auf Abonnementebene) verwaltet wird, und mehrere Benutzer dazu einladen, im gleichen Abonnement zu arbeiten, ohne dass diese über Administratorrechte für das Konto verfügen. Aus verwaltungs- und abrechnungstechnischer Sicht ist die rollenbasierte Zugriffssteuerung hinsichtlich Zeit- und Verwaltungsaufwand eine effiziente Option für die Nutzung von Azure in verschiedenen Szenarien.

## <a name="prerequisites"></a>Voraussetzungen
Für die Nutzung der rollenbasierten Zugriffssteuerung in einer Azure-Umgebung ist Folgendes erforderlich:

* Ein eigenständiges Azure-Abonnement, das einem Benutzer als Besitzer zugewiesen ist (Abonnementrolle)
* Besitzerrolle des Azure-Abonnements
* Zugriff auf das [Azure-Portal](https://portal.azure.com)
* Stellen Sie sicher, dass der folgende Ressourcenanbieter für das Benutzerabonnement registriert ist: **Microsoft.Authorization**. Weitere Informationen zum Registrieren von Ressourcenanbietern finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für Resource Manager](../azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Office 365-Abonnements oder Azure Active Directory-Lizenzen (beispielsweise: Zugriff auf Azure Active Directory), die über das Office 365 Admin Center bereitgestellt wurden, sind für die Nutzung bei der rollenbasierten Zugriffssteuerung nicht qualifiziert.

## <a name="how-can-rbac-be-used"></a>Verwenden der rollenbasierten Zugriffssteuerung
Die rollenbasierte Zugriffssteuerung kann in Azure in drei verschiedenen Bereichen angewendet werden. Hierbei handelt es sich um die folgenden drei Bereiche, nach Wichtigkeit sortiert:

* Abonnement (höchste)
* Ressourcengruppe
* Ressourcenbereich (die niedrigste Zugriffsebene, die gezielt Berechtigungen für einen einzelnen Azure-Ressourcenbereich bietet)

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Zuweisen von RBAC-Rollen für den Abonnementbereich
In folgenden beiden Szenarien wird die rollenbasierte Zugriffssteuerung im Allgemeinen genutzt (es gibt allerdings noch weitere Szenarien):

* Organisationsexterne Benutzer (die nicht zum Azure Active Directory-Mandanten des Administratorbenutzers gehören) werden eingeladen, bestimmte Ressourcen oder das gesamte Abonnement zu verwalten.
* Sie arbeiten mit Benutzern innerhalb der Organisation (diese gehören zum Azure Active Directory-Mandanten des Benutzers), die aber verschiedenen Teams oder Gruppen angehören und differenzierten Zugriff auf das gesamte Abonnement oder auf bestimmte Ressourcengruppen oder Ressourcenbereiche in der Umgebung benötigen.

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Gewähren des Zugriffs auf Abonnementebene für einen Benutzer außerhalb von Azure Active Directory
RBAC-Rollen können nur von **Besitzern** des Abonnements zugeteilt werden. Daher muss der Administrator als ein Benutzer angemeldet sein, dem diese Rolle vorab zugewiesen wurde, oder der das Azure-Abonnement erstellt hat.

Nachdem Sie sich als Administrator beim Azure-Portal angemeldet haben, wählen Sie „Abonnements“ und dann das gewünschte Abonnement aus.
![Blatt „Abonnement“ im Azure-Portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) Wenn der Administratorbenutzer das Azure-Abonnement erworben hat, wird der Benutzer standardmäßig als **Kontoadministrator** angezeigt. Dies ist die Abonnementrolle. Weitere Informationen zu den Azure-Abonnementrollen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](/billing/billing-add-change-azure-subscription-administrator.md).

In diesem Beispiel ist der Benutzer „alflanigan@outlook.com“ der **Besitzer** des Abonnements „Free Trial“ im AAD-Mandanten „Default tenant Azure“. Da dieser Benutzer das Azure-Abonnement mit dem ursprünglichen Microsoft-Konto „Outlook“ (Microsoft-Konto = Outlook, Live usw.) erstellt hat, lautet der Standarddomänenname für alle weiteren Benutzer, die diesem Mandanten hinzugefügt werden, **„@alflaniganuoutlook.onmicrosoft.com“**. Die Syntax der neuen Domäne wird gebildet, indem der Benutzername und der Domänenname des Benutzers, der den Mandanten erstellt hat, zusammengesetzt werden und die Erweiterung **„.onmicrosoft.com“** angefügt wird.
Darüber hinaus können Benutzer sich mit einem benutzerdefinierten Domänennamen beim Mandanten anmelden, nachdem dieser Name dem neuen Mandanten hinzugefügt und in diesem überprüft wurde. Weitere Informationen zum Überprüfen eines benutzerdefinierten Domänennamens in einem Azure Active Directory-Mandanten finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Verzeichnis](/active-directory/active-directory-add-domain).

In diesem Beispiel enthält das Verzeichnis „Default tenant Azure“ nur Benutzer mit dem Domänennamen „@alflanigan.onmicrosoft.com“.

Nach dem Auswählen des Abonnements muss der Administratorbenutzer auf **Zugriffssteuerung (IAM)** und dann auf **Neue Rolle hinzufügen** klicken.





![Zugriffssteuerung (IAM) im Azure-Portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Hinzufügen eines neuen Benutzers im Feature „Zugriffssteuerung (IAM)“ im Azure-Portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

Der nächste Schritt besteht darin, die zuzuweisende RBAC-Rolle sowie den Benutzer auszuwählen, dem die Rolle zugewiesen werden soll. Im Dropdownmenü **Rolle** werden dem Administratorbenutzer nur die integrierten RBAC-Rollen angezeigt, die in Azure verfügbar sind. Ausführlichere Erläuterungen der einzelnen Rollen und der zuweisbaren Bereiche finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](role-based-access-built-in-roles.md).

Der Administratorbenutzer muss dann die E-Mail-Adresse des externen Benutzers hinzufügen. Das erwartete Verhalten: Der Benutzer wird im vorhandenen Mandanten nicht angezeigt. Nachdem der externe Benutzer eingeladen wurde, wird er unter **Abonnements > Zugriffssteuerung (IAM)** mit allen aktuellen Benutzern angezeigt, denen derzeit für den Abonnementbereich eine RBAC-Rolle zugewiesen ist.





![Hinzufügen von Berechtigungen zur neuen RBAC-Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![Liste der RBAC-Rollen auf Abonnementebene](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

Der Benutzer „chessercarlton@gmail.com“ wurde eingeladen, **Besitzer** des Abonnements „Free Trial“ zu werden. Nach dem Senden der Einladung erhält der externe Benutzer eine E-Mail-Bestätigung mit einem Aktivierungslink.
![E-Mail-Einladung für eine RBAC-Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Da der neue Benutzer ein organisationsexterner Benutzer ist, sind im Verzeichnis „Default tenant Azure“ keine Attribute für ihn vorhanden. Diese werden erstellt, nachdem der externe Benutzer der Aufnahme in das Verzeichnis zugestimmt hat, das dem Abonnement zugeordnet ist, für das dem Benutzer eine Rolle zugewiesen wurde.





![E-Mail-Einladungsnachricht für eine RBAC-Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

Der externe Benutzer wird ab jetzt im Azure Active Directory-Mandanten als externer Benutzer angezeigt, auch im Azure-Portal.





![Blatt „Benutzer“ im Azure-Portal für Azure Active Directory](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)



In der **Benutzeransicht** können die externen Benutzer anhand des unterschiedlichen Symboltyps im Azure-Portal erkannt werden.

Wenn jedoch einem externen Benutzer im Bereich **Abonnement** Zugriff als **Besitzer** oder **Mitwirkender** erteilt wird, darf der Benutzer nicht auf das Verzeichnis des Administratorbenutzers zugreifen, sofern der **globale Administrator** dies nicht gestattet. In den Benutzereigenschaften kann der **Benutzertyp** ermittelt werden, der über zwei allgemeine Parameter verfügt: **Mitglied** und **Gast**. Ein Mitglied ist ein Benutzer, der im Verzeichnis registriert ist, ein Gast dagegen ist ein Benutzer, der von einer externen Quelle in das Verzeichnis eingeladen wurde. Weitere Informationen finden Sie unter [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Stellen Sie nach der Eingabe der Anmeldeinformationen im Portal sicher, dass der externe Benutzer das richtige Verzeichnis für die Anmeldung auswählt. Ein Benutzer kann über Zugriff auf mehrere Verzeichnisse verfügen und eins der Verzeichnisse auswählen, indem er in der oberen rechten Ecke des Azure-Portals auf den Benutzernamen klickt und dann aus der Dropdownliste das geeignete Verzeichnis auswählt.

Als Gast in einem Verzeichnis kann ein externer Benutzer alle Ressourcen für das Azure-Abonnement verwalten, aber nicht auf das Verzeichnis zugreifen.





![Zugriffsbeschränkung auf Azure Active Directory im Azure-Portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory und Azure-Abonnements weisen keine Überordnungs-/Unterordnungsbeziehungen auf, wie sie für andere Azure-Ressourcen (z.B. virtuelle Computer, virtuelle Netzwerke, Web-Apps, Speicher usw.) mit einem Azure-Abonnement gelten. Alle letztgenannten Ressourcen werden in einem Azure-Abonnement erstellt, verwaltet und abgerechnet. Ein Azure-Abonnement wird zum Verwalten des Zugriffs auf ein Azure-Verzeichnis verwendet. Weitere Informationen finden Sie unter [Beziehung zwischen einem Azure-Abonnement und Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Von allen integrierten RBAC-Rollen bieten **Besitzer** und **Mitwirkender** vollständigen Verwaltungszugriff auf alle Ressourcen in der Umgebung. Der Unterschied besteht darin, dass ein Mitwirkender keine RBAC-Rollen erstellen und löschen kann. Die anderen integrierten Rollen, wie z.B. **Mitwirkender für virtuelle Computer** bieten vollständigen Verwaltungszugriff nur auf die mit Namen angegebenen Ressourcen, unabhängig von der **Ressourcengruppe**, in der sie erstellt werden.

Durch Zuweisen der integrierten RBAC-Rolle **Mitwirkender für virtuelle Computer** auf Abonnementebene gilt für den Benutzer, dem die Rolle zugewiesen wurde, Folgendes:

* Anzeigen aller virtuellen Computer, unabhängig vom Bereitstellungsdatum und der Ressourcengruppe, zu der sie gehören
* Vollständiger Verwaltungszugriff auf die virtuellen Computer im Abonnement
* Keine Anzeige anderer Ressourcentypen im Abonnement
* Kein Anwenden von Änderungen im Hinblick auf die Abrechnung

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Zuweisen einer integrierten RBAC-Rolle zu einem externen Benutzer
In einem anderen Szenario in diesem Test wird der externe Benutzer „alflanigan@gmail.com“ als **Mitwirkender für virtuelle Computer** hinzugefügt.




![Integrierte Rolle: Mitwirkender von virtuellen Computern](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

Normalerweise kann dieser externe Benutzer mit dieser integrierten Rolle nur virtuelle Computer und die zugehörigen Resource Manager-Ressourcen anzeigen und verwalten, die für die Bereitstellung erforderlich sind. Auslegungsbedingt bieten diese eingeschränkten Rollen nur Zugriff auf ihre im Azure-Portal erstellten entsprechenden Ressourcen.



![Übersicht über die Rolle „Mitwirkender für virtuelle Computer“ im Azure-Portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Gewähren des Zugriffs auf Abonnementebene für einen Benutzer im gleichen Verzeichnis
Der Ablauf entspricht dem Prozess des Hinzufügens eines externen Benutzers – sowohl aus Sicht des Administrators, der die RBAC-Rolle gewährt, als auch aus Sicht des Benutzers, dem Zugriff auf die Rolle gewährt wird. Der Unterschied besteht darin, dass der eingeladene Benutzer keine E-Mail-Einladungen erhält, da nach dem Anmelden alle Ressourcenbereiche innerhalb des Abonnements auf dem Dashboard verfügbar sind.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Zuweisen von RBAC-Rollen für den Ressourcengruppenbereich
Das Zuweisen einer RBAC-Rolle für den Bereich einer **Ressourcengruppe** funktioniert genauso wie das Zuweisen einer Rolle auf Abonnementebene. Dies gilt für beide Arten von Benutzern – extern oder intern (zum gleichen Verzeichnis gehörend). Die Benutzer, denen eine RBAC-Rolle zugewiesen wurde, dürfen in ihrer Umgebung nur die Ressourcengruppe sehen, für die ihnen mithilfe des Symbols **Ressourcengruppen** im Azure-Portal Zugriff gewährt wurde.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Zuweisen von RBAC-Rollen für den Ressourcenbereich
Das Zuweisen einer RBAC-Rolle für den Ressourcenbereich in Azure funktioniert genauso wie das Zuweisen einer Rolle auf Abonnement- oder Ressourcengruppenebene. Beide Szenarien verwenden den gleichen Workflow. Auch hier können die Benutzer, denen eine RBAC-Rolle zugewiesen wurde, nur die Elemente anzeigen, für die ihnen auf der Registerkarte **Alle Ressourcen** oder direkt in ihrem Dashboard Zugriff gewährt wurde.

Ein wichtiger Aspekt bei der rollenbasierten Zugriffssteuerung im Ressourcengruppen- oder Ressourcenbereich ist, dass die Benutzer sicherstellen müssen, dass sie sich beim richtigen Verzeichnis anmelden.





![Anmelden beim Verzeichnis im Azure-Portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Zuweisen von RBAC-Rollen für eine Azure Active Directory-Gruppe
Alle Szenarien, bei denen die rollenbasierte Zugriffssteuerung in den drei verschiedenen Bereichen in Azure zum Einsatz kommt, bieten den Vorteil, dass ein zugewiesener Benutzer diverse Ressourcen verwalten und bereitstellen kann, ohne ein persönliches Abonnement verwalten zu müssen. Unabhängig davon, ob die RBAC-Rolle für ein Abonnement, eine Ressourcengruppe oder einen Ressourcenbereich zugewiesen wurde – alle Ressourcen, die von den zugewiesenen Benutzern erstellt werden, werden in dem Azure-Abonnement abgerechnet, auf das die Benutzer Zugriff haben. Auf diese Weise hat der Benutzer, der über Administratorberechtigungen für die Abrechnung für das gesamte Azure-Abonnement verfügt, einen vollständigen Übersicht über die Nutzung, unabhängig davon, wer die Ressourcen verwaltet.

In größeren Organisationen können RBAC-Rollen auf die gleiche Weise auf Azure Active Directory-Gruppen angewendet werden. Auch hier möchte der Administrator differenzierten Zugriff für Teams oder ganze Abteilungen gewähren, nicht für jeden einzelnen Benutzer. Aus diesem Grund ist die rollenbasierte Zugriffssteuerung eine ausgesprochen zeit- und verwaltungseffiziente Option. Um dieses Beispiel zu veranschaulichen, wurde die Rolle **Mitwirkender** einer der Gruppen auf Abonnementebene im Mandanten hinzugefügt.





![Zusätzliche RBAC-Rolle für AAD-Gruppen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Bei diesen Gruppen handelt es sich um Sicherheitsgruppen, die nur innerhalb von Azure Active Directory bereitgestellt und verwaltet werden.

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a>Erstellen einer benutzerdefinierten RBAC-Rolle zum Öffnen von Supportanfragen mithilfe von PowerShell
Die in Azure verfügbaren integrierten Rollen gewähren basierend auf den verfügbaren Ressourcen in der Umgebung einen bestimmten Berechtigungsumfang. Wenn die integrierten Rollen Ihren Anforderungen jedoch nicht entsprechen, können Sie benutzerdefinierte Rollen erstellen.

Zum Erstellen einer benutzerdefinierten Rolle können Sie mit einer integrierten Rolle beginnen, sie bearbeiten und dann eine neue Rolle erstellen. In diesem Beispiel wurde die integrierte Rolle **Leser** so angepasst, dass der Benutzer die Möglichkeit hat, Supportanfragen zu erstellen.

Verwenden Sie in PowerShell den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition), um die Rolle **Leser** im JSON-Format zu exportieren.

```powershell
Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json
```

Die JSON-Ausgabe für die Leserolle sieht wie folgt aus.

```json
{
    "Name":  "Reader",
    "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "IsCustom":  false,
    "Description":  "Lets you view everything, but not make any changes.",
    "Actions":  [
                    "*/read"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Als Nächstes bearbeiten Sie die JSON-Ausgabe, um Ihre benutzerdefinierte Rolle zu erstellen.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/subscriptions/11111111-1111-1111-1111-111111111111"
                         ]
}
```

Eine typische Rolle besteht aus drei Hauptabschnitten: **Actions**, **NotActions** und **AssignableScopes**.

Im Abschnitt **Action** sind alle zulässigen Vorgänge für die Rolle aufgelistet. In diesem Fall muss für das Erstellen von Supporttickets der Vorgang **Microsoft.Support/&ast;** hinzugefügt werden. Es ist wichtig zu wissen, dass jeder Vorgang über einen Ressourcenanbieter verfügbar gemacht wird. Zum Abrufen einer Liste der Vorgänge für einen Ressourcenanbieter können Sie den Befehl [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) verwenden, oder sehen Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](role-based-access-control-resource-provider-operations.md) nach.

Für das Einschränken aller Aktionen für eine bestimmte RBAC-Rolle sind Ressourcenanbieter im Abschnitt **NotActions** aufgelistet.
Es ist zwingend erforderlich, dass die Rolle die expliziten ID der Abonnements enthält, in denen sie verwendet wird. Die Abonnement-IDs sind unter **AssignableScopes** aufgelistet. Andernfalls können Sie die Rolle nicht in Ihr Abonnement importieren.

Zum Erstellen der benutzerdefinierten Rolle verwenden Sie den Befehl [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) und stellen die aktualisierte JSON-Rollendefinitionsdatei bereit.

```powershell
New-AzureRmRoleDefinition -InputFile "C:\rbacrole2.json"
```

In diesem Beispiel lautet der Name für diese benutzerdefinierte Rolle „Reader support tickets access level“. Sie ermöglicht dem Benutzer, alle Inhalte des Abonnement anzuzeigen und auch Supportanfragen zu erstellen.

> [!NOTE]
> Die beiden einzigen integrierten Rollen, die einem Benutzer das Erstellen von Supportanfragen ermöglichen, sind **Besitzer** und **Mitwirkender**. Damit ein Benutzer Supportanfragen eröffnen kann, muss dem Benutzer eine Rolle für den Abonnementbereich zugewiesen werden, da alle Supportanfragen basierend auf einem Azure-Abonnement erstellt werden.

Die neue benutzerdefinierte Rolle ist jetzt im Azure-Portal verfügbar und kann Benutzern zugewiesen werden.

![Screenshot der in das Azure-Portal importierten benutzerdefinierten Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)

![Screenshot der Zuweisung der benutzerdefinierten importierten Rolle an einen Benutzer im gleichen Verzeichnis](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)

![Screenshot der Berechtigungen für die benutzerdefinierte importierte Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

Benutzer mit dieser benutzerdefinierten Rolle können jetzt neue Supportanfragen erstellen.

![Screenshot der Erstellung von Supportanfragen durch benutzerdefinierte Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)

Benutzer mit dieser benutzerdefinierten Rolle können keine weiteren Aktionen wie z.B. das Erstellen von virtuellen Computern oder Ressourcengruppen ausführen.

![Screenshot einer benutzerdefinierten Rolle, die keine virtuellen Computer erstellen kann](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)

![Screenshot einer benutzerdefinierten Rolle, die keine neuen RG erstellen kann](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a>Erstellen einer benutzerdefinierten RBAC-Rolle zum Öffnen von Supportanfragen mithilfe der Azure CLI

Die Schritte zum Erstellen einer benutzerdefinierten Rolle mithilfe der Azure CLI sind ähnlich wie bei der Verwendung von PowerShell, mit dem Unterschied, dass die JSON-Ausgabe anders ist.

In diesem Beispiel können Sie mit der integrierten Rolle **Leser** beginnen. Zum Auflisten der Aktionen der Leserrolle verwenden Sie den Befehl [az role definition list](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition list --name "Reader" --output json
```

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "notActions": []
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Erstellen Sie eine JSON-Datei mit dem folgenden Format. Der Vorgang **Microsoft.Support/&ast;** wurde in den **Actions**-Abschnitten hinzugefügt, sodass dieser Benutzer Supportanfragen erstellen kann und weiterhin die Rolle „Leser“ innehat. Sie müssen die ID des Abonnements, in dem diese Rolle verwendet wird, im Abschnitt **AssignableScopes** hinzufügen.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes": [
                            "/subscriptions/11111111-1111-1111-1111-111111111111"
                        ]
}
```

Zum Erstellen der benutzerdefinierten Rolle verwenden Sie den Befehl [az role definition create](/cli/azure/role/definition#az_role_definition_create).

```azurecli
az role definition create --role-definition ~/roles/rbacrole1.json
```

Die neue benutzerdefinierte Rolle ist jetzt im Azure-Portal verfügbar, und der Verwendungsprozess dieser Rolle ist der gleiche wie im vorhergehenden PowerShell-Abschnitt.

![Screenshot des Azure-Portals mit der benutzerdefinierten, mithilfe der CLI 1.0 erstellten Rolle](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)
