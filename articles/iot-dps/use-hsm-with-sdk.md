---
title: 'Azure-Anleitung: Verwenden verschiedener Hardwaresicherheitsmodule mit dem Gerätebereitstellungsdienst-Client-SDK in Azure'
description: 'Azure-Anleitung: Verwenden verschiedener Hardwaresicherheitsmodule mit dem Gerätebereitstellungsdienst-Client-SDK in Azure'
services: iot-dps
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 03/28/2018
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: ''
manager: ''
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 0d392f4a8d935cb37b6f4cfcd69826de58b33880
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk-for-c"></a>Verwenden verschiedener Hardwaresicherheitsmodule mit dem Device Provisioning-Dienst-Client-SDK für C

In diesem Artikel erfahren Sie, wie Sie verschiedene [Hardwaresicherheitsmodule (HSMs)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) mit dem Device Provisioning-Dienst-Client-SDK für C verwenden – entweder mit einem physischen Gerät oder mit einem Emulator. Der Bereitstellungsdienst unterstützt die Authentifizierung für zwei Arten von Nachweismechanismen: X**.**509 und TPM (Trusted Platform Module).

## <a name="prerequisites"></a>Voraussetzungen

Bereiten Sie Ihre Entwicklungsumgebung gemäß dem Abschnitt „Vorbereiten der Entwicklungsumgebung“ aus dem [Leitfaden zum Erstellen und Bereitstellen eines simulierten Geräts](./quick-create-simulated-device.md) vor.

### <a name="choose-a-hardware-security-module"></a>Auswählen eines Hardwaresicherheitsmoduls

Als Gerätehersteller müssen Sie zunächst Hardwaresicherheitsmodule (HSMs) auswählen, die auf einem der unterstützten Typen basieren. Das [Device Provisioning-Dienst-Client-SDK für C](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) unterstützt derzeit folgende HSMs: 

- [Trusted Platform Module (TPM):](https://en.wikipedia.org/wiki/Trusted_Platform_Module) TPM ist ein etablierter Standard für die meisten Windows-basierten Geräteplattformen sowie für einige Linux-/Ubuntu-basierte Geräte. Gerätehersteller können sich für dieses HSM entscheiden, wenn auf ihren Geräten eines der folgenden Betriebssysteme ausgeführt wird und sie nach einem etablierten Standard für HSMs suchen. Bei TPM-Chips können Sie nur jedes Gerät einzeln beim Device Provisioning-Dienst registrieren. Für Entwicklungszwecke können Sie den TPM-Simulator auf dem Windows- oder Linux-Entwicklungscomputer verwenden.

- [X.509:](https://cryptography.io/en/latest/x509/) Bei X.509-basierten HSMs handelt es sich um relativ neue Chips. Bei Microsoft wird auch an RIoT- oder DICE-Chips gearbeitet, die die X.509-Zertifikate implementieren. Mit X.509-Chips können Sie Massengeräteregistrierungen im Portal durchführen. Darüber hinaus werden bestimmte Windows-fremde Betriebssysteme wie mbed OS unterstützt. Für Entwicklungszwecke unterstützt das Client-SDK des Device Provisioning-Diensts einen X.509-Gerätesimulator. 

Weitere Informationen finden Sie unter [Sicherheitskonzepte beim IoT Hub Device Provisioning-Dienst](concepts-security.md). 

## <a name="enable-authentication-for-supported-hsms"></a>Aktivieren der Authentifizierung für unterstützte HSMs

Der Authentifizierungsmodus (X**.**509 oder TPM) muss für das physische Gerät oder den Simulator aktiviert werden, damit es bzw. er im Azure-Portal registriert werden kann. Navigieren Sie zum Stammordner für „azure-iot-sdk-c“. Führen Sie den entsprechenden Befehl für den gewünschten Authentifizierungsmodus aus:

### <a name="use-x509-with-simulator"></a>Verwenden von X**.**509 mit Simulator

Der Bereitstellungsdienst verfügt standardmäßig über einen DICE-Emulator (Device Identity Composition Engine), der ein X**.**509-Zertifikat für die Geräteauthentifizierung generiert. Führen Sie den folgenden Befehl aus, um die X**.**509-Authentifizierung zu aktivieren: 

```
cmake -Ddps_auth_type=x509 ..
```

Informationen zu Hardware mit DICE finden Sie [hier](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>Verwenden von X**.**509 mit Hardware

Der Bereitstellungsdienst kann mit X**.**509 für andere Hardware verwendet werden. Für die Verbindungsherstellung wird eine Schnittstelle zwischen der Hardware und dem SDK benötigt. Informationen zur Schnittstelle erhalten Sie vom HSM-Hersteller.

### <a name="use-tpm"></a>Verwenden von TPM

Der Bereitstellungsdienst kann eine Verbindung mit Windows- und Linux-basierten TPM-Hardwarechips mit SAS-Token herstellen. Führen Sie den folgenden Befehl aus, um die TPM-Authentifizierung zu aktivieren:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Verwenden von TPM mit Simulator

Wenn Sie über kein Gerät mit TPM-Chips verfügen, können Sie bei der Entwicklung unter dem Windows-Betriebssystem einen Simulator verwenden. Führen Sie den folgenden Befehl aus, um die TPM-Authentifizierung zu aktivieren und den TPM-Simulator auszuführen:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>Erstellen des SDKs
Das SDK muss vor dem Erstellen der Geräteregistrierung erstellt werden.

### <a name="linux"></a>Linux
- So erstellen Sie das SDK unter Linux:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Fügen Sie zum Erstellen von Debugbinärdateien dem Projektgenerierungsbefehl die entsprechende CMake-Option hinzu. Beispiel:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Für die SDK-Erstellung stehen zahlreiche [CMake-Konfigurationsoptionen](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html) zur Verfügung. So können Sie dem CMake-Projektgenerierungsbefehl beispielsweise ein Argument hinzufügen, um einen der verfügbaren Protokollstapel zu deaktivieren:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Sie können auch einen Komponententest erstellen und ausführen:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Wenn Sie das SDK unter Windows erstellen möchten, führen Sie die folgenden Schritte aus, um Projektdateien zu generieren:
    - Öffnen Sie eine Developer-Eingabeaufforderung für VS2015.
    - Führen Sie im Stammverzeichnis des Repositorys die folgenden CMake-Befehle aus:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    Dieser Befehl erstellt x86-Bibliotheken. Wenn Sie Bibliotheken für x64 erstellen möchten, passen Sie das cmake-Generatorargument an: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Nach erfolgreicher Projekterstellung enthält der Ordner `cmake` eine Visual Studio-Projektmappendatei (SLN-Datei). So erstellen Sie das SDK:
    - Öffnen Sie **cmake\azure_iot_sdks.sln** in Visual Studio, und erstellen Sie die Projektmappe. **ODER:**
    - Führen Sie an der Eingabeaufforderung, die Sie zum Generieren der Projektdateien verwendet haben, den folgenden Befehl aus:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Verwenden Sie zum Erstellen von Debugbinärdateien das entsprechende MSBuild-Argument: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Für die SDK-Erstellung stehen zahlreiche CMake-Konfigurationsoptionen zur Verfügung. So können Sie dem CMake-Projektgenerierungsbefehl beispielsweise ein Argument hinzufügen, um einen der verfügbaren Protokollstapel zu deaktivieren:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Sie können auch Komponententests erstellen und ausführen:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Einzubeziehende Bibliotheken
- Die folgenden Bibliotheken sollten in Ihrem SDK enthalten sein:
    - Der Bereitstellungsdienst: dps_http_transport, dps_client, dps_security_client
    - IoT Hub-Sicherheit: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-device-provisioning-services"></a>Erstellen eines Geräteregistrierungseintrags in Device Provisioning-Diensten

### <a name="tpm"></a>TPM
Gehen Sie bei Verwendung von TPM gemäß der Anleitung unter [Erstellen und Bereitstellen eines simulierten TPM-Geräts mithilfe des C-Geräte-SDKs für den IoT Hub Device Provisioning-Dienst](./quick-create-simulated-device.md) vor, um in Ihrem Device Provisioning-Dienst einen Geräteregistrierungseintrag zu erstellen und den ersten Start zu simulieren.

### <a name="x509"></a>X**.**509
1. Wenn Sie ein Gerät beim Bereitstellungsdienst registrieren möchten, müssen Sie sich den Endorsement Key und die Registrierungs-ID des jeweiligen Geräts notieren. Die entsprechenden Angaben finden Sie im Bereitstellungstool des Client-SDKs. Führen Sie den folgenden Befehl aus, um das Zertifikat der Stammzertifizierungsstelle (für Registrierungsgruppen) und das Signaturgeberzertifikat (für die individuelle Registrierung) auszugeben:
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Gerätebereitstellungsdienst.
   - X**.**509, individuelle Registrierung: Wählen Sie auf dem Zusammenfassungsblatt des Bereitstellungsdiensts die Option **Registrierungen verwalten** aus. Klicken Sie auf der Registerkarte **Individual Enrollments** (Individuelle Registrierungen) im oberen Bereich auf die Schaltfläche **Hinzufügen**. Wählen Sie als *Mechanismus* für den Identitätsnachweis die Option **X**.**509** aus, und laden Sie das Signaturgeberzertifikat gemäß den Anweisungen auf dem Blatt hoch. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 
   - X**.**509, Gruppenregistrierung: Wählen Sie auf dem Zusammenfassungsblatt des Bereitstellungsdiensts die Option **Registrierungen verwalten** aus. Klicken Sie auf der Registerkarte **Group Enrollments** (Gruppenregistrierungen) im oberen Bereich auf die Schaltfläche **Hinzufügen**. Wählen Sie als *Mechanismus* für den Identitätsnachweis die Option **X**.**509** aus, geben Sie einen Gruppen- und einen Zertifizierungsnamen ein, und laden Sie das Zertifikat der Stammzertifizierungsstelle gemäß den Anweisungen auf dem Blatt hoch. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

## <a name="enable-authentication-for-custom-tpm-and-x509-devices-optional"></a>Aktivieren der Authentifizierung für benutzerdefinierte TPM- und X.509-Geräte (optional)

> [!NOTE]
> Dieser Abschnitt ist nur für Geräte relevant, die Unterstützung für eine benutzerdefinierte Plattform oder für ein benutzerdefiniertes HSM benötigen, die bzw. das derzeit nicht vom Device Provisioning-Dienst-Client-SDK für C unterstützt wird.

Entwickeln Sie zunächst ein Repository und eine Bibliothek für Ihr benutzerdefiniertes HSM:

1. Entwickeln Sie eine Bibliothek für den Zugriff auf Ihr HSM. Bei diesem Projekt muss eine statische Bibliothek zur Verwendung für das Device Provisioning-Dienst-SDK erzeugt werden.

2. Implementieren Sie in Ihrer Bibliothek die in der folgenden Headerdatei definierten Funktionen: 

    - Benutzerdefiniertes TPM: Implementieren Sie die benutzerdefinierten HSM-Funktionen, die unter [HSM TPM API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api) definiert sind.  
    - Benutzerdefiniertes X.509: Implementieren Sie die benutzerdefinierten HSM-Funktionen, die unter [HSM X509 API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api) definiert sind. 

Nach der erfolgreichen eigenständigen Erstellung Ihrer Bibliothek muss sie per Verknüpfung in das Device Provisioning-Dienst-Client-SDK integriert werden. :

1. Geben Sie das benutzerdefinierte HSM-GitHub-Repository, den Bibliothekspfad und den dazugehörigen Namen in den folgenden `cmake`-Befehl ein:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
2. Öffnen Sie die mit „cmake“ erstellte Visual Studio-Projektmappendatei (`\azure-iot-sdk-c\cmake\azure_iot_sdks.sln`), und erstellen Sie sie. 

    - Im Rahmen des Buildprozesses wird die SDK-Bibliothek kompiliert.
    - Das SDK versucht, eine Verknüpfung mit dem benutzerdefinierten HSM herzustellen, das im `cmake`-Befehl definiert ist.

3. Vergewissern Sie sich durch Ausführen der Beispiel-App „prov_dev_client_ll_sample“ (unter `\azure-iot-sdk-c\cmake\provisioning_client\samples\prov_dev_client_ll_sample`), dass Ihr HSM ordnungsgemäß implementiert wurde.

## <a name="connecting-to-iot-hub-after-provisioning"></a>Herstellen einer Verbindung mit IoT Hub nach der Bereitstellung

Nachdem das Gerät mit dem Bereitstellungsdienst bereitgestellt wurde, verwendet die folgende API den HSM-Authentifizierungsmodus, um eine Verbindung mit IoT Hub herzustellen: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```

