---
title: Azure-Benutzeroberflächenelement AvailabilityZoneDropdown | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement Microsoft.Network.AvailabilityZoneDropdown für das Azure-Portal beschrieben.
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
ms.openlocfilehash: 737971551e4bfb81c4f5fd1b53987b7a52c12669
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftnetworkavailabilityzonedropdown-ui-element"></a>Benutzeroberflächenelement Microsoft.Network.AvailabilityZoneDropdown
Ein Steuerelement zum Auswählen der Verfügbarkeitszone.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Network.AvailabilityZoneDropDown](./media/managed-application-elements/microsoft.network.availabilityzonedropdown.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.AvailabilityZoneDropdown",
  "label": "Availability zone dropdown label",
  "toolTip": "This is the tooltip for availability zones",
  "defaultValue": "None",
  "constraints": {
    "required": "true"
  },
  "options": {
  },
  "visible": true
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
