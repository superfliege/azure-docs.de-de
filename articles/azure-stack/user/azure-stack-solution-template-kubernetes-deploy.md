---
title: Bereitstellen eines Kubernetes-Clusters in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Kubernetes-Cluster in Azure Stack bereitstellen.
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
ms.date: 08/09/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: f6a2b3f242e5989d0c72083eef4faad9c4798cfe
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "41946608"
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Bereitstellen eines Kubernetes-Clusters in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!Note]  
> Azure Kubernetes Services (AKS) in Azure Stack befindet sich in der privaten Vorschau. Ihr Azure Stack-Bediener muss Zugriff auf das Marketplace-Element für Kubernetes anfordern, um die Anweisungen in diesem Artikel auszuführen.

Im folgenden Artikel wird die Verwendung einer Azure Resource Manager-Lösungsvorlage zum Bereitstellen der Ressourcen für Kubernetes in einem einzelnen, koordinierten Vorgang erläutert. Sie müssen die erforderlichen Informationen zu Ihrer Azure Stack-Installation sammeln, die Vorlage generieren und dann die Bereitstellung in der Cloud ausführen.

## <a name="kubernetes-and-containers"></a>Kubernetes und Container

Sie können Kubernetes mithilfe von Azure Resource Manager-Vorlagen, die mit der Azure Kubernetes Services-Engine (AKS) generiert wurden, in Azure Stack installieren. [Kubernetes](https://kubernetes.io) ist ein Open Source-System für die Automatisierung der Bereitstellung, Skalierung und Verwaltung von Anwendungen in Containern. Ein [Container](https://www.docker.com/what-container) ist ähnlich wie ein virtueller Computer in einem Image enthalten. Im Gegensatz zu einem virtuellen Computer enthält das Containerimage jedoch nur die Ressourcen, die zum Ausführen einer Anwendung benötigt werden, etwa den Code, die Runtime zum Ausführen des Codes, bestimmte Bibliotheken und Einstellungen.

Sie können Kubernetes für Folgendes verwenden:

- Entwickeln hochgradig skalierbarer, aktualisierbarer Anwendungen, die in wenigen Sekunden bereitgestellt werden können 
- Vereinfachen des Entwurfs Ihrer Anwendung und Verbessern ihrer Zuverlässigkeit durch verschiedene Helm-Anwendungen. [Helm](https://github.com/kubernetes/helm) ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten.
- Einfaches Überwachen und Diagnostizieren der Integrität Ihrer Anwendungen mit Skalierungs- und Upgradefunktionen

## <a name="prerequisites"></a>Voraussetzungen 

Stellen Sie zum Einstieg sicher, dass Sie über die erforderlichen Berechtigungen verfügen und dass Ihre Azure Stack-Instanz vorbereitet ist.

1. Überprüfen Sie, ob Sie Anwendungen in Ihrem Azure Active Directory-Mandanten (Azure AD) erstellen können. Sie benötigen diese Berechtigungen für die Kubernetes-Bereitstellung.

    Anweisungen zum Überprüfen Ihrer Berechtigungen finden Sie unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

1. Generieren Sie ein öffentlich-privates SSH-Schlüsselpaar für die Anmeldung beim virtuellen Linux-Computer in Azure Stack. Sie benötigen den öffentlichen Schlüssel bei der Clustererstellung.

    Anweisungen zum Generieren eines Schlüssels finden Sie unter [SSH Key Generation](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation) (SSH-Schlüsselgenerierung).

1. Stellen Sie sicher, dass Sie ein gültiges Abonnement im Azure Stack-Mandantenportal besitzen und ausreichend öffentliche IP-Adressen zum Hinzufügen neuer Anwendungen verfügbar sind.

    Der Cluster kann nicht in einem Azure Stack-Abonnement vom Typ **Administrator** bereitgestellt werden. Sie müssen ein Abonnement des Typs Benutzer verwenden. 

## <a name="create-a-service-principal-in-azure-ad"></a>Erstellen eines Dienstprinzipals in Azure AD

1. Melden Sie sich beim globalen [Azure-Portal](http://portal.azure.com) an.
1. Sie müssen mit dem der Azure Stack-Instanz zugeordneten Azure AD-Mandanten angemeldet sein.
1. Erstellen Sie eine Azure AD-Anwendung.

    a. Wählen Sie **Azure Active Directory** > **+ App-Registrierungen** > **Registrierung einer neuen Anwendung** aus.

    b. Geben Sie unter **Name** einen Namen für die Anwendung ein.

    c. Wählen Sie **Web-App/API** aus.

    d. Geben Sie `http://localhost` als **Anmelde-URL** ein.

    c. Klicken Sie auf **Create**.

1. Notieren Sie den Wert der **Anwendungs-ID**. Sie benötigen die ID bei der Clustererstellung. Die ID wird als **Client-ID des Dienstprinzipals** bezeichnet.

1. Wählen Sie **Einstellungen** > **Schlüssel** aus.

    a. Geben Sie die **Beschreibung** ein.

    b. Wählen Sie unter **Gültig bis** die Option **Läuft nie ab** aus.

    c. Wählen Sie **Speichern**aus. Notieren Sie sich die Schlüsselzeichenfolge. Sie benötigen die Schlüsselzeichenfolge bei der Clustererstellung. Der Schlüssel wird als **Clientgeheimnis des Dienstprinzipals** bezeichnet.



## <a name="give-the-service-principal-access"></a>Erteilen des Zugriffs für den Dienstprinzipal

Erteilen Sie dem Dienstprinzipal Zugriff auf Ihr Abonnement, sodass er Ressourcen erstellen kann.

1.  Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external) an.

1. Klicken Sie auf **Weitere Dienste** > **Benutzerabonnements** > **+ Hinzufügen**.

1. Wählen Sie das von Ihnen erstellte Abonnement aus.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** > **+ Hinzufügen**.

1. Wählen Sie die Rolle **Besitzer**.

1. Wählen Sie den für Ihren Dienstprinzipal erstellten Anwendungsnamen aus. Unter Umständen müssen Sie den Namen in das Suchfeld eingeben.

1. Klicken Sie auf **Speichern**.

## <a name="deploy-a-kubernetes-cluster"></a>Bereitstellen eines Kubernetes-Clusters

1. Öffnen Sie das [Azure Stack-Portal](https://portal.local.azurestack.external).

1. Klicken Sie auf **+Neu** > **Compute** > **Kubernetes-Cluster**. Klicken Sie auf **Create**.

    ![Bereitstellen einer Lösungsvorlage](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

1. Wählen Sie in „Kubernetes-Cluster erstellen“ die Option **Grundlagen** aus.

    ![Bereitstellen einer Lösungsvorlage](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Geben Sie unter **Linux VM Admin Username** (Benutzername des Linux-VM-Administrators) einen Namen ein. Hierbei handelt es sich um den Benutzernamen für die virtuellen Linux-Computer, die Teil des Kubernetes-Clusters und des DVM (Deployment Virtual Machine, virtueller Computer für die Bereitstellung) sind.

1. Geben Sie den **öffentlichen SSH-Schlüssel** für die Autorisierung bei allen Linux-Computern ein, die als Teil des Kubernetes-Clusters und des DVM erstellt werden.

1. Geben Sie das für die Region eindeutige **DNS-Präfix des Masterprofils** ein. Diese Angabe muss ein für die Region eindeutiger Name sein, beispielsweise `k8s-12345`. Es wird empfohlen, den gleichen Namen wie für die Ressourcengruppe zu wählen.

    > [!Note]  
    > Verwenden Sie für jeden Cluster ein neues, eindeutiges DNS-Präfix des Masterprofils.

1. Geben Sie unter **Agent Pool Profile Count** (Agentpool-Profilanzahl) einen Wert ein. Er gibt die Anzahl von Agents im Cluster an. Sie können einen Wert von 1 bis 4 angeben.

1. Geben Sie unter **Service Principal ClientId** (Dienstprinzipal-ClientId) einen Wert ein. Dieser wird vom Kubernetes Azure-Cloudanbieter verwendet.

1. Geben Sie unter **Service Principal Client Secret** das Clientgeheimnis des Dienstprinzipals ein, das Sie beim Erstellen der Dienstprinzipalanwendung generiert haben.

1. Geben Sie die **Version des Kubernetes Azure Cloudanbieters** ein. Dabei handelt es sich um die Version des Kubernetes Azure-Anbieters. Azure Stack veröffentlicht für jede Azure Stack-Version einen benutzerdefinierten Kubernetes-Build.

1. Wählen Sie die ID für Ihr **Abonnement** aus.

1. Geben Sie den Namen einer neuen Ressourcengruppe ein, oder wählen Sie eine vorhandene Ressourcengruppe aus. Der Ressourcenname muss alphanumerisch und in Kleinbuchstaben angegeben sein.

1. Wählen Sie den **Standort** der Ressourcengruppe aus. Dies ist die Region, die Sie für die Azure Stack-Installation auswählen.

### <a name="specify-the-azure-stack-settings"></a>Angeben der Einstellungen für Azure Stack

1. Wählen Sie **Azure Stack Stamp Settings** (Azure Stack-Stempeleinstellungen) aus.

    ![Bereitstellen einer Lösungsvorlage](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings.png)

1. Geben Sie unter **Tenant Arm Endpoint** (ARM-Mandantenendpunkt) den Endpunkt ein. Dabei handelt es sich um den Azure Resource Manager-Endpunkt für die Verbindungsherstellung, um die Ressourcengruppe für den Kubernetes-Cluster zu erstellen. Bei einem integrierten System müssen Sie den Endpunkt vom Azure Stack-Bediener erfragen. Für das Azure Stack Development Kit (ASDK) können Sie `https://management.local.azurestack.external` verwenden.

1. Geben Sie die **Mandanten-ID** für den Mandanten ein. Wenn Sie Unterstützung beim Ermitteln dieses Werts benötigen, lesen Sie die Informationen unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

## <a name="connect-to-your-cluster"></a>Herstellen einer Clusterverbindung

Sie können nun eine Verbindung mit Ihrem Cluster herstellen. Den Master mit dem Namen `k8s-master-<sequence-of-numbers>` finden Sie in Ihrer Clusterressourcengruppe. Verwenden Sie einen SSH-Client zum Herstellen einer Verbindung mit dem Master. Auf dem Master können Sie mit dem Kubernetes-Befehlszeilenclient **kubectl** Ihren Cluster verwalten. Anweisungen finden Sie unter [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Sie können zudem den **Helm**-Paket-Manager zum Installieren und Bereitstellen von Apps für Ihren Cluster verwenden. Anweisungen zur Installation und Verwendung von Helm mit Ihrem Cluster finden Sie unter [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines Kubernetes-Clusters zum Marketplace (für Azure Stack-Bediener)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes in Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
