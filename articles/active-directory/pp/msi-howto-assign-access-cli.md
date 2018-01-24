---
title: Zuweisen von Zugriff auf einer Azure-Ressource mit einer vom Benutzer zugewiesenen MSI mithilfe der Azure-Befehlszeilenschnittstelle
description: "Ausführliche Anweisungen für das Zuweisen einer vom Benutzer zugewiesenen MSI zu einer Ressource und des Zugriffs auf eine andere Ressource mithilfe der Azure-Befehlszeilenschnittstelle."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 732fc1981bdf95e7548ea0ebe0ca8ece00f483ce
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-user-assigned-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Zuweisen des Zugriffs auf eine Ressource mit einer vom Benutzer zugewiesenen MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe der Azure-Befehlszeilenschnittstelle

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Nachdem Sie eine vom Benutzer zugewiesene MSI erstellt haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Beispiel wird gezeigt, wie Sie der vom Benutzer zugewiesenen MSI mithilfe der Azure-Befehlszeilenschnittstelle Zugriff auf ein Azure Storage-Konto gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Für die Ausführung der CLI-Beispielskripts in diesem Tutorial stehen Ihnen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie [Azure Cloud Shell](~/articles/cloud-shell/overview.md) entweder vom Azure-Portal aus oder über die Schaltfläche „Ausprobieren“, die sich in der oberen rechten Ecke eines jeden Codeblocks befindet.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. Melden Sie sich dann mit [az login](/cli/azure/#login) bei Azure an. Verwenden Sie ein Konto, das mit dem Azure-Abonnement verknüpft ist, unter dem Sie die vom Benutzer zugewiesene MSI und VM bereitstellen möchten:

   ```azurecli
   az login
   ```

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um der MSI den Zugriff auf eine andere Ressource zuzuweisen.

Um eine MSI mit einer Hostressource (z.B. einem virtuellen Computer) für die Anmeldung oder den Zugriff auf Ressourcen verwenden zu können, muss der MSI zunächst über die Rollenzuweisung Zugriff auf Ressourcen erteilt werden. Dies ist vor dem Zuordnen der MSI zum Host oder danach möglich. Alle Schritte zum Erstellen und Zuordnen zu einem virtuellen Computer finden Sie unter [Konfigurieren einer vom Benutzer zugewiesenen MSI für einen virtuellen Computer mithilfe der Azure-Befehlszeilenschnittstelle](msi-qs-configure-cli-windows-vm.md).

Im folgenden Beispiel wird einer vom Benutzer zugewiesenen MSI Zugriff auf ein Speicherkonto gewährt.  

1. Um der MSI Zugriff zu gewähren, benötigen Sie die Client-ID (auch als App-ID bezeichnet) des Dienstprinzipals der MSI. Die Client-ID wird bei der Bereitstellung der MSI und ihres Dienstprinzipals (hier zu Referenzzwecken dargestellt) mit [az identity create](/cli/azure/identity#az_identity_create) bereitgestellt:

   ```azurecli-interactive
   az identity create -g rgPrivate -n msiServiceApp
   ```

   Eine erfolgreiche Antwort enthält die Client-ID des Dienstprinzipals der vom Benutzer zugewiesenen MSI in der `clientId`-Eigenschaft:

   ```json
   {
        "clientId": "9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp/credentials?tid=733a8f0p-ec41-4e69-8adz-971fc4b533bl&oid=z4baa4fc-fce4-4202-8250-5fb359abblll&aid=9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp",
        "location": "westcentralus",
        "name": "msiServiceApp",
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "resourceGroup": "rgPrivate",
        "tags": {},
        "tenantId": "733a8f0p-ec41-4e69-8adz-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
   }
   ```

2. Wenn die Client-ID bekannt ist, verwenden Sie [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create), um der MSI Zugriff auf eine andere Ressource zuzuweisen. Verwenden Sie unbedingt Ihre Client-ID für den `--assignee`-Parameter sowie die zugehörige Abonnement-ID und den Ressourcengruppennamen, die bei der Ausführung von `az identity create` zurückgegeben wurden. Hier wird der MSI der Zugriff „Leser“ auf das Speicherkonto „myStorageAcct“ zugewiesen:

   ```azurecli-interactive
   az role assignment create --assignee 9391e5b1-dada-4a8z-834a-43ad44f296bl --role Reader --scope /subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

   Eine erfolgreiche Antwort ähnelt der folgenden Ausgabe:

   ```json
   {
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct/providers/Microsoft.Authorization/roleAssignments/f4766864-9493-43c6-91d7-abd131c3c017",
        "name": "f4766864-9493-43c6-91d7-abd131c3c017",
        "properties": {
            "additionalProperties": {
            "createdBy": null,
            "createdOn": "2017-12-21T20:49:37.5590544Z",
            "updatedBy": "pd78b21f-17a4-41az-b7db-9aadec3376bl",
            "updatedOn": "2017-12-21T20:49:37.5590544Z"
            },
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "roleDefinitionId": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "scope": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct"
        },
        "resourceGroup": "rgPrivate",
        "type": "Microsoft.Authorization/roleAssignments"
   }
   ```

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Weitere Informationen zum Aktivieren einer vom Benutzer zugewiesenen MSI auf einer Azure-VM finden Sie unter [Konfigurieren einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) für eine VM unter Verwendung der Azure CLI](msi-qs-configure-cli-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

