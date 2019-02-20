---
title: Kontingente und Einschränkungen für Azure Media Services (v3) | Microsoft-Dokumentation
description: In diesem Thema werden die Kontingente und Einschränkungen für Azure Media Services v3 beschrieben.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 9f5cf0e8be0529ce59edc9aa4cd33d470415c8a6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190958"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Kontingente und Einschränkungen für Azure Media Services (v3)

In diesem Artikel werden die Kontingente und Einschränkungen für Azure Media Services v3 beschrieben.

| Ressource | Standardlimit | 
| --- | --- | 
| Medienobjekte pro Azure Media Services-Konto | 1.000.000|
| Dynamische Manifestfilter|100|
| „JobInputs“ pro Auftrag | 50  (feststehend)|
| „JobOutputs“ pro Auftrag/„TransformOutputs“ in einer Transformation | 20 (feststehend) |
| Dateien pro „JobInput“|10 (feststehend)|
| Dateigröße| In einigen Szenarien werden für die Verarbeitung in Media Services nur Dateien bis zu einer bestimmten Größe unterstützt. <sup>(1)</sup> |
| Aufträge pro Media Services-Konto | 500.000 <sup>(2)</sup> (feststehend)|
| Auflisten von Transformationen|Paginieren Sie die Antwort mit 1.000 Transformationen pro Seite|
| Auflisten von Aufträgen|Paginieren Sie die Antwort mit 500 Transformationen pro Seite|
| Liveereignisse pro Media Services-Konto |5|
| Media Services-Konten in einem einzelnen Abonnement | 25 (feststehend) |
| Liveausgaben im ausgeführten Zustand pro Liveereignis |3|
| Speicherkonten | 100<sup>(4)</sup> (feststehend) |
| Streamingendpunkte (angehalten oder ausgeführt) pro Media Services-Konto|2|
| Streamingrichtlinien | 100 <sup>(3)</sup> |
| Transformationen pro Media Services-Konto | 100  (feststehend)|
| Eindeutige Streaminglocator, die einem Medienobjekt gleichzeitig zugeordnet sind | 100<sup>(5)</sup> (fest) |

<sup>1</sup> In Azure Blob Storage werden derzeit als Größe für ein einzelnes Blob bis zu 5 TB unterstützt. Allerdings gelten basierend auf den vom Dienst verwendeten VM-Größen zusätzliche Grenzwerte in Azure Media Services. Wenn Ihre Quelldatei größer als 260 GB ist, wird Ihr Auftrag wahrscheinlich nicht erfolgreich sein. Bei 4K-Inhalten, die den Grenzwert von 260 GB überschreiten, kontaktieren Sie uns unter amshelp@microsoft.com, um Informationen zu möglichen Lösungen zur Unterstützung Ihres Szenarios zu erhalten.

<sup>2</sup> Diese Zahl umfasst fertig gestellte, aktive und abgebrochene Aufträge sowie Aufträge in der Warteschlange. Gelöschte Aufträge sind nicht enthalten. 

Alle Auftragsdatensätze in Ihrem Konto, die älter als 90 Tage sind, werden automatisch gelöscht, selbst wenn die Gesamtanzahl von Datensätzen unterhalb des maximalen Kontingents liegt. 

<sup>3</sup> Wenn Sie eine benutzerdefinierte [Streamingrichtlinie](https://docs.microsoft.com/rest/api/media/streamingpolicies) verwenden, sollten Sie eine begrenzte Sammlung solcher Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocator wiederverwenden, wenn dieselben Verschlüsselungsoptionen und Protokolle benötigt werden. Sie sollten nicht für jeden Streaminglocator eine neue Streamingrichtlinie erstellen.

<sup>4</sup> Die Speicherkonten müssen aus demselben Azure-Abonnement stammen.

<sup>5</sup> Streaminglocator sind nicht für die Verwaltung der benutzerbezogenen Zugriffssteuerung konzipiert. Um einzelnen Benutzern verschiedene Zugriffsrechte zu erteilen, verwenden Sie Lösungen zur Verwaltung digitaler Rechte (Digital Rights Management, DRM).

## <a name="support-ticket"></a>Supportticket

Für Ressourcen ohne feste Vorgabe können Sie die Erhöhung von Kontingenten anfordern, indem Sie ein [Supportticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) öffnen. Fügen Sie in die Anforderung ausführliche Informationen zu den gewünschten Kontingentänderungen, den Verwendungsfallszenarien und den benötigten Regionen ein. <br/>Erstellen Sie **keine** zusätzlichen Azure Media Services-Konten, um zu versuchen, höhere Grenzwerte zu erzielen.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](media-services-overview.md)
