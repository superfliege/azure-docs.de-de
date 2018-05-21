---
title: Benutzeroberflächenelement „InfoBox“ in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.InfoBox“ für das Azure-Portal beschrieben.
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
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9507280bf5df1e63c81f0b2488b9676784c69812
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcommoninfobox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.InfoBox“
Ein Steuerelement, das ein Informationsfeld hinzugefügt.

## <a name="schema"></a>Schema
```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="remarks"></a>Anmerkungen

* Verwenden Sie für `icon` entweder **None**, **Info**, **Warning** oder **Error**.
* Die `uri`-Eigenschaft ist optional.

## <a name="sample-output"></a>Beispielausgabe

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
