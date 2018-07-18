---
title: Hinzufügen eines Kubernetes-Clusters zum Azure Stack-Marketplace | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie dem Azure Stack-Marketplace einen Kubernetes-Cluster hinzufügen.
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
ms.date: 05/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 0ba0e1f3d9e0f1cbb6ba4109a21fc29dc41df5fc
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34603452"
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Hinzufügen eines Kubernetes-Clusters zum Azure Stack-Marketplace

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!note]  
> Azure Container Services (ACS) Kubernetes in Azure Stack befindet sich in der privaten Vorschau. Um Zugriff auf das Marketplace-Element für Kubernetes anzufordern, um die Anweisungen in diesem Artikel ausführen zu können, [übermitteln Sie eine Anforderung zum Zugriff](https://aka.ms/azsk8).

Sie können einen Kubernetes-Cluster als ein Marketplace-Element für Ihre Benutzer anbieten. Ihre Benutzer können Kubernetes in einem einzelnen, koordinierten Vorgang bereitstellen.

Im folgenden Artikel wird das Bereitstellen der Ressourcen für einen eigenständigen Kubernetes-Cluster mithilfe einer Azure Resource Manager-Vorlage betrachtet. Bevor Sie beginnen, überprüfen Sie Ihren Azure Stack und Ihre globalen Azure-Mandanteneinstellungen. Sammeln Sie die erforderlichen Informationen zu Ihrem Azure Stack. Fügen Sie Ihrem Mandanten und dem Azure Stack Marketplace erforderliche Ressourcen hinzu. Der Cluster ist von einem Ubuntu-Server, einem benutzerdefinierten Skript und davon, dass die Kubernetes-Clusterelemente sich im Marketplace befinden, abhängig.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Erstellen eines Plans, eines Angebots und eines Abonnements

Erstellen Sie einen Plan, ein Angebot und ein Abonnement für das Marketplace-Element für den Kubernetes-Cluster. Sie können auch einen vorhandenen Plan und ein vorhandenes Angebot verwenden.

1. Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external) an.

2. Erstellen Sie einen Plan als Basisplan. Anweisungen hierzu finden Sie unter [Erstellen von Plänen in Azure Stack](azure-stack-create-plan.md).

3. Erstellen Sie ein Angebot. Anweisungen hierzu finden Sie unter [Erstellen von Angeboten in Azure Stack](azure-stack-create-offer.md).

4. Wählen Sie **Angebote** aus, und suchen Sie das Angebot, das Sie erstellt haben.

5. Wählen Sie auf dem Blatt „Angebot“ die Option **Übersicht** aus.

6. Wählen Sie **Status ändern** aus. Wählen Sie **Öffentlich** aus.

7. Wählen Sie **+ Neu** > **Offers and Plans (Angebote und Pläne)** > **Abonnement** aus, um ein neues Abonnement zu erstellen.

    a. Geben Sie einen **Anzeigenamen** ein.

    b. Geben Sie einen **Benutzer** ein. Verwenden Sie das Azure AD-Konto, das mit Ihrem Mandanten verknüpft ist.

    c. **Anbieterbeschreibung**

    d. Legen Sie **Verzeichnismandant** auf den Azure AD-Mandanten für Ihren Azure Stack fest. 

    e. Wählen Sie **Angebot** aus. Wählen Sie den Namen des Angebots aus, das Sie erstellt haben. Notieren Sie sich die Abonnement-ID.

## <a name="add-an-ubuntu-server-image"></a>Fügen Sie ein Ubuntu-Serverimage hinzu.

Fügen Sie dem Marketplace das folgende Ubuntu-Serverimage hinzu:

1. Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external) an.

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung).

3. Wählen Sie **+ Add from Azure** (+ Aus Azure hinzufügen) aus.

4. Geben Sie `UbuntuServer` ein.

5. Wählen Sie den Server mit dem folgenden Profil aus:
    - **Herausgeber:** Canonical
    - **Angebot:** UbuntuServer
    - **SKU:** 16.04-LTS
    - **Version:** 16.04.201802220

    > [!Note]  
    > Möglicherweise werden mehrere Versionen von Ubuntu Server 16.04 LTS aufgeführt. Sie müssen die übereinstimmende Version hinzufügen. Für den Kubernetes-Cluster ist die exakte Version des Elements erforderlich.

6. Wählen Sie **Herunterladen** aus.

## <a name="add-a-custom-script-for-linux"></a>Hinzufügen eines benutzerdefinierten Skripts für Linux

Fügen Sie den Kubernetes-Cluster aus dem Marketplace hinzu:

1. Öffnen Sie das [Verwaltungsportal](https://adminportal.local.azurestack.external).

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung).

3. Wählen Sie **+ Add from Azure** (+ Aus Azure hinzufügen) aus.

4. Geben Sie `Custom Script for Linux` ein.

5. Wählen Sie das Skript mit dem folgenden Profil aus:
    - **Angebot:** benutzerdefiniertes Skript für Linux 2.0
    - **Version:** 2.0.3
    - **Herausgeber:** Microsoft Corp

    > [!Note]  
    > Möglicherweise werden mehrere Versionen des benutzerdefinierten Skripts für Linux aufgeführt. Sie müssen die übereinstimmende Version hinzufügen. Für den Kubernetes-Cluster ist die exakte Version des Elements erforderlich.

6. Wählen Sie **Herunterladen** aus.


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Hinzufügen des Kubernetes-Clusters zum Marketplace

1. Öffnen Sie das [Verwaltungsportal](https://adminportal.local.azurestack.external).

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung).

3. Wählen Sie **+ Add from Azure** (+ Aus Azure hinzufügen) aus.

4. Geben Sie `Kubernetes Cluster` ein.

5. Wählen Sie `Kubernetes Cluster` aus.

6. Wählen Sie **Herunterladen** aus.

    > [!note]  
    > Es kann fünf Minuten dauern, bis das Marketplace-Element im Marketplace angezeigt wird.

    ![Kubernetes-Cluster](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Aktualisieren oder Entfernen des Kubernetes-Clusters 

Beim Aktualisieren des Elements für den Kubernetes-Cluster müssen Sie das Element entfernen, das sich im Marketplace befindet. Anschließend können Sie die Anweisungen in diesem Artikel zum Hinzufügen des Kubernetes-Clusters zum Marketplace befolgen.

So entfernen Sie das Element für den Kubernetes-Cluster

1. Notieren Sie sich den Namen des aktuellen Elements, z.B. `Microsoft.AzureStackKubernetesCluster.0.1.0`.

2. Stellen Sie eine Verbindung mit Azure Stack über PowerShell her.

3. Verwenden Sie das folgende PowerShell-Cmdlet, um das Element zu entfernen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines Kubernetes-Clusters in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Übersicht über das Anbieten von Diensten in Azure Stack](azure-stack-offer-services-overview.md)