---
title: "Durchführen eines Firmwareupdates mit Azure IoT Hub (Java/Java) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Geräteverwaltung in Azure IoT Hub verwendet wird, um ein Firmwareupdate zu initiieren. Sie verwenden das Azure IoT-Geräte-SDK für Java, um eine simulierte Geräte-App und eine Dienst-App zu implementieren, die das Firmwareupdate auslöst."
services: iot-hub
documentationcenter: java
author: msebolt
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: v-masebo
ms.openlocfilehash: 024c2e9bf580f97b412a85913ca29d757872556a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-javajava"></a>Initiieren eines Firmwareupdates mithilfe der Geräteverwaltung (Java/Java)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

Im Tutorial [Get started with device management][lnk-dm-getstarted] (Erste Schritte mit der Geräteverwaltung) wurde erläutert, wie Sie mit einem [Gerätezwilling][lnk-devtwin] und [direkten Methoden][lnk-c2dmethod] Grundtypen für den Remoteneustart eines Geräts verwenden können. In diesem Tutorial werden die gleichen IoT Hub-Grundtypen verwendet, und Sie sehen, wie Sie ein simuliertes End-to-End-Firmwareupdate durchführen.  Dieses Muster wird in der Firmwareupdateimplementierung für das [Implementierungsbeispiel mit dem Raspberry Pi-Gerät][lnk-rpi-implementation] verwendet.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen einer Java-Konsolen-App, die die direkte **firmwareUpdate**-Methode auf der simulierten Geräte-App über Ihren IoT-Hub aufruft.
* Erstellen Sie eine Java-Konsolen-App, die das Gerät simuliert und die direkte **firmwareUpdate**-Methode implementiert. Diese Methode löst einen mehrstufigen Prozess aus, der auf das Herunterladen des Firmware-Images wartet, das Firmware-Image herunterlädt und schließlich das Firmware-Image anwendet. In allen Phasen des Updates nutzt das Gerät die gemeldeten Eigenschaften zum Berichten des Fortschritts.

Am Ende dieses Tutorials verfügen Sie über zwei Java-Konsolen-Apps:

**firmware-update** ruft eine direkte Methode auf dem simulierten Gerät auf und zeigt die Antwort sowie in regelmäßigen Abständen gemeldete Eigenschaftenupdates an.

**simulated-device** stellt eine Verbindung mit Ihrem IoT Hub mit der zuvor erstellten Geräteidentität her, empfängt den Aufruf der direkten firmwareUpdate-Methode und führt eine Firmwareupdatesimulation aus.

Für dieses Tutorial benötigen Sie Folgendes:

* Das neueste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen eines Remotefirmwareupdates auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie eine Jave-Konsolen-App, die auf einem Gerät ein Remotefirmwareupdate auslöst. Die App verwendet eine direkte Methode zum Auslösen des Updates und Gerätzwillingsabfragen, um in regelmäßigen Abständen den Status des aktiven Firmwareupdates abzurufen.

1. Erstellen Sie einen leeren Ordner mit dem Namen „fw-get-started“.

1. Erstellen Sie im Ordner „fw-get-started“ ein Maven-Projekt namens **firmware-update**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=firmware-update -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum firmware-update-Ordner.

1. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „firmware-update“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-service-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-service-client** mithilfe der [Maven-Suche](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Öffnen Sie die Quelldatei „firmware-update\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{youriothubconnectionstring}** durch die IoT Hub-Verbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer IoT Hub-Instanz* notiert haben:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "firmwareUpdate";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. Fügen Sie der **App**-Klasse Folgendes hinzu, um eine Methode zu implementieren, die die gemeldeten Eigenschaften vom Gerätezwilling liest:

    ```java
    public static void ShowReportedProperties() 
    {
        try 
        {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          
          Boolean firmwareUpdated = false;
          Integer timeoutCycle = 0;
    
          while (!firmwareUpdated)
          {
            if (timeoutCycle > 5)
            {
              System.out.println("Operation timed out");
              break;
            }
    
            Thread.sleep(1000);
              
            deviceTwins.getTwin(twinDevice);
    
            String reportedProperties = twinDevice.reportedPropertiesToString();
              
            if(reportedProperties.contains("status=waiting"))
            {
              System.out.println("Waiting on device...");
            }
            else if(reportedProperties.contains("status=downloadComplete"))
            {
              System.out.println("Download complete, applying firmware...");
            }
            else if (reportedProperties.contains("status=applyComplete"))
            {
              System.out.println("Firmware applied");
              System.out.println("Get reported properties from device twin");
              System.out.println(twinDevice.reportedPropertiesToString());
              firmwareUpdated = true;
            }
            else
            {
              timeoutCycle++;
            }
          }
        } catch (Exception ex) {
            System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
    }
    ```

1. Ändern Sie die Signatur der **main**-Methode, um die folgenden Ausnahmen auszulösen:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um die direkte firmwareUpdate-Methode auf dem simulierten Gerät aufzurufen:

    ```java
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      String payload = "https://someurl";
      
      System.out.println("Invoked firmware update on device.");
      System.out.println("Sent URL: " + payload);
      
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }

      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
      System.out.println(e.getMessage());
    }
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um die gemeldeten Eigenschaften vom simulierten Gerät abzurufen:

    ```java
    ShowReportedProperties();
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, damit Sie die App beenden können:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    System.out.println("Shutting down sample...");
    ```

1. Speichern und schließen Sie die Datei „firmware-update\src\main\java\com\mycompany\app\App.java“.

1. Erstellen Sie die Back-End-App **firmware-update**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum firmware-update-Ordner, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`

## <a name="simulate-a-device-to-handle-direct-method-calls"></a>Simulieren eines Geräts zum Behandeln direkter Methodenaufrufe
In diesem Abschnitt erstellen Sie eine Java-Konsolen-App für ein simuliertes Gerät, die die direkte firmwareUpdate-Methode empfangen kann. Die App durchläuft dann einen mehrstufigen Prozess zum Simulieren eines Firmwareupdates und verwendet reportedProperties, um den Status zu kommunizieren.

1. Erstellen Sie im Ordner „fw-get-started“ ein Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“.

1. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „firmware-update“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-service-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-device-client** mithilfe der [Maven-Suche](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    import java.util.HashMap;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{yourdeviceconnectionstring}** durch die Geräteverbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer Geräteidentität* notiert haben:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring";
    private static DeviceClient client;

    private static String downloadURL = "unknown";
    ```

1. Um die Funktionalität der direkten Methode zu implementieren, ermöglichen Sie Rückrufe durch Hinzufügen der folgenden geschachtelten Klassen zur **App**-Klasse:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "firmwareUpdate" :
          {
            System.out.println("Response to method '" + methodName + "' sent successfully");
    
            downloadURL = new String((byte[])methodData);
    
            int status = METHOD_SUCCESS;
            System.out.println("Starting firmware update");
            deviceMethodData = new DeviceMethodData(status, "Started firmware update");
            FirmwareUpdateThread firmwareUpdateThread = new FirmwareUpdateThread();
            Thread t = new Thread(firmwareUpdateThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Um die Gerätezwillingsfunktionalität zu implementieren, ermöglichen Sie Rückrufe durch Hinzufügen der folgenden geschachtelten Klassen zur **App**-Klasse:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. Fügen Sie der **App**-Klasse die folgende geschachtelte Klasse hinzu, um das Firmwareupdate zu implementieren:

    ```java
    protected static class FirmwareUpdateThread implements Runnable {
      public void run() {
        try {      
          HashMap initialUpdate = new HashMap();
          Property sentProperty = new Property("firmwareUpdate", initialUpdate);
          Set<Property> sentPackage = new HashSet<Property>();
          
          System.out.println("Downloading from " + downloadURL);
    
          initialUpdate.put("status", "waiting");
          initialUpdate.put("fwPackageUri", downloadURL);
          initialUpdate.put("startedWaitingTime", LocalDateTime.now().toString());   
          sentPackage.add(sentProperty);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          System.out.println("Download complete");
    
          HashMap downloadUpdate = new HashMap();
          downloadUpdate.put("status","downloadComplete");
          downloadUpdate.put("downloadCompleteTime", LocalDateTime.now().toString());
          downloadUpdate.put("startedApplyingImage", LocalDateTime.now().toString());
          sentProperty.setValue(downloadUpdate);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          System.out.println("Apply complete");
    
          HashMap applyUpdate = new HashMap();
          applyUpdate.put("status","applyComplete");
          applyUpdate.put("lastFirmwareUpdate", LocalDateTime.now().toString());
          sentProperty.setValue(applyUpdate);
    
          client.sendReportedProperties(sentPackage);
    
          Thread.sleep(5000);
    
          HashMap resetUpdate = new HashMap();
          applyUpdate.put("status","reset");
          sentProperty.setValue(resetUpdate);
    
          client.sendReportedProperties(sentPackage);
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. Ändern Sie die Signatur der **main**-Methode, um die folgenden Ausnahmen auszulösen:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Um die direkten Methoden und die Gerätezwillingsroutine zu initiieren, fügen Sie den folgenden Code der **main**-Methode hinzu:

    ```java
    client = new DeviceClient(connString, protocol);

    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Fügen Sie dem Ende der **main**-Methode den folgenden Code hinzu, damit Sie die App beenden können:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Speichern und schließen Sie die Datei „simulated-device\src\main\java\com\mycompany\app\App.java“.

1. Erstellen Sie die App **simulated-device**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie an einer Eingabeaufforderung im Ordner **simulated-device** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Firmwareupdate-Methode zu beginnen.
   
    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Führen Sie an einer Eingabeaufforderung im Ordner **firmware-update** den folgenden Befehl aus, um von Ihrem IoT Hub aus das Firmwareupdate aufzurufen und die Gerätezwillinge auf Ihrem simulierten Gerät abzufragen:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

3. Die Reaktion des simulierten Geräts auf die direkte Methode wird in der Konsole angezeigt.

    ![Firmware wurde erfolgreich aktualisiert][img-fwupdate]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät ausgelöst und mithilfe der gemeldeten Eigenschaften den Fortschritt der Firmwareaktualisierung überprüft.

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- images -->
[img-fwupdate]: media/iot-hub-java-java-firmware-update/firmwareUpdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-java-java-device-management-getstarted.md
[lnk-tutorial-jobs]: iot-hub-java-java-schedule-jobs.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device