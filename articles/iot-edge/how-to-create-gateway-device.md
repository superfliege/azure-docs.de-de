---
title: "Erstellen eines Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation"
description: "Verwenden von Azure IoT Edge zum Erstellen eines Gatewaygeräts, das Informationen für mehrere Geräte verarbeiten kann"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e1337ddf5ed84a06a62e2faa198f3e8fb49bc3bd
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-iot-edge-gateway-device-to-process-data-from-other-iot-devices---preview"></a>Erstellen eines IoT Edge-Gatewaygeräts zum Verarbeiten der Daten von anderen IoT-Geräten – Vorschau

IoT Edge-Geräte können auf zwei Arten als Gateways verwendet werden:

* Verbinden nachgeschalteter Geräte, die [Azure IoT-Geräte-SDK][lnk-devicesdk] verwenden
* Verbinden von Geräten, die das IoT Hub-Protokoll nicht verwenden

Die Verwendung eines IoT Edge-Geräts als Gateway bietet die folgenden Vorteile:

* **Multiplexing der Verbindung**. Alle Geräte, die mit IoT Hub über ein IoT Edge-Gerät verbunden sind, verwenden dieselbe zugrunde liegende Verbindung.
* **Glättung des Datenverkehrs**. Das IoT Edge-Gerät implementiert im Falle einer IoT Hub-Drosselung automatisch exponentielles Backoff, wobei die Nachrichten lokal beibehalten werden. Dadurch wird Ihre Lösung unempfindlich gegenüber Spitzen im Datenverkehr.
* **Eingeschränkte Offlineunterstützung**. Das Gatewaygerät speichert Nachrichten lokal, die nicht an IoT Hub übermittelt werden können.

In diesem Artikel wird ein IoT Edge-Gerät, das als Gateway verwendet wird, als *IoT Edge-Gateway* bezeichnet.

>[!NOTE]
>Derzeit gilt Folgendes:
> * Nachgeschaltete Geräte können sich nicht beim Gateway authentifizieren, wenn das Gateway von IoT Hub getrennt ist.
> * IoT Edge-Geräte können keine Verbindung mit IoT Edge-Gateways herstellen.

## <a name="use-the-azure-iot-device-sdk"></a>Verwenden des Azure IoT-Geräte-SDK

Der Edge Hub, der auf allen IoT Edge-Geräten installiert ist, stellt Folgendes für nachgeschaltete Geräte bereit:

* D2C- und C2D-Nachrichten
* Direkte Methoden
* Gerätezwillingsvorgänge

>[!NOTE]
>Derzeit können nachgeschaltete Geräte bei Verbindung über einen IoT Edge-Gateway keinen Dateiupload verwenden.

Wenn Sie Geräte mithilfe des Azure IoT-Geräte-SDK mit einem IoT Edge-Gateway verbinden, sind folgende Schritte erforderlich:

* Richten Sie das nachgeschaltete Gerät mit einer Verbindungszeichenfolge ein, die auf den Hostnamen des Gatewaygeräts verweist.
* Stellen Sie sicher, dass das nachgeschaltete Gerät dem Zertifikat vertraut, das vom Gatewaygerät zum Akzeptieren der Verbindung verwendet wird.

Beim Installieren der Azure IoT Edge-Laufzeit mithilfe des Steuerskripts wird ein Zertifikat für den Edge Hub erstellt, wie es im Tutorial zum Installieren von IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] und [Linux][lnk-tutorial1-lin] beschrieben ist. Dieses Zertifikat wird vom Edge Hub zum Akzeptieren eingehender TLS-Verbindungen verwendet, und es muss beim Herstellen einer Verbindung mit dem Gatewaygerät vom nachgeschalteten Gerät als vertrauenswürdig angesehen werden.

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von derselben Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Sicherheit][lnk-iothub-x509] in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT Hub zugeordnet (*IoT Hub-Besitzerzertifizierungsstelle*), und eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, ist auf den IoT Edge-Geräten installiert.

>[!IMPORTANT]
>Derzeit können IoT Edge-Geräte und nachgeschaltete Geräte nur [SAS-Token][lnk-iothub-tokens] für die Authentifizierung bei IoT Hub verwenden. Die Zertifikate werden nur zur Überprüfung der TLS-Verbindung zwischen dem Blattknoten- und Gatewaygerät verwendet.

Bei dieser Konfiguration wird die **IoT Hub-Besitzerzertifizierungsstelle** für folgende Zwecke verwendet:
* Signaturzertifikat für die Einrichtung der IoT Edge-Laufzeit auf allen IoT Edge-Geräten
* Öffentliches Schlüsselzertifikat, das auf nachgeschalteten Geräten installiert ist

Dies führt zu einer Lösung, bei der alle Geräte ein beliebiges IoT Edge-Gerät als Gateway verwenden können, sofern diese mit demselben IoT Hub verbunden sind.

### <a name="create-the-certificates-for-test-scenarios"></a>Erstellen der Zertifikate für Testszenarien

Sie können die PowerShell- und Bash-Beispielskripts verwenden, die unter dem [Beispiel zum Verwalten eines Zertifizierungsstellenzertifikats][lnk-ca-scripts] beschrieben sind, um ein selbstsigniertes Zertifikat der **IoT Hub-Besitzerzertifizierungsstelle** sowie Gerätezertifikate zu generieren, die damit signiert werden.

1. Führen Sie Schritt 1 in dem [Beispiel zum Verwalten eines Zertifizierungsstellenzertifikats][lnk-ca-scripts] aus, um die Skripts zu installieren.
2. Führen Sie Schritt 2 aus, um die **IoT Hub-Besitzerzertifizierungsstelle** zu generieren. Diese Datei wird von den nachgeschalteten Geräten zum Überprüfen der Verbindung verwendet.

Gehen Sie folgendermaßen vor, um ein Zertifikat für Ihr Gatewaygerät zu generieren.

#### <a name="bash"></a>Bash

* Führen Sie `./certGen.sh create_edge_device_certificate myGateway` aus, um das neue Gerätezertifikat zu erstellen.  
  Dadurch werden die Dateien „.\certs\new-edge-device.*“ erstellt, die den öffentlichen Schlüssel und die PFX enthalten, sowie „.\private\new-edge-device.key.pem“, die den privaten Schlüssel des Geräts enthält.  
* `cat new-edge-device.cert.pem azure-iot-test-only.intermediate.cert.pem azure-iot-test-only.root.ca.cert.pem > new-edge-device-full-chain.cert.pem`, um den öffentlichen Schlüssel abzurufen.
* `./private/new-edge-device.cert.pem` enthält den privaten Schlüssel des Geräts.

#### <a name="powershell"></a>PowerShell

* Führen Sie `New-CACertsEdgeDevice myGateway` aus, um das neue Gerätezertifikat zu erstellen.
  Dadurch werden die Dateien „myEdgeDevice*“ erstellt, die den öffentlichen Schlüssel, den privaten Schlüssel und die PFX dieses Zertifikats enthalten.  Wenn Sie während des Signierungsprozesses zur Eingabe eines Kennworts aufgefordert werden, geben Sie „1234“ ein.


>[!IMPORTANT]
>Dieses Beispiel dient lediglich Testzwecken. Im Falle von Produktionsszenarien finden Sie unter [Schützen Ihrer IoT-Bereitstellung][lnk-iothub-secure-deployment] die Azure IoT-Richtlinien zum Schützen Ihrer IoT-Lösung, und stellen Sie das Zertifikat entsprechend bereit.

### <a name="configure-an-iot-edge-device-as-a-gateway"></a>Konfigurieren eines IoT Edge-Geräts als Gateway

Zum Konfigurieren Ihres IoT Edge-Geräts als Gateway müssen Sie es lediglich für die Verwendung des im vorherigen Abschnitts erstellten Gerätezertifikats konfigurieren.

Hierbei wird von den folgenden Dateinamen aus den oben genannten Beispielskripts ausgegangen:

| Ausgabe | Bash-Skript | PowerShell |
| ------ | ----------- | ---------- |
| Gerätezertifikat | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| Privater Schlüssel des Geräts | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Gerätezertifikatkette | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| IoT Hub-Besitzerzertifizierungsstelle | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

 Analog zur Installation, die im Artikel zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] beschrieben ist, müssen Sie die oben angegebenen Informationen für die IoT Edge-Laufzeit bereitstellen. 
 
 Linux:

        sudo iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem

Windows:

        iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/RootCA.pem

Standardmäßig wird mit den Beispielskripts keine Passphrase für den privaten Schlüssel des Geräts festgelegt. Wenn Sie eine Passphrase festlegen, fügen Sie den folgenden Parameter hinzu:

        --device-ca-passphrase {passphrase}

Das Skript fordert zum Festlegen einer Passphrase für das Edge-Agent-Zertifikat auf.
Nach diesem Befehl müssen Sie die IoT Edge-Laufzeit neu starten.

### <a name="configure-an-iot-hub-device-application-as-a-downstream-device"></a>Konfigurieren einer IoT Hub-Geräteanwendung als nachgeschaltetes Gerät

Ein nachgeschaltetes Gerät kann eine beliebige Anwendung sein, die das [Azure IoT-Geräte-SDK][lnk-devicesdk] verwendet, wie beispielsweise die einfache Anwendung, die unter [Herstellen einer Verbindung zwischen Ihrem Gerät und Ihrem IoT Hub mit .NET][lnk-iothub-getstarted] beschrieben ist.

Zunächst muss eine nachgeschaltete Geräteanwendung dem Zertifikat der **IoT Hub-Besitzerzertifizierungsstelle** vertrauen, um die TLS-Verbindungen mit den Gatewaygeräten zu überprüfen. Dieser Schritt kann in der Regel auf zwei Arten ausgeführt werden: auf Betriebssystemebene oder (für bestimmte Sprachen) auf Anwendungsebene.

Sie können beispielsweise für .NET-Anwendungen den folgenden Codeausschnitt hinzufügen, um ein Zertifikat im PEM-Format, das im Pfad `certPath` gespeichert ist, als vertrauenswürdig einzustufen.

        using System.Security.Cryptography.X509Certificates;
        
        ...

        X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
        store.Open(OpenFlags.ReadWrite);
        store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
        store.Close();

Beachten Sie, dass mit den oben erwähnten Beispielskripts der öffentliche Schlüssel in der Datei `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) oder `RootCA.pem` (PowerShell) generiert wird.

Die Ausführung dieses Schritts auf Betriebssystemebene ist unter Windows und verschiedenen Linux-Distributionen unterschiedlich.

Im zweiten Schritt wird das IoT Hub-Geräte-SDK mit einer Verbindungszeichenfolge initialisiert, die auf den Hostnamen des Gatewaygeräts verweist.
Dazu wird die `GatewayHostName`-Eigenschaft an die Verbindungszeichenfolge des Geräts angefügt. Es folgt ein Beispiel für die Verbindungszeichenfolge eines Geräts mit angefügter `GatewayHostName`-Eigenschaft:

        HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com

Durch diese beiden Schritte ist es Ihrer Geräteanwendung möglich, eine Verbindung mit dem Gatewaygerät herzustellen.

## <a name="use-other-protocols"></a>Verwenden anderer Protokolle

Eine der wichtigsten Funktionen eines Gatewaygeräts ist das Interpretieren von Protokollen der nachgeschalteten Geräte. Zum Verbinden von Geräten, die das IoT Hub-Protokoll nicht verwenden, können Sie ein IoT Edge-Modul entwickeln, das diese Übersetzung ausführt und eine Verbindung im Auftrag des nachgeschalteten Geräts mit IoT Hub herstellt.

Sie können ein *transparentes* oder *nicht transparentes* Gateway erstellen:

| &nbsp; | Transparentes Gateway | Nicht transparentes Gateway|
|--------|-------------|--------|
| Identitäten, die in der IoT Hub-Identitätsregistrierung gespeichert sind | Identitäten aller verbundenen Geräte | Nur die Identität des Gatewaygeräts |
| Gerätezwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling | Nur das Gateway hat einen Geräte- und Modulzwilling |
| Direkte Methoden und C2D-Nachrichten | Die Cloud kann jedes verbundene Gerät einzeln adressieren | Die Cloud kann nur das Gatewaygerät adressieren |
| [IoT Hub-Drosselungen und -Kontingente][lnk-iothub-throttles-quotas] | Gelten für jedes Gerät | Gelten für das Gatewaygerät |

Bei Verwendung eines nicht transparenten Gatewaymusters verwenden alle Geräte, die eine Verbindung über dieses Gateway herstellen, dieselbe C2D-Warteschlange, die höchstens 50 Nachrichten enthalten kann. Daraus folgt, dass das nicht transparente Gatewaymuster nur verwendet werden sollte, wenn sehr wenige Geräte eine Verbindung über die einzelnen Bereichsgateways herstellen und der C2D-Datenverkehr gering ist.

Bei Implementierung eines nicht transparenten Gateways verwendet das Protokollübersetzungsmodul die Verbindungszeichenfolge des Moduls.

Bei Implementierung eines transparenten Gateways erstellt das Modul mehrere Instanzen des IoT Hub Geräteclients und verwendet die Verbindungszeichenfolgen für die nachgeschalteten Geräte.

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/CACertToolEdge/tools/CACertificates/CACertificateOverview.md