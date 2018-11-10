---
title: Erstellen von Avere-Rollen ohne Controller – Avere vFXT für Azure
description: Methode zum Erstellen der erforderlichen RBAC-Rolle ohne Clustercontroller-VM
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 547a0e0ac5254408a4064d627ec4c1e7c354a433
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670027"
---
# <a name="create-the-avere-vfxt-cluster-runtime-access-role-without-a-controller"></a>Erstellen der Zugriffsrolle für die Avere vFXT-Clusterlaufzeit ohne Controller

In diesem Dokument wird eine Methode zum Erstellen der Zugriffsrolle für Clusterknoten über die Befehlszeile veranschaulicht, bevor Sie die Clustercontroller-VM erstellen. 

Informationen zum Erstellen über den Clustercontroller finden Sie unter [Erstellen der Zugriffsrolle für Clusterknoten](avere-vfxt-deploy.md#create-the-cluster-node-access-role). Das Controllerimage enthält eine Rollenprototypdatei. Sie können die Datei mit Ihrer Abonnement-ID aktualisieren und damit die Rolle lokal auf dem virtuellen Controllercomputer definieren.

## <a name="create-an-azure-rbac-role"></a>Erstellen einer Azure RBAC-Rolle

Das Avere vFXT-System verwendet die [rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC), um die vFXT-Clusterknoten für die Ausführung erforderlicher Aufgaben zu autorisieren.  

Im Rahmen des normalen vFXT-Clusterbetriebs müssen einzelne vFXT-Knoten Aufgaben wie das Lesen von Azure-Ressourceneigenschaften, das Verwalten von Speicher und die Steuerung der Netzwerkschnittstelleneinstellungen anderer Knoten übernehmen. 

Diese Rolle wird nur für die vFXT-Knoten verwendet, nicht für die Clustercontroller-VM.  

Wenn Sie die Rolle vor dem Controller erstellen möchten, gehen Sie wie folgt vor: 

1. Öffnen Sie die Azure Cloud Shell im Azure-Portal oder navigieren Sie zu [https://shell.azure.com](https://shell.azure.com).

1. Verwenden Sie den Azure CLI-Befehl, um zu Ihrem vFXT-Abonnement zu wechseln:

   ```az account set --subscription YOUR_SUBSCRIPTION_ID```

1. Verwenden Sie diese Befehle, um die Rollendefinition aus dem Marketplace-Image herunterzuladen und zu bearbeiten. 

   ```bash
   wget -O- https://averedistribution.blob.core.windows.net/public/vfxtdistdoc.tgz | tar zxf - avere-cluster.json
   vi avere-cluster.json
   ``` 

4. Bearbeiten Sie die Datei so, dass sie Ihre Abonnement-ID enthält, und löschen Sie die darüberliegende Zeile. Speichern Sie die Datei als ``avere-cluster.json``.

   ![Text-Editor der Konsole mit der Abonnement-ID und der zum Löschen ausgewählten Option „Diese Zeile entfernen“](media/avere-vfxt-edit-role.png)

5. Erstellen Sie die Rolle:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Wenn Sie den Cluster erstellen, geben Sie den Namen der Rolle an (in diesem Fall `avere-cluster`). Das Skript zur Clustererstellung ordnet die Rolle den neu erstellten Clusterknoten zu. 

## <a name="sample-cluster-node-runtime-role-definition"></a>Beispiel für die Definition der Laufzeitrolle eines Clusterknotens

> [!IMPORTANT] 
> Diese Beispieldefinition wurde aus einer Version vor der GA-Version des Produkts übernommen. Wenn sich die Version, die Sie mit der aktuellen Produktverteilung erhalten, unterscheidet, verwenden Sie stattdessen diese Version.

```json

{
    "AssignableScopes": [
        "/subscriptions/YOUR_SUBSCRIPTION_ID_GOES_HERE"
    ],
    "Name": "avere-cluster",
    "IsCustom": "true",
    "Description": "Avere cluster runtime role",
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