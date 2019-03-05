---
title: 'Azure Media Services v3: Übersicht | Microsoft Docs'
description: Dieser Artikel bietet einen allgemeinen Überblick über Media Services und enthält Links zu Artikeln, die weitere Details enthalten.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, streamen, übertragen, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 02/26/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: de1bd8599682baae860737401d02c7c1dec9e223
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960648"
---
# <a name="what-is-azure-media-services-v3"></a>Was ist Azure Media Services v3?

Azure Media Services ist eine cloudbasierte Plattform für die Erstellung von Lösungen, die Videostreaming in Broadcastqualität ermöglichen, Barrierefreiheit und Verteilung optimieren, Inhalte analysieren und vieles mehr. Ganz gleich, ob Sie ein Anwendungsentwickler sind oder in einem Callcenter, bei einer Regierungsbehörde oder in einem Unterhaltungsunternehmen arbeiten, Media Services unterstützt Sie bei der Erstellung von Anwendungen, die Medienerlebnisse von herausragender Qualität für ein breites Publikum auf den beliebtesten mobilen Geräten und Browsern von heute bieten. 

## <a name="what-can-i-do-with-media-services"></a>Wie kann ich Media Services nutzen?

Media Services ermöglicht es Ihnen, eine Vielzahl von Medienworkflows in der Cloud zu erstellen. Im Folgenden finden Sie einige Beispiele dafür, was mit Media Services erreicht werden kann.  

* Übermitteln Sie Videos in verschiedenen Formaten, damit sie auf einer Vielzahl von Browsern und Geräten wiedergegeben werden können. Für die bedarfsgesteuerte und Livestreamingbereitstellung für verschiedene Clients (mobile Geräte, TV, PC usw.) müssen die Video- und Audioinhalte ordnungsgemäß codiert und verpackt werden. Informationen zum Bereitstellen und Streamen solcher Inhalte finden Sie unter [Schnellstart: Codieren und Streamen von Dateien](stream-files-dotnet-quickstart.md).
* Streamen Sie Livesportereignisse an ein großes Onlinepublikum, z.B. Fußball, Baseball, Uni- und Schulsport und vieles mehr. 
* Übertragen Sie öffentliche Versammlungen und Veranstaltungen, z.B. in Rathäusern, bei Stadtratssitzungen oder von Gesetzgebungsorganen.
* Analysieren Sie aufgezeichnete Videos oder Audioinhalte. Um beispielsweise eine höhere Kundenzufriedenheit zu erreichen, können Unternehmen Sprache-in-Text extrahieren und Suchindizes und Dashboards erstellen. Sie können dann Daten zu häufigen Beschwerden und ihren Ursachen sowie andere relevante Daten extrahieren.
* Erstellen Sie einen Abonnementvideodienst, und streamen Sie durch DRM geschützte Inhalte, wenn ein Kunde (z.B. ein Filmstudio) den Zugriff und die Nutzung von urheberrechtlich geschützten Werken einschränken muss.
* Stellen Sie Offlineinhalte für die Wiedergabe in Flugzeugen, Zügen und Autos bereit. Möglicherweise muss ein Kunde Inhalte zur Wiedergabe auf sein Smartphone oder Tablet herunterladen, wenn er eine Trennung vom Netzwerk erwartet.
* Implementieren Sie eine E-Learning-Videoplattform mit Azure Media Services und [Azure Cognitive Services-APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) für Sprache-in-Text-Untertitel, Übersetzung in mehrere Sprachen usw. 
* Verwenden Sie Azure Media Services zusammen mit [Azure Cognitive Services-APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai), um Untertitel zu Videos hinzuzufügen und so ein breiteres Publikum anzusprechen (z.B. Menschen mit Hörbehinderungen oder Menschen, die in einer anderen Sprache mitlesen möchten).
* Aktivieren Sie Azure CDN, um eine umfassende Skalierung zu erreichen und hohe Auslastungen besser zu bewältigen (z.B. den Beginn einer Produkteinführung). 

## <a name="v3-capabilities"></a>v3-Funktionen

v3 basiert auf einer vereinheitlichten API-Oberfläche, die sowohl Verwaltungs- als auch Betriebsfunktionen auf der Basis von Azure Resource Manager bereitstellt. 

Diese Version bietet die folgenden Funktionen:  

* **Transformationen**, die Ihnen helfen, einfache Workflows von Medienverarbeitungs- oder Analyseaufgaben zu definieren. Die Transformation ist eine Anleitung für die Verarbeitung von Video- und Audiodateien. Sie können diese dann wiederholt anwenden, um alle Dateien in Ihrer Inhaltsbibliothek zu verarbeiten, indem Sie Aufträge an die Transformation übermitteln.
* **Aufträge** zum Verarbeiten (Codieren oder Analysieren) Ihrer Videos. Ein Eingabeinhalt kann über HTTPS-URLs, SAS-URLs oder Pfade zu Dateien in Azure Blob Storage angegeben werden. AMS v3 unterstützt derzeit keine segmentierte Transfercodierung über HTTPS-URLs.
* **Benachrichtigungen**, die den Fortschritt des Auftrags oder seine Zustände bzw. den Start/Stopp von Livekanälen und Fehlerereignisse überwachen. Benachrichtigung sind in das Azure Event Grid-Benachrichtigungssystem integriert. Sie können problemlos Ereignisse für mehrere Ressourcen in Azure Media Services abonnieren. 
* Vorlagen der **Azure-Ressourcenverwaltung** können zum Erstellen und Bereitstellen von Transformationen, Streamingendpunkten, Kanälen und mehr verwendet werden.
* **Rollenbasierte Zugriffssteuerung** kann auf Ressourcenebene festgelegt werden, sodass Sie den Zugriff auf bestimmte Ressourcen wie Transformationen, Kanäle und mehr sperren können.
* **Client-SDKs** in mehreren Sprachen: .NET, .NET Core, Python, Go, Java und Node.js.

## <a name="naming-conventions"></a>Benennungskonventionen

Azure Media Services-v3-Ressourcennamen (beispielsweise Objekte, Aufträge und Transformationen) unterliegen den Namenseinschränkungen von Azure Resource Manager. Ressourcennamen sind gemäß den Vorgaben von Azure Resource Manager immer eindeutig. Daher können Sie beliebige Zeichenfolgen für eindeutige Bezeichner (beispielsweise GUIDs) als Ressourcennamen verwenden. 

Media Services-Ressourcennamen dürfen Folgendes nicht enthalten: „<“, „>“, „%“, „&“, „:“, „&#92;“, „?“, „/“, „*“, „+“, „.“, einzelne Anführungszeichen oder Steuerzeichen. Alle anderen Zeichen sind zulässig. Ein Ressourcenname darf maximal 260 Zeichen lang sein. 

Weitere Informationen zur Benennung in Azure Resource Manager finden Sie unter: [Namensanforderungen](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) und [Namenskonvention](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles"></a>Entwurfsprinzipien der v3-API

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Sie müssen eine separate Aktionsmethode zum Abrufen von geheimen Schlüsseln oder Anmeldeinformationen aufrufen. Separate Aktionen ermöglichen es Ihnen, verschiedene RBAC-Sicherheitsberechtigungen festzulegen, falls einige APIs geheime Schlüssel abrufen/anzeigen, während dies bei anderen APIs nicht der Fall ist. Informationen darüber, wie Sie den Zugriff über RBAC verwalten können, finden Sie unter [Verwenden von RBAC zum Verwalten des Zugriffs](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Beispiele hierfür sind: 

* Keine Rückgabe von ContentKey-Werten im Get-Vorgang von StreamingLocator. 
* Keine Rückgabe der Einschränkungsschlüssel im Get-Vorgang von ContentKeyPolicy. 
* Keine Rückgabe des Abfragezeichenfolgeteils der URL (um die Signatur zu entfernen) der HTTP-Eingabe-URLs von Aufträgen.

Sehen Sie sich das Beispiel zum [Abrufen der Richtlinie für den Inhaltsschlüssel (.NET)](get-content-key-policy-dotnet-howto.md) an.

## <a name="how-can-i-get-started-with-v3"></a>Wie kann ich erste Schritte mit v3 ausführen?

Informationen zum Einstieg in die Entwicklung mit der Media Services v3-API und verschiedenen Tools und SDKs finden Sie unter [Beginnen mit der Entwicklung](developers-guide.md).

## <a name="v3-content-map"></a>v3-Inhaltszuordnung

Der Media Services v3-Inhalt ist wie folgt strukturiert (wie auch im Inhaltsverzeichnis zu sehen):

|Abschnitte| BESCHREIBUNG|
|---|---|
| Übersicht | Hier werden die Funktionen von Media Services und die Möglichkeiten erläutert, die Ihnen der Dienst bietet.|
| Schnellstarts | Hier finden Sie grundlegende Anleitungen für Einsteiger, mit denen neue Kunden Media Services schnell ausprobieren können.|
| Lernprogramme | Hier finden Sie szenariobasierte Verfahren für einige der wichtigsten Media Services-Aufgaben.|
| Beispiele | Hier finden Sie Links zu Codebeispielen. |
| Konzepte | Hier finden Sie ausführliche Erläuterungen zu [Media Services v3-Konzepten und -Funktionen](concepts-overview.md). Es empfiehlt sich, sich vor Entwicklungsbeginn mit den grundlegenden Konzepten vertraut zu machen, die in diesen Themen behandelt werden.<br/><br/>* Clouduploads und Cloudspeicherung<br/>* Codierung<br/>* Medienanalyse<br/>* Verpackung, Bereitstellung, Schutz<br/>* Livestreaming<br/>* Überwachung<br/>* Beteiligte Kunden<br/><br/>und vieles mehr... |
| Anleitungen | Hier erfahren Sie, wie Sie eine Aufgabe ausführen.|

## <a name="next-steps"></a>Nächste Schritte

Um zu erfahren, wie einfach es ist, mit der Codierung und dem Streaming von Videodateien zu beginnen, lesen Sie [Streamen von Dateien](stream-files-dotnet-quickstart.md). 

