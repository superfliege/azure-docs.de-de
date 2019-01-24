---
title: Azure CLI-Skriptbeispiel – Löschen einer Azure Cache for Redis-Instanz | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Löschen einer Azure Cache for Redis-Instanz
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 4a7b239010d57e3f352ee29afe267273391ffbcd
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461823"
---
# <a name="delete-an-azure-cache-for-redis"></a>Löschen einer Azure Cache for Redis-Instanz

In diesem Szenario erfahren Sie, wie Sie eine Azure Cache for Redis-Instanz löschen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um eine Azure Cache for Redis-Instanz zu löschen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az redis delete](https://docs.microsoft.com/cli/azure/redis) | Löscht eine Azure Cache for Redis-Instanz. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche Azure Cache for Redis-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Cache for Redis](../cli-samples.md).
