---
title: Azure CLI-Skriptbeispiel – Abrufen von Details zu einer Azure Cache for Redis-Instanz | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Abrufen von Details zu einer Azure Cache for Redis-Instanz
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 0dd32ae2b8f0fb91ca8d6391d5c3f3979d13b90f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085262"
---
# <a name="get-details-of-an-azure-cache-for-redis"></a>Abrufen von Details zu einer Azure Cache for Redis-Instanz

In diesem Szenario erfahren Sie, wie die Details einer Azure Cache for Redis-Instanz abgerufen werden, einschließlich ihres Bereitstellungsstatus.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Cache for Redis")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Details einer Azure Cache for Redis-Instanz abzurufen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Ruft Details zu einer Azure Cache for Redis-Instanz ab. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche Azure Cache for Redis-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Cache for Redis](../cli-samples.md).