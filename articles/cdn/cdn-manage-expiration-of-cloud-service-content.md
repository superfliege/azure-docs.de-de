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
ms.openlocfilehash: d58a245923242b3963b188ca869e8290d999c0a2
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Verwalten des Ablaufs von Webinhalt im Azure Content Delivery Network
 im Azure CDN
> [!div class="op_single_selector"]
> * [Azure-Web-Apps/Cloud Services, ASP.NET oder IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Dateien von öffentlich zugänglichen Ursprungswebservern können bis zum Ende ihrer Gültigkeitsdauer im Azure Content Delivery Network (CDN) zwischengespeichert werden. Die Gültigkeitsdauer wird durch den [`Cache-Control`-Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort des Ursprungsservers bestimmt. In diesem Artikel erfahren Sie, wie Sie `Cache-Control`-Header für das Web-Apps-Feature von Microsoft Azure App Service, Azure Cloud Services, ASP.NET-Anwendungen und Internetinformationsdienste-Websites festlegen. Die Konfiguration ist dabei jeweils ähnlich.

> [!TIP]
> Sie haben auch die Möglichkeit, für eine Datei keine Gültigkeitsdauer festzulegen. In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen an.
> 
> Weitere Informationen dazu, wie Azure CDN den Zugriff auf Dateien und andere Ressourcen beschleunigen kann, finden Sie in der [Übersicht über Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-in-configuration-files"></a>Festlegen von Cache-Control-Headern in Konfigurationsdateien
Bei statischen Inhalten wie Bildern und Stylesheets können Sie das Aktualisierungsintervall durch Ändern der Datei **applicationHost.config** oder **web.config** für Ihre Webanwendung steuern. Das Element **system.webServer\staticContent\clientCache** in der Konfigurationsdatei legt den `Cache-Control`-Header für Ihre Inhalte fest. Für **web.config**-Dateien gilt: Die Konfigurationseinstellungen wirken sich auf alle Inhalte des Ordners sowie der Unterordner aus, solange diese nicht auf Unterordnerebene überschrieben werden. Beispielsweise können Sie im Stammordner eine Einstellung für die Standardgültigkeitsdauer festlegen, um alle statischen Inhalte drei Tage lang zwischenzuspeichern, sowie einen Unterordner mit stärker veränderlichem Inhalt zum Zwischenspeichern des Inhalts für nur sechs Stunden. Die **applicationHost.config**-Dateieinstellungen wirken sich auf alle Anwendungen des Standorts aus, aber sie werden durch die Einstellungen aller vorhandenen **web.config**-Dateien in den Anwendungen außer Kraft gesetzt.

Im folgenden XML-Beispiel wird veranschaulicht, wie Sie **clientCache** festlegen, um ein maximales Alter von drei Tagen anzugeben:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Durch Angeben von **UseMaxAge** wird der Antwort basierend auf dem im **cacheControlMaxAge**-Attribut angegebenen Wert ein Header vom Typ `Cache-Control: max-age=<nnn>` hinzugefügt. Das Format des Zeitraums für das **cacheControlMaxAge**-Attribut lautet `<days>.<hours>:<min>:<sec>`. Weitere Informationen zum **clientCache**-Knoten finden Sie unter [Clientcache<clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Festlegen von Cache-Control-Headern im Code
Bei ASP.NET-Anwendungen können Sie das Verhalten von CDN beim Zwischenspeichern mithilfe der **HttpResponse.Cache**-Eigenschaft programmgesteuert steuern. Weitere Informationen zur **HttpResponse.Cache**-Eigenschaft finden Sie unter [HttpResponse.Cache-Eigenschaft](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) und [HttpCachePolicy-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Führen Sie diese Schritte aus, um Anwendungsinhalt in ASP.NET programmgesteuert zwischenzuspeichern:
   1. Stellen Sie sicher, dass der Inhalt als zwischenspeicherbar gekennzeichnet ist, indem Sie `HttpCacheability` auf *Public* festlegen. 
   2. Legen Sie eine Cachevalidierung fest, indem Sie eine der folgenden Methoden aufrufen:
      - Rufen Sie `SetLastModified` auf, um einen LastModified-Zeitstempel festzulegen.
      - Rufen Sie `SetETag` auf, um einen `ETag`-Wert festzulegen.
   3. Geben Sie optional eine Ablaufzeit für den Cache an, indem Sie `SetExpires` aufrufen. Andernfalls gilt die Standard-Cacheheuristik, die weiter oben in diesem Dokument beschrieben wurde.

Fügen Sie beispielsweise folgenden C#-Code hinzu, um Inhalt eine Stunde lang zwischenzuspeichern:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Nächste Schritte
* [Lesen Sie ausführliche Informationen zum **clientCache**-Element.](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Lesen Sie die Dokumentation für die **HttpResponse.Cache**-Eigenschaft.](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Lesen Sie die Dokumentation für die **HttpCachePolicy**-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

