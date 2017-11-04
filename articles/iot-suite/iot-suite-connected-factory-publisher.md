---
title: Verwenden des mit Azure IoT Suite verbundenen werkseitigen OPC Publishers | Microsoft-Dokumentation
description: Erhalten Sie Informationen zum Erstellen und Bereitstellen der Referenzimplementierung des verbundenen werkseitigen OPC Publishers.
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: dobett
ms.openlocfilehash: fd823194f6e51600b9d4ca1daa053db837871fef
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="opc-publisher-for-azure-iot-edge"></a>OPC Publisher für Azure IoT Edge

In diesem Artikel wird die Verwendung der OPC Publisher-Referenzimplementierung beschrieben. In der Referenzimplementierung wird die Verwendung von Azure IoT Edge für die folgenden Zwecke veranschaulicht:

- Herstellen einer Verbindung mit vorhandenen OPC UA-Servern.
- Veröffentlichen von JSON-codierten Telemetriedaten von diesen Servern im OPC UA-*Pub/Sub*-Format mithilfe einer JSON-Nutzlast im Azure IoT Hub. Sie können ein beliebiges Transportprotokoll verwenden, das von Azure IoT Edge unterstützt wird.

Diese Referenzanwendung umfasst Folgendes:

- Einen OPC UA-*Client*, um eine Verbindung mit vorhandenen OPC UA-Servern in Ihrem Netzwerk herzustellen.
- Einen OPC UA-*Server*, der Port 62222 überwacht, über den Sie die veröffentlichten Inhalte verwalten können.

Die Anwendung wird mithilfe von .NET Core implementiert und kann auf Plattformen ausgeführt werden, die von .NET Core unterstützt werden.

Der Herausgeber implementiert eine Wiederholungslogik, wenn er Verbindungen mit Endpunkten herstellt. Der Herausgeber erwartet, dass Endpunkte innerhalb einer angegebenen Anzahl von Keep-Alive-Anforderungen antwortet. Diese Wiederholungslogik ermöglicht es dem Herausgeber entsprechende Bedingungen zu erkennen, z. B. einen Stromausfall für einen OPC UA-Server.

Für jedes eindeutige Veröffentlichungsintervall, das einen OPC UA-Server betrifft, wird ein separates Abonnement erstellt, über das alle Knoten mit diesem Veröffentlichungsintervall aktualisiert werden.

Der Herausgeber unterstützt die Batchverarbeitung von Daten, die an IoT Hub gesendet werden, um die Netzwerklast zu verringern. Ein Batch wird nur an IoT Hub gesendet, wenn die konfigurierte Paketgröße erreicht ist.

Diese Anwendung verwendet den OPC UA-Referenzstack der OPC Foundation und daher gelten entsprechende Einschränkungen hinsichtlich der Lizenzierung. Die OPC UA-Dokumentation und Lizenzierungsbedingungen finden Sie unter „http://opcfoundation.github.io/UA-.NETStandardLibrary/“.

Den Quellcode von OPC Publisher finden Sie im GitHub-Repository unter [OPC Publisher für Azure IoT Edge](https://github.com/Azure/iot-edge-opc-publisher).

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen der Anwendung benötigen Sie das [.NET Core SDK 1.1.](https://docs.microsoft.com/dotnet/core/sdk) für Ihr Betriebssystem.

## <a name="build-the-application"></a>Erstellen der Anwendung

### <a name="as-native-windows-application"></a>Als native Windows-Anwendung

Öffnen Sie das `OpcPublisher.sln`-Projekt mit Visual Studio 2017, und erstellen Sie die Projektmappe durch Drücken von F7.

### <a name="as-docker-container"></a>Als Docker-Container

Verwenden Sie die `Dockerfile.Windows`-Konfigurationsdatei, um die Anwendung als Windows Docker-Container zu erstellen.

Verwenden Sie die `Dockerfile`-Konfigurationsdatei, um die Anwendung als Linux Docker-Container zu erstellen.

Geben Sie auf dem Entwicklungscomputer im Stamm des Repositorys den folgenden Befehl in ein Konsolenfenster ein:

`docker build -f <docker-configfile-to-use> -t <your-container-name> .`

Die Option `-f` für `docker build` ist optional und standardmäßig wird die `Dockerfile`-Konfigurationsdatei verwendet.

Mit Docker können Sie die Erstellung direkt über ein Git-Repository starten. Sie können einen Linux Docker-Container mit dem folgenden Befehl erstellen:

`docker build -t <your-container-name> .https://github.com/Azure/iot-edge-opc-publisher`

## <a name="configure-the-opc-ua-nodes-to-publish"></a>Konfigurieren der zu veröffentlichenden OPC UA-Knoten

Erstellen Sie eine JSON-formatierte Konfigurationsdatei, um zu konfigurieren, welche OPC UA-Knoten ihre Werte für Azure IoT Hub veröffentlichen sollten. Der Standardname für diese Konfigurationsdatei ist `publishednodes.json`. Die Anwendung aktualisiert und speichert diese Konfigurationsdatei, wenn sie die OPC UA-Servermethoden **PublishNode** oder **UnpublishNode** verwendet.

Die Syntax der Konfigurationsdatei sieht wie folgt aus:

```json
[
    {
        // example for an EnpointUrl is: opc.tcp://win10iot:51210/UA/SampleServer
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "OpcNodes": [
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised publishing interval).
            {
                // The identifier specifies the NamespaceUri and the node identifier in XML notation as specified in Part 6 of the OPC UA specification in the XML Mapping section.
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258"
            },
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with an OPC publishing interval of 4 seconds.
            // Publisher will use for each dinstinct publishing interval (of nodes on the same EndpointUrl) a separate subscription. All nodes without a publishing interval setting,
            // will be on the same subscription and the OPC UA stack will publish with the lowest sampling interval of a node.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                "OpcPublishingInterval": 4000
            },
            // Publisher will request the server at EndpointUrl to sample the node with the given sampling interval of 1 second
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised interval).
            // If the OPC publishing interval is set to a lower value, Publisher will adjust the OPC publishing interval of the subscription to the OPC sampling interval value.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                // the OPC sampling interval to use for this node.
                "OpcSamplingInterval": 1000
            }
        ]
    },

    // the format below is only supported for backward compatibility. you need to ensure that the
    // OPC UA server on the configured EndpointUrl has the namespaceindex you expect with your configuration.
    // please use the ExpandedNodeId syntax instead.
    {
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "NodeId": {
            "Identifier": "ns=0;i=2258"
        }
    }
    // please consult the OPC UA specification for details on how OPC monitored node sampling interval and OPC subscription publishing interval settings are handled by the OPC UA stack.
    // the publishing interval of the data to Azure IoT Hub is controlled by the command line settings (or the default: publish data to IoT Hub at least each 1 second).
]
```

## <a name="run-the-application"></a>Ausführen der Anwendung

### <a name="command-line-options"></a>Befehlszeilenoptionen

Verwenden Sie die Befehlszeilenoption `--help`, um die vollständige Nutzung der Anwendung anzuzeigen. Das folgende Beispiel zeigt die Struktur eines Befehls:

```cmd/sh
OpcPublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]
```

`applicationname` ist der zu verwendende Name der OPC UA-Anwendung. Dieser Parameter ist erforderlich. Der Anwendungsname wird auch verwendet, um den Herausgeber in der IoT Hub-Geräteregistrierung zu registrieren.

`IoT Hubconnectionstring` ist die Verbindungszeichenfolge des IoT Hub-Besitzers. Dieser Parameter ist optional.

Die folgenden Optionen werden unterstützt:

```cmd/sh
--pf, --publishfile=VALUE
  the filename to configure the nodes to publish.
    Default: './publishednodes.json'
--sd, --shopfloordomain=VALUE
  the domain of the shopfloor. if specified this
    domain is appended (delimited by a ':' to the '
    ApplicationURI' property when telemetry is
    sent to IoT Hub.
    The value must follow the syntactical rules of a
    DNS hostname.
    Default: not set
--sw, --sessionconnectwait=VALUE
  specify the wait time in seconds publisher is
    trying to connect to disconnected endpoints and
    starts monitoring unmonitored items
    Min: 10
    Default: 10
--vc, --verboseconsole=VALUE
  the output of publisher is shown on the console.
    Default: False
--ih, --IoT Hubprotocol=VALUE
  the protocol to use for communication with Azure
    IoT Hub (allowed values: Amqp, Http1, Amqp_
    WebSocket_Only, Amqp_Tcp_Only, Mqtt, Mqtt_
    WebSocket_Only, Mqtt_Tcp_Only).
    Default: Mqtt
--ms, --IoT Hubmessagesize=VALUE
  the max size of a message which can be send to
    IoT Hub. when telemetry of this size is available
    it will be sent.
    0 will enforce immediate send when telemetry is
    available
    Min: 0
    Max: 256 * 1024
    Default: 4096
--si, --IoT Hubsendinterval=VALUE
  the interval in seconds when telemetry should be
    send to IoT Hub. If 0, then only the
    IoT Hubmessagesize parameter controls when
    telemetry is sent.
    Default: '1'
--lf, --logfile=VALUE
  the filename of the logfile to use.
    Default: './Logs/<applicationname>.log.txt'
--pn, --portnum=VALUE
  the server port of the publisher OPC server
    endpoint.
    Default: 62222
--pa, --path=VALUE
  the endpoint URL path part of the publisher OPC
    server endpoint.
    Default: '/UA/Publisher'
--lr, --ldsreginterval=VALUE
  the LDS(-ME) registration interval in ms. If 0,
    then the registration is disabled.
    Default: 0
--ot, --operationtimeout=VALUE
  the operation timeout of the publisher OPC UA
    client in ms.
    Default: 120000
--oi, --opcsamplinginterval=VALUE
  the publisher is using this as default value in
    milliseconds to request the servers to sample
    the nodes with this interval
    this value might be revised by the OPC UA
    servers to a supported sampling interval.
    please check the OPC UA specification for
    details how this is handled by the OPC UA stack.
    a negative value will set the sampling interval
    to the publishing interval of the subscription
    this node is on.
    0 will configure the OPC UA server to sample in
    the highest possible resolution and should be
    taken with care.
    Default: 1000
--op, --opcpublishinginterval=VALUE
  the publisher is using this as default value in
    milliseconds for the publishing interval setting
    of the subscriptions established to the OPC UA
    servers.
    please check the OPC UA specification for
    details how this is handled by the OPC UA stack.
    a value less than or equal zero will let the
    server revise the publishing interval.
    Default: 0
--ct, --createsessiontimeout=VALUE
  specify the timeout in seconds used when creating
    a session to an endpoint. On unsuccessful
    connection attemps a backoff up to 5 times the
    specified timeout value is used.
    Min: 1
    Default: 10
--ki, --keepaliveinterval=VALUE
  specify the interval in seconds the publisher is
    sending keep alive messages to the OPC servers
    on the endpoints it is connected to.
    Min: 2
    Default: 2
--kt, --keepalivethreshold=VALUE
  specify the number of keep alive packets a server
    can miss, before the session is disconneced
    Min: 1
    Default: 5
--st, --opcstacktracemask=VALUE
  the trace mask for the OPC stack. See github OPC .
    NET stack for definitions.
    To enable IoT Hub telemetry tracing set it to 711.
    Default: 285  (645)
--as, --autotrustservercerts=VALUE
  the publisher trusts all servers it is
    establishing a connection to.
    Default: False
--tm, --trustmyself=VALUE
  the publisher certificate is put into the trusted
    certificate store automatically.
    Default: True
--fd, --fetchdisplayname=VALUE
  enable to read the display name of a published
    node from the server. this will increase the
    runtime.
    Default: False
--at, --appcertstoretype=VALUE
  the own application cert store type.
    (allowed values: Directory, X509Store)
    Default: 'X509Store'
--ap, --appcertstorepath=VALUE
  the path where the own application cert should be
    stored
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/own'
--tt, --trustedcertstoretype=VALUE
  the trusted cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--tp, --trustedcertstorepath=VALUE
  the path of the trusted cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/trusted'
--rt, --rejectedcertstoretype=VALUE
  the rejected cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--rp, --rejectedcertstorepath=VALUE
  the path of the rejected cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/rejected'
--it, --issuercertstoretype=VALUE
  the trusted issuer cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--ip, --issuercertstorepath=VALUE
  the path of the trusted issuer cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/issuers'
--dt, --devicecertstoretype=VALUE
  the IoT Hub device cert store type.
    (allowed values: Directory, X509Store)
    Default: X509Store
--dp, --devicecertstorepath=VALUE
  the path of the iot device cert store
    Default Default (depends on store type):
    X509Store: 'My'
    Directory: 'CertificateStores/IoT Hub'
-h, --help
  show this message and exit
```

Mithilfe der folgenden Umgebungsvariablen können Sie die Anwendung steuern:
- `_HUB_CS`: Legt die Verbindungszeichenfolge des IoT Hub-Besitzers fest.
- `_GW_LOGP`: Legt den Dateinamen der zu verwendenden Protokolldatei fest.
- `_TPC_SP`: Legt den Pfad zum Speichern von Zertifikaten vertrauenswürdiger Stationen fest.
- `_GW_PNFP`: Legt den Dateinamen der Konfigurationsdatei für die Veröffentlichung fest.

Befehlszeilenargumente setzen Umgebungsvariableneinstellungen außer Kraft.

In der Regel geben Sie die Verbindungszeichenfolge des IoT Hub-Besitzers nur beim ersten Start der Anwendung an. Die Verbindungszeichenfolge ist verschlüsselt und wird im Zertifikatspeicher der Plattform gespeichert.

Bei nachfolgenden Aufrufen wird die Verbindungszeichenfolge aus dem Zertifikatspeicher der Plattform gelesen und erneut verwendet. Wenn Sie die Verbindungszeichenfolge bei jedem Start angeben, wird das Gerät in der IoT Hub-Geräteregistrierung entfernt und jedes Mal neu erstellt.

### <a name="native-on-windows"></a>Nativ unter Windows

Öffnen Sie zum nativen Ausführen der Anwendung unter Windows das `OpcPublisher.sln`-Projekt mit Visual Studio 2017. Dann erstellen und veröffentlichen Sie die Projektmappe. Sie können die Anwendung wie folgt im **Zielverzeichnis** starten, in das die Veröffentlichung erfolgt ist:

```cmd
dotnet OpcPublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-self-built-container"></a>Verwenden eines selbst erstellten Containers

Erstellen und starten Sie Ihren eigenen Container, um die Anwendung in einem selbst erstellten Container auszuführen:

```cmd/sh
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-container-from-hubdockercom"></a>Verwenden eines Containers von hub.docker.com

Auf DockerHub ist ein vordefinierter Container verfügbar. Führen Sie den folgenden Befehl aus, um ihn zu starten:

```cmd/sh
docker run microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="important-when-using-a-container"></a>Wichtige Informationen beim Verwenden eines Containers

#### <a name="access-to-the-publisher-opc-ua-server"></a>Zugriff auf den Publisher OPC-UA-Server

Der Publisher OPC-UA-Server überwacht standardmäßig Port 62222. Um diesen eingehenden Port in einem Container verfügbar zu machen, müssen Sie die `docker run`-Option `-p` verwenden:

```cmd/sh
docker run -p 62222:62222 microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="enable-intercontainer-name-resolution"></a>Aktivieren der Namensauflösung zwischen Containern

Sie müssen wie folgt vorgehen, um die Namensauflösung innerhalb eines Containers für andere Container zu aktivieren:

- Erstellen Sie ein benutzerdefiniertes Docker-Bridge-Netzwerk.
- Verbinden Sie den Container mithilfe der `--network`-Option mit dem Netzwerk.
- Weisen Sie dem Container mit der `--name`-Option einen Namen zu.

Im folgenden Beispiel werden diese Konfigurationsoptionen gezeigt:

```cmd/sh
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Der Container kann jetzt über das Netzwerk mithilfe des Namens `publisher` von anderen Containern erreicht werden.

#### <a name="assign-a-hostname"></a>Zuweisen eines Hostnamens

Publisher verwendet den Hostnamen des Computers, auf dem es ausgeführt wird, um Zertifikate und Endpunkte zu generieren. Docker wählt einen zufälligen Hostnamen aus, sofern Sie keinen Hostnamen über die Option `-h` festlegen. Hier folgt ein Beispiel zum Festlegen des internen Hostnamens des Containers auf `publisher`:

```cmd/sh
docker run -h publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="using-bind-mounts-shared-filesystem"></a>Verwenden von Bindungseinbindungen (freigegebenes Dateisystem)

In einigen Szenarien möchten Sie Konfigurationsinformationen aus Speicherorten auf dem Host lesen oder Protokolldateien an diesen Speicherorten erstellen, anstatt das Dateisystem des Containers zu verwenden. Verwenden Sie die `-v`-Option von `docker run` im Modus für die Bindungseinbindung, um dieses Verhalten zu konfigurieren. Beispiel:

```cmd/sh
-v //D/docker:/build/out/Logs
-v //D/docker:/build/out/CertificateStores
-v //D/docker:/shared
-v //D/docker:/root/.dotnet/corefx/cryptography/x509stores
```

#### <a name="store-for-x509-certificates"></a>Speicher für X509-Zertifikate

Das Speichern von X509-Zertifikaten funktioniert nicht mit Bindungseinbindungen, da die Berechtigungen für den Pfad zum Speicher für den Besitzer `rw` sein müssen. Stattdessen müssen Sie die `-v`-Option von `docker run` im Volumenmodus verwenden.

## <a name="debug-the-application"></a>Debuggen der Anwendung

### <a name="native-on-windows"></a>Nativ unter Windows

Öffnen Sie das `OpcPublisher.sln`-Projekt mit Visual Studio 2017, und starten Sie das Debuggen für die Anwendung durch Drücken von F5.

### <a name="in-a-docker-container"></a>In einem Docker-Container

Visual Studio 2017 unterstützt das Debuggen von Anwendungen in einem Docker-Container mithilfe von `docker-compose`. Sie können jedoch mit dieser Methode keine Befehlszeilenparameter übergeben.

Eine alternative Debugoption, die von Visual Studio 2017 unterstützt wird, ist das Debuggen über `ssh`. Sie können die Konfigurationsdatei `Dockerfile.ssh` des Docker-Builds im Stammverzeichnis des Repositorys verwenden, um einen für SSH aktivierten Container zu erstellen:

```cmd/sh
docker build -f .\Dockerfile.ssh -t publisherssh .
```

Sie können jetzt den Container starten, um den Herausgeber zu debuggen:

```cmd/sh
docker run -it publisherssh
```

Im Container müssen Sie den **ssh**-Daemon manuell starten:

```cmd/sh
service ssh start
```

An diesem Punkt können Sie eine **ssh**-Sitzung als Benutzer `root` mit dem Kennwort `Passw0rd` erstellen.

Führen Sie die folgenden zusätzlichen Schritte aus, um das Debuggen der Anwendung im Container vorzubereiten:

1. Erstellen Sie auf der Hostseite eine `launch.json`-Datei:

    ```json
    {
      "version": "0.2.0",
      "adapter": "<path>\\plink.exe",
      "adapterArgs": "root@localhost -pw Passw0rd -batch -T ~/vsdbg/vsdbg --interpreter=vscode",
      "languageMappings": {
        "C#": {
          "languageId": "3F5162F8-07C6-11D3-9053-00C04FA302A1",
          "extensions": [ "*" ]
        }
      },
      "exceptionCategoryMappings": {
        "CLR": "449EC4CC-30D2-4032-9256-EE18EB41B62B",
        "MDA": "6ECE07A9-0EDE-45C4-8296-818D8FC401D4"
      },
      "configurations": [
        {
          "name": ".NET Core Launch",
          "type": "coreclr",
          "cwd": "~/publisher",
          "program": "Opc.Ua.Publisher.dll",
          "args": "<put-the-publisher-command-line-options-here>",

          "request": "launch"
        }
      ]
    }
    ```

1. Erstellen Sie das Projekt, und veröffentlichen Sie es in einem Verzeichnis Ihrer Wahl.

1. Verwenden Sie ein Tool wie `WinSCP`, um die veröffentlichten Dateien in das Verzeichnis `/root/publisher` im Container zu kopieren. Wenn Sie ein anderes Verzeichnis auswählen, aktualisieren Sie die `cdw`-Eigenschaft in der Datei `launch.json`.

Jetzt können Sie das Debuggen mithilfe des folgenden Befehls im Visual Studio-Befehlsfenster starten:

```cmd
DebugAdapterHost.Launch /LaunchJson:"<path-to-the-launch.json-file-you-saved>"
```

## <a name="next-steps"></a>Nächste Schritte

Als nächster Schritt wird empfohlen, dass Sie das [Bereitstellen eines Gateways unter Windows oder Linux für die vorkonfigurierte verbundene Factory-Lösung](iot-suite-connected-factory-gateway-deployment.md) kennenlernen.