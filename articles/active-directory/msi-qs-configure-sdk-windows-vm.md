---
title: "Konfigurieren eines MSI-fähigen virtuellen Azure-Computers mithilfe eines Azure SDK"
description: "Schrittweise Anweisungen zum Konfigurieren und Verwenden einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mithilfe eines Azure SDK"
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: bryanla
ms.openlocfilehash: 84fcb0181d635ecd8ebc68b31365fba9d6be21ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Konfigurieren einer VM-MSI mithilfe eines Azure SDK

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (AD) bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Computer mit einem Azure SDK aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure SDKs mit MSI-Unterstützung 

Azure unterstützt mit einer Reihe von [Azure SDKs](https://azure.microsoft.com/downloads) mehrere Programmierplattformen. Mehrere davon wurden aktualisiert, um MSI zu unterstützen, und enthalten entsprechende Beispiele, um die Nutzung zu demonstrieren. Diese Liste wird jeweils aktualisiert, wenn weitere Unterstützung hinzugefügt wird:

| SDK | Beispiel |
| --- | ------ | 
| .NET   | [Manage resource from an MSI-enabled VM](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) (Verwalten von Ressourcen über eine MSI-fähige VM) |
| Java   | [Manage Storage from an MSI-enabled VM](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/) (Verwalten von Speicher über eine MSI-fähige VM)|
| Node.js| [Create a VM with MSI authentication enabled](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) (Erstellen eines virtuellen Computers mit aktivierter MSI-Authentifizierung) |
| Python | [Create a VM with MSI authentication enabled](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) (Erstellen eines virtuellen Computers mit aktivierter MSI-Authentifizierung) |
| Ruby   | [Create Azure virtual machines with Managed Service Identity using Ruby](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) (Erstellen virtueller Azure-Computer mit verwalteter Dienstidentität mithilfe von Ruby) |

## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie auch das Azure-Portal, PowerShell, die CLI und Ressourcenvorlagen verwenden können, finden Sie in den verwandten Artikeln unter „Configure MSI for an Azure VM“ (Konfigurieren von MSI für einen virtuellen Azure-Computer).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
