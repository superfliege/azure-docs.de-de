---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fee97b29f24d8bb4f50a2929c3ceb33af85a5e21
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852366"
---
Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten.



Wenn Sie Azure PowerShell lokal ausführen, stellen Sie eine Verbindung mit Ihrem Azure-Konto her. Das Cmdlet *Connect-AzureRmAccount* fordert Sie zur Eingabe von Anmeldeinformationen auf. Nach der Authentifizierung werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen. Wenn Sie PowerShell nicht lokal ausführen und stattdessen über Azure Cloud Shell die Funktion zum Testen im Browser verwenden, können Sie diesen ersten Schritt überspringen. Es wird automatisch eine Verbindung mit Ihrem Azure-Konto hergestellt.

```azurepowershell
Connect-AzureRmAccount
```

Falls Sie über mehrere Abonnements verfügen, rufen Sie eine Liste Ihrer Azure-Abonnements ab.

```azurepowershell-interactive
Get-AzureRmSubscription
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurepowershell-interactive
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```