---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0dcb591db5f487a103feccf92ffa577a1cbf8b23
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
1. Öffnen Sie die Seite Ihres Gateways für virtuelle Netzwerke. Dazu gibt es verschiedene Möglichkeiten. Sie können über **TestVNet1 > Überblick > Verbundene Geräte > VNet1GW** zum Gateway „VNet1GW“ navigieren.
2. Klicken Sie auf der Seite für „VNet1GW“ auf **Verbindungen**. Klicken Sie oben auf der Seite „Verbindungen“ auf **+Hinzufügen**, um die Seite **Verbindung hinzufügen** zu öffnen.

  ![Einrichten einer Standort-zu-Standort-Verbindung](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. Konfigurieren Sie auf der Seite **Verbindung hinzufügen** die Werte für Ihre Verbindung.

  - **Name:** Geben Sie einen Namen für die Verbindung ein.
  - **Verbindungstyp:** Wählen Sie **Standort-zu-Standort (IPsec)** aus.
  - **Gateway für virtuelle Netzwerke:** Der Wert ist festgelegt, da Sie von diesem Gateway aus die Verbindung herstellen.
  - **Lokales Netzwerkgateway:** Klicken Sie auf **Lokales Netzwerkgateway auswählen**, und wählen Sie das lokale Netzwerkgateway aus, das Sie verwenden möchten.
  - **Gemeinsam verwendeter Schlüssel:** Dieser Wert muss mit dem Wert übereinstimmen, den Sie für Ihr lokales VPN-Gerät verwenden. Im Beispiel wird „abc123“ verwendet. Sie können (und sollten) allerdings einen komplexeren Wert verwenden. Entscheidend ist Folgendes: Der Wert, den Sie hier angeben, muss dem Wert entsprechen, den Sie beim Konfigurieren Ihres VPN-Geräts angeben.
  - Die verbleibenden Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** wurden korrigiert.

4. Klicken Sie auf **OK** , um die Verbindung zu erstellen. Auf dem Bildschirm blinkt der Hinweis *Verbindung wird erstellt* .
5. Die Verbindung wird auf der Seite **Verbindungen** des Gateways für virtuelle Netzwerke angezeigt. Der Status wechselt von *Unbekannt* zu *Verbindung wird hergestellt* und dann zu *Erfolgreich*.