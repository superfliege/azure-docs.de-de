---
title: Problembehandlung bei der Komprimierung in Azure-CDN | Microsoft Docs
description: Behandeln von Problemen mit der Komprimierung der Azure-CDN-Datei aus.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5ef8a8262eb40aa827161764f03a63d031e43273
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-file-compression"></a>Problembehandlung bei dateikomprimierung CDN
Dieser Artikel hilft Ihnen die Problembehandlung [CDN dateikomprimierung](cdn-improve-performance.md).

Wenn Sie weitere Hilfe zu einem beliebigen Zeitpunkt in diesem Artikel benötigen, erhalten Sie die Azure-Experten auf [Azure MSDN und den Stack Overflow-Foren](https://azure.microsoft.com/support/forums/). Alternativ können Sie auch einen Azure-Support-Vorfall einreichen. Wechseln Sie zu der [Azure-Support-Website](https://azure.microsoft.com/support/options/) , und klicken Sie auf **Supports**.

## <a name="symptom"></a>Symptom
Komprimierung für Ihren Endpunkt aktiviert ist, aber Dateien werden dekomprimiert zurückgegeben wird.

> [!TIP]
> Um zu überprüfen, ob die Dateien komprimierte zurückgegeben werden, müssen Sie ein Tool wie [Fiddler](http://www.telerik.com/fiddler) oder Ihres Browsers [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Überprüfen Sie die HTTP-Antwortheader zurückgegeben, mit Ihrer zwischengespeicherten CDN Inhalt.  Es ist ein Header mit dem Namen `Content-Encoding` mit einem Wert von **Gzip**, **bzip2**, oder **deflate**, Ihre Inhalte werden komprimiert.
> 
> ![Content-Encoding-header](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Ursache
Es gibt mehrere mögliche Ursachen, einschließlich:

* Der angeforderte Inhalt ist nicht für die Komprimierung geeignet.
* Die Komprimierung ist nicht für den Typ der angeforderten Datei aktiviert.
* Die HTTP-Anforderung einen Anforderung einen gültige Komprimierungstyp Header nicht enthalten.

## <a name="troubleshooting-steps"></a>Problembehandlungsschritte
> [!TIP]
> Wie bei der Bereitstellung von neuen Endpunkte, nehmen Änderungen an der Konfiguration von CDN etwas Zeit über das Netzwerk übertragen.  Änderungen werden in der Regel innerhalb von 90 Minuten angewendet.  Wenn dies das erste Mal die Komprimierung für Ihre CDN-Endpunkt eingerichtet haben ist, sollten Sie warten von 1 bis 2 Stunden um sicherzustellen, dass die Komprimierung, die Einstellungen auf Popups weitergegeben wurden. 
> 
> 

### <a name="verify-the-request"></a>Überprüfen Sie die Anforderung
Es sollte zunächst eine Prüfung eine schnelle integritätsprüfung für die Anforderung erfolgen.  Können Sie Ihr Browser [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) So zeigen Sie die Anforderungen an.

* Überprüfen Sie, ob die Anforderung an die Endpunkt-URL gesendet wird `<endpointname>.azureedge.net`, und nicht von Ihrem Ursprung.
* Überprüfen Sie, ob die Anforderung enthält ein **Accept-Encoding** Header und den Wert für diesen Header enthält **Gzip**, **deflate**, oder **bzip2**.

> [!NOTE]
> **Azure-CDN von Akamai** Profile unterstützen nur **Gzip** Codierung.
> 
> 

![CDN-Anforderungsheader](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Überprüfen Sie die komprimierungseinstellungen (Standard-CDN-Profil)
> [!NOTE]
> Dieser Schritt gilt nur, wenn das CDN-Profil ist ein **Azure CDN Standard von Verizon** oder **Azure CDN Standard von Akamai** Profil. 
> 
> 

Navigieren Sie zu Ihrem Endpunkt in der [Azure-Portal](https://portal.azure.com) , und klicken Sie auf die **konfigurieren** Schaltfläche.

* Stellen Sie sicher, dass die Komprimierung aktiviert ist.
* Stellen Sie sicher, dass der MIME-Typ für den Inhalt komprimiert werden in der Liste der komprimierten Formate enthalten ist.

![CDN-komprimierungseinstellungen](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Überprüfen Sie die komprimierungseinstellungen (Premium-CDN-Profil)
> [!NOTE]
> Dieser Schritt gilt nur, wenn das CDN-Profil ist ein **Azure CDN Premium aus Verizon** Profil.
> 
> 

Navigieren Sie zu Ihrem Endpunkt in der [Azure-Portal](https://portal.azure.com) , und klicken Sie auf die **verwalten** Schaltfläche.  Der CDN-Portal wird geöffnet.  Zeigen Sie auf die **HTTP große** tab, dann zeigen Sie auf die **Cacheeinstellungen** Flyout.  Klicken Sie auf **Komprimierung**. 

* Stellen Sie sicher, dass die Komprimierung aktiviert ist.
* Überprüfen Sie die **Dateitypen** Liste enthält eine durch Trennzeichen getrennte Liste (ohne Leerzeichen) der MIME-Typen.
* Stellen Sie sicher, dass der MIME-Typ für den Inhalt komprimiert werden in der Liste der komprimierten Formate enthalten ist.

![Premium-komprimierungseinstellungen CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Stellen Sie sicher, dass der Inhalt zwischengespeichert wird
> [!NOTE]
> Dieser Schritt gilt nur, wenn das CDN-Profil ist ein **Azure CDN von Verizon** Profil (Standard oder Premium).
> 
> 

Überprüfen Sie mithilfe des Browsers Entwicklertools die Antwortheader, um sicherzustellen, dass die Datei in der Region zwischengespeichert wird, in denen es angefordert wird.

* Überprüfen Sie die **Server** Antwortheader.  Der Header sollte das Format aufweisen **Plattform (POP-Server-ID)**, wie im folgenden Beispiel hervorgeht.
* Überprüfen Sie die **X-Cache** Antwortheader.  Der Header sollte lesen **erreicht**.  

![CDN-Antwortheader](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Stellen Sie sicher, dass die Datei die größenanforderungen erfüllt.
> [!NOTE]
> Dieser Schritt gilt nur, wenn das CDN-Profil ist ein **Azure CDN von Verizon** Profil (Standard oder Premium).
> 
> 

Um für die Komprimierung kann, muss eine Datei die folgenden größenanforderungen erfüllen:

* Größer als 128 Bytes.
* Kleiner als 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Überprüfen Sie die Anforderung auf dem Ausgangsserver für eine **über** Header
Die **über** HTTP-Header der Webserver zeigt an, dass die Anforderung von einem Proxyserver übergeben wird.  Microsoft IIS-Webserver in der Standardeinstellung nicht komprimieren Antworten, wenn die Anforderung enthält ein **über** Header.  Um dieses Verhalten zu überschreiben, führen Sie folgende Schritte aus:

* **IIS 6**: [festgelegt HcNoCompressionForProxies = "FALSE" in der IIS-Metabasis](https://msdn.microsoft.com/library/ms525390.aspx)
* **IIS 7 und höher**: [setzen Sie beide **noCompressionForHttp10** und **NoCompressionForProxies** auf "false" in der Serverkonfiguration](http://www.iis.net/configreference/system.webserver/httpcompression)

