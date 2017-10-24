---
title: "Verwenden von Azure IoT Hub-Gerätezwillingseigenschaften (Java) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Geräte konfigurieren. Sie verwenden das Azure IoT-Geräte-SDK für Java, um eine simulierte Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK für Java, um eine Dienst-App zu implementieren, die eine Gerätekonfiguration mithilfe eines Gerätezwillings ändert."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: dobett
ms.openlocfilehash: 3881e3791c97baf2922722f01f9e6e6ea55ed2e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten

[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Java-Konsolen-Apps:

* **simulated-device**, eine simulierte Geräte-App, die auf eine gewünschte Konfigurationsaktualisierung wartet und den Status eines simulierten Konfigurationsaktualisierungsvorgangs meldet.
* **set-configuration**, eine Back-End-App, die die gewünschte Konfiguration für ein Gerät festlegt und den Konfigurationsaktualisierungsprozess abfragt.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Das neueste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Wenn Sie das Tutorial [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial] ausgeführt haben, haben Sie bereits einen IoT Hub und die Geräteidentität **myDeviceId**. In diesem Fall können Sie den Abschnitt [Erstellen der simulierten Geräte-App][lnk-how-to-configure-createapp] überspringen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Erstellen der simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die als **myDeviceId** eine Verbindung mit dem Hub herstellt, auf eine gewünschte Konfigurationsaktualisierung wartet und dann Aktualisierungen für den simulierten Konfigurationsaktualisierungsvorgang meldet.

1. Erstellen Sie einen leeren Ordner mit dem Namen „dt-get-started“.

1. Erstellen Sie im Ordner „dt-get-started“ ein Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“.

1. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „simulated-device“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-service-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.30</version>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-device-client** mithilfe der [Maven-Suche][lnk-maven-device-search].

1. Fügen Sie den Knoten **build** hinter dem Knoten **dependencies** hinzu. Diese Konfiguration weist Maven an, Java 1.8 zu verwenden, um die App zu erstellen:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Speichern und schließen Sie die Datei „pom.xml“.

1. Öffnen Sie die Quelldatei „simulated-device\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{yourdeviceconnectionstring}** durch die Geräteverbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer Geräteidentität* notiert haben:

    ```java
    private static String deviceId = "myDeviceId";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    private static TelemetryConfig telemetryConfig;
    ```

1. Fügen Sie der **App**-Klasse die folgende geschachtelte Klasse hinzu, um einen Rückrufhandler für Statusereignisse des Gerätezwillings (zum Debuggen) zu implementieren:

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback 
    {
        public void execute(IotHubStatusCode status, Object context) 
        {
            //System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. Fügen Sie der **App** -Klasse die folgende geschachtelte Klasse hinzu:

    ```java
    private static class TelemetryConfig extends Device 
    {
        private String configId = "0000";
        private String sendFrequency = "45m";

        private Boolean initialRun = true;
        
        private Property telemetryConfig = new Property("telemetryConfig", "{configId=" + configId + ", sendFrequency=" + sendFrequency + "}");

        public void InitTelemetry() throws IOException 
        {
            System.out.println("Report initial telemetry config:");
            System.out.println(this.telemetryConfig);

            this.setReportedProp(this.telemetryConfig);

            client.sendReportedProperties(this.getReportedProp());
        }

        private void UpdateReportedConfiguration() 
        {
            try {
                System.out.println("Initiating config change");
                    
                this.setReportedProp(this.telemetryConfig);
                client.sendReportedProperties(this.getReportedProp());
                    
                System.out.println("Simulating device reset");
                
                Thread.sleep(10000);
                
                System.out.println("Completing config change");
                System.out.println("Config change complete");
            } catch (Exception e) {
                System.out.println("Exception \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
            }
        }

        @Override
        public void PropertyCall(String propertyKey, Object propertyValue, Object context) 
        {
            if (propertyKey.equals("telemetryConfig")) {
                if (!(initialRun)) {
                    System.out.println("Desired property change:");
                    System.out.println(propertyKey + ":" + propertyValue);

                    telemetryConfig.setValue(propertyValue);

                    UpdateReportedConfiguration();
                } else {
                    initialRun = false;
                }
            } 
        }
    }
    ```

    > [!NOTE]
    > Die TelemetryConfig-Klasse erweitert die [Device-Klasse][lnk-java-device-class] für den Zugriff auf die gewünschten Eigenschaftenrückrufe.

1. Ändern Sie die Signatur der **main**-Methode, um die folgenden Ausnahmen auszulösen:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um einen **DeviceClient** und **TelemetryConfig** zu instanziieren:

    ```java
    client = new DeviceClient(connString, protocol);

    telemetryConfig = new TelemetryConfig();
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um Gerätezwillingsdienste zu starten:

    ```java
    try 
    {
        System.out.println("Connecting to hub");
        client.open();
        client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, telemetryConfig, null);

        telemetryConfig.InitTelemetry();

        System.out.println("Wait for desired telemetry...");
        client.subscribeToDesiredProperties(telemetryConfig.getDesiredProp());
    }
    catch (Exception e) 
    {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
        telemetryConfig.clean();
        client.close();
        System.out.println("Shutting down...");
    }
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um den Gerätesimulator nötigenfalls herunterzufahren:

    ```java
    System.out.println("Press enter to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    telemetryConfig.clean();
    client.close();
    ```

1. Speichern und schließen Sie die Datei „simulated-device\src\main\java\com\mycompany\app\App.java“.

1. Erstellen Sie die Back-End-App **simulated-device**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`

   > [!NOTE]
   > In diesem Tutorial wird nicht das Verhalten für gleichzeitige Konfigurationsaktualisierungen simuliert. Bei einigen Konfigurationsaktualisierungsvorgängen können Änderungen der Zielkonfiguration möglicherweise während der Ausführung der Aktualisierung erfolgen, und bei einigen müssen diese eventuell in die Warteschlange gestellt werden. Bei anderen wiederum können sie ohne eine Fehlerbedingung abgelehnt werden. Stellen Sie sicher, dass Sie das gewünschte Verhalten für den spezifischen Konfigurationsvorgang berücksichtigen, und fügen Sie vor dem Initiieren der Konfigurationsänderung die entsprechende Logik hinzu.
   > 
   > 

## <a name="create-the-service-app"></a>Erstellen der Dienst-App
In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die die *gewünschten Eigenschaften* für den Gerätezwilling, der **myDeviceId** zugeordnet ist, mit einem neuen Telemetriekonfigurationsobjekt aktualisiert. Anschließend werden die im IoT Hub gespeicherten Gerätezwillinge abgefragt und die Unterschiede zwischen den gewünschten und den gemeldeten Konfigurationen des Geräts angezeigt.

1. Erstellen Sie im Ordner „dt-get-started“ ein Maven-Projekt namens **set-configuration**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=set-configuration -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum Ordner „set-configuration“.

1. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „trigger-reboot“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-service-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-service-client** mithilfe der [Maven-Suche][lnk-maven-service-search].

1. Fügen Sie den Knoten **build** hinter dem Knoten **dependencies** hinzu. Diese Konfiguration weist Maven an, Java 1.8 zu verwenden, um die App zu erstellen:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Speichern und schließen Sie die Datei „pom.xml“.

1. Öffnen Sie die Quelldatei „set-configuration\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{youriothubconnectionstring}** durch die IoT Hub-Verbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer IoT Hub-Instanz* notiert haben:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um Gerätezwillinge auf dem simulierten Gerät abzufragen und zu aktualisieren:

    ```java
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);

    String sendFrequency= "12h";

    try {
        twinClient.getTwin(device);

        // make sure a differing value exists before calling the updateTwin() method, else a null device exception will be thrown
        // this allows the program to be run multiple times for demonstration purposes
        String desiredProperties = device.desiredPropertiesToString();
        if (desiredProperties.contains("sendFrequency=" + sendFrequency))
        {
            sendFrequency = "8h";
        }
            
        Set<Pair> tags = new HashSet<Pair>();
        tags.add(new Pair("telemetryConfig", "{configId=0001, sendFrequency=" + sendFrequency + "}"));

        twinClient.getTwin(device);
        device.setDesiredProperties(tags);

        System.out.println("Config report for: " + deviceId);   
        System.out.println(device);

        twinClient.updateTwin(device);

        String reportedProperties = device.reportedPropertiesToString();
        Boolean waitFlag = true;

        while (waitFlag) {
            if (!reportedProperties.contains("sendFrequency=" + sendFrequency)) {
                Thread.sleep(10000);
            }
            else 
            {
                waitFlag = false;
            }

            twinClient.getTwin(device);
            reportedProperties = device.reportedPropertiesToString();
        }
            
        SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "properties.reported.telemetryConfig='{configId=0001, sendFrequency=" + sendFrequency + "}'", null);

        Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
        while (twinClient.hasNextDeviceTwin(twinQuery)) {
            DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
            System.out.println(d.getDeviceId() + " found with changed telemetryConfig");
        }

        System.out.println("Config report for: " + deviceId);
        twinClient.getTwin(device);
        System.out.println(device);

        } catch (IotHubException e) {
            System.out.println(e.getMessage());
        } catch (IOException e) {
            System.out.println(e.getMessage());
        } catch (InterruptedException e) {
            System.out.println(e.getMessage());
        }
    ```

1. Speichern und schließen Sie die Datei „set-desired-configuration\src\main\java\com\mycompany\app\App.java“.

1. Erstellen Sie die Back-End-App **set-configuration**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum Ordner „set-configuration“, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`
   
    Mit diesem Code wird der Gerätezwilling für **myDeviceId** abgerufen, und dann werden die zugehörigen gewünschten Eigenschaften mit einem neuen Telemetriekonfigurationsobjekt aktualisiert.
    Danach werden die im IoT Hub gespeicherten Gerätezwillinge alle 10 Sekunden abgefragt und die gewünschten und gemeldeten Telemetriekonfigurationen gedruckt. Informationen zum Generieren von umfassenden Berichten für alle Ihre Geräte finden Sie im Artikel zur [IoT Hub-Abfragesprache][lnk-query].
   
   > [!IMPORTANT]
   > Diese Anwendung fragt IoT Hub zur Veranschaulichung alle 10 Sekunden ab, bis das Gerät aktualisiert wird. Verwenden Sie Abfragen, um Berichte für Benutzer für viele Geräte zu generieren, und nicht, um Änderungen zu erkennen. Wenn für Ihre Lösung Echtzeitbenachrichtigungen von Geräteereignissen erforderlich sind, verwenden Sie [twin notifications][lnk-twin-notifications].
   > 

   > [!IMPORTANT]
   > Es gibt eine Verzögerung von bis zu einer Minute zwischen dem Vorgang für den Gerätebericht und dem Ergebnis der Abfrage. Dies ermöglicht die Abfrageinfrastruktur mit umfangreicher Skalierung.  Zum Abrufen einheitlicher Ansichten eines einzelnen Gerätezwillings verwenden Sie die **getDeviceTwin**-Methode in der **DeviceTwin**-Klasse.
   > 
   > 

## <a name="run-the-apps"></a>Ausführen der Apps

Sie können die Apps nun ausführen.

1. Führen Sie an einer Eingabeaufforderung im Ordner „simulated-device“ den folgenden Befehl aus, um mit dem Lauschen auf Gerätezwillingsaufrufe von Ihrer IoT Hub-Instanz zu beginnen:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Führen Sie an einer Eingabeaufforderung im Ordner „set-configuration“ den folgenden Befehl aus, um von Ihrem IoT Hub aus Gerätezwillinge auf Ihrem simulierten Gerät abzufragen und zu aktualisieren:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Das simulierte Gerät reagiert auf den Aufruf der direkten reboot-Methode:

    ![Simulierte Java IoT Hub-Geräte-App reagiert auf den Gerätezwillingsaufruf][img-deviceconfigured]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine gewünschte Konfiguration als *gewünschte Eigenschaften* im Lösungs-Back-End festgelegt und eine Geräte-App geschrieben, die diese Änderung erkennt und einen Aktualisierungsvorgang in mehreren Schritten simuliert, bei dem der entsprechende Status als gemeldete Eigenschaften gemeldet wird.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub][lnk-iothub-getstarted]
* Planen oder Durchführen von Vorgängen für eine Vielzahl von Geräten im Tutorial [Schedule and broadcast jobs][lnk-schedule-jobs] (Planen und Übertragen von Aufträgen)
* Interaktive Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Use direct methods][lnk-methods-tutorial] (Verwenden von direkten Methoden)

<!-- images -->
[img-deviceconfigured]: media/iot-hub-java-java-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-java-device-class]: https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.sdk.iot.device._device_twin._device

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
