---
title: Informationen zu Azure-Point-to-Site-VPN-Verbindungen | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zu P2S-Verbindungen (Point-to-Site) und erleichtert Ihnen die Entscheidung, welcher P2S-VPN-Gateway-Authentifizierungstyp verwendet werden sollte.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: cherylmc
ms.openlocfilehash: bf84ec16d5d13439796b386a8ab4f40840ca4eaa
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438410"
---
# <a name="about-point-to-site-vpn"></a>Informationen zu Point-to-Site-VPN

Mit einer P2S-VPN-Gatewayverbindung (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Diese Lösung ist nützlich für Telearbeiter, die an einem Remotestandort (beispielsweise zu Hause oder in einer Konferenz) eine Verbindung mit Azure-VNETs herstellen möchten. Wenn nur einige wenige Clients eine Verbindung mit einem VNET herstellen müssen, ist ein P2S-VPN (und nicht ein S2S-VPN) ebenfalls eine nützliche Lösung. Dieser Artikel gilt für das Resource Manager-Bereitstellungsmodell.

## <a name="protocol"></a>Welches Protokoll verwendet P2S?

P2S-VPN kann eins der folgenden Protokolle verwenden:

* OpenVPN, ein auf SSL/TLS basierendes VPN-Protokoll. Eine SSL-VPN-Lösung kann Firewalls durchdringen, da die meisten Firewalls den von SSL verwendeten TCP-Port 443 öffnen. OpenVPN kann zum Herstellen einer Verbindung von Android-, iOS-, Linux- und Mac-Geräten (OSX-Version 10.11 und höher) verwendet werden.

* Secure Socket Tunneling-Protokoll (SSTP), ein proprietäres SSL-basiertes VPN-Protokoll. Eine SSL-VPN-Lösung kann Firewalls durchdringen, da die meisten Firewalls den von SSL verwendeten TCP-Port 443 öffnen. SSTP wird nur auf Windows-Geräten unterstützt. Azure unterstützt alle Versionen von Windows, die über SSTP verfügen (Windows 7 und höher).

* IKEv2-VPN, eine standardbasierte IPsec-VPN-Lösung. IKEv2-VPN kann zum Herstellen einer Verbindung von Mac-Geräten (OSX-Version 10.11 und höher) verwendet werden.


>[!NOTE]
>IKEv2 und OpenVPN für P2S sind ausschließlich für das Resource Manager-Bereitstellungsmodell verfügbar. Für das klassische Bereitstellungsmodell sind sie nicht verfügbar.
>

## <a name="authentication"></a>Wie werden P2S-VPN-Clients authentifiziert?

Bevor Azure eine P2S-VPN-Verbindung akzeptiert, muss zunächst der Benutzer authentifiziert werden. Azure bietet zwei Mechanismen zum Authentifizieren eines Benutzers, der eine Verbindung herstellt.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Authentifizieren mit der nativen Azure-Zertifikatauthentifizierung

Bei Verwendung der nativen Azure-Zertifikatauthentifizierung wird der Benutzer, der eine Verbindung herstellt, mit einem auf dem Gerät vorhandenen Clientzertifikat authentifiziert. Clientzertifikate werden über ein vertrauenswürdiges Stammzertifikat generiert und dann auf jedem Clientcomputer installiert. Sie können ein Stammzertifikat verwenden, das mit einer Unternehmenslösung generiert wurde, oder ein selbstsigniertes Zertifikat generieren.

Die Überprüfung des Clientzertifikats wird vom VPN-Gateway durchgeführt und erfolgt während der Herstellung der P2S-VPN-Verbindung. Das Stammzertifikat ist für die Überprüfung erforderlich und muss in Azure hochgeladen werden.

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Authentifizieren mit Active Directory-Domänenserver (AD)

Die AD-Domänenauthentifizierung ermöglicht Benutzern das Herstellen einer Verbindung mit Azure mit ihren Anmeldeinformationen für die Organisationsdomäne. Dafür ist ein RADIUS-Server erforderlich, der in den AD-Server integriert wird. Organisationen können auch ihre vorhandene RADIUS-Bereitstellung nutzen.   
  
Der RADIUS-Server kann lokal oder im Azure-VNET bereitgestellt werden. Während der Authentifizierung fungiert das Azure-VPN-Gateway als Vermittler und leitet Authentifizierungsnachrichten zwischen dem RADIUS-Server und dem Gerät weiter, das eine Verbindung herstellt. Die Erreichbarkeit des Gateways durch den RADIUS-Server ist daher wichtig. Wenn sich der RADIUS-Server in der lokalen Umgebung befindet, ist aus Gründen der Erreichbarkeit eine VPN-S2S-Verbindung zwischen Azure und dem lokalen Standort erforderlich.  
  
Der RADIUS-Server kann auch in AD-Zertifikatdienste integriert werden. Dadurch können Sie den RADIUS-Server und Ihre Unternehmenszertifikatbereitstellung für die P2S-Zertifikatauthentifizierung als Alternative zur Azure-Zertifikatauthentifizierung verwenden. Der Vorteil besteht darin, dass Sie keine Stammzertifikate und gesperrten Zertifikate in Azure hochladen müssen.

Ein RADIUS-Server kann auch in andere externe Identitätssysteme integriert werden. Dadurch ergeben sich zahlreiche Authentifizierungsoptionen für P2S-VPNs, einschließlich Optionen für die mehrstufige Authentifizierung.

>[!NOTE]
>Das OpenVPN-Protokoll wird mit der RADIUS-Authentifizierung nicht unterstützt.
>

![Point-to-Site](./media/point-to-site-about/p2s.png "Point-to-Site")

## <a name="what-are-the-client-configuration-requirements"></a>Welche Anforderungen an die Clientkonfiguration müssen erfüllt sein?

>[!NOTE]
>Bei Windows-Clients müssen Sie über Administratorrechte auf dem Clientgerät verfügen, um die VPN-Verbindung von dem Clientgerät zu Azure zu initiieren.
>

Benutzer verwenden die nativen VPN-Clients auf Windows- und Mac-Geräten für P2S. Azure bietet eine ZIP-Datei mit der VPN-Clientkonfiguration, die Einstellungen enthält, die diese nativen Clients zum Herstellen einer Verbindung mit Azure benötigen.

* Bei Windows-Geräten besteht die VPN-Clientkonfiguration aus einem Installer-Paket, das Benutzer auf ihren Geräten installieren.
* Bei Mac-Geräten besteht sie aus der Datei „mobileconfig“, die Benutzer auf ihren Geräten installieren.

Die ZIP-Datei enthält zudem die Werte einiger wichtiger Einstellungen in Azure, mit denen Sie ein eigenes Profil für diese Geräte erstellen können. Zu den Werten zählen die VPN-Gatewayadresse, konfigurierte Tunneltypen, Routen und das Stammzertifikat für die Gatewayüberprüfung.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="gwsku"></a>Welche Gateway-SKUs unterstützen P2S-VPN?

[!INCLUDE [aggregate throughput sku](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

* Gateway-SKU-Empfehlungen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

>[!NOTE]
>Die Basic-SKU unterstützt keine IKEv2- oder RADIUS-Authentifizierung.
>

## <a name="configure"></a>Wie konfiguriere ich eine P2S-Verbindung?

Eine P2S-Konfiguration erfordert einige bestimmte Schritte. Die folgenden Artikel enthalten die Schritte, anhand derer Sie eine P2S-Konfiguration durchführen können, und Links zur Konfiguration der VPN-Clientgeräte:

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der RADIUS-Authentifizierung](point-to-site-how-to-radius-ps.md)

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der nativen Azure-Zertifikatauthentifizierung](vpn-gateway-howto-point-to-site-rm-ps.md)

* [Konfigurieren von OpenVPN](vpn-gateway-howto-openvpn.md)

## <a name="faqcert"></a>Häufig gestellte Fragen zur nativen Azure-Zertifikatauthentifizierung

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>Häufig gestellte Fragen zur RADIUS-Authentifizierung

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der RADIUS-Authentifizierung](point-to-site-how-to-radius-ps.md)

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der nativen Azure-Zertifikatauthentifizierung](vpn-gateway-howto-point-to-site-rm-ps.md)
