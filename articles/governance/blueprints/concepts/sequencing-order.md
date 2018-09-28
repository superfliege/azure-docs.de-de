---
title: Verstehen der Bereitstellungsreihenfolge in Azure Blueprint
description: Erfahren Sie mehr über den Lebenszyklus einer Blaupause und die einzelnen Phasen.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: c09fb26d8375e08281241aaed3f6f6e30acc755b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955451"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Verstehen der Bereitstellungsreihenfolge in Azure Blueprint

Azure Blueprint bestimmt mit einer **Reihenfolge**, die Abfolge der Ressourcenerstellung beim Verarbeiten der Zuweisung einer Blaupause. Dieser Artikel erläutert die standardmäßig verwendete Reihenfolge, wie Sie diese anpassen können und wie die benutzerdefinierte Reihenfolge verarbeitet wird.

In den JSON-Beispielen gibt es Variablen, die Sie durch Ihre eigenen Werte ersetzen müssen:

- Ersetzen Sie `{YourMG}` durch den Namen Ihrer Verwaltungsgruppe.

## <a name="default-sequencing-order"></a>Standardreihenfolge

Wenn die Blaupause keine Anweisung zum Bereitstellen von Artefakten enthält oder gleich NULL ist, wird die folgende Reihenfolge verwendet:

- **Rollenzuweisungsartefakte** auf Abonnementebene werden nach Artefaktnamen sortiert
- **Richtlinienzuweisungsartefakte** auf Abonnementebene werden nach Artefaktnamen sortiert
- **Azure Resource Manager-Vorlagenartefakte** auf Abonnementebene werden nach Artefaktnamen sortiert
- **Ressourcengruppenartefakte** (und untergeordnete Artefakte) werden nach Platzhalternamen sortiert

In jedem verarbeiteten **Ressourcengruppenartefakt** wird die folgende Reihenfolge für Artefakte verwendet, die innerhalb dieser Ressourcengruppe erstellt werden:

- Einer Ressourcengruppe untergeordnete **Rollenzuweisungsartefakte** werden nach Artefaktnamen sortiert
- Einer Ressourcengruppe untergeordnete **Richtlinienzuweisungsartefakte** werden nach Artefaktnamen sortiert
- Einer Ressourcengruppe untergeordnete **Azure Resource Manager-Vorlagenartefakte** werden nach Artefaktnamen sortiert

## <a name="customizing-the-sequencing-order"></a>Anpassen der Reihenfolge

Beim Erstellen großer Blaupausen muss eine Ressource ggf. in einer bestimmten Reihenfolge in Beziehung zu einer anderen Ressource erstellt werden. Im gängigsten Szenario enthält eine Blaupause mehrere Azure Resource Manager-Vorlagen. Um das zu verarbeiten, ermöglichen Blaupausen das Definieren der Reihenfolge.

Dafür wird eine `dependsOn`-Eigenschaft in JSON definiert. Nur die Blaupause (für Ressourcengruppen) und Artefaktobjekte unterstützen diese Eigenschaft. `dependsOn` ist ein Zeichenfolgenarray von Artefaktnamen, das das jeweilige Artefakt im Vorfeld für seine Erstellung benötigt.

### <a name="example---blueprint-with-ordered-resource-group"></a>Beispiel: Blaupause mit sortierter Ressourcengruppe

Dies ist eine Beispielblaupause mit einer Ressourcengruppe, deren benutzerdefinierte Reihenfolge durch einen Wert für `dependsOn` definiert wird, und einer Standardressourcengruppe. In diesem Fall wird das Artefakt namens **assignPolicyTags** vor der Ressourcengruppe **ordered-rg** verarbeitet.
**standard-rg** wird gemäß der Standardreihenfolge verarbeitet.

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

Dies ist ein Beispiel für ein Richtlinienartefakt, das von einer Azure Resource Manager-Vorlage abhängt. Standardmäßig wird ein Richtlinienartefakt vor der Azure Resource Manager-Vorlage erstellt. So kann das Richtlinienartefakt warten, bis die Azure Resource Manager-Vorlage erstellt wurde.

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

Beim Erstellen wird eine topologische Sortierung verwendet, um das Abhängigkeitsdiagramm der Blaupause und ihrer Artefakte zu erstellen. So wird sichergestellt, dass mehrere Abhängigkeitsebenen zwischen Ressourcengruppen und Artefakten unterstützt werden.

Wenn die Abhängigkeit für eine Blaupause oder ein Artefakt deklariert wird, die die Standardreihenfolge nicht ändert, wird die Reihenfolge beibehalten. Beispiele: Eine Ressourcengruppe, die von einer Richtlinie auf Abonnementebene abhängt, oder eine der Ressourcengruppe „standard-rg“ untergeordnete Richtlinienzuweisung, die von der Rollenzuweisung abhängt, die der Ressourcengruppe „standard-rg“ untergeordnet ist. In beiden Fällen würde `dependsOn` die Standardreihenfolge nicht ändern, sodass keine Änderungen anfallen würden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](parameters.md) vertraut.
- Erfahren Sie, wie Sie[Ressourcen in Blaupausen sperren](resource-locking.md).
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).
- Beheben Sie bei der Blaupausenzuweisung auftretende Probleme mithilfe der [allgemeinen Lösungsanleitung](../troubleshoot/general.md).