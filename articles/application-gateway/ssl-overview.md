---
title: Aktivieren von End-to-End-SSL in Azure Application Gateway
description: Dieser Artikel enthält eine Übersicht über die Unterstützung von End-to-End-SSL in Application Gateway.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 8/6/2018
ms.author: amsriva
ms.openlocfilehash: 4575bed18697a5661d58dc350c24a9497f7c46ff
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578812"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Übersicht über End-to-End-SSL mit Application Gateway

Application Gateway unterstützt die SSL-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. Mit diesem Feature können Webserver vom kostspieligen Verschlüsselungs- und Entschlüsselungsaufwand befreit werden. Für einige Kunden ist die unverschlüsselte Kommunikation mit den Back-End-Servern jedoch keine akzeptable Option. Der Grund für diese unverschlüsselte Kommunikation können Sicherheits- oder Konformitätsanforderungen sein, oder die Anwendung akzeptiert unter Umständen nur eine sichere Verbindung. Für Anwendungen dieser Art unterstützt Application Gateway die End-to-End-SSL-Verschlüsselung.

End-to-End-SSL ermöglicht die sichere, verschlüsselte Übertragung sensibler Daten an das Back-End und nutzt gleichzeitig die Vorteile der von Application Gateway bereitgestellten Lastenausgleichsfeatures (Schicht 7). Zu diesen Features zählen unter anderem cookiebasierte Sitzungsaffinität, URL-basiertes Routing, Unterstützung von standortbasiertem Routing und die Möglichkeit zum Einfügen von X-Forwarded-*-Headern.

Bei der Konfiguration mit dem End-to-End-SSL-Kommunikationsmodus beendet Application Gateway die SSL-Sitzungen am Gateway und entschlüsselt den Benutzerdatenverkehr. Anschließend werden die konfigurierten Regeln angewendet, um eine geeignete Instanz des Back-End-Pools auszuwählen, an die der Datenverkehr geroutet werden soll. Application Gateway initiiert anschließend eine neue SSL-Verbindung mit dem Back-End-Server und verschlüsselt die Daten mit dem öffentlichen Schlüssel des Back-End-Serverzertifikats erneut, bevor die Anforderung an das Back-End übertragen wird. End-to-End-SSL wird aktiviert, indem Sie die Protokolleinstellung in **BackendHTTPSetting** auf „HTTPS“ festlegen. Diese Einstellung wird anschließend auf einen Back-End-Pool angewendet. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-SSL muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

![End-to-End-SSL-Szenario][1]

In diesem Beispiel werden Anforderungen, für die TLS 1.2 verwendet wird, an Back-End-Server in Pool1 geleitet, indem End-to-End-SSL genutzt wird.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-End-SSL und Whitelists für Zertifikate

Application Gateway kommuniziert nur mit bekannten Back-End-Instanzen, deren Zertifikat in der Whitelist des Anwendungsgateways enthalten ist. Um die Whitelistfunktion für Zertifikate zu aktivieren, müssen Sie den öffentlichen Schlüssel der Back-End-Serverzertifikate auf das Anwendungsgateway hochladen (nicht das Stammzertifikat). Es sind dann nur Verbindungen mit bekannten und in Whitelists enthaltenen Back-Ends zulässig. Die übrigen Back-Ends führen zu einem Gatewayfehler. Selbstsignierte Zertifikate dienen nur zu Testzwecken und werden für Produktionsworkloads nicht empfohlen. Solche Zertifikate müssen, wie unter den obigen Schritten beschrieben, in der Whitelist des Anwendungsgateways enthalten sein, damit sie verwendet werden können.

> [!NOTE]
> Die Einrichtung des Authentifizierungszertifikats ist für vertrauenswürdige Azure-Dienste, z.B. Azure-Web-Apps, nicht erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über End-to-End-SSL informiert haben, wechseln Sie zu [Konfigurieren eines Anwendungsgateways mit SSL-Beendigung mithilfe des Azure-Portals](create-ssl-portal.md), um ein Anwendungsgateway mithilfe von End-to-End-SSL zu erstellen.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
