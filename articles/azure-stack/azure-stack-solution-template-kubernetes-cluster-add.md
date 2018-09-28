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
ms.date: 09/25/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 69bf788ef30a18bbe70e251fdd6a814d0f528f55
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994563"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Hinzufügen von Kubernetes zum Azure Stack-Marketplace

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!note]  
> Kubernetes in Azure Stack befindet sich in der Vorschauphase. Um Zugriff auf das Marketplace-Element für den Kubernetes-Cluster anzufordern, um die Anweisungen in diesem Artikel ausführen zu können, [übermitteln Sie eine Zugriffsanforderung](https://aka.ms/azsk8).

Sie können Kubernetes als ein Marketplace-Element für Ihre Benutzer anbieten. Ihre Benutzer können Kubernetes in einem einzelnen, koordinierten Vorgang bereitstellen.

Im folgenden Artikel wird das Bereitstellen der Ressourcen für einen eigenständigen Kubernetes-Cluster mithilfe einer Azure Resource Manager-Vorlage betrachtet. Bevor Sie beginnen, überprüfen Sie Ihren Azure Stack und Ihre globalen Azure-Mandanteneinstellungen. Sammeln Sie die erforderlichen Informationen zu Ihrem Azure Stack. Fügen Sie Ihrem Mandanten und dem Azure Stack Marketplace erforderliche Ressourcen hinzu. Der Cluster ist von einem Ubuntu-Server, einem benutzerdefinierten Skript und davon, dass die Kubernetes-Elemente sich im Marketplace befinden, abhängig.

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

1. Wählen Sie die neueste Version des Servers mit dem folgenden Profil aus:
    - **Herausgeber:** Canonical
    - **Angebot:** UbuntuServer
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

1. Notieren Sie sich den Namen des aktuellen Elements, z.B. `Microsoft.AzureStackKubernetesCluster.0.2.0`.

1. Stellen Sie eine Verbindung mit Azure Stack über PowerShell her.

1. Verwenden Sie das folgende PowerShell-Cmdlet, um das Element zu entfernen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.2.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Kubernetes in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Übersicht über das Anbieten von Diensten in Azure Stack](azure-stack-offer-services-overview.md)