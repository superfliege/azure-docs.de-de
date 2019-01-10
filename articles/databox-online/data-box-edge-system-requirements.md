---
title: Systemanforderungen für Microsoft Azure Data Box Edge | Microsoft-Dokumentation
description: Erfahren Sie etwas über die Software- und Netzwerkanforderungen für Ihr Azure Data Box Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 4d6060c1774b2ad09c3b813bc107b73309dae801
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020216"
---
# <a name="azure-data-box-edge-system-requirements-preview"></a>Systemanforderungen für Azure Data Box Edge (Vorschauversion)

In diesem Artikel werden wichtige Systemanforderungen für Ihre Microsoft Azure Data Box Edge-Lösung und die Clients beschrieben, die mit Azure Data Box Edge verbunden sind. Es wird empfohlen, dass Sie die Informationen sorgfältig überprüfen, bevor Sie Data Box Edge bereitstellen. Sie können diese Informationen ggf. während der Bereitstellung und des nachfolgenden Betriebs als Referenz nutzen.

Die Systemanforderungen von Data Box Edge:

- **Softwareanforderungen für Hosts:** beschreibt die unterstützten Plattformen, Browser für die lokale Konfigurationsbenutzeroberfläche, SMB-Clients und alle weiteren Anforderungen an Clients, die auf das Gerät zugreifen.
- **Netzwerkanforderungen für das Gerät:** enthält Informationen zu den Netzwerkanforderungen für den Betrieb des physischen Geräts.

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen.

## <a name="supported-os-for-clients-connected-to-device"></a>Unterstütztes Betriebssystem für Clients, die mit dem Gerät verbunden sind

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Unterstützte Protokolle für Clients, die auf das Gerät zugreifen

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Unterstützte Speichertypen

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Unterstützte Browser für die lokale Webbenutzeroberfläche

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="port-configuration-for-data-box-edge"></a>Portkonfiguration für Data Box Edge

In der folgenden Tabelle sind die Ports aufgeführt, die in der Firewall für SMB-, Cloud- oder Verwaltungsdatenverkehr geöffnet werden müssen. In dieser Tabelle bezieht sich *ein* oder *eingehend* auf die Richtung, aus der eingehende Clientanforderungen auf das Gerät zugreifen. Entsprechend bezieht sich *aus* oder *ausgehend* auf die Richtung, in der das Data Box Edge-Gerät Daten über die Bereitstellung hinaus an externe Ziele sendet: z.B. ausgehende Verbindungen mit dem Internet.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="port-configuration-for-iot-edge"></a>Portkonfiguration für IoT Edge

Azure IoT Edge ermöglicht die ausgehende Kommunikation von einem lokalen Edgegerät mit der Azure-Cloud über unterstützte IoT Hub-Protokolle. Die eingehende Kommunikation ist nur für bestimmte Szenarien erforderlich, in denen Azure IoT Hub Nachrichten an das Azure IoT Edge-Gerät (z.B. C2D-Nachrichten) pushen muss.

Die folgende Tabelle enthält die Portkonfiguration für Server, die die Azure IoT Edge-Runtime hosten:

| Port-Nr. | ein oder aus | Portbereich | Erforderlich | Anleitungen |
|----------|-----------|------------|----------|----------|
| TCP 5671 (AMQP)| aus       | WAN        | JA      | Standardkommunikationsprotokoll für IoT Edge. Muss offen sein, wenn Azure IoT Edge nicht für andere unterstützte Protokolle konfiguriert oder AMQP das gewünschte Kommunikationsprotokoll ist. <br>5672 für AMQP wird von IoT Edge nicht unterstützt. <br>Blockieren Sie diesen Port, wenn Azure IoT Edge ein anderes von IoT Hub unterstütztes Protokoll verwendet. |
| TCP 443 (HTTPS)| aus       | WAN        | JA      | Ausgehend offen für die IoT Edge-Bereitstellung. Wenn Sie über ein transparentes Gateway mit Blattknotengeräten verfügen, die ggf. Methodenanforderungen senden. In diesem Fall muss der Port 443 nicht für externe Netzwerke geöffnet werden, um eine Verbindung mit IoT Hub herzustellen oder IoT Hub-Dienste über Azure IoT Edge bereitzustellen. Die eingehende Regel kann somit darauf beschränkt werden, nur eingehende Verbindungen aus dem internen Netzwerk zu öffnen. |
| TCP 5671 (AMQP) | Geben Sie in        |            | Nein        | Eingehende Verbindungen sollten blockiert werden.|
| TCP 443 (HTTPS) | Geben Sie in        |            | In einigen Fällen, siehe Kommentare | Die eingehende Verbindung sollte nur für bestimmte Szenarien geöffnet werden. Falls keine HTTP-fremden Protokolle wie AMQP oder MQTT konfiguriert werden können, können die Nachrichten über WebSockets an Port 443 gesendet werden. |

Ausführliche Informationen finden Sie unter [Firewall- und Portkonfigurationsregeln für die IoT Edge-Bereitstellung](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>URL-Muster für Firewallregeln

Netzwerkadministratoren können häufig erweiterte, auf den URL-Mustern basierende Firewallregeln konfigurieren, die zum Filtern des eingehenden und ausgehenden Verkehrs verwendet werden. Ihr Data Box Edge-Gerät und -Dienst hängen von anderen Microsoft-Anwendungen wie Azure Service Bus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern ab. Die URL-Muster, die diesen Anwendungen zugeordnet sind, können verwendet werden, um Firewallregeln zu konfigurieren. Es ist wichtig, zu verstehen, dass sich diese den Anwendungen zugeordneten URL-Muster ändern können. Diese Änderungen erfordern, dass der Netzwerkadministrator die Firewallregeln für Ihre Data Box Edge-Instanz ggf. überwachen und aktualisieren muss.

Es empfiehlt sich, die Firewallregeln für den ausgehenden Verkehr basierend auf den festen IP-Adressen für Data Box Edge in den meisten Fällen recht locker festzulegen. Sie können jedoch die folgenden Informationen verwenden, um erweiterte Firewallregeln festzulegen, die erforderlich sind, um sichere Umgebungen zu erstellen.

> [!NOTE]
> - Die Geräte-IPs (Quell-IPs) sollten immer für alle cloudaktivierten Netzwerkschnittstellen eingerichtet sein.
> - Die Ziel-IPs sollten auf die [IP-Bereiche des Azure-Datencenters](https://www.microsoft.com/download/confirmation.aspx?id=41653) festgelegt werden.

|    URL-Muster                                                                                                                                                                                                                                                                                                                                                                                                                                                      |    Komponente oder Funktion                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://\*.databoxedge.azure.com/\*<br>https://\*.servicebus.windows.net/\*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                                                                           |    Azure Data Box Edge-Dienst<br>Azure-Servicebus<br>Authentifizierungsdienst                           |
|    http://\*.backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                                |    Geräteaktivierung                                                                                    |
|    http://crl.microsoft.com/pki/\*<br>http://www.microsoft.com/pki/\*                                                                                                                                                                                                                                                                                                                                                                                                  |    Zertifikatswiderruf                                                                               |
|    https://\*.core.windows.net/\*<br>https://\*.data.microsoft.com<br>http://\*.msftncsi.com                                                                                                                                                                                                                                                                                                                                                                            |    Azure-Speicherkonten und Überwachung                                                                |
|    http://windowsupdate.microsoft.com<br>http://\*.windowsupdate.microsoft.com<br>https://\*.windowsupdate.microsoft.com<br>http://\*.update.microsoft.com<br>https://\*.update.microsoft.com<br>http://\*.windowsupdate.com<br>http://download.microsoft.com<br>http://\*.download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://\*.ws.microsoft.com<br>https://\*.ws.microsoft.com<br>http://\*.mp.microsoft.com |    Microsoft Update-Server                                                                             |
|    http://\*.deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                                          |    Akamai CDN                                                                                           |
|    https://\*.partners.extranet.microsoft.com/\*                                                                                                                                                                                                                                                                                                                                                                                                                    |    Supportpaket                                                                                      |
|    http://\*.data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                                     |    Telemetriedienst in Windows. Informationen finden Sie im „Update für Kundenzufriedenheit und Diagnosetelemetrie“.      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                                         |

## <a name="internet-bandwidth"></a>Internetbandbreite

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Nächster Schritt

* [Bereitstellen von Azure Data Box Edge](data-box-Edge-deploy-prep.md)
