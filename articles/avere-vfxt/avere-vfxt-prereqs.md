---
title: Voraussetzungen für Avere vFXT – Azure
description: Voraussetzungen für Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 823bf50a54ff43fa95f7136c137e3d8f3303c3e0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669527"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Vorbereiten der Avere vFXT-Erstellung

In diesem Artikel werden die Voraussetzungen für die Erstellung eines Avere vFXT-Clusters erläutert.

## <a name="create-a-new-subscription"></a>Erstellen eines neuen Abonnements

Beginnen Sie mit der Erstellung eines neuen Azure-Abonnements. Verwenden Sie ein separates Abonnement für jedes Avere vFXT-Projekt, damit Sie alle Projektressourcen und -kosten leicht nachverfolgen können, andere Projekte vor einer möglichen Ressourcenbegrenzung während der Bereitstellung schützen und die Bereinigung vereinfachen können.  

So erstellen Sie ein neues Azure-Abonnement im Azure-Portal

* Navigieren Sie zum Blatt [Abonnements](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
* Klicken Sie ganz oben auf die Schaltfläche **+ Hinzufügen**.
* Melden Sie sich an, wenn Sie dazu aufgefordert werden.
* Wählen Sie ein Angebot aus und führen Sie die Schritte durch, um ein neues Abonnement zu erstellen.

## <a name="configure-subscription-owner-permissions"></a>Konfigurieren der Berechtigungen des Abonnementbesitzers

Ein Benutzer mit Besitzerberechtigungen für das Abonnement sollte den vFXT-Cluster erstellen. Für diese Aktionen sind unter anderem Berechtigungen des Abonnementbesitzers erforderlich:

* Akzeptieren der Bedingungen für die Avere vFXT-Software
* Erstellen der Zugriffsrolle für Clusterknoten
* Zulassen der Verwaltung von Ressourcengruppen und virtuellen Netzwerken durch den Clustercontrollerknoten 
* Zulassen der Erstellung und Änderung von Clusterknoten durch den Clustercontroller 

Es gibt zwei Möglichkeiten, wenn Sie den Benutzern, die den vFXT erstellen, keinen Besitzerzugriff gewähren möchten:

* Ein Ressourcengruppenbesitzer kann einen Cluster erstellen, wenn die folgenden Bedingungen erfüllt sind:

  * Ein Abonnementbesitzer muss [die Nutzungsbedingungen für die Avere vFXT-Software](#accept-software-terms-in-advance) akzeptieren und [die Zugriffsrolle für Clusterknoten](avere-vfxt-deploy.md#create-the-cluster-node-access-role) erstellen.
  * Alle Avere vFXT-Ressourcen müssen innerhalb der Ressourcengruppe bereitgestellt werden, einschließlich:
    * Clustercontroller
    * Clusterknoten
    * Blob Storage
    * Netzwerkelemente
 
* Ein Benutzer ohne Besitzerberechtigungen kann vFXT-Cluster erstellen, wenn eine zusätzliche Zugriffsrolle erstellt und dem Benutzer vorab zugewiesen wird. Diese Rolle verleiht diesen Benutzern jedoch erhebliche Berechtigungen. [In diesem Artikel](avere-vfxt-non-owner.md) wird erläutert, wie Nicht-Besitzer für die Erstellung von Clustern autorisiert werden.

## <a name="quota-for-the-vfxt-cluster"></a>Kontingent für den vFXT-Cluster

Sie müssen über ein ausreichendes Kontingent für die folgenden Azure-Komponenten verfügen. Fordern Sie bei Bedarf eine [Kontingenterhöhung](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) an.

> [!NOTE]
> Die hier aufgeführten virtuellen Computer und SSD-Komponenten gelten für den vFXT-Cluster selbst. Sie benötigen zusätzliche Kontingente für die virtuellen Computer und SSDs, die Sie für Ihre Serverfarm verwenden möchten.  Stellen Sie sicher, dass das Kontingent für die Region aktiviert ist, in der Sie den Workflow ausführen möchten.

|Azure-Komponente|Kontingent|
|----------|-----------|
|Virtuelle Computer|Mindestens drei (D16s_v3 oder E32s_v3)|
|SSD Premium-Speicher|200 GB Speicherplatz für das Betriebssystem plus 1 bis 4 TB Cachespeicherplatz pro Knoten |
|Speicherkonto (optional) |V2|
|Back-End-Datenspeicher (optional) |Ein neuer LRS-Blobcontainer |

## <a name="accept-software-terms-in-advance"></a>Nutzungsbedingungen der Software im Voraus akzeptieren

> [!NOTE] 
> Dieser Schritt ist nicht erforderlich, wenn ein Abonnementbesitzer den Avere vFXT-Cluster erstellt.

Bevor Sie einen Cluster erstellen können, müssen Sie die Nutzungsbedingungen für die Avere vFXT-Software akzeptieren. Wenn Sie kein Abonnementbesitzer sind, lassen Sie einen Abonnementbesitzer die Bedingungen vorab akzeptieren. Dieser Schritt muss nur einmal pro Abonnement durchgeführt werden.

So akzeptieren Sie die Nutzungsbedingungen der Software im Voraus 

1. Öffnen Sie eine Cloud-Shell im Azure-Portal oder Navigieren Sie zu <https://shell.azure.com>. Melden Sie sich mit Ihrer Abonnement-ID an.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Führen Sie diesen Befehl aus, um Nutzungsbedingungen zu akzeptieren und den programmgesteuerten Zugriff auf die Softwareimages von Avere vFXT für Azure zu ermöglichen: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-node:latest
   ```

## <a name="next-step-create-the-vfxt-cluster"></a>Nächster Schritt: Erstellen des vFXT-Clusters

Nach Abschluss dieser Voraussetzungen können Sie in die Erstellung des Clusters einsteigen. Anweisungen finden Sie unter [Bereitstellen des vFXT-Clusters](avere-vfxt-deploy.md).