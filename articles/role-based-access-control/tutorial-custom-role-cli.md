---
title: 'Tutorial: Erstellen einer benutzerdefinierten Rolle für Azure-Ressourcen mit der Azure CLI | Microsoft-Dokumentation'
description: Erste Schritte beim Erstellen einer benutzerdefinierten Rolle für Azure-Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/20/2019
ms.author: rolyon
ms.openlocfilehash: de1805d91f48b5718ecf293c2b8672ba40fb81a9
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56588159"
---
# <a name="tutorial-create-a-custom-role-for-azure-resources-using-azure-cli"></a>Tutorial: Erstellen einer benutzerdefinierten Rolle für Azure-Ressourcen mithilfe der Azure CLI

Wenn die integrierten Rollen für Azure-Ressourcen den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen [benutzerdefinierten Rollen](built-in-roles.md) erstellen. In diesem Tutorial erstellen Sie mithilfe der Azure CLI eine benutzerdefinierte Rolle namens „Reader Support Tickets“. Die benutzerdefinierte Rolle ermöglicht dem Benutzer, alle Inhalte der Verwaltungsebene eines Abonnement anzuzeigen und auch Supporttickets zu erstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer benutzerdefinierten Rolle
> * Auflisten benutzerdefinierter Rollen 
> * Aktualisieren einer benutzerdefinierten Rolle
> * Löschen einer benutzerdefinierten Rolle

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Lernprogramm abzuschließen, benötigen Sie:

- Berechtigungen zum Erstellen von benutzerdefinierten Rollen, etwa [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator)
- [Azure Cloud Shell](../cloud-shell/overview.md) oder [Azure CLI](/cli/azure/install-azure-cli).

## <a name="sign-in-to-azure-cli"></a>Anmelden bei der Azure-Befehlszeilenschnittstelle

Melden Sie sich bei der [Azure-Befehlszeilenschnittstelle](/cli/azure/authenticate-azure-cli) an.

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Zum Erstellen einer benutzerdefinierten Rolle beginnen Sie am besten mit einer JSON-Vorlage, fügen Ihre Änderungen hinzu und erstellen dann eine neue Rolle.

1. Überprüfen Sie die Liste der Vorgänge für den [Microsoft.Support-Ressourcenanbieter](resource-provider-operations.md#microsoftsupport). Es ist hilfreich, die Vorgänge zu kennen, die zum Erstellen der Berechtigungen verfügbar sind.

    | Vorgang | BESCHREIBUNG |
    | --- | --- |
    | Microsoft.Support/register/action | Führt die Registrierung beim Supportressourcenanbieter durch. |
    | Microsoft.Support/supportTickets/read | Ruft Details zu Supporttickets (einschließlich Status, Schweregrad, Kontaktdetails und Kommunikation) oder die Liste mit Supporttickets (abonnementübergreifend) ab. |
    | Microsoft.Support/supportTickets/write | Erstellt oder aktualisiert ein Supportticket. Sie können ein Supportticket zu technischen Problemen bzw. zu Problemen im Zusammenhang mit Abrechnung, Kontingenten oder Abonnementverwaltung erstellen. Der Schweregrad, die Kontaktinformationen und die Kommunikation für vorhandene Supporttickets können aktualisiert werden. |
    
1. Erstellen Sie eine neue Datei namens **ReaderSupportRole.json**.

1. Öffnen Sie „ReaderSupportRole.json“ in einem Editor, und fügen Sie den folgenden JSON-Code hinzu.

    Informationen zu den verschiedenen Eigenschaften finden Sie unter [Benutzerdefinierte Rollen für Azure-Ressourcen](custom-roles.md).

    ```json
    {
      "Name": "",
      "IsCustom": true,
      "Description": "",
      "Actions": [],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/{subscriptionId1}"
      ]
    }
    ```
    
1. Fügen Sie die folgenden Vorgänge zur Eigenschaft `Actions` hinzu. Diese Aktionen ermöglichen dem Benutzer, alle Inhalte des Abonnement anzuzeigen und Supporttickets zu erstellen.

    ```
    "*/read",
    "Microsoft.Support/*"
    ```

1. Rufen Sie die ID Ihres Abonnements mithilfe des Befehls [az account list](/cli/azure/account#az-account-list) ab.

    ```azurecli
    az account list --output table
    ```

1. Ersetzen Sie in `AssignableScopes` die Zeichenfolge `{subscriptionId1}` durch Ihre Abonnement-ID.

    Sie müssen explizite Abonnement-IDs hinzufügen. Andernfalls können Sie die Rolle nicht in Ihr Abonnement importieren.

1. Ändern Sie die Eigenschaften `Name` und `Description` in „Reader Support Tickets“ und „View everything in the subscription and also open support tickets“.

    Ihre JSON-Datei sollte wie folgt aussehen:

    ```json
    {
      "Name": "Reader Support Tickets",
      "IsCustom": true,
      "Description": "View everything in the subscription and also open support tickets.",
      "Actions": [
        "*/read",
        "Microsoft.Support/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    
1. Zum Erstellen der neuen benutzerdefinierten Rolle verwenden Sie den Befehl [az role definition create](/cli/azure/role/definition#az-role-definition-create) und geben die JSON-Rollendefinitionsdatei an.

    ```azurecli
    az role definition create --role-definition "~/CustomRoles/ReaderSupportRole.json"
    ```

    ```Output
    {
      "additionalProperties": {},
      "assignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ],
      "description": "View everything in the subscription and also open support tickets.",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
      "name": "22222222-2222-2222-2222-222222222222",
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Support/*"
          ],
          "additionalProperties": {},
          "dataActions": [],
          "notActions": [],
          "notDataActions": []
        }
      ],
      "roleName": "Reader Support Tickets",
      "roleType": "CustomRole",
      "type": "Microsoft.Authorization/roleDefinitions"
    }
    ```

    Die neue benutzerdefinierte Rolle ist jetzt verfügbar und kann wie integrierte Rollen Benutzern, Gruppen oder Dienstprinzipalen zugewiesen werden.

## <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen 

- Verwenden Sie zum Auflisten aller benutzerdefinierten Rollen den Befehl [az role definition list](/cli/azure/role/definition#az-role-definition-list) mit dem Parameter `--custom-role-only`.

    ```azurecli
    az role definition list --custom-role-only true
    ```
    
    ```Output
    [
      {
        "additionalProperties": {},
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ],
        "description": "View everything in the subscription and also open support tickets.",
        "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
        "name": "22222222-2222-2222-2222-222222222222",
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Support/*",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Insights/diagnosticSettings/*/read"
            ],
            "additionalProperties": {},
            "dataActions": [],
            "notActions": [],
            "notDataActions": []
          }
        ],
        "roleName": "Reader Support Tickets",
        "roleType": "CustomRole",
        "type": "Microsoft.Authorization/roleDefinitions"
      }
    ]
    ```
    
    Sie können die benutzerdefinierte Rolle auch im Azure-Portal anzeigen.

    ![Screenshot der in das Azure-Portal importierten benutzerdefinierten Rolle](./media/tutorial-custom-role-cli/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>Aktualisieren einer benutzerdefinierten Rolle

Wenn Sie die benutzerdefiniert Rolle aktualisieren möchten, aktualisieren Sie die JSON-Datei und dann die benutzerdefinierte Rolle.

1. Öffnen Sie die Datei „ReaderSupportRole.json“.

1. Fügen Sie in `Actions` den Vorgang zum Erstellen und Verwalten von Ressourcengruppenbereitstellungen (`"Microsoft.Resources/deployments/*"`) hinzu. Achten Sie darauf, ein Komma nach dem vorherigen Vorgang einzufügen.

    Ihre aktualisierte JSON-Datei sollte wie folgt aussehen:

    ```json
    {
      "Name": "Reader Support Tickets",
      "IsCustom": true,
      "Description": "View everything in the subscription and also open support tickets.",
      "Actions": [
        "*/read",
        "Microsoft.Support/*",
        "Microsoft.Resources/deployments/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
        
1. Führen Sie zum Aktualisieren der benutzerdefinierten Rolle den Befehl [az role definition update](/cli/azure/role/definition#az-role-definition-update) aus, und geben Sie die aktualisierte JSON-Datei an.

    ```azurecli
    az role definition update --role-definition "~/CustomRoles/ReaderSupportRole.json"
    ```

    ```Output
    {
      "additionalProperties": {},
      "assignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ],
      "description": "View everything in the subscription and also open support tickets.",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
      "name": "22222222-2222-2222-2222-222222222222",
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Support/*",
            "Microsoft.Resources/deployments/*"
          ],
          "additionalProperties": {},
          "dataActions": [],
          "notActions": [],
          "notDataActions": []
        }
      ],
      "roleName": "Reader Support Tickets",
      "roleType": "CustomRole",
      "type": "Microsoft.Authorization/roleDefinitions"
    }
    ```
    
## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

- Verwenden Sie den Befehl [az role definition delete](/cli/azure/role/definition#az-role-definition-delete), und geben Sie den Rollennamen oder die Rollen-ID an, um die benutzerdefinierte Rolle zu löschen.

    ```azurecli
    az role definition delete --name "Reader Support Tickets"
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen von benutzerdefinierten Rollen für Azure-Ressourcen mithilfe der Azure CLI](custom-roles-cli.md)