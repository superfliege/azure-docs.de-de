---
title: Bereitstellen eines simulierten X.509-Geräts für Azure IoT Hub mithilfe von Java und Registrierungsgruppen | Microsoft-Dokumentation
description: Azure-Tutorial – Erstellen und Bereitstellen eines simulierten X.509-Geräts per Java-Gerät und Dienst-SDK sowie Registrierungsgruppen für den IoT Hub Device Provisioning-Dienst
author: wesmc7777
ms.author: wesmc
ms.date: 01/04/2018
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 7f51ac0e1137bf09c220c892e2c21b154f2f2433
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522644"
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-and-service-sdk-and-group-enrollments-for-iot-hub-device-provisioning-service"></a>Erstellen und Bereitstellen eines simulierten X.509-Geräts per Java-Gerät und Dienst-SDK sowie Registrierungsgruppen für den IoT Hub Device Provisioning-Dienst

Mit diesen Schritten wird veranschaulicht, wie Sie ein X.509-Gerät auf Ihrem Entwicklungscomputer mit Windows-Betriebssystem simulieren und ein Codebeispiel verwenden, um für dieses simulierte Gerät eine Verbindung mit dem Device Provisioning-Dienst und Ihrem IoT Hub über Registrierungsgruppen herzustellen. 

Führen Sie zunächst die Schritte unter [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md) aus, bevor Sie mit dieser Anleitung fortfahren.


## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung 

1. Vergewissern Sie sich, dass das [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) auf Ihrem Computer installiert ist.

1. Laden Sie [Maven](https://maven.apache.org/install.html) herunter, und installieren Sie es.

1. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Unter [Git-Clienttools von Software Freedom Conservancy](https://git-scm.com/download/) finden Sie die neueste Version der zu installierenden `git`-Tools. Hierzu zählt auch die Befehlszeilen-App **Git Bash**, über die Sie mit Ihrem lokalen Git-Repository interagieren können. 

1. Verwenden Sie die Informationen in der [Zertifikatübersicht](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md), um Ihre Testzertifikate zu erstellen.

    > [!NOTE]
    > Für diesen Schritt ist [OpenSSL](https://www.openssl.org/) erforderlich. OpenSSL kann entweder über die Quelle erstellt und installiert oder über einen [Drittanbieter](https://wiki.openssl.org/index.php/Binaries) (z.B. [hier](https://sourceforge.net/projects/openssl/)) installiert werden. Sie können diesen Schritt überspringen, falls Sie Ihre _Stamm_-, _Zwischen_- und _Geräte_zertifikate bereits erstellt haben.
    >

    1. Führen Sie die ersten beiden Schritte aus, um Ihre _Stamm_- und _Zwischenzertifikate_ zu erstellen.

    1. Melden Sie sich am Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Provisioning-Dienst.

        1. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Zertifikate** aus, und klicken Sie anschließend im oberen Bereich auf die Schaltfläche **Hinzufügen**.

        1. Geben Sie unter **Zertifikat hinzufügen** die folgenden Informationen ein:
            - Geben Sie einen eindeutigen Zertifikatnamen ein.
            - Wählen Sie die Datei **_RootCA.pem_** aus, die Sie gerade erstellt haben.
            - Klicken Sie abschließend auf die Schaltfläche **Speichern**.

        ![Hinzufügen des Zertifikats](./media/tutorial-group-enrollments/add-certificate.png)

        1. Wählen Sie das neu erstellte Zertifikat aus:
            - Klicken Sie auf **Prüfcode generieren**. Kopieren Sie den generierten Code.
            - Führen Sie den Überprüfungsschritt aus. Geben Sie den _Prüfcode_ ein, oder klicken Sie mit der rechten Maustaste, um ihn in Ihr aktives PowerShell-Fenster einzufügen.  Drücken Sie die **EINGABETASTE**.
            - Wählen Sie im Azure-Portal die neu erstellte Datei **_verifyCert4.pem_** aus. Klicken Sie auf **Überprüfen**.

            ![Überprüfen des Zertifikats](./media/tutorial-group-enrollments/validate-certificate.png)

    1. Schließen Sie den Vorgang ab, indem Sie die Schritte zum Erstellen der Gerätezertifikate und zum Bereinigen der Ressourcen ausführen.

    > [!NOTE]
    > Stellen Sie beim Erstellen von Zertifikaten sicher, dass Sie für den Namen des Geräts nur Kleinbuchstaben und Zahlen sowie Bindestriche verwenden.
    >


## <a name="create-a-device-enrollment-entry"></a>Erstellen eines Geräteregistrierungseintrags

1. Öffnen Sie eine Eingabeaufforderung. Klonen Sie das GitHub-Repository für Java SDK-Codebeispiele:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Navigieren Sie im heruntergeladenen Quellcode zum Beispielordner **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**. Öffnen Sie die Datei **_/src/main/java/samples/com/microsoft/azure/sdk/iot/ServiceEnrollmentGroupSample.java_** in einem Editor Ihrer Wahl, und fügen Sie die folgenden Details hinzu:

    1. Fügen Sie das Element `[Provisioning Connection String]` für Ihren Provisioning-Dienst wie folgt aus dem Portal hinzu:

        1. Navigieren Sie zu Ihrem Provisioning-Dienst im [Azure-Portal](https://portal.azure.com). 

        1. Öffnen Sie **Freigegebene Zugriffsrichtlinien**, und wählen Sie eine Richtlinie aus, die über die Berechtigung *EnrollmentWrite* verfügt.
    
        1. Kopieren Sie die **Primärschlüssel-Verbindungszeichenfolge**. 

            ![Abrufen der Verbindungszeichenfolge für die Bereitstellung aus dem Portal](./media/tutorial-group-enrollments/provisioning-string.png)  

        1. Ersetzen Sie in der Beispielcodedatei **_ServiceEnrollmentGroupSample.java_** das Element `[Provisioning Connection String]` durch die **Primärschlüssel-Verbindungszeichenfolge**.

            ```java
            private static final String PROVISIONING_CONNECTION_STRING = "[Provisioning Connection String]";
            ```

    1. Öffnen Sie die Datei **_RootCA.pem_** in einem Text-Editor. Weisen Sie den Wert des **Stammzertifikats** wie hier dargestellt dem Parameter **PUBLIC_KEY_CERTIFICATE_STRING** zu:

        ```java
        private static final String PUBLIC_KEY_CERTIFICATE_STRING =
                "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
        ```
 
    1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum IoT Hub, der mit Ihrem Provisioning-Dienst verknüpft ist. Öffnen Sie die Registerkarte **Übersicht** für den Hub, und kopieren Sie den **Hostnamen**. Weisen Sie diesen **Hostnamen** dem Parameter *IOTHUB_HOST_NAME* zu.

        ```java
        private static final String IOTHUB_HOST_NAME = "[Host name].azure-devices.net";
        ```

    1. Sehen Sie sich den Beispielcode näher an. Er bewirkt, dass eine Gruppenregistrierung für X.509-Geräte erstellt, aktualisiert, abgefragt und gelöscht wird. Um zu überprüfen, ob die Registrierung im Portal erfolgreich war, können Sie die folgenden Codezeilen am Ende der Datei _ServiceEnrollmentGroupSample.java_ vorübergehend auskommentieren:

        ```java
        // ************************************** Delete info of enrollmentGroup ***************************************
        System.out.println("\nDelete the enrollmentGroup...");
        provisioningServiceClient.deleteEnrollmentGroup(enrollmentGroupId);
        ```

    1. Speichern Sie die Datei _ServiceEnrollmentGroupSample.java_. 

1. Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Ordner **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**.

1. Erstellen Sie den Beispielcode mit diesem Befehl:

    ```cmd\sh
    mvn install -DskipTests
    ```

1. Führen Sie das Beispiel aus, indem Sie im Befehlsfenster diese Befehle verwenden:

    ```cmd\sh
    cd target
    java -jar ./service-enrollment-group-sample-{version}-with-deps.jar
    ```

1. Verfolgen Sie im Ausgabefenster, ob die Registrierung erfolgreich ist.

    ![Erfolgreiche Registrierung](./media/tutorial-group-enrollments/enrollment.png) 

1. Navigieren Sie im Azure-Portal zu Ihrem Provisioning-Dienst. Klicken Sie auf **Registrierungen verwalten**. Beachten Sie, dass Ihre Gruppe mit X.509-Geräten unter der Registerkarte **Registrierungsgruppen** mit einem automatisch generierten *GRUPPENNAMEN* angezeigt wird. 


## <a name="simulate-the-device"></a>Simulieren des Geräts

1. Klicken Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts auf **Übersicht**, und notieren Sie sich den _ID-Bereich_ und den _globalen Endpunkt für den Bereitstellungsdienst_.

    ![Dienstinformationen](./media/tutorial-group-enrollments/extract-dps-endpoints.png)

1. Öffnen Sie eine Eingabeaufforderung. Navigieren Sie zum Ordner des Beispielprojekts.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Geben Sie die Informationen zur Registrierungsgruppe wie folgt ein:

    - Bearbeiten Sie `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java`, um die zuvor notierten Werte für den _ID-Bereich_ und den _globalen Endpunkt für den Bereitstellungsdienst_ einzufügen. Öffnen Sie die Datei **_{deviceName}-public.pem_**, und fügen Sie diesen Wert als _Client Cert_ (Clientzertifikat) ein. Öffnen Sie die Datei **_{deviceName}-all.pem_**, und kopieren Sie den Text von _-----BEGIN PRIVATE KEY-----_ bis _-----END PRIVATE KEY-----_.  Verwenden Sie diesen Text als _Client Cert Private Key_ (Privater Schlüssel für das Clientzertifikat).

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

1. Erstellen Sie das Beispiel. Navigieren Sie zum Zielordner, und führen Sie die erstellte JAR-Datei aus.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

    ![Erfolgreiche Registrierung](./media/tutorial-group-enrollments/registration.png)

1. Navigieren Sie im Portal zu dem IoT Hub, der mit Ihrem Provisioning-Dienst verknüpft ist, und öffnen Sie das Blatt **Device Explorer**. Nach der erfolgreichen Bereitstellung des simulierten X.509-Geräts auf dem Hub wird die dazugehörige Geräte-ID auf dem Blatt **Device Explorer** mit dem *STATUS* **Aktiviert** angezeigt. Beachten Sie, dass Sie unter Umständen oben auf die Schaltfläche **Aktualisieren** klicken müssen, wenn Sie das Blatt bereits vor dem Ausführen der Beispielgeräteanwendung geöffnet haben. 

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/tutorial-group-enrollments/hub-registration.png) 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Geräteclientbeispiel weiter verwenden und erkunden möchten, überspringen Sie die Bereinigung der in dieser Schnellstartanleitung erstellten Ressourcen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle erstellten Ressourcen zu löschen, die im Rahmen dieser Schnellstartanleitung erstellt wurden:

1. Schließen Sie auf Ihrem Computer das Ausgabefenster des Geräteclientbeispiels.
1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihren Device Provisioning-Dienst aus. Öffnen Sie das Blatt **Registrierungen verwalten** für Ihren Dienst, und klicken Sie dann auf die Registerkarte **Individuelle Registrierungen**. Wählen Sie die *REGISTRIERUNGS-ID* des Geräts aus, das Sie mit dieser Schnellstartanleitung registriert haben, und klicken Sie im oberen Bereich auf die Schaltfläche **Löschen**. 
1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub-Instanz aus. Öffnen Sie das Blatt **IoT-Geräte** für Ihren Hub, wählen Sie die *GERÄTE-ID* des Geräts aus, die Sie in dieser Schnellstartanleitung registriert haben, und klicken Sie dann im oberen Bereich auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie auf Ihrem Windows-Computer ein simuliertes X.509-Gerät erstellt und es mithilfe des Azure IoT Hub Device Provisioning-Diensts und der Registrierungsgruppen für Ihre IoT Hub-Instanz bereitgestellt. Fahren Sie mit den Gerätekonzepten fort, um weitere Informationen zu Ihrem X.509-Gerät zu erhalten. 

> [!div class="nextstepaction"]
> [Konzepte für Geräte mit dem IoT Hub Device Provisioning-Dienst](concepts-device.md)
