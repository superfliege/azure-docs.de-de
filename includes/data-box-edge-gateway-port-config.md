---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 67de9042af11a2b17c4b65f8225ecd0580b95466
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263475"
---
| Port-Nr.| ein oder aus | Portbereich| Erforderlich|   Notizen |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|aus|WAN |Nein |Der ausgehende Port wird für den Internetzugriff zum Abrufen von Updates verwendet. <br>Der ausgehende Webproxy kann vom Benutzer konfiguriert werden. |
| TCP 443 (HTTPS)|aus|WAN|JA|Der ausgehende Port wird für den Zugriff auf Daten in der Cloud verwendet.<br>Der ausgehende Webproxy kann vom Benutzer konfiguriert werden.|   
| UDP 53 (DNS)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen internetbasierten DNS-Server verwenden.<br>Es wird empfohlen, den lokalen DNS-Server zu verwenden. |
| UDP 123 (NTP)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen internetbasierten NTP-Server verwenden.  |
| UDP 67 (DHCP)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen DHCP-Server verwenden.  |
| TCP 80 (HTTP)|Geben Sie in|LAN|JA|Dies ist der eingehende Port für die lokale Benutzeroberfläche auf dem Gerät für die lokale Verwaltung. <br>Beim Zugriff auf die lokale Benutzeroberfläche über HTTP erfolgt automatisch eine Umleitung auf HTTPS.  |
| TCP 443 (HTTPS)|Geben Sie in|LAN|JA|Dies ist der eingehende Port für die lokale Benutzeroberfläche auf dem Gerät für die lokale Verwaltung. |