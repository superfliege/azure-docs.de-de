---
title: "Verwenden einer vom Benutzer zugewiesenen verwalteten Dienstidentität aus Azure SDKs auf einem virtuellen Computer"
description: Codebeispiele zur Verwendung von Azure SDKs mit einer vom Benutzer zugewiesenen MSI auf einem virtuellen Azure-Computer.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f9a31a0500a6f5f1c49fc45d5811e28788e6f2b1
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Verwenden von Azure SDKs mit einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)] Dieser Artikel enthält eine Liste mit SDK-Beispielen, mit denen die Verwendung der Unterstützung des jeweiligen Azure SDK für vom Benutzer zugewiesene MSI veranschaulicht wird.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - Bei allen Beispielcodes und -skripts in diesem Artikel wird vorausgesetzt, dass der Client auf einem MSI-fähigen virtuellen Computer ausgeführt wird. Verwenden Sie die Funktion „Verbinden“ im Azure-Portal zum Herstellen einer Remoteverbindung mit Ihrem virtuellen Computer. Weitere Informationen zur Aktivierung von MSI auf einer VM finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe der Azure-Befehlszeilenschnittstelle](msi-qs-configure-cli-windows-vm.md) oder in einem der verwandten Artikel (PowerShell, Azure-Portal, Vorlage oder Azure SDK). 

## <a name="sdk-code-samples"></a>SDK-Codebeispiele

| SDK             | Codebeispiel |
| --------------- | ----------- |
| Python          | [Use MSI to authenticate simply from inside a VM](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) (Verwenden von MSI zum einfachen Authentifizieren über eine VM) |
| Ruby            | [Manage resources from an MSI-enabled VM](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) (Verwalten von Ressourcen über eine MSI-fähige VM) |

## <a name="next-steps"></a>Nächste Schritte

- Die vollständige Liste der Azure SDK-Ressourcen (einschließlich Bibliothekdownloads, Dokumentation usw.) finden Sie unter [Azure SDKs](https://azure.microsoft.com/downloads/).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.








