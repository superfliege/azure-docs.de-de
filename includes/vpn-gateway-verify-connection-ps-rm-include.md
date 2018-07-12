---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/29/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 508685954e23a357fa5fc48b0e89c5e7daedb5c6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765920"
---
Sie können überprüfen, ob die Verbindungserstellung erfolgreich war, indem Sie das Cmdlet „Get-AzureRmVirtualNetworkGatewayConnection“ mit oder ohne den Zusatz „-Debug“ verwenden. 

1. Verwenden Sie das folgende Cmdlet-Beispiel, und konfigurieren Sie die Werte so, dass sie Ihren eigenen Werten entsprechen. Wählen Sie ggf. die Option „A“ für „Alle ausführen“ aus. In dem Beispiel verweist „-Name“ auf den Namen der Verbindung, die Sie testen möchten.

  ```azurepowershell-interactive
  Get-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
  ```
2. Sehen Sie sich nach Abschluss des Cmdlets die Werte an. Im Beispiel weiter unten ist der Verbindungsstatus als „Connected“ (Verbunden) angegeben, und Sie sehen die Eingangs- und Ausgangsbytes.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```