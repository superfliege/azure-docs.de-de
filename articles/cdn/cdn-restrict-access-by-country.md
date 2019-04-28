---
title: Einschränken von Azure CDN-Inhalten nach Ländern | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff auf Ihre Azure CDN-Inhalte mithilfe der Geofilterung nach Ländern einschränken.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: magattus
ms.openlocfilehash: 248a51da76cdee06e55438a706c543c70dcf141e
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526180"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Einschränken von Azure-CDN-Inhalten nach Land

## <a name="overview"></a>Übersicht
Wenn ein Benutzer Ihren Inhalt anfordert, wird der Inhalt standardmäßig unabhängig vom Standort des Benutzers angezeigt, der die Anforderung vorgenommen hat. In einigen Fällen sollten Sie den Zugriff auf Ihre Inhalte jedoch nach Ländern einzuschränken. Mit dem Feature *Geofilterung* können Sie Regeln zu bestimmten Pfaden auf Ihrem CDN-Endpunkt erstellen, um Inhalt in ausgewählten Ländern zuzulassen oder zu blockieren.

> [!IMPORTANT]
> Profile vom Typ **Azure CDN Standard von Microsoft** unterstützen die pfadbasierte Geofilterung nicht.
> 

## <a name="standard-profiles"></a>Standardprofile
Die Verfahren in diesem Abschnitt gelten nur für Profile des Typs **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon**. 

Für Profile des Typs **Azure CDN Premium von Verizon** müssen Sie die Geofilterung im **Verwaltungsportal** aktivieren. Weitere Informationen finden Sie unter [Profile des Typs „Azure CDN Premium von Verizon“](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Definieren des Verzeichnispfads
Um auf das Geofilterungsfeature zuzugreifen, wählen Sie Ihren CDN-Endpunkt im Portal aus, und wählen Sie dann im linken Menü unter „EINSTELLUNGEN“ **Geofilterung** aus. 

![Geofilterungsstandard](./media/cdn-filtering/cdn-geo-filtering-standard.png)

Geben Sie im Feld **PFAD** den relativen Pfad zu dem Speicherort an, für den Benutzern der Zugriff gewährt oder verweigert wird. 

Sie können Geofilterung durch Eingabe eines Schrägstrichs (/) auf alle Dateien anwenden oder bestimmte Ordner durch Angabe von Verzeichnispfaden (z.B. */pictures/*) auswählen. Sie können Geofilterung auch auf eine einzelne Datei anwenden (z.B. */pictures/city.png*). Mehrere Regeln sind zulässig. Nachdem Sie eine Regel eingegeben haben, wird eine leere Zeile angezeigt, in der Sie die nächste Regel eingeben können.

Alle folgenden Verzeichnispfadfilter sind z.B. gültig:   
*/*                                 
*/Photos/*     
*/Photos/Straßburg /*     
*/Photos/Strasbourg/city.png*

### <a name="define-the-type-of-action"></a>Definieren des Aktionstyps

Wählen Sie in der Liste **AKTION** die Option **Zulassen** oder **Blockieren** aus: 

- **Zulassen**: Nur für Benutzer aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen zugelassen.

- **Blockieren**: Benutzern aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen verweigert. Wenn keine anderen Länderfilteroptionen für diesen Standort konfiguriert wurden, wird der Zugriff für alle anderen Benutzer zugelassen.

Eine Geofilterungsregel zum Blockieren des Pfads */Photos/Strasbourg/* filtert z.B. die folgenden Dateien:     
*http:\//\<Endpunkt>.azureedge.net/Photos/Strasbourg/1000.jpg*
*http:\//\<Endpunkt>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countries"></a>Definieren der Länder
Wählen Sie in der Liste **LÄNDERCODES** die Länder aus, die Sie für den Pfad zulassen oder blockieren möchten. 

Wählen Sie nach dem Auswählen der Länder **Speichern**, um die neue Geofilterungsregel zu aktivieren. 

![Geofilterungsregeln](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Um eine Regel zu löschen, wählen Sie sie in der Liste auf der Seite **Geofilterung** aus, und wählen Sie dann **Löschen**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Profile des Typs „Azure CDN Premium von Verizon“
Für Profile des Typs **Azure CDN Premium von Verizon** sieht die Benutzeroberfläche zum Erstellen einer Geofilterungsregel anders aus:

1. Wählen Sie im obersten Menü Ihres Azure CDN-Profils **Verwalten** aus.

2. Wählen Sie im Verizon-Portal **HTTP Groß** und dann **Länderfilterung** aus.

    ![Geofilterungsstandard](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Wählen Sie **Länderfilter hinzufügen** aus.

    Die Seite **Schritt 1:** wird angezeigt.

4. Geben Sie den Pfad des Verzeichnisses ein, wählen Sie **Blockieren** oder **Hinzufügen** aus und dann **Weiter**.

    Die Seite **Schritt 2:** wird angezeigt. 

5. Wählen Sie mindestens ein Land in der Liste aus und dann **Fertig stellen**, um die Regel zu aktivieren. 
    
    Die neue Regel wird in der Tabelle auf der Seite **Länderfilterung** angezeigt.

    ![Geofilterungsregeln](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wählen Sie in der Länderfilterungsregel-Tabelle das Löschsymbol neben einer Regel aus, um sie zu löschen, oder das Bearbeitungssymbol, um sie zu bearbeiten.

## <a name="considerations"></a>Überlegungen
* Änderungen an der Geofilterungskonfiguration treten nicht sofort in Kraft:
   * Bei Profilen vom Typ **Azure CDN Standard von Microsoft** ist die Weitergabe in der Regel in zehn Minuten abgeschlossen. 
   * Bei **Azure CDN Standard von Akamai**-Profilen ist die Weitergabe in der Regel in einer Minute abgeschlossen. 
   * Bei Profilen vom Typ **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** ist die Weitergabe in der Regel in zehn Minuten abgeschlossen. 
 
* Dieses Feature unterstützt keine Platzhalterzeichen (z.B. „*“).

* Die dem relativen Pfad zugeordnete Geofilterkonfiguration wird rekursiv auf diesen Pfad angewendet.

* Auf einen relativen Pfad kann immer nur jeweils eine Regel angewendet werden. Sie können also nicht mehrere Länderfilter erstellen, die auf den gleichen relativen Pfad verweisen. Weil Länderfilter rekursiv sind, kann ein Ordner jedoch über mehrere Länderfilter verfügen. Anders ausgedrückt: Ein Unterordner eines zuvor konfigurierten Ordners kann einem anderen Länderfilter zugewiesen werden.

* Das Geofilterungsfeature verwendet Ländercodes, um die Länder zu definieren, von denen aus eine Anforderung für ein sicheres Verzeichnis zugelassen oder blockiert wird. Obwohl Akamai- und Verizon-Profile größtenteils die gleichen Ländercodes unterstützen, gibt es einige Unterschiede. Weitere Informationen finden Sie unter [Azure CDN-Ländercodes](/previous-versions/azure/mt761717(v=azure.100)). 

