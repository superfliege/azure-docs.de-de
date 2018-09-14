---
title: Includedatei
description: Includedatei
services: container-service
author: mmacy
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 1e8913d31677f3da9b6eb9d2216d8d9ec8b186b4
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666999"
---
| Ressource | Standardlimit |
| --- | :--- |
| Max. Cluster pro Abonnement | 100 |
| Max. Knoten pro Cluster | 100 |
| Max. Pods pro Knoten: [grundlegende Netzwerke][basic-networking] mit Kubernetes | 110 |
| Max. Pods pro Knoten: [erweiterte Netzwerke][advanced-networking] mit Azure CNI | Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle: 110<sup>1</sup><br />Resource Manager-Vorlage: 110<sup>1</sup><br />Bereitstellung über das Portal: 30 |

<sup>1</sup> Dieser Wert kann beim Bereitstellen eines AKS-Clusters mit der Azure-Befehlszeilenschnittstelle oder einer Resource Manager-Vorlage konfiguriert werden.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
