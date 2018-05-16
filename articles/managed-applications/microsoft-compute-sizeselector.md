---
title: Benutzeroberflächenelement SizeSelector in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement Microsoft.Compute.SizeSelector für das Azure-Portal beschrieben.
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
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: a38a5461e96d741b3a0d556990418e022afdb305
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Benutzeroberflächenelement „Microsoft.Compute.SizeSelector“
Ein Steuerelement zum Auswählen einer Größe für VM-Instanzen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- `recommendedSizes` muss mindestens eine Größe enthalten. Die erste empfohlene Größe wird als Standardwert verwendet.
- Wenn eine empfohlene Größe am ausgewählten Standort nicht verfügbar ist, wird die Größe automatisch übersprungen. Stattdessen wird die nächste empfohlene Größe verwendet.
- Alle nicht in `constraints.allowedSizes` angegebenen Größen werden ausgeblendet, und nicht in `constraints.excludedSizes` angegebene Größen werden angezeigt.
`constraints.allowedSizes` und `constraints.excludedSizes` sind optional, können jedoch nicht gleichzeitig angegeben werden. Die Liste der verfügbaren Größen kann mithilfe der Informationen unter [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region) (Auflisten verfügbarer VM-Größen für ein Abonnement) abgerufen werden.
- `osPlatform` muss angegeben werden. Mögliche Optionen: **Windows** oder **Linux**. Damit werden die Hardwarekosten der virtuellen Computer ermittelt.
- `imageReference` wird für Images von Erstanbietern ausgelassen, für Images von Drittanbietern jedoch angegeben. Damit werden die Softwarekosten der virtuellen Computer ermittelt.
- `count` dient zum Festlegen des entsprechenden Multiplikators für das Element. Das Element unterstützt einen statischen Wert wie **2** oder einen dynamischen Wert aus einem anderen Element, etwa `[steps('step1').vmCount]`. Der Standardwert ist **1**.
- Für `numAvailabilityZonesRequired` kann 1, 2 oder 3 festgelegt werden.

## <a name="sample-output"></a>Beispielausgabe
```json
"Standard_D1"
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
