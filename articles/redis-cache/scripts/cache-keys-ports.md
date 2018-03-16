---
title: "Azure CLI-Skriptbeispiel: Abrufen des Hostnamens, der Ports und Schlüssel für Azure Redis Cache | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Abrufen des Hostnamens, der Ports und Schlüssel für eine Azure Redis Cache-Instanz"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 9a2a6a7ed4bb56f93c965a5bfd24b21368117c7b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Abrufen des Hostnamens, der Ports und Schlüssel für Azure Redis Cache

In diesem Szenario erfahren Sie, wie den Hostnamen, die Ports und Schlüssel für Azure Redis Cache abrufen, um eine Verbindung mit einer Azure Redis Cache-Instanz herzustellen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um den Hostnamen, die Schlüssel und Ports einer Azure-Redis-Cache-Instanz abzurufen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Abrufen von Details einer Azure Redis Cache-Instanz. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Abrufen von Zugriffsschlüsseln für eine Azure Redis Cache-Instanz. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche Azure Redis Cache CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Redis Cache](../cli-samples.md).