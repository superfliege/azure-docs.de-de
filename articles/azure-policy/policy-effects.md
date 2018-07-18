---
title: Grundlegendes zu Azure Policy-Auswirkungen
description: Die Azure Policy-Definition hat verschiedene Auswirkungen, mit denen festgelegt wird, wie die Konformität verwaltet und gemeldet wird.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 1566cf2b61749121c4eaff5a32b0a940f3341f7e
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751777"
---
# <a name="understanding-policy-effects"></a>Grundlegendes zu Richtlinienauswirkungen

Jede Richtliniendefinition in Azure Policy hat eine einzelne Auswirkung. Diese bestimmt, was beim Überprüfen geschieht, wenn das **if**-Segment der Richtlinienregel für die überprüfte Ressource ausgewertet wird. Die Auswirkungen für eine neue Ressource, eine aktualisierte Ressource oder eine vorhandene Ressource können hierbei unterschiedlich sein.

Aktuell werden in einer Richtliniendefinition fünf Auswirkungen unterstützt:

- Anfügen
- Audit
- Auswirkung „AuditIfNotExists“
- Verweigern
- Auswirkung „DeployIfNotExists“

## <a name="order-of-evaluation"></a>Reihenfolge der Auswertung

Wenn über den Azure Resource Manager eine Anforderung zum Erstellen oder Aktualisieren einer Ressource gesendet wird, verarbeitet Azure Policy verschiedene Auswirkungen, bevor die Anforderung an den geeigneten Ressourcenanbieter übergeben wird.
Auf diese Weise wird eine unnötige Verarbeitung durch einen Ressourcenanbieter verhindert, wenn eine Ressource nicht den konfigurierten Governancevorgaben von Azure Policy entspricht. Azure Policy erstellt eine Liste aller – per Richtlinie oder Initiative – zugewiesenen Richtliniendefinitionen, die gemäß Bereich (abzüglich der Ausnahmen) für die Ressource gelten, und bereitet die Auswertung der Ressource anhand jeder Definition vor.

- Zuerst wird **append** ausgewertet. Die Anforderung kann durch „append“ geändert werden, deshalb kann eine über „append“ durchgeführte Änderung die Auslösung der Auswirkungen „audit“ oder „deny“ verhindern.
- Anschließend wird **deny** ausgewertet. Durch die Auswertung von „deny“ vor „audit“ wird eine zweimalige Protokollierung einer unerwünschten Ressource verhindert.
- Anschließend wird **audit** ausgewertet, bevor die Anforderung an den Ressourcenanbieter weitergeleitet wird.

Sobald die Anforderung an den Ressourcenanbieter übergeben wurde und der Ressourcenanbieter einen Erfolgsstatuscode zurückgibt, werden **AuditIfNotExists** und **DeployIfNotExists** ausgewertet, um zu bestimmen, ob eine anschließende Konformitätsprotokollierung oder -aktion erforderlich ist.

## <a name="append"></a>Anfügen

„append“ wird verwendet, um der angeforderten Ressource während der Erstellung oder Aktualisierung zusätzliche Felder hinzuzufügen. Dies kann nützlich sein, um Ressourcen Tags wie z.B. „costCenter“ hinzuzufügen oder zugelassene IP-Adressen für eine Speicherressource anzugeben.

### <a name="append-evaluation"></a>Auswertung von „append“

Wie bereits erwähnt, wird „append“ vor der Anforderungsverarbeitung durch einen Ressourcenanbieter während der Erstellung oder Aktualisierung einer Ressource ausgewertet. Mit „append“ werden Felder zur Ressource hinzugefügt, wenn die **if**-Bedingung der Richtlinienregel erfüllt ist. Wenn die Auswirkung „append“ einen Wert in der ursprünglichen Anforderung mit einem anderen Wert überschreiben würde, fungiert sie als Auswirkung „deny“ und lehnt die Anforderung ab.

Wenn eine Richtliniendefinition mit Auswirkung „append“ im Rahmen eines Auswertungszyklus ausgeführt wird, werden keine Änderungen an bereits vorhandenen Ressourcen durchgeführt. Stattdessen werden alle Ressourcen mit Erfüllung der **if**-Bedingung als nicht konform markiert.

### <a name="append-properties"></a>Eigenschaften von „append“

Die Auswirkung „append“ weist nur ein Array **details** auf, das erforderlich ist. Da es sich bei **details** um ein Array handelt, kann es entweder ein einzelnes **field/value**-Paar oder mehrere Wertepaare verwenden. Eine Liste der zulässigen Felder finden Sie in der [Richtliniendefinition](policy-definition.md#fields).

### <a name="append-examples"></a>Beispiele für „append“

Beispiel 1: Einzelnes **field/value**-Paar zum Anfügen eines Tags.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Beispiel 2: Mehrere **field/value**-Paare zum Anfügen einer Gruppe von Tags.

```json
"then": {
    "effect": "append",
    "details": [{
            "field": "tags.myTag",
            "value": "myTagValue"
        },
        {
            "field": "tags.myOtherTag",
            "value": "myOtherTagValue"
        }
    ]
}
```

Beispiel 3: Einzelnes **field/value**-Paar mit einem [alias](policy-definition.md#aliases), in dem das **value**-Array IP-Regeln für ein Speicherkonto festlegt.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

## <a name="deny"></a>Verweigern

Mit „deny“ werden Ressourcenanforderungen abgelehnt, welche die über eine Richtliniendefinition festgelegten gewünschten Standards nicht erfüllen. Für die Anforderung wird anschließend ein Fehler ausgegeben.

### <a name="deny-evaluation"></a>Auswertung von „deny“

Beim Erstellen oder Aktualisieren einer Ressourcen verhindert „deny“, dass die Anforderung an den Ressourcenanbieter gesendet wird. Für die Anforderung wird ein 403-Fehler (Verboten) zurückgegeben. Im Portal kann „Verboten“ als Status für die Bereitstellung angezeigt werden, die aufgrund der Richtlinienzuweisung verhindert wurde.

Während eines Auswertungszyklus führen Richtliniendefinitionen mit der Auswirkung „deny“ dazu, dass übereinstimmende Ressourcen als nicht konform markiert werden. Es wird aber keine Aktion für diese Ressource ausgeführt.

### <a name="deny-properties"></a>Eigenschaften von „deny“

Die Auswirkung „deny“ umfasst keine zusätzlichen Eigenschaften zur Verwendung in der **then**-Bedingung der Richtliniendefinition.

### <a name="deny-example"></a>Beispiel für „deny“

Beispiel zur Verwendung der Auswirkung „deny“

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Audit

Die Auswirkung „audit“ wird verwendet, um ein Warnungsereignis im Überwachungsprotokoll zu erstellen, wenn eine nicht konforme Ressource ausgewertet wird. Die Anforderung wird jedoch nicht beendet.

### <a name="audit-evaluation"></a>Auswertung von „audit“

Die Auswirkung „audit“ wird beim Erstellen oder Aktualisieren einer Ressource zuletzt ausgeführt, bevor die Anforderung an den Ressourcenanbieter gesendet wird. „audit“ funktioniert gleichermaßen für eine Ressourcenanforderung innerhalb eines Auswertungszyklus und führt einen `Microsoft.Authorization/policies/audit/action`-Vorgang im Aktivitätsprotokoll durch. In beiden Fällen wird die Ressource als nicht konform markiert.

### <a name="audit-properties"></a>Eigenschaften von „audit“

Die Auswirkung „audit“ umfasst keine zusätzlichen Eigenschaften zur Verwendung in der **then**-Bedingung der Richtliniendefinition.

### <a name="audit-example"></a>Beispiel für „audit“

Beispiel zur Verwendung der Auswirkung „audit“

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>Auswirkung „AuditIfNotExists“

Die Auswirkung „AuditIfNotExists“ ermöglicht das Überwachen einer Ressource, welche die **if**-Bedingung erfüllt, für die aber keine Komponenten in den **details** der **then** -Bedingung angegeben sind.

### <a name="auditifnotexists-evaluation"></a>Auswertung von „AuditIfNotExists“

„AuditIfNotExists“ wird ausgeführt, nachdem ein Ressourcenanbieter eine Anforderung zum Erstellen oder Aktualisieren einer Ressource verarbeitet hat und ein Erfolgsstatuscode zurückgegeben wurde. Die Auswirkung wird ausgelöst, wenn keine entsprechenden Ressourcen vorhanden sind oder wenn die über **ExistenceCondition** definierten Ressourcen nicht als TRUE ausgewertet werden. Wenn die Auswirkung ausgelöst wird, wird ebenso wie bei der Auswirkung „audit“ ein `Microsoft.Authorization/policies/audit/action`-Vorgang für das Aktivitätsprotokoll ausgeführt. Bei Auslösung ist die Ressource, die die **if**-Bedingung erfüllt hat, die Ressource, die als nicht konform markiert wird.

### <a name="auditifnotexists-properties"></a>Eigenschaften von „AuditIfNotExists“

Die **details**-Eigenschaft der Auswirkung „AuditIfNotExists“ umfasst die folgenden Untereigenschaften zur Definition der entsprechenden Ressourcen für den Abgleich.

- **Type** [erforderlich]
  - Gibt den Typ der entsprechenden abzugleichenden Ressource an.
  - Zunächst wird versucht, eine Ressource unterhalb der **if**-Bedingungsressource abzurufen, anschließend wird eine Abfrage innerhalb derselben Ressourcengruppe wie der **if**-Bedingungsressource durchgeführt.
- **Name** (optional)
  - Gibt den exakten Namen der Ressource für den Abgleich an und führt dazu, dass die Richtlinie nicht alle Ressourcen des angegebenen Typs, sondern eine bestimmte Ressource abruft.
- **ResourceGroupName** (optional)
  - Hierdurch ist es möglich, für den Abgleich eine Ressource aus einer anderen Ressourcengruppe festzulegen.
  - Diese Einstellung ist nicht anwendbar, wenn **type** eine Ressource unterhalb der **if**-Bedingungsressource angibt.
  - Standardmäßig wird die Ressourcengruppe der **if**-Bedingungsressource verwendet.
- **ExistenceScope** (optional)
  - Zulässige Werte sind _Subscription_ und _ResourceGroup_.
  - Legt den Bereich fest, aus dem die entsprechende Ressource für den Abgleich abgerufen werden soll.
  - Diese Einstellung ist nicht anwendbar, wenn **type** eine Ressource unterhalb der **if**-Bedingungsressource angibt.
  - Für _ResourceGroup_ bedeutet dies eine Beschränkung auf die Ressourcengruppe der **if** -Bedingungsressource oder die in **ResourceGroupName** angegebene Ressourcengruppe.
  - Für _Subscription_ wird das gesamte Abonnement nach der entsprechenden Ressource abgefragt.
  - Die Standardeinstellung ist _ResourceGroup_.
- **ExistenceCondition** (optional)
  - Sofern nicht angegeben, erfüllt jede Ressource mit entsprechendem **type** die Bedingung der Auswirkung und löst keine Überwachung aus.
  - Verwendet dieselbe Sprachsyntax wie die Richtlinienregel für die **if**-Bedingung, wird jedoch für jede entsprechende Ressource einzeln ausgeführt.
  - Wenn eine übereinstimmende Ressource als TRUE ausgewertet wird, ist die Auswirkung erfüllt und löst keine Überwachung aus.
  - [field()] kann verwendet werden, um auf Äquivalenz mit Werten in der **if**-Bedingung zu überprüfen.
  - Beispielsweise könnte so überprüft werden, ob die übergeordnete Ressource (in der **if**-Bedingung) sich am selben Ressourcenstandort befindet wie die übereinstimmende Ressource.

### <a name="auditifnotexists-example"></a>Beispiel für „AuditIfNotExists“

Beispiel: Mithilfe einer Auswertung wird ermittelt, ob die Antischadsoftware-Erweiterung auf virtuellen Computern vorhanden ist. Fehlt die Erweiterung, wird eine Überwachung ausgelöst.

```json
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "auditIfNotExists",
        "details": {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "existenceCondition": {
                "allOf": [{
                        "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                        "equals": "Microsoft.Azure.Security"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/extensions/type",
                        "equals": "IaaSAntimalware"
                    }
                ]
            }
        }
    }
}
```

## <a name="deployifnotexists"></a>Auswirkung „DeployIfNotExists“

Ähnlich wie „AuditIfNotExists“ führt „DeployIfNotExists“ eine Vorlagenbereitstellung durch, wenn die Bedingung erfüllt ist.

### <a name="deployifnotexists-evaluation"></a>Auswertung von „DeployIfNotExists“

„DeployIfNotExists“ wird ausgeführt, nachdem ein Ressourcenanbieter eine Anforderung zum Erstellen oder Aktualisieren einer Ressource verarbeitet hat und ein Erfolgsstatuscode zurückgegeben wurde. Die Auswirkung wird ausgelöst, wenn keine entsprechenden Ressourcen vorhanden sind oder wenn die über **ExistenceCondition** definierten Ressourcen nicht als TRUE ausgewertet werden. Bei Auslösung der Auswirkung wird eine Vorlagenbereitstellung durchgeführt.

Während eines Auswertungszyklus führen Richtliniendefinitionen mit der Auswirkung „DeployIfNotExists“ dazu, dass übereinstimmende Ressourcen als nicht konform markiert werden. Es wird aber keine Aktion für diese Ressource ausgeführt.

### <a name="deployifnotexists-properties"></a>Eigenschaften von „DeployIfNotExists“

Die **details**-Eigenschaft der Auswirkung „AuditIfNotExists“ umfasst die folgenden Untereigenschaften zur Definition der entsprechenden Ressourcen für den Abgleich.

- **Type** [erforderlich]
  - Gibt den Typ der entsprechenden abzugleichenden Ressource an.
  - Zunächst wird versucht, eine Ressource unterhalb der **if**-Bedingungsressource abzurufen, anschließend wird eine Abfrage innerhalb derselben Ressourcengruppe wie der **if**-Bedingungsressource durchgeführt.
- **Name** (optional)
  - Gibt den exakten Namen der Ressource für den Abgleich an und führt dazu, dass die Richtlinie nicht alle Ressourcen des angegebenen Typs, sondern eine bestimmte Ressource abruft.
- **ResourceGroupName** (optional)
  - Hierdurch ist es möglich, für den Abgleich eine Ressource aus einer anderen Ressourcengruppe festzulegen.
  - Diese Einstellung ist nicht anwendbar, wenn **type** eine Ressource unterhalb der **if**-Bedingungsressource angibt.
  - Standardmäßig wird die Ressourcengruppe der **if**-Bedingungsressource verwendet.
  - Wenn eine Vorlagenbereitstellung durchgeführt wird, erfolgt diese in der Ressourcengruppe, die über diesen Wert angegeben wurde.
- **ExistenceScope** (optional)
  - Zulässige Werte sind _Subscription_ und _ResourceGroup_.
  - Legt den Bereich fest, aus dem die entsprechende Ressource für den Abgleich abgerufen werden soll.
  - Diese Einstellung ist nicht anwendbar, wenn **type** eine Ressource unterhalb der **if**-Bedingungsressource angibt.
  - Für _ResourceGroup_ bedeutet dies eine Beschränkung auf die Ressourcengruppe der **if** -Bedingungsressource oder die in **ResourceGroupName** angegebene Ressourcengruppe.
  - Für _Subscription_ wird das gesamte Abonnement nach der entsprechenden Ressource abgefragt.
  - Die Standardeinstellung ist _ResourceGroup_.
- **ExistenceCondition** (optional)
  - Sofern nicht angegeben, erfüllt jede entsprechende Ressource mit **type** die Bedingung der Auswirkung und löst keine Überwachung aus.
  - Verwendet dieselbe Sprachsyntax wie die Richtlinienregel für die **if**-Bedingung, wird jedoch für jede entsprechende Ressource einzeln ausgeführt.
  - Wenn eine übereinstimmende Ressource als TRUE ausgewertet wird, ist die Auswirkung erfüllt und löst keine Bereitstellung aus.
  - [field()] kann verwendet werden, um auf Äquivalenz mit Werten in der **if**-Bedingung zu überprüfen.
  - Beispielsweise könnte so überprüft werden, ob die übergeordnete Ressource (in der **if**-Bedingung) sich am selben Ressourcenstandort befindet wie die übereinstimmende Ressource.
- **Deployment** [erforderlich]
  - Diese Eigenschaft muss die vollständige Vorlagenbereitstellung enthalten, so wie sie an die PUT-API `Microsoft.Resources/deployments` übergeben würde. Weitere Informationen finden Sie im Artikel zur [REST-API für die Bereitstellung](/rest/api/resources/deployments).

  > [!NOTE]
  > Alle Funktionen innerhalb der **Deployment**-Eigenschaft werden als Komponenten der Vorlage – nicht der Richtlinie – ausgewertet. Eine Ausnahme ist die Eigenschaft **parameters**, die Werte von der Richtlinie an die Vorlage übergibt. Der in diesem Abschnitt unter einem Vorlagenparameternamen angegebene Wert für **value** wird verwendet, um diese Wertübergabe durchzuführen (siehe _fullDbName_ im DeployIfNotExists-Beispiel).

### <a name="deployifnotexists-example"></a>Beispiel für „DeployIfNotExists“

Beispiel: Mithilfe einer Auswertung von SQL Server-Datenbanken wird bestimmt, ob „transparentDataEncryption“ aktiviert ist. Falls nicht, wird eine Bereitstellung zur Aktivierung dieser Option durchgeführt.

```json
"if": {
    "field": "type",
    "equals": "Microsoft.Sql/servers/databases"
},
"then": {
    "effect": "DeployIfNotExists",
    "details": {
        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
        "name": "current",
        "existenceCondition": {
            "field": "Microsoft.Sql/transparentDataEncryption.status",
            "equals": "Enabled"
        },
        "deployment": {
            "properties": {
                "mode": "incremental",
                "template": {
                    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "fullDbName": {
                            "type": "string"
                        }
                    },
                    "resources": [{
                        "name": "[concat(parameters('fullDbName'), '/current')]",
                        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
                        "apiVersion": "2014-04-01",
                        "properties": {
                            "status": "Enabled"
                        }
                    }]
                },
                "parameters": {
                    "fullDbName": {
                        "value": "[field('fullName')]"
                    }
                }
            }
        }
    }
}
```

## <a name="layering-policies"></a>Mehrere Richtlinien

Eine Ressource kann durch mehrere Zuweisungen beeinflusst werden. Diese Zuweisungen können für denselben Bereich (eine bestimmte Ressource, Ressourcengruppe, ein bestimmtes Abonnement oder eine Verwaltungsgruppe) oder verschiedene Bereiche gelten. Für jede dieser Zuweisungen ist wahrscheinlich auch eine andere Auswirkung definiert. Trotzdem werden die Bedingung und die Auswirkung für jede Richtlinie (direkt oder als Teil einer Initiative zugewiesen) unabhängig ausgewertet. Wenn beispielsweise Richtlinie 1 eine Bedingung enthält, durch die das Erstellen des Ressourcenspeicherorts für Abonnement A mittels Auswirkung „deny“ auf „USA, Westen“ beschränkt wird, und Richtlinie 2 eine Bedingung enthält, die das Erstellen des Ressourcenspeicherorts für Ressourcengruppe B (die sich im Abonnement A befindet) mittels Auswirkung „audit“ auf „USA, Osten“ beschränkt, würde die Zuweisung beider Richtlinien zu folgendem Ergebnis führen:

- Jede Ressource, die sich bereits in Ressourcengruppe B und am Standort „USA, Osten“ befindet, ist gemäß Richtlinie 2 konform, aber wird gemäß Richtlinie 1 als nicht konform markiert.
- Jede Ressource, die sich bereits in Ressourcengruppe B und nicht am Standort „USA, Osten“ befindet, wird gemäß Richtlinie 2 als nicht konform markiert. Die Ressource wird auch durch Richtlinie 1 als nicht konform markiert, wenn sie sich nicht am Standort „USA, Westen“ befindet.
- Jede neue Ressource in Abonnement A, die nicht am Standort „USA, Westen“ erstellt wird, wird von Richtlinie 1 abgelehnt.
- Jede neue Ressource in Abonnement A/Ressourcengruppe B am Standort „USA, Westen“ wird gemäß Richtlinie 2 als nicht konform markiert, aber erstellt (da sie gemäß Richtlinie 1 konform ist und die Auswirkung von Richtlinie 2 „audit“ lautet und nicht „deny“).

Wenn sowohl Richtlinie 1 als auch Richtlinie 2 die Auswirkung „deny“ verwenden würden, ändert sich die Situation folgendermaßen:

- Jede Ressource, die sich bereits in Ressourcengruppe B und nicht am Standort „USA, Osten“ befindet, wird gemäß Richtlinie 2 als nicht konform markiert.
- Jede Ressource, die sich bereits in Ressourcengruppe B und nicht am Standort „USA, Westen“ befindet, wird gemäß Richtlinie 1 als nicht konform markiert.
- Jede neue Ressource in Abonnement A, die nicht am Standort „USA, Westen“ erstellt wird, wird von Richtlinie 1 abgelehnt.
- Jede neue Ressource in Abonnement A/Ressourcengruppe B würde abgelehnt werden (da die Standortbedingung nie sowohl für Richtlinie 1 als auch für Richtlinie 2 erfüllt werden kann).

Weil jede Zuweisung einzeln ausgewertet wird, ist es nicht möglich, dass eine Ressource aufgrund unterschiedlicher Bereiche „durch das Netz schlüpfen“ kann. Die Anwendung mehrerer oder sich überschneidender Richtlinien führt also **kumulativ zur stärksten Einschränkung**. Anders ausgedrückt: Eine Ressource, die Sie erstellen möchten, wird möglicherweise durch sich überschneidende oder konfliktverursachende Richtlinien blockiert (wie im obigen Beispiel, wenn sowohl Richtlinie 1 als auch Richtlinie 2 die Auswirkung „deny“ verwenden). Wenn die Ressource dennoch im Zielbereich erstellt werden soll, überprüfen Sie die Ausnahmen für jede Zuweisung, um sicherzustellen, dass die richtigen Richtlinien auf die richtigen Bereiche angewendet werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über ein umfassenderes Verständnis der Auswirkungen von Richtliniendefinitionen verfügen, sehen Sie sich die folgenden Richtlinienbeispiele an:

- Unter [Azure Policy-Beispiele](json-samples.md) finden Sie weitere Beispiele.
