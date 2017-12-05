---
title: Verwalten des Ablaufs von Webinhalt im Azure Content Delivery Network | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Ablauf von Inhalten aus Azure-Web-Apps/Cloud Services, ASP.NET oder IIS in Azure CDN verwalten.
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: fe519c3ad5f99899277bf005929142c52a4c4724
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Verwalten des Ablaufs von Webinhalt im Azure Content Delivery Network
> [!div class="op_single_selector"]
> * [Azure-Webinhalt](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Dateien von öffentlich zugänglichen Ursprungswebservern können bis zum Ende ihrer Gültigkeitsdauer im Azure Content Delivery Network (CDN) zwischengespeichert werden. Die Gültigkeitsdauer wird durch den `Cache-Control`-Header in der HTTP-Antwort des Ursprungsservers bestimmt. In diesem Artikel erfahren Sie, wie Sie `Cache-Control`-Header für das Web-Apps-Feature von Microsoft Azure App Service, Azure Cloud Services, ASP.NET-Anwendungen und Internetinformationsdienste-Websites (Internet Information Services, IIS) festlegen. Die Konfiguration ist dabei jeweils ähnlich. Sie können den `Cache-Control`-Header entweder mithilfe von Konfigurationsdateien oder programmgesteuert festlegen.

> [!TIP]
> Sie haben auch die Möglichkeit, für eine Datei keine Gültigkeitsdauer festzulegen. In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen an. Diese Standardgültigkeitsdauer gilt nur für die Optimierung allgemeiner Webbereitstellungen. Bei der Optimierung großer Dateien beträgt die Standardgültigkeitsdauer einen Tag, bei der Optimierung für das Medienstreaming beträgt die Standardgültigkeitsdauer ein Jahr.
> 
> Weitere Informationen dazu, wie Azure CDN den Zugriff auf Dateien und andere Ressourcen beschleunigen kann, finden Sie in der [Übersicht über Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Festlegen von Cache-Control-Headern mithilfe von Konfigurationsdateien
Bei statischen Inhalten wie Bildern und Stylesheets können Sie die Aktualisierungshäufigkeit durch Ändern der Konfigurationsdateien **ApplicationHost.config** oder **Web.config** für Ihre Webanwendung steuern. Das `<system.webServer>/<staticContent>/<clientCache>`-Element in beiden Dateien legt den `Cache-Control`-Header für Ihren Inhalt fest.

### <a name="using-applicationhostconfig-files"></a>Verwenden der „ApplicationHost.config“-Datei
Die Datei **ApplicationHost.config** ist die Stammdatei des Konfigurationssystems für die Internetinformationsdienste. Die Konfigurationseinstellungen in einer **ApplicationHost.config**-Datei wirken sich auf alle Anwendungen in der Website aus, werden aber durch die Einstellungen aller für eine Webanwendung vorhandenen **Web.config**-Dateien außer Kraft gesetzt.

### <a name="using-webconfig-files"></a>Verwenden von Web.config-Dateien
Mit einer **Web.config**-Datei können Sie das Verhalten Ihrer gesamten Webanwendung oder eines bestimmten Verzeichnisses in Ihrer Webanwendung anpassen. In der Regel befindet sich im Stammordner Ihrer Webanwendung mindestens eine **Web.config**-Datei. Für jede **Web.config**-Datei in einem bestimmten Ordner gilt: Die Konfigurationseinstellungen wirken sich auf alle Inhalte dieses Ordners sowie aller Unterordner aus, solange diese nicht auf Unterordnerebene von einer anderen **Web.config**-Datei außer Kraft gesetzt werden. Sie können z.B. ein `<clientCache>`-Element in einer **Web.config**-Datei im Stammordner Ihrer Webanwendung festlegen, um alle statischen Inhalte Ihrer Webanwendung drei Tage lang zwischenzuspeichern. Sie können auch eine **Web.config**-Datei in einem Unterordner mit variableren Inhalten hinzufügen (z.B. `\frequent`) und das `<clientCache>`-Element dieser Datei so festlegen, dass der Inhalt des Unterordners sechs Stunden lang zwischengespeichert wird. Dies führt zu folgendem Ergebnis: Der Inhalt der gesamten Website wird drei Tage lang zwischengespeichert, mit Ausnahme aller Inhalte im Verzeichnis `\frequent`, die nur sechs Stunden lang zwischengespeichert werden.  

Das folgende XML-Beispiel veranschaulicht, wie Sie das `<clientCache>`-Element in einer Konfigurationsdatei festlegen, um ein maximales Alter von drei Tagen anzugeben:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Um das **cacheControlMaxAge**-Attribut zu verwenden, müssen Sie den Wert für das **cacheControlMode**-Attribut auf `UseMaxAge` festlegen. Diese Einstellung führt dazu, dass der HTTP-Header und die HTTP-Anweisung `Cache-Control: max-age=<nnn>` zur Antwort hinzugefügt werden. Das Format des timespan-Werts für das **cacheControlMaxAge**-Attribut ist `<days>.<hours>:<min>:<sec>`. Der Wert wird in Sekunden konvertiert und als Wert der `Cache-Control` `max-age`-Anweisung verwendet. Weitere Informationen zum `<clientCache>`-Element finden Sie unter [Clientcache<clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Programmgesteuertes Festlegen von Cache-Control-Headern
Bei ASP.NET-Anwendungen steuern Sie das Verhalten von CDN beim Zwischenspeichern mithilfe der **HttpResponse.Cache**-Eigenschaft der .NET-API programmgesteuert. Informationen zur **HttpResponse.Cache**-Eigenschaft finden Sie unter [HttpResponse.Cache-Eigenschaft](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) und [HttpCachePolicy-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Führen Sie diese Schritte aus, um Anwendungsinhalt in ASP.NET programmgesteuert zwischenzuspeichern:
   1. Stellen Sie sicher, dass der Inhalt als für die Zwischenspeicherung geeignet gekennzeichnet ist, indem Sie `HttpCacheability` auf `Public` festlegen. 
   2. Legen Sie eine Cachevalidierung fest, indem Sie eine der folgenden `HttpCachePolicy`-Methoden aufrufen:
      - Rufen Sie `SetLastModified` auf, um einen timestamp-Wert für den `Last-Modified`-Header festzulegen.
      - Rufen Sie `SetETag` auf, um einen Wert für den `ETag`-Header festzulegen.
   3. Geben Sie optional eine Ablaufzeit für den Cache an, indem Sie `SetExpires` aufrufen, um einen Wert für den `Expires`-Header festzulegen. Andernfalls gilt die Standard-Cacheheuristik, die weiter oben in diesem Dokument beschrieben wurde.

Fügen Sie beispielsweise folgenden C#-Code hinzu, um Inhalt eine Stunde lang zwischenzuspeichern:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Testen des Cache-Control-Headers
Sie können die Einstellungen für die Gültigkeitsdauer Ihres Webinhalts ganz einfach überprüfen. Testen Sie mithilfe der [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) Ihres Browsers, ob Ihr Webinhalt den `Cache-Control`-Antwortheader enthält. Sie können auch ein Tool wie **wget**, [Postman](https://www.getpostman.com/) oder [Fiddler](http://www.telerik.com/fiddler) verwenden, um die Antwortheader zu untersuchen.

## <a name="next-steps"></a>Nächste Schritte
* [Lesen Sie ausführliche Informationen zum **clientCache**-Element.](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Lesen Sie die Dokumentation für die **HttpResponse.Cache**-Eigenschaft.](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Lesen Sie die Dokumentation für die **HttpCachePolicy**-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

