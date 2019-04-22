---
title: Entwickeln mit v3-APIs – Azure | Microsoft-Dokumentation
description: In diesem Artikel werden Regeln erläutert, die für Entitäten und APIs gelten, wenn Sie mit Media Services v3 entwickeln.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 18b72ceaee0ca0747a0bf2144d5f9ffddbee8b8c
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471788"
---
# <a name="developing-with-media-services-v3-apis"></a>Entwickeln mit Media Services v3-APIs

In diesem Artikel werden Regeln erläutert, die für Entitäten und APIs gelten, wenn Sie mit Media Services v3 entwickeln.

## <a name="naming-conventions"></a>Benennungskonventionen

Azure Media Services-v3-Ressourcennamen (beispielsweise Objekte, Aufträge und Transformationen) unterliegen den Namenseinschränkungen von Azure Resource Manager. Ressourcennamen sind gemäß den Vorgaben von Azure Resource Manager immer eindeutig. Daher können Sie beliebige Zeichenfolgen für eindeutige Bezeichner (beispielsweise GUIDs) als Ressourcennamen verwenden. 

Media Services-Ressourcennamen dürfen Folgendes nicht enthalten: „<“, „>“, „%“, „&“, „:“, „&#92;“, „?“, „/“, „*“, „+“, „.“, einzelne Anführungszeichen oder Steuerzeichen. Alle anderen Zeichen sind zulässig. Ein Ressourcenname darf maximal 260 Zeichen lang sein. 

Weitere Informationen zur Benennung in Azure Resource Manager finden Sie unter: [Namensanforderungen](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) und [Namenskonvention](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles-and-rbac"></a>Entwurfsprinzipien und rollenbasierte Zugriffssteuerung (RBAC) in der v3-API

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Der Benutzer muss eine separate Aktionsmethode zum Abrufen von Geheimnissen oder Anmeldeinformationen aufrufen. Die **Leser**-Rolle kann keine Vorgänge aufrufen, also auch keine Vorgänge wie „Asset.ListContainerSas“, „StreamingLocator.ListContentKeys“ oder „ContentKeyPolicies.GetPolicyPropertiesWithSecrets“. Getrennte Aktionen ermöglichen es Ihnen, bei Bedarf noch genauer abgestimmte RBAC-Sicherheitsberechtigungen in einer benutzerdefinierten Rolle festzulegen.

Weitere Informationen finden Sie unter

- [Definitionen integrierter Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)
- [Verwenden der rollenbasierten Zugriffssteuerung (RBAC) zum Verwalten des Zugriffs](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Rollenbasierte Zugriffssteuerung für Media Services-Konten](rbac-overview.md)
- [Abrufen der Richtlinie für symmetrische Schlüssel – .NET](get-content-key-policy-dotnet-howto.md)

## <a name="long-running-operations"></a>Zeitintensive Vorgänge

Die in den [swagger-Dateien](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) von Azure Media Services mit `x-ms-long-running-operation` gekennzeichneten Vorgänge sind zeitintensive Vorgänge. 

Weitere Informationen zum Verfolgen asynchroner Azure-Vorgänge finden Sie unter [Asynchrone Vorgänge](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation).

Media Services verfügt über die folgenden zeitintensiven Vorgänge:

* Create LiveEvent
* Update LiveEvent
* Delete LiveEvent
* Start LiveEvent
* Stop LiveEvent
* Reset LiveEvent
* Create LiveOutput
* Delete LiveOutput
* Create StreamingEndpoint
* Update StreamingEndpoint
* Delete StreamingEndpoint
* Start StreamingEndpoint
* Stop StreamingEndpoint
* Scale StreamingEndpoint

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Filterung, Sortierung und Paginierung von Media Services-Entitäten

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Azure Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

[Einstieg in die Entwicklung mit der Media Services v3-API und mit SDKs/Tools](developers-guide.md)
