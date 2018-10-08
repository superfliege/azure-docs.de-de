---
title: Azure Front Door Service – PowerShell-Referenz | Microsoft-Dokumentation
description: Diese Referenz enthält Details zu den verschiedenen PowerShell-Cmdlets, die bei Azure Front Door Service unterstützt werden.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2018
ms.author: sharadag
Module Name: AzureRM.FrontDoor
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/new-azurermfrontdoormatchconditionobject
schema: 2.0.0
ms.openlocfilehash: 3a6804abf38126e9296760f5802f75ef35335294
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046015"
---
# New-AzureRmFrontDoorMatchConditionObject
Erstellen eines MatchCondition-Objekts zur WAF-Richtlinienerstellung

## SYNTAX

```
New-AzureRmFrontDoorMatchConditionObject -MatchVariable <PSMatchVariable>
 -OperatorProperty <PSOperatorProperty> -MatchValue <String[]> [-Selector <String>]
 [-NegateCondition <Boolean>] [-DefaultProfile <IAzureContextContainer>] [<CommonParameters>]
```

## Beschreibung
Erstellen eines MatchCondition-Objekts zur WAF-Richtlinienerstellung

## BEISPIELE

### Beispiel 1
```powershell
PS C:\> New-AzureRmFrontDoorMatchConditionObject -MatchVariable RequestHeader -OperatorProperty Contains -Selector "UserAgent" -MatchValue "Windows"


MatchVariable    : RequestHeader
OperatorProperty : Contains
MatchValue       : {Windows}
Selector         : UserAgent
NegateCondition  : False
```

Erstellen eines MatchCondition-Objekts

## PARAMETER

### -DefaultProfile
Anmeldeinformationen, Konto, Mandant und Abonnement für die Kommunikation mit Azure

```yaml
Type: Microsoft.Azure.Commands.Common.Authentication.Abstractions.IAzureContextContainer
Parameter Sets: (All)
Aliases: AzureRmContext, AzureCredential

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MatchValue
Übereinstimmungswert

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MatchVariable
Übereinstimmungsvariable.
Folgende Werte sind möglich: „RemoteAddr“, „RequestMethod“, „QueryString“, „PostArgs“, „RequestUri“, „RequestHeader“, „RequestBody“

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSMatchVariable
Parameter Sets: (All)
Aliases:
Accepted values: RemoteAddr, RequestMethod, QueryString, PostArgs, RequestUri, RequestHeader, RequestBody

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NegateCondition
Beschreibt, ob dies eine negierende Bedingung ist. Der Standardwert ist „false“.

```yaml
Type: System.Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OperatorProperty
Beschreibt den abzugleichenden Operator.
Folgende Werte sind möglich: „Any“, „IPMatch“, „GeoMatch“, „Equal“, „Contains“, „LessThan“, „GreaterThan“, „LessThanOrEqual“, „GreaterThanOrEqual“, „BeginsWith“, „EndsWith“

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSOperatorProperty
Parameter Sets: (All)
Aliases:
Accepted values: Any, IPMatch, GeoMatch, Equal, Contains, LessThan, GreaterThan, LessThanOrEqual, GreaterThanOrEqual, BeginsWith, EndsWith

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Selector
Name des Selektors im abzugleichenden „RequestHeader“ oder „RequestBody“

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable. Weitere Informationen finden Sie unter „about_CommonParameters“ (http://go.microsoft.com/fwlink/?LinkID=113216).
