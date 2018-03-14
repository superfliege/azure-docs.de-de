---
title: Konfigurieren einer MSI in einer Azure-VM-Skalierungsgruppe mit der Azure CLI
description: "Schritt-für-Schritt-Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) in einer Azure-VM-Skalierungsgruppe mit der Azure CLI."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: c58ad9cc8bfa16b11201735bcc513e6dd692a2a9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Konfigurieren einer MSI (Managed Service Identity, verwaltete Dienstidentität) für eine VM-Skalierungsgruppe mit der Azure CLI

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie eine MSI für eine Azure-VM-Skalierungsgruppe mit der Azure CLI aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:

- Verwenden Sie [Azure Cloud Shell](../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
- Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set"></a>Aktivieren der MSI während der Erstellung einer Azure-VM-Skalierungsgruppe

So erstellen Sie eine MSI-fähige VM-Skalierungsgruppe:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/#az_login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die VM-Skalierungsgruppe bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen der VM-Skalierungsgruppe und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#az_group_create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Erstellen Sie mit [az vmss create](/cli/azure/vmss/#az_vmss_create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine VM-Skalierungsgruppe mit dem Namen *myVMSS* mit einer MSI erstellt, wie vom Parameter `--assign-identity` angefordert. Der `--admin-username`-Parameter und der `--admin-password`-Parameter geben den Namen und das Kennwort des Administratorbenutzers für die Anmeldung am virtuellen Computer an. Aktualisieren Sie diese Werte ggf. mit den entsprechenden Werten für Ihre Umgebung: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

## <a name="enable-msi-on-an-existing-azure-virtual-machine-scale-set"></a>Aktivieren einer MSI für eine vorhandene Azure-VM-Skalierungsgruppe

Wenn Sie eine MSI für eine vorhandene Azure-VM-Skalierungsgruppe aktivieren müssen, gehen Sie wie folgt vor:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/#az_login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

   ```azurecli-interactive
   az login
   ```

2. Verwenden Sie [az vmss assign-identity](/cli/azure/vm/#az_vmss_assign_identity) mit dem `--assign-identity`-Parameter, um einem vorhandenen virtuellen Computer eine MSI hinzuzufügen:

   ```azurecli-interactive
   az vmss assign-identity -g myResourceGroup -n myVMSS
   ```

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Entfernen einer MSI aus einer Azure-VM-Skalierungsgruppe

Wenn die MSI in einer VM-Skalierungsgruppe nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/#az_login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

   ```azurecli-interactive
   az login
   ```

2. Verwenden Sie den `--identities`-Switch mit [az vmss remove-identity](/cli/azure/vmss/#az_vmss_remove_identity), um die MSI zu entfernen:

   ```azurecli-interactive
   az vmss remove-identity -g myResourceGroup -n myVMSS --identities readerID writerID
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über verwaltete Dienstidentitäten](msi-overview.md)
- Die vollständige Schnellstartanleitung für die Erstellung einer Azure-VM-Skalierungsgruppe finden Sie hier: 

  - [Erstellen einer VM-Skalierungsgruppe und Bereitstellen einer hoch verfügbaren App unter Linux](../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
















