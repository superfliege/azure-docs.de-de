---
title: Struktur von Azure Policy-Definitionen
description: Beschreibt, wie die von Azure Policy verwendete Definition von Ressourcenrichtlinien es Ihnen ermöglicht, Konventionen für Ressourcen in Ihrer Organisation einzurichten, indem Sie beschreiben, wann die Richtlinie erzwungen werden soll und welche Auswirkungen erfolgen sollen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 0ff56b86243956d1fa6b51a6dfd14af9e00d8367
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212776"
---
# <a name="azure-policy-definition-structure"></a>Struktur von Azure Policy-Definitionen

Von Azure Policy verwendete Definitionen von Ressourcenrichtlinien ermöglichen es Ihnen, Konventionen für Ressourcen in Ihrer Organisation einzurichten, indem Sie beschreiben, wann die Richtlinie erzwungen werden soll und welche Folgen diese haben sollen. Durch Definieren von Konventionen können Sie Kosten beeinflussen und Ihre Ressourcen einfacher verwalten. Sie können beispielsweise angeben, dass nur bestimmte Typen virtueller Computer zulässig sind. Oder Sie können festlegen, dass alle Ressourcen ein bestimmtes Tag aufweisen. Richtlinien werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe.

Das von Azure Policy verwendete Schema finden Sie hier: [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

Eine Richtliniendefinition wird mithilfe von JSON erstellt. Die Richtliniendefinition enthält Elemente für Folgendes:

- Modus
- Parameter
- Anzeigename
- Beschreibung
- Richtlinienregel
  - Logische Auswertung
  - Wirkung

Die folgende JSON-Datei zeigt beispielsweise eine Richtlinie, die einschränkt, wo Ressourcen bereitgestellt werden:

```json
{
    "properties": {
        "mode": "all",
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

Alle Azure Policy-Beispiele finden Sie unter [Azure Policy-Beispiele](../samples/index.md).

## <a name="mode"></a>Mode

Der Modus (**mode**) bestimmt, welche Ressourcentypen für eine Richtlinie ausgewertet werden. Unterstützte Modi:

- `all`: Ressourcengruppen und alle Ressourcentypen werden ausgewertet.
- `indexed`: Nur Ressourcentypen, die Tags und Speicherort unterstützen, werden ausgewertet.

Es wird empfohlen, **mode** in den meisten Fällen auf `all` zu setzen. Alle über das Portal erstellten Richtliniendefinitionen verwenden für „mode“ die Option `all`. Wenn Sie PowerShell oder die Azure CLI verwenden, können Sie den **mode**-Parameter manuell angeben. Wenn die Richtliniendefinition keinen Wert für **mode** enthält, wird aus Gründen der Abwärtskompatibilität standardmäßig in Azure PowerShell auf `all` und in der Azure CLI auf `null` festgelegt, was `indexed` entspricht.

`indexed` sollte beim Erstellen von Richtlinien verwendet werden, die Tags oder Speicherorte erzwingen. Dies ist nicht erforderlich, verhindert aber, dass Ressourcen, die keine Tags und Speicherorte unterstützen, bei der Konformitätsprüfung als nicht konform angezeigt werden. Die einzige Ausnahme hierbei ist **Ressourcengruppen**. Richtlinien zum Erzwingen von Speicherort oder Tags einer Ressourcengruppe sollten **mode** auf `all` festlegen und speziell auf den Typ `Microsoft.Resources/subscriptions/resourceGroup` abzielen. Ein Beispiel finden Sie unter [Ressourcengruppen-Tags erzwingen](../samples/enforce-tag-rg.md).

## <a name="parameters"></a>Parameter

Parameter vereinfachen Ihre Richtlinienverwaltung, indem sie die Anzahl von Richtliniendefinitionen reduzieren. Es handelt sich dabei z.B. um Parameter wie die folgenden Felder auf einem Formular: `name`, `address`, `city`, `state`. Diese Parameter bleiben immer gleich, allerdings ändern sich ihre Werte auf Grundlage der Einträge des Einzelnen.
Parameter funktionieren beim Erstellen von Richtlinien genauso. Sie können die Richtlinie für verschiedene Szenarios wiederverwenden, indem Sie Parameter in eine Richtliniendefinition einbeziehen und verschiedene Werte verwenden.

> [!NOTE]
> Die Parameterdefinition für eine Richtlinien- oder Initiativendefinition kann nur bei der ersten Erstellung der Richtlinie oder Initiative konfiguriert werden. Die Parameterdefinition kann später nicht geändert werden.
> Dadurch wird verhindert, dass vorhandene Zuweisungen der Richtlinie oder Initiative indirekt als ungültig erklärt werden.

Beispielsweise könnten Sie eine Richtlinie für eine Ressourceneigenschaft verwenden, um die Standorte, an denen Ressourcen bereitgestellt werden können, einzuschränken. In diesem Fall sollten Sie die folgenden Parameter deklarieren, wenn Sie eine Richtlinie erstellen:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        }
    }
}
```

Der Typ eines Parameters kann Zeichenfolge oder Array sein. Die Metadateneigenschaft wird für Tools wie das Azure-Portal verwendet, um benutzerfreundliche Informationen anzuzeigen.

Innerhalb der Metadateneigenschaft können Sie mit **strongType** eine Liste der Optionen im Azure-Portal mit Mehrfachauswahl angeben. Derzeit zulässige Werte für **strongType** sind:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

In der Richtlinienregel wird die folgende Syntax für die `parameters`Bereitstellungswertefunktion verwendet, um auf Parameter zu verweisen:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Definitionsspeicherort

Beim Erstellen einer Initiative oder Richtliniendefinition ist es wichtig, den Definitionsspeicherort anzugeben.

Der Definitionsspeicherort bestimmt den Bereich, dem die Initiative oder Richtliniendefinition zugewiesen werden kann. Der Speicherort kann als Verwaltungsgruppe oder Abonnement angegeben werden.

> [!NOTE]
> Wenn Sie diese Richtliniendefinition auf mehrere Abonnements anwenden möchten, muss der Speicherort eine Verwaltungsgruppe sein, die die Abonnements enthält, denen Sie die Initiative oder Richtlinie zuweisen.

## <a name="display-name-and-description"></a>Anzeigename und Beschreibung

Sie können **displayName** und **description** verwenden, um die Richtliniendefinition zu definieren und den Kontext für die Verwendung anzugeben.

## <a name="policy-rule"></a>Richtlinienregel

Die Richtlinienregel besteht aus **If**- und **Then**-Blöcken. Im **If**-Block definieren Sie mindestens eine Bedingung, die angibt, wann die Richtlinie erzwungen wird. Auf diese Bedingungen können logische Operatoren angewendet werden, um das Szenario für eine Richtlinie präzise zu definieren.

Im **Then**-Block definieren Sie die Wirkung, die eintritt, wenn die **If**-Bedingungen erfüllt sind.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
    }
}
```

### <a name="logical-operators"></a>Logische Operatoren

Folgende logische Operatoren werden unterstützt:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

Die **not**-Syntax kehrt das Ergebnis der Bedingung um. Die **allOf**-Syntax gleicht der logischen **And**-Operation und erfordert, dass alle Bedingungen erfüllt sind. Die **anyOf**-Syntax gleicht der logischen **Or**-Operation und erfordert, dass mindestens eine Bedingung erfüllt ist.

Logische Operatoren können geschachtelt werden. Das folgende Beispiel zeigt eine **not**-Operation, die in einer **allOf**-Operation geschachtelt ist.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Bedingungen

Eine Bedingung überprüft, ob ein **Feld** bestimmte Kriterien erfüllt. Folgende Bedingungen werden unterstützt:

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"notMatch": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"exists": "bool"`

Bei Verwendung der Bedingungen **like** und **notLike** können Sie im Wert einen Platzhalter `*` angeben.
Der Wert darf maximal einen Platzhalter `*` enthalten.

Geben Sie bei Verwendung der Bedingungen **match** und **notMatch** für eine Ziffer `#`, für einen Buchstaben `?`, für alle Zeichen `.` und für ein Zeichen das gewünschte Zeichen ein. Beispiele finden Sie unter [Zulassen mehrerer Namensmuster](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Felder

Bedingungen werden mithilfe von Feldern gebildet. Ein Feld stellt Eigenschaften in der Anforderungsnutzlast einer Ressource dar, mit der der Zustand der Ressource beschrieben wird.

Folgende Felder werden unterstützt:

- `name`
- `fullName`
  - Gibt den vollständigen Namen der Ressource zurück. Der vollständige Name einer Ressource setzt sich zusammen aus dem Namen der Ressource und den vorangestellten Namen übergeordneter Ressourcen, sofern vorhanden (Beispiel: meinServer/meineDatenbank).
- `kind`
- `type`
- `location`
- `tags`
- `tags.<tagName>`
  - Wobei **\<tagName\>** der Name des Tags ist, auf das die Bedingung geprüft wird.
  - Beispiel: `tags.CostCenter`, wobei **CostCenter** der Name des Tags ist.
- `tags[<tagName>]`
  - Diese Klammersyntax unterstützt Tagnamen, die Punkte enthalten.
  - Wobei **\<tagName\>** der Name des Tags ist, auf das die Bedingung geprüft wird.
  - Beispiel: `tags[Acct.CostCenter]`, wobei **Acct.CostCenter** der Name des Tags ist.
- Eigenschaftenaliase – Eine Liste finden Sie unter [Aliase](#aliases).

### <a name="effect"></a>Wirkung

Die Richtlinie unterstützt die folgenden Arten von Effekten:

- **Deny** generiert ein Ereignis im Aktivitätsprotokoll und führt zu einem Fehler bei der Anforderung.
- **Audit** generiert eine Warnung im Überwachungsprotokoll, führt jedoch nicht zu einem Fehler bei der Anforderung.
- **Append** fügt der Anforderung verschiedene definierte Felder hinzu.
- **AuditIfNotExists** aktiviert das Überwachen, wenn eine Ressource nicht vorhanden ist.
- **DeployIfNotExists** stellt eine Ressource bereit, falls noch keine vorhanden ist.

Für **append**müssen Sie die folgenden Details angeben:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

Der Wert kann entweder eine Zeichenfolge oder ein Objekt im JSON-Format sein.

Mit **AuditIfNotExists** und **DeployIfNotExists** können Sie das Vorhandensein einer zugehörigen Ressource auswerten und eine Regel anwenden, wenn diese Ressource nicht vorhanden ist. Sie können z.B. erforderlich machen, dass ein Network Watcher für alle virtuellen Netzwerke bereitgestellt wird. Ein Beispiel für das Überwachen, wenn keine VM-Erweiterung bereitgestellt wird, finden Sie unter [Audit if extension does not exist (Überwachen bei nicht vorhandener Erweiterung)](../samples/audit-ext-not-exist.md).

Umfassende Informationen zu den einzelnen Auswirkungen, zur Reihenfolge der Auswertung und zu Eigenschaften sowie Beispiele finden Sie unter [Grundlegendes zu Richtlinienauswirkungen](effects.md).

### <a name="policy-functions"></a>Richtlinienfunktionen

Eine Teilmenge der [Resource Manager-Vorlagenfunktionen](../../../azure-resource-manager/resource-group-template-functions.md) können innerhalb einer Richtlinienregel verwendet werden. Diese Funktionen werden derzeit unterstützt:

- [parameters](../../../azure-resource-manager/resource-group-template-functions-deployment.md#parameters)
- [concat](../../../azure-resource-manager/resource-group-template-functions-array.md#concat)
- [Ressourcengruppe](../../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)
- [Abonnement](../../../azure-resource-manager/resource-group-template-functions-resource.md#subscription)

Darüber hinaus ist die `field` Funktion für Richtlinienregeln verfügbar. Diese Funktion ist in erster Linie für die Verwendung mit **AuditIfNotExists** und **DeployIfNotExists** zum Verweisen auf Felder in der Ressource bestimmt, die ausgewertet wird. Ein Beispiel hierfür finden Sie im [DeployIfNotExists-Beispiel](effects.md#deployifnotexists-example).

#### <a name="policy-function-examples"></a>Beispiele für Richtlinienfunktionen

Dieses Richtlinienregelbeispiel verwendet die Ressourcenfunktion `resourceGroup`, um die Eigenschaft **name** zu erhalten, kombiniert mit dem Array `concat` und der Objektfunktion, um eine `like`-Bedingung zu erstellen, die für den Ressourcennamen erzwingt, mit dem Ressourcengruppennamen zu beginnen.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

Dieses Richtlinienregelbeispiel verwendet die Ressourcenfunktion `resourceGroup`, um den Wert des Eigenschaftsarrays **tags** des Tags **CostCenter** in der Ressourcengruppe abzurufen und an das Tag **CostCenter** in der neuen Ressource anzuhängen.

```json
{
    "if": {
        "field": "tags.CostCenter",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "tags.CostCenter",
            "value": "[resourceGroup().tags.CostCenter]"
        }]
    }
}
```

## <a name="aliases"></a>Aliase

Eigenschaftenaliase dienen zum Zugreifen auf bestimmte Eigenschaften für einen Ressourcentyp. Mithilfe von Aliasen können Sie beschränken, welche Werte oder Bedingungen für eine Eigenschaft einer Ressourcen zulässig sind. Jeder Alias wird Pfaden in verschiedenen API-Versionen für einen bestimmten Ressourcentyp zugeordnet. Bei der Richtlinienauswertung ruft das Richtlinienmodul den Eigenschaftenpfad für diese API-Version ab.

Die Liste der Aliase wächst ständig. Um zu ermitteln, welche Aliase derzeit von Azure Policy unterstützt werden, verwenden Sie eine der folgenden Methoden:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  # Use Get-AzureRmPolicyAlias to list available providers
  Get-AzureRmPolicyAlias -ListAvailable

  # Use Get-AzureRmPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzureRmPolicyAlias -NamespaceMatch 'automation'
  ```

- Azure-Befehlszeilenschnittstelle

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST-API/ARM-Client

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>Grundlegendes zum [*]-Alias

Einige der verfügbaren Aliase weisen eine Version auf, die als „normaler“ Name angezeigt wird, an andere wird **[\*]** angefügt. Beispiel: 

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

Das erste Beispiel dient zur Auswertung des gesamten Arrays, wobei der Alias **[\*]** jedes Element des Arrays auswertet.

Sehen wir uns eine Richtlinienregel als Beispiel an. Diese Richtlinie wendet **Deny** (Verweigern) auf ein Speicherkonto an, für das ipRules konfiguriert sind, wenn **keine** der ipRules einen Wert von „127.0.0.0.1“ aufweist.

```json
"policyRule": {
    "if": {
        "allOf": [{
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "127.0.0.1"
            }
        ]
    },
    "then": {
        "effect": "deny",
    }
}
```

Das **IpRules**-Array sieht für das Beispiel wie folgt aus:

```json
"ipRules": [{
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Und so wird dieses Beispiel verarbeitet:

- `networkAcls.ipRules`: Überprüfen, ob das Array ungleich NULL ist. Die Auswertung ergibt TRUE, daher wird die Auswertung fortgesetzt.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
    "exists": "true"
  }
  ```

- `networkAcls.ipRules[*].value`: Überprüft jede _value_-Eigenschaft im **IpRules**-Array.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
    "notEquals": "127.0.0.1"
  }
  ```

  - Da es sich um ein Array handelt, wird jedes Element verarbeitet.

    - "127.0.0.1" != "127.0.0.1" wird als FALSE ausgewertet.
    - "127.0.0.1" != "192.168.1.1" wird als TRUE ausgewertet.
    - Mindestens eine _value_-Eigenschaft im **IpRules**-Array wurde als FALSE" ausgewertet, daher wird die Auswertung beendet.

Da eine Bedingung als FALSE ausgewertet wurde, wird die **Deny**-Auswirkung nicht ausgelöst.

## <a name="initiatives"></a>Initiativen

Mithilfe von Initiativen können Sie mehrere verwandte Richtliniendefinitionen gruppieren, um Zuweisungen und das Verwalten zu vereinfachen, indem Sie mit einer Gruppe als einzelnes Element arbeiten. Beispielsweise können Sie alle verknüpften Richtliniendefinitionen zum Markieren in einer einzelnen Initiative gruppieren. Anstatt jede Richtlinie einzeln zuzuweisen, wenden Sie die Initiative an.

Im folgenden Beispiel wird veranschaulicht, wie eine Initiative zur Behandlung der Tags `costCenter` und `productName` erstellt werden kann. Es werden zwei integrierte Richtlinien verwendet, um den Standardtagwert anzuwenden.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](../samples/index.md) finden Sie Beispiele
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md)
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Entdecken Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
