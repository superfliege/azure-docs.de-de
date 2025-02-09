---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 77019068279261a36f8ba40247e5f5694ca77892
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238650"
---
| Resource | Begrenzung |
| --- | --- |
| Cachegröße |530 GB |
| Datenbanken |64 |
| Maximale Anzahl verbundener Clients |40.000 |
| Azure Cache for Redis-Replikate, für Hochverfügbarkeit |1 |
| Shards in einem Premium-Cache mit Clustering |10 |

Die erhältliche Größe und die Konfigurationsbeschränkungen von Azure Cache for Redis unterscheiden sich je nach Tarif. Die Tarife und die damit verbundenen Größen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/).

Weitere Informationen zu den Konfigurationsbeschränkungen von Azure Cache for Redis finden Sie unter [Standardmäßige Redis-Serverkonfiguration](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Da die Konfiguration und Verwaltung von Azure Cache for Redis-Instanzen von Microsoft übernommen wird, werden nicht alle Redis-Befehle in Azure Cache for Redis unterstützt. Weitere Informationen finden Sie unter [Redis-Befehle, die in Azure Cache for Redis nicht unterstützt werden](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

