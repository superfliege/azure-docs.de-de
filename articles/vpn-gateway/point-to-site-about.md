---
title: Informationen zu Azure-Point-to-Site-VPN-Verbindungen | Microsoft-Dokumentation
description: "Dieser Artikel enthält Informationen zu P2S-Verbindungen (Point-to-Site) und erleichtert Ihnen die Entscheidung, welcher P2S-VPN-Gateway-Authentifizierungstyp verwendet werden sollte."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: adb998754615150f1e74cba01ffc2fc9e53827ed
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---
# <a name="about-point-to-site-vpn"></a>Informationen zu Point-to-Site-VPN

Mit einer P2S-VPN-Gatewayverbindung (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Diese Lösung ist nützlich für Telearbeiter, die an einem Remotestandort (beispielsweise zu Hause oder in einer Konferenz) eine Verbindung mit Azure-VNETs herstellen möchten. Wenn nur einige wenige Clients eine Verbindung mit einem VNET herstellen müssen, ist ein P2S-VPN (und nicht ein S2S-VPN) ebenfalls eine nützliche Lösung.

## <a name="protocol"></a>Welches Protokoll verwendet P2S?

P2S-VPN kann eins der folgenden Protokolle verwenden:

* Secure Socket Tunneling-Protokoll (SSTP), ein proprietäres SSL-basiertes VPN-Protokoll. Eine SSL-VPN-Lösung kann Firewalls durchdringen, da die meisten Firewalls den von SSL verwendeten TCP-Port 443 öffnen. SSTP wird nur auf Windows-Geräten unterstützt. Azure unterstützt alle Versionen von Windows, die über SSTP verfügen (Windows 7 und höher).

* IKEv2-VPN, eine standardbasierte IPsec-VPN-Lösung. IKEv2-VPN kann zum Herstellen einer Verbindung von Mac-Geräten (OSX-Version 10.11 und höher) verwendet werden. Azure unterstützt IKEv2-VPN unter Windows nicht. 

Konfigurieren Sie in einer gemischten Clientumgebung mit Windows- und Mac-Geräten sowohl SSTP als auch IKEv2.

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

![Point-to-Site]](./media/point-to-site-about/p2s.png "Point-to-Site")

### <a name="configuration-requirements-for-client-devices"></a>Konfigurationsanforderungen für Clientgeräte

Benutzer verwenden die nativen VPN-Clients auf Windows- und Mac-Geräten für P2S. Azure bietet eine ZIP-Datei mit der VPN-Clientkonfiguration, die Einstellungen enthält, die diese nativen Clients zum Herstellen einer Verbindung mit Azure benötigen.

  * Bei Windows-Geräten besteht die VPN-Clientkonfiguration aus einem Installer-Paket, das Benutzer auf ihren Geräten installieren.
  * Bei Mac-Geräten besteht sie aus der Datei „mobileconfig“, die Benutzer auf ihren Geräten installieren.

Die ZIP-Datei enthält zudem die Werte einiger wichtiger Einstellungen in Azure, mit denen Sie ein eigenes Profil für diese Geräte erstellen können. Zu den Werten zählen die VPN-Gatewayadresse, konfigurierte Tunneltypen, Routen und das Stammzertifikat für die Gatewayüberprüfung.

### <a name="which-gateway-skus-support-p2s-vpn"></a>Welche Gateway-SKUs unterstützen P2S-VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* Der Benchmark für den aggregierten Durchsatz basiert auf Messungen von mehreren, durch ein Gateway aggregierten Tunneln. Aufgrund der Bedingungen für den Internetdatenverkehr und dem Verhalten Ihrer Anwendung kann der Durchsatz nicht garantiert werden.
* Informationen zu den Preisen finden Sie auf der Seite mit den Preisen. 
* Informationen zum SLA (Vereinbarung zum Servicelevel) finden Sie auf der SLA-Seite.

## <a name="faqcert"></a>P2S – Häufig gestellte Fragen zur Azure-Zertifikatauthentifizierung

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>P2S – Häufig gestellte zur RADIUS-Authentifizierung

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von P2S-Verbindungen – RADIUS-Authentifizierung](point-to-site-how-to-radius-ps.md)

[Konfigurieren von P2S-Verbindungen – native Azure-Zertifikatauthentifizierung](vpn-gateway-howto-point-to-site-rm-ps.md)
