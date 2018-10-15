---
title: Includedatei
description: Includedatei
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 71294824bd3dd5215c388cfcd44382c7eee123ad
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48874096"
---
| Ressource | Standardlimit |
| --- | :--- |
| Max. Cluster pro Abonnement | 100 |
| Max. Knoten pro Cluster | 100 |
| Max. Pods pro Knoten: [grundlegende Netzwerke][basic-networking] mit Kubernetes | 110 |
| Max. Pods pro Knoten: [erweiterte Netzwerke][advanced-networking] mit Azure CNI | Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle: 30<sup>1</sup><br />Resource Manager-Vorlage: 30<sup>1</sup><br />Bereitstellung über das Portal: 30 |

<sup>1</sup> Dieser Wert kann beim Bereitstellen eines AKS-Clusters mit der Azure-Befehlszeilenschnittstelle oder einer Resource Manager-Vorlage konfiguriert werden.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
