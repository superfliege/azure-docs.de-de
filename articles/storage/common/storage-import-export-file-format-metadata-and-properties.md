---
title: Format der Metadaten- und Eigenschaftendateien von Azure Import/Export | Microsoft Docs
description: Informationen zum Angeben von Metadaten und Eigenschaften für mindestens ein Blob, das Teil eines Import- oder Exportauftrags ist.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 5a886244b43ad006a95e9be0350d9c69fd987ad9
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526231"
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Format der Metadaten- und Eigenschaftendateien des Azure Import/Export-Diensts
Sie können Metadaten und Eigenschaften als Teil eines Import- oder Exportauftrags für mindestens ein Blob angeben. Zum Festlegen von Metadaten oder Eigenschaften für Blobs, die als Teil eines Importauftrags erstellt werden, geben Sie eine Metadaten- oder Eigenschaftendatei auf der Festplatte an, die die zu importierenden Daten enthält. Bei einem Exportauftrag werden Metadaten und Eigenschaften in eine entsprechende Datei auf der Festplatte geschrieben, die an Sie zurückgesendet wird.  
  
## <a name="metadata-file-format"></a>Format der Metadatendatei  
Das Format einer Metadatendatei lautet wie folgt:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|XML-Element|Typ|BESCHREIBUNG|  
|-----------------|----------|-----------------|  
|`Metadata`|Stammelement|Das Stammelement der Metadatendatei|  
|`metadata-name`|Zeichenfolge|Optional. Das XML-Element gibt den Namen der Metadaten für das Blob an, und sein Wert gibt den Wert der Metadateneinstellung an.|  
  
## <a name="properties-file-format"></a>Format der Eigenschaftendatei  
Das Format einer Eigenschaftendatei lautet wie folgt:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|XML-Element|Typ|BESCHREIBUNG|  
|-----------------|----------|-----------------|  
|`Properties`|Stammelement|Das Stammelement der Eigenschaftendatei|  
|`Last-Modified`|Zeichenfolge|Optional. Die Zeit der letzten Änderung des Blobs. Nur für Exportaufträge|  
|`Etag`|Zeichenfolge|Optional. Der ETag-Wert des Blobs. Nur für Exportaufträge|  
|`Content-Length`|Zeichenfolge|Optional. Die Größe des Blobs in Byte. Nur für Exportaufträge|  
|`Content-Type`|Zeichenfolge|Optional. Der Inhaltstyp des Blobs|  
|`Content-MD5`|Zeichenfolge|Optional. MD5-Hash des Blobs|  
|`Content-Encoding`|Zeichenfolge|Optional. Inhaltscodierung des Blobs|  
|`Content-Language`|Zeichenfolge|Optional. Inhaltssprache des Blob|  
|`Cache-Control`|Zeichenfolge|Optional. Die Cachesteuerelement-Zeichenfolge für das Blob|  

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Regeln zum Festlegen der Blobmetadaten und -eigenschaften finden Sie unter [Festlegen von Blobeigenschaften](/rest/api/storageservices/set-blob-properties), [Festlegen von Blobmetadaten](/rest/api/storageservices/set-blob-metadata) und [Festlegen und Abrufen von Eigenschaften und Metadaten für Blobressourcen](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources).
