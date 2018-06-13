---
title: Benutzeroberflächenelement PublicIpAddressCombo in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement Microsoft.Network.PublicIpAddressCombo für das Azure-Portal beschrieben.
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
ms.openlocfilehash: bf0ef5be609fba14ab12e1e6f9f97bc63f032aae
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260559"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Network.PublicIpAddressCombo“
Eine Gruppe von Steuerelementen zum Auswählen einer neuen oder vorhandenen öffentlichen IP-Adresse.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Wenn der Benutzer für die öffentliche IP-Adresse „Keine“ auswählt, wird das Textfeld für die Domänennamenbezeichnung ausgeblendet.
- Wenn der Benutzer eine vorhandene öffentliche IP-Adresse auswählt, wird das Textfeld für die Domänennamenbezeichnung deaktiviert. Der Wert ist die Domänennamenbezeichnung der ausgewählten IP-Adresse.
- Das Domänennamensuffix (z.B. „westus.cloudapp.azure.com“) wird basierend auf dem ausgewählten Standort automatisch aktualisiert.

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wenn `constraints.required.domainNameLabel` auf **true** festgelegt ist, muss der Benutzer beim Erstellen einer neuen öffentlichen IP-Adresse eine Domänennamenbezeichnung angeben. Vorhandene öffentliche IP-Adressen ohne Bezeichnung stehen nicht zur Auswahl zur Verfügung.
- Wenn `options.hideNone` auf **true** festgelegt ist, wird die Option zum Auswählen von **Keine** für die öffentlichen IP-Adresse ausgeblendet. Der Standardwert ist **false**.
- Wenn `options.hideDomainNameLabel` auf **true** festgelegt ist, wird das Textfeld für die Domänennamenbezeichnung ausgeblendet. Der Standardwert ist **false**.
- Wenn `options.hideExisting` auf „true“ festgelegt ist, kann der Benutzer keine vorhandene öffentliche IP-Adresse auswählen. Der Standardwert ist **false**.
- Für `zone` sind nur öffentliche IP-Adressen für die angegebene Zone oder zonenresiliente öffentliche IP-Adressen verfügbar.

## <a name="sample-output"></a>Beispielausgabe
Wenn der Benutzer keine öffentliche IP-Adresse auswählt, wird die folgende Ausgabe erwartet:
```json
{
  "newOrExistingOrNone": "none"
}
```

Wenn der Benutzer eine neue oder vorhandene öffentliche IP-Adresse auswählt, wird die folgende Ausgabe erwartet:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "newOrExistingOrNone": "new"
}
```
- Wenn `options.hideNone` als **true** festgelegt ist, hat `newOrExistingOrNone` nur den Wert **new** oder **existing**.
- Wenn `options.hideDomainNameLabel` als **true** festgelegt ist, wird `domainNameLabel` nicht deklariert.

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
