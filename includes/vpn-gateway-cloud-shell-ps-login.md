---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: deabef0c2c3540e515fe72a161710c95a20fa86f
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418064"
---
Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten.



Wenn Sie Azure PowerShell lokal ausführen, stellen Sie eine Verbindung mit Ihrem Azure-Konto her. Das Cmdlet *Connect-AzAccount* fordert Sie zur Eingabe von Anmeldeinformationen auf. Nach der Authentifizierung werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen. Wenn Sie PowerShell nicht lokal ausführen und stattdessen über Azure Cloud Shell die Funktion zum Testen im Browser verwenden, können Sie diesen ersten Schritt überspringen. Es wird automatisch eine Verbindung mit Ihrem Azure-Konto hergestellt.

```azurepowershell
Connect-AzAccount
```

Falls Sie über mehrere Abonnements verfügen, rufen Sie eine Liste Ihrer Azure-Abonnements ab.

```azurepowershell-interactive
Get-AzSubscription
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```