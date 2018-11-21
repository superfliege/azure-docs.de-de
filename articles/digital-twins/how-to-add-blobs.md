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
ms.openlocfilehash: 0929a4a63eee35d21723c980887d6b4217898682
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688318"
---
# <a name="how-to-add-blobs-to-objects-in-azure-digital-twins"></a>Hinzufügen von Blobs zu Objekten in Azure Digital Twins

Blobs sind unstrukturierte Darstellungen gängiger Dateitypen (etwa Bilder oder Protokolle). Blobs verwenden einen MIME-Typ (beispielsweise „image/jpeg“) sowie Metadaten (Name, Beschreibung, Typ usw.), um die Art der dargestellten Daten nachzuverfolgen.

Azure Digital Twins unterstützt das Anfügen von Blobs an Geräte, Räume und Benutzer. Blobs können ein Profilbild für einen Benutzer, ein Gerätefoto, ein Video, eine Karte oder ein Protokoll darstellen.

> [!NOTE]
> In diesem Artikel wird Folgendes vorausgesetzt:
> * Ihre Instanz ist ordnungsgemäß für den Empfang von Verwaltungs-API-Anforderungen konfiguriert.
> * Sie haben sich ordnungsgemäß über einen REST-Client Ihrer Wahl authentifiziert.

## <a name="uploading-blobs-an-overview"></a>Hochladen von Blobs: Übersicht

Blobs werden unter Verwendung mehrteiliger Anforderungen in bestimmte Endpunkte und in ihre jeweiligen Funktionen hochgeladen.

> [!IMPORTANT]
> Mehrteilige Anforderungen müssen drei Angaben umfassen. Für Azure Digital Twins:
> * Einen Header vom Typ **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Eine Angabe vom Typ **Content-Disposition**: `form-data; name="metadata"`
> * Den hochzuladenden Dateiinhalt
>
> Die Angaben für **Content-Type** und **Content-Disposition** können je nach Verwendungsszenario variieren.

Jede mehrteilige Anforderung wird in mehrere Blöcke aufgeteilt. Mehrteilige Anforderungen, die an Verwaltungs-APIs von Azure Digital Twins gerichtet werden, werden in **zwei** (**2**) Teile aufgeteilt:

1. Der erste Teil ist erforderlich und enthält Blobmetadaten wie etwa einen zugeordneten MIME-Typ gemäß **Content-Type** und **Content-Disposition** von weiter oben.

1. Der zweite Teil enthält den eigentlichen Blobinhalt (also den unstrukturierten Inhalt der Datei).  

Bei **PATCH**-Anforderungen wird keiner der beiden Teile benötigt. Bei **POST**- oder Erstellungsvorgängen werden dagegen beide Teile benötigt.

### <a name="blob-metadata"></a>Blobmetadaten

Neben **Content-Type** und **Content-Disposition** müssen mehrteilige Anforderungen auch den korrekten JSON-Text angeben. Der zu übermittelnde JSON-Text hängt von der Art des ausgeführten HTTP-Anforderungsvorgangs ab.

Im Anschluss finden Sie die vier wichtigsten JSON-Schemas:

![Raumblobs][1]

Diese Modellschemas werden in der bereitgestellten Swagger-Dokumentation umfassend beschrieben.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Informationen zur Verwendung der bereitgestellten Referenzdokumentation finden Sie unter [Verwenden von Azure Digital Twins-Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Beispiele

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

So erstellen Sie eine POST-Anforderung, die eine Textdatei als Blob hochlädt und einem Raum zuordnet:

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

Im Anschluss finden Sie eine .NET-Implementierung des gleichen Blob-Uploads mit der Klasse [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metaData in a suitable format
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

Dieser Abschnitt enthält ausführliche Informationen zu wichtigen Endpunkten und ihren spezifischen Funktionen.

### <a name="devices"></a>Geräte

Blobs können an Geräte angefügt werden. Die folgende Abbildung der Swagger-Referenzdokumentation für Ihre Verwaltungs-APIs gibt gerätebezogene API-Endpunkte für die Blobnutzung sowie ggf. zu übergebende Pfadparameter an:

![Geräteblobs][2]

Wenn Sie beispielsweise ein Blob aktualisieren oder erstellen und es an ein Gerät anfügen möchten, richten Sie eine PATCH-Anforderung an:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID. |

Bei erfolgreichen Anforderungen wird in der Antwort ein DeviceBlob-JSON-Objekt zurückgegeben. DeviceBlob-Objekte entsprechen dem folgenden JSON-Schema:

| Attribut | Typ | Beschreibung | Beispiele |
| --- | --- | --- | --- |
| **DeviceBlobType** | Zeichenfolge | Eine Blobkategorie, die an ein Gerät angefügt werden kann. | `Model` und `Specification` |
| **DeviceBlobSubtype*** | Zeichenfolge | Eine Blobunterkategorie, die präziser ist als „DeviceBlobType“. | `PhysicalModel`, `LogicalModel`, `KitSpecification` und `FunctionalSpecification` |

> [!TIP]
> Verwenden Sie die obige Tabelle, um erfolgreich zurückgegebene Anforderungsdaten zu behandeln.

### <a name="spaces"></a>Räume

Blobs können auch an Räume angefügt werden. Die folgende Abbildung enthält alle Raum-API-Endpunkte für die Behandlung von Blobs sowie ggf. zu übergebende Pfadparameter:

![Raumblobs][3]

Wenn beispielsweise ein an einen Raum angefügtes Blob zurückgegeben werden soll, richten Sie eine GET-Anforderung an:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID. |

Mit einer PATCH-Anforderung an den gleichen Endpunkt können Sie eine Metadatenbeschreibung aktualisieren und eine neue Version des Blobs erstellen. Die HTTP-Anforderung wird unter Verwendung der PATCH-Methode zusammen mit sämtlichen erforderlichen Metadaten und mehrteiligen Formulardaten ausgeführt.

Von erfolgreichen Vorgängen wird ein Raumblob zurückgegeben, das dem folgenden Schema entspricht und zur Nutzung zurückgegebener Daten verwendet werden kann:

| Attribut | Typ | Beschreibung | Beispiele |
| --- | --- | --- | --- |
| **SpaceBlobType** | Zeichenfolge | Eine Blobkategorie, die an einen Raum angefügt werden kann. | `Map` und `Image` |
| **SpaceBlobSubtype** | Zeichenfolge | Eine Blobunterkategorie, die präziser ist als „SpaceBlobType“. | `GenericMap`, `ElectricalMap`, `SatelliteMap` und `WayfindingMap` |

### <a name="users"></a>Benutzer

Blobs können an Benutzermodelle angefügt werden (etwa um ein Profilbild zuzuordnen). Die folgende Abbildung zeigt die relevanten Benutzer-API-Endpunkte sowie alle ggf. erforderlichen Pfadparameter (beispielsweise `id`):

![Benutzerblobs][4]

Wenn Sie beispielsweise ein Blob abrufen möchten, das an einen Benutzer angefügt ist, richten Sie eine GET-Anforderung mit allen ggf. erforderlichen Formulardaten an:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_BLOB_ID* | Die gewünschte Blob-ID. |

Der zurückgegebene JSON-Code (UserBlobs) entspricht den folgenden JSON-Modellen:

| Attribut | Typ | Beschreibung | Beispiele |
| --- | --- | --- | --- |
| **UserBlobType** | Zeichenfolge | Eine Blobkategorie, die an einen Benutzer angefügt werden kann. | `Image` und `Video` |
| **UserBlobSubtype** |  Zeichenfolge | Eine Blobunterkategorie, die präziser ist als „UserBlobType“. | `ProfessionalImage`, `VacationImage` und `CommercialVideo` |

## <a name="common-errors"></a>Häufige Fehler

Verwendung falscher Headerinformationen:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur bereitgestellten Swagger-Referenzdokumentation von Azure Digital Twins finden Sie unter [Verwenden von Azure Digital Twins-Swagger](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
