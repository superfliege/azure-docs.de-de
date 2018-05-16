---
title: Kontingente und Einschränkungen für Azure Media Services (v3) | Microsoft-Dokumentation
description: In diesem Thema werden die Kontingente und Einschränkungen für Azure Media Services v3 beschrieben.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 21fc80d7cb274197ae75d2fd5524e76e1e6288d9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Kontingente und Einschränkungen für Azure Media Services (v3)

In diesem Thema werden die Kontingente und Einschränkungen für Azure Media Services v3 beschrieben.

| Ressource | Standardlimit | 
| --- | --- | 
| Medienobjekte pro Azure Media Services-Konto | 1.000.000|
| „JobInputs“ pro Auftrag | 100 |
| „JobOutputs“ pro Auftrag | 30 (feststehend) |
| Dateigröße| In einigen Szenarien werden für die Verarbeitung in Media Services nur Dateien bis zu einer bestimmten Größe unterstützt. <sup>(1)</sup> |
| Aufträge pro Media Services-Konto | 50.000<sup>(2)</sup> |
| „LiveEvents“ pro Media Services-Konto |5|
| Media Services-Konten in einem einzelnen Abonnement | 25 (feststehend) |
| StreamingPolicies | 1.000.000<sup>(3)</sup> |
| „LiveOutputs“ im Ausführungsstatus pro „LiveEvent“ |3|
| „LiveOutputs“ im beendeten Status pro „LiveEvent“ |50|
| Speicherkonten | 1.000<sup>(4)</sup> (fest) |
| Streamingendpunkte im ausgeführten Status pro Media Services-Konto|2|
| Transformationen pro Media Services-Konto | 20 |
| Eindeutige „StreamingLocators“, die einem Medienobjekt gleichzeitig zugeordnet sind | 20<sup>(5)</sup> |
  
<sup>1</sup>In Azure Blob Storage wird derzeit als Größe für ein einzelnes Blob bis zu 5 TB unterstützt. Allerdings gelten basierend auf den vom Dienst verwendeten VM-Größen zusätzliche Grenzwerte in Azure Media Services. Wenn Ihre Quelldatei größer als 260 GB ist, wird Ihr Auftrag wahrscheinlich nicht erfolgreich sein. Bei 4K-Inhalten, die den Grenzwert von 260 GB überschreiten, kontaktieren Sie uns unter amshelp@microsoft.com, um Informationen zu möglichen Lösungen zur Unterstützung Ihres Szenarios zu erhalten.

<sup>2</sup> Diese Zahl umfasst fertig gestellte, aktive und abgebrochene Aufträge sowie Aufträge in der Warteschlange. Gelöschte Aufträge sind nicht enthalten. 

Alle Auftragsdatensätze in Ihrem Konto, die älter als 90 Tage sind, werden automatisch gelöscht, selbst wenn die Gesamtanzahl von Datensätzen unterhalb des maximalen Kontingents liegt. 

<sup>3</sup>Es gilt ein Grenzwert von 1 Mio. „StreamingPolicy“-Einträgen für verschiedene Media Services-Richtlinien (z.B. für die „StreamingLocator“-Richtlinie oder für „ContentKeyAuthorizationPolicy“). 

>[!NOTE]
> Wenn Sie immer die gleichen Tage und Zugriffsberechtigungen usw. verwenden, sollten Sie die gleiche Richtlinien-ID verwenden. 

<sup>4</sup> Die Speicherkonten müssen aus demselben Azure-Abonnement stammen.

<sup>5</sup> StreamingLocators sind nicht für die Verwaltung der benutzerbezogenen Zugriffssteuerung konzipiert. Um einzelnen Benutzern verschiedene Zugriffsrechte zu erteilen, verwenden Sie Lösungen zur Verwaltung digitaler Rechte (Digital Rights Management, DRM).

## <a name="support-ticket"></a>Supportticket

Für Ressourcen ohne feste Vorgabe können Sie die Erhöhung von Kontingenten anfordern, indem Sie ein [Supportticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) öffnen. Fügen Sie in die Anforderung ausführliche Informationen zu den gewünschten Kontingentänderungen, den Verwendungsfallszenarien und den benötigten Regionen ein. <br/>Erstellen Sie **keine** zusätzlichen Azure Media Services-Konten, um zu versuchen, höhere Grenzwerte zu erzielen.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](media-services-overview.md)
