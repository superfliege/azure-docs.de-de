---
title: Ermitteln der Ursachen für Nichtkonformität
description: Wenn eine Ressource nicht konform ist, kann das viele mögliche Ursachen haben. Erfahren Sie, wie Sie die Ursache für die Nichtkonformität ermitteln können.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 8a593e92d7f24885c35043b874528e881d2e021e
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276098"
---
# <a name="determine-causes-of-non-compliance"></a>Ermitteln der Ursachen für Nichtkonformität

Wenn festgestellt wird, dass eine Azure-Ressource nicht konform mit einer Richtlinienregel ist, ist es wichtig zu bestimmen, gegen welchen Teil der Regel die Ressource verstößt. Außerdem ist es ebenso nützlich, ermitteln zu können, welche Änderung dazu geführt hat, dass eine ehemals konforme Ressource nun nicht mehr konform ist. Es gibt zwei Möglichkeiten, diese Informationen zu bestimmen:

> [!div class="checklist"]
> - [Konformitätsdetails](#compliance-details)
> - [Änderungsverlauf (Vorschau)](#change-history-preview)

## <a name="compliance-details"></a>Konformitätsdetails

Wenn eine Ressource nicht konform ist, können Sie über die Seite **Richtlinienkonformität** auf die Konformitätsdetails dieser Ressource zugreifen. Der Bereich mit den Kompatibilitätsdetails enthält die folgenden Informationen:

- Angaben zur Ressource wie der Name, der Typ, der Speicherort und die Ressourcen-ID
- Konformitätsstatus und Zeitstempel der letzten Evaluierung für die aktuelle Richtlinienzuweisung
- Eine Liste mit _Gründen_ für die Nichtkonformität der Ressource

> [!IMPORTANT]
> Die Konformitätsdetails einer _nicht konformen_ Ressource enthalten die aktuellen Werte der Eigenschaften der Ressource. Deshalb muss der **Benutzer über Leseberechtigungen** für den **Ressourcentyp** verfügen. Wenn die _nicht konforme_ Ressource z.B. vom Typ **Microsoft.Compute/virtualMachines** ist, muss der Leser über die Berechtigung **Microsoft.Compute/virtualMachines/read** verfügen. Wenn der Benutzer nicht über die erforderliche Berechtigung verfügt, wird ein Zugriffsfehler angezeigt.

Führen Sie die folgenden Schritte durch, um die Konformitätsdetails anzuzeigen:

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen.

1. Wählen Sie auf der Seite **Übersicht** oder **Konformität** eine Richtlinie mit dem **Konformitätsstatus** _Nicht konform_ aus.

1. Öffnen Sie auf der Seite **Richtlinienkonformität** auf der Registerkarte **Ressourcenkonformität** das Kontextmenü, oder klicken Sie auf die Auslassungspunkte neben einer Ressource mit dem **Konformitätsstatus** _Nicht konform_. Wählen Sie dann **Konformitätsdetails anzeigen** aus.

   ![Option „Konformitätsdetails anzeigen“](../media/determine-non-compliance/view-compliance-details.png)

1. Im Bereich **Konformitätsdetails** werden Informationen der letzten Auswertung der Ressource mit der aktuellen Richtlinienzuweisung angezeigt. In diesem Beispiel enthält das Feld **Microsoft.Sql/servers/version** _12.0_, und die Richtliniendefinition hat _14.0_ erwartet. Wenn es mehrere Gründen für die Nichtkonformität der Ressource gibt, werde diese alle in diesem Bereich aufgeführt.

   ![Bereich „Konformitätsdetails“ und Gründe für fehlende Konformität](../media/determine-non-compliance/compliance-details-pane.png)

   Für **auditIfNotExists**- oder **deployIfNotExists**-Richtliniendefinitionen enthalten die Details die Eigenschaft **details.type** und alle optionalen Eigenschaften. Eine Liste finden Sie unter [„Grundlegendes zu Azure Policy-Auswirkungen“ im Abschnitt „Eigenschaften von „auditIfNotExists“](../concepts/effects.md#auditifnotexists-properties) und unter [„Grundlegendes zu Azure Policy-Auswirkungen“ im Abschnitt „Eigenschaften von „DeployIfNotExists“](../concepts/effects.md#deployifnotexists-properties). **Zuletzt ausgewertete Ressource** ist eine verwandte Ressource aus dem **Detailabschnitt** der Definition.

   Hier sehen Sie einen Beispielausschnitt aus der Definition von **deployIfNotExists**:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Bereich „Konformitätsdetails“, *ifnotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> Der aktuelle Wert zeigt Asteriske an, wenn ein Eigenschaftswert ein _Geheimnis_ ist, um Daten zu schützen.

In den Details wird angegeben, warum eine Ressource nicht konform ist. Allerdings wird nicht angegeben, wann eine Änderung an der Ressource vorgenommen wurde, die dazu geführt hat, dass sie nicht mehr konform ist. Zu diesen Informationen erfahren Sie im Abschnitt [Änderungsverlauf (Vorschau)](#change-history-preview) mehr.

### <a name="compliance-reasons"></a>Konformitätsgründe

In der folgenden Tabelle wird jeder mögliche _Grund_ der entsprechenden [Bedingung](../concepts/definition-structure.md#conditions) in der Richtliniendefinition zugeordnet:

|Grund | Bedingung |
|-|-|
|Der aktuelle Wert muss den Zielwert als Schlüssel enthalten. |containsKey oder **nicht** notContainsKey |
|Der aktuelle Wert muss den Zielwert enthalten. |contains oder **nicht** notContains |
|Der aktuelle Wert muss gleich dem Zielwert sein. |equals der **nicht** notEquals |
|Der aktuelle Wert muss vorhanden sein. |exists |
|Der aktuelle Wert muss im Zielwert enthalten sein. |in der **nicht** notIn |
|Der aktuelle Wert muss dem Zielwert entsprechen. |like oder **nicht** notLike |
|Der aktuelle Wert muss mit dem Zielwert übereinstimmen (Berücksichtigung der Groß-/Kleinschreibung). |match oder **nicht** notMatch |
|Der aktuelle Wert muss mit dem Zielwert übereinstimmen (keine Berücksichtigung der Groß-/Kleinschreibung). |matchInsensitively oder **nicht** notMatchInsensitively |
|Der aktuelle Wert darf den Zielwert nicht als Schlüssel enthalten. |notContainsKeyontainsKey oder **nicht** ContainsKey|
|Der aktuelle Wert darf nicht den Zielwert enthalten. |notContains oder **nicht** contains |
|Der aktuelle Wert darf nicht gleich dem Zielwert sein. |notEquals der **nicht** equals |
|Der aktuelle Wert darf nicht vorhanden sein. |**nicht** exists  |
|Der aktuelle Wert darf nicht im Zielwert enthalten sein. |notIn oder **nicht** in |
|Der aktuelle Wert darf nicht dem Zielwert entsprechen. |notLike oder **nicht** like |
|Der aktuelle Wert darf nicht mit dem Zielwert übereinstimmen (Berücksichtigung der Groß-/Kleinschreibung). |notMatch oder **nicht** match |
|Der aktuelle Wert darf nicht mit dem Zielwert übereinstimmen (keine Berücksichtigung der Groß-/Kleinschreibung). |notMatchInsensitively oder **nicht** matchInsensitively |
|Keine der zugehörigen Ressourcen entspricht den Effektdetails in der Richtliniendefinition. |Eine Ressource des in **then.details.type** definierten Typs, die mit der im **if**-Abschnitt der Richtlinienregel definierten Ressource verwandt ist, ist nicht vorhanden. |

## <a name="change-history-preview"></a>Änderungsverlauf (Vorschau)

Im Rahmen einer neuen **öffentlichen Vorschau** sind die letzten 14 Tage des Änderungsverlaufs für alle Azure-Ressourcen verfügbar, die die [Löschung im vollständigen Modus](../../../azure-resource-manager/complete-mode-deletion.md) unterstützen. Der Änderungsverlauf enthält Details dazu, wann eine Änderung erkannt wurde, und eine _Visual Diff_ jeder Änderung. Eine Änderungserkennung wird ausgelöst, wenn Resource Manager-Eigenschaften hinzugefügt, entfernt oder geändert werden.

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen.

1. Wählen Sie auf der Seite **Übersicht** oder **Konformität** eine Richtlinie mit einem beliebigen **Konformitätsstatus** aus.

1. Wählen Sie auf der Seite **Richtlinienkonformität** auf der Registerkarte **Ressourcenkonformität** eine Ressource aus.

1. Wählen Sie auf der Seite **Ressourcenkonformität** die Registerkarte **Änderungsverlauf (Vorschau)** aus. Eine Liste der erkannten Änderungen, falls vorhanden, wird angezeigt.

   ![Registerkarte „Richtlinien-Änderungsverlauf“ auf der Seite „Ressourcenkonformität“](../media/determine-non-compliance/change-history-tab.png)

1. Wählen Sie eine der erkannten Änderungen aus. Die _Visual Diff_ für die Ressource wird auf der Seite **Änderungsverlauf** angezeigt.

   ![Visual Diff des Richtlinien-Änderungsverlaufs auf der Seite „Änderungsverlauf“](../media/determine-non-compliance/change-history-visual-diff.png)

Die _Visual Diff_ hilft, Änderungen an einer Ressource zu identifizieren. Die erkannten Änderungen müssen nicht zwangsläufig mit dem geänderten Konformitätsstatus der Ressource zusammenhängen.

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](../samples/index.md) finden Sie Beispiele
- Befassen Sie sich mit der [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md)
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).