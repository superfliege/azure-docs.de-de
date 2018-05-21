---
title: Azure Event Grid-SDKs
description: Beschreibt die SDKs für Azure Event Grid. Diese SDKs bieten Verwaltung, Veröffentlichung und Verwendung.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 82f08341f8c96695a4ceddb19d1b610d70b89f88
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Event Grid-SDKs für Verwaltung und Veröffentlichung

Event Grid bietet SDKs, mit denen Sie programmgesteuert Ihre Ressourcen verwalten und Ereignisse bereitstellen können.

## <a name="management-sdks"></a>Verwaltungs-SDKs

Die Verwaltungs-SDKs ermöglichen Ihnen das Erstellen, Aktualisieren und Löschen von Event Grid-Themen und -Abonnements. Derzeit sind folgende SDKs verfügbar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure.eventgrid-2018-01-01/azure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>SDKs für Datenebenen

Mit den Datenebenen-SDKs können Sie Ereignisse durch Authentifizieren, Bilden des Ereignisses und asynchrones Bereitstellen am angegebenen Endpunkt Themen zur Verfügung stellen. Sie ermöglichen außerdem die Nutzung von Erstanbieterereignissen. Derzeit sind folgende SDKs verfügbar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Informationen zu Event Grid-Befehlen in der Azure-CLI finden Sie unter [Azure-CLI](/cli/azure/eventgrid).
* Informationen zu Event Grid-Befehlen in PowerShell finden Sie unter [PowerShell](/powershell/module/azurerm.eventgrid).