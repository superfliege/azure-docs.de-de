---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 4f5dbbd1da8a221fb5f8b9641e016829987db538
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
Im Folgenden finden Sie vor dem Erstellen von Elementen Informationen über das Konzept von Bereichen. Azure bietet vier Verwaltungsebenen: Verwaltungsgruppen, Abonnement, Ressourcengruppe und Ressource. [Verwaltungsgruppen](../articles/billing/billing-enterprise-mgmt-group-overview.md) befinden sich in der Vorschauversion. Die folgende Abbildung zeigt ein Beispiel dieser Ebenen.

![Umfang](./media/resource-manager-governance-scope/scope-levels.png)

Sie wenden die Verwaltungseinstellungen auf einer dieser Bereichsebenen an. Die von Ihnen ausgewählte Ebene bestimmt, wie umfassend die Einstellung angewendet wird. Niedrigere Ebenen erben die Einstellungen von höheren Ebenen. Wenn Sie eine Einstellung auf das Abonnement anwenden, gilt diese Einstellung für alle Ressourcengruppen und Ressourcen in Ihrem Abonnement. Wenn Sie eine Einstellung auf die Ressourcengruppe anwenden, gilt diese Einstellung für die Ressourcengruppe und alle zugehörigen Ressourcen. Für eine andere Ressourcengruppe gilt die Einstellung jedoch nicht.

In der Regel ist es sinnvoll, wichtige Einstellungen auf höheren Ebenen und projektspezifische Anforderungen auf niedrigeren Ebenen anzuwenden. Sie könnten beispielsweise sicherstellen, dass alle Ressourcen für Ihre Organisation in bestimmten Regionen bereitgestellt werden. Dazu wenden Sie eine Richtlinie, die die zulässigen Standorte angibt, auf das Abonnement an. Wenn andere Benutzer in Ihrer Organisation neue Ressourcengruppen und Ressourcen hinzufügen, werden die zulässigen Standorte automatisch erzwungen. 
