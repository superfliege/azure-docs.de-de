---
title: "Übereinstimmungsbedingungen für die Azure CDN-Regel-Engine | Microsoft-Dokumentation"
description: "Regel zu Übereinstimmungsbedingungen für die Azure Content Delivery Network-Regel-Engine."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 9986e654b076df099e3912f9da628728723b5c3d
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="match-conditions-for-the-azure-cdn-rules-engine"></a>Übereinstimmungsbedingungen für die Azure CDN-Regel-Engine
Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen für die [Regel-Engine](cdn-rules-engine.md) des Azure Content Delivery Network (CDN).

Der zweite Teil einer Regel ist die Übereinstimmungsbedingung. Eine Übereinstimmungsbedingung gibt bestimmte Typen von Anforderungen an, für die verschiedene sog. Features ausgeführt werden.

Sie können eine Übereinstimmungsbedingung z.B. für Folgendes verwenden:
- Filtern von Anforderungen für Inhalte an einem bestimmten Standort
- Filtern von Anforderungen, die von einer bestimmten IP-Adresse oder einem bestimmten Land generiert wurden
- Filtern von Anforderungen nach Headerinformationen

## <a name="always-match-condition"></a>Übereinstimmungsbedingung „Always“

Die Übereinstimmungsbedingung „Always“ wendet einen Standardsatz von Features auf alle Anforderungen an.

NAME | Zweck
-----|--------
[Always](#always) | Wendet einen Standardsatz von Features auf alle Anforderungen an.

## <a name="device-match-condition"></a>Übereinstimmungsbedingung „Device“

Die Übereinstimmungsbedingung „Device“ bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften.  

NAME | Zweck
-----|--------
[Device](#device) | Bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften.

## <a name="location-match-conditions"></a>Location-Übereinstimmungsbedingungen

Die Location-Übereinstimmungsbedingungen bestimmen Anforderungen basierend auf dem Standort des Anfordernden.

NAME | Zweck
-----|--------
[AS Number](#as-number) | Bestimmt Anforderungen, die aus einem bestimmten Netzwerk stammen.
[Country](#country) | Bestimmt Anforderungen, die aus den angegebenen Ländern stammen.

## <a name="origin-match-conditions"></a>Origin-Übereinstimmungsbedingungen

Die Origin-Übereinstimmungsbedingungen bestimmen Anforderungen, die auf CDN-Speicher oder einen Kundenursprungsserver zeigen.

NAME | Zweck
-----|--------
[CDN Origin](#cdn-origin) | Bestimmt Anforderungen für Inhalte, die in Content Delivery Network-Speicher gespeichert sind.
[Customer Origin](#customer-origin) | Bestimmt Anforderungen von Inhalten, die auf einem spezifischen Kundenursprungsserver gespeichert sind.

## <a name="request-match-conditions"></a>Request-Übereinstimmungsbedingungen

Die Request-Übereinstimmungsbedingungen bestimmen Anforderungen basierend auf ihren Eigenschaften.

NAME | Zweck
-----|--------
[Client IP Address](#client-ip-address) | Bestimmt Anforderungen, die von einer bestimmten IP-Adresse stammen.
[Cookie Parameter](#cookie-parameter) | Überprüft die Cookies, die jeder Anforderung für den angegebenen Wert zugeordnet sind.
[Cookie Parameter Regex](#cookie-parameter-regex) | Überprüft die Cookies, die jeder Anforderung zugeordnet sind, auf den angegebenen regulären Ausdruck.
[Edge Cname](#edge-cname) | Bestimmt Anforderungen, die auf einen bestimmten Edge-CNAME zeigen.
[Referring Domain](#referring-domain) | Bestimmt Anforderungen, auf die von den angegebenen Hostnamen verwiesen wurde.
[Request Header Literal](#request-header-literal) | Bestimmt Anforderungen, die den angegebenen auf einen bestimmten Wert festgelegten Header enthalten.
[Request Header Regex](#request-header-regex) | Bestimmt Anforderungen, die den angegebenen Header enthalten, der auf einen Wert festgelegt ist, der dem angegebenen regulären Ausdruck entspricht.
[Request Header Wildcard](#request-header-wildcard) | Bestimmt Anforderungen, die den angegebenen Header enthalten, der auf einen Wert festgelegt ist, der dem angegebenen Muster entspricht.
[Request Method](#request-method) | Bestimmt Anforderungen anhand ihrer HTTP-Methode.
[Request Scheme](#request-scheme) | Bestimmt Anforderungen anhand ihres HTTP-Protokolls.

## <a name="url-match-conditions"></a>URL-Übereinstimmungsbedingungen

Die URL-Übereinstimmungsbedingungen bestimmen Anforderungen basierend auf ihren URLs.

NAME | Zweck
-----|--------
URL Path Directory | Bestimmt Anforderungen anhand ihres relativen Pfads.
URL Path Extension | Bestimmt Anforderungen anhand ihrer Dateinamenerweiterung.
URL Path Filename | Bestimmt Anforderungen anhand ihres Dateinamens.
URL Path Literal | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen Wert.
URL Path Regex | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen regulären Ausdruck.
URL Path Wildcard | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen Muster.
URL Query Literal | Vergleicht die Abfragezeichenfolge einer Anforderung mit dem angegebenen Wert.
URL Query Parameter | Bestimmt Anforderungen, die den angegebenen Abfragezeichenfolgen-Parameter enthalten, der auf einen Wert festgelegt ist, der einem angegebenen Muster entspricht.
URL Query Regex | Bestimmt Anforderungen, die den angegebenen Abfragezeichenfolgen-Parameter enthalten, der auf einen Wert festgelegt ist, der einem angegebenen regulären Ausdruck entspricht.
URL Query Wildcard | Vergleicht den angegebenen Wert mit der Abfragezeichenfolge der Anforderung.


## <a name="reference-for-rules-engine-match-conditions"></a>Referenz für Übereinstimmungsbedingungen der Regel-Engine

---
### <a name="always"></a>Always

Die Übereinstimmungsbedingung „Always“ wendet einen Standardsatz von Features auf alle Anforderungen an.

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>AS Number 
Das AS Number-Netzwerk wird durch seine autonome Systemnummer (ASN) definiert. Es gibt eine Option, mit der angegeben werden kann, ob diese Bedingung erfüllt wird, wenn das Netzwerk eines Clients der angegebenen ASN „entspricht“ oder „nicht entspricht“.

Wichtige Informationen:
- Wenn Sie mehrere ASNs angeben möchten, trennen Sie die Nummern durch ein einzelnes Leerzeichen voneinander. Die Angabe „64514 64515“ beispielsweise führt zur Übereinstimmung mit Anforderungen von 64514 oder 64515.
- Bestimmte Anforderungen geben möglicherweise keine gültige ASN zurück. Ein Fragezeichen (?) kennzeichnet Anforderungen, für die keine gültige ASN ermittelt werden konnte.
- Sie müssen die gesamte ASN für das gewünschte Netzwerk angeben. Für Teilwerte gibt es keine Übereinstimmung.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cdn-origin"></a>CDN Origin
Die Übereinstimmungsbedingung „CDN Origin“ ist erfüllt, wenn beide folgenden Bedingungen erfüllt sind:
- Es wurde Inhalt aus dem Content Delivery Network-Speicher angefordert.
- Der Anforderungs-URI verwendet den Inhaltszugriffspunkt (z.B. /000001), der in dieser Übereinstimmungsbedingung definiert ist.
  - Content Delivery Network-URL: Der Anforderungs-URI muss den ausgewählten Inhaltszugriffspunkt enthalten.
  - Edge-CNAME-URL: Die entsprechende Edge-CNAME-Konfiguration muss auf den ausgewählten Inhaltszugriffspunkt zeigen.
  
Wichtige Informationen:
 - Der Inhaltszugriffspunkt bestimmt den Dienst, der den angeforderten Inhalt bereitstellen soll.
 - Verwenden Sie keine AND IF-Anweisung, um bestimmte Übereinstimmungsbedingungen zu kombinieren. Durch Kombination einer CDN Origin-Übereinstimmungsbedingung mit einer Customer Origin-Übereinstimmungsbedingung würde beispielsweise ein Übereinstimmungsmuster entstehen, für das nie eine Übereinstimmung gefunden werden kann. Aus diesem Grund können zwei CDN Origin-Übereinstimmungsbedingungen nicht mit einer AND IF-Anweisung kombiniert werden.

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>Client IP Address
Es gibt eine Option, mit der angegeben werden kann, ob die Client IP Address-Bedingung erfüllt wird, wenn die angegebene IP-Adresse eines Clients den angegebenen IP-Adressen „entspricht“ oder „nicht entspricht“.

Wichtige Informationen:
- Stellen Sie sicher, dass Sie die CIDR-Notation verwenden.
- Wenn Sie mehrere IP-Adressen und/oder IP-Adressblöcke angeben, trennen Sie diese mit einem einzelnen Leerzeichen.
  - **IPv4-Beispiel**: Die Angabe „1.2.3.4 10.20.30.40“ führt zur Übereinstimmung mit Anforderungen von den Adressen 1.2.3.4 oder 10.20.30.40.
  - **IPv6-Beispiel**: Die Angabe „1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80“ führt zur Übereinstimmung mit Anforderungen von den Adressen 1:2:3:4:5:6:7:8 oder 10:20:30:40:50:60:70:80.
- Die Syntax für einen IP-Adressblock besteht aus der IP-Basisadresse, gefolgt von einem Schrägstrich und der Präfixgröße.
  - **IPv4-Beispiel**: Die Angabe „5.5.5.64/26“ führt zur Übereinstimmung mit Anforderungen von den Adressen 5.5.5.64 bis 5.5.5.127.
  - **IPv6-Beispiel**: Die Angabe „1:2:3:/48“ führt zur Übereinstimmung mit Anforderungen von den Adressen 1:2:3:0:0:0:0:0 bis 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter"></a>Cookie Parameter
Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Cookie Parameter“ erfüllt wird.
- **Entspricht**: Erfordert, dass eine Anforderung das angegebene Cookie mit einem Wert enthält, der mindestens einem der in dieser Übereinstimmungsbedingung definierten Werte entspricht.
- **Entspricht nicht**: Erfordert, dass die Anforderung eins der folgenden Kriterien erfüllt:
  - Die Anforderung enthält das angegebene Cookie nicht.
  - Die Anforderung enthält das angegebene Cookie, aber der Wert entspricht keinem der Werte, die in dieser Übereinstimmungsbedingung definiert sind.
  
Wichtige Informationen:
- Cookiename: 
  - Sonderzeichen, einschließlich Sternchen, werden bei der Angabe eines Cookienamens nicht unterstützt. Dies bedeutet, dass beim Vergleich nur exakte Namensübereinstimmungen berücksichtigt werden.
  - Für jede Instanz dieser Übereinstimmungsbedingung kann nur ein einziger Cookiename angegeben werden.
  - Beim Vergleichen von Cookienamen wird die Groß-/Kleinschreibung nicht beachtet.
- Cookiewert: 
  - Wenn Sie mehrere Cookiewerte angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.
  - In Cookiewerten können Sonderzeichen verwendet werden. 
  - Wenn kein Platzhalterzeichen angegeben wurde, erfüllt nur eine exakte Entsprechung diese Übereinstimmungsbedingung. Durch die Angabe von „Wert“ wird eine Übereinstimmung mit „Wert“ erzielt, aber nicht mit „Wert1“ oder „Wert2“.
  - Die Option **Groß-/Kleinschreibung ignorieren** bestimmt, ob beim Vergleich des Cookiewerts einer Anforderung die Groß-/Kleinschreibung beachtet wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter-regex"></a>Cookie Parameter Regex
Die Übereinstimmungsbedingung „Cookie Parameter Regex“ definiert einen Cookienamen und einen Cookiewert. Sie können reguläre Ausdrücke verwenden, um den gewünschten Cookiewert zu definieren. 

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen diese Übereinstimmungsbedingung erfüllt wird.
- **Entspricht**: Erfordert, dass Anforderungen das angegebene Cookie mit einem Wert enthalten, der dem angegebenen regulären Ausdruck entspricht.
- **Entspricht nicht**: Erfordert, dass die Anforderung eins der folgenden Kriterien erfüllt:
  - Die Anforderung enthält das angegebene Cookie nicht.
  - Die Anforderung enthält das angegebene Cookie, aber der Wert entspricht nicht dem angegebenen regulären Ausdruck.
  
Wichtige Informationen:
- Cookiename: 
  - Reguläre Ausdrücke und Sonderzeichen, einschließlich Sternchen, werden bei der Angabe eines Cookienamens nicht unterstützt. Dies bedeutet, dass beim Vergleich nur exakte Namensübereinstimmungen berücksichtigt werden.
  - Für jede Instanz dieser Übereinstimmungsbedingung kann nur ein einziger Cookiename angegeben werden.
  - Beim Vergleichen von Cookienamen wird die Groß-/Kleinschreibung nicht beachtet.
- Cookiewert: 
  - In Cookiewerten können reguläre Ausdrücke verwendet werden.
  - Die Option **Groß-/Kleinschreibung ignorieren** bestimmt, ob beim Vergleich des Cookiewerts einer Anforderung die Groß-/Kleinschreibung beachtet wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

--- 
### <a name="country"></a>Country
Sie können ein Land anhand seines Ländercodes angeben. Es gibt eine Option, mit der angegeben werden kann, ob diese Bedingung erfüllt wird, wenn das Land, aus dem eine Anforderung stammt, den angegebenen Werten „entspricht“ oder „nicht entspricht“.

Wichtige Informationen:
- Wenn Sie mehrere Ländercodes angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.
- Bei der Angabe von Ländercodes werden keine Platzhalterzeichen unterstützt.
- Die Ländercodes „EU“ und „AP“ umfassen nicht alle IP-Adressen in diesen Regionen.
- Bestimmte Anforderungen geben möglicherweise keinen gültigen Ländercode zurück. Ein Fragezeichen (?) kennzeichnet Anforderungen, für die kein gültiger Ländercode ermittelt werden konnte.
- Bei Ländercodes wird die Groß-/Kleinschreibung berücksichtigt.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Customer Origin

Wichtige Informationen: 
- Die Übereinstimmungsbedingung „Customer Origin“ wird erfüllt, unabhängig davon, ob Inhalt über eine Content Delivery Network-URL oder eine Edge-CNAME-URL angefordert wird, die auf den ausgewählten Kundenursprung zeigt.
- Eine Kundenursprungskonfiguration, auf die von einer Regel verwiesen wird, kann von der Seite „Customer Origin“ nicht gelöscht werden. Bevor Sie versuchen, eine Kundenursprungskonfiguration zu löschen, stellen Sie sicher, dass die folgenden Konfigurationen nicht darauf verweisen:
  - Eine Customer Origin-Übereinstimmungsbedingung
  - Eine Edge-CNAME-Konfiguration
- Verwenden Sie keine AND IF-Anweisung, um bestimmte Übereinstimmungsbedingungen zu kombinieren. Durch Kombination einer Customer Origin-Übereinstimmungsbedingung mit einer CDN Origin-Übereinstimmungsbedingung würde beispielsweise ein Übereinstimmungsmuster entstehen, für das nie eine Übereinstimmung gefunden werden kann. Aus diesem Grund können zwei Customer Origin-Übereinstimmungsbedingungen nicht mit einer AND IF-Anweisung kombiniert werden.

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Gerät

Die Übereinstimmungsbedingung „Device“ bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften. Die Mobilgeräterkennung wird mithilfe von [WURFL](http://wurfl.sourceforge.net/) durchgeführt. Die folgende Tabelle führt WURFL-Funktionen und die zugehörigen Variablen für die Content Delivery Network-Regel-Engine auf.
<br>
> [!NOTE] 
> Die folgenden Variablen werden für die Features **Modify Client Request Header** und **Modify Client Response Header** unterstützt.

Funktion | Variable | BESCHREIBUNG | Beispielwerte
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | Eine Zeichenfolge, die den Markennamen des Geräts angibt. | Samsung
Device OS | %{wurfl_cap_device_os} | Eine Zeichenfolge, die das auf dem Gerät installierte Betriebssystem angibt. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | Eine Zeichenfolge, die die Versionsnummer des auf dem Gerät installierten Betriebssystems angibt. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | Ein boolescher Wert, der angibt, ob das Gerät die duale Ausrichtung unterstützt. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | Eine Zeichenfolge, die die bevorzugte Dokumenttypdefinition (DTD) des mobilen Geräts für den HTML-Inhalt angibt. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | Ein boolescher Wert, der angibt, ob das Gerät Base64-codierte Images unterstützt. | false
Is Android | %{wurfl_vcap_is_android} | Ein boolescher Wert, der angibt, ob für das Gerät das Android-Betriebssystem verwendet wird. | true
Is IOS | %{wurfl_vcap_is_ios} | Ein boolescher Wert, der angibt, ob für das Gerät das iOS-Betriebssystem verwendet wird. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | Ein boolescher Wert, der angibt, ob es sich um ein Smart TV-Gerät handelt. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | Ein boolescher Wert, der angibt, ob es sich beim Gerät um ein Smartphone handelt. | true
Is Tablet | %{wurfl_cap_is_tablet} | Ein boolescher Wert, der angibt, ob es sich beim Gerät um ein Tablet handelt. Dies ist eine vom Betriebssystem unabhängige Beschreibung. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | Ein boolescher Wert, der angibt, ob das Gerät als drahtloses Gerät betrachtet wird. | true
Marketing Name | %{wurfl_cap_marketing_name} | Eine Zeichenfolge, die den Marketingnamen des Geräts angibt. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | Eine Zeichenfolge, die den Browser angibt, der zum Anfordern des Inhalts vom Gerät verwendet wird. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | Eine Zeichenfolge, die die Version des Browsers angibt, der zum Anfordern des Inhalts vom Gerät verwendet wird. | 31
Model Name | %{wurfl_cap_model_name} | Eine Zeichenfolge, die den Modellnamen des Geräts angibt. | s3
Progressive Download | %{wurfl_cap_progressive_download} | Ein boolescher Wert, der angibt, ob das Gerät die Wiedergabe von Audio- und Videodaten während des Herunterladens unterstützt. | true
Release Date | %{wurfl_cap_release_date} | Eine Zeichenfolge, die das Jahr und den Monat angibt, in dem das Gerät der WURFL-Datenbank hinzugefügt wurde.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | Eine ganze Zahl, die für das Gerät die Höhe in Pixel angibt. | 768
Resolution Width | %{wurfl_cap_resolution_width} | Eine ganze Zahl, die für das Gerät die Breite in Pixel angibt. | 1024

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Edge Cname
Wichtige Informationen: 
- Die Liste verfügbarer Edge-CNAMEs ist auf die Namen beschränkt, die auf der Edge-CNAME-Seite für die Plattform konfiguriert wurden, auf der die HTTP-Regel-Engine konfiguriert wird.
- Bevor Sie versuchen, eine Edge-CNAME-Konfiguration zu löschen, stellen Sie sicher, dass keine Edge Cname-Übereinstimmungsbedingung darauf verweist. Edge CNAME-Konfigurationen, die in einer Regel definiert wurden, können nicht von der Edge-CNAME-Seite gelöscht werden. 
- Verwenden Sie keine AND IF-Anweisung, um bestimmte Übereinstimmungsbedingungen zu kombinieren. Durch Kombination einer Edge Cname-Übereinstimmungsbedingung mit einer Customer Origin-Übereinstimmungsbedingung würde beispielsweise ein Übereinstimmungsmuster entstehen, für das nie eine Übereinstimmung gefunden werden kann. Aus diesem Grund können zwei Edge Cname-Übereinstimmungsbedingungen nicht mit einer AND IF-Anweisung kombiniert werden.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="referring-domain"></a>Referring Domain
Der Hostname des verweisenden Elements, über das der Inhalt angefordert wurde, bestimmt, ob die Referring Domain-Bedingung erfüllt wird. Es gibt eine Option, mit der angegeben werden kann, ob diese Bedingung erfüllt wird, wenn der Name des verweisenden Hosts den angegebenen Werten „entspricht“ oder „nicht entspricht“.

Wichtige Informationen:
- Wenn Sie mehrere Hostnamen angeben möchten, trennen Sie die Namen durch ein einzelnes Leerzeichen voneinander.
- Diese Übereinstimmungsbedingung unterstützt Sonderzeichen.
- Wenn der angegebene Wert kein Sternchen enthält, muss er exakt mit dem Namen des verweisenden Hosts übereinstimmen. Bei der Angabe von „mydomain.com“ würde beispielsweise keine Übereinstimmung mit „www.mydomain.com“ erzielt.
- Die Option **Groß-/Kleinschreibung ignorieren** bestimmt, ob beim Vergleich die Groß-/Kleinschreibung beachtet wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-literal"></a>Request Header Literal
Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen diese Übereinstimmungsbedingung erfüllt wird.
- **Entspricht**: Erfordert, dass die Anforderung den angegebenen Header enthält. Der Wert der Anforderung muss dem Wert entsprechen, der in dieser Übereinstimmungsbedingung definiert ist.
- **Entspricht nicht**: Erfordert, dass die Anforderung eins der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Header nicht.
  - Die Anforderung enthält den angegebenen Header, aber der Wert entspricht nicht dem Wert, der in dieser Übereinstimmungsbedingung definiert ist.
  
Wichtige Informationen:
- Beim Vergleichen von Headernamen wird die Groß-/Kleinschreibung nie beachtet. Die Option **Groß-/Kleinschreibung ignorieren** bestimmt, ob bei Vergleichen von Headerwerten die Groß- und Kleinschreibung beachtet wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-regex"></a>Request Header Regex
Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Request Header Regex“ erfüllt wird.
- **Entspricht**: Erfordert, dass die Anforderung den angegebenen Header enthält. Der Wert der Anforderung muss dem Muster entsprechen, das im angegebenen regulären Ausdruck definiert ist.
- **Entspricht nicht**: Erfordert, dass die Anforderung eins der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Header nicht.
  - Die Anforderung enthält den angegebenen Header, aber der Wert entspricht nicht dem angegebenen regulären Ausdruck.

Wichtige Informationen:
- Headername: 
  - Beim Vergleichen von Headernamen wird die Groß-/Kleinschreibung nicht beachtet.
  - Leerzeichen im Headernamen sollten durch „%20“ ersetzt werden. 
- Headerwert: 
  - In Headerwerten können reguläre Ausdrücke verwendet werden.
  - Die Option **Groß-/Kleinschreibung ignorieren** bestimmt, ob bei Vergleichen von Headerwerten die Groß- und Kleinschreibung beachtet wird.
  - Diese Bedingung wird nur erfüllt, wenn der Headerwert mindestens einem der angegebenen Muster exakt entspricht.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale 

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-header-wildcard"></a>Request Header Wildcard
Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Request Header Wildcard“ erfüllt wird.
- **Entspricht**: Erfordert, dass die Anforderung den angegebenen Header enthält. Der Wert der Anforderung muss mindestens einem der Werte entsprechen, die in dieser Übereinstimmungsbedingung definiert sind.
- **Entspricht nicht**: Erfordert, dass die Anforderung eins der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Header nicht.
  - Die Anforderung enthält den angegebenen Header, aber der Wert entspricht keinem der angegebenen Werte.
  
Wichtige Informationen:
- Headername: 
  - Beim Vergleichen von Headernamen wird die Groß-/Kleinschreibung nicht beachtet.
  - Leerzeichen im Headernamen sollten durch „%20“ ersetzt werden. Sie können diesen Wert auch verwenden, um Leerzeichen in einem Headerwert anzugeben.
- Headerwert: 
  - In Headerwerten können Sonderzeichen verwendet werden.
  - Die Option **Groß-/Kleinschreibung ignorieren** bestimmt, ob bei Vergleichen von Headerwerten die Groß- und Kleinschreibung beachtet wird.
  - Diese Bedingung wird nur erfüllt, wenn der Headerwert mindestens einem der angegebenen Muster exakt entspricht.
  - Wenn Sie mehrere Werte angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-method"></a>Request Method
Nur Ressourcen, die mit der ausgewählten Anforderungsmethode angefordert wurden, erfüllen die Bedingung „Request Method“. Folgende Anforderungsmethoden sind verfügbar:
- GET
- HEAD 
- POST 
- OPTIONS 
- PUT 
- DELETE 
- TRACE 
- CONNECT 

Wichtige Informationen:
- Standardmäßig kann nur die GET-Anforderungsmethode zwischengespeicherten Inhalt im Netzwerk generieren. Alle anderen Anforderungsmethoden werden per Proxy durch das Netzwerk gesendet.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-scheme"></a>Request Scheme
Nur Ressourcen, die mit dem ausgewählten Protokoll angefordert wurden, erfüllen die Bedingung „Request Scheme“. Verfügbar sind die Protokolle HTTP und HTTPS.

Wichtige Informationen:
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Azure Content Delivery Network](cdn-overview.md)
* [Regel-Engine – Referenz](cdn-rules-engine-reference.md)
* [Regel-Engine – bedingte Ausdrücke](cdn-rules-engine-reference-conditional-expressions.md)
* [Regel-Engine – Features](cdn-rules-engine-reference-features.md)
* [Überschreiben des HTTP-Standardverhaltens mithilfe der Regel-Engine](cdn-rules-engine.md)

