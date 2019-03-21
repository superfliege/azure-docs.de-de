---
title: Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten | Microsoft-Dokumentation
description: Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten
author: ganganarayanan
ms.author: gangan
ms.date: 6/13/2018
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 204deaf3a67984c0dd5eca5352686719e7767885
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792292"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Anforderungen zur Erhöhung von Resource Manager-vCPU-Kontingenten

Resource Manager-vCPU-Kontingente werden auf Regionsebene und auf der Ebene der SKU-Familie erzwungen.
Weitere Informationen zur Erzwingung von Kontingenten finden Sie auf der Seite [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://aka.ms/quotalimits).
Auf der Seite [Virtuelle Windows-Computer – Preise](https://aka.ms/pricingcompute) können Sie die Kosten und die Leistung vergleichen und sich ausführlicher über SKU-Familien informieren.

Wenn Sie eine Erhöhung anfordern möchten, führen Sie die Anweisungen unten aus, um über das Blatt „Usage + quota“ (Nutzung + Kontingente) im Azure-Portal eine Supportanfrage zu erstellen. 

## <a name="request-quota-increase-at-subscription-level"></a>Anfordern einer Erhöhung des Kontingents auf Abonnementebene

1. Klicken Sie unter https://portal.azure.com auf **Abonnements**.

   ![Abonnements](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

   ![Wählen Sie das Abonnement aus.](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Wählen Sie **Nutzung + Kontingente** aus.

   ![Auswählen von „Nutzung + Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Klicken Sie in der Ecke oben rechts auf **Erhöhung anfordern**.

   ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

5. Schritt 1: Wählen Sie **Kerne** als Kontingenttyp aus. 

   ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/forms.png)
   
6. Schritt 2: Wählen Sie als Bereitstellungsmodell „Resource Manager“ und anschließend einen Speicherort aus.

    ![Blatt für das Kontingentproblem](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Wählen Sie die SKU-Familien aus, für die eine Erhöhung erforderlich ist.

    ![Ausgewählte SKU-Serie](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Geben Sie die gewünschten Grenzwerte für das Abonnement ein.

    ![Neue SKU-Kontingentanforderung](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol.
Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie auf der Problemschrittseite auf „Weiter“, um die Erstellung der Supportanfrage fortzusetzen.

