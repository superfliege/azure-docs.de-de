---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 97d33bfcc8251b10ba121b7fb013800904450563
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kann ich für Punkt-zu-Standort-Verbindungen meine eigene interne PKI-Stammzertifizierungsstelle verwenden?

Ja. Bisher konnten nur selbstsignierte Stammzertifikate verwendet werden. Sie können weiterhin 20 Stammzertifikate hochladen.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Mit welchen Tools kann ich Zertifikate erstellen?

Sie können Ihre Unternehmens-PKI-Lösung (interne PKI), Azure PowerShell, MakeCert und OpenSSL verwenden.

### <a name="certsettings"></a>Gibt es Anleitungen für Zertifikateinstellungen und -parameter?

* **Interne PKI/Unternehmens-PKI-Lösung:** Sehen Sie sich die Schritte zum [Generieren von Zertifikaten](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert) an.

* **Azure PowerShell:** Entsprechende Schritte finden Sie im Artikel zu [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md).

* **MakeCert:** Die erforderlichen Schritte finden Sie im Artikel zu [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md).

* **OpenSSL:** 

    * Stellen Sie beim Exportieren von Zertifikaten sicher, dass das Stammzertifikat in Base64 konvertiert wird.

    * Clientzertifikat:

      * Geben Sie beim Erstellen des privaten Schlüssels als Länge 4096 an.
      * Geben Sie beim Erstellen des Zertifikats für den *-extensions*-Parameter *usr_cert* an.