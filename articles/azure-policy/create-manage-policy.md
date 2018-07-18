---
title: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung in Organisationen mithilfe von Azure Policy
description: Verwenden Sie Azure Policy, um die Einhaltung von Standards, gesetzlichen Vorschriften und Auditanforderungen durchzusetzen, Kosten zu steuern, für konsistente Sicherheit und Leistung zu sorgen und unternehmensweite Entwurfsprinzipien vorzugeben.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 5a26a07bae46fb62e067853b9d85dc905f63d5f8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602158"
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung

Zur Einhaltung Ihrer Unternehmensstandards und Vereinbarungen zum Servicelevel müssen Sie mit der Erstellung und Verwaltung von Richtlinien in Azure vertraut sein. In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Policy einige allgemeinere Aufgaben im Zusammenhang mit der organisationsweiten Erstellung, Zuweisung und Verwaltung von Richtlinien durchführen:

> [!div class="checklist"]
> - Zuweisen einer Richtlinie zur Erzwingung einer Bedingung für die spätere Erstellung von Ressourcen
> - Erstellen und Zuweisen einer Initiativdefinition zur Nachverfolgung der Konformität für mehrere Ressourcen
> - Beheben von Problemen mit einer nicht konformen oder abgelehnten Ressource
> - Implementieren einer neuen Richtlinie in der gesamten Organisation

Lesen Sie die Schnellstartartikel, wenn Sie eine Richtlinie zum Ermitteln des aktuellen Konformitätszustands Ihrer vorhandenen Ressourcen zuweisen möchten. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="assign-a-policy"></a>Zuweisen einer Richtlinie

Für die Konformitätserzwingung mit Azure Policy muss zunächst eine Richtliniendefinition zugewiesen werden. Eine Richtliniendefinition definiert, unter welcher Bedingung eine Richtlinie erzwungen wird und welche Auswirkung sie haben soll. In diesem Beispiel weisen Sie eine integrierte Richtliniendefinition namens *SQL Server-Version 12.0 erfordern* zu, um die Bedingung durchzusetzen, dass alle SQL Server-Datenbanken die Version 12.0 aufweisen müssen, um die Konformitätsanforderungen zu erfüllen.

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen.

   ![Suchen nach „Policy“](media/create-manage-policy/search-policy.png)

2. Wählen Sie links auf der Seite „Azure Policy“ die Option **Zuweisungen**. Eine Zuweisung ist eine zugewiesene Richtlinie, die innerhalb eines bestimmten Bereichs angewendet werden soll.
3. Wählen Sie im oberen Bereich der Seite **Richtlinien – Zuweisungen** die Option **Richtlinie zuweisen**.

   ![Zuweisen einer Richtliniendefinition](media/create-manage-policy/select-assign-policy.png)

4. Wählen Sie auf der Seite **Richtlinie zuweisen** den **Bereich** aus, indem Sie auf die Auslassungspunkte klicken und ein Abonnement (erforderlich) und eine Ressourcengruppe (optional) auswählen. Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird.  Klicken Sie dann unten im Abschnitt der Seite **Bereich** auf **Auswählen**.

   Dieses Beispiel verwendet das **Contoso-Abonnement**. Ihr Abonnement wird sich davon unterscheiden.

5. Wenn Sie eine oder mehrere Ressourcengruppen (wenn Sie nur ein Abonnement erfasst haben) oder bestimmte Ressourcen innerhalb einer Ressourcengruppe (bei der Bereichswahl) ausschließen möchten, können Sie **Ausschlüsse** von der Richtlinienzuweisung konfigurieren. Lassen Sie dieses Feld im Moment leer.

6. Wählen Sie die **Richtliniendefinition** mit den Auslassungspunkten, um die Liste der verfügbaren Definitionen zu öffnen. Sie können den **Typ** der Richtliniendefinition nach *Integriert* filtern, um alle Definitionen anzuzeigen und ihre Beschreibungen zu lesen.

7. Wählen Sie **SQL Server-Version 12.0 erfordern** aus. Wenn Sie die Option nicht direkt finden, geben Sie in das Suchfeld **SQL Server erfordern** ein, und drücken Sie dann die EINGABETASTE oder klicken Sie außerhalb des Suchfelds. Klicken Sie unten auf der Seite **Verfügbare Definitionen** auf **Auswählen**, nachdem Sie die Richtliniendefinition gefunden und ausgewählt haben.

   ![Suchen nach einer Richtlinie](media/create-manage-policy/select-available-definition.png)

8. Der **Zuweisungsname** wird automatisch mit dem ausgewählten Richtliniennamen gefüllt, kann aber geändert werden. In diesem Beispiel verwenden Sie *SQL Server-Version 12.0 erfordern*. Geben Sie ggf. auch eine **Beschreibung** ein. Die Beschreibung enthält Details zu dieser Richtlinienzuweisung.

9. Klicken Sie auf **Zuweisen**.

## <a name="implement-a-new-custom-policy"></a>Implementieren einer neuen benutzerdefinierten Richtlinie

Sie haben eine integrierte Richtliniendefinition zugewiesen und können nun weitere Aktionen mit Azure Policy ausführen. Erstellen Sie als Nächstes eine neue benutzerdefinierte Richtlinie zur Kosteneinsparung, die sicherstellt, dass in der Umgebung keine virtuellen Computer der G-Serie erstellt werden können. Wenn also ein Benutzer in Ihrer Organisation versucht, einen virtuellen Computer aus der G-Serie zu erstellen, wird die Anforderung abgelehnt.

1. Wählen Sie links auf der Seite „Azure Policy“ unter **ERSTELLUNG** die Option **Definitionen**.

   ![„Definition“ unter „Authoring“ (Erstellung)](media/create-manage-policy/definition-under-authoring.png)

2. Wählen Sie oben auf der Seite **+ Richtliniendefinition** aus. Daraufhin wird die Seite **Richtliniendefinition** geöffnet.
3. Geben Sie Folgendes ein:

   - Die Verwaltungsgruppe oder das Abonnement, in der bzw. in dem die Richtliniendefinition gespeichert ist. Wählen Sie über die Auslassungspunkte bei **Speicherort der Definition** aus.

     > [!NOTE]
     > Wenn Sie diese Richtliniendefinition auf mehrere Abonnements anwenden möchten, muss der Speicherort eine Verwaltungsgruppe sein, die die Abonnements enthält, denen Sie die Richtlinie zuweisen. Dasselbe gilt für eine Initiativdefinition.

   - Den Namen der Richtliniendefinition: *Require VM SKUs smaller than the G series* (VM-SKUs müssen kleiner als die G-Serie sein)
   - Den Zweck der Richtliniendefinition: *This policy definition enforces that all VMs created in this scope have SKUs smaller than the G series to reduce cost.* (Diese Richtliniendefinition dient zur Kostensenkung und bewirkt, dass die SKU von virtuellen Computern, die in diesem Bereich erstellt werden, kleiner als die G-Serie ist.)
   - Wählen Sie eine der vorhandenen Optionen, oder erstellen Sie für diese Richtliniendefinition eine neue Kategorie.
   - Kopieren Sie den folgenden JSON-Code, und aktualisieren Sie ihn dann mit folgenden Mitteln für Ihre Anforderungen:
      - Richtlinienparameter
      - Richtlinienregeln/-bedingungen (in unserem Fall: VM-SKU-Größe gleich G-Serie)
      - Wirkung der Richtlinie (in unserem Fall: **deny**, also „Ablehnen“)

    Der JSON-Code sollte wie folgt aussehen: Fügen Sie den überarbeiteten Code in das Azure-Portal ein.

    ```json
    {
        "policyRule": {
            "if": {
                "allOf": [{
                        "field": "type",
                        "equals": "Microsoft.Compute/virtualMachines"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/sku.name",
                        "like": "Standard_G*"
                    }
                ]
            },
            "then": {
                "effect": "deny"
            }
        }
    }
    ```

    In der Richtlinienregel muss die Eigenschaft *field* einen der folgenden Werte haben: „name“, „type“, „location“, „tags“ oder einen Alias. Ein Beispiel für einen Alias wäre `"Microsoft.Compute/VirtualMachines/Size"`.

    Weitere Beispiele für Azure-Richtlinien finden Sie unter [Vorlagen für Azure Policy](json-samples.md).

4. Wählen Sie **Speichern**aus.

## <a name="create-a-policy-definition-with-rest-api"></a>Erstellen einer Richtliniendefinition mit der REST-API

Sie können eine Richtlinie mit der REST-API für Richtliniendefinitionen erstellen. Die REST-API ermöglicht es Ihnen, Richtliniendefinitionen zu erstellen und zu löschen sowie Informationen zu vorhandenen Definitionen abzurufen.
Um eine Richtliniendefinition zu erstellen, verwenden Sie das folgende Beispiel:

```http-interactive
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Nehmen Sie einen Anforderungstext auf, der dem im folgenden Beispiel dargestellten ähnelt:

```json
{
    "properties": {
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Erstellen einer Richtliniendefinition mit PowerShell

Vergewissern Sie sich, dass Sie die neueste Version von Azure PowerShell installiert haben, bevor Sie mit dem PowerShell-Beispiel fortfahren. In Version 3.6.0 wurden Richtlinienparameter hinzugefügt. Bei Verwendung einer älteren Version wird bei den Beispielen ein Fehler mit dem Hinweis zurückgegeben, dass der Parameter nicht gefunden wurde.

Sie können eine Richtliniendefinition über das Cmdlet `New-AzureRmPolicyDefinition` erstellen.

Um eine Richtliniendefinition auf der Grundlage einer Datei zu erstellen, übergeben Sie den Pfad zur Datei. Verwenden Sie das folgende Beispiel für eine externe Datei:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Verwenden Sie das folgende Beispiel für eine lokale Datei:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Verwenden Sie zum Erstellen einer Richtliniendefinition mit einer Inline-Regel das folgende Beispiel:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "not": {
                    "field": "Microsoft.Storage/storageAccounts/accessTier",
                    "equals": "cool"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

Die Ausgabe wird in einem `$definition`-Objekt gespeichert, das während der Richtlinienzuweisung verwendet wird.
Im folgenden Beispiel wird eine Richtliniendefinition mit Parametern erstellt:

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying storage accounts.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzureRmPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>Anzeigen von Richtliniendefinitionen mit PowerShell

Verwenden Sie den folgenden Befehl, um alle Richtliniendefinitionen in Ihrem Abonnement anzuzeigen:

```azurepowershell-interactive
Get-AzureRmPolicyDefinition
```

Es gibt alle verfügbaren Richtliniendefinitionen, einschließlich integrierten Richtlinien, zurück. Jede Richtlinie wird im folgenden Format zurückgeben:

```
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Erstellen einer Richtliniendefinition mit Azure CLI

Sie können eine Richtliniendefinition mit dem entsprechenden Befehl über Azure CLI erstellen.
Verwenden Sie zum Erstellen einer Richtliniendefinition mit einer Inline-Regel das folgende Beispiel:

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "not": {
                    "field": "Microsoft.Storage/storageAccounts/accessTier",
                    "equals": "cool"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

### <a name="view-policy-definitions-with-azure-cli"></a>Anzeigen von Richtliniendefinitionen mit Azure CLI

Verwenden Sie den folgenden Befehl, um alle Richtliniendefinitionen in Ihrem Abonnement anzuzeigen:

```azurecli-interactive
az policy definition list
```

Es gibt alle verfügbaren Richtliniendefinitionen, einschließlich integrierten Richtlinien, zurück. Jede Richtlinie wird im folgenden Format zurückgeben:

```json
{
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",
    "displayName": "Allowed locations",
    "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "policyRule": {
        "if": {
            "not": {
                "field": "location",
                "in": "[parameters('listOfAllowedLocations')]"
            }
        },
        "then": {
            "effect": "Deny"
        }
    },
    "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Erstellen und Zuweisen einer Initiativdefinition

Mit einer Initiativdefinition können Sie mehrere Richtliniendefinitionen zu einer Gruppe zusammenfassen, um ein übergeordnetes Ziel zu erreichen. Eine Initiativdefinition stellt sicher, dass Ressourcen innerhalb des Definitionsbereichs mit den Richtliniendefinitionen konform sind, aus denen sich die Initiativdefinition zusammensetzt.  Weitere Informationen zu Initiativdefinitionen finden Sie in der [Übersicht über Azure Policy](azure-policy-introduction.md).

### <a name="create-an-initiative-definition"></a>Erstellen einer Initiativdefinition

1. Wählen Sie links auf der Seite „Azure Policy“ unter **ERSTELLUNG** die Option **Definitionen**.

   ![Klicken auf „Definitionen“](media/create-manage-policy/select-definitions.png)

2. Klicken Sie im oberen Bereich der Seite auf **+ Initiativdefinition**, um zur Seite **Initiativdefinition** zu gelangen.

   ![Initiativdefinition](media/create-manage-policy/initiative-definition.png)

3. Wählen Sie über die Auslassungspunkte unter **Speicherort der Definition** ein Abonnement zum Speichern der Definition aus.

4. Geben Sie **Name** und **Beschreibung** der Initiative ein.

   Dieses Beispiel stellt sicher, dass Ressourcen mit Richtliniendefinitionen zur Verbesserung der Sicherheit konform sind. Daher nennen wir die Initiative **Get Secure** (Sicherheit verbessern) und geben als Beschreibung Folgendes an: **This initiative has been created to handle all policy definitions associated with securing resources.** (Diese Initiative gilt für alle Richtliniendefinitionen, die mit dem Schutz von Ressourcen in Zusammenhang stehen.)

5. Wählen Sie für **Kategorie** eine der vorhandenen Optionen, oder erstellen Sie eine neue Kategorie.

6. Durchsuchen Sie die Liste mit den verfügbaren **Definitionen** (rechte Hälfte der Seite **Initiativdefinition**), und wählen Sie die Richtliniendefinitionen aus, die Sie dieser Initiative hinzufügen möchten. Fügen Sie für die Initiative **Get secure** die folgenden integrierten Richtliniendefinitionen hinzu, indem Sie auf **+** neben den Informationen zur Richtliniendefinition oder auf eine Zeile mit den Richtliniendefinitionen und dann auf der Detailseite auf die Option **+ Hinzufügen** klicken:
   - Require SQL Server version 12.0 (SQL Server-Version 12.0 fordern)
   - [Preview]: Monitor unprotected web applications in Security Center.
   - [Preview]: Monitor permissive network across in Security Center.
   - [Preview]: Monitor possible app Whitelisting in Security Center.
   - [Preview]: Monitor unencrypted VM Disks in Security Center.

   Nachdem Sie die Richtliniendefinition in der Liste ausgewählt haben, wird sie unter **RICHTLINIEN UND PARAMETER** hinzugefügt.

   ![Initiativdefinitionen](media/create-manage-policy/initiative-definition-2.png)

7. Klicken Sie auf **Speichern**.

### <a name="assign-an-initiative-definition"></a>Zuweisen einer Initiativdefinition

1. Wählen Sie links auf der Seite „Azure Policy“ unter **ERSTELLUNG** die Option **Definitionen**.
2. Suchen Sie die Initiativdefinition **Get Secure**, die Sie zuvor erstellt haben, und wählen Sie sie aus.
3. Wählen Sie oben auf der Seite **Zuweisen** aus, um die Seite **Get Secure: Initiative zuweisen** zu öffnen.

   ![Zuweisen einer Definition](media/create-manage-policy/assign-definition.png)

   Alternativ können Sie mit der rechten Maustaste auf die ausgewählte Zeile oder mit der linken Maustaste auf die Auslassungspunkte am Ende der Zeile für ein Kontextmenü klicken.  Wählen Sie dann **Zuweisen** aus.

   ![Rechtsklick auf eine Zeile](media/create-manage-policy/select-right-click.png)

4. Füllen Sie die Seite **Get Secure: Initiative zuweisen** aus, indem Sie die folgenden Beispielinformationen eingeben. Sie können Ihre eigenen Daten verwenden.

   - Bereich: Das Abonnement, für das Sie die Initiative gespeichert haben, ist die Standardeinstellung.  Sie können den Bereich ändern, um die Initiative einer Ressourcengruppe innerhalb des Speicherorts des Abonnements zuzuweisen.
   - Ausschlüsse: Konfigurieren Sie alle Ressourcen innerhalb des Bereichs, um zu verhindern, dass die Zuweisung der Initiative auf sie angewendet wird.
   - Initiativdefinition und Zuweisungsname: Get Secure (vorab ausgefüllt als Name der zuzuweisenden Initiative).
   - Beschreibung: Diese Initiativenzuweisung dient zur Erzwingung dieser Gruppe von Richtliniendefinitionen.

5. Klicken Sie auf **Zuweisen**.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Ausnehmen einer nicht konformen oder abgelehnten Ressource mithilfe eines Ausschlusses

Das obige Beispiel bewirkt, dass nach dem Zuweisen der Richtliniendefinition zur Forderung der SQL Server-Version 12.0 die Erstellung einer SQL Server-Instanz mit einer anderen Version als 12.0 abgelehnt wird. In diesem Abschnitt erfahren Sie, wie Sie das Problem, dass die Erstellung einer SQL Server-Instanz abgelehnt wird, durch Erstellen eines Ausschlusses für eine einzelne Ressourcengruppe lösen. Der Ausschluss verhindert die Durchsetzung der Richtlinie (oder Initiative) für diese Ressource. Im folgenden Beispiel ist jede SQL Server-Version in einer einzigen Ressourcengruppe erlaubt. Ein Ausschluss kann für eine Ressourcengruppe gelten oder auf einzelne Ressourcen eingeschränkt werden.

Eine aufgrund einer zugewiesenen Richtlinie oder Initiative verhinderte Bereitstellung kann an zwei Standorten eingesehen werden:

- In der Ressourcengruppe, auf die die Bereitstellung abzielt: Wählen Sie links auf der Seite die Option **Bereitstellungen**, und klicken Sie auf **Bereitstellungsname** der fehlerhaften Bereitstellung. Die abgelehnte Ressource wird mit dem Status _Unzulässig_ angezeigt. Um die Richtlinie oder Initiative und die Zuweisung zu bestimmen, die die Ressource abgelehnt hat, klicken Sie auf der Übersichtsseite zur Bereitstellung auf **Fehler. Klicken Sie hier, um Details anzuzeigen. ->**. Rechts auf der Seite wird ein Fenster mit den Fehlerinformationen geöffnet. Unter **Fehlerdetails** werden die GUIDs der zugehörigen Richtlinienobjekte angezeigt.

   ![Bereitstellung durch Richtlinienzuweisung abgelehnt](media/create-manage-policy/rg-deployment-denied.png)

- Wählen Sie links auf der Seite „Azure Policy“ die Option **Konformität** aus, und klicken Sie auf die Richtlinie **SQL Server-Version 12.0 erfordern**. Auf der Seite, die geöffnet wird, sehen Sie dann eine Erhöhung der Anzahl für **Ablehnen**. Auf der Registerkarte **Ereignisse** sehen Sie auch, wer die Bereitstellung versucht hat, die von der Richtlinie abgelehnt wurde.

   ![Konformitätsübersicht zu einer zugewiesenen Richtlinie](media/create-manage-policy/compliance-overview.png)

In diesem Beispiel hat Trent Baker, einer der Virtualisierungsspezialisten von Contoso, die erforderliche Arbeit geleistet. Sie müssen ihm eine Ausnahme gewähren, aber Sie möchten keine SQL-Server der Version 12.0 in einer der Ressourcengruppen haben. Zu diesem Zweck dient eine neue Ressourcengruppe **SQLServers_Excluded**, der nun eine Ausnahme für diese Richtlinienzuweisung gewährt wird.

### <a name="update-assignment-with-exclusion"></a>Aktualisieren einer Zuweisung mit Ausschluss

1. Wählen Sie links auf der Seite „Azure Policy“ unter **ERSTELLUNG** die Option **Zuweisungen**.
2. Durchsuchen Sie alle Richtlinienzuweisungen, und öffnen Sie die Zuweisung *Require SQL Server version 12.0* (SQL Server-Version 12.0 fordern).
3. Legen Sie den **Ausschluss** fest, indem Sie auf die Auslassungspunkte klicken und die auszuschließende Ressourcengruppe auswählen (in diesem Beispiel *SQLServers_Excluded*).

   ![Anfordern eines Ausschlusses](media/create-manage-policy/request-exclusion.png)

   > [!NOTE]
   > Je nach Richtlinie und Auswirkung kann der Ausschluss auch für bestimmte Ressourcen innerhalb einer Ressourcengruppe im Bereich der Zuweisung gewährt werden. Da in diesem Tutorial die Auswirkung **Ablehnen** verwendet wurde, wäre es nicht sinnvoll, den Ausschluss auf eine bestimmte Ressource festzulegen, die bereits vorhanden ist.

4. Klicken Sie auf **Auswählen**, und klicken Sie dann auf **Speichern**.

In diesem Abschnitt haben Sie die Ablehnung des Versuchs, eine unzulässige Version des SQL-Servers zu erstellen, aufgehoben, indem Sie einen Ausschluss für eine einzelne Ressourcengruppe erstellt haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie die Ressourcen dieses Tutorials nicht mehr benötigen, führen Sie die folgenden Schritte aus, um alle weiter oben erstellten Zuweisungen oder Definitionen zu löschen:

1. Klicken Sie links auf der Seite „Azure Policy“ unter **ERSTELLUNG** auf **Definitionen** (oder auf **Zuweisungen**, wenn Sie eine Zuweisung löschen möchten).
2. Suchen Sie nach der neuen Initiativ- oder Richtliniendefinition (bzw. der Zuweisung), die Sie entfernen möchten.
3. Klicken Sie mit der rechten Maustaste auf die Zeile, oder wählen Sie die Auslassungspunkte am Ende der Definition (oder Zuweisung), und wählen Sie anschließend **Definition löschen** (bzw. **Zuweisung löschen**) aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes erreicht:

> [!div class="checklist"]
> - Sie haben eine Richtlinie zur Erzwingung einer Bedingung für die spätere Erstellung von Ressourcen zugewiesen.
> - Sie haben eine Initiativdefinition zur Nachverfolgung der Konformität für mehrere Ressourcen erstellt und zugewiesen.
> - Sie haben ein Problem mit einer nicht konformen oder abgelehnten Ressource behoben.
> - Sie haben eine neue Richtlinie in der gesamten Organisation implementiert.

Weitere Informationen zu den Strukturen von Richtliniendefinitionen finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Struktur von Azure Policy-Definitionen](policy-definition.md)