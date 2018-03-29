---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f76dd47081a826e341d15fedc583d29f0373e475
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. Weitere Informationen zur Protokollierung finden Sie unter [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) (Erste Schritte mit Azure CLI 2.0).

```azurecli
az login
```

Listen Sie die Abonnements für das Konto auf, wenn Sie über mehr als ein Azure-Abonnement verfügen.

```azurecli
az account list --all
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```