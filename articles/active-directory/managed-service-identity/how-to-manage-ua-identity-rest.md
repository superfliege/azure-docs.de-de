---
title: Verwalten benutzerzugewiesener verwalteter Identitäten in Azure mithilfe von REST
description: Schrittweise Anweisungen zum Erstellen, Auflisten und Löschen einer benutzerzugewiesenen verwalteten Identität zum Durchführen von REST-API-Aufrufen.
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
ms.date: 06/26/2018
ms.author: daveba
ms.openlocfilehash: 70a8c9018cdc2929abc85336211beecf82bf32cb
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188045"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-rest-api-calls"></a>Erstellen, Auflisten oder Löschen einer benutzerzugewiesenen Identität mithilfe von REST-API-Aufrufen

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Die Funktion „Verwaltete Identität“ ermöglicht es Azure-Diensten, sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie mithilfe von CURL zum Durchführen von REST-API-Aufrufen eine benutzerzugewiesene Identität erstellen, auflisten und löschen können.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Wenn Sie Windows verwenden, installieren Sie das [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) oder verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) im Azure-Portal.
- Wenn Sie das [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) oder eine [Linux-Distribution](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) verwenden, [installieren Sie die lokale Konsole für die Azure-Befehlszeilenschnittstelle](/azure/install-azure-cli).
- Wenn Sie die lokale Konsole für die Azure-Befehlszeilenschnittstelle verwenden, melden Sie sich über `az login` bei Azure mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das Sie bereitstellen möchten oder für das Sie Informationen zu benutzerzugewiesenen verwalteten Identitäten abrufen möchten.
- Um die Verwaltungsvorgänge in diesem Artikel durchzuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - Rolle [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) zum Erstellen, Lesen (Auflisten), Aktualisieren und Löschen einer vom Benutzer zugewiesenen Identität
    - Rolle [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator) zum Lesen (Auflisten) der Eigenschaften einer vom Benutzer zugewiesenen Identität
- Rufen Sie mit `az account get-access-token` ein Bearer-Zugriffstoken ab, um die folgenden Vorgänge für benutzerzugewiesene verwaltete Identitäten auszuführen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer vom Benutzer zugewiesenen verwalteten Identität 

Zum Erstellen einer benutzerzugewiesenen verwalteten Identität verwenden Sie die folgende CURL-Anforderung für die Azure Resource Manager-API. Ersetzen Sie die Werte `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>` und `<ACCESS TOKEN>` durch Ihre eigenen Werte:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Auflisten von benutzerzugewiesenen verwalteten Identitäten

Zum Auflisten von benutzerzugewiesenen verwalteten Identitäten verwenden Sie die folgende CURL-Anforderung für die Azure Resource Manager-API. Ersetzen Sie die Werte `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` und `<ACCESS TOKEN>` durch Ihre eigenen Werte:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer benutzerzugewiesenen verwalteten Identität

Zum Löschen einer benutzerzugewiesenen verwalteten Identität verwenden Sie die folgende CURL-Anforderung für die Azure Resource Manager-API. Ersetzen Sie die Parameterwerte `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` und `<ACCESS TOKEN>` durch Ihre eigenen Werte:

> [!NOTE]
> Durch das Löschen einer benutzerzugewiesenen Identität wird der Verweis nicht für andere Ressourcen entfernt, denen er zugewiesen wurde. Informationen zum Entfernen einer benutzerzugewiesenen verwalteten Identität von einem virtuellen Computer mithilfe von CURL finden Sie unter [Entfernen einer benutzerzugewiesenen Identität von einem virtuellen Azure-Computer](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Zuweisen einer benutzerzugewiesenen Identität zu einer Azure-VM/VMSS mithilfe von CURL finden Sie unter [Konfigurieren einer verwalteten Identität auf einem virtuellen Azure-Computer mithilfe von CURL](qs-configure-rest-vm.md#user-assigned-identity) und [Konfigurieren einer verwalteten Identität in einer VM-Skalierungsgruppe mithilfe von CURL](qs-configure-rest-vmss.md#user-assigned-identity).


