---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c1b5560e16b68565c37365ac9c2cba217d9b1b90
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150050"
---
Mit dem Cmdlet „Get-AzureVNetConnection“ können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde.

1. Verwenden Sie das folgende Cmdlet-Beispiel, und konfigurieren Sie die Werte so, dass sie Ihren eigenen Werten entsprechen. Der Name des virtuellen Netzwerks muss in Anführungszeichen gesetzt werden, wenn er Leerstellen enthält.

   ```azurepowershell
   Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
   ```
2. Sehen Sie sich nach Abschluss des Cmdlets die Werte an. Im Beispiel weiter unten ist der Verbindungsstatus als „Connected“ (Verbunden) angegeben, und Sie sehen die Eingangs- und Ausgangsbytes.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal