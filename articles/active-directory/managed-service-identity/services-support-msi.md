---
title: Azure-Dienste, die die verwaltete Dienstidentität unterstützen
description: Liste der Dienste, die die verwaltete Dienstidentität und die Azure AD-Authentifizierung unterstützen
services: active-directory
author: daveba
ms.author: daveba
ms.date: 03/28/2018
ms.topic: reference
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: d31c169600c594fc4764262cb8d080e9aee96b21
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34353610"
---
# <a name="services-that-support-managed-service-identity"></a>Dienste, die die verwaltete Dienstidentität unterstützen 

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Mit einer verwalteten Identität können Sie sich bei jedem Dienst authentifizieren, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. MSI und die Azure AD-Authentifizierung werden derzeit in Azure integriert. Sie sollten sich regelmäßig über Aktualisierungen informieren.

## <a name="azure-services-that-support-managed-service-identity"></a>Azure-Dienste, die verwaltete Dienstidentitäten unterstützen

Die folgenden Azure-Dienste unterstützen verwaltete Dienstidentitäten.

| Dienst | Status | Datum | Konfigurieren | Abrufen von Token |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Vorschau | September 2017 | [Azure-Portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure-CLI](qs-configure-cli-windows-vm.md)<br>[Azure-Ressourcen-Manager-Vorlagen](qs-configure-template-windows-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Azure App Service | Vorschau | September 2017 | [Azure-Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure Resource Manager-Vorlage](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure-Funktionen | Vorschau | September 2017 | [Azure-Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure Resource Manager-Vorlage](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure Data Factory V2 | Vorschau | November 2017 | [Azure-Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| Azure API Management | Vorschau | Oktober 2017 | [Azure Resource Manager-Vorlage](/azure/api-management/api-management-howto-use-managed-service-identity) |

## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-Dienste, die die Azure AD-Authentifizierung unterstützen

Die folgenden Dienste unterstützen die Azure AD-Authentifizierung und wurden mit Clientdiensten getestet, die verwaltete Dienstidentitäten verwenden.

| Dienst | Ressourcen-ID | Status | Datum | Zuweisen des Zugriffs |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Verfügbar | September 2017 | [Azure-Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure-CLI](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Verfügbar | September 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Verfügbar | September 2017 | |
| Azure SQL | https://database.windows.net/ | Verfügbar | Oktober 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Verfügbar | Dezember 2017 | |
| Azure-Servicebus | https://servicebus.azure.net | Verfügbar | Dezember 2017 | |
