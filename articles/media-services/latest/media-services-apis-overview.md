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
ms.date: 04/15/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 5ae7563892cb4792f5c329b2850d7b88d37c0e7d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698265"
---
# <a name="developing-with-media-services-v3-apis"></a>Entwickeln mit Media Services v3-APIs

In diesem Artikel werden Regeln erläutert, die für Entitäten und APIs gelten, wenn Sie mit Media Services v3 entwickeln.

## <a name="accessing-the-azure-media-services-api"></a>Zugreifen auf die Azure Media Services-API

Um auf die Azure Media Services-Ressourcen zuzugreifen, können Sie die Dienstprinzipalauthentifizierung von Azure Active Directory (AD) verwenden.
Die Media Services-API erfordert, dass der Benutzer oder die Anwendung, die die REST-API-Anforderungen sendet, Zugriff auf die Media Services-Kontoressource hat und eine Rolle **Mitwirkender** oder **Besitzer** verwendet. Auf die API kann mit der Rolle **Leser** zugegriffen werden, es sind aber nur **Get**- oder **List** -Operationen verfügbar. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung für Media Services-Konten](rbac-overview.md).

Anstatt ein Dienstprinzipal zu erstellen, sollten Sie die Verwendung verwalteter Identitäten für Azure-Ressourcen in Betracht ziehen, um über den Azure Resource Manager auf die Media Services-API zuzugreifen. Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-ad-service-principal"></a>Azure AD-Dienstprinzipal 

Wenn Sie eine Azure AD-Anwendung und ein Dienstprinzipal erstellen, muss sich die Anwendung in ihrem eigenen Mandanten befinden. Nachdem Sie die Anwendung erstellt haben, können Sie für die App den Zugriff auf das Media Services-Konto über die Rolle **Mitwirkender** oder **Besitzer** gewähren. 

Wenn Sie sich nicht sicher sind, ob Sie über Berechtigungen zum Erstellen einer Azure AD-Anwendung verfügen, lesen Sie bitte [Erforderliche Berechtigungen](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

In der folgenden Abbildung stellen die Zahlen den Fluss der Anforderungen in chronologischer Reihenfolge dar:

![Apps der mittleren Ebene](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Eine App der mittleren Ebene fordert ein Azure AD-Zugriffstoken an, das die folgenden Parameter enthält:  

   * Azure AD-Mandantenendpunkt.
   * Media Services-Ressourcen-URI.
   * Ressourcen-URI für REST Media Services
   * Werte der Azure AD-Anwendung: Client-ID und geheimer Clientschlüssel.
   
   Informationen zum Abrufen aller erforderlichen Werte finden Sie in [Zugriff auf Azure Media Services API mit Azure CLI](access-api-cli-how-to.md)

2. Das Azure AD-Zugriffstoken wird an die mittlere Ebene gesendet.
4. Die mittlere Ebene sendet eine Anforderung mit dem Azure AD-Token an die Azure Media-REST-API.
5. Die mittlere Ebene erhält die Daten von den Media Services zurück.

## <a name="naming-conventions"></a>Benennungskonventionen

Azure Media Services-v3-Ressourcennamen (beispielsweise Objekte, Aufträge und Transformationen) unterliegen den Namenseinschränkungen von Azure Resource Manager. Ressourcennamen sind gemäß den Vorgaben von Azure Resource Manager immer eindeutig. Daher können Sie beliebige Zeichenfolgen für eindeutige Bezeichner (beispielsweise GUIDs) als Ressourcennamen verwenden. 

Media Services-Ressourcennamen dürfen Folgendes nicht enthalten: „<“, „>“, „%“, „&“, „:“, „&#92;“, „?“, „/“, „*“, „+“, „.“, einzelne Anführungszeichen oder Steuerzeichen. Alle anderen Zeichen sind zulässig. Ein Ressourcenname darf maximal 260 Zeichen lang sein. 

Weitere Informationen zur Benennung in Azure Resource Manager finden Sie unter: [Namensanforderungen](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) und [Namenskonvention](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

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
