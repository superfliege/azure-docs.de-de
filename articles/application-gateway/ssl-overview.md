---
title: Aktivieren von End-to-End-SSL in Azure Application Gateway
description: Dieser Artikel enthält eine Übersicht über die Unterstützung von End-to-End-SSL in Application Gateway.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2018
ms.author: amsriva
ms.openlocfilehash: e7020ef5c1f7411c7226e7a2db489112ee6bf0a4
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945500"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Übersicht über End-to-End-SSL mit Applicaiton Gateway

Application Gateway unterstützt die SSL-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. Mit diesem Feature können Webserver vom kostspieligen Verschlüsselungs- und Entschlüsselungsaufwand befreit werden. Für einige Kunden ist die unverschlüsselte Kommunikation mit den Back-End-Servern jedoch keine akzeptable Option. Der Grund für diese unverschlüsselte Kommunikation können Sicherheits- oder Konformitätsanforderungen sein, oder die Anwendung akzeptiert unter Umständen nur eine sichere Verbindung. Für Anwendungen dieser Art unterstützt Application Gateway die End-to-End-SSL-Verschlüsselung.

End-to-End-SSL ermöglicht die sichere, verschlüsselte Übertragung sensibler Daten an das Back-End und nutzt gleichzeitig die Vorteile der von Application Gateway bereitgestellten Lastenausgleichsfeatures (Schicht 7). Zu diesen Features zählen unter anderem cookiebasierte Sitzungsaffinität, URL-basiertes Routing, Unterstützung von standortbasiertem Routing und die Möglichkeit zum Einfügen von X-Forwarded-*-Headern.

Bei der Konfiguration mit dem End-to-End-SSL-Kommunikationsmodus beendet Application Gateway die SSL-Sitzungen am Gateway und entschlüsselt den Benutzerdatenverkehr. Anschließend werden die konfigurierten Regeln angewendet, um eine geeignete Instanz des Back-End-Pools auszuwählen, an die der Datenverkehr geroutet werden soll. Application Gateway initiiert anschließend eine neue SSL-Verbindung mit dem Back-End-Server und verschlüsselt die Daten mit dem öffentlichen Schlüssel des Back-End-Serverzertifikats erneut, bevor die Anforderung an das Back-End übertragen wird. End-to-End-SSL wird aktiviert, indem Sie die Protokolleinstellung in **BackendHTTPSetting** auf „HTTPS“ festlegen. Diese Einstellung wird anschließend auf einen Back-End-Pool angewendet. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-SSL muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

![End-to-End-SSL-Szenario][1]

In diesem Beispiel werden Anforderungen, für die TLS 1.2 verwendet wird, an Back-End-Server in Pool1 geleitet, indem End-to-End-SSL genutzt wird.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-End-SSL und Whitelists für Zertifikate

Application Gateway kommuniziert nur mit bekannten Back-End-Instanzen, deren Zertifikat in der Whitelist des Anwendungsgateways enthalten ist. Um die Whitelistfunktion für Zertifikate zu aktivieren, müssen Sie den öffentlichen Schlüssel der Back-End-Serverzertifikate auf das Anwendungsgateway hochladen (nicht das Stammzertifikat). Es sind dann nur Verbindungen mit bekannten und in Whitelists enthaltenen Back-Ends zulässig. Die übrigen Back-Ends führen zu einem Gatewayfehler. Selbstsignierte Zertifikate dienen nur zu Testzwecken und werden für Produktionsworkloads nicht empfohlen. Solche Zertifikate müssen, wie unter den obigen Schritten beschrieben, in der Whitelist des Anwendungsgateways enthalten sein, damit sie verwendet werden können.

> [!NOTE]
> Die Einrichtung des Authentifizierungszertifikats ist für vertrauenswürdige Azure-Dienste, z.B. Azure-Web-Apps, nicht erforderlich.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>End-to-End-SSL mit der v2-SKU

Authentifizierungszertifikate wurden als veraltet markiert und durch Trusted Root-Zertifikate in der v2-SKU von Application Gateway ersetzt. Sie funktionieren auf ähnliche Weise wie Authentifizierungszertifikate mit einigen wichtigen Unterschieden:

- Zertifikate, die von bekannten Zertifizierungsstellen, deren CN dem Hostnamen in den HTTP-Einstellungen des Back-Ends entspricht, zertifiziert wurden, erfordern keinen zusätzlichen Schritt, damit End-to-End-SSL funktioniert. 

   Wenn beispielsweise die Back-End-Zertifikate von einer bekannten Zertifizierungsstelle ausgestellt wurden, der CN „contoso.com“ lautet und in der HTTP-Einstellung des Back-Ends das Feld „Host“ ebenfalls auf „contoso.com“ festgelegt wurde, sind keine weiteren Schritte erforderlich. Sie können das Protokoll in der HTTP-Einstellung des Back-Ends auf HTTPS festlegen. In diesem Fall ist SSL sowohl für den Integritätstest als auch für den Datenpfad aktiviert. Wenn Sie Azure-Web-Apps oder andere Azure-Webdienste als Back-End verwenden, gelten diese ebenfalls als implizit vertrauenswürdig, und für End-to-End-SSL sind keine weiteren Schritte erforderlich.
- Wenn das Zertifikat selbstsigniert ist oder von einem unbekannten Vermittler signiert wurde, müssen Sie zum Aktivieren von End-to-End-SSL in der v2-SKU ein vertrauenswürdiges Stammzertifikat definieren. Application Gateway kommuniziert nur mit Back-Ends, bei denen das Stammzertifikat des Serverzertifikats mit einem der vertrauenswürdigen Stammzertifikate in der Liste der HTTP-Einstellung für das Back-End, das dem Pool zugeordnet ist, übereinstimmt.
- Zusätzlich zur Übereinstimmung des Stammzertifikats überprüft Application Gateway auch, ob die Einstellung „Host“ in der HTTP-Einstellung des Back-Ends mit dem allgemeinen Namen (Common Name, CN), der vom SSL-Zertifikat des Back-End-Servers angegeben wird, übereinstimmt. Beim Herstellen einer SSL-Verbindung mit dem Back-End legt Application Gateway die SNI-Erweiterung (Server Name Indication, Servernamensanzeige) auf den Host fest, der in der HTTP-Einstellung des Back-Ends angegeben wurde.
- Wenn das **Auswählen des Hostnamen aus der Back-End-Adresse** anstelle des Felds „Host“ in der HTTP-Einstellung des Back-Ends ausgewählt wurde, wird der SNI-Header immer auf den FQDN des Back-End-Pools festgelegt, und der CN im SSL-Zertifikat des Back-End-Servers muss mit diesem FQDN übereinstimmen. Mitglieder des Back-End-Pools mit IP-Adressen werden in diesem Szenario nicht unterstützt.
- Das Stammzertifikat ist ein Base64-codiertes Stammzertifikat aus den Zertifikaten des Back-End-Servers.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über End-to-End-SSL informiert haben, fahren Sie mit dem [Konfigurieren von End-to-End-SSL mit Application Gateway und PowerShell](application-gateway-end-to-end-ssl-powershell.md) fort, um ein Anwendungsgateway mit End-to-End-SSL zu erstellen.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
