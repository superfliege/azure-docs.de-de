---
title: Voraussetzungen für Avere vFXT – Azure
description: Voraussetzungen für Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 9c3301ba16bfaeb7014658a380e287a36a505be8
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299204"
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

Es gibt zwei Möglichkeiten, wenn Sie den Benutzern, die den vFXT erstellen, keinen Besitzerzugriff gewähren möchten:

* Ein Ressourcengruppenbesitzer kann einen Cluster erstellen, wenn die folgenden Bedingungen erfüllt sind:

  * Ein Abonnementbesitzer muss [die Nutzungsbedingungen für die Avere vFXT-Software](#accept-software-terms) akzeptieren und [die Zugriffsrolle für Clusterknoten](#create-the-cluster-node-access-role) erstellen. 
  * Alle Avere vFXT-Ressourcen müssen innerhalb der Ressourcengruppe bereitgestellt werden, einschließlich:
    * Clustercontroller
    * Clusterknoten
    * Blob Storage
    * Netzwerkelemente
 
* Ein Benutzer ohne Besitzerberechtigung kann vFXT-Cluster erstellen, indem er dem Benutzer über die rollenbasierte Zugriffskontrolle (RBAC) vorab Berechtigungen zuweist. Diese Methode gewährt diesen Benutzern jedoch erhebliche Berechtigungen. [In diesem Artikel](avere-vfxt-non-owner.md) wird erläutert, wie eine Zugriffsrolle erstellt wird, um Nicht-Besitzer für die Erstellung von Clustern zu autorisieren.

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

## <a name="accept-software-terms"></a>Nutzungsbedingungen der Software akzeptieren

> [!NOTE] 
> Dieser Schritt ist nicht erforderlich, wenn ein Abonnementbesitzer den Avere vFXT-Cluster erstellt.

Bei der Clustererstellung müssen Sie die Nutzungsbedingungen für die Avere vFXT-Software akzeptieren. Wenn Sie kein Abonnementbesitzer sind, lassen Sie einen Abonnementbesitzer die Bedingungen vorab akzeptieren. Dieser Schritt muss nur einmal pro Abonnement durchgeführt werden.

So akzeptieren Sie die Nutzungsbedingungen der Software im Voraus 

1. Öffnen Sie eine Cloud-Shell im Azure-Portal oder Navigieren Sie zu <https://shell.azure.com>. Melden Sie sich mit Ihrer Abonnement-ID an.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Führen Sie diesen Befehl aus, um Nutzungsbedingungen zu akzeptieren und den programmgesteuerten Zugriff auf das Softwareimage von Avere vFXT für Azure zu ermöglichen: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-access-roles"></a>Zugriffsrollen erstellen 

[Rollenbasierte Zugriffssteuerung](../role-based-access-control/index.yml) (RBAC) gibt dem vFXT-Clustercontroller und Clusterknoten die Berechtigung, erforderliche Aufgaben auszuführen.

* Der Clustercontroller benötigt die Berechtigung zum Erstellen und Ändern virtueller Computer, um den Cluster zu erstellen. 

* Im Rahmen des normalen vFXT-Clusterbetriebs müssen einzelne vFXT-Knoten Aufgaben wie das Lesen von Azure-Ressourceneigenschaften, das Verwalten von Speicher und die Steuerung der Netzwerkschnittstelleneinstellungen anderer Knoten übernehmen.

Bevor Sie Ihren Avere vFXT-Cluster erstellen können, müssen Sie eine benutzerdefinierte Rolle für die Verwendung mit den Clusterknoten definieren. 

Für den Clustercontroller können Sie die Standardrolle aus der Vorlage übernehmen. Der Standardwert gewährt dem Clustercontroller die Berechtigungen eines Ressourcengruppenbesitzers. Wenn Sie eine benutzerdefinierte Rolle für den Controller erstellen möchten, lesen Sie [Angepasste Controllerzugriffsrolle](avere-vfxt-controller-role.md).

> [!NOTE] 
> Nur ein Abonnementbesitzer oder ein Benutzer mit der Rolle „Besitzer“ oder „Benutzerzugriffsadministrator“ kann Rollen anlegen. Die Rollen können vorab erstellt werden.  

### <a name="create-the-cluster-node-access-role"></a>Erstellen der Zugriffsrolle für Clusterknoten

<!-- caution - this header is linked to in the template so don't change it unless you can change that -->

Sie müssen die Clusterknotenrolle anlegen, bevor Sie den Avere vFXT for Azure-Cluster erstellen können.

> [!TIP] 
> Interne Microsoft-Benutzer sollten die vorhandene Rolle mit dem Namen „Avere Cluster Runtime Operator“ verwenden, anstatt zu versuchen, eine neue zu erstellen. 

1. Kopieren Sie diese Datei. Fügen Sie Ihre Abonnement-ID in die Zeile „AssignableScopes“ ein.

   (Die aktuelle Version dieser Datei befindet sich im Repository github.com/Azure/Avere unter der Bezeichnung [AvereOperator.txt](https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereOperator.txt).)  

   ```json
   {
      "AssignableScopes": [
          "/subscriptions/PUT_YOUR_SUBSCRIPTION_ID_HERE"
      ],
      "Name": "Avere Operator",
      "IsCustom": "true",
      "Description": "Used by the Avere vFXT cluster to manage the cluster",
      "NotActions": [],
      "Actions": [
          "Microsoft.Compute/virtualMachines/read",
          "Microsoft.Network/networkInterfaces/read",
          "Microsoft.Network/networkInterfaces/write",
          "Microsoft.Network/virtualNetworks/subnets/read",
          "Microsoft.Network/virtualNetworks/subnets/join/action",
          "Microsoft.Network/networkSecurityGroups/join/action",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "DataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ]
   }
   ```

1. Speichern Sie die Datei unter ``avere-operator.json`` oder einem ähnlichen einprägsamen Namen. 


1. Öffnen Sie eine Azure Cloud Shell, und melden Sie sich mit Ihrer Abonnement-ID an ([weiter oben in diesem Dokument](#accept-software-terms) beschrieben). Verwenden Sie diesen Befehl, um die Rolle zu erstellen:

   ```bash
   az role definition create --role-definition /avere-operator.json
   ```

Der Rollenname wird beim Erstellen des Clusters verwendet. In diesem Beispiel lautet der Name ``avere-operator``.

## <a name="next-step-create-the-vfxt-cluster"></a>Nächster Schritt: Erstellen des vFXT-Clusters

Nach Abschluss dieser Voraussetzungen können Sie in die Erstellung des Clusters einsteigen. Anweisungen finden Sie unter [Bereitstellen des vFXT-Clusters](avere-vfxt-deploy.md).