---
title: Struktur von Azure Policy-Definitionen | Microsoft-Dokumentation
description: "Beschreibt, wie die Definition von Ressourcenrichtlinien von Azure Policy verwendet wird, um Konventionen für Ressourcen in Ihrer Organisation einzurichten, indem beschrieben wird, wann die Richtlinie erzwungen wird, und welche Aktionen durchzuführen sind."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/31/2017
ms.topic: article
ms.service: azure-policy
ms.custom: 
ms.openlocfilehash: 8ff85f842356eff3f12ccd04e337d71c52d0efcd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="azure-policy-definition-structure"></a>Struktur von Azure Policy-Definitionen

Von Azure Policy verwendete Definition von Ressourcenrichtlinien ermöglichen es Ihnen, Konventionen für Ressourcen in Ihrer Organisation einzurichten, indem Sie beschreiben, wann die Richtlinie erzwungen werden soll, und welche Aktionen durchzuführen sind. Durch Definieren von Konventionen können Sie Kosten beeinflussen und Ihre Ressourcen einfacher verwalten. Sie können beispielsweise angeben, dass nur bestimmte Typen virtueller Computer zulässig sind. Oder Sie können festlegen, dass alle Ressourcen ein bestimmtes Tag aufweisen. Richtlinien werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe.

Eine Richtliniendefinition wird mithilfe von JSON erstellt. Die Richtliniendefinition enthält Elemente für Folgendes:

* Modus
* Parameter
* Anzeigename
* Beschreibung
* Richtlinienregel
  * Logische Auswertung
  * Wirkung

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

Zusätzliche Vorlagenbeispiele für Azure Policy finden Sie unter [Templates for Azure Policy (Vorlagen für Azure Policy)](json-samples.md).

## <a name="mode"></a>Mode

Es wird empfohlen, dass Sie `mode` auf `all` festlegen, damit eine Richtlinienzuweisung sämtliche Ressourcengruppen und -typen überprüft. Ihnen wird unter [Allow custom VM image from a Resource Group (Zulassen eines benutzerdefinierten VM-Images aus einer Ressourcengruppe)](scripts/allow-custom-vm-image.md) ein Beispiel für eine Richtliniendefinition angezeigt, die Tags für Ressourcengruppen erzwingen.

Wenn Sie ihn auf **alle** setzen, werden Ressourcengruppen und alle Ressourcentypen für die Richtlinie ausgewertet. Das Portal verwendet **alle** für alle Richtlinien. Wenn Sie PowerShell oder Azure-CLI verwenden, müssen Sie den `mode`-Parameter angeben und auf **alle** setzen.

Sämtliche Richtliniendefinitionen, die über das Portal erstellt werden, verwenden einen `all`-Modus. Wenn Sie allerdings PowerShell oder Azure CLI verwenden möchten, müssen Sie den `mode`-Parameter angeben und auf `all` festlegen.

Wenn Sie den Modus auf `indexed` festlegen, wird die Richtlinienzuweisung nur auf Ressourcentypen geprüft, die Tags und den Standort unterstützen.


## <a name="parameters"></a>Parameter

Parameter vereinfachen Ihre Richtlinienverwaltung, indem sie die Anzahl von Richtliniendefinitionen reduzieren. Es handelt sich dabei z.B. um Parameter wie die folgenden Felder auf einem Formular: `name`, `address`, `city`, `state`. Diese Parameter bleiben immer gleich, allerdings ändern sich ihre Werte auf Grundlage der Einträge des Einzelnen. Parameter funktionieren beim Erstellen von Richtlinien genauso. Sie können die Richtlinie für verschiedene Szenarios wiederverwenden, indem Sie Parameter in eine Richtliniendefinition einbeziehen und verschiedene Werte verwenden.

Beispielsweise könnten Sie eine Richtlinie für eine Ressourceneigenschaft verwenden, um die Standorte, an denen Ressourcen bereitgestellt werden können, einzuschränken. In diesem Fall sollten Sie die folgenden Parameter deklarieren, wenn Sie eine Richtlinie erstellen:


```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

Der Typ eines Parameters kann Zeichenfolge oder Array sein. Die Metadateneigenschaft wird für Tools wie das Azure-Portal verwendet, um benutzerfreundliche Informationen anzuzeigen.

In der Richtlinienregel wird die folgende Syntax verwendet, um auf Parameter zu verweisen:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

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
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logische Operatoren

Folgende logische Operatoren werden unterstützt:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

Die **not**-Syntax kehrt das Ergebnis der Bedingung um. Die **allOf**-Syntax gleicht der logischen **And**-Operation und erfordert, dass alle Bedingungen erfüllt sind. Die **anyOf**-Syntax gleicht der logischen **Or**-Operation und erfordert, dass mindestens eine Bedingung erfüllt ist.

Logische Operatoren können geschachtelt werden. Das folgende Beispiel zeigt eine **not**-Operation, die in einer **allOf**-Operation geschachtelt ist.

```json
"if": {
  "allOf": [
    {
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

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Bei Verwendung der Bedingung **like** können Sie im Wert einen Platzhalter (*) angeben.

Geben Sie bei Verwendung der **match**-Bedingung für eine Ziffer `#`, für einen Buchstaben `?` und für ein Zeichen das gewünschte Zeichen ein. Beispiele finden Sie unter [Approved VM images (Genehmigte VM-Images)](scripts/allowed-custom-images.md).

### <a name="fields"></a>Felder
Bedingungen werden mithilfe von Feldern gebildet. Ein Feld stellt Eigenschaften in der Anforderungsnutzlast einer Ressource dar, mit der der Zustand der Ressource beschrieben wird.  

Folgende Felder werden unterstützt:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*`
* Eigenschaftenaliase – Eine Liste finden Sie unter [Aliase](#aliases).

### <a name="effect"></a>Effekt
Die Richtlinie unterstützt die folgenden Arten von Effekten:

* **Deny** generiert ein Ereignis im Überwachungsprotokoll und lässt die Anforderung fehlschlagen.
* **Audit** generiert eine Warnung im Überwachungsprotokoll, lässt die Anforderung aber nicht fehlschlagen.
* **Append** fügt der Anforderung verschiedene definierte Felder hinzu.
* **AuditIfNotExists** aktiviert das Überwachen, wenn eine Ressource nicht vorhanden ist.
* **DeployIfNotExists** stellt eine Ressource bereit, falls noch keine vorhanden ist. Dieser Effekt wird derzeit nur über integrierte Richtlinien unterstützt.
* **DenyIfNotExists** verweigert das Erstellen einer exist-Bedingung, wenn diese nicht vorhanden ist.

Für **append**müssen Sie die folgenden Details angeben:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Der Wert kann entweder eine Zeichenfolge oder ein Objekt im JSON-Format sein.

Mit **AuditIfNotExists**, **DeployIfNotExists** und **DenyIfNotExists** können Sie das Vorhandensein einer untergeordneten Ressource auswerten und eine Regel anwenden, wenn diese Ressource nicht vorhanden ist. Sie können z.B. verlangen, dass ein Network Watcher für alle virtuellen Netzwerke bereitgestellt wird.
Ein Beispiel für das Überwachen, wenn keine VM-Erweiterung bereitgestellt wird, finden Sie unter [Audit if extension does not exist (Überwachen bei nicht vorhandener Erweiterung)](scripts/audit-ext-not-exist.md).


## <a name="aliases"></a>Aliase

Eigenschaftenaliase dienen zum Zugreifen auf bestimmte Eigenschaften für einen Ressourcentyp. Mithilfe von Aliasen können Sie beschränken, welche Werte oder Bedingungen für eine Eigenschaft einer Ressourcen zulässig sind. Jeder Alias wird Pfaden in verschiedenen API-Versionen für einen bestimmten Ressourcentyp zugeordnet. Bei der Richtlinienauswertung ruft das Richtlinienmodul den Eigenschaftenpfad für diese API-Version ab.

**Microsoft.Cache/Redis**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Hiermit wird festgelegt, ob der Nicht-SSL-Redis-Serverport (6379) aktiviert ist. |
| Microsoft.Cache/Redis/shardCount | Hiermit wird die Anzahl der zu erstellenden Shards auf einem Premium-Clustercache festgelegt.  |
| Microsoft.Cache/Redis/sku.capacity | Hiermit wird die Größe des bereitzustellenden Redis-Caches festgelegt.  |
| Microsoft.Cache/Redis/sku.family | Hiermit wird die zu verwendende SKU-Familie festgelegt. |
| Microsoft.Cache/Redis/sku.name | Hiermit wird der Typ des bereitzustellenden Redis-Caches festgelegt. |

**Microsoft.Cdn/profiles**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Hiermit wird der Name des Tarifs festgelegt. |

**Microsoft.Compute/disks**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Hiermit wird das Angebot des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imagePublisher | Hiermit wird der Herausgeber des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imageSku | Hiermit wird die SKU des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imageVersion | Hiermit wird die Version des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |


**Microsoft.Compute/virtualMachines**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Compute/imageId | Legen Sie den Bezeichner des Images fest, das zum Erstellen des virtuellen Computers verwendet wurde. |
| Microsoft.Compute/imageOffer | Hiermit wird das Angebot des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imagePublisher | Hiermit wird der Herausgeber des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imageSku | Hiermit wird die SKU des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imageVersion | Hiermit wird die Version des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/licenseType | Hiermit wird festgelegt, dass das Image oder der Datenträger lokal lizenziert ist. Dieser Wert wird nur für Images mit dem Betriebssystem Windows Server verwendet.  |
| Microsoft.Compute/virtualMachines/imageOffer | Hiermit wird das Angebot des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/virtualMachines/imagePublisher | Hiermit wird der Herausgeber des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/virtualMachines/imageSku | Hiermit wird die SKU des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/virtualMachines/imageVersion | Hiermit wird die Version des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Hiermit wird der VHD-URI festgelegt. |
| Microsoft.Compute/virtualMachines/sku.name | Legen Sie die Größe des virtuellen Computers fest. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Hiermit wird der Name des Herausgebers der Erweiterung festgelegt. |
| Microsoft.Compute/virtualMachines/extensions/type | Hiermit wird der Typ der Erweiterung festgelegt. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Hiermit wird die Version der Erweiterung festgelegt. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Compute/imageId | Legen Sie den Bezeichner des Images fest, das zum Erstellen des virtuellen Computers verwendet wurde. |
| Microsoft.Compute/imageOffer | Hiermit wird das Angebot des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imagePublisher | Hiermit wird der Herausgeber des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imageSku | Hiermit wird die SKU des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/imageVersion | Hiermit wird die Version des Plattformimage oder des Marketplace-Image festgelegt, mit dem die VM erstellt wird. |
| Microsoft.Compute/licenseType | Hiermit wird festgelegt, dass das Image oder der Datenträger lokal lizenziert ist. Dieser Wert wird nur für Images mit dem Betriebssystem Windows Server verwendet. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Hiermit wird das Computernamenspräfix für alle virtuellen Computer in der Skalierungsgruppe festgelegt. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Hiermit wird der Blob-URI für das Benutzerimage festgelegt. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Hiermit werden die Container-URLs zum Speichern von Betriebssystemdatenträgern für die Skalierungsgruppe festgelegt. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Hiermit wird die Größe der virtuellen Computer in einer Skalierungsgruppe festgelegt. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Hiermit wird die Ebene der virtuellen Computer in einer Skalierungsgruppe festgelegt. |

**Microsoft.Network/applicationGateways**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Hiermit wird die Größe des Gateways festgelegt. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Hiermit wird der Typ des virtuellen Netzwerkgateways festgelegt. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Hiermit wird der Name der Gateway-SKU festgelegt. |

**Microsoft.Sql/servers**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Hiermit wird die Version des Servers festgelegt. |

**Microsoft.Sql/databases**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Hiermit wird die Edition der Datenbank festgelegt. |
| Microsoft.Sql/servers/databases/elasticPoolName | Hiermit wird der Name des Pools für elastische Datenbanken festgelegt, in dem die Datenbank enthalten ist. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Hiermit wird die ID des konfigurierten SLO der Datenbank festgelegt. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Hiermit wird der Name des konfigurierten SLO der Datenbank festgelegt.  |

**Microsoft.Sql/elasticpools**

| Alias | Beschreibung |
| ----- | ----------- |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Hiermit wird die freigegebene Gesamt-DTU für den Pool für elastische Datenbanken festgelegt. |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Hiermit wird die Edition des Pools für elastische Datenbanken festgelegt. |

**Microsoft.Storage/storageAccounts**

| Alias | Beschreibung |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Hiermit wird die Zugriffsebene für die Abrechnung festgelegt. |
| Microsoft.Storage/storageAccounts/accountType | Hiermit wird der SKU-Name festgelegt. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Hiermit wird festgelegt, ob der Dienst die Daten beim Speichern im Blob-Speicherdienst verschlüsselt. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Hiermit wird festgelegt, ob der Dienst die Daten beim Speichern im Dateispeicherdienst verschlüsselt. |
| Microsoft.Storage/storageAccounts/sku.name | Hiermit wird der SKU-Name festgelegt. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Hiermit wird festgelegt, dass nur HTTPS-Datenverkehr zum Speicherdienst zugelassen wird. |

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
        "policyDefinitions": [
            {
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

- Zusätzliche Vorlagenbeispiele für Azure Policy finden Sie unter [Templates for Azure Policy (Vorlagen für Azure Policy)](json-samples.md).
