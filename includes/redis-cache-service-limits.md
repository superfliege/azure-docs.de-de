---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 01d6d933474d4a9c1e428b8df89fc766c9b40f20
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572149"
---
| Ressource | Begrenzung |
| --- | --- |
| Cachegröße |530 GB |
| Datenbanken |64 |
| Max. Anzahl verbundener Clients |40.000 |
| Redis Cache-Replikate (für Hochverfügbarkeit) |1 |
| Shards in einem Premium-Cache mit Clustering |10 |

Die erhältliche Größe und die Konfigurationsbeschränkungen von Azure Redis Cache unterscheiden sich je nach Tarif. Die Tarife und die damit verbundenen Größen finden Sie unter [Azure Redis Cache-Preise](https://azure.microsoft.com/pricing/details/cache/).

Weitere Informationen zu den Konfigurationsbeschränkungen von Azure Redis Cache finden Sie unter [Standardardmäßige Redis-Server-Konfiguration](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).

Da die Konfiguration und Verwaltung von Azure Redis Cache-Instanzen von Microsoft übernommen wird, werden nicht alle Redis-Befehle in Azure Redis Cache unterstützt. Weitere Informationen finden Sie unter [Redis-Befehle, die in Azure Redis Cache nicht unterstützt werden](../articles/redis-cache/cache-configure.md#redis-commands-not-supported-in-azure-redis-cache).

