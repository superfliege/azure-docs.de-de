---
title: "Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten | Microsoft-Dokumentation"
description: "Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten"
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: c22a6dde0067385a1bf8d889cc76178bb44dd0ac
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Anforderungen zur Erhöhung von Resource Manager-vCPU-Kontingenten

Resource Manager-vCPU-Kontingente werden auf Regionsebene und auf der Ebene der SKU-Familie erzwungen.
Weitere Informationen zur Erzwingung von Kontingenten finden Sie auf der Seite [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](http://aka.ms/quotalimits).
Auf der Seite [Virtuelle Windows-Computer – Preise](http://aka.ms/pricingcompute) können Sie die Kosten und die Leistung vergleichen und sich ausführlicher über SKU-Familien informieren.

Wenn Sie eine Erhöhung anfordern möchten, erstellen Sie im Azure-Portal ([https://portal.azure.com](https://portal.azure.com)) eine kontingentbezogene Supportanfrage für vCPUs.

> [!NOTE]
> Informationen zum Erstellen einer Supportanfrage im Azure-Portal finden Sie [hier](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

1. Wählen Sie auf der Seite für neue Supportanfragen den Problemtyp „Kontingent“ und den Kontingenttyp „Kerne“ aus.

    ![Blatt für die Kontingentgrundeinstellungen](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. Wählen Sie als Bereitstellungsmodell die Option „Resource Manager“ und anschließend einen Speicherort aus.

    ![Blatt für das Kontingentproblem](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Wählen Sie die SKU-Familien aus, für die eine Erhöhung erforderlich ist.

    ![Ausgewählte SKU-Serie](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Geben Sie die gewünschten Grenzwerte für das Abonnement ein.

    ![Neue SKU-Kontingentanforderung](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol.
Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie auf der Problemschrittseite auf „Weiter“, um die Erstellung der Supportanfrage fortzusetzen.
