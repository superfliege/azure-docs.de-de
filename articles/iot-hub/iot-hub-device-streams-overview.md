---
title: Azure IoT Hub-Gerätestreams (Vorschau) | Microsoft-Dokumentation
description: Übersicht über IoT Hub-Gerätestreams
author: rezasherafat
manager: briz
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 7ffe4a087ae94d6c96019cc045d3d7ff071780d4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830006"
---
# <a name="iot-hub-device-streams-preview"></a>IoT Hub-Gerätestreams (Vorschau)

## <a name="overview"></a>Übersicht
Azure IoT Hub-*Gerätestreams* vereinfachen das Erstellen sicherer bidirektionaler TCP-Tunnel für eine Vielzahl von Szenarios mit Kommunikation zwischen Cloud und Gerät. Ein Gerätestream wird durch einen IoT Hub-*Streamingendpunkt* vermittelt, der als Proxy zwischen Ihrem Gerät und den Dienstendpunkten fungiert. Dieses Setup wird im folgenden Diagramm dargestellt. Es eignet sich besonders, wenn die Geräte sich hinter einer Netzwerkfirewall oder in einem privaten Netzwerk befinden. So erfüllen IoT Hub-Gerätestreams die Kundenanforderung, IoT-Geräte firewallfreundlich zu erreichen, ohne Eingangs- und Ausgangsports der Netzwerkfirewall weit zu öffnen.

![Alternativtext](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png "Übersicht über IoT Hub-Gerätestreams")


Mithilfe von IoT Hub-Gerätestreams bleiben Geräte geschützt und müssen nur TCP-Ausgangsverbindungen für den IoT Hub-Streamingendpunkt über Port 443 öffnen. Nachdem ein Stream eingerichtet wurde, besitzen die dienstseitigen und geräteseitige Anwendungen jeweils programmgesteuerten Zugriff auf ein WebSocket-Clientobjekt, um sich gegenseitig Rohbytes zu senden bzw. zu empfangen. Die durch diesen Tunnel bereitgestellte Zuverlässigkeit und garantierte Reihenfolge ist gleichwertig mit TCP.

## <a name="benefits"></a>Vorteile
IoT Hub-Gerätestreams bieten folgende Vorteile:
- **Firewallfreundliche, sichere Konnektivität:** IoT-Geräte können von Dienstendpunkten aus erreicht werden, ohne auf dem Gerät oder im Netzwerkumkreis einen Eingangsfirewallport zu öffnen (nur Ausgangskonnektivität zu IoT Hub über Port 443 ist erforderlich).

- **Authentifizierung:** Sowohl die Geräte- als auch die Dienstseite des Tunnels müssen sich bei IoT Hub mit den entsprechenden Anmeldeinformationen authentifizieren.

- **Verschlüsselung:** Standardmäßig verwenden IoT Hub-Gerätestreams TLS-aktivierte Verbindungen. Dadurch wird sichergestellt, dass der Datenverkehr immer verschlüsselt ist, unabhängig davon, ob die Anwendung Verschlüsselung verwendet.

- **Einfache Konnektivität:** Durch die Verwendung von Gerätestreams ist zum Herstellen von Konnektivität mit IoT-Geräten kein komplexes Setup virtueller privater Netzwerke mehr erforderlich.

- **Kompatibilität mit TCP/IP-Stapel:** IoT Hub-Gerätestreams können TCP/IP-Anwendungsdatenverkehr verarbeiten. Dies bedeutet, dass dieses Feature von einer Vielzahl proprietärer sowie auf Standards basierender Protokolle genutzt werden kann.

- **Einfache Verwendung in privaten Netzwerksetups:** Der Dienst kann ein Gerät durch einen Verweis auf die dazugehörige Geräte-ID statt über die IP-Adresse erreichen. Dies ist hilfreich in Situationen, in denen sich ein Gerät in einem privaten Netzwerk befindet und über eine private IP-Adresse verfügt oder die IP-Adresse dynamisch zugewiesen wird und der Dienstseite unbekannt ist.

## <a name="device-stream-workflows"></a>Gerätestreamworkflows
Ein Gerätestream wird initiiert, wenn der Dienst eine Verbindung mit einem Gerät durch Bereitstellung der Geräte-ID anfordert. Dieser Workflow passt besonders in das Client-/Server-Kommunikationsmuster, einschließlich SSH und RDP, in dem ein Benutzer über ein SSH- oder RDP-Clientprogramm eine Remoteverbindung mit dem SSH- oder RDP-Server herstellen möchte, der auf dem Gerät ausgeführt wird.

Der Erstellungsvorgang für Gerätestreams umfasst eine Aushandlung zwischen dem Gerät, dem Dienst, dem IoT Hub-Hauptendpunkt und den Streamingendpunkten. Während der IoT Hub-Hauptendpunkt die Erstellung eines Gerätestreams orchestriert, verarbeitet der Streamingendpunkt den Datenverkehr zwischen dem Dienst und dem Gerät.

### <a name="device-stream-creation-flow"></a>Ablauf zur Gerätestreamerstellung
Die programmgesteuerte Erstellung eines Gerätestreams mit dem SDK umfasst die folgenden Schritte, die auch in der folgenden Abbildung dargestellt werden:

![Alternativtext](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png "Handshakeprozess des Gerätestreams")


1. Die Geräteanwendung registriert einen Rückruf, um im Voraus benachrichtigt zu werden, wenn ein neuer Gerätestream zum Gerät initiiert wird. Dieser Schritt findet in der Regel statt, wenn das Gerät gestartet wird und eine Verbindung mit der IoT Hub-Instanz herstellt.

2. Das dienstseitige Programm initiiert bei Bedarf einen Gerätestream durch Bereitstellen der Geräte-ID (_nicht_ der IP-Adresse).

3. IoT Hub benachrichtigt das dienstseitige Programm durch Aufrufen des in Schritt 1 registrierten Rückrufs. Das Gerät kann die Anforderung der Streaminitiierung annehmen oder ablehnen. Diese Logik kann speziell auf Ihr Anwendungsszenario zugeschnitten sein. Wenn die Streamanforderung vom Gerät abgelehnt wird, informiert IoT Hub den Dienst entsprechend. Andernfalls folgen die unten aufgeführten Schritte.

4. Das Gerät erstellt über Port 443 eine sichere ausgehende TCP-Verbindung mit dem Streamingendpunkt und führt ein Upgrade der Verbindung auf WebSocket durch. Die URL des Streamingendpunkts sowie die zum Authentifizieren verwendeten Anmeldeinformationen werden dem Gerät von IoT Hub im Rahmen der Anforderung bereitgestellt, die in Schritt 3 gesendet wurde.

5. Der Dienst wird über das Ergebnis der Annahme des Streams durch das Gerät benachrichtigt und erstellt dann einen eigenen WebSocket für den Streamingendpunkt. Auf ähnliche Weise empfängt er die Streamingendpunkt-URL und die Authentifizierungsinformationen von IoT Hub.

Beim obigen Handshakevorgang:
- Der Handshakevorgang muss innerhalb von 60 Sekunden abgeschlossen werden (Schritt 2 bis 5). Andernfalls tritt ein Timeoutfehler auf, und der Dienst wird entsprechend benachrichtigt.

- Nach Abschluss des obigen Ablaufs zur Streamerstellung fungiert der Streamingendpunkt als Proxy und überträgt Datenverkehr zwischen dem Dienst und dem Gerät über die jeweiligen WebSockets.

- Gerät und Dienst benötigen ausgehende Konnektivität über Port 443 mit dem wichtigsten IoT Hub-Endpunkt sowie dem Streamingendpunkt. Die URL dieser Endpunkte ist auf der Registerkarte „Übersicht“ im IoT Hub-Portal verfügbar.

- Die Zuverlässigkeit und garantierte Reihenfolge eines eingerichteten Streams ist gleichwertig mit TCP.

- Alle Verbindungen mit IoT Hub und dem Streamingendpunkt verwenden TLS und sind verschlüsselt.

### <a name="termination-flow"></a>Beendigungsablauf
Ein eingerichteter Stream wird beendet, wenn eine der TCP-Verbindungen mit dem Gateway (durch den Dienst oder das Gerät) getrennt wird. Dies kann durch Schließen des WebSockets im Geräte- oder Dienstprogramm geplant stattfinden oder unbeabsichtigt aufgrund eines Timeouts bei der Netzwerkkonnektivität oder aufgrund eines Prozessfehlers. Bei Beendigung einer der Verbindungen (Gerät oder Dienst) mit dem Streamingendpunkt wird auch die andere TCP-Verbindung zwangsweise beendet, und Dienst und Gerät sind dafür zuständig, den Stream bei Bedarf neu zu erstellen.


## <a name="connectivity-requirements"></a>Konnektivitätsanforderungen

Sowohl die Geräte- als auch die Dienstseite eines Gerätestreams muss dazu in der Lage sein, TLS-fähige Verbindungen mit IoT Hub und dem zugehörigen Streamingendpunkt herzustellen. Hierfür ist eine ausgehende Konnektivität über Port 443 mit diesen Endpunkten erforderlich. Den diesen Endpunkten zugeordneten Hostnamen finden Sie auf der IoT Hub-Registerkarte *Übersicht*, wie in der folgenden Abbildung gezeigt: ![Alternativtext](./media/iot-hub-device-streams-overview/device-stream-portal.png "Gerätestreamingendpunkte")

Alternativ dazu können die Endpunktinformationen über die Azure CLI unter dem Abschnitt mit dem Hubeigenschaften abgerufen werden, insbesondere über die Schlüssel `property.hostname` und `property.deviceStreams`.

```azurecli-interactive
az iot hub show --name <YourIoTHubName>
```

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Problembehandlung über Aktivitätsprotokolle für Gerätestreams

Sie können Azure Log Analytics zum Erfassen des Aktivitätsprotokolls für Gerätestreams in Ihrer IoT Hub-Instanz einrichten. Dies kann zur Problembehandlung sehr hilfreich sein.

Führen Sie die folgenden Schritte aus, um Azure Log Analytics für Ihre IoT Hub-Gerätestreamaktivitäten zu konfigurieren:

1. Navigieren Sie in Ihrer IoT Hub-Instanz zur Registerkarte *Diagnoseeinstellungen*, und klicken Sie auf den Link *Diagnose aktivieren*.

  ![Alternativtext](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings.PNG "Aktivieren von Diagnoseprotokollen")


2. Geben Sie einen Namen für Ihre Diagnoseeinstellungen ein, und wählen Sie die Option *An Log Analytics senden* aus. Sie werden durch die Auswahl einer vorhandenen Log Analytics-Ressource oder durch die Erstellung einer neuen Ressource geführt. Aktivieren Sie außerdem den Eintrag *DeviceStreams* in der Liste.

    ![Alternativtext](./media/iot-hub-device-streams-overview/device-streams-diagnostics.PNG "Gerätestreamprotokolle aktivieren")

3. Sie können jetzt im IoT Hub-Verwaltungsportal auf der Registerkarte *Protokolle* auf Ihre Gerätestreamprotokolle zugreifen. Gerätestream-Aktivitätsprotokolle werden in der Tabelle `AzureDiagnostics` aufgeführt mit `Category=DeviceStreams`.

    <p>
Wie unten dargestellt, werden auch die Identität des Zielgeräts und das Ergebnis des Vorgangs in den Protokollen angegeben.
    ![Alternativtext](./media/iot-hub-device-streams-overview/device-streams-log-analytics.PNG "Zugriff auf Gerätestreamprotokolle")
    

## <a name="whitelist-device-streaming-endpoints"></a>Aufnehmen der Gerätestreamingendpunkte in die Whitelist

Wie [zuvor](#Overview) bereits erwähnt, erstellt Ihr Gerät während des Initiierungsprozesses für Gerätestreams eine ausgehende Verbindung mit dem IoT Hub-Streamingendpunkt. Die Firewalls auf dem Gerät oder im Netzwerk müssen ausgehende Konnektivität mit dem Streaminggateway über Port 443 zulassen. (Hierbei handelt es sich um eine WebSocket-Verbindung, die über TLS verschlüsselt ist.)

Den Hostnamen des Gerätestreamingendpunkts finden Sie im Azure IoT Hub-Portal auf der Registerkarte „Übersicht“. ![Alternativtext](./media/iot-hub-device-streams-overview/device-stream-portal.PNG "Gerätestreamingendpunkte")

Alternativ finden Sie diese Informationen mithilfe der Azure CLI:
```cmd/sh
az iot hub show --name tcpstreaming-preview
```

## <a name="sdk-availability"></a>SDK-Verfügbarkeit
Zwei Seiten jedes Streams (auf der Geräte- und der Dienstseite) verwenden das IoT Hub SDK zum Einrichten des Tunnels. Während der öffentlichen Vorschau können Kunden aus den folgenden SDK-Sprachen auswählen:
- Die C und C# SDKs unterstützen Gerätestreams auf der Geräteseite.

- Die NodeJS und C# SDKs unterstützen Gerätestreams auf der Dienstseite.

## <a name="iot-hub-device-stream-samples"></a>Beispiele für IoT Hub-Gerätstreams
Wir haben zwei Beispiele aufgeführt, um die Verwendung von Gerätestreams durch Anwendungen zu veranschaulichen. Das Beispiel *Echo* zeigt die programmgesteuerte Verwendung von Gerätestreams (durch Aufrufen der SDK-APIs). Das Beispiel *Lokaler Proxy* veranschaulicht die Verwendung der SDK-Funktionen zum Erstellen eines Tunnels für Standard-Anwendungsdatenverkehr (z.B. SSH, RDP oder Web) über Gerätestreams.

### <a name="echo-sample"></a>Echobeispiel
Das Echobeispiel veranschaulicht die programmgesteuerte Verwendung von Gerätestreams zum Senden und Empfangen von Bytes zwischen Dienst- und Geräteanwendung. Verwenden Sie die folgenden Links, um auf die Schnellstarthandbücher zuzugreifen. (Sie können Dienst- und Geräteprogramme in verschiedenen Sprachen verwenden. Beispielsweise funktioniert ein C-Geräteprogramm mit einem C#-Dienstprogramm.)

| SDK    | Dienstprogramm                                          | Geräteprogramm                                           |
|--------|----------------------------------------------------------|----------------------------------------------------------|
| C#     | [Link](quickstart-device-streams-echo-csharp.md) | [Link](quickstart-device-streams-echo-csharp.md) |
| NodeJS | [Link](quickstart-device-streams-echo-nodejs.md) | -                                                        |
| C      | -                                                        | [Link](quickstart-device-streams-echo-c.md)      |

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Beispiel mit lokalem Proxy (für SSH oder RDP)
Das Beispiel mit lokalem Proxy zeigt eine Möglichkeit zum Aktivieren eines Tunnels für den Datenverkehr einer vorhandenen Anwendung, der die Kommunikation zwischen einem Client- und einem Serverprogramm umfasst. Dieses Setup funktioniert für Client-/Serverprotokolle wie SSH und RDP, in denen die Dienstseite als Client fungiert (und SSH- oder RDP-Clientprogramme ausführt) und die Geräteseite als Server fungiert (und SSH-Daemon- oder RDP-Serverprogramme ausführt). 

Dieser Abschnitt beschreibt die Verwendung von Gerätestreams, um SSH-Szenarien für ein Gerät über Gerätestreams zu ermöglichen. (Die Situation ist für RDP oder andere Client-/Server-Protokolle ähnlich, dort wird der entsprechende Port des Protokolls verwendet.)

Das Setup nutzt zwei Programme für *lokale Proxys*, die in der folgenden Abbildung dargestellt sind: *Lokaler Geräteproxy* und *Lokaler Dienstproxy*. Die lokalen Proxys sind für die Durchführung des [Handshakes zur Gerätestreaminitiierung](#Device-stream-creation-flow) mit IoT Hub sowie für die Interaktion mit dem SSH-Client und dem SSH-Daemon über reguläre Client-/Server-Socketprogrammierung zuständig.

![Alternativtext](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png "Gerätestream-Proxysetup für SSH/RDP")

1. Der Benutzer führt den lokalen Dienstproxy aus, um einen Gerätestream an das Gerät zu initiieren.

2. Das Gerät akzeptiert die Streaminitiierung, und der Tunnel wird mit dem Streamingendpunkt des IoT Hub hergestellt (wie oben beschrieben).

3. Der lokale Geräteproxy stellt eine Verbindung mit dem SSH-Daemonendpunkt her, der an Port 22 auf dem Gerät lauscht.

4. Der lokale Dienstproxy lauscht an einem bestimmten Port und wartet auf neue SSH-Verbindungen vom Benutzer. (Port 2222 im Beispiel steht für einen beliebigen Port.) Der Benutzer verweist den SSH-Client an den Port des lokalen Dienstproxys auf „localhost“.

### <a name="notes"></a>Notizen
- Durch die oben genannten Schritte wird ein End-to-End-Tunnel zwischen dem SSH-Client (auf der rechten Seite) und dem SSH-Daemon (auf der linken Seite) eingerichtet. 

- Die Pfeile in der Abbildung zeigen die Richtung, in der Verbindungen zwischen Endpunkten hergestellt werden. Beachten Sie insbesondere, dass keine Verbindungen beim Gerät eingehen. (Dies wird häufig durch eine Firewall blockiert.)

- Die Auswahl von Port `2222` auf dem lokalen Dienstproxy ist eine willkürliche Auswahl. Der Proxy kann so konfiguriert werden, dass ein beliebiger anderer verfügbarer Port verwendet wird.

- Die Auswahl von Port `22` ist protokollabhängig und in diesem Fall spezifisch für SSH. Im Fall von RDP muss Port `3389` verwendet werden. Dies kann in den bereitgestellten Beispielprogrammen konfiguriert werden.

Verwenden Sie die folgenden Links, um Anweisungen zum Ausführen der lokalen Proxyprogramme in der Sprache Ihrer Wahl zu erhalten. Wie im Echobeispiel können Sie den lokalen Geräteproxy und den lokalen Dienstproxy in verschiedenen Sprachen ausführen, da sie vollständig kompatibel sind.

| SDK    | Lokaler Dienstproxy                                       | Lokaler Geräteproxy                                |
|--------|-----------------------------------------------------------|---------------------------------------------------|
| C#     | [Link](quickstart-device-streams-proxy-csharp.md)  | [Link](quickstart-device-streams-proxy-csharp.md) |
| NodeJS | [Link](quickstart-device-streams-proxy-nodejs.md)         | -                                                 |
| C      | -                                                         | [Link](quickstart-device-streams-proxy-c.md)      |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [Show zu Gerätestreams in IoT (Channel 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
> [Schnellstartanleitung zum Ausprobieren von Gerätestreams](/azure/iot-hub)
