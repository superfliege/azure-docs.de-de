---
title: Erstellen, Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Dienstidentität per Azure PowerShell
description: Enthält eine Schritt-für-Schritt-Anleitung zum Erstellen, Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Dienstidentität mit Azure PowerShell.
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
ms.openlocfilehash: cc9b1b002c882a847d0ba2359caf4a193ea8d648
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930789"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen Identität per Azure PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Die verwaltete Dienstidentität (Managed Service Identity, MSI) stellt Azure-Dienste mit einer verwalteten Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie per Azure PowerShell eine vom Benutzer zugewiesene Identität erstellen, auflisten und löschen.

## <a name="prerequisites"></a>Voraussetzungen

- Falls Sie nicht mit der verwalteten Dienstidentität vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Installieren Sie [die aktuelle Version von Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM), sofern noch nicht geschehen.
- Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-user-assigned-identity"></a>Erstellen einer vom Benutzer zugewiesenen Identität

Verwenden Sie den Befehl [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity), um eine vom Benutzer zugewiesene Identität zu erstellen. Der Parameter `ResourceGroupName` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene Identität erstellt wurde, während der Parameter `-Name` den jeweiligen Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

> [!IMPORTANT]
> Für die Erstellung von Identitäten, die vom Benutzer zugewiesen wurden, werden nur alphanumerische Zeichen und Bindestriche („0-9“, „a-Z“ bzw. „A-Z“ oder „-“) unterstützt. Darüber hinaus sollten Namen max. 24 Zeichen enthalten, damit die Zuordnung zur VM/VMSS richtig funktioniert. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen. Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](known-issues.md).

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Auflisten der vom Benutzer zugewiesenen Identitäten

Verwenden Sie den Befehl [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity), um vom Benutzer zugewiesene Identitäten aufzulisten.  Der Parameter `-ResourceGroupName` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene Identität erstellt wurde.  Ersetzen Sie `<RESOURCE GROUP>` durch Ihren eigenen Wert:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
In der Antwort wird bei Benutzeridentitäten der Wert `"Microsoft.ManagedIdentity/userAssignedIdentities"` für den Schlüssel `Type` zurückgegeben.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Löschen einer vom Benutzer zugewiesenen Identität

Verwenden Sie zum Löschen einer Benutzeridentität den Befehl [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity).  Mit dem Parameter `-ResourceGroupName` wird die Ressourcengruppe angegeben, in der die vom Benutzer zugewiesene Identität erstellt wurde, und mit dem Parameter `-Name` wird der dazugehörige Name angegeben.  Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

 ```azurecli-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Durch das Löschen einer vom Benutzer zugewiesenen Identität wird der Verweis nicht für andere Ressourcen entfernt, denen er zugewiesen wurde. Zuweisungen von Identitäten müssen separat entfernt werden.

## <a name="related-content"></a>Verwandte Inhalte

Eine vollständige Liste und weitere Details zu Azure PowerShell-MSI-Befehlen finden Sie unter [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).


 
