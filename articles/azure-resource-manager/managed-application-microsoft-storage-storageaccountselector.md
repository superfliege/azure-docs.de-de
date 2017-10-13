---
title: "Benutzeroberflächenelement „StorageAccountSelector“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Storage.StorageAccountSelector“ für verwaltete Azure-Anwendungen beschrieben."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tomfitz
ms.openlocfilehash: 8971232f9700bc85830c4ac37f6c37f1bb5df0b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Benutzeroberflächenelement „Microsoft.Storage.StorageAccountSelector“
Ein Steuerelement zum Auswählen eines neuen oder vorhandenen Speicherkontos. Dieses Element verwenden Sie beim [Erstellen einer verwalteten Azure-Anwendung](managed-application-publishing.md).

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wird `defaultValue.name` angegeben, wird das Element automatisch auf Eindeutigkeit überprüft. Wenn der Name des Speicherkontos nicht eindeutig ist, muss der Benutzer einen anderen Namen angeben oder ein vorhandenes Speicherkonto auswählen.
- Der Standardwert für `defaultValue.type` lautet **Premium_LRS**.
- Alle nicht in `constraints.allowedTypes` angegebenen Typen werden ausgeblendet, und nicht in `constraints.excludedTypes` angegebene Typen werden angezeigt.
`constraints.allowedTypes` und `constraints.excludedTypes` sind optional, können jedoch nicht gleichzeitig angegeben werden.
- Wenn **true** für `options.hideExisting` angegeben ist, kann der Benutzer kein vorhandenes Speicherkonto auswählen. Der Standardwert ist **false**.


## <a name="sample-output"></a>Beispielausgabe
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).
