---
title: "Steuern des Azure CDN-Zwischenspeicherverhaltens mit Abfragezeichenfolgen – Premium | Microsoft-Dokumentation"
description: Das Zwischenspeichern von Azure CDN-Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn diese Abfragezeichenfolgen enthalten.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: ba9c28f0e6df25b101b45edf836d0b95056cbc6f
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings---premium"></a>Steuern des Azure Content Delivery Network-Zwischenspeicherverhaltens mit Abfragezeichenfolgen – Premium
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium von Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Übersicht
Mit Azure Content Delivery Network (CDN) können Sie steuern, wie Dateien für eine Webanforderung, die eine Abfragezeichenfolge enthält, zwischengespeichert werden. In einer Webanforderung mit einer Abfragezeichenfolge ist die Abfragezeichenfolge der Teil der Anforderung, die nach dem Zeichen `?` steht. Eine Abfragezeichenfolge kann einen oder mehrere Parameter enthalten, die durch das Zeichen `&` voneinander getrennt sind. Beispiel: `http://www.domain.com/content.mov?data1=true&data2=false`. Wenn eine Anforderung mehr als einen Abfragezeichenfolgenparameter aufweist, spielt die Reihenfolge der Parameter keine Rolle. 

> [!IMPORTANT]
> Die CDN-Produkte „Standard“ und „Premium“ bieten die gleiche Funktionalität zum Zwischenspeichern von Abfragezeichenfolgen, aber mit einer anderen Benutzeroberfläche.  In diesem Artikel wird die Benutzeroberfläche für **Azure CDN Premium von Verizon** beschrieben. Informationen zum Zwischenspeichern von Abfragezeichenfolgen mit **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon** finden Sie unter [Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen](cdn-query-string.md).
>

Es sind drei Abfragezeichenfolgenmodi verfügbar:

- **standard-cache**: Standardmodus. In diesem Modus übergibt der CDN-Edgeknoten die Abfragezeichenfolgen bei der ersten Anforderung vom Anforderer an den Ursprung und speichert das Objekt im Cache zwischen. Für alle nachfolgenden Anforderungen des Objekts, die vom Edgeknoten verarbeitet werden, werden die Abfragezeichenfolgen bis zum Ablauf des zwischengespeicherten Objekts ignoriert.
- **no-cache:**In diesem Modus werden Anforderungen mit Abfragezeichenfolgen nicht auf dem CDN-Edgeknoten zwischengespeichert. Der Edgeknoten ruft das Objekt direkt vom Ursprung ab und übergibt es bei jeder Anforderung an den Anforderer.
- **unique-cache**: In diesem Modus wird jede Anforderung mit einer eindeutigen URL, einschließlich der Abfragezeichenfolge, als eindeutiges Objekt mit eigenem Cache behandelt. So wird beispielsweise die Antwort vom Ursprung für eine Anforderung für `example.ashx?q=test1` auf dem Edgeknoten zwischengespeichert und für nachfolgende Caches mit der gleichen Abfragezeichenfolge zurückgegeben. Eine Anforderung für `example.ashx?q=test2` wird als separates Objekt mit eigener Einstellung für die Gültigkeitsdauer zwischengespeichert.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Ändern der Einstellungen für das Zwischenspeichern von Abfragezeichenfolgen für CDN-Premiumprofile
1. Öffnen Sie ein CDN-Profil, und klicken Sie anschließend auf **Verwalten**.
   
    ![Schaltfläche „Verwalten“ für CDN-Profile](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Das CDN-Verwaltungsportal wird geöffnet.
2. Zeigen Sie auf die Registerkarte **HTTP Groß** und dann auf das Flyout-Menü **Cacheeinstellungen**. Klicken Sie auf **Query-String Caching**.
   
    Die Zwischenspeicherungsoptionen für Abfragezeichenfolgen werden angezeigt.
   
    ![Zwischenspeicherungsoptionen für CDN-Abfragezeichenfolgen](./media/cdn-query-string-premium/cdn-query-string.png)
3. Wählen Sie einen Abfragezeichenfolgenmodus aus, und klicken Sie auf **Aktualisieren**.

> [!IMPORTANT]
> Da die Verteilung der Registrierung über das CDN eine Weile dauern kann, sind die Änderungen der Cachezeichenfolgen-Einstellungen unter Umständen nicht sofort sichtbar. Bei Profilen vom Typ **Azure CDN Premium von Verizon** ist die Weitergabe in der Regel in 90 Minuten abgeschlossen, aber in einigen Fällen kann es auch länger dauern.
 

