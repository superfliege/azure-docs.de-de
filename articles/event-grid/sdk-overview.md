---
title: Azure Event Grid-SDKs
description: "Beschreibt die SDKs für Azure Event Grid. Diese SDKs bieten Verwaltung, Veröffentlichung und Verwendung."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9c56e4c3314090ad55017d5c681a0cfd7bf5722c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Event Grid-SDKs für Verwaltung und Veröffentlichung

Event Grid bietet SDKs, mit denen Sie programmgesteuert Ihre Ressourcen verwalten und Ereignisse bereitstellen können.

## <a name="management-sdks"></a>Verwaltungs-SDKs

Die Verwaltungs-SDKs ermöglichen Ihnen das Erstellen, Aktualisieren und Löschen von Event Grid-Themen und -Abonnements. Derzeit sind folgende SDKs verfügbar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="publish-sdks"></a>Veröffentlichungs-SDKs

Mit den Veröffentlichungs-SDKs können Sie Ereignisse durch Authentifizieren, Bilden des Ereignisses und asynchrones Bereitstellen am angegebenen Endpunkt Themen zur Verfügung stellen. Derzeit sind folgende SDKs verfügbar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Informationen zu Event Grid-Befehlen in der Azure-CLI finden Sie unter [Azure-CLI](/cli/azure/eventgrid).
* Informationen zu Event Grid-Befehlen in PowerShell finden Sie unter [PowerShell](/powershell/module/azurerm.eventgrid).