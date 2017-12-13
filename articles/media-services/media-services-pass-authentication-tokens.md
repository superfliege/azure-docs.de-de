---
title: "Übergeben von Authentifizierungstoken an Azure Media Services | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Authentifizierungstoken vom Client an den Schlüsselbereitstellungsdienst von Azure Media Services senden."
services: media-services
keywords: Content Protection, DRM, Tokenauthentifizierung
documentationcenter: 
author: dbgeorge
manager: jasonsue
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: dwgeo
ms.openlocfilehash: 0e56726266898e5738dd797a8a019987d457170e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
---
# <a name="how-clients-pass-tokens-to-azure-media-services-key-delivery-service"></a>Übergeben von Token an den Schlüsselbereitstellungsdienst von Azure Media Services durch Clients
Wir erhalten ständig Fragen darüber, wie ein Player Token an unsere Schlüsselbereitstellungsdienste übergeben kann, die verifiziert werden, während der Player den Schlüssel erhält. Wir unterstützen Simple Web Token (SWT) und JSON Web Token (JWT) als Tokenformate. Die Tokenauthentifizierung kann auf jede Art von Schlüssel angewendet werden, unabhängig davon, ob Sie Common Encryption oder die AES-Umschlagverschlüsselung im System verwenden.

Die folgenden Möglichkeiten, wie Sie den Token mit Ihrem Player übergeben können, hängen vom Player und der Plattform ab, auf die sie ausgerichtet sind:
- Über die HTTP-Autorisierungsheader.
> [!NOTE]
> Beachten Sie, dass das Präfix „Bearer“ gemäß der OAuth 2.0-Spezifikationen erwartet wird. Es gibt einen Beispielplayer mit einer Tokenkonfiguration, der auf der Azure Media Player-[Demoseite](http://ampdemo.azureedge.net/) gehostet wird. Wählen Sie AES (JWT-Token) oder AES (SWT-Token) aus, um die Videoquelle festzulegen. Das Token wird über den Autorisierungsheader übergeben.

- Durch Hinzufügen eines URL-Abfrageparameters mit „token=tokenvalue“.  
> [!NOTE]
> Beachten Sie, dass das Präfix „Bearer“ nicht erwartet wird. Da das Token über eine URL gesendet wird, müssen Sie die Tokenzeichenfolge schützen. Hier folgt ein C#-Beispielcode zur Vorgehensweise:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

- Über das Feld „CustomData“.
Nur für den Erwerb von PlayReady-Lizenzen über das Feld „CustomData“ der PlayReady License Acquisition Challenge. In diesem Fall muss sich das Token innerhalb des unten beschriebenen XML-Dokuments befinden.

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```
Platzieren Sie Ihr Authentifizierungstoken im Element <Token>.

- Über eine alternative HLS-Wiedergabeliste. Wenn Sie die Tokenauthentifizierung für die AES- und HLS-Wiedergabe unter iOS/Safari konfigurieren müssen, gibt es keine Möglichkeit, das Token direkt zu senden. Informationen zum Ändern der Wiedergabeliste, damit dieses Szenario ermöglicht wird, finden Sie in diesem [Blogbeitrag](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]