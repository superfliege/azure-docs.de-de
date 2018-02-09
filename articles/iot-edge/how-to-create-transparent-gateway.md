---
title: "Erstellen eines transparenten Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation"
description: "Verwenden von Azure IoT Edge zum Erstellen eines transparenten Gatewaygeräts, das Informationen für mehrere Geräte verarbeiten kann"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/04/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1a35a6adae1eec88b314252ea0e3f2eb86f10c1d
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-iot-edge-device-that-acts-as-a-transparent-gateway---preview"></a>Erstellen eines IoT Edge-Geräts, das als transparentes Gateway fungiert – Vorschauversion

Dieser Artikel enthält eine ausführliche Anleitung zur Verwendung eines IoT Edge-Geräts als transparentes Gateway. Im restlichen Teil dieses Artikels bezieht sich der Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät, das als transparentes Gateway verwendet wird. Ausführlichere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway][lnk-edge-as-gateway], das einen konzeptionellen Überblick bietet. 

>[!NOTE]
>Derzeit gilt Folgendes:
> * Wenn das Gateway vom IoT Hub getrennt wird, können sich nachgeschaltete Geräte nicht am Gateway authentifizieren.
> * IoT Edge-Geräte können keine Verbindung mit IoT Edge-Gateways herstellen.

## <a name="understand-the-azure-iot-device-sdk"></a>Informationen zum Azure IoT-Geräte-SDK


Der Edge Hub, der auf allen IoT Edge-Geräten installiert ist, stellt Folgendes für nachgeschaltete Geräte bereit:

* D2C- und C2D-Nachrichten
* Direkte Methoden
* Gerätezwillingsvorgänge

Derzeit können nachgeschaltete Geräte bei Verbindung über einen IoT Edge-Gateway keinen Dateiupload verwenden.

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

## <a name="create-the-certificates-for-test-scenarios"></a>Erstellen der Zertifikate für Testszenarien

Sie können die PowerShell- und Bash-Beispielskripts verwenden, die unter dem [Beispiel zum Verwalten eines Zertifizierungsstellenzertifikats][lnk-ca-scripts] beschrieben sind, um ein selbstsigniertes Zertifikat der **IoT Hub-Besitzerzertifizierungsstelle** sowie Gerätezertifikate zu generieren, die damit signiert werden.

>[!IMPORTANT]
>Dieses Beispiel dient lediglich Testzwecken. Im Falle von Produktionsszenarien finden Sie unter [Schützen Ihrer IoT-Bereitstellung][lnk-iothub-secure-deployment] die Azure IoT-Richtlinien zum Schützen Ihrer IoT-Lösung, und stellen Sie das Zertifikat entsprechend bereit.


1. Klonen Sie die Microsoft Azure IoT SDKs und -Bibliotheken für C von GitHub:

   ```
   git clone -b modules-preview https://github.com/Azure/azure-iot-sdk-c.git 
   ```

2. Um die Zertifikatsskripts zu installieren, befolgen Sie die Anweisungen in **Schritt 1 – Anfangssetup** von [Beispiel zum Verwalten eines Zertifizierungsstellenzertifikats][lnk-ca-scripts]. 
3. Zum Generieren der **IoT Hub-Besitzerzertifizierungsstelle** befolgen Sie die Anweisungen in **Schritt 2 – Erstellen der Zertifikatskette**. Diese Datei wird von den nachgeschalteten Geräten verwendet, um die Verbindung zu überprüfen.
4. Verwenden Sie entweder die Bash- oder PowerShell-Anweisungen, um ein Zertifikat für Ihr Gatewaygerät zu erstellen:

### <a name="bash"></a>Bash

Erstellen Sie das neue Gerätezertifikat:

   ```
   ./certGen.sh create_edge_device_certificate myGateway
   ```

Neue Dateien werden erstellt: „.\certs\new-edge-device.*“ enthält den öffentlichen Schlüssel und die PFX und „.\private\new-edge-device.key.pem“ enthält den privaten Schlüssel des Geräts.
 
Führen Sie im Verzeichnis `certs` den folgenden Befehl aus, um die vollständige Kette des öffentlichen Geräteschlüssels zu erhalten:

   ```
   cat ./new-edge-device.cert.pem ./azure-iot-test-only.intermediate.cert.pem ./azure-iot-test-only.root.ca.cert.pem > ./new-edge-device-full-chain.cert.pem
   ```

### <a name="powershell"></a>PowerShell

Erstellen Sie das neue Gerätezertifikat: 
   ```
   New-CACertsEdgeDevice myGateway
   ```

Es werden neue „myEdgeDevice*“-Dateien erstellt, die den öffentlichen Schlüssel, den privaten Schlüssel und die PFX dieses Zertifikats enthalten. 

Wenn Sie während des Signierungsprozesses zur Eingabe eines Kennworts aufgefordert werden, geben Sie „1234“ ein.

## <a name="configure-a-gateway-device"></a>Konfigurieren eines Gatewaygeräts

Zum Konfigurieren Ihres IoT Edge-Geräts als Gateway müssen Sie es lediglich für die Verwendung des im vorherigen Abschnitts erstellten Gerätezertifikats konfigurieren.

Hierbei wird von den folgenden Dateinamen aus den oben genannten Beispielskripts ausgegangen:

| Output | Bash-Skript | PowerShell |
| ------ | ----------- | ---------- |
| Gerätezertifikat | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| Privater Schlüssel des Geräts | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Gerätezertifikatkette | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| IoT Hub-Besitzerzertifizierungsstelle | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

Stellen Sie die Geräte- und Zertifikatsinformationen der IoT Edge-Laufzeit zur Verfügung. 
 
Unter Linux mithilfe der Bash-Ausgabe:

   ```
   sudo iotedgectl setup --connection-string {device connection string}
        --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
        --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
        --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
        --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
        --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem
   ```

Unter Windows mithilfe der PowerShell-Ausgabe:

   ```
   iotedgectl setup --connection-string {device connection string}
        --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
        --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
        --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
        --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
        --owner-ca-cert-file {full path}/RootCA.pem
   ```

Standardmäßig wird mit den Beispielskripts keine Passphrase für den privaten Schlüssel des Geräts festgelegt. Wenn Sie eine Passphrase festlegen, fügen Sie den folgenden Parameter hinzu:

   ```
   --device-ca-passphrase {passphrase}
   ```

Das Skript fordert Sie zum Festlegen einer Passphrase für das Edge-Agent-Zertifikat auf. Starten Sie die IoT Edge-Laufzeit nach diesem Befehl neu:

   ```
   iotedgectl restart
   ```

## <a name="configure-a-downstream-device"></a>Konfigurieren eines nachgeschalteten Geräts

Ein nachgeschaltetes Gerät kann eine beliebige Anwendung sein, die das [Azure IoT-Geräte-SDK][lnk-devicesdk] verwendet, wie beispielsweise die einfache Anwendung, die unter [Herstellen einer Verbindung zwischen Ihrem Gerät und Ihrem IoT Hub mit .NET][lnk-iothub-getstarted] beschrieben ist.

Zunächst muss eine nachgeschaltete Geräteanwendung dem Zertifikat der **IoT Hub-Besitzerzertifizierungsstelle** vertrauen, um die TLS-Verbindungen mit den Gatewaygeräten zu überprüfen. Dieser Schritt kann in der Regel auf zwei Arten ausgeführt werden: auf Betriebssystemebene oder (für bestimmte Sprachen) auf Anwendungsebene.

Sie können beispielsweise für .NET-Anwendungen den folgenden Codeausschnitt hinzufügen, um ein Zertifikat im PEM-Format, das im Pfad `certPath` gespeichert ist, als vertrauenswürdig einzustufen. In Abhängigkeit von der verwendeten Version des Skripts verweist der Pfad entweder auf `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) oder `RootCA.pem` (PowerShell).

   ```
   using System.Security.Cryptography.X509Certificates;
   
   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

Die Ausführung dieses Schritts auf Betriebssystemebene ist unter Windows und verschiedenen Linux-Distributionen unterschiedlich.

Im zweiten Schritt wird das IoT Hub-Geräte-SDK mit einer Verbindungszeichenfolge initialisiert, die auf den Hostnamen des Gatewaygeräts verweist.
Dazu wird die `GatewayHostName`-Eigenschaft an die Verbindungszeichenfolge des Geräts angefügt. Es folgt ein Beispiel für die Verbindungszeichenfolge eines Geräts mit angefügter `GatewayHostName`-Eigenschaft:

   ```
   HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com
   ```

Durch diese beiden Schritte ist es Ihrer Geräteanwendung möglich, eine Verbindung mit dem Gatewaygerät herzustellen.

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/modules-preview/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus