---
title: Angepasste Controllerzugriffsrolle – Avere vFXT for Azure
description: Erstellen einer benutzerdefinierten Zugriffsrolle für den Avere vFXT-Clustercontroller
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: c408efa7ca01928e25ac03f5ca63d0aef7d88839
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770035"
---
# <a name="customized-controller-access-role"></a>Angepasste Controllerzugriffsrolle

Der Avere vFXT for Azure-Clustercontroller verwendet eine verwaltete Identität und die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC), um den Cluster erstellen und verwalten zu können. 

Standardmäßig wird dem Clustercontroller die [integrierte Rolle „Besitzer“](../role-based-access-control/built-in-roles.md#owner) zugewiesen. Außerdem wird der Zugriff des Controllers auf die zugehörige Ressourcengruppe beschränkt – eine Änderung von Elementen außerhalb der Ressourcengruppe des Clusters ist nicht möglich.

In diesem Artikel wird erläutert, wie Sie eine eigene Zugriffsrolle für den Clustercontroller erstellen, anstatt die Standardeinstellung zu verwenden. 

## <a name="edit-the-role-prototype"></a>Bearbeiten des Rollenprototyps

Beginnen Sie mit der Prototyprolle, die unter <https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereContributor.txt> verfügbar ist.

```json
{
  "AssignableScopes": [
    "/subscriptions/YOUR SUBSCRIPTION ID HERE"
  ],
  "Name": "Avere custom contributor",
  "IsCustom": true,
  "Description": "Can create and manage an Avere vFXT cluster.",
  "NotActions": [],
  "Actions": [
    "Microsoft.Authorization/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/availabilitySets/*",
    "Microsoft.Compute/virtualMachines/*",
    "Microsoft.Compute/disks/*",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Network/*/read",
    "Microsoft.Network/networkInterfaces/*",
    "Microsoft.Network/virtualNetworks/subnets/join/action",
    "Microsoft.Network/virtualNetworks/subnets/read",
    "Microsoft.Resources/deployments/*",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Storage/storageAccounts/listKeys/action",
    "Microsoft.Support/*"
  ],
  "DataActions": []
}
```

Fügen Sie in der AssignableScopes-Anweisung die Abonnement-ID für die Avere vFXT for Azure-Bereitstellung hinzu. Passen Sie den Namen an, und ergänzen oder ändern Sie die Definitionen nach Bedarf. 

Gehen Sie bei der Einschränkung von Berechtigungen vorsichtig vor. Die Clustererstellung ist möglicherweise nicht erfolgreich, wenn der Controller über keinen ausreichenden Zugriff verfügt. 

Um Unterstützung in Bezug auf die Berechtigungen zu erhalten, die der Clustercontroller für die Clustererstellung benötigt, [öffnen Sie ein Supportticket](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt). 

Speichern Sie Ihre benutzerdefinierte Rollendefinition als JSON-Datei. 

## <a name="define-the-role"></a>Definieren der Rolle 

Führen Sie diese Schritte aus, um Ihrem Abonnement die benutzerdefinierte Rollendefinition hinzuzufügen. 

1. Öffnen Sie die Azure Cloud Shell im Azure-Portal oder navigieren Sie zu [https://shell.azure.com](https://shell.azure.com).

1. Verwenden Sie den Azure CLI-Befehl, um zu Ihrem vFXT-Abonnement zu wechseln:

   ```azurecli
   az account set --subscription YOUR_SUBSCRIPTION_ID
   ```

1. Erstellen Sie die Rolle:

   ```azurecli
   az role definition create --role-definition /avere-contributor-custom.json
   ```

   Ersetzen Sie ```/avere-contributor-custom.json``` in diesem Beispiel durch Ihren Dateinamen und -pfad. 

Speichern Sie die Ausgabe des Befehls für die Rollendefinition – diese enthält den Rollenbezeichner, den Sie in der Vorlage für die Clustererstellung angeben müssen. 

## <a name="find-the-role-id"></a>Ermitteln der Rollen-ID

Die Avere vFXT-Bereitstellungsvorlage benötigt die globale ID (GUID) der Rolle, um dem Controller eine benutzerdefinierte Rolle zuzuweisen. 

Die Rollen-GUID ist eine aus 32 Zeichen bestehende Zeichenfolge mit diesem Format: 8e3af657-a8ff-443c-a75c-2fe8c4bcb635

Um die GUID Ihrer Rolle abzurufen, verwenden Sie diesen Befehl mit dem Namen Ihrer Rolle im ```--name```-Parameter.

```azurecli
az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
```
Geben Sie diese Zeichenfolge während der Avere vFXT for Azure-Bereitstellung im Feld **Rollen-ID zum Erstellen des Avere-Clusters** ein.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Informationen zum Bereitstellen von Avere vFXT for Azure unter [Bereitstellen des vFXT-Clusters](avere-vfxt-deploy.md).
