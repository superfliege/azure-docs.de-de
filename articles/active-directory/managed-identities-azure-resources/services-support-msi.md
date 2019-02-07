---
title: Azure-Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen
description: Liste der Dienste, die verwaltete Identitäten für Azure-Ressourcen und die Azure AD-Authentifizierung unterstützen
services: active-directory
author: priyamohanram
ms.author: priyamo
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: daveba
ms.openlocfilehash: 8274909b40d872309230f9c9f6c045b4283fd6eb
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752329"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Mit einer verwalteten Identität können Sie sich bei jedem Dienst authentifizieren, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. Verwaltete Identitäten für Azure-Ressourcen und die Azure AD-Authentifizierung werden derzeit in Azure integriert. Sie sollten sich regelmäßig über Aktualisierungen informieren.

> [!NOTE]
> „Verwaltete Identitäten für Azure-Ressourcen“ ist der neue Name für den Dienst, der früher als „Verwaltete Dienstidentität“ (Managed Service Identity, MSI) bezeichnet wurde.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure-Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen

Die folgenden Azure-Dienste unterstützen verwaltete Identitäten für Azure-Ressourcen:

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

|Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Vorschau | Vorschau | Vorschau | Vorschau |
| Vom Benutzer zugewiesen | Vorschau | Vorschau | Vorschau | Vorschau | Vorschau

Konfigurieren Sie die verwaltete Identität für Azure Virtual Machines anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](qs-configure-cli-windows-vm.md)
- [Azure-Ressourcen-Manager-Vorlagen](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Skalierungsgruppen für virtuelle Azure-Computer

|Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Vorschau | Vorschau | Vorschau |
| Vom Benutzer zugewiesen | Vorschau | Vorschau | Vorschau | Vorschau

Konfigurieren Sie die verwaltete Identität für Azure Virtual Machine Scale Sets anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](qs-configure-cli-windows-vm.md)
- [Azure-Ressourcen-Manager-Vorlagen](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

|Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| Vom Benutzer zugewiesen | Vorschau | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar

Konfigurieren Sie die verwaltete Identität für Azure App Service anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure-Befehlszeilenschnittstelle](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager-Vorlage](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-functions"></a>Azure-Funktionen

Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| Vom Benutzer zugewiesen | Vorschau | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar

Konfigurieren Sie die verwaltete Identität für Azure Functions anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure-Befehlszeilenschnittstelle](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager-Vorlage](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| Vom Benutzer zugewiesen | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar

Konfigurieren Sie die verwaltete Identität für Azure Logic Apps anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Portal](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure Resource Manager-Vorlage](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| Vom Benutzer zugewiesen | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar

Konfigurieren Sie die verwaltete Identität für Azure Data Factory V2 anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Verfügbar | Verfügbar | Nicht verfügbar | Nicht verfügbar |
| Vom Benutzer zugewiesen | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar

Konfigurieren Sie die verwaltete Identität für Azure API Management anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure Resource Manager-Vorlage](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

Typ der verwalteten Identität |  Allgemein verfügbar<br>Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Vom System zugewiesen | Linux: Vorschau<br>Windows: Nicht verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| Vom Benutzer zugewiesen | Linux: Vorschau<br>Windows: Nicht verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar

Konfigurieren Sie die verwaltete Identität für Azure Container Instances anhand der folgenden Liste (in Regionen, in denen sie verfügbar ist):

- [Azure-Befehlszeilenschnittstelle](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager-Vorlage](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)


## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-Dienste, die die Azure AD-Authentifizierung unterstützen

Die folgenden Dienste unterstützen die Azure AD-Authentifizierung und wurden mit Clientdiensten getestet, die verwaltete Identitäten für Azure-Ressourcen verwenden.

| Dienst | Ressourcen-ID | Status | Zuweisen des Zugriffs |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Verfügbar | [Azure-Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure-Befehlszeilenschnittstelle](howto-assign-access-CLI.md) <br>[Azure Resource Manager-Vorlage](../../role-based-access-control/role-assignments-template.md) |
| Azure Key Vault | `https://vault.azure.net` | Verfügbar |  
| Azure Data Lake | `https://datalake.azure.net/` | Verfügbar |
| Azure SQL | `https://database.windows.net/` | Verfügbar |
| Azure Event Hubs | `https://eventhubs.azure.net` | Vorschau |
| Azure-Servicebus | `https://servicebus.azure.net` | Vorschau |
| Azure Storage | `https://storage.azure.com/` | Vorschau |
