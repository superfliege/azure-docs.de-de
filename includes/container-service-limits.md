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
ms.openlocfilehash: 4251f379c517d5ccfd0430987e3d5280208590ff
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400441"
---
| Ressource | Standardlimit |
| --- | :--- |
| Max. Cluster pro Abonnement | 100 |
| Max. Knoten pro Cluster | 100 |
| Max. Pods pro Knoten: [grundlegende Netzwerke][basic-networking] mit Kubernetes | 110 |
| Max. Pods pro Knoten: [erweiterte Netzwerke][advanced-networking] mit Azure CNI | Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle: 30<sup>1</sup><br />Resource Manager-Vorlage: 30<sup>1</sup><br />Bereitstellung über das Portal: 30 |

<sup>1</sup> Wenn Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle oder über eine Resource Manager-Vorlage bereitstellen, kann dieser Wert auf bis zu **110 Pods pro Knoten** festgelegt werden. Wenn Sie bereits einen AKS-Cluster bereitgestellt haben oder einen Cluster über das Azure-Portal bereitstellen, kann die maximale Anzahl von Pods pro Knoten nicht konfiguriert werden.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
