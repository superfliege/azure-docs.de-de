---
title: Ressourcenverfügbarkeit für Azure Container Instances
description: Verfügbarkeit von Compute- und Arbeitsspeicherressourcen für den Azure Container Instances-Dienst in verschiedenen Azure-Regionen
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 03/01/2019
ms.author: danlep
ms.openlocfilehash: 1ca23a95c746139963aa70ed20bb888152fd5cd8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554873"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Ressourcenverfügbarkeit für Azure Container Instances in Azure-Regionen

In diesem Artikel wird die Verfügbarkeit der Compute- und Arbeitsspeicherressourcen von Azure Container Instances in Azure-Regionen erläutert. 

Die angezeigten Werte stellen die maximalen Ressourcen dar, die pro Bereitstellung einer [Containergruppe](container-instances-container-groups.md) zur Verfügung stehen. Die Werte sind zum Zeitpunkt der Veröffentlichung aktuell. Verwenden Sie die API zum [Auflisten von Funktionen](/rest/api/container-instances/listcapabilities/listcapabilities), um aktuelle Informationen abzurufen. 

> [!NOTE]
> Containergruppen, die innerhalb dieser Ressourcenlimits erstellt werden, unterliegen der Verfügbarkeit in der Bereitstellungsregion. Wenn eine Region stark ausgelastet ist, kann bei der Bereitstellung von Instanzen ein Fehler auftreten. Um einen solchen Fehler bei der Bereitstellung zu beheben, versuchen Sie, Instanzen mit niedrigeren Ressourceneinstellungen bereitzustellen, oder führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.

Weitere Informationen zu Kontingenten und anderen Grenzwerten in Ihren Bereitstellungen finden Sie unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Verfügbarkeit: allgemein

| Standort | Betriebssystem | CPU | Arbeitsspeicher (GB) |
| -------- | -- | :---: | :-----------: |
| „Kanada, Mitte“, „USA, Mitte“, „USA, Osten 2“, „USA, Süden-Mitte“ | Linux | 4 | 16 |
| „USA, Osten“, „Europa, Norden“, „Europa, Westen“, „USA, Westen“, „USA, Westen 2“ | Linux | 4 | 14 |
| Japan, Osten | Linux | 2 | 8 |
| „Australien, Osten“, „Asien, Südosten“ | Linux | 2 | 7 |
| „Indien, Mitte“, Asien, Osten“, „USA, Norden-Mitte“, „Indien, Süden“ | Linux | 2 | 3,5 |
| „USA, Osten“, „Europa, Westen“, „USA, Westen“ | Windows | 4 | 14 |
| „Australien, Osten“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten 2“, „Japan, Osten“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Indien, Süden“, „Asien, Südosten“, „USA, Westen 2“ | Windows | 2 | 3,5 |

## <a name="availability---virtual-network-deployment-preview"></a>Verfügbarkeit: Bereitstellung eines virtuellen Netzwerks (Vorschauversion)

Für eine Containergruppe, die in einem [virtuellen Azure-Netzwerk](container-instances-vnet.md) (Vorschau) bereitgestellt wird, sind die folgenden Regionen und Ressourcen verfügbar:

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Verfügbarkeit: GPU-Ressourcen (Vorschauversion)

Für eine Containergruppe, die mit [GPU-Ressourcen](container-instances-gpu.md) (Vorschau) bereitgestellt wird, sind die folgenden Regionen und Ressourcen verfügbar:

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

Teilen Sie dem Team auf [aka.ms/aci/feedback](https://aka.ms/aci/feedback) mit, wenn zusätzliche Regionen und eine Erhöhung der Ressourcenverfügbarkeit erforderlich sind.

Informationen zur Problembehandlung bei der Bereitstellung von Containerinstanzen finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).
