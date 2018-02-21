---
title: Erstellen von Angeboten in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie als Cloudadministrator ein Angebot für Ihre Benutzer in Azure Stack erstellen."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 2666aacbbaf44ae9b3bcf2df480e835457e6f449
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Erstellen von Angeboten in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

[Angebote](azure-stack-key-features.md) sind Gruppen mit einem oder mehreren Plänen, die Anbieter Benutzern zum Erwerben oder Abonnieren anbieten. Dieses Dokument zeigt Ihnen, wie Sie ein Angebot erstellen, das den im letzten Schritt [von Ihnen erstellten Plan](azure-stack-create-plan.md) enthält. Durch dieses Angebot erhalten Abonnenten die Möglichkeit, virtuelle Computer bereitzustellen.

1. Melden Sie sich beim Azure Stack-Administratorportal an (https://adminportal.local.azurestack.external) > klicken Sie auf **Neu** > **Tenant Offers + Plans** (Mandantenangebote und Pläne)  >  **Angebot**.

   ![](media/azure-stack-create-offer/image01.png)
2. Geben Sie im Bereich **Neues Angebot** den **Anzeigenamen** und den **Ressourcennamen** an, und wählen Sie anschließend eine neue oder vorhandene **Ressourcengruppe** aus. Der Anzeigename ist der verständliche Anzeigename des Angebots. Dieser Anzeigename ist die einzige Information zum Angebot, die Benutzern beim Abonnieren angezeigt wird. Verwenden Sie daher einen intuitiven Namen, anhand dessen die Benutzer wissen, was im Angebot enthalten ist. Nur der Administrator kann den Ressourcennamen einsehen. Es handelt sich um den Namen, mit dem Administratoren das Angebot als Azure-Ressourcen-Manager-Ressource bearbeiten.

   ![](media/azure-stack-create-offer/image01a.png)
3. Klicken Sie auf **Basispläne**, um den Bereich **Plan** zu öffnen, und wählen Sie die Pläne aus, die im Angebot enthalten sein sollen. Klicken Sie anschließend auf **Auswählen**. Klicken Sie auf **Erstellen** , um das Angebot zu erstellen.

   ![](media/azure-stack-create-offer/image02.png)
4. Klicken Sie auf **Alle Ressourcen**, suchen Sie Ihr neues Angebot, und klicken Sie dann zuerst auf das neue Angebot, dann auf **Status ändern** und anschließend auf **Öffentlich**.

   ![](media/azure-stack-create-offer/image03.png)

Angebote müssen öffentlich gemacht werden, damit Benutzer beim Abonnieren die vollständige Ansicht erhalten. Angebote können Folgendes sein:

* **Öffentlich:** Für Benutzer sichtbar
* **Privat:** nur für die Cloudadministratoren sichtbar. Dies ist hilfreich beim Entwerfen des Plans oder Angebots, oder wenn der Cloudadministrator [jedes Abonnement für Benutzer erstellen](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator) möchte.
* **Außer Betrieb:**für neue Abonnenten geschlossen. Der Cloudadministrator kann mit dem Status „Außer Betrieb“ zukünftige Abonnements verhindern und derzeitige Abonnenten unverändert lassen.

Änderungen am Angebot sind für den Benutzer nicht sofort sichtbar. Damit Ihnen die Änderungen angezeigt werden, müssen Sie sich möglicherweise ab- und wieder anmelden, damit das neue Abonnement in der Abonnementauswahl beim Erstellen von Ressourcen/Ressourcengruppen angezeigt wird.

> [!NOTE]
>Sie können mithilfe von PowerShell auch Standardangebote, -pläne und -kontingente erstellen. Informationen dazu finden Sie in der [Infodatei für Azure Stack-Dienstadministratoren](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Nächste Schritte
[Erstellen von Abonnements](azure-stack-subscribe-plan-provision-vm.md)      
[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)
