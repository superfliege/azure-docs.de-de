---
title: 'Azure Media Services v3: Übersicht | Microsoft Docs'
description: Dieser Artikel bietet einen allgemeinen Überblick über Media Services und enthält Links zu Artikeln, die weitere Details enthalten.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: azure media services, streamen, übertragen, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 06/14/2018
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 489801852202163ef40d57da0082e39793196d85
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264093"
---
# <a name="what-is-azure-media-services-v3"></a>Was ist Azure Media Services v3?

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Version 2: allgemein verfügbar](../previous/media-services-overview.md)
> * [Version 3: Vorschau](media-services-overview.md)

> [!NOTE]
> Die neueste Version von Azure Media Services liegt als Vorschau vor und kann als v3 bezeichnet werden.

Azure Media Services ist eine cloudbasierte Plattform für die Erstellung von Lösungen, die Videostreaming in Broadcastqualität ermöglichen, Barrierefreiheit und Verteilung optimieren, Inhalte analysieren und vieles mehr. Ganz gleich, ob Sie ein Anwendungsentwickler sind oder in einem Callcenter, bei einer Regierungsbehörde oder in einem Unterhaltungsunternehmen arbeiten, Media Services unterstützt Sie bei der Erstellung von Anwendungen, die Medienerlebnisse von herausragender Qualität für ein breites Publikum auf den beliebtesten mobilen Geräten und Browsern von heute bieten. 

## <a name="what-can-i-do-with-media-services"></a>Wie kann ich Media Services nutzen?

Media Services ermöglicht es Ihnen, eine Vielzahl von Medienworkflows in der Cloud zu erstellen. Im Folgenden finden Sie einige Beispiele dafür, was mit Media Services erreicht werden kann.  

* Übermitteln Sie Videos in verschiedenen Formaten, damit sie auf einer Vielzahl von Browsern und Geräten wiedergegeben werden können. Für die bedarfsgesteuerte und Livestreamingbereitstellung für verschiedene Clients (mobile Geräte, TV, PC usw.) müssen die Video- und Audioinhalte ordnungsgemäß codiert und verpackt werden. Informationen zum Bereitstellen und Streamen solcher Inhalte finden Sie unter [Schnellstart: Codieren und Streamen von Dateien](stream-files-dotnet-quickstart.md).
* Streamen Sie Livesportereignisse an ein großes Onlinepublikum, z.B. Fußball, Baseball, Uni- und Schulsport und vieles mehr. 
* Übertragen Sie öffentliche Versammlungen und Veranstaltungen, z.B. in Rathäusern, bei Stadtratssitzungen oder von Gesetzgebungsorganen.
* Analysieren Sie aufgezeichnete Videos oder Audioinhalte. Um beispielsweise eine höhere Kundenzufriedenheit zu erreichen, können Unternehmen Sprache-in-Text extrahieren und Suchindizes und Dashboards erstellen. Sie können dann Daten zu häufigen Beschwerden und ihren Ursachen sowie andere relevante Daten extrahieren. 
* Erstellen Sie einen Abonnementvideodienst, und streamen Sie durch DRM geschützte Inhalte, wenn ein Kunde (z.B. ein Filmstudio) den Zugriff und die Nutzung von urheberrechtlich geschützten Werken einschränken muss.
* Stellen Sie Offlineinhalte für die Wiedergabe in Flugzeugen, Zügen und Autos bereit. Möglicherweise muss ein Kunde Inhalte zur Wiedergabe auf sein Smartphone oder Tablet herunterladen, wenn er eine Trennung vom Netzwerk erwartet.
* Fügen Sie Untertitel zu Videos hinzu, um ein breiteres Publikum anzusprechen (z.B. Menschen mit Hörbehinderungen oder Menschen, die in einer anderen Sprache mitlesen möchten). 
* Implementieren Sie eine E-Learning-Videoplattform mit Azure Media Services und [Azure Cognitive Services-APIs](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=ai) für Sprache-in-Text-Untertitel, Übersetzung in mehrere Sprachen usw.
* Aktivieren Sie Azure CDN, um eine große Skalierung zu erreichen und hohe Auslastungen besser zu bewältigen (z.B. den Beginn einer Produkteinführung). 

## <a name="v3-capabilities"></a>v3-Funktionen

v3 basiert auf einer vereinheitlichten API-Oberfläche, die sowohl Verwaltungs- als auch Betriebsfunktionen auf der Basis von Azure Resource Manager bereitstellt. 

Diese Version bietet die folgenden Funktionen:  

* **Transformationen**, die Ihnen helfen, einfache Workflows von Medienverarbeitungs- oder Analyseaufgaben zu definieren. Die Transformation ist eine Anleitung für die Verarbeitung von Video- und Audiodateien. Sie können diese dann wiederholt anwenden, um alle Dateien in Ihrer Inhaltsbibliothek zu verarbeiten, indem Sie Aufträge an die Transformation übermitteln.
* **Aufträge** zum Verarbeiten (Codieren oder Analysieren) Ihrer Videos. Ein Eingabeinhalt kann über HTTP(S)-URLs, SAS-URLs oder Pfade zu Dateien in Azure Blob Storage angegeben werden. 
* **Benachrichtigungen**, die den Fortschritt des Auftrags oder seine Zustände bzw. den Start/Stopp von Livekanälen und Fehlerereignisse überwachen. Benachrichtigung sind in das Azure Event Grid-Benachrichtigungssystem integriert. Sie können problemlos Ereignisse für mehrere Ressourcen in Azure Media Services abonnieren. 
* Vorlagen der **Azure-Ressourcenverwaltung** können zum Erstellen und Bereitstellen von Transformationen, Streamingendpunkten, Kanälen und mehr verwendet werden.
* **Rollenbasierte Zugriffssteuerung** kann auf Ressourcenebene festgelegt werden, sodass Sie den Zugriff auf bestimmte Ressourcen wie Transformationen, Kanäle und mehr sperren können.
* **Client-SDKs** in mehreren Sprachen: .NET, .NET Core, Python, Go, Java und Node.js.

## <a name="naming-conventions"></a>Benennungskonventionen

Azure Media Services-v3-Ressourcennamen (beispielsweise Objekte, Aufträge und Transformationen) unterliegen den Namenseinschränkungen von Azure Resource Manager. Ressourcennamen sind gemäß den Vorgaben von Azure Resource Manager immer eindeutig. Daher können Sie beliebige Zeichenfolgen für eindeutige Bezeichner (beispielsweise GUIDs) als Ressourcennamen verwenden. 

Media Services-Ressourcennamen dürfen Folgendes nicht enthalten: „<“, „>“, „%“, „&“, „:“, „&#92;“, „?“, „/“, „*“, „+“, „.“, einzelne Anführungszeichen oder Steuerzeichen. Alle anderen Zeichen sind zulässig. Ein Ressourcenname darf maximal 260 Zeichen lang sein. 

Weitere Informationen zu Azure Resource Manager-Benennungen finden Sie in den [Benennungsanforderungen](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) und in den [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="how-can-i-get-started-with-v3"></a>Wie kann ich erste Schritte mit v3 ausführen?

Als Entwickler können Sie die [REST-API](https://go.microsoft.com/fwlink/p/?linkid=873030) von Media Services oder Clientbibliotheken verwenden, die es Ihnen ermöglichen, mit der REST-API auf einfache Weise zu interagieren, um benutzerdefinierte Medienworkflows zu erstellen, zu verwalten und zu warten. Ein REST-Beispiel für Postman finden Sie [hier](https://github.com/Azure-Samples/media-services-v3-rest-postman). Sie können auch die [Azure Resource Manager-basierte REST-API](https://github.com/Azure-Samples/media-services-v3-arm-templates) verwenden.

Microsoft generiert und unterstützt die folgenden Clientbibliotheken: 

|Clientbibliothek|Beispiele|
|---|---|
|[Azure CLI SDK](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)|[Azure CLI-Beispiele](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)|
|[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0)|[Beispiele für .NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials)|
|[.NET Core SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0) (Klicken Sie auf die Registerkarte **.NET CLI**.)|[.NET Core-Beispiele](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials)|
|[Java SDK](https://docs.microsoft.com/java/api/overview/azure/mediaservices)||
|[Node.js SDK](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/index?view=azure-node-latest)|[Beispiele für Node.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials)|
|[Python SDK](https://pypi.org/project/azure-mgmt-media/1.0.0rc1/)||
|[Go SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/mediaservices/mgmt/2018-03-30-preview/media)||

Media Services bietet [Swagger-Dateien](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media), die Sie verwenden können, um SDKs für Ihre bevorzugte Sprache/Technologie zu generieren.  

## <a name="next-steps"></a>Nächste Schritte

Um zu erfahren, wie einfach es ist, mit der Codierung und dem Streaming von Videodateien zu beginnen, lesen Sie [Streamen von Dateien](stream-files-dotnet-quickstart.md). 

