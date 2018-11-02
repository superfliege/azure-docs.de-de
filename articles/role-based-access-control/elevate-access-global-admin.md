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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/15/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: a2f66078a817f5e6ad7296df11634a1a6130a055
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321664"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory

Wenn Sie [globaler Administrator](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) in Azure Active Directory (Azure AD) sind, möchten Sie unter Umständen folgende Aktionen ausführen:

- Wiedererlangen des Zugriffs auf ein Azure-Abonnement, wenn ein Benutzer keinen Zugriff mehr hat
- Gewähren des Zugriffs auf ein Azure-Abonnement für einen anderen Benutzer oder Sie selbst
- Anzeigen aller Azure-Abonnements in einer Organisation
- Zulassen des Zugriffs auf alle Azure-Abonnements für eine Automation-App (etwa eine App für die Rechnungsstellung oder Überwachung)

In diesem Artikel werden die verschiedenen Möglichkeiten beschrieben, wie Sie Ihre Zugriffsrechte in Azure AD erhöhen können.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="overview"></a>Übersicht

Azure AD und Azure-Ressourcen werden unabhängig voneinander geschützt. Das bedeutet, dass Azure AD-Rollenzuweisungen keinen Zugriff auf Azure-Ressourcen gewähren und Azure-Rollenzuweisungen keinen Zugriff auf Azure AD. Wenn Sie jedoch globaler Administrator in Azure AD sind, können Sie sich selbst Zugriff auf alle Azure-Abonnements und Verwaltungsgruppen in Ihrem Verzeichnis zuweisen. Verwenden Sie diese Funktion, wenn Sie keinen Zugriff auf Azure-Abonnementressourcen wie virtuelle Computer oder Speicherkonten haben und Sie Ihre globalen Administratorrechte verwenden möchten, um Zugriff auf diese Ressourcen zu erhalten.

Wenn Sie Ihre Zugriffsrechte erhöhen, wird Ihnen die Rolle [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) in Azure für den Stammbereich (`/`) zugewiesen. Auf diese Weise können Sie alle Ressourcen anzeigen und den Zugriff in jeder Abonnement- oder Verwaltungsgruppe im Verzeichnis zuweisen. Die Rollenzuweisung „Benutzerzugriffsadministrator“ kann mit PowerShell entfernt werden.

Sie sollten dieses erhöhte Zugriffsrecht entfernen, sobald Sie die Änderungen vorgenommen haben, die im Stammbereich erforderlich sind.

![Erhöhen von Zugriffsrechten](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Azure-Portal

Führen Sie diese Schritte aus, um die Zugriffsrechte für einen globalen Administrator mit dem Azure-Portal zu erhöhen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) oder [Azure Active Directory Admin Center](https://aad.portal.azure.com) als globaler Administrator an.

1. Klicken Sie in der Navigationsliste auf **Azure Active Directory** und dann auf **Eigenschaften**.

   ![Azure AD-Eigenschaften: Screenshot](./media/elevate-access-global-admin/aad-properties.png)

1. Legen Sie unter **Zugriffsverwaltung für Azure-Ressourcen** den Schalter auf **Ja** fest.

   ![Zugriffsverwaltung für Azure-Ressourcen – Screenshot](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Wenn Sie den Schalter auf **Ja** festlegen, wird Ihnen in Azure RBAC im Stammbereich (/) die Rolle „Benutzerzugriffsadministrator“ zugewiesen. Dadurch erhalten Sie die Berechtigung, Rollen in allen Azure-Abonnements und Verwaltungsgruppen zuzuweisen, die diesem Azure AD-Verzeichnis zugeordnet sind. Dieser Schalter ist nur für Benutzer verfügbar, denen in Azure AD die Rolle des globalen Administrators zugewiesen wurde.

   Wenn Sie den Schalter auf **Nein** festlegen, wird die Rolle „Benutzerzugriffsadministrator“ in Azure RBAC aus Ihrem Benutzerkonto entfernt. Sie können dann keine Rollen mehr in allen Azure-Abonnements und Verwaltungsgruppen zuweisen, die diesem Azure AD-Verzeichnis zugeordnet sind. Sie können nur die Azure-Abonnements und Verwaltungsgruppen anzeigen und verwalten, für die Ihnen der Zugriff gewährt wurde.

1. Klicken Sie auf **Speichern**, um Ihre Einstellung zu speichern.

   Diese Einstellung ist keine globale Eigenschaft und gilt nur für den aktuell angemeldeten Benutzer.

1. Führen Sie die Aufgaben aus, die Sie mit erhöhten Zugriffsrechten vornehmen müssen. Wenn Sie fertig sind, legen Sie wieder **Nein** fest.

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-role-assignment-at-the-root-scope-"></a>Auflisten der Rollenzuweisung im Stammbereich (/)

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

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>Entfernen einer Rollenzuweisung im Stammbereich (/)

Verwenden Sie den Befehl [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment), um die Zuweisung der Rolle „Benutzerzugriffsadministrator“ für einen Benutzer im Stammbereich (`/`) zu entfernen.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="rest-api"></a>REST-API

### <a name="elevate-access-for-a-global-administrator"></a>Erhöhen der Zugriffsrechte für einen globalen Administrator

Führen Sie die folgenden grundlegenden Schritte aus, um mithilfe der REST-API die Zugriffsrechte für einen globalen Administrator zu erhöhen.

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

### <a name="list-role-assignments-at-the-root-scope-"></a>Auflisten der Rollenzuweisungen im Stammbereich (/)

Sie können alle Rollenzuweisungen für einen Benutzer im Stammbereich (`/`) auflisten.

- Rufen Sie [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) auf. Dabei entspricht `{objectIdOfUser}` der Objekt-ID des Benutzers, dessen Rollenzuweisungen Sie abrufen möchten.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-the-root-scope-"></a>Auflisten von Ablehnungszuweisungen im Stammbereich (/)

Sie können alle Ablehnungszuweisungen für einen Benutzer im Stammbereich (`/`) auflisten.

- Rufen Sie „GET denyAssignments“ auf. Hierbei entspricht `{objectIdOfUser}` der Objekt-ID des Benutzers, dessen Ablehnungszuweisungen Sie abrufen möchten.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Entfernen der erhöhten Zugriffsrechte

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

2. Außerdem müssen Sie die Rollenzuweisung für den Verzeichnisadministrator im Verzeichnisbereich auflisten. Listen Sie alle Zuweisungen im Verzeichnisbereich für `principalId` des Verzeichnisadministrators auf, der den Aufruf mit erhöhten Zugriffsrechten vorgenommen hat. Dadurch werden alle Zuweisungen im Verzeichnis für „objectid“ aufgelistet.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Ein Verzeichnisadministrator sollte nicht über zu viele Zuweisungen verfügen. Wenn die obige Abfrage zu viele Zuweisungen zurückgibt, können Sie auch Abfragen für alle Zuweisungen auf der Verzeichnisbereichsebene durchführen und dann die Ergebnisse filtern: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Mit den oben aufgeführten Aufrufe wird eine Liste von Rollenzuweisungen zurückgegeben. Suchen Sie die Rollenzuweisung, bei der der Bereich `"/"` lautet und `roleDefinitionId` mit der Rollennamen-ID endet, die Sie in Schritt 1 ermittelt haben, und `principalId` der Objekt-ID des Verzeichnisadministrators entspricht. 
    
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

- [Grundlegendes zu den verschiedenen Rollen](rbac-and-directory-admin-roles.md)
- [Rollenbasierte Zugriffssteuerung mit REST](role-assignments-rest.md)
