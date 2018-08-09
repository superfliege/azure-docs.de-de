---
title: Zuweisen von MSI-Zugriff zu einer Azure-Ressource mit der Azure-CLI
description: Schrittweise Anweisungen für das Zuweisen von MSI zu einer Ressource und das Zuweisen des Zugriffs für eine andere Ressource mithilfe der Azure-CLI.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2017
ms.author: daveba
ms.openlocfilehash: a5da06eac7f4680282aad305f57cb9ca1c9d5730
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424431"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Zuweisen des MSI-Zugriffs (Managed Service Identity, verwaltete Dienstidentität) auf eine Ressource mit der Azure-CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Nachdem Sie eine Azure-Ressource mit einer MSI konfiguriert haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Beispiel wird gezeigt, wie Sie der MSI eines virtuellen Azure-Computers oder einer VM-Skalierungsgruppe mit der Azure-CLI den Zugriff auf ein Azure-Speicherkonto gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:

- Verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
- Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um der MSI den Zugriff auf eine andere Ressource zuzuweisen.

Gehen Sie wie folgt vor, nachdem Sie die MSI auf einer Azure-Ressource aktiviert haben, z.B. einem [virtuellen Azure-Computer](qs-configure-cli-windows-vm.md) oder einer [Azure-VM-Skalierungsgruppe](qs-configure-cli-windows-vmss.md): 

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/reference-index#az-login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die VM oder VM-Skalierungsgruppe bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. In diesem Beispiel wird einem virtuellen Azure-Computer der Zugriff auf ein Speicherkonto gewährt. Zunächst verwenden wir [az resource list](/cli/azure/resource/#az-resource-list), um den Dienstprinzipal für den virtuellen Computer mit dem Namen „myVM“ abzurufen:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Für eine Azure-VM-Skalierungsgruppe ist der Befehl identisch, aber Sie rufen jetzt den Dienstprinzipal für die VM-Skalierungsgruppe „DevTestVMSS“ ab:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Nachdem Sie die Dienstprinzipal-ID beschafft haben, können Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) verwenden, um dem virtuellen Computer oder der VM-Skalierungsgruppe Lesezugriff („Reader“) auf das Speicherkonto „myStorageAcct“ zu gewähren:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Problembehandlung

Wenn die MSI für die Ressource nicht in der Liste der verfügbaren Identitäten angezeigt wird, stellen Sie sicher, dass die MSI ordnungsgemäß aktiviert wurde. In unserem Fall können wir im [Azure-Portal](https://portal.azure.com) zurück zu unserem virtuellen Azure-Computer bzw. zur VM-Skalierungsgruppe navigieren und wie folgt vorgehen:

- Überprüfen Sie auf der Seite „Konfiguration“, ob für die MSI-Aktivierung „Ja“ ausgewählt ist.
- Zeigen Sie die Seite „Erweiterungen“ an, und überprüfen Sie, ob die MSI-Erweiterung erfolgreich bereitgestellt wurde (die Seite **Erweiterungen** ist nicht für eine Azure-VM-Skalierungsgruppe verfügbar).

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](overview.md).
- Informationen zum Aktivieren der MSI auf einem virtuellen Azure-Computer finden Sie unter [Configure an Azure VM Managed Service Identity (MSI) using Azure CLI](qs-configure-cli-windows-vm.md) (Konfigurieren einer Azure-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure-CLI).
- Informationen zum Aktivieren der MSI in einer Azure-VM-Skalierungsgruppe finden Sie unter [Configure an Azure Virtual Machine Scale Set Managed Service Identity (MSI) using the Azure portal](qs-configure-portal-windows-vmss.md) (Konfigurieren einer Azure-VM-MSI (Managed Service Identity) über das Azure-Portal).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

