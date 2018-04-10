---
title: Erstellen von Angeboten in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Cloudadministrator ein Angebot für Ihre Benutzer in Azure Stack erstellen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2018
ms.author: brenduns
ms.openlocfilehash: 6a59d0c8144492ef907e5c395f05e4e8a16678a5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Erstellen von Angeboten in Azure Stack

[Angebote](azure-stack-key-features.md) sind Gruppen mit einem oder mehreren Plänen, die Anbieter Benutzern zum Erwerben oder Abonnieren anbieten. Dieses Dokument zeigt Ihnen, wie Sie ein Angebot erstellen, das den im letzten Schritt [von Ihnen erstellten Plan](azure-stack-create-plan.md) enthält. Durch dieses Angebot erhalten Abonnenten die Möglichkeit, virtuelle Computer bereitzustellen.

1. Melden Sie sich beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) an, und klicken Sie auf **Neu** > **Mandantenangebote + Pläne** > **Angebot**.

   ![](media/azure-stack-create-offer/image01.png)
2. Geben Sie im Bereich **Neues Angebot** den **Anzeigenamen** und den **Ressourcennamen** an, und wählen Sie anschließend eine neue oder vorhandene **Ressourcengruppe** aus. Der Anzeigename ist der verständliche Anzeigename des Angebots. Dieser Anzeigename ist die einzige Information zum Angebot, die Benutzern beim Abonnieren angezeigt wird. Verwenden Sie daher einen intuitiven Namen, anhand dessen die Benutzer wissen, was im Angebot enthalten ist. Nur der Administrator kann den Ressourcennamen einsehen. Es handelt sich um den Namen, mit dem Administratoren das Angebot als Azure-Ressourcen-Manager-Ressource bearbeiten.

   ![](media/azure-stack-create-offer/image01a.png)
3. Klicken Sie auf **Basispläne**, um den Bereich **Plan** zu öffnen, und wählen Sie die Pläne aus, die im Angebot enthalten sein sollen. Klicken Sie anschließend auf **Auswählen**. Klicken Sie auf **Erstellen** , um das Angebot zu erstellen.

   ![](media/azure-stack-create-offer/image02.png)
4. Nach dem Erstellen des Angebots können Sie dessen Zustand ändern. Angebote müssen *öffentlich* gemacht werden, damit Benutzer beim Abonnieren die vollständige Ansicht erhalten. Angebote können Folgendes sein:
   - **Öffentlich:** Für Benutzer sichtbar.
   - **Privat:** Nur für Cloudadministratoren sichtbar. Dies ist hilfreich beim Entwerfen des Plans oder Angebots, oder wenn der Cloudadministrator [jedes Abonnement für Benutzer erstellen](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator) möchte.
   - **Außer Betrieb:**für neue Abonnenten geschlossen. Der Cloudadministrator kann mit dem Status „Außer Betrieb“ zukünftige Abonnements verhindern und derzeitige Abonnenten unverändert lassen.

   > [!TIP]  
   > Änderungen am Angebot sind für den Benutzer nicht sofort sichtbar. Um die Änderungen zu sehen, müssen sich Benutzer möglicherweise abmelden und erneut beim Benutzerportal anmelden, damit das neue Angebot angezeigt wird. 

   So ändern Sie den Zustand des Angebots: 

   - **Ab Version 1803:**  
     Klicken Sie auf dem Übersichtsblatt für das Angebot auf **Status der Barrierefreiheit**, wählen Sie den gewünschten Zustand aus (beispielsweise *Öffentlich*), und klicken Sie anschließend auf **Speichern**. 
 
     ![Auswählen von „Status der Barrierefreiheit“](media/azure-stack-create-offer/change-state.png) 

     Alternativ können Sie nach dem Zugriff auf ein Angebot zu **Angebotseinstellungen** navigieren und auf **Status der Barrierefreiheit** klicken, um den Zustand zu ändern. 

   - **Vor Version 1803:**  
     Klicken Sie auf **Alle Ressourcen**, suchen Sie Ihr neues Angebot, und klicken Sie dann zuerst auf das neue Angebot, dann auf **Status ändern** und anschließend auf **Öffentlich**.

  
   > [!NOTE] 
   > Sie können auch PowerShell verwenden, um Standardangebote, Pläne und Kontingente zu erstellen. Weitere Informationen finden Sie in den [Informationen für Azure Stack-Dienstadministratoren](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
   >


### <a name="next-steps"></a>Nächste Schritte
[Erstellen von Abonnements](azure-stack-subscribe-plan-provision-vm.md)      
[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)
