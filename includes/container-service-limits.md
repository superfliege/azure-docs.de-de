---
title: Includedatei
description: Includedatei
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 23c25953d2f493d2dd799bfd11dbbb69db002d1b
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55736050"
---
| Ressource | Standardlimit |
| --- | :--- |
| Max. Cluster pro Abonnement | 100 |
| Max. Knoten pro Cluster | 100 |
| Max. Pods pro Knoten: [Basisnetzwerke][basic-networking] mit Kubenet | 110 |
| Max. Pods pro Knoten: [Erweiterte Netzwerke][advanced-networking] mit Azure CNI | Bereitstellung mithilfe der Azure CLI: 30<sup>1</sup><br />Resource Manager-Vorlage: 30<sup>1</sup><br />Bereitstellung über das Portal: 30 |

<sup>1</sup> Wenn Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle oder über eine Resource Manager-Vorlage bereitstellen, kann dieser Wert auf bis zu **110 Pods pro Knoten** festgelegt werden. Wenn Sie bereits einen AKS-Cluster bereitgestellt haben oder einen Cluster über das Azure-Portal bereitstellen, kann die maximale Anzahl von Pods pro Knoten nicht konfiguriert werden.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
