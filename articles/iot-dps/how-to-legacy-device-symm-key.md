---
title: Verwenden von symmetrischen Schlüsseln zum Bereitstellen von Legacygeräten mit dem Azure IoT Hub Device Provisioning Service | Microsoft-Dokumentation
description: Verwenden von symmetrischen Schlüsseln zum Bereitstellen von Legacygeräten mit Ihrer Device Provisioning Service-Instanz
author: wesmc7777
ms.author: wesmc
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 9553d1dd5dd8d8ff11ea480618b471b9898985e3
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456557"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>Bereitstellen von Legacygeräten mit symmetrischen Schlüsseln


Ein häufiges Problem bei vielen Legacygeräten ist, dass diese oft über eine Identität verfügen, die aus nur einem Informationselement besteht. Bei diesen Identitätsinformationen handelt es sich normalerweise um eine MAC-Adresse oder eine Seriennummer. Legacygeräte verfügen unter Umständen nicht über ein Zertifikat, TPM oder ein anderes Sicherheitsfeature, das verwendet werden kann, um das Gerät sicher zu identifizieren. Der Device Provisioning Service für den IoT Hub enthält einen Vorgang zum Nachweisen symmetrischer Schlüssel. Der Nachweis des symmetrischen Schlüssels kann genutzt werden, um ein Gerät anhand von Informationen wie der MAC-Adresse oder der Seriennummer zu identifizieren.

Wenn Sie ohne größeren Aufwand ein [Hardwaresicherheitsmodul (HSM)](concepts-security.md#hardware-security-module) und ein Zertifikat installieren können, ist dies unter Umständen ein besserer Ansatz zum Identifizieren und Bereitstellen Ihrer Geräte. Bei diesem Ansatz können Sie nämlich ggf. die Aktualisierung des Codes umgehen, der auf Ihren Geräten bereitgestellt wird, und in Ihr Geräteimage ist kein geheimer Schlüssel eingebettet.

In diesem Artikel wird vorausgesetzt, dass weder ein HSM noch ein Zertifikat eine geeignete Option darstellen. Es wird aber davon ausgegangen, dass Sie über eine Methode zum Aktualisieren des Gerätecodes verfügen, um den Device Provisioning Service für die Bereitstellung dieser Geräte zu nutzen. 

Weiterhin wird in diesem Artikel davon ausgegangen, dass das Geräteupdate in einer sicheren Umgebung stattfindet, um den unberechtigten Zugriff auf den Hauptgruppenschlüssel oder den abgeleiteten Geräteschlüssel zu verhindern.

In diesem Artikel wird von der Nutzung einer Windows-Arbeitsstation ausgegangen. Allerdings können Sie die Verfahren auch unter Linux ausführen. Ein Beispiel für Linux finden Sie unter [Bereitstellen für Mehrinstanzenfähigkeit](how-to-provision-multitenant.md).


## <a name="overview"></a>Übersicht

Für jedes Gerät wird basierend auf Informationen, mit denen das Gerät identifiziert wird, eine eindeutige Registrierungs-ID definiert. Beispiele hierfür sind die MAC-Adresse oder eine Seriennummer.

Mit dem Device Provisioning Service wird eine Registrierungsgruppe erstellt, für die der [Nachweis symmetrischer Schlüssel](concepts-symmetric-key-attestation.md) verwendet wird. Die Registrierungsgruppe enthält einen Gruppenhauptschlüssel. Dieser Hauptschlüssel wird für die Hasherstellung für jede eindeutige Registrierungs-ID verwendet, um einen eindeutigen Geräteschlüssel für jedes Gerät zu erhalten. Das Gerät verwendet diesen abgeleiteten Geräteschlüssel mit seiner eindeutigen Registrierungs-ID, um gegenüber dem Device Provisioning Service den Nachweis zu erbringen und einem IoT Hub zugewiesen zu werden.

Der in diesem Artikel veranschaulichte Gerätecode basiert auf dem gleichen Muster wie unter [Schnellstart: Bereitstellen eines simulierten Geräts mit symmetrischen Schlüsseln](quick-create-simulated-device-symm-key.md). Mit diesem Code wird ein Gerät simuliert, indem ein Beispiel aus dem [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) genutzt wird. Wie in der Schnellstartanleitung gezeigt, verwendet das simulierte Gerät für den Nachweis keine einzelne Registrierung, sondern eine Registrierungsgruppe.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

* Durcharbeiten der Schnellstartanleitung [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md).
* Visual Studio 2015 oder [Visual Studio 2017](https://www.visualstudio.com/vs/) mit der aktivierten Workload ["Desktopentwicklung mit C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/).
* Die neueste Version von [Git](https://git-scm.com/download/) ist installiert.


## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Vorbereiten einer Azure IoT C SDK-Entwicklungsumgebung

In diesem Abschnitt bereiten Sie eine Entwicklungsumgebung vor, die zum Erstellen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) verwendet wird. 

Das SDK enthält den Beispielcode für das simulierte Gerät. Dieses simulierte Gerät versucht, während seiner Startsequenz die Bereitstellung durchzuführen.

1. Laden Sie Version 3.11.4 des [CMake-Buildsystems](https://cmake.org/download/) herunter. Überprüfen Sie die heruntergeladene Binärdatei mit dem entsprechenden kryptografischen Hashwert. Im folgenden Beispiel wird Windows PowerShell verwendet, um den kryptografischen Hash für Version 3.11.4 der x64 MSI-Verteilung zu überprüfen:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    Zum Zeitpunkt der Erstellung dieses Artikels wurden auf der Website von CMake die folgenden Hashwerte für Version 3.11.4 aufgeführt:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

    Wichtig: Die Voraussetzungen für Visual Studio (Visual Studio und die Workload „Desktopentwicklung mit C++“) müssen **vor** Beginn der Installation von `CMake` auf dem Computer installiert sein. Sobald die Voraussetzungen erfüllt sind und der Download überprüft wurde, installieren Sie das CMake-Buildsystem.

2. Öffnen Sie eine Eingabeaufforderung oder die Git Bash-Shell. Führen Sie den folgenden Befehl zum Klonen des Azure IoT C SDK-GitHub-Repositorys aus:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Die Größe dieses Repositorys beträgt derzeit ca. 220 MB. Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.


3. Erstellen Sie ein `cmake`-Unterverzeichnis im Stammverzeichnis des Git-Repositorys, und navigieren Sie zu diesem Ordner. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Erstellen Sie mithilfe des folgenden Befehls eine spezifische SDK-Version für Ihre Entwicklungsclientplattform. Im `cmake`-Verzeichnis wird eine Visual Studio-Projektmappe für das simulierte Gerät generiert. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON ..
    ```
    
    Falls `cmake` Ihren C++-Compiler nicht findet, treten beim Ausführen des obigen Befehls unter Umständen Buildfehler auf. Führen Sie den Befehl in diesem Fall an der [Visual Studio-Eingabeaufforderung](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) aus. 

    Nach erfolgreicher Erstellung ähneln die letzten Ausgabezeilen der folgenden Ausgabe:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-a-symmetric-key-enrollment-group"></a>Erstellen einer Registrierungsgruppe mit symmetrischem Schlüssel

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an, und öffnen Sie Ihre Instanz des Device Provisioning-Diensts.

2. Wählen Sie die Registerkarte **Registrierungen verwalten** aus, und klicken Sie dann oben auf der Seite auf die Schaltfläche **Registrierungsgruppe hinzufügen**. 

3. Geben Sie unter **Registrierungsgruppe hinzufügen** die folgenden Informationen ein, und klicken Sie auf die Schaltfläche **Speichern**.

    - **Gruppenname**: Geben Sie **mylegacydevices** ein.

    - **Nachweistyp**: Wählen Sie **Symmetrischer Schlüssel**.

    - **Schlüssel automatisch generieren**: Aktivieren Sie dieses Kontrollkästchen.

    - **Wählen Sie, wie Geräte den Hubs zugewiesen werden sollen**: Wählen Sie **Statische Konfiguration**, um das Zuweisen zu einem bestimmten Hub zu ermöglichen.

    - **Select the IoT hubs this group can be assigned to** (Wählen Sie die IoT-Hubs aus, denen diese Gruppe zugewiesen werden kann): Wählen Sie einen Ihrer Hubs aus.

    ![Hinzufügen einer Registrierungsgruppe für den Nachweis des symmetrischen Schlüssels](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. Nachdem Sie Ihre Registrierung gespeichert haben, werden der **Primärschlüssel** und **Sekundärschlüssel** generiert und dem Registrierungseintrag hinzugefügt. Ihre Registrierungsgruppe mit symmetrischem Schlüssel wird als **mylegacydevices** unter der Spalte *Gruppenname* auf der Registerkarte *Registrierungsgruppen* angezeigt. 

    Öffnen Sie die Registrierung, und kopieren Sie den Wert des von Ihnen generierten **Primärschlüssels**. Dieser Schlüssel ist Ihr Gruppenhauptschlüssel.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Auswählen einer eindeutigen Registrierungs-ID für das Gerät

Eine eindeutige Registrierungs-ID muss definiert werden, um jedes Gerät zu identifizieren. Sie können die MAC-Adresse, Seriennummer oder eindeutige Informationen vom Gerät verwenden. 

In diesem Beispiel wird eine Kombination aus MAC-Adresse und Seriennummer genutzt, um die folgende Zeichenfolge für eine Registrierungs-ID zu bilden.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Erstellen Sie eine eindeutige Registrierungs-ID für Ihr Gerät. Gültige Zeichen sind alphanumerische Kleinbuchstaben und Bindestriche („-“).


## <a name="derive-a-device-key"></a>Ableiten eines Geräteschlüssels 

Verwenden Sie für die Generierung des Geräteschlüssels den Gruppenhauptschlüssel, um einen [HMAC-SHA256](https://wikipedia.org/wiki/HMAC)-Wert für die eindeutige Registrierungs-ID für das Gerät zu berechnen und das Ergebnis in das Base64-Format zu konvertieren.

Fügen Sie Ihren Gruppenhauptschlüssel nicht in Ihren Gerätecode ein.


#### <a name="linux-workstations"></a>Linux-Arbeitsstationen

Wenn Sie eine Linux-Arbeitsstation verwenden, können Sie Ihren abgeleiteten Geräteschlüssel mit OpenSSL generieren, wie im folgenden Beispiel gezeigt.

Ersetzen Sie den Wert von **KEY** durch den **Primärschlüssel**, den Sie zuvor notiert haben.

Ersetzen Sie den Wert von **REG_ID** durch Ihre Registrierungs-ID.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Windows-Arbeitsstationen

Wenn Sie eine Windows-Arbeitsstation verwenden, können Sie die abgeleiteten Geräteschlüssel mit PowerShell generieren, wie im folgenden Beispiel gezeigt.

Ersetzen Sie den Wert von **KEY** durch den **Primärschlüssel**, den Sie zuvor notiert haben.

Ersetzen Sie den Wert von **REG_ID** durch Ihre Registrierungs-ID.

```PowerShell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```PowerShell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


Ihr Gerät verwendet den abgeleiteten Geräteschlüssel mit Ihrer eindeutigen Registrierungs-ID, um während der Bereitstellung den Nachweis des symmetrischen Schlüssels mit der Registrierungsgruppe durchzuführen.



## <a name="create-a-device-image-to-provision"></a>Erstellen eines Geräteimages für die Bereitstellung

In diesem Abschnitt aktualisieren Sie ein Bereitstellungsbeispiel mit dem Namen **prov\_dev\_client\_sample** im Azure IoT C SDK, das Sie zuvor eingerichtet haben. 

Dieser Beispielcode simuliert eine Gerätestartsequenz, von der die Bereitstellungsanforderung an die Instanz des Device Provisioning-Diensts gesendet wird. Die Startsequenz bewirkt, dass das Gerät erkannt und dem IoT-Hub zugewiesen wird, den Sie in der Registrierungsgruppe konfiguriert haben.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Übersicht** für Ihren Device Provisioning-Dienst, und notieren Sie sich den Wert unter **_ID-Bereich_**.

    ![Extrahieren von Informationen zum Device Provisioning Service-Endpunkt aus dem Portalblatt](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Öffnen Sie in Visual Studio die Projektmappendatei **azure_iot_sdks.sln**, die zuvor durch das Ausführen von CMake generiert wurde. Die Projektmappendatei befindet sich am folgenden Speicherort:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. Navigieren Sie im Visual Studio-Fenster *Projektmappen-Explorer* zum Ordner **Provision\_Samples**. Erweitern Sie das Beispielprojekt mit dem Namen **prov\_dev\_client\_sample**. Erweitern Sie **Quelldateien**, und öffnen Sie **prov\_dev\_client\_sample.c**.

4. Suchen Sie die Konstante `id_scope`, und ersetzen Sie den Wert durch Ihren **ID-Bereich**-Wert, den Sie zuvor kopiert haben. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Suchen Sie die Definition für die Funktion `main()` in der gleichen Datei. Stellen Sie sicher, dass die Variable `hsm_type` wie unten dargestellt auf `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` festgelegt ist:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Klicken Sie mit der rechten Maustaste auf das Projekt **prov\_dev\_client\_sample**, und wählen Sie **Als Startprojekt festlegen** aus. 

7. Navigieren Sie im *Projektmappen-Explorer* von Visual Studio zum Projekt **hsm\_security\_client**, und erweitern Sie es. Erweitern Sie **Quelldateien**, und öffnen Sie **hsm\_client\_key.c**. 

    Suchen Sie die Deklaration der Konstanten `REGISTRATION_NAME` und `SYMMETRIC_KEY_VALUE`. Nehmen Sie die folgenden Änderungen an der Datei vor, und speichern Sie sie.

    Aktualisieren Sie den Wert der Konstante `REGISTRATION_NAME` mit der **eindeutigen Registrierungs-ID für Ihr Gerät**.
    
    Aktualisieren Sie den Wert der Konstante `SYMMETRIC_KEY_VALUE` mit Ihrem **abgeleiteten Geräteschlüssel**.

    ```c
    static const char* const REGISTRATION_NAME = "sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6";
    static const char* const SYMMETRIC_KEY_VALUE = "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=";
    ```

7. Wählen Sie im Visual Studio-Menü die Option **Debuggen** > **Starten ohne Debugging** aus, um die Projektmappe auszuführen. Klicken Sie in der Eingabeaufforderung zum Neuerstellen des Projekts auf **Ja**, um das Projekt vor der Ausführung neu zu erstellen.

    Die folgende Ausgabe ist ein Beispiel für das erfolgreiche Starten des simulierten Geräts und das Herstellen der Verbindung mit der Provisioning Service-Instanz, die einem IoT Hub zugewiesen werden soll:

    ```cmd
    Provisioning API Version: 1.2.8

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: 
    test-docs-hub.azure-devices.net, deviceId: sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

    Press enter key to exit:
    ```

8. Navigieren Sie im Portal zu der IoT Hub-Instanz, der Ihr simuliertes Gerät zugewiesen wurde, und klicken Sie auf die Registerkarte **IoT-Geräte**. Nach erfolgreicher Bereitstellung des simulierten Geräts auf dem Hub wird die dazugehörige Geräte-ID auf dem Blatt **IoT-Geräte** angezeigt, und der *STATUS* lautet **Aktiviert**. Unter Umständen müssen Sie oben auf die Schaltfläche **Aktualisieren** klicken. 

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Sicherheitsaspekte

Beachten Sie, dass der abgeleitete Geräteschlüssel hierbei als Teil des Images vorliegt und dass dies nicht einer empfohlenen bewährten Sicherheitsmethode entspricht. Dies ist ein Grund, warum zwischen Sicherheit und Benutzerfreundlichkeit immer ein Kompromiss gefunden werden muss. 





## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur erneuten Bereitstellung finden Sie unter [IoT Hub Device-Konzepte für die erneute Bereitstellung](concepts-device-reprovision.md). 
* [Schnellstart: Bereitstellen eines simulierten Geräts mit symmetrischen Schlüsseln](quick-create-simulated-device-symm-key.md)
* Weitere Informationen zum Aufheben der Bereitstellung finden Sie unter [Aufheben der Bereitstellung von Geräten, die zuvor automatisch bereitgestellt wurden](how-to-unprovision-devices.md). 











