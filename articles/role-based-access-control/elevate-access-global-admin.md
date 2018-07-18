---
title: Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie mit dem Azure-Portal oder der REST-API die Zugriffsrechte für einen globalen Administrator in Azure Active Directory erhöhen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e1e46d5fb786b09a4c006b61f52b3ac99aafd555
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266501"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory

Wenn Sie [globaler Administrator](../active-directory/active-directory-assign-admin-roles-azure-portal.md#global-administrator) in Azure Active Directory (Azure AD) sind, möchten Sie unter Umständen folgende Aktionen ausführen:

- Wiedererlangen des Zugriffs auf ein Azure-Abonnement, wenn ein Benutzer keinen Zugriff mehr hat
- Gewähren des Zugriffs auf ein Azure-Abonnement für einen anderen Benutzer oder Sie selbst
- Anzeigen aller Azure-Abonnements in einer Organisation
- Zulassen des Zugriffs auf alle Azure-Abonnements für eine Automation-App (etwa eine App für die Rechnungsstellung oder Überwachung)

Azure AD-Administratorrollen und rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure erstrecken sich nicht standardmäßig auf Azure AD und Azure. Als globaler Administrator in Azure AD können Sie jedoch Ihre Zugriffsrechte für die Verwaltung von Azure-Abonnements und Verwaltungsgruppen erhöhen. Bei Erhöhung der Zugriffsrechte wird Ihnen die Rolle [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) (eine RBAC-Rolle) für alle Abonnements für einen bestimmten Mandanten zugewiesen. Die Rolle „Benutzerzugriffsadministrator“ ermöglicht es Ihnen, anderen Benutzern Zugriff auf Azure-Ressourcen im Stammbereich (`/`) zu erteilen.

Die erhöhten Zugriffsrechte sollten nur vorübergehend und nur bei Bedarf zugewiesen werden.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="elevate-access-for-a-global-administrator-using-the-azure-portal"></a>Erhöhen der Zugriffsrechte für einen globalen Administrator mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) oder [Azure Active Directory Admin Center](https://aad.portal.azure.com) an.

1. Klicken Sie in der Navigationsliste auf **Azure Active Directory** und dann auf **Eigenschaften**.

   ![Azure AD-Eigenschaften: Screenshot](./media/elevate-access-global-admin/aad-properties.png)

1. Legen Sie für **Der globale Administrator kann Azure-Abonnements und Verwaltungsgruppen verwalten.** die Option **Ja** fest.

   ![Screenshot von „Der globale Administrator kann Azure-Abonnements und Verwaltungsgruppen verwalten.“](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Wenn Sie **Ja** festlegen, wird das Konto des globalen Administrators (der derzeit angemeldete Benutzer) in Azure RBAC zur Rolle „Benutzerzugriffsadministrator“ im Stammbereich (`/`) hinzugefügt. Dadurch erhalten Sie Zugriff, um alle mit Ihrem Azure AD-Mandanten verknüpften Azure-Abonnements anzuzeigen und Berichte für sie zu erstellen.

   Wenn Sie **Nein** festlegen, wird das Konto des globalen Administrators (der derzeit angemeldete Benutzer) in Azure RBAC aus der Rolle „Benutzerzugriffsadministrator“ entfernt. Sie können nicht alle mit dem Azure AD-Mandanten verknüpften Azure-Abonnements anzeigen und können nur die Azure-Abonnements anzeigen und verwalten, auf die Sie Zugriff erhalten haben.

1. Klicken Sie auf **Speichern**, um Ihre Einstellung zu speichern.

   Diese Einstellung ist keine globale Eigenschaft und gilt nur für den aktuell angemeldeten Benutzer.

1. Führen Sie die Aufgaben aus, die Sie mit erhöhten Zugriffsrechten vornehmen müssen. Wenn Sie fertig sind, legen Sie wieder **Nein** fest.

## <a name="list-role-assignment-at-the-root-scope--using-powershell"></a>Auflisten der Rollenzuweisung im Stammbereich (/) mit PowerShell

Verwenden Sie den Befehl [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment), um die Zuweisung der Rolle „Benutzerzugriffsadministrator“ für einen Benutzer im Stammbereich (`/`) aufzulisten.

```azurepowershell
Get-AzureRmRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
```

## <a name="remove-a-role-assignment-at-the-root-scope--using-powershell"></a>Entfernen einer Rollenzuweisung im Stammbereich (/) mit PowerShell

Verwenden Sie den Befehl [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment), um die Zuweisung der Rolle „Benutzerzugriffsadministrator“ für einen Benutzer im Stammbereich (`/`) zu entfernen.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="elevate-access-for-a-global-administrator-using-the-rest-api"></a>Erhöhen der Zugriffsrechte für einen globalen Administrator mit der REST-API

Führen Sie die folgenden grundlegenden Schritte aus, um mithilfe der REST-API die Zugriffsrechte für einen globalen Administrator zu erhöhen:

1. Rufen Sie mithilfe von REST `elevateAccess` auf. So erhalten Sie die Rolle des Benutzerzugriffsadministrators im Stammbereich (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Erstellen Sie eine [Rollenzuweisung](/rest/api/authorization/roleassignments), um eine beliebige Rolle in einem beliebigen Bereich zuzuweisen. Das folgende Beispiel zeigt die Eigenschaften für die Zuweisung der Rolle „{roleDefinitionID}“ im Stammbereich (`/`):

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Solange Sie Benutzerzugriffsadministrator sind, können Sie auch Rollenzuweisungen im Stammbereich (`/`) entfernen.

1. Entfernen Sie Ihre Berechtigungen als Benutzerzugriffsadministrator, bis sie wieder benötigt werden.

## <a name="list-role-assignments-at-the-root-scope--using-the-rest-api"></a>Auflisten der Rollenzuweisung im Stammbereich (/) mit der REST-API

Sie können alle Rollenzuweisungen für einen Benutzer im Stammbereich (`/`) auflisten.

- Rufen Sie [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) auf. Dabei entspricht `{objectIdOfUser}` der Objekt-ID des Benutzers, dessen Rollenzuweisungen Sie abrufen möchten.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

## <a name="remove-elevated-access-using-the-rest-api"></a>Entfernen des Zugriffs mit erhöhten Rechten über die REST-API

Beim Aufruf von `elevateAccess` erstellen Sie eine Rollenzuweisung für sich selbst. Um diese Berechtigungen zu widerrufen, müssen Sie die Zuweisung entfernen.

1. Rufen Sie [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) mit „User Access Administrator“ für `roleName` auf, um die Namens-ID der Rolle des Benutzerzugriffsadministrators abzurufen.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
        "roleName": "User Access Administrator",
        "type": "BuiltInRole",
        "description": "Lets you manage user access to Azure resources.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Authorization/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedOn": "2016-05-31T23:14:04.6964687Z",
        "createdBy": null,
        "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Speichern Sie die ID aus dem `name`-Parameter (in diesem Fall `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`).

2. Sie müssen auch die Rollenzuweisung für den Mandantenadministrator im Mandantenbereich auflisten. Listen Sie alle Zuweisungen im Mandantenbereich für `principalId` des Mandantenadministrators auf, der den Aufruf mit erhöhten Zugriffsrechten vorgenommen hat. Dadurch werden alle Zuweisungen im Mandanten für „objectid“ aufgelistet.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Ein Mandantenadministrator sollte nicht über zu viele Zuweisungen verfügen. Wenn die obige Abfrage zu viele Zuweisungen zurückgibt, können Sie auch Abfragen für alle Zuweisungen auf der Mandantenbereichsebene durchführen und dann die Ergebnisse filtern: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Mit den oben aufgeführten Aufrufe wird eine Liste von Rollenzuweisungen zurückgegeben. Suchen Sie die Rollenzuweisung, bei der der Bereich `"/"` lautet und `roleDefinitionId` mit der Rollennamen-ID endet, die Sie in Schritt 1 ermittelt haben, und `principalId` der Objekt-ID des Mandantenadministrators entspricht. 
    
    Beispielrollenzuweisung:

        ```json
        {
          "value": [
            {
              "properties": {
                "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                "principalId": "{objectID}",
                "scope": "/",
                "createdOn": "2016-08-17T19:21:16.3422480Z",
                "updatedOn": "2016-08-17T19:21:16.3422480Z",
                "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
                "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
              },
              "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
              "type": "Microsoft.Authorization/roleAssignments",
              "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
            }
          ],
          "nextLink": null
        }
        ```
        
    Speichern Sie die ID aus dem Parameter `name` erneut ( in diesem Fall e7dd75bc-06f6-4e71-9014-ee96a929d099).

    3. Verwenden Sie abschließend die Rollenzuweisungs-ID, um die durch `elevateAccess` hinzugefügte Zuweisung zu entfernen:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Rollenbasierte Zugriffssteuerung mit REST](role-assignments-rest.md)
- [Verwalten von Zugriffszuweisungen](role-assignments-users.md)
