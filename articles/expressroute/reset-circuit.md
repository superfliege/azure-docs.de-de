---
title: 'Zurücksetzen einer fehlerhaften Azure ExpressRoute-Verbindung: PowerShell | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie eine ExpressRoute-Verbindung zurücksetzen, die sich in einem fehlerhaften Zustand befindet.
documentationcenter: na
services: expressroute
author: anzaman
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 423bc1d6409e5b7fe02339a05d0775f4ff42de49
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
ms.locfileid: "31590682"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Zurücksetzen einer fehlerhaften ExpressRoute-Verbindung

Wenn ein Vorgang über eine ExpressRoute-Verbindung nicht erfolgreich abgeschlossen wird, gelangt die Verbindung möglicherweise in einen Fehlerstatus. In diesem Artikel erfahren Sie, wie Sie eine fehlerhafte Azure ExpressRoute-Verbindung zurücksetzen.

## <a name="reset-a-circuit"></a>Zurücksetzen einer Verbindung

1. Installieren Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps).

2. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Überprüfen Sie die Abonnements für das Konto, wenn Sie über mehrere Azure-Abonnements verfügen.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Führen Sie die folgenden Befehle aus, um eine Verbindung zurückzusetzen, die sich in einem Fehlerzustand befindet:

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Die Verbindung sollte jetzt fehlerfrei sein. Öffnen Sie ein Supportticket beim [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls die Verbindung weiterhin fehlerhaft ist.

## <a name="next-steps"></a>Nächste Schritte

Öffnen Sie ein Supportticket beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , wenn weiterhin Probleme auftreten.
