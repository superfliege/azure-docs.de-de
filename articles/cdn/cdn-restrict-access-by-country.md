---
title: Einschränken von Azure CDN-Inhalten nach Ländern | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff auf Ihre Azure CDN-Inhalte mithilfe der Geofilterung einschränken.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: 93321c4c8a7f8d79835d702ca07132eed94f6493
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260751"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Einschränken von Azure-CDN-Inhalten nach Land

## <a name="overview"></a>Übersicht
Wenn ein Benutzer Ihren Inhalt anfordert, wird unabhängig davon, wo der Benutzer diese Anforderung vorgenommen hat, der Inhalt angezeigt. In einigen Fällen empfiehlt es sich, den Zugriff auf Ihre Inhalte nach Ländern einzuschränken. In diesem Artikel wird erläutert, wie Sie die *Geofilterung* einsetzen, um den Zugriff je nach Land zu genehmigen oder zu verweigern.

> [!IMPORTANT]
> Die Azure CDN-Produkte bieten die gleichen Geofilterungsfunktionen, die jeweils unterstützten Ländercodes unterscheiden sich jedoch geringfügig. Weitere Informationen finden Sie unter [Azure CDN Country Codes](https://msdn.microsoft.com/library/mt761717.aspx) (Azure CDN-Ländercodes).


Informationen zu Überlegungen, die Sie beim Konfigurieren dieser Art von Einschränkungen berücksichtigen sollten, finden Sie im Abschnitt [Überlegungen](cdn-restrict-access-by-country.md#considerations).  

![Länderfilter](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a>Schritt 1: Definieren des Verzeichnispfads
> [!IMPORTANT]
> Profile vom Typ **Azure CDN Standard von Microsoft** unterstützen die pfadbasierte Geofilterung nicht.
>


Wählen Sie den Endpunkt innerhalb des Portals, und suchen Sie die Registerkarte Geofilterung in der linken Navigationsleiste.

Wenn Sie einen Länderfilter konfigurieren, müssen Sie den relativen Pfad zum Speicherort angeben, an dem der Zugriff für Benutzer zugelassen oder verweigert wird. Sie können Geofilterung für alle Dateien durch Eingabe eines Schrägstrichs (/) oder für ausgewählte Ordner durch Angabe von Verzeichnispfaden (*/pictures/*) anwenden. Sie können die Geofilterung auch auf eine einzige Datei anwenden, indem Sie die Datei angeben, und den nachgestellten Schrägstrich (*/pictures/city.png*) weglassen.

Beispiel für Verzeichnispfadfilter:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a>Schritt 2: Definieren der Aktion: Blockieren oder Zulassen
**Blockieren:** Benutzern aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen verweigert. Wenn keine anderen Länderfilteroptionen für diesen Standort konfiguriert wurden, wird der Zugriff für alle anderen Benutzer zugelassen.

**Zulassen:** Nur für Benutzer aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen zugelassen.

## <a name="step-3-define-the-countries"></a>Schritt 3: Definieren der Länder
Wählen Sie die Länder aus, die Sie für den Pfad zulassen oder blockieren möchten. 

Beispielsweise werden durch die Regel zum Blockieren von "/Photos/Strasbourg/" die folgenden Dateien gefiltert:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Landeskennzahlen
Die Geofilterung verwendet Ländercodes, um die Länder zu definieren, von denen aus eine Anforderung für ein sicheres Verzeichnis zugelassen oder blockiert wird. Obwohl die Azure CDN-Produkte die gleichen Geofilterungsfunktionen bieten, unterscheiden sich die jeweils unterstützten Ländercodes geringfügig. Weitere Informationen finden Sie unter [Azure CDN Country Codes](https://msdn.microsoft.com/library/mt761717.aspx) (Azure CDN-Ländercodes). 

## <a name="considerations"></a>Überlegungen
* Änderungen an der Länderfilterkonfiguration treten nicht sofort in Kraft:
   * Bei Profilen vom Typ **Azure CDN Standard von Microsoft** ist die Weitergabe in der Regel in zehn Minuten abgeschlossen. 
   * Bei **Azure CDN Standard von Akamai**-Profilen ist die Weitergabe in der Regel in einer Minute abgeschlossen. 
   * Bei Profilen vom Typ **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** ist die Weitergabe in der Regel in zehn Minuten abgeschlossen. 
 
* Diese Funktion unterstützt keine Platzhalterzeichen (z. B. "*").

* Die dem relativen Pfad zugeordnete Geofilterkonfiguration wird rekursiv auf diesen Pfad angewendet.

* Auf einen relativen Pfad kann immer nur jeweils eine Regel angewendet werden. Sie können also nicht mehrere Länderfilter erstellen, die auf den gleichen relativen Pfad verweisen. Aufgrund der rekursiven Natur der Länderfilter kann ein Ordner jedoch über mehrere Länderfilter verfügen. Anders ausgedrückt: Ein Unterordner eines zuvor konfigurierten Ordners kann einem anderen Länderfilter zugewiesen werden.

