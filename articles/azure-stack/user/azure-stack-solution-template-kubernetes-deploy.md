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
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 7cf865f0ce75d8308d6d42306e8e05852f763cae
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Bereitstellen eines Kubernetes-Clusters in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!Note]  
> Azure Container Services (ACS) Kubernetes in Azure Stack befindet sich in der privaten Vorschau. Ihr Azure Stack-Bediener muss Zugriff auf das Marketplace-Element für Kubernetes anfordern, um die Anweisungen in diesem Artikel auszuführen.

Im folgenden Artikel wird die Verwendung einer Azure Resource Manager-Lösungsvorlage zum Bereitstellen der Ressourcen für Kubernetes in einem einzelnen, koordinierten Vorgang erläutert. Sie müssen die erforderlichen Informationen zu Ihrer Azure Stack-Installation sammeln, die Vorlage generieren und dann die Bereitstellung in der Cloud ausführen.

## <a name="kubernetes-and-containers"></a>Kubernetes und Container

Sie können Azure Container Services (ACS) Kubernetes in Azure Stack installieren. [Kubernetes](https://kubernetes.io) ist ein Open Source-System für die Automatisierung der Bereitstellung, Skalierung und Verwaltung von Anwendungen in Containern. Ein [Container](https://www.docker.com/what-container) ist ähnlich wie ein virtueller Computer in einem Image enthalten. Im Gegensatz zu einem virtuellen Computer enthält das Containerimage jedoch nur die Ressourcen, die es zum Ausführen einer Anwendung benötigt, etwa den Code, die Runtime zum Ausführen des Codes, bestimmte Bibliotheken und Einstellungen.

Sie können Kubernetes für Folgendes verwenden:

- Entwickeln hochgradig skalierbarer, aktualisierbarer Anwendungen, die in wenigen Sekunden bereitgestellt werden können 
- Vereinfachen des Entwurfs Ihrer Anwendung und Verbessern ihrer Zuverlässigkeit durch verschiedene Helm-Anwendungen. [Helm](https://github.com/kubernetes/helm) ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten.
- Einfaches Überwachen und Diagnostizieren der Integrität Ihrer Anwendungen mit Skalierungs- und Upgradefunktionen

## <a name="prerequisites"></a>Voraussetzungen 

Stellen Sie zum Einstieg sicher, dass Sie über die erforderlichen Berechtigungen verfügen und dass Ihre Azure Stack-Instanz vorbereitet ist.

1. Überprüfen Sie, ob Sie Anwendungen in Ihrem Azure Active Directory-Mandanten (Azure AD) erstellen können. Sie benötigen diese Berechtigungen für die Kubernetes-Bereitstellung.

    Anweisungen zum Überprüfen Ihrer Berechtigungen finden Sie unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

2. Generieren Sie ein öffentlich-privates SSH-Schlüsselpaar für die Anmeldung beim virtuellen Linux-Computer in Azure Stack. Sie benötigen den öffentlichen Schlüssel bei der Clustererstellung.

    Anweisungen zum Generieren eines Schlüssels finden Sie unter [SSH Key Generation](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation) (SSH-Schlüsselgenerierung).

3. Stellen Sie sicher, dass Sie ein gültiges Abonnement im Azure Stack-Mandantenportal besitzen und ausreichend öffentliche IP-Adressen zum Hinzufügen neuer Anwendungen verfügbar sind.

## <a name="create-a-service-principal-in-azure-ad"></a>Erstellen eines Dienstprinzipals in Azure AD

1. Melden Sie sich beim globalen [Azure-Portal](http://www.poartal.azure.com) an.
2. Sie müssen mit dem Azure Stack zugeordneten Azure AD-Mandanten angemeldet sein.
3. Erstellen Sie eine Azure AD-Anwendung.

    a. Wählen Sie **Azure Active Directory** > **+ App-Registrierungen** > **Registrierung einer neuen Anwendung** aus.

    b. Geben Sie unter **Name** einen Namen für die Anwendung ein.

    c. Klicken Sie auf **Web-App/API**.

    d. Geben Sie `http://localhost` als **Anmelde-URL** ein.

    c. Klicken Sie auf **Erstellen**

4. Notieren Sie den Wert der **Anwendungs-ID**. Sie benötigen die ID bei der Clustererstellung. Die ID wird als **Client-ID des Dienstprinzipals** bezeichnet.

5. Wählen Sie **Einstellungen** > **Schlüssel** aus.

    a. Geben Sie die **Beschreibung** ein.

    b. Wählen Sie unter **Gültig bis** die Option **Läuft nie ab** aus.

    c. Wählen Sie **Speichern**aus. Notieren Sie sich die Schlüsselzeichenfolge. Sie benötigen die Schlüsselzeichenfolge bei der Clustererstellung. Der Schlüssel wird als **Clientgeheimnis des Dienstprinzipals** bezeichnet.



## <a name="give-the-service-principal-access"></a>Erteilen des Zugriffs für den Dienstprinzipal

Erteilen Sie dem Dienstprinzipal Zugriff auf Ihr Abonnement, sodass er Ressourcen erstellen kann.

1.  Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external) an.

2. Klicken Sie auf **Weitere Dienste** > **Benutzerabonnements** > **+ Hinzufügen**.

3. Wählen Sie das von Ihnen erstellte Abonnement aus.

4. Klicken Sie auf **Zugriffssteuerung (IAM)** > **+ Hinzufügen**.

5. Wählen Sie die Rolle **Besitzer**.

6. Wählen Sie den für Ihren Dienstprinzipal erstellten Anwendungsnamen aus. Unter Umständen müssen Sie den Namen in das Suchfeld eingeben.

7. Klicken Sie auf **Speichern**.

## <a name="deploy-a-kubernetes-cluster"></a>Bereitstellen eines Kubernetes-Clusters

1. Öffnen Sie das [Azure Stack-Portal](https://portal.local.azurestack.external).

2. Klicken Sie auf **+Neu** > **Compute** > **Kubernetes-Cluster**.

    ![Bereitstellen einer Lösungsvorlage](../media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template.png)

3. Klicken Sie unter „Lösungsvorlage bereitstellen“ auf **Parameter**.

    ![Bereitstellen einer Lösungsvorlage](../media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template-parameters.png)

2. Geben Sie den **Linux-Administratorbenutzernamen** ein. Hierbei handelt es sich um den Benutzernamen für die virtuellen Linux-Computer, die Teil des Kubernetes-Clusters und des DVM (Deployment Virtual Machine, virtueller Computer für die Bereitstellung) sind.

3. Geben Sie den **öffentlichen SSH-Schlüssel** für die Autorisierung bei allen Linux-Computern ein, die als Teil des Kubernetes-Clusters und des DVM erstellt werden.

4. Geben Sie den **Mandantenendpunkt** ein. Dabei handelt es sich um den Azure Resource Manager-Endpunkt für die Verbindungsherstellung, um die Ressourcengruppe für den Kubernetes-Cluster zu erstellen. Bei einem integrierten System müssen Sie den Endpunkt vom Azure Stack-Bediener erfragen. Für das Azure Stack Development Kit (ASDK) können Sie `https://management.local.azurestack.external` verwenden.

5. Geben Sie die **Mandanten-ID** für den Mandanten ein. Wenn Sie Unterstützung beim Ermitteln dieses Werts benötigen, lesen Sie die Informationen unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

6. Geben Sie das für die Region eindeutige **DNS-Präfix des Masterprofils** ein. Diese Angabe muss ein für die Region eindeutiger Name sein, beispielsweise `k8s-12345`. Es wird empfohlen, den gleichen Namen wie für die Ressourcengruppe zu wählen.

    > [!Note]  
    > Verwenden Sie für jeden Cluster ein neues, eindeutiges DNS-Präfix des Masterprofils.

7. Geben Sie die Anzahl von Agents im Cluster ein. Dieser Wert wird als **Agent Pool Profile Count** (Agentpool-Profilanzahl) bezeichnet. Sie können einen Wert von 1 bis 32 angeben.

8. Geben Sie die **Dienstprinzipal-Anwendungs-ID** ein. Sie wird vom Kubernetes Azure-Cloudanbieter verwendet.

9. Geben Sie das **Clientgeheimnis des Dienstprinzipals** ein, das Sie beim Erstellen der Dienstprinzipalanwendung generiert haben.

10. Geben Sie die **Version des Kubernetes Azure Cloudanbieters** ein. Dabei handelt es sich um die Version des Kubernetes Azure-Anbieters. Azure Stack veröffentlicht für jede Azure Stack-Version einen benutzerdefinierten Kubernetes-Build.

12. Klicken Sie auf **OK**.

### <a name="specify-the-solution-values"></a>Angeben der Lösungswerte

1. Wählen Sie das **Abonnement** aus.

2. Geben Sie den Namen einer neuen Ressourcengruppe ein, oder wählen Sie eine vorhandene Ressourcengruppe aus. Der Ressourcenname muss alphanumerisch und in Kleinbuchstaben angegeben sein.

3. Geben Sie den Standort der Ressourcengruppe ein, etwa **lokal**.

4. Klicken Sie auf **Erstellen**.

## <a name="connect-to-your-cluster"></a>Herstellen einer Clusterverbindung

Sie können nun eine Verbindung mit Ihrem Cluster herstellen. Sie benötigen den Kubernetes-Befehlszeilenclient **kubectl**. Anweisungen zum Herstellen einer Verbindung mit dem Cluster sowie zur Verwaltung des Clusters finden Sie in der Azure Container Services-Dokumentation.   

Anweisungen hierzu finden Sie unter [Bereitstellen eines Kubernetes-Clusters für Linux-Container](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough#connect-to-the-cluster).

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines Kubernetes-Clusters zum Marketplace (für Azure Stack-Bediener)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes in Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)