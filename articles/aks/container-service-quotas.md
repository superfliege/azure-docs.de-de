---
title: 'Azure Kubernetes Service (AKS): Kontingente und Verfügbarkeit in den Regionen'
description: Hier finden Sie Informationen zu den Standardkontingenten und zur Verfügbarkeit von Azure Kubernetes Service (AKS) in den Regionen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: iainfou
ms.openlocfilehash: c8a2c0cac963fcc0622cff547e85593a13aa076a
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243830"
---
# <a name="quotas-and-region-availability-for-azure-kubernetes-service-aks"></a>Kontingente und Verfügbarkeit von Azure Kubernetes Service (AKS) in den Regionen

Für die Ressourcen und Funktionen aller Azure-Dienste gelten bestimmte Standardlimits und Kontingente. Die folgenden Abschnitte enthalten Informationen zu den Standardressourcenlimits für mehrere AKS-Ressourcen (Azure Kubernetes Service) sowie zur Verfügbarkeit des AKS-Diensts in Azure-Regionen.

## <a name="service-quotas-and-limits"></a>Dienstkontingente und Limits

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Bereitgestellte Infrastruktur

Alle anderen Netzwerk-, Compute- und Speichereinschränkungen gelten für die bereitgestellte Infrastruktur. Die relevanten Grenzwerte finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

## <a name="region-availability"></a>Regionale Verfügbarkeit

Azure Kubernetes Service (AKS) ist in den folgenden Regionen verfügbar:

- Australien (Osten)
- Australien, Südosten
- Kanada, Mitte
- Kanada, Osten
- USA (Mitte)
- Asien, Osten
- USA (Ost)
- USA (Ost 2)
- Frankreich, Mitte
- Japan, Osten
- Nordeuropa
- Asien, Südosten
- Indien (Süden)
- UK, Süden
- UK, Westen
- Europa, Westen
- USA (Westen)
- USA, Westen 2

## <a name="next-steps"></a>Nächste Schritte

Bestimmte Standardlimits und Kontingente können erhöht werden. Wenn Sie eine Erhöhung von Ressourcen anfordern möchten, für die dies unterstützt wird, müssen Sie eine [Azure-Supportanfrage][azure-support] übermitteln und dabei als **Problemtyp** die Option „Kontingent“ auswählen.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
