---
title: 'Gewusst wie: Verwalten einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) über die Azure CLI'
description: Ausführliche Anweisungen zum Erstellen, Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Dienstidentität über die Azure CLI
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
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 24172ebac8c7f124d0873b9d93d260fa2e1a8a44
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34594610"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen Identität mithilfe der Azure CLI

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Die verwaltete Dienstidentität (Managed Service Identity, MSI) stellt Azure-Dienste mit einer verwalteten Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure CLI eine vom Benutzer zugewiesene Identität erstellen, auflisten und löschen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.

- Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:

    - Verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
    - Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
    - [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. Melden Sie sich mit `az login` mit einem Konto bei Azure an, das mit dem Azure-Abonnement verknüpft ist, unter dem Sie die vom Benutzer zugewiesene Identität bereitstellen möchten.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer vom Benutzer zugewiesenen verwalteten Identität 

Um eine vom Benutzer zugewiesenen Identität zu erstellen, verwenden Sie den Befehl [az identity create](/cli/azure/identity#az-identity-create). Der Parameter `-g` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene Identität erstellt wurde, während der Parameter `-n` den jeweiligen Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Auflisten der vom Benutzer zugewiesenen Identitäten

Verwenden Sie zum Auflisten von Identitäten, die vom Benutzer zugewiesen werden, den Befehl [az identity list](/cli/azure/identity#az-identity-list).  Der Parameter `-g` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene Identität erstellt wurde.  Ersetzen Sie `<RESOURCE GROUP>` durch Ihren eigenen Wert:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
In der JSON-Antwort wird bei Benutzeridentitäten der Wert `"Microsoft.ManagedIdentity/userAssignedIdentities"` für den Schlüssel `type` zurückgegeben.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Löschen einer vom Benutzer zugewiesenen Identität

Um eine vom Benutzer zugewiesene Identität zu löschen, verwenden Sie den Befehl [az identity delete](/cli/azure/identity#az-identity-delete).  Der Parameter „-n“ gibt den Namen an, während der Parameter „-g“ die Ressourcengruppe angibt, in der die vom Benutzer zugewiesene Identität erstellt wurde.  Ersetzen Sie die Parameterwerte `<USER ASSIGNED IDENTITY NAME>` und `<RESOURCE GROUP>` durch Ihre eigenen Werte:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Durch das Löschen einer vom Benutzer zugewiesenen Identität wird nicht der Verweis von anderen Ressourcen entfernt, der diesen zugewiesen wurde. Entfernen Sie diese mit dem Befehl `az vm/vmss identity remove` von der VM/VMSS.

## <a name="related-content"></a>Verwandte Inhalte

Eine vollständige Liste der Identitätsbefehle für die Azure CLI finden Sie unter [az identity](/cli/azure/identity).

Informationen zum Zuweisen einer vom Benutzer zugewiesenen Identität zu einer Azure-VM finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-identity).


 
