---
title: "Bereitstellen eines simulierten X.509-Geräts für Azure IoT Hub mithilfe von Java | Microsoft-Dokumentation"
description: "Azure-Schnellstartanleitung: Erstellen und Bereitstellen eines simulierten X.509-Geräts mithilfe des Java-Geräte-SDKs für den IoT Hub Device Provisioning-Dienst"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 12/21/2017
ms.topic: quickstart
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: d966a1ce5f30531668c05e68bfe709057c6dee35
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-sdk-for-iot-hub-device-provisioning-service"></a>Erstellen und Bereitstellen eines simulierten X.509-Geräts mithilfe des Java-Geräte-SDKs für den IoT Hub Device Provisioning-Dienst
[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Mit diesen Schritten wird veranschaulicht, wie Sie ein X.509-Gerät auf Ihrem Entwicklungscomputer mit Windows-Betriebssystem simulieren und ein Codebeispiel verwenden, um für dieses simulierte Gerät eine Verbindung mit dem Device Provisioning-Dienst und Ihrem IoT Hub herzustellen. 

Führen Sie zunächst die Schritte unter [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md) aus, bevor Sie mit dieser Anleitung fortfahren.


## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung 

1. Vergewissern Sie sich, dass das [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) auf Ihrem Computer installiert ist.

1. Laden Sie [Maven](https://maven.apache.org/install.html) herunter, und installieren Sie es.

1. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Unter [Git-Clienttools von Software Freedom Conservancy](https://git-scm.com/download/) finden Sie die neueste Version der zu installierenden `git`-Tools. Hierzu zählt auch die Befehlszeilen-App **Git Bash**, über die Sie mit Ihrem lokalen Git-Repository interagieren können. 

1. Öffnen Sie eine Eingabeaufforderung. Klonen Sie das GitHub-Repository mit dem Codebeispiel für die Gerätesimulation:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Navigieren Sie zum Zertifikatgeneratorprojekt, und erstellen Sie es. 

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator
    mvn clean install
    ```

1. Navigieren Sie zum Zielordner, und führen Sie die erstellte JAR-Datei aus.

    ```cmd/sh
    cd target
    java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
    ```

1. Verwenden Sie abhängig von Ihrer Einrichtung eine der folgenden Methoden, um die Registrierungsinformationen zu erstellen:

    - **Individuelle Registrierung:**

        1. Beantworten Sie die Frage _Do you want to input common name_ (Möchten Sie einen allgemeinen Namen eingeben?) mit **N**. Kopieren Sie die Ausgabe für `Client Cert` von *-----BEGIN CERTIFICATE-----* bis *-----END CERTIFICATE-----* in die Zwischenablage.

            ![Generator für individuelles Zertifikat](./media/java-quick-create-simulated-device-x509/individual.png)

        1. Erstellen Sie auf Ihrem Windows-Computer eine Datei mit dem Namen **_X509individual.pem_**, öffnen Sie sie in einem Editor Ihrer Wahl, und kopieren Sie den Inhalt der Zwischenablage in diese Datei. Speichern Sie die Datei .

        1. Beantworten Sie die Frage _Do you want to input Verification Code_ (Möchten Sie einen Prüfcode eingeben?) mit **N**, und lassen Sie die Programmausgabe für später geöffnet. Notieren Sie sich die Werte _Client Cert_ und _Client Cert Private Key_.
    
    - **Registrierungsgruppen:**

        1. Beantworten Sie die Frage _Do you want to input common name_ (Möchten Sie einen allgemeinen Namen eingeben?) mit **N**. Kopieren Sie die Ausgabe für `Root Cert` von *-----BEGIN CERTIFICATE-----* bis *-----END CERTIFICATE-----* in die Zwischenablage.

            ![Generator für Gruppenzertifikat](./media/java-quick-create-simulated-device-x509/group.png)

        1. Erstellen Sie auf Ihrem Windows-Computer eine Datei mit dem Namen **_X509group.pem_**, öffnen Sie sie in einem Editor Ihrer Wahl, und kopieren Sie den Inhalt aus der Zwischenablage in diese Datei. Speichern Sie die Datei .

        1. Beantworten Sie die Frage _Do you want to input Verification Code_ (Möchten Sie einen Prüfcode eingeben?) mit **Y**, und lassen Sie das Programm für später geöffnet. Notieren Sie sich die Werte _Client Cert_, _Client Cert Private Key_, _Signer Cert_ und _Root Cert_.


## <a name="create-a-device-enrollment-entry"></a>Erstellen eines Geräteregistrierungseintrags

1. Melden Sie sich am Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Provisioning-Dienst.

1. Geben Sie die Registrierungsinformationen abhängig von Ihrer Einrichtung auf eine der folgenden Arten ein:

    - **Individuelle Registrierung:** 

        1. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Registrierungen verwalten** aus. Klicken Sie auf der Registerkarte **Individual Enrollments** (Individuelle Registrierungen) im oberen Bereich auf die Schaltfläche **Hinzufügen**. 

        1. Geben Sie unter **Add enrollment list entry** (Registrierungslisteneintrag hinzufügen) die folgenden Informationen ein:
            - Wählen Sie **X.509** als *Mechanismus* für den Nachweis der Identität.
            - Wählen Sie unter *PEM- oder CER-Zertifikatsdatei* die im vorherigen Schritt erstellte Zertifikatsdatei **_X509individual.pem_** über das Widget *Datei-Explorer* aus.
            - Optional können Sie die folgenden Informationen angeben:
                - Wählen Sie einen IoT Hub aus, der mit Ihrem Bereitstellungsdienst verknüpft ist.
                - Geben Sie eine eindeutige Geräte-ID ein. Achten Sie darauf, dass Sie beim Benennen Ihres Geräts keine sensiblen Daten angeben. 
                - Aktualisieren Sie **Initial device twin state** (Anfänglicher Gerätezwillingsstatus) mit der gewünschten Anfangskonfiguration für das Gerät.
            - Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

        ![Eingeben von Informationen zur X.509-Geräteregistrierung auf dem Portalblatt](./media/java-quick-create-simulated-device-x509/enter-device-enrollment.png)  

       Nach der Registrierung wird Ihr X.509-Gerät auf der Registerkarte *Individuelle Registrierungen* in der Spalte *Registrierungs-ID* als **microsoftriotcore** angezeigt. 

    - **Registrierungsgruppen:** 

        1. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Zertifikate** aus, und klicken Sie anschließend im oberen Bereich auf die Schaltfläche **Hinzufügen**.

        1. Geben Sie unter **Zertifikat hinzufügen** die folgenden Informationen ein:
            - Geben Sie einen eindeutigen Zertifikatnamen ein.
            - Wählen Sie die zuvor erstellte Datei **_X509group.pem_** aus.
            - Klicken Sie abschließend auf die Schaltfläche **Speichern**.

        ![Hinzufügen des Zertifikats](./media/java-quick-create-simulated-device-x509/add-certificate.png)

        1. Wählen Sie das neu erstellte Zertifikat aus:
            - Klicken Sie auf **Prüfcode generieren**. Kopieren Sie den generierten Code.
            - Geben Sie den _Prüfcode_ ein, oder klicken Sie mit der rechten Maustaste, um ihn in das aktive Fenster _provisioning-x509-cert-generator_ einzufügen.  Drücken Sie die **EINGABETASTE**.
            - Kopieren Sie die Ausgabe für `Verification Cert` von *-----BEGIN CERTIFICATE-----* bis *-----END CERTIFICATE-----* in die Zwischenablage.
            
                ![Prüfcodegenerator](./media/java-quick-create-simulated-device-x509/validation-generator.png)

            - Erstellen Sie auf Ihrem Windows-Computer eine Datei mit dem Namen **_X509validation.pem_**, öffnen Sie sie in einem Editor Ihrer Wahl, und kopieren Sie den Inhalt aus der Zwischenablage in diese Datei. Speichern Sie die Datei .
            - Wählen Sie im Azure-Portal die Datei **_X509validation.pem_** aus. Klicken Sie auf **Überprüfen**.

            ![Überprüfen des Zertifikats](./media/java-quick-create-simulated-device-x509/validate-certificate.png)

        1. Klicken Sie auf **Registrierungen verwalten**. Klicken Sie auf der Registerkarte **Registrierungsgruppen** im oberen Bereich auf die Schaltfläche **Hinzufügen**.
            - Geben Sie einen eindeutigen Gruppennamen ein.
            - Wählen Sie den eindeutigen Zertifikatnamen aus, den Sie zuvor erstellt haben.
            - Optional können Sie die folgenden Informationen angeben:
                - Wählen Sie einen IoT Hub aus, der mit Ihrem Bereitstellungsdienst verknüpft ist.
                - Aktualisieren Sie **Initial device twin state** (Anfänglicher Gerätezwillingsstatus) mit der gewünschten Anfangskonfiguration für das Gerät.

        ![Eingeben von Informationen zur X.509-Gruppenregistrierung auf dem Portalblatt](./media/java-quick-create-simulated-device-x509/enter-group-enrollment.png)

        Nach erfolgreicher Registrierung wird die X.509-Gerätegruppe auf der Registerkarte *Registrierungsgruppen* in der Spalte *Gruppenname* angezeigt.


## <a name="simulate-the-device"></a>Simulieren des Geräts

1. Klicken Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts auf **Übersicht**, und notieren Sie sich den _ID-Bereich_ und den _globalen Endpunkt für den Bereitstellungsdienst_.

    ![Dienstinformationen](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

1. Öffnen Sie eine Eingabeaufforderung. Navigieren Sie zum Ordner des Beispielprojekts.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Geben Sie die Registrierungsinformationen abhängig von Ihrer Einrichtung auf eine der folgenden Arten ein:

    - **Individuelle Registrierung:** 

        1. Bearbeiten Sie `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java`, um die zuvor notierten Werte für den _ID-Bereich_ und den _globalen Endpunkt für den Bereitstellungsdienst_ einzufügen. Fügen Sie außerdem die zuvor notierten Werte für _Client Cert_ und _Client Cert Private Key_ ein.

            ```java
            private static final String idScope = "[Your ID scope here]";
            private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
            private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
            private static final String leafPublicPem = "<Your Public PEM Certificate here>";
            private static final String leafPrivateKey = "<Your Private PEM Key here>";
            ```

            - Verwenden Sie für Ihr Zertifikat und Ihren Schlüssel das folgende Format:
            
                ```java
                private static final String leafPublicPem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                private static final String leafPrivateKey = "-----BEGIN PRIVATE KEY-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXX\n" +
                    "-----END PRIVATE KEY-----\n";
                ```

    - **Registrierungsgruppen:** 

        1. Befolgen Sie die obigen Anweisungen für die **individuelle Registrierung**.

        1. Fügen Sie am Anfang der `main`-Funktion die folgenden Codezeilen hinzu:
        
            ```java
            String intermediatePem = "<Your Signer Certificate here>";          
            String rootPem = "<Your Root Certificate here>";
                
            signerCertificates.add(intermediatePem);
            signerCertificates.add(root);
            ```
    
            - Verwenden Sie für Ihre Zertifikate das folgende Format:
        
                ```java
                String intermediatePem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                String rootPem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                ```

1. Erstellen Sie das Beispiel. Navigieren Sie zum Zielordner, und führen Sie die erstellte JAR-Datei aus.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

1. Navigieren Sie im Portal zu dem IoT Hub, der mit Ihrem Provisioning-Dienst verknüpft ist, und öffnen Sie das Blatt **Device Explorer**. Nach der erfolgreichen Bereitstellung des simulierten X.509-Geräts auf dem Hub wird die dazugehörige Geräte-ID auf dem Blatt **Device Explorer** mit dem *STATUS* **Aktiviert** angezeigt. Beachten Sie, dass Sie unter Umständen oben auf die Schaltfläche **Aktualisieren** klicken müssen, wenn Sie das Blatt bereits vor dem Ausführen der Beispielgeräteanwendung geöffnet haben. 

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/java-quick-create-simulated-device-x509/hub-registration.png) 

> [!NOTE]
> Wenn Sie den *anfänglichen Gerätezwillingsstatus* im Registrierungseintrag für Ihr Gerät gegenüber dem Standardwert geändert haben, kann der gewünschte Zwillingsstatus vom Hub abgerufen werden, und es können entsprechende Aktionen durchgeführt werden. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).
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
