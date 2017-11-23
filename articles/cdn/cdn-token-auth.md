---
title: "Schützen von Azure CDN-Assets mit Tokenauthentifizierung | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die Tokenauthentifizierung zum Schützen des Zugriffs auf Ihre Azure CDN-Assets verwenden."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/03/2017
ms.author: mezha
ms.openlocfilehash: 2f62c0c6783c3cdaf1ffda3299673071b8e4a6f2
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Schützen von Azure Content Delivery Network-Assets mit Tokenauthentifizierung

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Übersicht

Die Tokenauthentifizierung ist ein Mechanismus, mit dem Sie verhindern können, dass Assets im Azure Content Delivery Network (CDN) für nicht autorisierte Clients bereitgestellt werden. Die Tokenauthentifizierung wird normalerweise genutzt, um das „Hotlinking“ von Inhalten zu verhindern. Dabei verwendet eine andere Website (häufig ein Diskussionsforum) Ihre Assets ohne Erlaubnis. Hotlinking kann sich auf Ihre Kosten für die Inhaltsbereitstellung auswirken. Durch Aktivieren der Tokenauthentifizierung im CDN werden Anforderungen von CDN-Edge POPs authentifiziert, bevor das CDN den Inhalt übermittelt. 

## <a name="how-it-works"></a>So funktioniert's

Bei der Tokenauthentifizierung wird überprüft, ob Anforderungen von einer vertrauenswürdigen Website generiert werden. Dazu müssen die Anforderungen einen Tokenwert mit codierten Informationen zur anfordernden Person enthalten. Inhalte werden der anfordernden Person nur bereitgestellt, wenn die codierten Informationen die erforderlichen Voraussetzungen erfüllen. Andernfalls werden die Anforderungen abgelehnt. Sie können die Anforderungen einrichten, indem Sie einen oder mehrere der folgenden Parameter verwenden:

- Land: Lassen Sie Anforderungen zu, die aus den Ländern stammen, die durch den [Ländercode](https://msdn.microsoft.com/library/mt761717.aspx) angegeben sind, oder verweigern Sie solche Anforderungen.
- URL: Lassen Sie nur Anforderungen zu, die dem angegebenen Asset oder Pfad entsprechen.
- Host: Lassen Sie Anforderungen zu, die im Anforderungsheader die angegebenen Hosts verwenden, oder verweigern Sie solche Anforderungen.
- Verweiser: Lassen Sie Anforderungen vom angegebenen Verweiser zu, oder verweigern Sie solche Anforderungen.
- IP-Adresse: Lassen Sie nur Anforderungen zu, die von einer bestimmten IP-Adresse oder aus einem bestimmten IP-Subnetz stammen.
- Protokoll: Lassen Sie Anforderungen basierend auf dem Protokoll zu, das zum Anfordern des Inhalts verwendet wird, oder verweigern Sie solche Anforderungen.
- Ablaufzeit: Weisen Sie einen Datums- und Zeitbereich zu, um sicherzustellen, dass ein Link nur für eine begrenzte Zeit gültig bleibt.

Weitere Informationen finden Sie in den ausführlichen Konfigurationsbeispielen für jeden Parameter unter [Einrichten der Tokenauthentifizierung](#setting-up-token-authentication).

## <a name="reference-architecture"></a>Referenzarchitektur

Im folgenden Workflowdiagramm wird beschrieben, wie das CDN die Tokenauthentifizierung verwendet, um mit der Web-App zu arbeiten.

![Worflow der CDN-Tokenauthentifizierung](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Tokenüberprüfungslogik auf dem CDN-Endpunkt
    
Im folgenden Flussdiagramm wird beschrieben, wie Azure CDN eine Clientanforderung überprüft, wenn die Tokenauthentifizierung auf dem CDN-Endpunkt konfiguriert ist.

![Logik der CDN-Tokenüberprüfung](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Einrichten der Tokenauthentifizierung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem CDN-Profil, und klicken Sie dann auf **Verwalten**, um das zusätzliche Portal zu starten.

    ![Schaltfläche „Verwalten“ für CDN-Profile](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Zeigen Sie mit der Maus auf **HTTP Large**, und klicken Sie im Flyout dann auf **Token Auth**. Sie können dann den Verschlüsselungsschlüssel und Verschlüsselungsparameter wie folgt einrichten:

    1. Erstellen Sie einen oder mehrere Verschlüsselungsschlüssel. Bei einem Verschlüsselungsschlüssel muss die Groß-/Kleinschreibung beachtet werden, und er kann eine beliebige Kombination aus alphanumerischen Zeichen enthalten. Alle anderen Arten von Zeichen, einschließlich Leerzeichen, sind nicht zulässig. Die maximale Länge beträgt 250 Zeichen. Um sicherzustellen, dass die Verschlüsselungsschlüssel auf Zufallsbasis generiert werden, sollten sie mit dem OpenSSL-Tool erstellt werden. Das OpenSSL-Tool hat die folgende Syntax: `rand -hex <key length>`. Beispiel: `OpenSSL> rand -hex 32`. Erstellen Sie sowohl einen Primär- als auch einen Sicherungsschlüssel, um Downtime zu vermeiden. Ein Sicherungsschlüssel ermöglicht während der Aktualisierung des Primärschlüssels unterbrechungsfreien Zugriff auf Ihre Inhalte.
    
    2. Geben Sie einen eindeutigen Verschlüsselungsschlüssel in das Feld **Primärschlüssel** ein, und geben Sie optional einen Sicherungsschlüssel in das Feld **Sicherungsschlüssel** ein.

    3. Wählen Sie in der Dropdownliste **Minimum Encryption Version** (Verschlüsselungsmindestversion) die Verschlüsselungsmindestversion aus, und klicken Sie dann auf **Aktualisieren**:
       - **V2**: Gibt an, dass mit dem Schlüssel Token der Version 2.0 und 3.0 generiert werden können. Verwenden Sie diese Option nur bei der Umstellung von einem älteren Verschlüsselungsschlüssel der Version 2.0 auf einen Schlüssel der Version 3.0.
       - **V3**: (Empfohlen) Gibt an, dass mit dem Schlüssel nur Token der Version 3.0 generiert werden können.

    ![CDN-Tokenauthentifizierung – Einrichten des Schlüssels](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    4. Verwenden Sie das Verschlüsselungstool, um Verschlüsselungsparameter einzurichten und ein Token zu generieren. Mit dem Verschlüsselungstool können Sie Anforderungen basierend auf Ablaufzeit, Land, Verweiser, Protokoll und Client-IP (in beliebigen Kombinationen) zulassen oder ablehnen. Anzahl und Art der Parameter, die zur Bildung eines Tokens kombiniert werden können, sind zwar nicht beschränkt, die Gesamtlänge des Tokens darf jedoch maximal 512 Zeichen betragen. 

       ![CDN-Verschlüsselungstool](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       Geben Sie Werte für einen oder mehrere der folgenden Verschlüsselungsparameter im Abschnitt für das **Verschlüsselungstool** ein:  

       - **ec_expire**: Dient zum Zuweisen einer Ablaufzeit zu einem Token, nach der das Token abläuft. Anforderungen, die nach der Ablaufzeit übermittelt werden, werden abgelehnt. Dieser Parameter verwendet einen UNIX-Zeitstempel, der auf der Anzahl der Sekunden seit Beginn der Standardepoche `1/1/1970 00:00:00 GMT` basiert. (Sie können Onlinetools für die Konvertierung zwischen der Standardzeit und der UNIX-Zeit verwenden.) Wenn das Token beispielsweise am `12/31/2016 12:00:00 GMT` ablaufen soll, verwenden Sie den UNIX-Zeitstempelwert `1483185600` wie folgt. 
    
         ![CDN-Beispiel für „ec_expire“](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - **ec_url_allow**: Dient zum Anpassen von Token an ein bestimmtes Asset oder einen Pfad. Der Zugriff wird auf Anforderungen beschränkt, deren URL mit einem bestimmten relativen Pfad beginnt. Bei URLs wird die Groß-/Kleinschreibung berücksichtigt. Geben Sie mehrere Pfade ein, indem Sie als Trennzeichen jeweils ein Komma verwenden. Je nach Ihren Anforderungen können Sie andere Werte angeben, um unterschiedliche Zugriffsebenen bereitzustellen. 
        
         Für die URL `http://www.mydomain.com/pictures/city/strasbourg.png` sind diese Anforderungen z.B. für die folgenden Eingabewerte zulässig:

         - Eingabewert `/`: Alle Anforderungen sind zulässig.
         - Eingabewert `/pictures`: Die folgenden Anforderungen sind zulässig:
            - `http://www.mydomain.com/pictures.png`
            - `http://www.mydomain.com/pictures/city/strasbourg.png`
            - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
         - Eingabewert `/pictures/`: Nur Anforderungen mit dem Pfad `/pictures/` sind zulässig. Beispiel: `http://www.mydomain.com/pictures/city/strasbourg.png`.
         - Eingabewert `/pictures/city/strasbourg.png`: Nur Anforderungen für diesen speziellen Pfad und dieses Asset sind zulässig.
    
       - **ec_country_allow**: Es sind nur Anforderungen zulässig, die aus einem oder mehreren der angegebenen Ländern stammen. Anforderungen aus allen anderen Ländern werden abgelehnt. Verwenden Sie Ländercodes, und trennen Sie sie durch Kommas voneinander. Beispiel: Wenn Sie den Zugriff nur aus den USA und aus Frankreich zulassen möchten, geben Sie wie folgt „US, FR“ in das Feld ein.  
        
           ![CDN-Beispiel für „ec_country_allow“](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - **ec_country_deny**: Dient zum Ablehnen von Anforderungen, die aus einem oder mehreren angegebenen Ländern stammen. Anforderungen aus allen anderen Ländern werden zugelassen. Verwenden Sie Ländercodes, und trennen Sie sie durch Kommas voneinander. Beispiel: Wenn Sie den Zugriff aus den USA und aus Frankreich ablehnen möchten, geben Sie „US, FR“ in das Feld ein.
    
       - **ec_ref_allow**: Es werden nur Anforderungen vom angegebenen Verweiser zugelassen. Mit einem Verweiser wird die URL der Webseite identifiziert, die als Link zur angeforderten Ressource dient. Nehmen Sie das Protokoll nicht in den Parameterwert des Verweisers auf. Die folgenden Typen von Eingaben sind für den Parameterwert zulässig:
           - Ein Hostname oder ein Hostname und ein Pfad.
           - Mehrere Verweiser. Um mehrere Verweiser hinzuzufügen, trennen Sie die Verweiser durch Kommas. Wenn Sie einen Verweiserwert angeben, die entsprechenden Informationen aufgrund der Browserkonfiguration aber nicht mit der Anforderung gesendet werden, wird die Anforderung standardmäßig abgelehnt. 
           - Anforderungen mit fehlenden Verweiserinformationen. Um diese Typen von Anforderungen zuzulassen, geben Sie den Text „missing“ oder einen leeren Wert ein. 
           - Unterdomänen. Um Unterdomänen zuzulassen, geben Sie ein Sternchen (\*) ein. Um beispielsweise alle Unterdomänen von `consoto.com` zuzulassen, geben Sie `*.consoto.com` ein. 
           
          Das folgende Beispiel zeigt die Eingabe, um Zugriff für Anforderungen von `www.consoto.com`, allen Unterdomänen unter `consoto2.com` und Anforderungen mit leeren oder fehlenden Verweisern zuzulassen.
        
          ![CDN-Beispiel für „ec_ref_allow“](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - **ec_ref_deny**: Dient zum Ablehnen von Anforderungen über den angegebenen Verweiser. Die Implementierung ist identisch mit dem ec_ref_allow-Parameter.
         
       - **ec_proto_allow**: Es werden nur Anforderungen vom angegebenen Protokoll zugelassen. Beispiel: HTTP oder HTTPS.
            
       - **ec_proto_deny**: Dient zum Ablehnen von Anforderungen vom angegebenen Protokoll. Beispiel: HTTP oder HTTPS.
    
       - **ec_clientip**: Beschränkt den Zugriff auf die IP-Adresse der angegebenen anfordernden Person. IPv4 und IPv6 werden unterstützt. Sie können eine einzelne IP-Adresse oder ein IP-Subnetz für eine Anforderung angeben.
            
         ![CDN-Beispiel für „ec_clientip“](./media/cdn-token-auth/cdn-token-auth-clientip.png)

    5. Nach der Eingabe von Werten für die Verschlüsselungsparameter wählen Sie den Schlüssel zum Verschlüsseln (wenn Sie einen Primär- und einen Sicherungsschlüssel erstellt haben) aus der Liste **Schlüssel zum Verschlüsseln** aus.
    
    6. Wählen Sie eine Verschlüsselungsversion aus der Liste **Verschlüsselungsversion** aus: **V2** für Version 2 bzw. **V3** für Version 3 (empfohlen). Klicken Sie auf **Verschlüsseln**, um das Token zu generieren.

    Nachdem das Token generiert wurde, wird es im Feld **Generated Token** (Generiertes Token) angezeigt. Zur Verwendung des Tokens fügen Sie es als Abfragezeichenfolge am Ende der Datei im URL-Pfad ein. Beispiel: `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
    7. Sie können Ihr Token optional mit dem Entschlüsselungstool testen. Fügen Sie den Tokenwert in das Feld **Token zum Entschlüsseln** ein. Wählen Sie die Nutzung des Verschlüsselungsschlüssels aus der Dropdownliste **Schlüssel zum Entschlüsseln** aus, und klicken Sie auf **Entschlüsseln**.

    Nach der Entschlüsselung des Tokens werden seine Parameter im Feld **Ursprüngliche Parameter** angezeigt.

    8. Passen Sie optional den Typ des Antwortcodes an, der zurückgegeben wird, wenn eine Anforderung abgelehnt wird. Wählen Sie den Code aus der Dropdownliste **Antwortcode** aus, und klicken Sie auf **Speichern**. Der Antwortcode **403** („Unzulässig“) ist standardmäßig aktiviert. Für bestimmte Antwortcodes können Sie auch die URL Ihrer Fehlerseite in das Feld **Headerwert** eingeben. 

3. Klicken Sie unter **HTTP Large** auf **Regelmodul**. Sie verwenden das Regelmodul, um Pfade zum Anwenden der Funktion zu definieren und die Tokenauthentifizierung sowie weitere Funktionen zur Tokenauthentifizierung zu aktivieren. Weitere Informationen finden Sie unter [Azure CDN-Regelmodul](cdn-rules-engine-reference.md).

    1. Wählen Sie eine vorhandene Regel aus, oder erstellen Sie eine neue Regel, um das Asset oder den Pfad zu definieren, auf das bzw. den Sie die Tokenauthentifizierung anwenden möchten. 
    2. Zum Aktivieren der Tokenauthentifizierung für eine Regel wählen Sie **[Token Auth](cdn-rules-engine-reference-features.md#token-auth)** aus der Dropdownliste **Features** und dann **Aktiviert** aus. Klicken Sie auf **Aktualisieren**, wenn Sie eine Regel aktualisieren, oder auf **Hinzufügen**, wenn Sie eine Regel erstellen.
        
    ![Beispiel für das CDN-Regelmodul: Tokenauthentifizierung aktiviert](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Im Regelmodul können Sie auch weitere Features im Zusammenhang mit der Tokenauthentifizierung aktivieren. Um die folgenden Features zu aktivieren, wählen Sie sie in der Dropdownliste **Features** aus, und wählen Sie dann **Aktiviert** aus.
    
    - **[Token Auth Denial Code](cdn-rules-engine-reference-features.md#token-auth-denial-code)**: Gibt den Typ der Antwort an, die an einen Benutzer zurückgegeben wird, wenn eine Anforderung abgelehnt wird. Hier festgelegte Regeln setzen den Antwortcode außer Kraft, der im Abschnitt **Custom Denial Handling** auf der Seite für die tokenbasierte Authentifizierung festgelegt wurde.
    - **[Token Auth Ignore URL Case](cdn-rules-engine-reference-features.md#token-auth-ignore-url-case)**: Legt fest, ob für die URL, die zum Überprüfen des Tokens verwendet wird, die Groß-/Kleinschreibung berücksichtigt wird.
    - **[Token Auth Parameter](cdn-rules-engine-reference-features.md#token-auth-parameter)**: Benennt den Parameter für die Tokenauthentifizierung-Abfragezeichenfolge um, der in der angeforderten URL angezeigt wird. 
        
    ![Beispiel für das CDN-Regelmodul: Einstellungen der Tokenauthentifizierung](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Sie können Ihr Token anpassen, indem Sie auf Quellcode in [GitHub](https://github.com/VerizonDigital/ectoken) zugreifen.
Verfügbare Sprachen:
    
- C
- C#
- PHP
- Perl
- Java
- Python    

## <a name="azure-cdn-features-and-provider-pricing"></a>Preise für Azure CDN-Funktionen und -Anbieter

Informationen zu Features finden Sie unter [Übersicht über das Azure Content Delivery Network (CDN)](cdn-overview.md). Weitere Informationen zur Preisgestaltung finden Sie unter [Azure Content Delivery Network – Preise ](https://azure.microsoft.com/pricing/details/cdn/).
