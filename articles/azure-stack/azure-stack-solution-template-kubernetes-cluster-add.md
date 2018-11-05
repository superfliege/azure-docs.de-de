---
title: Hinzufügen von Kubernetes zum Azure Stack-Marketplace | Microsoft Docs
description: Erfahren Sie, wie Sie dem Azure Stack-Marketplace Kubernetes hinzufügen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 5a3d63637d7b680a012057b92546ccde87ac73de
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233360"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Hinzufügen von Kubernetes zum Azure Stack-Marketplace

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!note]  
> Kubernetes in Azure Stack befindet sich in der Vorschauphase.

Sie können Kubernetes als ein Marketplace-Element für Ihre Benutzer anbieten. Ihre Benutzer können Kubernetes in einem einzelnen, koordinierten Vorgang bereitstellen.

Im folgenden Artikel wird das Bereitstellen der Ressourcen für einen eigenständigen Kubernetes-Cluster mithilfe einer Azure Resource Manager-Vorlage betrachtet. Für das Marketplace-Element Kubernetes-Cluster 0.3.0 ist Azure Stack Version 1808 erforderlich. Bevor Sie beginnen, überprüfen Sie Ihren Azure Stack und Ihre globalen Azure-Mandanteneinstellungen. Sammeln Sie die erforderlichen Informationen zu Ihrem Azure Stack. Fügen Sie Ihrem Mandanten und dem Azure Stack Marketplace erforderliche Ressourcen hinzu. Der Cluster ist von einem Ubuntu-Server, einem benutzerdefinierten Skript und davon, dass die Kubernetes-Elemente sich im Marketplace befinden, abhängig.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Erstellen eines Plans, eines Angebots und eines Abonnements

Erstellen Sie einen Plan, ein Angebot und ein Abonnement für das Marketplace-Element für Kubernetes. Sie können auch einen vorhandenen Plan und ein vorhandenes Angebot verwenden.

1. Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external) an.

1. Erstellen Sie einen Plan als Basisplan. Anweisungen hierzu finden Sie unter [Erstellen von Plänen in Azure Stack](azure-stack-create-plan.md).

1. Erstellen Sie ein Angebot. Anweisungen hierzu finden Sie unter [Erstellen von Angeboten in Azure Stack](azure-stack-create-offer.md).

1. Wählen Sie **Angebote** aus, und suchen Sie das Angebot, das Sie erstellt haben.

1. Wählen Sie auf dem Blatt „Angebot“ die Option **Übersicht** aus.

1. Wählen Sie **Status ändern** aus. Wählen Sie **Öffentlich** aus.

1. Wählen Sie **Ressource erstellen** > **Angebote und Pläne** > **Abonnement** aus, um ein neues Abonnement zu erstellen.

    a. Geben Sie einen **Anzeigenamen** ein.

    b. Geben Sie einen **Benutzer** ein. Verwenden Sie das Azure AD-Konto, das mit Ihrem Mandanten verknüpft ist.

    c. **Anbieterbeschreibung**

    d. Legen Sie **Verzeichnismandant** auf den Azure AD-Mandanten für Ihren Azure Stack fest. 

    e. Wählen Sie **Angebot** aus. Wählen Sie den Namen des Angebots aus, das Sie erstellt haben. Notieren Sie sich die Abonnement-ID.

## <a name="add-an-ubuntu-server-image"></a>Fügen Sie ein Ubuntu-Serverimage hinzu.

Fügen Sie dem Marketplace das folgende Ubuntu-Serverimage hinzu:

1. Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external) an.

1. Wählen Sie **Alle Dienste** aus, und wählen Sie dann unter der Kategorie **VERWALTUNG** die Option **Marketplace-Verwaltung** aus.

1. Wählen Sie **+ Add from Azure** (+ Aus Azure hinzufügen) aus.

1. Geben Sie `UbuntuServer` ein.

1. Wählen Sie die neueste Version des Servers aus. Überprüfen Sie die vollständige Version, und stellen Sie sicher, dass Sie über die neueste Version verfügen:
    - **Herausgeber:** Canonical
    - **Angebot:** UbuntuServer
    - **Version**: 16.04.201806120
    - **SKU:** 16.04-LTS

1. Wählen Sie **Herunterladen** aus.

## <a name="add-a-custom-script-for-linux"></a>Hinzufügen eines benutzerdefinierten Skripts für Linux

Fügen Sie Kubernetes aus dem Marketplace hinzu:

1. Öffnen Sie das [Verwaltungsportal](https://adminportal.local.azurestack.external).

1. Wählen Sie **Alle Dienste** aus, und wählen Sie dann unter der Kategorie **VERWALTUNG** die Option **Marketplace-Verwaltung** aus.

1. Wählen Sie **+ Add from Azure** (+ Aus Azure hinzufügen) aus.

1. Geben Sie `Custom Script for Linux` ein.

1. Wählen Sie das Skript mit dem folgenden Profil aus:
    - **Angebot:** benutzerdefiniertes Skript für Linux 2.0
    - **Version**: 2.0.6
    - **Herausgeber:** Microsoft Corp

    > [!Note]  
    > Möglicherweise werden mehrere Versionen des benutzerdefinierten Skripts für Linux aufgeführt. Sie müssen die übereinstimmende Version hinzufügen. Für Kubernetes ist die exakte Version des Elements erforderlich.

1. Wählen Sie **Herunterladen** aus.


## <a name="add-kubernetes-to-the-marketplace"></a>Hinzufügen von Kubernetes zum Marketplace

1. Öffnen Sie das [Verwaltungsportal](https://adminportal.local.azurestack.external).

1. Wählen Sie **Alle Dienste** aus, und wählen Sie dann unter der Kategorie **VERWALTUNG** die Option **Marketplace-Verwaltung** aus.

1. Wählen Sie **+ Add from Azure** (+ Aus Azure hinzufügen) aus.

1. Geben Sie `Kubernetes` ein.

1. Wählen Sie `Kubernetes Cluster` aus.

1. Wählen Sie **Herunterladen** aus.

    > [!note]  
    > Es kann fünf Minuten dauern, bis das Marketplace-Element im Marketplace angezeigt wird.

    ![kubernetes](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Aktualisieren oder Entfernen von Kubernetes 

Beim Aktualisieren des Elements für Kubernetes müssen Sie das Element entfernen, das sich im Marketplace befindet. Anschließend können Sie die Anweisungen in diesem Artikel zum Hinzufügen von Kubernetes zum Marketplace befolgen.

So entfernen Sie das Kubernetes-Element:

1. Stellen Sie als Operator eine Verbindung mit Azure Stack über PowerShell her. Anleitungen dazu finden Sie unter [Herstellen einer Verbindung mit Azure Stack über PowerShell als Operator](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Suchen Sie im Katalog nach dem aktuellen Element „Kubernetes-Cluster“.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Notieren Sie sich den Namen des aktuellen Elements, z.B. `Microsoft.AzureStackKubernetesCluster.0.2.0`.

4. Verwenden Sie das folgende PowerShell-Cmdlet, um das Element zu entfernen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Kubernetes in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Übersicht über das Anbieten von Diensten in Azure Stack](azure-stack-offer-services-overview.md)