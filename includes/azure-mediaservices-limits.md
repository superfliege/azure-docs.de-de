---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 1e1316ef568cbc6409a8653022d9acff9837b59d
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279645"
---
>[!NOTE]
>Für Ressourcen ohne feste Vorgabe können Sie die Erhöhung von Kontingenten anfordern, indem Sie ein Supportticket öffnen. Erstellen Sie **keine** zusätzlichen Azure Media Services-Konten, um zu versuchen, höhere Grenzwerte zu erzielen.

| Ressource | Standardlimit | 
| --- | --- | 
| Azure Media Services (AMS)-Konten in einem einzelnen Abonnement | 25 (feststehend) |
| Reservierte Einheiten für Medien (Media Reserved Units; RUs) pro AMS-Konto |25 (S1)<br/>10 (S2, S3) <sup>(1)</sup> | 
| Aufträge pro AMS-Konto | 50,000<sup>(2)</sup> |
| Verkettete Aufgaben pro Auftrag | 30 (feststehend) |
| Objekte pro AMS-Konto | 1.000.000|
| Objekte pro Aufgabe | 50 |
| Objekte pro Auftrag | 100 |
| Eindeutige Locators, die einem Objekt gleichzeitig zugeordnet sind | 5<sup>(4)</sup> |
| Livekanäle pro AMS-Konto  |5|
| Programme im angehaltenen Zustand pro Kanal  |50|
| Programme im ausgeführten Zustand pro Kanal  |3|
| Streamingendpunkte im ausgeführten Zustand pro AMS-Konto|2|
| Streamingeinheiten pro Streamingendpunkt  |10 |
| Speicherkonten | 1.000<sup>(5)</sup> (feststehend) |
| Richtlinien | 1,000,000<sup>(6)</sup> |
| Dateigröße| In einigen Szenarien werden für die Verarbeitung in Media Services nur Dateien bis zu einer bestimmten Größe unterstützt. <sup>7</sup> |
  
<sup>1</sup> Wenn Sie den Typ ändern (etwa von S2 in S1), werden die maximalen RU-Grenzwerte zurückgesetzt.

<sup>2</sup> Diese Zahl umfasst fertig gestellte, aktive und abgebrochene Aufträge sowie Aufträge in der Warteschlange. Gelöschte Aufträge sind nicht enthalten. Sie können die alten Aufträge mithilfe von **IJob.Delete** oder der HTTP **DELETE**-Anforderung löschen.

Ab dem 1. April 2017 werden alle Auftragsdatensätze in Ihrem Konto, die älter als 90 Tage sind, sowie alle zugehörigen Aufgabendatensätze automatisch gelöscht, selbst wenn die Gesamtanzahl von Datensätzen unterhalb des maximalen Kontingents liegt. Wenn Sie die Auftrags-/Aufgabeninformationen archivieren müssen, können Sie dazu den [hier](../articles/media-services/previous/media-services-dotnet-manage-entities.md) beschriebenen Code verwenden.

<sup>3</sup> Wenn Sie eine Anforderung zum Auflisten von Auftragsentitäten senden, werden maximal 1.000 Aufträge pro Anforderung zurückgegeben. Falls Sie alle gesendeten Aufträge nachverfolgen müssen, können Sie "Nach oben"/"Überspringen" wie in [OData-Systemabfrageoptionen](https://msdn.microsoft.com/library/gg309461.aspx)beschrieben verwenden.

<sup>4</sup> Locators sind nicht für die Verwaltung der Zugriffssteuerung pro Benutzer konzipiert. Um einzelnen Benutzern verschiedene Zugriffsrechte zu erteilen, verwenden Sie Lösungen zur Verwaltung digitaler Rechte (Digital Rights Management, DRM). Weitere Informationen finden Sie in [diesem](../articles/media-services/previous/media-services-content-protection-overview.md) Abschnitt.

<sup>5</sup> Die Speicherkonten müssen aus demselben Azure-Abonnement stammen.

<sup>6</sup> Es gilt ein Grenzwert von 1.000.000 Richtlinien für verschiedene AMS-Richtlinien (z.B. für die Locator-Richtlinie oder für ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Wenn Sie immer die gleichen Tage und Zugriffsberechtigungen usw. verwenden, sollten Sie die gleiche Richtlinien-ID verwenden. Weitere Informationen und ein Beispiel finden Sie in [diesem](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies) Abschnitt.

<sup>7</sup>Berücksichtigen Sie die Einschränkungen hinsichtlich der unterstützten maximalen Dateigrößen, wenn Sie Inhalte an ein Medienobjekt in Azure Media Services hochladen und mit einem der Medienprozessoren im Dienst (also mit Encodern wie Media Encoder Standard und Media Encoder Premium Workflow oder mit Analyse-Engines wie Gesichtserkennung) verarbeiten möchten. 

In Azure Blob Storage werden derzeit als Größe für ein einzelnes Blob bis zu 5 TB unterstützt. Allerdings gelten basierend auf den vom Dienst verwendeten VM-Größen zusätzliche Grenzwerte in Azure Media Services. Die folgende Tabelle enthält die Grenzwerte für die einzelnen reservierten Einheiten für Medien (S1, S2, S3.) Wenn die Quelldatei größer als die in der Tabelle definierten Grenzwerte ist, tritt beim Codierungsauftrag ein Fehler auf. Beim Codieren von Quellen mit 4K-Auflösung und langer Dauer müssen Sie reservierte Einheiten für Medien vom Typ „S3“ verwenden, um die erforderliche Leistung zu erzielen. Bei 4K-Inhalten, die den Grenzwert von 260 GB für reservierte Einheiten für Medien vom Typ „S3“ überschreiten, kontaktieren Sie uns unter amshelp@microsoft.com, um Informationen zu möglichen Lösungen zur Unterstützung Ihres Szenarios zu erhalten.

| Reservierte Einheiten für Medien (Typ) | Maximale Eingabegröße (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
