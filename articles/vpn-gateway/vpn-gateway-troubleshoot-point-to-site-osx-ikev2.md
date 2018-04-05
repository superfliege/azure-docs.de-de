---
title: Problembehandlung bei Point-to-Site-Azure-VPN-Verbindungen von Mac OS X-Clients | Microsoft-Dokumentation
description: Schritte zur Problembehandlung bei P2S Mac OS X-VPN-Clientverbindungen
services: vpn-gateway
documentationcenter: na
author: anzaman
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: 1cf8195cbf65f27c71a4db18c0c61c8a25673acd
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Problembehandlung bei Point-to-Site-VPN-Verbindungen von Mac OS X-VPN-Clients

Dieser Artikel hilft Ihnen beim Beheben von Point-to-Site-Konnektivitätsproblemen von Mac OS X unter Verwendung des nativen VPN-Clients und IKEv2. Der VPN-Client in Mac für IKEv2 ist sehr einfach und bietet wenig Anpassungsspielraum. Es müssen nur vier Einstellungen überprüft werden:

* Serveradresse
* Remote-ID
* Lokale ID
* Authentifizierungseinstellungen
* Betriebssystemversion (10.11 oder höher)


## <a name="VPNClient"></a> Problembehandlung bei zertifikatbasierter Authentifizierung
1. Überprüfen Sie die VPN-Clienteinstellungen. Wechseln Sie durch Drücken von BEFEHLTASTE + UMSCHALTTASTE ZU **Network Setting (Netzwerkeinstellung)**, und geben Sie dann „VPN“ ein, um die VPN-Clienteinstellungen zu überprüfen. Klicken Sie in der Liste auf den VPN-Eintrag, der untersucht werden muss.

  ![IKEv2-zertifikatbasierte Authentifizierung](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Überprüfen Sie, ob die **Server Address (Serveradresse)** der vollständige FQDN ist und die „cloudapp.net“ enthält.
3. Die **Remote ID** sollte mit der „Server Address“ (Serveradresse) (Gateway-FQDN) des Servers identisch sein.
4. Die **Local ID (lokale ID)** muss mit dem **Antragsteller** des Clientzertifikats identisch sein.
5. Klicken Sie auf **Authentication Settings (Authentifizierungseinstellungen)** um die Seite „Authentication Settings“ zu öffnen.

  ![Authentifizierungseinstellungen](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Überprüfen Sie, ob **Certificate (Zertifikat)** in der Dropdownliste ausgewählt ist.
7. Klicken Sie auf die Schaltfläche **Select (Auswählen)**, und stellen Sie sicher, dass das richtige Zertifikat ausgewählt ist. Klicken Sie zum Speichern etwaiger Änderungen auf **OK**.

## <a name="ikev2"></a> Problembehandlung bei Benutzernamen- und Kennwortauthentifizierung

1. Überprüfen Sie die VPN-Clienteinstellungen. Wechseln Sie durch Drücken von BEFEHLTASTE + UMSCHALTTASTE ZU **Network Setting (Netzwerkeinstellung)**, und geben Sie dann „VPN“ ein, um die VPN-Clienteinstellungen zu überprüfen. Klicken Sie in der Liste auf den VPN-Eintrag, der untersucht werden muss.

  ![IKEv2-Benutzername/-Kennwort](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Überprüfen Sie, ob die **Server Address (Serveradresse)** der vollständige FQDN ist und die „cloudapp.net“ enthält.
3. Die **Remote ID** sollte mit der „Server Address“ (Serveradresse) (Gateway-FQDN) des Servers identisch sein.
4. Die **Local ID (lokale ID)** kann leer sein.
5. Klicken Sie auf die Schaltfläche **Authentication Setting (Authentifizierungseinstellung)**, und stellen Sie sicher, dass „Username“ (Benutzername) in der Dropdownliste ausgewählt ist.

  ![Authentifizierungseinstellungen](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.jpg)
6. Stellen Sie sicher, dass die richtigen Anmeldeinformationen eingegeben sind.

## <a name="additional"></a>Zusätzliche Schritte

Wenn Sie die vorherigen Schritte versucht haben, und alles ordnungsgemäß konfiguriert ist, laden Sie [Wireshark](https://www.wireshark.org/#download) herunter, und führen Sie eine Paketerfassung durch.

1. Filtern Sie nach *iskmp*, und achten Sie auf die **IKE_SA**-Pakete. Sie sollten die SA-Vorschlagdetails unter **Payload: Security Association (Nutzlast: Sicherheitszuordnung)** sehen. 
2. Stellen Sie sicher, dass Client und Server über einen gemeinsamen Satz verfügen.

  ![Paket](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, wenden Sie sich an den [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
