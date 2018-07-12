---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 7843410043b726526380b2a916d96f414a2decda
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740571"
---
Nachdem Sie Tags auf Ressourcen angewendet haben, können Sie die Kosten für Ressourcen mit diesen Tags anzeigen. Es dauert eine Weile, bis die Kostenanalyse die jüngste Nutzung anzeigt, daher werden möglicherweise noch keine Kosten angezeigt. Wenn die Kosten verfügbar sind, können Sie diese für Ressourcen in verschiedenen Ressourcengruppen Ihres Abonnements anzeigen. Benutzer müssen über [Zugriff auf Abrechnungsinformationen auf Abonnementebene](../articles/billing/billing-manage-access.md) verfügen, um die Kosten sehen zu können.

Um Kosten nach Tag im Portal anzuzeigen, wählen Sie Ihr Abonnement und danach **Kostenanalyse** aus.

![Kostenanalyse](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

Filtern Sie die Anzeige nach Tagwert, und wählen Sie **Anwenden** aus.

![Anzeigen von Kosten nach Tag](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

Sie können auch die [Azure-Abrechnungs-APIs](../articles/billing/billing-usage-rate-card-overview.md) verwenden, um Kosten programmgesteuert anzuzeigen.
