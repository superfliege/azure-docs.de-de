---
title: Hinzufügen von Blobs zu Objekten in Azure Digital Twins | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Objekten in Azure Digital Twins Blobs hinzufügen.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 36f4caac38f2f4891af6f61b78b55c7eff15eae4
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54116737"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Hinzufügen von Blobs zu Objekten in Azure Digital Twins

Blobs sind unstrukturierte Darstellungen gängiger Dateitypen (z. B. Bilder oder Protokolle). Blobs verfolgen mithilfe eines MIME-Typs (beispielsweise „image/jpeg“) sowie Metadaten (Name, Beschreibung, Typ usw.) die Art der dargestellten Daten nach.

Azure Digital Twins unterstützt das Anfügen von Blobs an Geräte, Räume und Benutzer. Blobs können ein Profilbild für einen Benutzer, ein Gerätefoto, ein Video, eine Karte, ein Firmware-ZIP-Archiv, JSON-Daten, ein Protokoll o.ä. darstellen.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-an-overview"></a>Hochladen von Blobs: Übersicht

Sie können Blobs unter Verwendung mehrteiliger Anforderungen in bestimmte Endpunkte und deren jeweilige Funktionen hochladen.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Blobmetadaten

Neben **Content-Type** und **Content-Disposition** müssen mehrteilige Anforderungen für Azure Digital Twins-Blobs auch den richtigen JSON-Text angeben. Der zu übermittelnde JSON-Text hängt von der Art des ausgeführten HTTP-Anforderungsvorgangs ab.

Nachfolgend finden Sie die vier wichtigsten JSON-Schemas:

![JSON-Schemas][1]

In der Swagger-Dokumentation werden diese Modell-Schemas umfassend beschrieben.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Informationen zur Verwendung der Referenzdokumentation finden Sie unter [Verwenden von Azure Digital Twins-Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Beispiele

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Um eine Textdatei als Blob hochzuladen und einem Bereich zuzuordnen, richten Sie eine authentifizierte HTTP POST-Anforderung an:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Verwenden Sie folgenden Text:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Wert | Ersetzen durch |
| --- | --- |
| USER_DEFINED_BOUNDARY | Name für die Begrenzung mehrteiliger Inhalte |

Der folgende Code ist eine .NET-Implementierung des gleichen Blob-Uploads mit der Klasse [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

In beiden Beispielen gilt:

1. Vergewissern Sie sich, dass die Header Folgendes enthalten: `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
1. Vergewissern Sie sich, dass der Hauptteil mehrteilig ist:

   - Der erste Teil enthält die erforderliche Metadaten für das Blob.
   - Der zweite Teil enthält die Textdatei.

1. Vergewissern Sie sich, dass die Textdatei als `Content-Type: text/plain` bereitgestellt wird.

## <a name="api-endpoints"></a>API-Endpunkte

Die folgenden Abschnitte beschreiben die wichtigsten blobbezogenen API-Endpunkte und ihre Funktionen.

### <a name="devices"></a>Geräte

Blobs können an Geräte angefügt werden. Die folgende Abbildung zeigt die Swagger-Referenzdokumentation für Ihre Verwaltungs-APIs. Sie gibt gerätebezogene API-Endpunkte für die Blobnutzung sowie ggf. zu übergebende erforderliche Pfadparameter an.

![Geräteblobs][2]

Wenn Sie beispielsweise ein Blob aktualisieren oder erstellen und dann an ein Gerät anfügen möchten, richten Sie eine authentifizierte HTTP PATCH-Anforderung an:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID |

Bei erfolgreichen Anforderungen wird in der Antwort ein **DeviceBlob**-JSON-Objekt zurückgegeben. **DeviceBlob**-Objekte entsprechen dem folgenden JSON-Schema:

| Attribut | Typ | BESCHREIBUNG | Beispiele |
| --- | --- | --- | --- |
| **DeviceBlobType** | Zeichenfolge | Eine Blobkategorie, die an ein Gerät angefügt werden kann | `Model` und `Specification` |
| **DeviceBlobSubtype** | Zeichenfolge | Eine Blobunterkategorie, die präziser ist als **DeviceBlobType** | `PhysicalModel`, `LogicalModel`, `KitSpecification` und `FunctionalSpecification` |

> [!TIP]
> Verwenden Sie die vorherige Tabelle, um erfolgreich zurückgegebene Anforderungsdaten zu verarbeiten.

### <a name="spaces"></a>Leerzeichen

Blobs können auch an Räume angefügt werden. Die folgende Abbildung enthält alle Raum-API-Endpunkte für die Verarbeitung von Blobs. Aufgelistet sind auch alle Pfadparameter, die ggf. an diese Endpunkte übergeben werden müssen.

![Raumblobs][3]

Wenn beispielsweise ein an einen Raum angefügtes Blob zurückgegeben werden soll, richten Sie eine authentifizierte HTTP GET-Anforderung an:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID |

Eine PATCH-Anforderung an denselben Endpunkt aktualisiert die Metadatenbeschreibungen und erstellt neue Versionen des Blobs. Die HTTP-Anforderung wird unter Verwendung der PATCH-Methode zusammen mit den erforderlichen Metadaten und mehrteiligen Formulardaten ausgeführt.

Bei erfolgreichen Vorgängen wird ein **Raumblob** zurückgegeben, das dem folgenden Schema entspricht. Es kann zur Nutzung zurückgegebener Daten verwendet werden.

| Attribut | Typ | BESCHREIBUNG | Beispiele |
| --- | --- | --- | --- |
| **SpaceBlobType** | Zeichenfolge | Eine Blobkategorie, die an einen Raum angefügt werden kann | `Map` und `Image` |
| **SpaceBlobSubtype** | Zeichenfolge | Eine Blobunterkategorie, die präziser ist als **SpaceBlobType** | `GenericMap`, `ElectricalMap`, `SatelliteMap` und `WayfindingMap` |

### <a name="users"></a>Benutzer

Blobs können an Benutzermodelle angefügt werden (z. B. um ein Profilbild zuzuordnen). Die folgende Abbildung zeigt die relevanten Benutzer-API-Endpunkte sowie alle ggf. erforderlichen Pfadparameter (z. B. `id`):

![Benutzerblobs][4]

Wenn Sie beispielsweise ein Blob abrufen möchten, das an einen Benutzer angefügt ist, richten Sie eine authentifizierte HTTP GET-Anforderung mit den erforderlichen Formulardaten an:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID |

Der zurückgegebene JSON-Code (**UserBlob**-Objekte) entspricht den folgenden JSON-Modellen:

| Attribut | Typ | BESCHREIBUNG | Beispiele |
| --- | --- | --- | --- |
| **UserBlobType** | Zeichenfolge | Eine Blobkategorie, die an einen Benutzer angefügt werden kann | `Image` und `Video` |
| **UserBlobSubtype** |  Zeichenfolge | Eine Blobunterkategorie, die präziser ist als **UserBlobType** | `ProfessionalImage`, `VacationImage` und `CommercialVideo` |

## <a name="common-errors"></a>Häufige Fehler

Ein häufiger Fehler ist die Verwendung falscher Headerinformationen:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Swagger-Referenzdokumentation von Azure Digital Twins finden Sie unter [Verwenden von Azure Digital Twins-Swagger](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
