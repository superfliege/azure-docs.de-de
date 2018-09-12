---
title: Verwenden von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mit Azure SDKs
description: Codebeispiele für die Verwendung von Azure SDKs mit einem virtuellen Azure-Computer, der über verwaltete Identitäten für Azure-Ressourcen verfügt.
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
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 36460257b5c170c600ef08eff07bb3f64a391cd8
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43343347"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Verwenden von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mit Azure SDKs 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Dieser Artikel enthält eine Liste mit SDK-Beispielen, mit denen die Verwendung der Unterstützung des jeweiligen Azure SDK für verwaltete Identitäten für Azure-Ressourcen veranschaulicht wird.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Bei allen Beispielcodes/-skripts in diesem Artikel wird vorausgesetzt, dass der Client auf einem virtuellen Computer mit aktivierten verwalteten Identitäten für Azure-Ressourcen ausgeführt wird. Verwenden Sie die Funktion „Verbinden“ im Azure-Portal zum Herstellen einer Remoteverbindung mit Ihrem virtuellen Computer. Ausführliche Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Computer finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Computer über das Azure-Portal](qs-configure-portal-windows-vm.md) oder in einem der verwandten Artikel (PowerShell, CLI, Vorlage oder Azure SDK). 

## <a name="sdk-code-samples"></a>SDK-Codebeispiele

| SDK             | Codebeispiel |
| --------------- | ----------- |
| .NET            | [Bereitstellen einer Azure Resource Manager-Vorlage von einer Windows-VM mit verwalteten Identitäten für Azure-Ressourcen](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Aufrufen von Azure-Diensten von einer Linux-VM mit verwalteten Identitäten für Azure-Ressourcen](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Verwalten von Ressourcen mithilfe verwalteter Identitäten für Azure-Ressourcen](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Verwenden von verwalteten Identitäten für Azure-Ressourcen für einfaches Authentifizieren über eine VM](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Verwalten von Ressourcen auf einem virtuellen Computer mit aktivierten verwalteten Identitäten für Azure-Ressourcen](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Nächste Schritte

- Die vollständige Liste der Azure SDK-Ressourcen (einschließlich Bibliothekdownloads, Dokumentation usw.) finden Sie unter [Azure SDKs](https://azure.microsoft.com/downloads/).
- Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Computer über das Azure-Portal](qs-configure-portal-windows-vm.md).








