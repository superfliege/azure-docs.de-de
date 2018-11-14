---
title: Konfigurieren von nachgeschalteten Geräten mit Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie nachgeschaltete oder Blattgeräte für eine Verbindung über Azure IoT Edge-Gatewaygeräte konfigurieren.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7b9993650492574fc45b7f15fa3424060079f5fe
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50915073"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway

Azure IoT Edge ermöglicht transparente Gatewayszenarien, in denen mindestens ein Gerät Nachrichten über ein einziges Gatewaygerät weiterleiten kann, das die Verbindung zu IoT Hub aufrechterhält. Wenn Sie das Gatewaygerät konfiguriert haben, müssen Sie als Nächstes die nachgeschalteten Geräte sicher verbinden. 

Dieser Artikel erläutert häufige Probleme beim Verbinden nachgeschalteter Geräte und führt Sie durch ihre Einrichtung. Behandelt werden die folgenden Themen: 

* Grundlagen von Transport Layer Security (TLS) und Zertifikaten 
* Funktionsweise von TLS-Bibliotheken mit verschiedenen Betriebssystemen und Verarbeiten von Zertifikaten durch die Betriebssysteme
* Azure IoT-Beispiele für mehrere Sprachen zum Einstieg 

In diesem Artikel beziehen sich die Begriffe *Gateway* und *IoT Edge-Gateway* auf ein IoT Edge-Gerät, das als transparentes Gateway konfiguriert wurde. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Schritte in diesem Artikel ausführen, müssen zwei Geräte einsatzbereit sein:

1. Ein IoT Edge-Gerät, das als transparentes Gateway eingerichtet ist. 
    [Konfigurieren Sie ein IoT Edge-Gerät so, dass es als transparentes Gateway fungiert.](how-to-create-transparent-gateway.md)

    Wenn Sie Ihr Gatewaygerät konfiguriert haben, kopieren Sie das Zertifikat **azure-iot-test-only.root.ca.cert.pem** aus dem Gateway, und stellen Sie es auf Ihrem nachgeschalteten Gerät zur Verfügung. 

2. Ein nachgeschaltetes Gerät, das eine Geräteidentität von IoT Hub hat. 
    Sie können kein IoT Edge-Gerät als nachgeschaltetes Gerät verwenden. Verwenden Sie stattdessen ein Gerät, das als normales IoT-Gerät in IoT Hub registriert ist. Im Portal können Sie im Abschnitt **IoT-Geräte** ein neues Gerät registrieren. Alternativ können Sie die Azure CLI zum [Registrieren eines Geräts](../iot-hub/quickstart-send-telemetry-c.md#register-a-device) verwenden. Kopieren Sie die Verbindungszeichenfolge, und stellen Sie sie zur späteren Verwendung zur Verfügung. 

    Derzeit können sich nur nachgeschaltete Geräte mit der Authentifizierung mit symmetrischen Schlüsseln über IoT Edge-Gateways verbinden. X.509-Zertifizierungsstellen und selbstsignierte X.509-Zertifikate werden derzeit nicht unterstützt. 

## <a name="prepare-a-downstream-device"></a>Vorbereiten eines nachgeschalteten Geräts

Ein nachgeschaltetes Gerät kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub)-Clouddienst erstellt wurde. Oft verwenden diese Anwendungen das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). In der Praxis kann ein nachgeschaltetes Gerät sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird. 

Zum Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway ist Folgendes erforderlich:

1. Ein Geräte oder eine Anwendung, die mit einer IoT Hub-Geräte-Verbindungszeichenfolge konfiguriert ist, der Informationen für die Verbindung mit dem Gateway angefügt sind. 

    Die Verbindungszeichenfolge ist so formatiert: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Fügen Sie dem Ende der Verbindungszeichenfolge die Eigenschaft **GatewayHostName** mit dem Hostnamen des Gatewaygeräts an. Der Wert von **GatewayHostName** muss dem **Hostname**-Wert in der Datei „config.yaml“ des Gatewaygeräts entsprechen. 

    Die endgültige Zeichenfolge sieht wie folgt aus: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. Das Gerät oder die Anwendung muss dem Zertifikat der **Stammzertifizierungsstelle** oder der **Besitzerzertifizierungsstelle** des Gateways vertrauen, um die TLS-Verbindungen mit den Gatewaygeräten zu überprüfen. 

    Dieser kompliziertere Schritt wird im weiteren Verlauf dieses Artikels ausführlich erläutert. Dieser Schritt kann auf zwei Arten ausgeführt werden: durch Installation des Zertifizierungsstellenzertifikats (ZS-Zertifikat) im Zertifikatspeicher des Betriebssystems oder (für bestimmte Sprachen) durch Verweisen auf das Zertifikat in Anwendungen mit Azure IoT SDKs.

## <a name="tls-and-certificate-fundamentals"></a>Grundlagen von TLS und Zertifikaten

Die sichere Verbindung nachgeschalteter Geräte mit IoT Edge unterliegt den gleichen Herausforderungen wie jede andere sichere Client/Server-Kommunikation, die über das Internet erfolgt. Ein Client und ein Server kommunizieren mithilfe von [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) sicher über das Internet. TLS wird mit Standardkonstrukten der [Public Key-Infrastruktur](https://en.wikipedia.org/wiki/Public_key_infrastructure) erstellt, die als Zertifikate bezeichnet werden. TLS ist ziemlich raffiniert und deckt zahlreiche Themen in Hinblick auf die Sicherung von zwei Endpunkten ab. Der folgende Abschnitt beschreibt jedoch kurz und bündig, wie Sie Geräte sicher mit einem IoT Edge-Gateway verbinden.

Wenn sich ein Client mit einem Server verbindet, präsentiert der Server eine Kette von Zertifikaten, die als *Serverzertifikatkette* bezeichnet wird. Eine Zertifikatkette besteht i.d.R. aus einem Stammzertifizierungsstellen-Zertifikat, mindestens einem Zwischen-ZS-Zertifikat und dem Zertifikat des Servers selbst. Ein Client stellt eine Vertrauensstellung mit einem Server her, indem er die gesamte Serverzertifikatkette kryptographisch überprüft. Diese Clientvalidierung der Serverzertifikatkette wird als *Serverauthentifizierung* bezeichnet. Zum Überprüfen einer Serverzertifikatkette benötigt der Client eine Kopie des Stamm-ZS-Zertifikats, mit dem das Serverzertifikat erstellt (oder ausgestellt) wurde. Normalerweise ist ein Browser beim Verbinden mit Websites mit häufig verwendeten ZS-Zertifikaten vorkonfiguriert, um den Workflow des Clients zu vereinfachen. 

Wenn sich ein Gerät mit Azure IoT Hub verbindet, ist das Gerät der Client und der IoT Hub-Clouddienst der Server. Der IoT Hub-Clouddienst wird durch ein Stamm-ZS-Zertifikat namens **Baltimore CyberTrust Root** gesichert, das öffentlich verfügbar und weitverbreitet ist. Da das IoT Hub-ZS-Zertifikat auf den meisten Geräten bereits installiert ist, wird es von vielen TLS-Implementierungen (OpenSSL, Schannel, LibreSSL) bei der Serverzertifikatüberprüfung automatisch verwendet. Ein Gerät, das sich erfolgreich mit IoT Hub verbinden kann, hat möglicherweise Probleme beim Verbinden mit einem IoT Edge-Gateway.

Wenn sich ein Gerät mit einem IoT Edge-Gateway verbindet, ist das nachgeschaltete Gerät der Client und das Gatewaygerät der Server. Mit Azure IoT Edge können Operatoren (und Benutzer) Gatewayzertifikatketten nach Belieben erstellen. Der Operator hat die Möglichkeit, ein öffentliches ZS-Zertifikat wie Baltimore oder ein selbstsigniertes (oder internes) Stamm-ZS-Zertifikat zu verwenden. Öffentliche ZS-Zertifikate sind oft mit Kosten verbunden und werden deswegen i.d.R. in Produktionsszenarien eingesetzt. Selbstsignierte ZS-Zertifikate werden für Entwicklung und Tests bevorzugt. Die im Abschnitt „Voraussetzungen“ aufgeführten Artikel zum Einrichten transparenter Gateways verwenden selbstsignierte Stamm-ZS-Zertifikate. 

Wenn Sie ein selbstsigniertes Stamm-ZS-Zertifikat für ein IoT Edge-Gateway verwenden, muss es auf allen nachgeschalteten Geräten installiert oder bereitgestellt werden, die sich mit dem Gateway verbinden wollen. 

Weitere Informationen zu IoT Edge-Zertifikaten und einigen Auswirkungen auf die Produktion finden Sie unter [Details zur Verwendung von IoT Edge-Zertifikaten](iot-edge-certs.md).

## <a name="install-certificates-using-the-os"></a>Installieren von Zertifikaten mit dem Betriebssystem

Dieser Artikel verweist mit der *Besitzerzertifizierungsstelle* auf das Stamm-ZS-Zertifikat, da dies der Begriff ist, der von den Skripts im Artikel über die Gatewayvoraussetzungen verwendet wird. 

Die Installation des Besitzer-ZS-Zertifikats im Zertifikatspeicher des Betriebssystems ermöglicht den meisten Anwendungen die Verwendung des Besitzer-ZS-Zertifikats. Es gibt einige Ausnahmen, z.B. die NodeJS-Anwendung, die nicht den Zertifikatspeicher des Betriebssystems verwendet, sondern den internen Zertifikatspeicher der Node-Runtime. Wenn Sie das Zertifikat nicht auf Betriebssystemebene installieren können, lesen Sie die sprachspezifischen Beispiele weiter unten in diesem Artikel, um das Zertifikat mit dem Azure IoT SDK in Anwendungen zu verwenden. 

### <a name="ubuntu"></a>Ubuntu

Die folgenden Befehle sind ein Beispiel für die Installation eines ZS-Zertifikats auf einem Ubuntu-Host. In diesem Beispiel wird davon ausgegangen, dass Sie das Zertifikat **azure-iot-test-only.root.ca.cert.pem** aus den Artikeln über die Voraussetzungen verwenden und das Zertifikat an einen Speicherort auf dem nachgeschalteten Gerät kopiert haben.  

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

Es sollte die Meldung „Updating certificates in /etc/ssl/certs 1 added, 0 removed; done“ (Zertifikate in /etc/ssl/certs werden aktualisiert... 1 hinzugefügt, 0 entfernt; fertig) angezeigt werden.

### <a name="windows"></a>Windows

Die folgenden Schritte sind ein Beispiel für die Installation eines ZS-Zertifikats auf einem Windows-Host. In diesem Beispiel wird davon ausgegangen, dass Sie das Zertifikat **azure-iot-test-only.root.ca.cert.pem** aus den Artikeln über die Voraussetzungen verwenden und das Zertifikat an einen Speicherort auf dem nachgeschalteten Gerät kopiert haben.  

1. Suchen Sie im Startmenü nach **Computerzertifikate verwalten**, und wählen Sie die Option aus. Daraufhin wird das Hilfsprogramm **certlm** geöffnet.
2. Navigieren Sie zu **Zertifikate – Lokaler Computer** > **Vertrauenswürdige Stammzertifizierungsstellen**.
3. Klicken Sie mit der rechten Maustaste auf **Zertifikat**, und wählen Sie **Alle Aufgaben** > **Importieren** aus. Der Zertifikatimport-Assistent wird gestartet. 
4. Folgen Sie den Anweisungen und importieren Sie die Zertifikatsdatei `<path>/azure-iot-test-only.root.ca.cert.pem`. Wenn Sie damit fertig sind, sollte die Meldung „Der Import war erfolgreich.“ angezeigt werden. 

Sie können Zertifikate auch programmgesteuert über .NET-APIs installieren (siehe .NET-Beispiel weiter unten in diesem Artikel). 

I.d.R. verwenden Anwendungen den von Windows bereitgestellten TLS-Stapel namens [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel), um eine sichere Verbindung über TLS herzustellen. Schannel *erfordert*, dass alle Zertifikate im Windows-Zertifikatspeicher installiert werden, bevor eine TLS-Verbindung hergestellt werden kann.

## <a name="use-certificates-with-azure-iot-sdks"></a>Verwenden von Zertifikaten mit Azure IoT SDKs

Dieser Artikel referenziert das Stamm-ZS-Zertifikat als *Besitzerzertifizierungsstelle*, da dies der Begriff ist, der von den Skripts verwendet wird, die das selbstsignierte Zertifikat in den Artikeln über die Voraussetzungen generieren. 

In diesem Abschnitt wird beschrieben, wie sich die Azure IoT SDKs mithilfe einfacher Beispielanwendungen mit einem IoT Edge-Gerät verbinden. Das Ziel aller Beispielanwendungen ist es, den Geräteclient zu verbinden und Telemetrienachrichten an das Gateway zu senden, dann die Verbindung zu schließen und zu beenden. 

### <a name="common-concepts-across-all-azure-iot-sdks"></a>Allgemeine Konzepte für alle Azure IoT SDKs

Zwei Voraussetzungen müssen erfüllt sein, bevor Sie die Anwendungsbeispiele verwenden können:

1. Sie haben die IoT Hub-Verbindungszeichenfolge Ihres nachgeschalteten Geräts so geändert, dass sie auf das Gatewaygerät verweist.

    Die Verbindungszeichenfolge ist so formatiert: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Fügen Sie dem Ende der Verbindungszeichenfolge die Eigenschaft **GatewayHostName** mit dem Hostnamen des Gatewaygeräts an. Der Wert von **GatewayHostName** muss dem **Hostname**-Wert in der Datei „config.yaml“ des Gatewaygeräts entsprechen. 

    Die endgültige Zeichenfolge sieht wie folgt aus: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. Sie haben den vollständigen Pfad zum Stamm-ZS-Zertifikat kopiert und auf Ihrem nachgeschalteten Gerät gespeichert.

    Beispiel: `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>NodeJS

Dieser Abschnitt enthält eine Beispielanwendung zum Verbinden eines Azure IoT-NodeJS-Geräteclients mit einem IoT Edge-Gateway. Für Linux- und Windows-Hosts müssen Sie das Stamm-ZS-Zertifikat wie hier gezeigt auf Anwendungsebene installieren, da NodeJS-Anwendungen nicht den Zertifikatspeicher des Systems verwenden. 

1. Rufen Sie das Beispiel für **edge_downstream_device.js** aus dem [Repository für das Azure IoT-Geräte-SDK für Node.js-Beispiele](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) ab. 
2. Stellen Sie sicher, dass alle Voraussetzungen zum Ausführen des Beispiels erfüllt sind, indem Sie die Datei **readme.md** überprüfen. 
3. Aktualisieren Sie in der Datei „edge_downstream_device_device.js“ die Variablen **connectionString** und **edge_ca_cert_path**. 
4. In der SDK-Dokumentation finden Sie Anweisungen zum Ausführen des Beispiels auf Ihrem Gerät. 

Zum besseren Verständnis dieses Beispiels zeigt der folgende Codeausschnitt, wie das Client-SDK die Zertifikatsdatei liest und damit eine sichere TLS-Verbindung herstellt: 

```nodejs
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

Dieser Abschnitt stellt eine Beispielanwendung zum Verbinden eines Azure IoT-.NET-Geräteclients mit einem IoT Edge-Gateway vor. .NET-Anwendungen können automatisch alle installierten Zertifikate im Systemzertifikatspeicher sowohl auf Linux- als auch auf Windows-Hosts verwenden.

1. Rufen Sie das Beispiel für **EdgeDownstreamDevice** aus dem Ordner [IoT Edge-.NET-Beispiele](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice) ab. 
2. Stellen Sie sicher, dass alle Voraussetzungen zum Ausführen des Beispiels erfüllt sind, indem Sie die Datei **readme.md** überprüfen. 
3. Aktualisieren Sie in der Datei **Properties/launchSettings.json** die Variablen **DEVICE_CONNECTION_STRING** und **CA_CERTIFICATE_PATH**. Wenn Sie das im vertrauenswürdigen Zertifikatspeicher auf dem Hostsystem installierte Zertifikat verwenden möchten, lassen Sie diese Variable leer. 
4. In der SDK-Dokumentation finden Sie Anweisungen zum Ausführen des Beispiels auf Ihrem Gerät. 

Um ein vertrauenswürdiges Zertifikat programmgesteuert über eine .NET-Anwendung im Zertifikatspeicher zu installieren, verwenden Sie die Funktion **InstallCACert()** in der Datei **EdgeDownstreamDevice/Program.cs**. Dieser Vorgang ist idempotent, sodass er mehrfach mit den gleichen Werten ohne weitere Auswirkungen ausgeführt werden kann. 

### <a name="c"></a>C

Dieser Abschnitt stellt eine Beispielanwendung zum Verbinden eines Azure IoT-C-Geräteclients mit einem IoT Edge-Gateway vor. Das C SDK ist mit vielen TLS-Bibliotheken kompatibel, darunter OpenSSL, WolfSSL und Schannel. Weitere Informationen finden Sie unter [Azure IoT SDK für C](https://github.com/Azure/azure-iot-sdk-c). 

1. Rufen Sie die Anwendung **iotedge_downstream_device_sample** aus den [Beispielen für das Azure IoT-Geräte-SDK für C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples) ab. 
2. Stellen Sie sicher, dass alle Voraussetzungen zum Ausführen des Beispiels erfüllt sind, indem Sie die Datei **readme.md** überprüfen. 
3. Aktualisieren Sie in der Datei „iotedge_downstream_device_sample.c“ die Variablen **connectionString** und **edge_ca_cert_path**. 
4. In der SDK-Dokumentation finden Sie Anweisungen zum Ausführen des Beispiels auf Ihrem Gerät. 

Das Azure IoT-Geräte-SDK für C bietet die Möglichkeit, beim Einrichten des Clients ein ZS-Zertifikat zu registrieren. Dieser Vorgang installiert das Zertifikat nicht, sondern verwendet ein Zeichenfolgenformat des Zertifikats im Speicher. Das gespeicherte Zertifikat wird dem zugrunde liegenden TLS-Stapel beim Verbindungsaufbau bereitgestellt. 

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

Wenn Sie weder OpenSSL noch andere TLS-Bibliotheken verwenden, nutzt das SDK auf Windows-Hosts standardmäßig Schannel. Damit Schannel funktioniert, muss das IoT Edge-Stamm-ZS-Zertifikat im Windows-Zertifikatspeicher installiert und nicht mit Vorgang `IoTHubDeviceClient_SetOption` eingerichtet werden. 

### <a name="java"></a>Java

Dieser Abschnitt stellt eine Beispielanwendung zum Verbinden eines Azure IoT-Java-Geräteclients mit einem IoT Edge-Gateway vor. 

1. Rufen Sie das Beispiel für **Send-event** aus [den Beispielen für das Azure IoT-Geräte-SDK für Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) ab. 
2. Stellen Sie sicher, dass alle Voraussetzungen zum Ausführen des Beispiels erfüllt sind, indem Sie die Datei **readme.md** überprüfen. 
3. In der SDK-Dokumentation finden Sie Anweisungen zum Ausführen des Beispiels auf Ihrem Gerät.

### <a name="python"></a>Python

Dieser Abschnitt stellt eine Beispielanwendung zum Verbinden eines Azure IoT-Python-Geräteclients mit einem IoT Edge-Gateway vor. 

1. Rufen Sie das Beispiel für **edge_downstream_client** aus [den Beispielen für das Azure IoT-Geräte-SDK für Python](https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples) ab. 
2. Stellen Sie sicher, dass alle Voraussetzungen zum Ausführen des Beispiels erfüllt sind, indem Sie die Datei **readme.md** überprüfen. 
3. Aktualisieren Sie in der Datei „edge_downstream_client.py“ die Variablen **CONNECTION_STRING** und **TRUSTED_ROOT_CA_CERTIFICATE_PATH**. 
4. In der SDK-Dokumentation finden Sie Anweisungen zum Ausführen des Beispiels auf Ihrem Gerät. 


## <a name="test-the-gateway-connection"></a>Testen der Gatewayverbindung

Dies ist ein Beispielbefehl, der überprüft, ob alles ordnungsgemäß eingerichtet wurde. Es sollte die Meldung „Überprüfung OK“ angezeigt werden.

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie IoT Edge [Offlinefunktionen](offline-capabilities.md) für nachgeschaltete Geräte erweitern kann. 