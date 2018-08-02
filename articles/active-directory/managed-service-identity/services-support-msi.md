---
title: Azure-Dienste, die die verwaltete Dienstidentität unterstützen
description: Liste der Dienste, die die verwaltete Dienstidentität und die Azure AD-Authentifizierung unterstützen
services: active-directory
author: daveba
ms.author: daveba
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: 74fb9e784122dadd1ad2f6f29a497398eacf7464
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412881"
---
# <a name="services-that-support-managed-service-identity"></a>Dienste, die die verwaltete Dienstidentität unterstützen 

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Mit einer verwalteten Identität können Sie sich bei jedem Dienst authentifizieren, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. Die verwaltete Dienstidentität (Managed Service Identity, MSI) und die Azure AD-Authentifizierung werden derzeit in Azure integriert. Sie sollten sich regelmäßig über Aktualisierungen informieren.

## <a name="azure-services-that-support-managed-service-identity"></a>Azure-Dienste, die verwaltete Dienstidentitäten unterstützen

Die folgenden Azure-Dienste unterstützen verwaltete Dienstidentitäten.

| Dienst | Vom System zugewiesener Status | Vom Benutzer zugewiesener Status| Konfigurieren | Abrufen von Token |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Vorschau | Vorschau | [Azure-Portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure-CLI](qs-configure-cli-windows-vm.md)<br>[Azure-Ressourcen-Manager-Vorlagen](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Virtual Machine Scale Sets | Vorschau | Vorschau | [Azure-Portal](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[Azure-CLI](qs-configure-cli-windows-vmss.md)<br>[Azure-Ressourcen-Manager-Vorlagen](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Azure App Service | Verfügbar | Nicht verfügbar | [Azure-Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure-CLI](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Azure Resource Manager-Vorlage](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Azure-Funktionen | Verfügbar | Nicht verfügbar | [Azure-Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure-CLI](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Azure Resource Manager-Vorlage](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Azure Data Factory V2 | Vorschau | Nicht verfügbar | [Azure-Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| Azure API Management | Vorschau | Nicht verfügbar | [Azure Resource Manager-Vorlage](/azure/api-management/api-management-howto-use-managed-service-identity) | 


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
| Azure Storage | https://storage.azure.com/ | Vorschau | Mai 2018 | |
