---
title: Azure Front Door Service – PowerShell-Referenz | Microsoft-Dokumentation
description: Diese Referenz enthält Details zu den verschiedenen PowerShell-Cmdlets, die für Azure Front Door Service unterstützt werden.
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
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/remove-azurermfrontdoor
schema: 2.0.0
ms.openlocfilehash: ddc6a40521b951c8083ac185368621ea792b1c85
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046470"
---
# Remove-AzureRmFrontDoor
Entfernen des Front Door-Lastenausgleichs

## SYNTAX

### ByFieldsParameterSet (Standard)
```
Remove-AzureRmFrontDoor -ResourceGroupName <String> -Name <String> [-PassThru]
 [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByObjectParameterSet
```
Remove-AzureRmFrontDoor -InputObject <PSFrontDoor> [-PassThru] [-DefaultProfile <IAzureContextContainer>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ResourceIdParameterSet
```
Remove-AzureRmFrontDoor -ResourceId <String> [-PassThru] [-DefaultProfile <IAzureContextContainer>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Beschreibung
Das Cmdlet **Remove-AzureRmFrontDoor** entfernt einen Front Door-Lastenausgleich unter dem aktuellen Abonnement.

## BEISPIELE

### Beispiel 1: Entfernen von „frontdoor1“ in der Ressourcengruppe „rg1“ unter dem aktuellen Abonnement
```powershell
PS C:\> Remove-AzureRmFrontDoor -Name "frontdoor1" -ResourceGroupName "rg1"
```

Entfernen von „frontdoor1“ in der Ressourcengruppe „rg1“ unter dem aktuellen Abonnement

### Beispiel 2: Entfernen aller FrontDoor-Instanzen in der Ressourcengruppe „rg1“ unter dem aktuellen Abonnement
```powershell
PS C:\> Get-AzureRmFrontDoor -ResourceGroupName "rg1" | Remove-AzureRmFrontDoor
```

Entfernen aller FrontDoor-Instanzen in der Ressourcengruppe „rg1“ unter dem aktuellen Abonnement

### Beispiel 3: Entfernen aller FrontDoor-Instanzen unter dem aktuellen Abonnement
```powershell
PS C:\> Get-AzureRmFrontDoor | Remove-AzureRmFrontDoor
```

Entfernen aller FrontDoor-Instanzen unter dem aktuellen Abonnement

### Beispiel 4: Entfernen aller FrontDoor-Instanzen namens „frontdoor1“ unter dem aktuellen Abonnement
```powershell
PS C:\> Remove-AzureRmFrontDoor -Name "frontdoor1" | Remove-AzureRmFrontDoor
```

Entfernen aller FrontDoor-Instanzen namens „frontdoor1“ unter dem aktuellen Abonnement

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

### -InputObject
Das zu löschende Front Door-Objekt

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSFrontDoor
Parameter Sets: ByObjectParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name
Der Name der zu löschenden Front Door-Instanz

```yaml
Type: System.String
Parameter Sets: ByFieldsParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru
Das Rückgabeobjekt (falls angegeben)

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceGroupName
Die Ressourcengruppe, zu der die Front Door-Instanz gehört.

```yaml
Type: System.String
Parameter Sets: ByFieldsParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceId
Die Ressourcen-ID der zu löschenden Front Door-Instanz

```yaml
Type: System.String
Parameter Sets: ResourceIdParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Confirm
Hiermit werden Sie vor der Ausführung des Cmdlets zur Bestätigung aufgefordert.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf
Zeigt, was geschieht, wenn das Cmdlet ausgeführt wird.
Das Cmdlet wird nicht ausgeführt.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable. Weitere Informationen finden Sie unter "about_CommonParameters" (http://go.microsoft.com/fwlink/?LinkID=113216).
