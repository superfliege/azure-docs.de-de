---
title: Azure Media Services v3 SDKs – Azure
description: Dieser Artikel enthält eine Übersicht über die ersten Entwicklungsschritte mit der Media Services v3-API und mit SDKs/Tools.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 02/22/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 97123ec795353073b912bd83e0abb45e23549a75
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56749799"
---
# <a name="start-developing-with-media-services-v3-api-using-sdkstools"></a>Einstieg in die Entwicklung mit der Media Services v3-API und mit SDKs/Tools

Als Entwickler können Sie die [REST-API](https://aka.ms/ams-v3-rest-ref) von Media Services oder Clientbibliotheken verwenden, die es Ihnen ermöglichen, mit der REST-API auf einfache Weise zu interagieren, um benutzerdefinierte Medienworkflows zu erstellen, zu verwalten und zu pflegen. Die [Media Services v3](https://aka.ms/ams-v3-rest-sdk)-API basiert auf der OpenAPI-Spezifikation (ehemals Swagger).

Dieses Thema enthält Links zu den SDKs und Tools sowie zur Dokumentation. Außerdem finden Sie hier einige hilfreiche Informationen für verschiedene Entwicklungsumgebungen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Entwicklung mit Media Services benötigen Sie Folgendes:

- Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [Erstellen eines Media Services-Kontos](create-account-cli-how-to.md)

## <a name="start-developing"></a>Beginnen mit der Entwicklung

Azure Media Services unterstützt folgende SDKs/Tools: 

- [Azure-Befehlszeilenschnittstelle](https://aka.ms/ams-v3-cli) 
- [.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk)
- [Python SDK](https://aka.ms/ams-v3-python-sdk)
- [Go SDK](https://aka.ms/ams-v3-go-sdk)
- [Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)

### <a name="azure-media-services-explorer"></a>Azure Media Services Explorer

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) ist ein Tool für Windows-Kunden, die sich über Media Services informieren möchten. Bei AMSE handelt es sich um eine WinForms-/C#-Anwendung zum Hochladen, Herunterladen, Codieren und Streamen von VoD- und Liveinhalten mit Media Services. Das AMSE-Tool richtet sich an Kunden, die Media Services ohne Programmieraufwand testen möchten. Der AMSE-Code wird als Ressource für Kunden bereitgestellt, die mit Media Services entwickeln möchten.

AMSE ist ein Open-Source-Projekt mit communitybasiertem Support. (Probleme können hier gemeldet werden: https://github.com/Azure/Azure-Media-Services-Explorer/issues) Für dieses Projekt wird der [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Verhaltenskodex für Microsoft Open Source) beachtet. Weitere Informationen finden Sie in den [häufig gestellten Fragen zum Verhaltenskodex](https://opensource.microsoft.com/codeofconduct/faq/). Sie können sich auch an opencode@microsoft.com wenden, wenn Sie weitere Fragen oder Anmerkungen haben.

## <a name="rest"></a>REST

Installieren Sie einen REST-Client, um mit der Entwicklung mit Media Services-REST-APIs zu beginnen. Beispiele wären etwa **Postman**, **Visual Studio Code** mit dem REST-Plug-In und **Telerik Fiddler**. Für Media Services v3-Beispiele wird [Postman](media-rest-apis-with-postman.md) verwendet.

Machen Sie sich mit der [REST-API-Referenzdokumentation](https://aka.ms/ams-v3-rest-ref) für Media Services sowie mit den folgenden Beispielen vertraut:

- [Tutorial: Codieren einer Remotedatei anhand einer URL und Streamen des Videos über – REST](stream-files-tutorial-with-rest.md)
- [Hochladen von Dateien in ein Media Services-Konto mit REST](upload-files-rest-how-to.md)
- [Erstellen von Filtern mit der Media Services-REST-API](filters-dynamic-manifest-rest-howto.md)
- [Auf Azure Resource Manager basierte REST-API](https://github.com/Azure-Samples/media-services-v3-arm-templates)

<!-- ## CLI -->
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

Machen Sie sich mit der [Azure CLI-Referenzdokumentation](https://aka.ms/ams-v3-cli-ref) für Media Services sowie mit den folgenden Beispielen vertraut:

- [Skalieren der Medienverarbeitung](media-reserved-units-cli-how-to.md)
- [CLI-Beispiel: Erstellen eines Azure Media Services-Kontos](./scripts/cli-create-account.md) 
- [CLI-Beispiel: Zurücksetzen der Kontoanmeldeinformationen](./scripts/cli-reset-account-credentials.md)
- [CLI-Beispiel: Erstellen eines Medienobjekts](./scripts/cli-create-asset.md)
- [CLI-Beispiel: Hochladen einer lokalen Datei in einen Container](./scripts/cli-upload-file-asset.md)
- [CLI-Beispiel: Erstellen einer Transformation](./scripts/cli-create-transform.md)
- [CLI-Beispiel: Erstellen und Übermitteln eines Auftrags](./scripts/cli-create-jobs.md)
- [CLI-Beispiel: Erstellen eines Azure Event Grid-Abonnements](./scripts/cli-create-event-grid.md)
- [CLI-Beispiel: Veröffentlichen eines Medienobjekts](./scripts/cli-publish-asset.md)
- [Erstellen von Filtern mit der CLI](filters-dynamic-manifest-cli-howto.md)

## <a name="net"></a>.NET

Machen Sie sich mit der [.NET-Referenzdokumentation](https://aka.ms/ams-v3-dotnet-ref) für Media Services sowie mit den folgenden Beispielen vertraut:

- [Tutorial: Hochladen, Codieren und Streamen von Videos mithilfe von .NET](stream-files-tutorial-with-api.md) 
- [Tutorial: Livestreaming mit Media Services v3 unter Verwendung von .NET](stream-live-tutorial-with-api.md)
- [Tutorial: Analysieren von Videos mit Media Services v3 mithilfe von .NET](analyze-videos-tutorial-with-api.md)
- [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md)
- [Erstellen einer Auftragseingabe aus einer HTTPS-URL](job-input-from-http-how-to.md)
- [Codieren einer benutzerdefinierten Transformation](customize-encoder-presets-how-to.md)
- [Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](protect-with-aes128.md)
- [Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts](protect-with-drm.md)
- [Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie](get-content-key-policy-dotnet-howto.md)
- [Erstellen von Filtern mit dem Media Services .NET SDK](filters-dynamic-manifest-dotnet-howto.md)
- [Erweiterte Video-on-Demand-Beispiele von Azure Functions v2 mit Media Services v3](http://aka.ms/ams3functions)

## <a name="java"></a>Java

Machen Sie sich mit der [Java-Referenzdokumentation](https://aka.ms/ams-v3-java-ref) vertraut.

## <a name="nodejs"></a>Node.js

Machen Sie sich mit der [Node.js-Referenzdokumentation](https://aka.ms/ams-v3-nodejs-ref) für Media Services sowie mit [Beispielen](https://github.com/Azure-Samples/media-services-v3-node-tutorials) vertraut, die die Verwendung der Media Services-API mit Node.js veranschaulichen.

## <a name="python"></a>Python

Machen Sie sich mit der [Python-Referenzdokumentation](https://aka.ms/ams-v3-python-ref) vertraut.

## <a name="go"></a>Go

Machen Sie sich mit der [Go-Referenzdokumentation](https://aka.ms/ams-v3-go-ref) vertraut.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Azure Media Services-Kontos](create-account-cli-how-to.md)
- [Zugriff auf Azure Media Services-API mit Azure CLI](access-api-cli-how-to.md)

