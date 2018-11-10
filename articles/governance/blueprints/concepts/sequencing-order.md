---
title: Verstehen der Bereitstellungsreihenfolge in Azure Blueprint
description: Erfahren Sie mehr über den Lebenszyklus einer Blaupause und die einzelnen Phasen.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 4adf427727e7244bbde64a673e7353c1f8270c8a
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094577"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Verstehen der Bereitstellungsreihenfolge in Azure Blueprint

Azure Blueprint bestimmt mit einer **Reihenfolge**, die Abfolge der Ressourcenerstellung beim Verarbeiten der Zuweisung einer Blaupause. In diesem Artikel werden die folgenden Konzepte erläutert:

- Verwendete Standardreihenfolge
- Vorgehensweise zum Anpassen der Reihenfolge
- Verarbeitung der angepassten Reihenfolge

In den JSON-Beispielen gibt es Variablen, die Sie durch Ihre eigenen Werte ersetzen müssen:

- Ersetzen Sie `{YourMG}` durch den Namen Ihrer Verwaltungsgruppe.

## <a name="default-sequencing-order"></a>Standardreihenfolge

Wenn die Blaupause keine Anweisung zum Bereitstellen von Artefakten enthält oder gleich NULL ist, wird die folgende Reihenfolge verwendet:

- **Rollenzuweisungsartefakte** auf Abonnementebene werden nach Artefaktnamen sortiert
- **Richtlinienzuweisungsartefakte** auf Abonnementebene werden nach Artefaktnamen sortiert
- **Azure Resource Manager-Vorlagenartefakte** auf Abonnementebene werden nach Artefaktnamen sortiert
- **Ressourcengruppenartefakte** (und untergeordnete Artefakte) werden nach Platzhalternamen sortiert

In jedem Artefakt vom Typ **Ressourcengruppe** wird für Artefakte, die innerhalb dieser Ressourcengruppe erstellt werden, die folgende Reihenfolge verwendet:

- Einer Ressourcengruppe untergeordnete **Rollenzuweisungsartefakte** werden nach Artefaktnamen sortiert
- Einer Ressourcengruppe untergeordnete **Richtlinienzuweisungsartefakte** werden nach Artefaktnamen sortiert
- Einer Ressourcengruppe untergeordnete **Azure Resource Manager-Vorlagenartefakte** werden nach Artefaktnamen sortiert

## <a name="customizing-the-sequencing-order"></a>Anpassen der Reihenfolge

Beim Erstellen großer Blaupausen müssen Ressourcen ggf. in einer bestimmten Reihenfolge erstellt werden. Im gängigsten Muster dieses Szenarios enthält eine Blaupause mehrere Azure Resource Manager-Vorlagen. Für diesen Fall ermöglicht Blueprints das Definieren der Reihenfolge.

Zum Angeben der Reihenfolge wird im JSON-Code eine Eigenschaft vom Typ `dependsOn` definiert. Nur die Blaupause (für Ressourcengruppen) und Artefaktobjekte unterstützen diese Eigenschaft. `dependsOn` ist ein Zeichenfolgenarray von Artefaktnamen, das das jeweilige Artefakt im Vorfeld für seine Erstellung benötigt.

### <a name="example---blueprint-with-ordered-resource-group"></a>Beispiel: Blaupause mit sortierter Ressourcengruppe

Diese exemplarische Blaupause verfügt über eine Ressourcengruppe mit benutzerdefinierter Reihenfolge (mittels Deklarierung eines Werts für `dependsOn`) sowie über eine Standardressourcengruppe. In diesem Fall wird das Artefakt namens **assignPolicyTags** vor der Ressourcengruppe **ordered-rg** verarbeitet. **standard-rg** wird gemäß der Standardreihenfolge verarbeitet.

```json
{
    "properties": {
        "description": "Example blueprint with custom sequencing order",
        "resourceGroups": {
            "ordered-rg": {
                "dependsOn": [
                    "assignPolicyTags"
                ],
                "metadata": {
                    "description": "Resource Group that waits for 'assignPolicyTags' creation"
                }
            },
            "standard-rg": {
                "metadata": {
                    "description": "Resource Group that follows the standard sequence ordering"
                }
            }
        },
        "targetScope": "subscription"
    },
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint",
    "type": "Microsoft.Blueprint/blueprints",
    "name": "mySequencedBlueprint"
}
```

### <a name="example---artifact-with-custom-order"></a>Beispiel: Artefakt mit benutzerdefinierter Reihenfolge

Bei diesem Beispiel handelt es sich um ein Richtlinienartefakt, das von einer Azure Resource Manager-Vorlage abhängt. Standardmäßig wird ein Richtlinienartefakt vor der Azure Resource Manager-Vorlage erstellt. Bei Verwendung dieser Reihenfolge kann das Richtlinienartefakt warten, bis die Azure Resource Manager-Vorlage erstellt wurde.

```json
{
    "properties": {
        "displayName": "Assigns an identifying tag",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "resourceGroup": "standard-rg",
        "dependsOn": [
            "customTemplate"
        ]
    },
    "kind": "policyAssignment",
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint/artifacts/assignPolicyTags",
    "type": "Microsoft.Blueprint/artifacts",
    "name": "assignPolicyTags"
}
```

## <a name="processing-the-customized-sequence"></a>Verarbeiten der benutzerdefinierten Reihenfolge

Beim Erstellen wird eine topologische Sortierung verwendet, um das Abhängigkeitsdiagramm der Blaupausenartefakte zu erstellen. Die Überprüfung stellt sicher, dass alle Abhängigkeitsebenen zwischen Ressourcengruppen und Artefakten unterstützt werden.

Wird eine Artefaktabhängigkeit deklariert, die keine Änderung der Standardreihenfolge zur Folge hat, wird keine Änderung vorgenommen. Ein Beispiel wäre etwa eine Ressourcengruppe, die von einer Richtlinie auf Abonnementebene abhängt. Ein weiteres Beispiel wäre eine untergeordnete Richtlinienzuweisung der Ressourcengruppe „standard-rg“, die von der untergeordneten Rollenzuweisung der Ressourcengruppe „standard-rg“ abhängt. In beiden Fällen würde `dependsOn` die Standardreihenfolge nicht ändern, sodass keine Änderungen anfallen würden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](parameters.md) vertraut.
- Erfahren Sie, wie Sie[Ressourcen in Blaupausen sperren](resource-locking.md).
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).
- Beheben Sie Probleme bei der Blueprintzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](../troubleshoot/general.md).