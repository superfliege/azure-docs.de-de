---
title: Benutzeroberflächenelement „UserNameTextBox“ in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement „Microsoft.Compute.UserNameTextBox“ für das Azure-Portal beschrieben.
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
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: b65b62389fbb66c9461430d1dd2df42e71bf6cff
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260721"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Compute.UserNameTextBox“
Ein Textfeldsteuerelement mit integrierter Überprüfung für Windows- und Linux-Benutzernamen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wenn `constraints.required` auf **true** festgelegt ist, muss das Textfeld für eine erfolgreiche Überprüfung einen Wert enthalten. Der Standardwert lautet **true**.
- `osPlatform` muss angegeben werden. Mögliche Optionen: **Windows** oder **Linux**.
- `constraints.regex` ist ein Muster für einen regulären JavaScript-Ausdruck. Wenn der Wert des Textfelds angegeben wird, muss er für eine erfolgreiche Überprüfung dem Muster entsprechen. Der Standardwert lautet **null**.
- `constraints.validationMessage` ist eine Zeichenfolge, die angezeigt wird, wenn bei der durch `constraints.regex` angegebenen Überprüfung des Werts des Textfelds ein Fehler auftritt. Wird kein Wert angegeben, werden die integrierten Überprüfungsnachrichten des Textfelds verwendet. Der Standardwert lautet **null**.
- Dieses Element verfügt über eine integrierte Überprüfung, die auf dem für `osPlatform` angegebenen Wert basiert. Die integrierte Überprüfung kann zusammen mit einem benutzerdefinierten regulären Ausdruck verwendet werden.
Wenn ein Wert für `constraints.regex` angegeben ist, werden die integrierte und die benutzerdefinierte Überprüfung ausgelöst.

## <a name="sample-output"></a>Beispielausgabe
```json
"tabrezm"
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
