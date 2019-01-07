---
title: Anzeigen des Dienstprinzipals einer verwalteten Identität über PowerShell
description: Schrittanleitungen zur Anzeige des Dienstprinzipals einer verwalteten Identität über PowerShell.
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
ms.date: 11/29/2018
ms.author: daveba
ms.openlocfilehash: 0e8558d96803e15da355f083c6d1988d5ee6cb68
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52884995"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Anzeigen des Dienstprinzipals einer verwalteten Identität über PowerShell

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erhalten Sie Informationen zur Anzeige des Dienstprinzipals einer verwalteten Identität über PowerShell.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter.
- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.
- Aktivieren Sie eine [systemseitig zugewiesene Identität auf einem virtuellen Computer](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) oder in einer [Anwendung](/azure/app-service/app-service-managed-service-identity#adding-a-system-assigned-identity).
- Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. 
- Wenn Sie PowerShell lokal ausführen, ist außerdem Folgendes erforderlich: 
    - Führen Sie zum Starten `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen.
    - Installieren Sie die [neueste Version von PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Führen Sie `Install-Module -Name PowerShellGet -AllowPrerelease` aus, um die Vorabversion des `PowerShellGet`-Moduls abzurufen (möglicherweise müssen Sie `Exit` in der aktuellen PowerShell-Sitzung ausführen, nachdem Sie diesen Befehl zum Installieren des `AzureRM.ManagedServiceIdentity`-Moduls ausgeführt haben).
    - Führen Sie `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` aus, um die Vorabversion des `AzureRM.ManagedServiceIdentity`-Moduls zu installieren und die Vorgänge für vom Benutzer zugewiesene verwaltete Identitäten in diesem Artikel auszuführen.

## <a name="view-the-service-principal"></a>Anzeigen des Dienstprinzipals

Mit dem folgenden Befehl zeigen Sie den Dienstprinzipal einer VM oder einer Anwendung mit aktivierter systemseitig zugewiesener Identität an. Ersetzen Sie `<VM or application name>` durch Ihre eigenen Werte.

```PowerShell
Get-AzureRmADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Anzeigen von Azure AD-Dienstprinzipalen mithilfe von PowerShell finden Sie unter [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal).


