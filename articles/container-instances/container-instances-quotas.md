---
title: 'Azure Container Instances: Kontingente und Regionsverfügbarkeit'
description: Hier finden Sie Informationen zu den Standardkontingenten und zur Regionsverfügbarkeit des Azure Container Instances-Diensts.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 12/07/2018
ms.author: danlep
ms.openlocfilehash: a7b61702feb062c57fdec84f335ace44a47d0283
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249480"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Kontingente und Regionsverfügbarkeit für Azure Container Instances

Für die Ressourcen und Funktionen aller Azure-Dienste gelten bestimmte Standardlimits und Kontingente. Die folgenden Abschnitte enthalten Informationen zu den Standardressourcenlimits für mehrere ACI-Ressourcen (Azure Container Instances) sowie zur Verfügbarkeit des ACI-Diensts in Azure-Regionen.

## <a name="service-quotas-and-limits"></a>Dienstkontingente und Limits

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Regionale Verfügbarkeit

Azure Container Instances ist mit den angegebenen CPU- und Arbeitsspeicherlimits in den folgenden Regionen verfügbar:

| Standort | Betriebssystem | CPU | Arbeitsspeicher (GB) |
| -------- | -- | :---: | :-----------: |
| „USA, Osten“, „Europa, Norden“, „Europa, Westen“, „USA, Westen“, „USA, Westen 2“ | Linux | 4 | 14 |
| Japan, Osten | Linux | 2 | 8 |
| „Australien, Osten“, „USA, Osten 2“, „Asien, Südosten“ | Linux | 2 | 7 |
| „Kanada, Mitte“, „Indien, Mitte“, „Asien, Osten“, „USA, Norden-Mitte“, „USA, Süden-Mitte“ | Linux | 2 | 3,5 |
| „USA, Osten“, „Europa, Westen“, „USA, Westen“ | Windows | 4 | 14 |
| „Australien, Osten“, „Kanada, Mitte“, „Indien, Mitte“, „Asien, Osten“, „USA, Osten 2“, „Japan, Osten“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „USA, Westen 2“ | Windows | 2 | 3,5 |

Containerinstanzen, die innerhalb dieser Ressourcenlimits erstellt werden, unterliegen der Verfügbarkeit in der Bereitstellungsregion. Wenn eine Region stark ausgelastet ist, kann bei der Bereitstellung von Instanzen ein Fehler auftreten. Um einen solchen Fehler bei der Bereitstellung zu beheben, versuchen Sie, Instanzen mit niedrigeren CPU- und Arbeitsspeichereinstellungen bereitzustellen, oder führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.

Teilen Sie dem Team auf [aka.ms/aci/feedback](https://aka.ms/aci/feedback) mit, wenn zusätzliche Regionen und eine Erhöhung der CPU-/Arbeitsspeichergrenzwerte erforderlich sind.

Weitere Informationen zur Problembehandlung bei der Bereitstellung von Containerinstanzen finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Nächste Schritte

Bestimmte Standardlimits und Kontingente können erhöht werden. Wenn Sie eine Erhöhung von Ressourcen anfordern möchten, für die dies unterstützt wird, müssen Sie eine [Azure-Supportanfrage][azure-support] übermitteln und dabei als **Problemtyp** die Option „Kontingent“ auswählen.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
