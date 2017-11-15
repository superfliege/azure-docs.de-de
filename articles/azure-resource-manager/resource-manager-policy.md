---
title: Azure-Ressourcenrichtlinien | Microsoft-Dokumentation
description: "Beschreibt, wie Sie mithilfe von Azure Resource Manager-Richtlinien sicherstellen, dass bei der Bereitstellung konsistente Ressourceneigenschaften festgelegt werden. Richtlinien können auf das Abonnement oder auf Ressourcengruppen angewendet werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: tomfitz
ms.openlocfilehash: 6faa8b4d259eddb1b26345d8900e829f6ca4dba8
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="resource-policy-overview"></a>Übersicht über Ressourcenrichtlinien
Mithilfe von Ressourcenrichtlinien können Sie Konventionen für Ressourcen in Ihrer Organisation einrichten. Durch Definieren von Konventionen können Sie Kosten beeinflussen und Ihre Ressourcen einfacher verwalten. Sie können beispielsweise angeben, dass nur bestimmte Typen virtueller Computer zulässig sind. Oder Sie können festlegen, dass alle Ressourcen ein bestimmtes Tag aufweisen. Richtlinien werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe.

Im Zusammenhang mit Richtlinien gibt es zwei wichtige Konzepte:

* Richtliniendefinition: Sie beschreiben, wann die Richtlinie erzwungen wird und welche Aktion ausgeführt werden soll.
* Richtlinienzuweisung: Sie wenden die Richtliniendefinition auf einen Bereich (Abonnement oder Ressourcengruppe) an.

Dieses Thema beschäftigt sich mit der Richtliniendefinition. Weitere Informationen zur Richtlinienzuweisung finden Sie unter [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md) und [Zuweisen und Verwalten von Richtlinien mit Skripts](resource-manager-policy-create-assign.md).

Richtlinien werden beim Erstellen und Aktualisieren von Ressourcen (PUT- und PATCH-Vorgänge) ausgewertet.

## <a name="how-is-it-different-from-rbac"></a>Worin unterscheidet sich dies von der rollenbasierten Zugriffssteuerung (RBAC)?
Zwischen einer Richtlinie und der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) gibt es einige entscheidende Unterschiede. Bei RBAC stehen **Benutzeraktionen** in verschiedenen Bereichen im Mittelpunkt. Beispiel: Sie werden der Rolle „Mitwirkender“ für eine Ressourcengruppe in einem bestimmten Bereich hinzugefügt, um Änderungen an der Ressourcengruppe vornehmen zu können. Bei einer Richtlinie stehen **Ressourceneigenschaften** während der Bereitstellung im Mittelpunkt. Sie können beispielsweise über Richtlinien steuern, welche Ressourcentypen bereitgestellt werden können. Oder Sie können die Standorte einschränken, an denen die Ressourcen bereitgestellt werden können. Im Gegensatz zur RBAC stellen Richtlinien ein Standardsystem für das Zulassen und explizite Verweigern dar. 

Zum Verwenden von Richtlinien müssen Sie über die RBAC authentifiziert sein. Ihr Konto benötigt insbesondere Folgendes:

* Die Berechtigung `Microsoft.Authorization/policydefinitions/write` zum Definieren einer Richtlinie
* Die Berechtigung `Microsoft.Authorization/policyassignments/write` zum Zuweisen einer Richtlinie 

Diese Berechtigungen sind in der Rolle **Mitwirkender** nicht enthalten.

## <a name="built-in-policies"></a>Integrierte Richtlinien

Azure stellt einige integrierte Richtliniendefinitionen bereit, um ggf. die Anzahl der zu definierenden Richtlinien zu verringern. Bevor Sie mit Richtliniendefinitionen fortfahren, sollten Sie erwägen, ob die benötigte Definition bereits durch eine integrierte Richtlinie vorgegeben wird. Integrierte Richtliniendefinitionen:

* Allowed locations (Zulässige Speicherorte)
* Zulässige Ressourcentypen
* Allowed storage account SKUs (Zulässige Speicherkonto-SKUs)
* Allowed virtual machine SKUs (Zulässige VM-SKUs)
* Apply tag and default value (Tag und Standardwert anwenden)
* Enforce tag and value (Tag und Wert erzwingen)
* Not allowed resource types (Unzulässige Ressourcentypen)
* Require SQL Server version 12.0 (SQL Server-Version 12.0 fordern)
* Require storage account encryption (Speicherkontoverschlüsselung fordern)

## <a name="policy-definition-structure"></a>Struktur von Richtliniendefinitionen
Eine Richtliniendefinition wird mithilfe von JSON erstellt. Die Richtliniendefinition enthält Elemente für Folgendes:

* Modus
* Parameter
* Anzeigename
* Beschreibung
* Richtlinienregel
  * Logische Auswertung
  * Wirkung

Das folgende Beispiel zeigt eine Richtlinie, die einschränkt, wo Ressourcen bereitgestellt werden:

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

## <a name="mode"></a>Mode

Es wird empfohlen, `mode` auf `all` zu setzen. Wenn Sie ihn auf **alle** setzen, werden Ressourcengruppen und alle Ressourcentypen für die Richtlinie ausgewertet. Das Portal verwendet **alle** für alle Richtlinien. Wenn Sie PowerShell oder Azure-CLI verwenden, müssen Sie den `mode`-Parameter angeben und auf **alle** setzen.
 
Zuvor wurden Richtlinien nur nach Ressourcentypen ausgewertet, die Tags und den Speicherort unterstützten. Im `indexed`-Modus wird dieses Verhalten fortgesetzt. Bei Verwendung des **alle**-Modus werden Richtlinien auch anhand von Ressourcentypen ausgewertet, die Tags und den Speicherort nicht unterstützen. [Virtual Network-Subnetz](https://github.com/Azure/azure-policy-samples/tree/master/samples/Network/enforce-nsg-on-subnet) ist ein Beispiel für einen neu hinzugefügten Typ. Darüber hinaus werden Ressourcengruppen ausgewertet, wenn der Modus auf **alle** festgelegt ist. Sie können z.B. [Tags für eine Ressourcengruppe erzwingen](https://github.com/Azure/azure-policy-samples/tree/master/samples/ResourceGroup/enforce-resourceGroup-tags). 

## <a name="parameters"></a>Parameter
Das Verwenden von Parametern vereinfacht die Richtlinienverwaltung, da die Anzahl von Richtliniendefinitionen reduziert wird. Sie definieren eine Richtlinie für eine Ressourceneigenschaft (beispielsweise, um die Orte einzuschränken, an denen Ressourcen bereitgestellt werden können) und schließen Parameter in die Definition ein. Diese Richtliniendefinition verwenden Sie dann auch für andere Szenarien, indem Sie beim Zuweisen der Richtlinie andere Werte (etwa eine Gruppe von Standorten für ein Abonnement) übergeben.

Sie deklarieren die Parameter beim Erstellen von Richtliniendefinitionen.

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

**displayName** und **description** dienen zum Identifizieren der Richtliniendefinition und geben den Kontext für die Verwendung an.

## <a name="policy-rule"></a>Richtlinienregel

Die Richtlinienregel besteht aus **If**- und **Then**-Blöcken. Im **If**-Block definieren Sie mindestens eine Bedingung, die angibt, wann die Richtlinie erzwungen wird. Auf diese Bedingungen können logische Operatoren angewendet werden, um das Szenario für eine Richtlinie präzise zu definieren. Im **Then**-Block definieren Sie die Wirkung, die eintritt, wenn die **If**-Bedingungen erfüllt sind.

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
Die Bedingung überprüft, ob ein **Feld** bestimmte Kriterien erfüllt. Folgende Bedingungen werden unterstützt:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Bei Verwendung der Bedingung **like** können Sie im Wert einen Platzhalter (*) angeben.

Geben Sie bei Verwendung der **match**-Bedingung für eine Ziffer `#`, für einen Buchstaben `?` und für ein Zeichen das gewünschte Zeichen ein. Beispiele finden Sie unter [Anwenden von Ressourcenrichtlinien für Namen und Text](resource-manager-policy-naming-convention.md).

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
Die Richtlinie unterstützt fünf Arten von Effekten: `deny`, `audit`, `append`, `AuditIfNotExists` und `DeployIfNotExists`. 

* **Deny** generiert ein Ereignis im Überwachungsprotokoll, und die Anforderung ist nicht erfolgreich.
* **Audit** generiert eine Warnung im Überwachungsprotokoll, und die Anforderung ist erfolgreich.
* **Append** fügt der Anforderung den definierten Satz von Feldern hinzu. 
* **AuditIfNotExists**: aktiviert die Überwachung, wenn eine Ressource nicht vorhanden ist.
* **DeployIfNotExists**: stellt eine Ressource bereit, wenn sie nicht bereits vorhanden ist. Dieser Effekt wird derzeit nur über integrierte Richtlinien unterstützt.

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

Mit **AuditIfNotExists** und **DeployIfNotExists** können Sie das Vorhandensein einer untergeordneten Ressource auswerten und eine Regel anwenden, wenn diese Ressource nicht vorhanden ist. Sie können z.B. erforderlich machen, dass ein Network Watcher für alle virtuellen Netzwerke bereitgestellt wird.

Ein Beispiel für die Überwachung, wenn keine VM-Erweiterung bereitgestellt wird, finden Sie unter [Audit VM extensions (Überwachung-VM-Erweiterungen)](https://github.com/Azure/azure-policy-samples/blob/master/samples/Compute/audit-vm-extension/azurepolicy.json).

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

## <a name="policy-sets"></a>Richtliniensätze

Richtliniensätze ermöglichen es Ihnen, mehrere verwandte Richtliniendefinitionen zu gruppieren. Der Richtliniensatz vereinfacht die Zuweisung und Verwaltung, da Sie mit der Gruppe als ein einzelnes Element arbeiten. Beispielsweise können Sie alle verknüpften Tagging-Richtlinien in einem einzelnen Richtliniensatz gruppieren. Statt jede Richtlinie einzeln zuzuweisen, wenden Sie den Richtliniensatz an.
 
Im folgenden Beispiel wird veranschaulicht, wie ein Richtliniensatz zur Behandlung von zwei Tags („costCenter“ und „productName“) erstellt werden kann. Es werden zwei integrierte Richtlinien für das Anwenden des Standardwerts für den Tag und die Durchsetzung des Tagwerts verwendet. Die Richtlinie deklariert zwei Parameter, „costCenterValue“ und „productNameValue“ für Wiederverwendbarkeit. Es wird mehrmals auf die beiden integrierten Richtliniendefinitionen mit verschiedenen Parametern verwiesen. Für jeden Parameter können Sie entweder einen festen Wert bereitstellen, wie für „tagName“ gezeigt, oder einen Parameter aus dem Richtliniensatz, wie für „tagValue“ gezeigt.

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

Sie fügen einen Richtliniensatz mit dem PowerShell-Befehl **New-AzureRMPolicySetDefinition** hinzu.

Für REST-Vorgänge verwenden Sie die API-Version **2017-06-01-preview**, wie im folgenden Beispiel gezeigt:

```
PUT /subscriptions/<subId>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicySet?api-version=2017-06-01-preview
```

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel weisen Sie sie einem Bereich zu. Wenn Sie Richtlinien über das Portal zuweisen möchten, siehe [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md). Wenn Sie Richtlinien über die REST-API, PowerShell oder die Azure-CLI zuweisen möchten, siehe [Zuweisen und Verwalten von Richtlinien mit Skripts](resource-manager-policy-create-assign.md).
* Beispiele für Richtlinien finden Sie unter [Azure resource policy GitHub repository (Azure Ressourcenrichtlinien-GitHub-Repository)](https://github.com/Azure/azure-policy-samples).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
* Das Richtlinienschema wird unter [http://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](http://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json) veröffentlicht. 

