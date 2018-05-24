---
title: Benutzeroberflächenelement „OptionsGroup“ in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.OptionsGroup“ für das Azure-Portal beschrieben.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 5387f3911c58b115629c461420737230fce6b85a
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.OptionsGroup“
Ein Auswahlsteuerelement mit einer Zeile der verfügbaren Optionen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "my value",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Die Bezeichnung für `constraints.allowedValues` ist der Anzeigetext für ein Element, und sein Wert ist der Ausgabewert des Elements bei der Auswahl.
- Wenn ein Wert angegeben wird, muss der Standardwert einer in `constraints.allowedValues` vorhandenen Bezeichnung entsprechen. Wird kein Wert angegeben, wird standardmäßig das erste Element in `constraints.allowedValues` ausgewählt. Der Standardwert lautet **null**.
- `constraints.allowedValues` muss mindestens ein Element enthalten.
- Dieses Element unterstützt die `constraints.required`-Eigenschaft nicht. Für eine erfolgreiche Überprüfung muss ein Element ausgewählt werden.

## <a name="sample-output"></a>Beispielausgabe
```json
"Bar"
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
