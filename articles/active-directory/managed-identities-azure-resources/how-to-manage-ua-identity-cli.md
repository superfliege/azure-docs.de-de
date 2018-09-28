---
title: Verwalten einer vom Benutzer zugewiesenen verwalteten Identität mithilfe der Azure-Befehlszeilenschnittstelle
description: Ausführliche Anweisungen zum Erstellen, Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe der Azure-Befehlszeilenschnittstelle
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 19963edc3742cc5f09ed02249d313728410711c8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973495"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe der Azure-Befehlszeilenschnittstelle

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie mit der Azure-Befehlszeilenschnittstelle eine vom Benutzer zugewiesene verwaltete Identität erstellen, auflisten und löschen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, zu lesen (aufzulisten), zu aktualisieren und zu löschen
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um die Eigenschaften einer vom Benutzer zugewiesenen verwalteten Identität zu lesen (auflisten)
- Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:
    - Verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
    - Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
    - [Installieren Sie die neueste Version der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. Melden Sie sich mit `az login` mit einem Konto, das mit dem Azure-Abonnement verknüpft ist, unter dem Sie die vom Benutzer zugewiesene verwaltete Identität bereitstellen möchten, bei Azure an.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer vom Benutzer zugewiesenen verwalteten Identität 

Um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, verwenden Sie den Befehl [az identity create](/cli/azure/identity#az-identity-create). Der Parameter `-g` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene verwaltete Identität erstellt werden soll, während der Parameter `-n` den Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

Verwenden Sie den Befehl [az identity list](/cli/azure/identity#az-identity-list), um vom Benutzer zugewiesene verwaltete Identitäten aufzulisten. Ersetzen Sie `<RESOURCE GROUP>` durch Ihren eigenen Wert:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
In der JSON-Antwort wird bei vom Benutzer zugewiesenen verwalteten Identitäten der Wert `"Microsoft.ManagedIdentity/userAssignedIdentities"` für den Schlüssel `type` zurückgegeben.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

Um eine vom Benutzer zugewiesene verwaltete Identität zu löschen, verwenden Sie den Befehl [az identity delete](/cli/azure/identity#az-identity-delete).  Der Parameter „-n“ gibt den Namen an, während der Parameter „-g“ die Ressourcengruppe angibt, in der die vom Benutzer zugewiesene verwaltete Identität erstellt wurde. Ersetzen Sie die Parameterwerte `<USER ASSIGNED IDENTITY NAME>` und `<RESOURCE GROUP>` durch Ihre eigenen Werte:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Durch das Löschen einer vom Benutzer zugewiesenen verwalteten Identität wird der Verweis nicht für andere Ressourcen entfernt, denen sie zugewiesen wurde. Entfernen Sie diese mit dem Befehl `az vm/vmss identity remove` von der VM/VMSS.

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Liste der Identitätsbefehle für die Azure CLI finden Sie unter [az identity](/cli/azure/identity).

Informationen zum Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einer Azure-VM finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe der Azure-Befehlszeilenschnittstelle](qs-configure-cli-windows-vm.md#user-assigned-managed-identity).


 
