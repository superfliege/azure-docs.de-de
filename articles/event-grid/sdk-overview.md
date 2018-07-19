---
title: Azure Event Grid-SDKs
description: Beschreibt die SDKs für Azure Event Grid. Diese SDKs bieten Verwaltung, Veröffentlichung und Verwendung.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 06/29/2018
ms.author: tomfitz
ms.openlocfilehash: 3c085074863aa166a5766116b6c63b7dc341ad96
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130834"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Event Grid-SDKs für Verwaltung und Veröffentlichung

Event Grid bietet SDKs, mit denen Sie programmgesteuert Ihre Ressourcen verwalten und Ereignisse bereitstellen können.

## <a name="management-sdks"></a>Verwaltungs-SDKs

Die Verwaltungs-SDKs ermöglichen Ihnen das Erstellen, Aktualisieren und Löschen von Event Grid-Themen und -Abonnements. Derzeit sind folgende SDKs verfügbar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
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

* Beispielanwendungen finden Sie unter [Codebeispiele für Event Grid](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid)
* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Informationen zu Event Grid-Befehlen in der Azure-CLI finden Sie unter [Azure-CLI](/cli/azure/eventgrid).
* Informationen zu Event Grid-Befehlen in PowerShell finden Sie unter [PowerShell](/powershell/module/azurerm.eventgrid).
