---
title: "Bereitstellen eines simulierten X.509-Geräts für Azure IoT Hub mithilfe von C | Microsoft-Dokumentation"
description: "Azure-Schnellstartanleitung: Erstellen und Bereitstellen eines simulierten X.509-Geräts mithilfe des C-Geräte-SDKs für den Azure IoT Hub Device Provisioning-Dienst"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 12/20/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 4d723a3b78a43d3b609d5a884591a92606ca11cc
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="create-and-provision-an-x509-simulated-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>Erstellen und Bereitstellen eines simulierten X.509-Geräts mithilfe des C-Geräte-SDKs für den IoT Hub Device Provisioning-Dienst
[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Mit diesen Schritten wird veranschaulicht, wie Sie ein X.509-Gerät auf Ihrem Entwicklungscomputer mit Windows-Betriebssystem simulieren und ein Codebeispiel verwenden, um für dieses simulierte Gerät eine Verbindung mit dem Device Provisioning-Dienst und Ihrem IoT Hub herzustellen. 

Führen Sie zunächst die Schritte unter [Set up the IoT Hub Device Provisioning Service (preview) with the Azure portal](./quick-setup-auto-provision.md) (Einrichten des IoT Hub Device Provisioning-Diensts (Vorschauversion) über das Azure-Portal) aus, bevor Sie mit diesem Artikel fortfahren.

<a id="setupdevbox"></a>

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung 

1. Vergewissern Sie sich, dass auf Ihrem Computer entweder Visual Studio 2015 oder [Visual Studio 2017](https://www.visualstudio.com/vs/) installiert ist. Für Ihre Visual Studio-Installation muss die Workload [Desktopentwicklung mit C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) aktiviert sein.

2. Laden Sie das [CMake-Buildsystem](https://cmake.org/download/) herunter, und installieren Sie es. Wichtig: Visual Studio mit der Workload „Desktopentwicklung mit C++“ muss **vor** der Installation von `cmake` auf dem Computer installiert sein. 

3. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Unter [Git-Clienttools von Software Freedom Conservancy](https://git-scm.com/download/) finden Sie die neueste Version der zu installierenden `git`-Tools. Hierzu zählt auch die Befehlszeilen-App **Git Bash**, über die Sie mit Ihrem lokalen Git-Repository interagieren können. 

4. Öffnen Sie eine Eingabeaufforderung oder Git Bash. Klonen Sie das GitHub-Repository mit dem Codebeispiel für die Gerätesimulation:
    
    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

5. Erstellen Sie in Ihrer lokalen Kopie des GitHub-Repositorys einen Ordner für den CMake-Buildprozess. 

    ```cmd
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

6. Führen Sie den folgenden Befehl aus, um die Visual Studio-Lösung für den Bereitstellungsclient zu erstellen.

    ```cmd
    cmake -Duse_prov_client:BOOL=ON ..
    ```
    
    Falls `cmake` Ihren C++-Compiler nicht findet, treten beim Ausführen des obigen Befehls unter Umständen Buildfehler auf. Führen Sie den Befehl in diesem Fall an der [Visual Studio-Eingabeaufforderung](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) aus. 


<a id="portalenroll"></a>

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Erstellen eines Geräteregistrierungseintrags im Device Provisioning-Dienst

1. Öffnen Sie die im Ordner *cmake* generierte Projektmappe `azure_iot_sdks.sln`, und erstellen Sie sie in Visual Studio.

2. Klicken Sie im Ordner **Provision\_Tools** mit der rechten Maustaste auf das Projekt **dice\_device\_enrollment**, und wählen Sie die Option **Als Startprojekt festlegen** aus. Führen Sie die Lösung aus. Geben Sie im Ausgabefenster **i** für die individuelle Registrierung ein, wenn die entsprechende Aufforderung angezeigt wird. Im Ausgabefenster wird ein lokal generiertes X.509-Zertifikat für Ihr simuliertes Gerät angezeigt. Kopieren Sie die Ausgabe zwischen *-----BEGIN CERTIFICATE-----* und dem ersten Vorkommen von *-----END CERTIFICATE-----* in die Zwischenablage, und schließen Sie dabei auch diese beiden Zeile mit ein. Sie benötigen nur das erste Zertifikat aus dem Ausgabefenster.
 
3. Erstellen Sie auf Ihrem Windows-Computer eine Datei mit dem Namen **_X509testcert.pem_**, öffnen Sie sie in einem Editor Ihrer Wahl, und kopieren Sie den Inhalt aus der Zwischenablage in diese Datei. Speichern Sie die Datei . 

4. Melden Sie sich am Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Provisioning-Dienst.

4. Öffnen Sie das Blatt **Registrierungen verwalten** für Ihren Dienst. Klicken Sie auf der Registerkarte **Individuelle Registrierungen** im oberen Bereich auf die Schaltfläche **Hinzufügen**. 

5. Geben Sie unter **Add enrollment list entry** (Registrierungslisteneintrag hinzufügen) die folgenden Informationen ein:
    - Wählen Sie **X.509** als *Mechanismus* für den Nachweis der Identität.
    - Wählen Sie unter *PEM- oder CER-Zertifikatsdatei* die im vorherigen Schritt erstellte Zertifikatsdatei **_X509testcert.pem_** über das Widget *Datei-Explorer* aus.
    - Optional können Sie die folgenden Informationen angeben:
        - Wählen Sie einen IoT Hub aus, der mit Ihrem Bereitstellungsdienst verknüpft ist.
        - Geben Sie eine eindeutige Geräte-ID ein. Achten Sie darauf, dass Sie beim Benennen Ihres Geräts keine sensiblen Daten angeben. 
        - Aktualisieren Sie **Initial device twin state** (Anfänglicher Gerätezwillingsstatus) mit der gewünschten Anfangskonfiguration für das Gerät.
    - Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

    ![Eingeben von Informationen zur X.509-Geräteregistrierung auf dem Portalblatt](./media/quick-create-simulated-device-x509/enter-device-enrollment.png)  

   Nach der erfolgreichen Registrierung wird Ihr X.509-Gerät als **riot-device-cert**auf der Registerkarte *Individuelle Registrierungen* in der Spalte *Registrierungs-ID* angezeigt. 



<a id="firstbootsequence"></a>

## <a name="simulate-first-boot-sequence-for-the-device"></a>Simulieren der ersten Startsequenz für das Gerät

1. Navigieren Sie im Azure-Portal zum Blatt **Übersicht** für Ihren Device Provisioning-Dienst, und notieren Sie sich den Wert unter **_ID-Bereich_**.

    ![Extrahieren der Endpunktinformationen des DP-Diensts aus dem Portalblatt](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Navigieren Sie in Visual Studio auf Ihrem Computer zum Beispielprojekt mit dem Namen **prov\_dev\_client\_sample** im Ordner **Provision\_Samples**, und öffnen Sie die Datei **prov\_dev\_client\_sample.c**.

3. Weisen Sie den Wert für _ID Scope_ (ID-Bereich) der Variablen `id_scope` zu. 

    ```c
    static const char* id_scope = "[ID Scope]";
    ```

4. In der gleichen Datei muss **SECURE_DEVICE_TYPE** in der Funktion **main()** auf „X.509“ festgelegt sein.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

   Kommentieren Sie die ggf. vorhandene Anweisung `hsm_type = SECURE_DEVICE_TYPE_TPM;` aus, oder löschen Sie sie. 

5. Klicken Sie mit der rechten Maustaste auf das Projekt **prov\_dev\_client\_sample**, und wählen Sie **Als Startprojekt festlegen** aus. Führen Sie das Beispiel aus. Beachten Sie die Nachrichten, die den Start und die Verbindungsherstellung des Geräts mit dem Device Provisioning-Dienst simulieren, um Ihre IoT Hub-Informationen abzurufen. Suchen Sie nach einer Nachricht mit einem Hinweis zur erfolgreichen Registrierung beim Hub: *Registration Information received from service: yourhuburl!* (Registrierungsinformationen vom Dienst empfangen: ihre_hub-url!). Schließen Sie das Fenster, wenn die Aufforderung angezeigt wird.

6. Navigieren Sie im Portal zu der mit Ihrem Bereitstellungsdienst verknüpften IoT Hub-Instanz, und öffnen Sie das Blatt **IoT-Geräte**. Nach erfolgreicher Bereitstellung des simulierten X.509-Geräts auf dem Hub wird die dazugehörige Geräte-ID auf dem Blatt **IoT-Geräte** angezeigt, und der *STATUS* lautet **Aktiviert**. Beachten Sie, dass Sie unter Umständen oben auf die Schaltfläche **Aktualisieren** klicken müssen, wenn Sie das Blatt bereits vor dem Ausführen der Beispielgeräteanwendung geöffnet haben. 

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/quick-create-simulated-device/hub-registration.png) 

    Wenn Sie den *anfänglichen Gerätezwillingsstatus* im Registrierungseintrag für Ihr Gerät gegenüber dem Standardwert geändert haben, kann der gewünschte Zwillingsstatus vom Hub abgerufen werden, und es können entsprechende Aktionen durchgeführt werden. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).


> [!IMPORTANT]
> Wenn Sie eine *Gruppenregistrierung* von X.509-Geräten durchführen möchten, können Sie die Schritte in dieser Schnellstartanleitung wie folgt abwandeln:
>    1. Konfigurieren Sie Ihren Windows-Computer so, dass anstelle der Standardoption **SChannel** die Bibliothek **OpenSSL** verwendet wird. Eine entsprechende Anleitung finden Sie unter [Set up a Windows development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#windows) (Einrichten einer Windows-Entwicklungsumgebung) im Abschnitt zu **WebSockets**. Bei Linux-Computern wird standardmäßig OpenSSL verwendet. 
>    2. Geben Sie in Schritt 2 ([Erstellen eines Geräteregistrierungseintrags im Device Provisioning-Dienst](#portalenroll)) des obigen Abschnitts **g** für „Gruppenregistrierung“ ein.
>    3. Wählen Sie in den Schritten 4 und 5 des [gleichen Abschnitts](#portalenroll) die Option **Registrierungsgruppen** aus, und geben Sie die erforderlichen Informationen für den Gruppeneintrag ein.  
>

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Geräteclientbeispiel weiter verwenden und erkunden möchten, überspringen Sie die Bereinigung der in dieser Schnellstartanleitung erstellten Ressourcen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle erstellten Ressourcen zu löschen, die im Rahmen dieser Schnellstartanleitung erstellt wurden:

1. Schließen Sie auf Ihrem Computer das Ausgabefenster des Geräteclientbeispiels.
1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihren Device Provisioning-Dienst aus. Öffnen Sie das Blatt **Registrierungen verwalten** für Ihren Dienst, und klicken Sie dann auf die Registerkarte **Individuelle Registrierungen**. Wählen Sie die *REGISTRIERUNGS-ID* des Geräts aus, das Sie mit dieser Schnellstartanleitung registriert haben, und klicken Sie im oberen Bereich auf die Schaltfläche **Löschen**. 
1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub-Instanz aus. Öffnen Sie das Blatt **IoT-Geräte** für Ihren Hub, wählen Sie die *GERÄTE-ID* des Geräts aus, die Sie in dieser Schnellstartanleitung registriert haben, und klicken Sie dann im oberen Bereich auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie auf Ihrem Windows-Computer ein simuliertes X.509-Gerät erstellt und es im Portal mithilfe des Azure IoT Hub Device Provisioning-Diensts für Ihre IoT Hub-Instanz bereitgestellt. Informationen zum programmgesteuerten Registrieren Ihres X.509-Geräts finden Sie in der Schnellstartanleitung für die programmgesteuerte Registrierung von X.509-Geräten. 

> [!div class="nextstepaction"]
> [Registrieren von X.509-Geräten für den Azure IoT Hub Device Provisioning-Dienst per Java-Dienst-SDK](quick-enroll-device-x509-java.md)
