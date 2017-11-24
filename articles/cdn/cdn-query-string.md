---
title: Steuern des Azure CDN-Zwischenspeicherverhaltens mit Abfragezeichenfolgen | Microsoft-Dokumentation
description: Das Zwischenspeichern von Azure CDN-Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn diese Abfragezeichenfolgen enthalten.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 28e724f34c32edb0d5641b24f9ffedb7dc5f9680
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Steuern des Azure Content Delivery Network-Zwischenspeicherverhaltens mit Abfragezeichenfolgen
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium von Verizon](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Übersicht
Mit Azure Content Delivery Network (CDN) können Sie steuern, wie Dateien für eine Webanforderung, die eine Abfragezeichenfolge enthält, zwischengespeichert werden. In einer Webanforderung mit einer Abfragezeichenfolge ist die Abfragezeichenfolge der Teil der Anforderung, die nach dem Zeichen `?` steht. Eine Abfragezeichenfolge kann einen oder mehrere Parameter enthalten, die durch das Zeichen `&` voneinander getrennt sind. Beispiel: `http://www.domain.com/content.mov?data1=true&data2=false`. Wenn eine Anforderung mehr als einen Abfragezeichenfolgenparameter aufweist, spielt die Reihenfolge der Parameter keine Rolle. 

> [!IMPORTANT]
> Die CDN-Produkte „Standard“ und „Premium“ bieten die gleiche Funktionalität zum Zwischenspeichern von Abfragezeichenfolgen, aber mit einer anderen Benutzeroberfläche.  Dieser Artikel beschreibt die Benutzeroberfläche für **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon**. Informationen zum Zwischenspeichern von Abfragezeichenfolgen mit **Azure CDN Premium von Verizon**finden Sie unter [Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen – Premium](cdn-query-string-premium.md).

Es sind drei Abfragezeichenfolgenmodi verfügbar:

- **Abfragezeichenfolgen ignorieren**: Standardmodus. In diesem Modus übergibt der CDN-Edgeknoten die Abfragezeichenfolgen bei der ersten Anforderung vom Anforderer an den Ursprung und speichert das Objekt im Cache zwischen. Alle nachfolgenden Anforderungen des Objekts, die vom Edgeknoten verarbeitet werden, ignorieren die Abfragezeichenfolgen bis zum Ablauf des zwischengespeicherten Objekts.
- **Zwischenspeicherung für Abfragezeichenfolgen umgehen**: In diesem Modus werden Anforderungen mit Abfragezeichenfolgen nicht auf dem CDN-Edgeknoten zwischengespeichert. Der Edgeknoten ruft das Objekt direkt vom Ursprung ab und übergibt es bei jeder Anforderung an den Anforderer.
- **Jede eindeutige URL zwischenspeichern**: In diesem Modus wird jede Anforderung mit einer eindeutigen URL, einschließlich der Abfragezeichenfolge, als eindeutiges Objekt mit eigenem Cache behandelt. So wird beispielsweise die Antwort vom Ursprung für eine Anforderung für `example.ashx?q=test1` auf dem Edgeknoten zwischengespeichert und für nachfolgende Caches mit der gleichen Abfragezeichenfolge zurückgegeben. Eine Anforderung für `example.ashx?q=test2` wird als separates Objekt mit eigener Einstellung für die Gültigkeitsdauer zwischengespeichert.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Ändern der Einstellungen für das Zwischenspeichern von Abfragezeichenfolgen für CDN-Standardprofile
1. Öffnen Sie ein CDN-Profil, und wählen Sie den zu verwaltenden CDN-Endpunkt aus.
   
   ![CDN-Profilendpunkte](./media/cdn-query-string/cdn-endpoints.png)
   
2. Klicken Sie unter „Einstellungen“ auf **Cache**.
   
    ![Schaltfläche „Cache“ im CDN-Profil](./media/cdn-query-string/cdn-cache-btn.png)
   
3. Wählen Sie in der Liste **Verhalten für das Zwischenspeichern von Abfragezeichenfolgen** einen Abfragezeichenfolgenmodus, und klicken Sie auf **Speichern**.
   
  <!--- Replace screen shot after general caching goes live ![CDN query string caching options](./media/cdn-query-string/cdn-query-string.png) --->

> [!IMPORTANT]
> Da die Verteilung der Registrierung über das CDN eine Weile dauern kann, sind die Änderungen der Cachezeichenfolgen-Einstellungen unter Umständen nicht sofort sichtbar. Bei **Azure CDN von Akamai** -Profilen ist die Weitergabe in der Regel in einer Minute abgeschlossen. Bei Profilen vom Typ **Azure CDN von Verizon** ist die Weitergabe in der Regel in 90 Minuten abgeschlossen, in manchen Fällen kann es aber länger dauern.


