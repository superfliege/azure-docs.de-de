---
title: Erstellen eines transparenten Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden eines Azure IoT Edge-Geräts als transparentes Gateway, das Informationen von nachgeschalteten Geräten verarbeiten kann
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a42f4ce85214ad2a8c5692736b7d36101ccb62ed
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556219"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Konfigurieren eines IoT Edge-Geräts als transparentes Gateway

Dieser Artikel enthält detaillierte Anweisungen zur Konfiguration von IoT Edge-Geräten, die als transparentes Gateway für andere Geräte zur Kommunikation mit IoT Hub dienen. In diesem Artikel bezieht sich der Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät, das als transparentes Gateway verwendet wird. Ausführlichere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway](./iot-edge-as-gateway.md), das einen konzeptionellen Überblick bietet.

>[!NOTE]
>Derzeit gilt Folgendes:
> * Wenn das Gateway vom IoT Hub getrennt wird, können sich nachgeschaltete Geräte nicht am Gateway authentifizieren.
> * Edge-fähige Geräte können keine Verbindung mit IoT Edge-Gateways herstellen. 
> * Nachgeschaltete Geräte können keinen Dateiupload verwenden.

Damit ein Gerät als Gateway fungieren kann, muss es in der Lage sein, sich sicher mit nachgeschalteten Geräten zu verbinden. Mit Azure IoT Edge können Sie mithilfe der Public Key-Infrastruktur sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des Edge-Geräts bestätigen, da Ihre Geräte nur Verbindungen mit Ihren Gateways und nicht mit potenziell schädlichen Gateways herstellen sollen.

Ein nachgeschaltetes Gerät kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub)-Clouddienst erstellt wurde. Oft verwenden diese Anwendungen das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). In der Praxis kann ein nachgeschaltetes Gerät sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird. 

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von der gleichen Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Sicherheit](../iot-hub/iot-hub-x509ca-overview.md) in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT Hub zugeordnet (der IoT Hub-Besitzerzertifizierungsstelle), und es ist eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, sowie eine Zertifizierungsstelle für die IoT Edge-Geräte vorhanden.

![Gatewayzertifikateinrichtung](./media/how-to-create-transparent-gateway/gateway-setup.png)

Das Gateway legt dem nachgeschalteten Gerät während der Initiierung der Verbindung das Zertifizierungsstellenzertifikat des eigenen Edge-Geräts vor. Die nachgeschaltete Gerät überprüft, ob das Zertifizierungsstellenzertifikat des Edge-Geräts mit dem Zertifizierungsstellenzertifikat des Besitzers signiert wurde. Durch diesen Vorgang kann das nachgeschaltete Gerät bestätigen, dass das Gateway aus einer vertrauenswürdigen Quelle stammt.

In den folgenden Schritten werden Sie durch den Prozess zum Erstellen der Zertifikate und zum Installieren an den richtigen Orten geführt.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure IoT Edge-Gerät, das als Gateway konfiguriert wurde. Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als IoT Edge-Gerät verwenden. Hier finden Sie die Anweisungen für die folgenden Betriebssysteme:
* [Windows](./how-to-install-iot-edge-windows.md)
* [Linux x64](./how-to-install-iot-edge-linux.md)
* [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

Sie können mit jedem beliebigen Computer Zertifikate generieren und sie dann auf Ihr IoT Edge-Gerät kopieren. 

## <a name="generate-certificates-with-windows"></a>Generieren von Zertifikaten unter Windows

Verwenden Sie die Schritte in diesem Abschnitt, um Testzertifikate auf einem Windows-Gerät zu erstellen. Sie können die Zertifikate auf Ihrem IoT Edge-Gerät selbst generieren oder einen separaten Computer verwenden und die endgültigen Zertifikate auf ein beliebiges IoT Edge-Gerät mit einem unterstützten Betriebssystem kopieren. 

Die in diesem Abschnitt generierten Zertifikate sind nur für Testzwecke bestimmt. 

### <a name="install-openssl"></a>Installieren von OpenSSL

Installieren Sie OpenSSL für Windows auf dem Computer, den Sie zum Generieren der Zertifikate verwenden. Es gibt zwei Möglichkeiten für die Installation von OpenSSL:

   >[!NOTE]
   >Wenn Sie OpenSSL bereits auf Ihrem Windows-Gerät installiert haben, können Sie diesen Schritt überspringen. Stellen Sie jedoch sicher, dass „openssl.exe“ in Ihrer PATH-Umgebungsvariablen verfügbar ist.

* **Einfacher:** Laden Sie beliebige Binärdateien von OpenSSL-Drittanbietern herunter und installieren diese, z.B. von [diesem Projekt auf SourceForge](https://sourceforge.net/projects/openssl/). Fügen Sie „openssl.exe“ den vollständigen Pfad zu Ihrer PATH-Umgebungsvariablen hinzu. 
   
* **Empfohlen.** Laden Sie den OpenSSL-Quellcode herunter, und erstellen Sie die Binärdateien entweder selbst oder mithilfe von [vcpkg](https://github.com/Microsoft/vcpkg) auf Ihrem Computer. In den nachfolgend aufgeführten Anweisungen wird in benutzerfreundlichen Schritten „vcpkg“ zum Herunterladen des Quellcodes sowie zum Kompilieren und Installieren von OpenSSL auf Ihrem Windows-Computer verwendet.

   1. Navigieren Sie zu einem Verzeichnis, in dem Sie „vcpkg“ installieren möchten. Wir bezeichnen das Verzeichnis als *\<VCPKGDIR>*. Folgen Sie den Anweisungen zum Herunterladen und Installieren von [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Führen Sie nach der Installation von „vcpkg“ über eine PowerShell-Eingabeaufforderung den folgenden Befehl für die Installation des OpenSSL-Pakets für Windows x64 aus. Diese Installation dauert in der Regel 5 Minuten.

      ```PowerShell
      .\vcpkg install openssl:x64-windows
      ```
   3. Fügen Sie `<VCPKGDIR>\installed\x64-windows\tools\openssl` Ihrer PATH-Umgebungsvariablen hinzu, damit die Datei „openssl.exe“ aufgerufen werden kann.

### <a name="prepare-creation-scripts"></a>Vorbereiten von Erstellungsskripts

Das Azure IoT-Geräte-SDK für C enthält Skripts, mit denen Sie Testzertifikate erstellen können. In diesem Abschnitt klonen Sie das SDK und konfigurieren PowerShell.

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus. 

2. Klonen Sie das Git-Repository, das Skripts zum Generieren von Zertifikaten enthält, die nicht in der Produktion eingesetzt werden. Diese Skripts helfen Ihnen dabei, die erforderlichen Zertifikate zum Einrichten eines transparenten Gateways zu erstellen. Verwenden Sie den Befehl `git clone`, oder [laden Sie die ZIP-Datei herunter](https://github.com/Azure/azure-iot-sdk-c/archive/master.zip). 

   ```PowerShell
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Navigieren Sie zu dem Verzeichnis, in dem Sie arbeiten möchten. Wir bezeichnen das Verzeichnis als *\<WRKDIR>*.  Alle Dateien werden in diesem Verzeichnis erstellt.

4. Kopieren Sie die Konfigurations- und Skriptdateien in Ihr Arbeitsverzeichnis. 

   ```PowerShell
   copy <path>\azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy <path>\azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   ```

   Wenn Sie das SDK als ZIP-Datei heruntergeladen haben, lautet der Ordnername `azure-iot-sdk-c-master` und der Rest des Pfads ist identisch. 

5. Legen Sie für die Umgebungsvariable „OPENSSL_CONF“ die Verwendung der Konfigurationsdatei „openssl_root_ca.cnf“ fest.

    ```PowerShell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```

6. Aktivieren Sie PowerShell zum Ausführen der Skripts.

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Fügen Sie die Funktionen, die von den Skripts verwendet werden, in den globalen Namespace von PowerShell ein.
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

8. Überprüfen Sie, ob OpenSSL richtig installiert wurde und keine Namenskonflikte mit vorhandenen Zertifikaten entstehen. Wenn Probleme auftreten, müsste das Skript Informationen darüber enthalten, wie diese auf Ihrem System behoben werden können.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Erstellen von Zertifikaten

In diesem Abschnitt erstellen Sie drei Zertifikate und verbinden sie dann als Kette. Wenn Sie die Zertifikate in einer Kettendatei platzieren, können Sie sie einfach auf Ihrem IoT Edge-Gatewaygerät und auf beliebigen nachgeschalteten Geräten installieren.  

1. Erstellen Sie das Besitzer-Zertifizierungsstellenzertifikat, das ein Zwischenzertifikat signiert. Die Zertifikate werden alle in *\<WRKDIR>* platziert.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

2. Erstellen Sie das Zertifizierungsstellenzertifikat und den privaten Schlüssel für das Edge-Gerät mit dem folgenden Befehl. Geben Sie einen Namen für das Gatewaygerät an, das zur Benennung der Dateien und bei der Zertifikatsgenerierung verwendet wird. 

   ```PowerShell
   New-CACertsEdgeDevice "<gateway name>"
   ```

3. Erstellen Sie mit dem folgenden Befehl eine Zertifikatkette aus dem Besitzer-Zertifizierungsstellenzertifikat, dem Zwischenzertifikat und dem Zertifizierungsstellenzertifikat des Edge-Geräts. 

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway name>"
   ```

   Das Skript erstellt die folgenden Zertifikate und Schlüssel:
   * `<WRKDIR>\certs\new-edge-device.*`
   * `<WRKDIR>\private\new-edge-device.key.pem`
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="generate-certificates-with-linux"></a>Generieren von Zertifikaten unter Linux

Verwenden Sie die Schritte in diesem Abschnitt, um Testzertifikate auf einem Linux-Gerät zu erstellen. Sie können die Zertifikate auf Ihrem IoT Edge-Gerät selbst generieren oder einen separaten Computer verwenden und die endgültigen Zertifikate auf ein beliebiges IoT Edge-Gerät mit einem unterstützten Betriebssystem kopieren. 

### <a name="prepare-creation-scripts"></a>Vorbereiten von Erstellungsskripts

1. Klonen Sie das Git-Repository, das Skripts zum Generieren von Zertifikaten enthält, die nicht in der Produktion eingesetzt werden. Diese Skripts helfen Ihnen dabei, die erforderlichen Zertifikate zum Einrichten eines transparenten Gateways zu erstellen. 

   ```bash
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

2. Navigieren Sie zu dem Verzeichnis, in dem Sie arbeiten möchten. Wir bezeichnen das Verzeichnis als *\<WRKDIR>*.  Alle Dateien werden in diesem Verzeichnis erstellt.
  
3. Kopieren Sie die Konfigurations- und Skriptdateien in Ihr Arbeitsverzeichnis.

   ```bash
   cp <path>/azure-iot-sdk-c/tools/CACertificates/*.cnf .
   cp <path>/azure-iot-sdk-c/tools/CACertificates/certGen.sh .
   ```

4. Konfigurieren Sie OpenSSL, um Zertifikate mithilfe des bereitgestellten Skripts zu generieren. 

   ```bash
   chmod 700 certGen.sh 
   ```

### <a name="create-certificates"></a>Erstellen von Zertifikaten

In diesem Abschnitt erstellen Sie drei Zertifikate und verbinden sie dann als Kette. Wenn Sie die Zertifikate in einer Kettendatei platzieren, können Sie sie einfacher auf Ihrem IoT Edge-Gatewaygerät und auf allen nachgeschalteten Geräten installieren.  

1.  Erstellen Sie das Zertifizierungsstellenzertifikat für den Eigentümer und ein Zwischenzertifikat. Die Zertifikate werden alle in *\<WRKDIR>* platziert.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Das Skript erstellt die folgenden Zertifikate und Schlüssel:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>/certs/azure-iot-test-only.intermediate.cert.pem`
   * `<WRKDIR>/private/azure-iot-test-only.root.ca.key.pem`
   * `<WRKDIR>/private/azure-iot-test-only.intermediate.key.pem`

2.  Erstellen Sie das Zertifizierungsstellenzertifikat und den privaten Schlüssel für das Edge-Gerät mit dem folgenden Befehl. Geben Sie einen Namen für das Gatewaygerät an, das zur Benennung der Dateien und bei der Zertifikatsgenerierung verwendet wird. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway name>"
   ```

   Das Skript erstellt die folgenden Zertifikate und Schlüssel:
   * `<WRKDIR>/certs/new-edge-device.*`
   * `<WRKDIR>/private/new-edge-device.key.pem`

3. Erstellen Sie eine Zertifikatkette mit dem Namen **new-edge-device-full-chain.cert.pem** aus dem Besitzer-Zertifizierungsstellenzertifikat, dem Zwischenzertifikat und dem Zertifizierungsstellenzertifikat des Edge-Geräts.

   ```bash
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="install-certificates-on-the-gateway"></a>Installieren von Zertifikaten auf dem Gateway

Nachdem Sie nun eine Zertifikatkette erstellt haben, müssen Sie diese auf dem IoT Edge-Gateway installieren und die IoT Edge-Runtime so konfigurieren, dass sie auf die neuen Zertifikate verweist. 

1. Kopieren Sie die folgenden Dateien aus *\<WRKDIR>*. Speichern Sie sie auf Ihrem IoT Edge-Gerät. Wir bezeichnen das Zielverzeichnis auf Ihrem IoT Edge-Gerät als *\<CERTDIR>*. 

   Wenn Sie die Zertifikate auf dem Edge-Gerät selbst erstellt haben, können Sie diesen Schritt überspringen und den Pfad für das Arbeitsverzeichnis verwenden.

   * Zertifizierungsstellenzertifikat des Geräts – `<WRKDIR>\certs\new-edge-device-full-chain.cert.pem`
   * Privater Zertifizierungsstellenschlüssel des Geräts – `<WRKDIR>\private\new-edge-device.key.pem`
   * Zertifizierungsstelle des Besitzers – `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. Öffnen Sie die Konfigurationsdatei des IoT Edge-Sicherheitsdaemons. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Legen Sie die Eigenschaften des **Zertifikats** in der Datei „config.yaml“ auf den Pfad fest, in dem Sie die Zertifikat- und Schlüsseldateien für das IoT Edge-Gerät platziert haben.

```yaml
certificates:
  device_ca_cert: "<CERTDIR>\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "<CERTDIR>\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Bereitstellen von Edge Hub für das Gateway

Wenn Sie IoT Edge zum ersten Mal auf einem Gerät installieren, startet nur ein Systemmodul automatisch: der Edge-Agent. Damit Ihr Gerät als Gateway fungieren kann, benötigen Sie beide Systemmodule. Wenn Sie noch keine Module auf Ihrem Gatewaygerät bereitgestellt haben, erstellen Sie eine Bereitstellung für Ihr Gerät, um das zweite Systemmodul zu starten: den Edge-Hub. Die Bereitstellung sieht leer aus, weil Sie keine Module im Assistenten hinzufügen, doch sie wird beide Systemmodule bereitstellen. 

Mit dem Befehl `iotedge list` können Sie überprüfen, welche Module auf einem Gerät ausgeführt werden.

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.

2. Wechseln Sie zu **IoT Edge**, und wählen Sie das IoT Edge-Gerät aus, das Sie als Gateway verwenden möchten.

3. Wählen Sie **Module festlegen** aus.

4. Klicken Sie auf **Weiter**.

5. Auf der Seite **Routen angeben** sollte eine Standardroute aufgeführt sein, über die alle Nachrichten von allen Modulen an IoT Hub gesendet werden. Ist das nicht der Fall, können Sie den folgenden Code eingeben und dann **Weiter** auswählen.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. Wählen Sie auf der Seite **Vorlage überprüfen** die Option **Senden** aus.

## <a name="route-messages-from-downstream-devices"></a>Weiterleiten von Nachrichten von nachgeschalteten Geräten
Die IoT Edge-Runtime kann Nachrichten für nachgeschaltete Geräte genau wie Nachrichten von Modulen weiterleiten. Dadurch können Sie die Analyse in einem auf dem Gateway ausgeführten Modul durchführen, bevor Daten an die Cloud gesendet werden. 

Zurzeit werden von nachgeschalteten Geräten gesendete Nachrichten weitergeleitet, indem sie von den von Modulen gesendeten Nachrichten unterschieden werden. Im Gegensatz zu Nachrichten, die von nachgeschalteten Geräten gesendet werden, enthalten von Modulen gesendete Nachrichten eine Systemeigenschaft namens **connectionModuleId**. Sie können die WHERE-Klausel der Route verwenden, um Nachrichten mit dieser Systemeigenschaft auszuschließen. 

Die nachstehende Route wird zum Senden von Nachrichten von einem nachgeschalteten Gerät an ein Modul mit dem Namen `ai_insights` verwendet.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Weitere Informationen zum Routing von Nachrichten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen](./module-composition.md#declare-routes).

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Nächste Schritte

Da nun ein IoT Edge-Gerät als transparentes Gateway fungiert, müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gateway vertrauen und Nachrichten daran senden. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).
