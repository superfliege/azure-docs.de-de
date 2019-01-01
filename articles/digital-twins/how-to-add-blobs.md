---
title: Hinzufügen von Blobs zu Objekten in Azure Digital Twins | Microsoft-Dokumentation
description: Grundlegendes zum Hinzufügen von Blobs zu Objekten in Azure Digital Twins
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: 8a68ba35ddf7caacbf2339d87c5aeef80f470ba4
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52725623"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Hinzufügen von Blobs zu Objekten in Azure Digital Twins

Blobs sind unstrukturierte Darstellungen gängiger Dateitypen (z. B. Bilder oder Protokolle). Blobs verwenden einen MIME-Typ (beispielsweise „image/jpeg“) sowie Metadaten (Name, Beschreibung, Typ usw.), um die Art der dargestellten Daten nachzuverfolgen.

Azure Digital Twins unterstützt das Anfügen von Blobs an Geräte, Räume und Benutzer. Blobs können ein Profilbild für einen Benutzer, ein Gerätefoto, ein Video, eine Karte oder ein Protokoll darstellen.

> [!NOTE]
> In diesem Artikel wird Folgendes vorausgesetzt:
> * Ihre Instanz ist ordnungsgemäß für den Empfang von Verwaltungs-API-Anforderungen konfiguriert.
> * Sie haben sich ordnungsgemäß über einen REST-Client Ihrer Wahl authentifiziert.

## <a name="uploading-blobs-an-overview"></a>Hochladen von Blobs: Übersicht

Sie können Blobs unter Verwendung mehrteiliger Anforderungen in bestimmte Endpunkte und deren jeweilige Funktionen hochladen.

> [!IMPORTANT]
> Mehrteilige Anforderungen müssen drei Informationselemente umfassen:
> * Header vom Typ **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Element vom Typ **Content-Disposition**: `form-data; name="metadata"`
> * Der hochzuladende Dateiinhalt
>
> Die Informationselemente **Content-Type** und **Content-Disposition** können je nach Verwendungsszenario variieren.

Mehrteilige Anforderungen an Verwaltungs-APIs von Azure Digital Twins bestehen aus zwei Teilen:

* Blobmetadaten (z. B. ein zugeordneter MIME-Typ) gemäß den Elementen **Content-Type** und **Content-Disposition**

* BLOB-Inhalte (der unstrukturierte Inhalt der Datei)  

Bei **PATCH**-Anforderungen wird keiner der beiden Teile benötigt. Bei **POST**- oder Erstellungsvorgängen werden dagegen beide Teile benötigt.

### <a name="blob-metadata"></a>Blobmetadaten

Neben **Content-Type** und **Content-Disposition** müssen mehrteilige Anforderungen auch den korrekten JSON-Text angeben. Der zu übermittelnde JSON-Text hängt von der Art des ausgeführten HTTP-Anforderungsvorgangs ab.

Nachfolgend finden Sie die vier wichtigsten JSON-Schemas:

![JSON-Schemas][1]

In der Swagger-Dokumentation werden diese Modell-Schemas umfassend beschrieben.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Informationen zur Verwendung der Referenzdokumentation finden Sie unter [Verwenden von Azure Digital Twins-Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Beispiele

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

So erstellen Sie eine **POST**-Anforderung, die eine Textdatei als Blob hochlädt und einem Raum zuordnet:

```plaintext
POST YOUR_MANAGEMENT_API_URL/spaces/blobs HTTP/1.1
Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"

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

| Parameterwert | Ersetzen durch |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Name für die Begrenzung mehrteiliger Inhalte |

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

## <a name="api-endpoints"></a>API-Endpunkte

Die folgenden Abschnitte enthalten ausführliche Informationen zu wichtigen Endpunkten und ihren Funktionen.

### <a name="devices"></a>Geräte

Blobs können an Geräte angefügt werden. Die folgende Abbildung zeigt die Swagger-Referenzdokumentation für Ihre Verwaltungs-APIs. Sie gibt gerätebezogene API-Endpunkte für die Blobnutzung sowie ggf. zu übergebende erforderliche Pfadparameter an.

![Geräteblobs][2]

Wenn Sie beispielsweise ein Blob aktualisieren oder erstellen und dann an ein Gerät anfügen möchten, richten Sie eine **PATCH**-Anforderung an:

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

### <a name="spaces"></a>Räume

Blobs können auch an Räume angefügt werden. Die folgende Abbildung enthält alle Raum-API-Endpunkte für die Verarbeitung von Blobs. Aufgelistet sind auch alle Pfadparameter, die ggf. an diese Endpunkte übergeben werden müssen.

![Raumblobs][3]

Wenn beispielsweise ein an einen Raum angefügtes Blob zurückgegeben werden soll, richten Sie eine **GET**-Anforderung an:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID |

Mit einer **PATCH**-Anforderung an den gleichen Endpunkt können Sie eine Metadatenbeschreibung aktualisieren und eine neue Version des Blobs erstellen. Die HTTP-Anforderung wird unter Verwendung der **PATCH**-Methode zusammen mit sämtlichen erforderlichen Metadaten und mehrteiligen Formulardaten ausgeführt.

Bei erfolgreichen Vorgängen wird ein **Raumblob** zurückgegeben, das dem folgenden Schema entspricht. Es kann zur Nutzung zurückgegebener Daten verwendet werden.

| Attribut | Typ | BESCHREIBUNG | Beispiele |
| --- | --- | --- | --- |
| **SpaceBlobType** | Zeichenfolge | Eine Blobkategorie, die an einen Raum angefügt werden kann | `Map` und `Image` |
| **SpaceBlobSubtype** | Zeichenfolge | Eine Blobunterkategorie, die präziser ist als **SpaceBlobType** | `GenericMap`, `ElectricalMap`, `SatelliteMap` und `WayfindingMap` |

### <a name="users"></a>Benutzer

Blobs können an Benutzermodelle angefügt werden (z. B. um ein Profilbild zuzuordnen). Die folgende Abbildung zeigt die relevanten Benutzer-API-Endpunkte sowie alle ggf. erforderlichen Pfadparameter (z. B. `id`):

![Benutzerblobs][4]

Wenn Sie beispielsweise ein Blob abrufen möchten, das an einen Benutzer angefügt ist, richten Sie eine **GET**-Anforderung mit allen ggf. erforderlichen Formulardaten an:

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

Weitere Informationen zur Swagger-Referenzdokumentation von Azure Digital Twins finden Sie unter [Verwenden von Azure Digital Twins-Swagger](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
