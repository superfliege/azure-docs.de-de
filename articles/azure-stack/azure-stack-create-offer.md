---
title: Erstellen von Angeboten in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Cloudadministrator ein Angebot für Ihre Benutzer in Azure Stack erstellen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 230415b84f06beac549693d49055d7cb998163d8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251451"
---
# <a name="create-an-offer-in-azure-stack"></a>Erstellen von Angeboten in Azure Stack

[Angebote](azure-stack-key-features.md) sind Gruppen mit mindestens einem Plan, die Anbieter Benutzern zum Kauf oder in Form von Abonnements anbieten. In diesem Artikel wird beschrieben, wie Sie ein Angebot erstellen, das den [von Ihnen erstellten Plan](azure-stack-create-plan.md) enthält. Durch dieses Angebot erhalten Abonnenten die Möglichkeit, virtuelle Computer (VMs) einzurichten.

1. Melden Sie sich beim [Azure Stack-Administratorportal](https://adminportal.local.azurestack.external) an, und wählen Sie die Optionen **+ Ressource erstellen**, **Mandantenangebote + Pläne** und dann **Angebot**.

   ![Erstellen von Angeboten](media/azure-stack-create-offer/image01.png)
  
2. Geben Sie unter **Neues Angebot** einen Wert für **Anzeigename** und **Ressourcenname** ein, und klicken Sie anschließend unter **Ressourcenname** auf **Neu erstellen** oder **Vorhandenes Element verwenden**. Der Anzeigename ist der verständliche Anzeigename des Angebots. Dieser Anzeigename ist die einzige Information zum Angebot, die Benutzern beim Abonnieren eines Angebots angezeigt wird. Verwenden Sie einen aussagekräftigen Namen, der Aufschluss darüber gibt, was das Angebot umfasst. Nur der Administrator kann den Ressourcennamen anzeigen. Es handelt sich um den Namen, mit dem Administratoren das Angebot als Azure-Ressourcen-Manager-Ressource bearbeiten.

   ![Neues Angebot](media/azure-stack-create-offer/image01a.png)
  
3. Klicken Sie auf **Basispläne**, um den **Plan** zu öffnen. Wählen Sie die Pläne aus, die Sie in das Angebot einschließen möchten, und klicken Sie anschließend auf **Auswählen**. Klicken Sie zum Erstellen des Angebots auf **Erstellen**.

   ![Auswählen des Plans](media/azure-stack-create-offer/image02.png)
  
4. Nach dem Erstellen des Angebots können Sie dessen Zustand ändern. Angebote müssen **öffentlich** gemacht werden, damit Benutzer beim Abonnieren die vollständige Ansicht erhalten. Angebote können Folgendes sein:

   - **Öffentlich**: Für Benutzer sichtbar.
   - **Privat**: Nur für Cloudadministratoren sichtbar. Diese Einstellung ist hilfreich beim Entwerfen des Plans oder Angebots oder wenn der Cloudadministrator [jedes Abonnement für Benutzer erstellen](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator) möchte.
   - **Außer Betrieb**: Für neue Abonnenten geschlossen. Der Cloudadministrator kann Angebote außer Betrieb nehmen, um zukünftige Abonnements zu verhindern, ohne aktuelle Abonnenten zu beeinträchtigen.

   > [!TIP]  
   > Änderungen am Angebot sind für den Benutzer nicht sofort sichtbar. Um die Änderungen zu sehen, müssen sich Benutzer möglicherweise ab- und erneut beim Benutzerportal anmelden, damit das neue Angebot angezeigt wird.

   Klicken Sie im Übersichtsfenster für das Angebot auf **Status der Barrierefreiheit**. Wählen Sie den gewünschten Status aus (beispielsweise **Öffentlich**), und klicken Sie anschließend auf **Speichern**.

     ![Ändern des Status](media/azure-stack-create-offer/change-stage-1807.png)

     Klicken Sie alternativ auf **Status ändern**, und wählen Sie dann einen Status aus.

    ![Auswählen von „Status der Barrierefreiheit“](media/azure-stack-create-offer/change-stage-select-1807.png)

   > [!NOTE]
   > Sie können auch PowerShell verwenden, um Standardangebote, Pläne und Kontingente zu erstellen. Weitere Informationen finden Sie unter [Azure Stack-Modul 1.4.0](/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen von Abonnements](azure-stack-subscribe-plan-provision-vm.md)
- [Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)
