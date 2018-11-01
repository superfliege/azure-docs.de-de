---
title: Überwachen eines Gebäudebereichs mit Azure Digital Twins | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure Digital Twins Ihre räumlichen Ressourcen bereitstellen und die Arbeitsbedingungen überwachen.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/26/2018
ms.author: dkshir
ms.openlocfilehash: cf45cb8de0e40dfe5f5772dcb1a0be2aa7585fd6
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50156679"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins"></a>Tutorial: Bereitstellen des Gebäudes und Überwachen der Arbeitsbedingungen mit Azure Digital Twins

In diesem Tutorial wird veranschaulicht, wie Sie Ihre Gebäudebereiche mit Azure Digital Twins auf die gewünschten Temperaturbedingungen und weitere Komfortwerte überwachen. Nach dem [Konfigurieren des Beispielgebäudes](tutorial-facilities-setup.md) können Sie wie hier beschrieben das Gebäude bereitstellen und benutzerdefinierte Funktionen für Ihre Sensordaten ausführen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Definieren der zu überwachenden Bedingungen
> * Erstellen einer benutzerdefinierten Funktion
> * Simulieren von Sensordaten
> * Abrufen der Ergebnisse einer benutzerdefinierten Funktion

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie das [Azure Digital Twins-Setup konfiguriert](tutorial-facilities-setup.md) haben. Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie fortfahren:
- Ein [Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Instanz von Azure Digital Twins. 
- Die [C#-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) (auf den Arbeitscomputer heruntergeladen und extrahiert). 
- [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer zum Erstellen und Ausführen des Beispiels. Führen Sie `dotnet --version` aus, um zu überprüfen, ob die richtige Version installiert ist. 
- [Visual Studio Code](https://code.visualstudio.com/) zum Untersuchen des Beispielcodes. 

## <a name="define-conditions-to-monitor"></a>Definieren der zu überwachenden Bedingungen
Sie können eine Reihe spezifischer Bedingungen definieren, die Sie in den Geräte- oder Sensordaten überwachen möchten. Diese Bedingungen werden als **Abgleicher** (Matcher) bezeichnet. Anschließend können Sie Funktionen definieren, die als *benutzerdefinierte Funktionen* bezeichnet werden. Diese Funktionen führen benutzerdefinierte Logik für Daten von Ihren Gebäudebereichen und Geräten aus, wenn die mit den Abgleichern festgelegten Bedingungen auftreten. Weitere Informationen finden Sie unter [Data Processing and User-Defined Functions](concepts-user-defined-functions.md) (Datenverarbeitung und benutzerdefinierte Funktionen). 

Öffnen Sie aus dem Beispielprojekt **_occupancy-quickstart_** die Datei **_src\actions\provisionSample.yaml_** in Visual Studio Code. Beachten Sie den Abschnitt, der mit dem Typ **matchers** beginnt. Jeder Eintrag unter diesem Typ erstellt einen Abgleicher mit dem angegebenen **Namen**, der einen Sensor vom Typ **dataTypeValue** überwacht. Beachten Sie die Beziehung zum Gebäudebereich namens *Focus Room A1*, der einen Knoten **devices** mit einigen **sensors** enthält. Um einen Abgleicher bereitzustellen, der einen dieser Sensoren nachverfolgt, muss dessen **dataTypeValue** mit dem **dataType**-Wert des Sensors übereinstimmen. 

Fügen Sie den folgenden Abgleicher unterhalb der vorhandenen Abgleicher hinzu, und stellen Sie sicher, dass die Schlüssel korrekt angepasst sind und Leerzeichen nicht durch Tabstopps ersetzt werden:

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Dieser Abgleicher überwacht den Sensor *SAMPLE_SENSOR_TEMPERATURE*, den Sie im [ersten Tutorial](tutorial-facilities-setup.md) hinzugefügt haben. Beachten Sie, dass diese Zeilen auch in der Datei *provisionSample.yaml* als auskommentierte Zeilen vorhanden sind. Sie können die Auskommentierung einfach aufheben, indem Sie das Zeichen „#“ am Anfang jeder Zeile entfernen. 

<a id="udf" />

## <a name="create-a-user-defined-function"></a>Erstellen einer benutzerdefinierten Funktion
Mithilfe von benutzerdefinierten Funktionen (User Defined Functions, UDFs) können Sie die Verarbeitung Ihrer Sensordaten anpassen. Bei diesen Funktionen handelt es sich um benutzerdefinierten JavaScript-Code, der in Ihrer Digital Twins-Instanz ausgeführt werden kann, wenn bestimmte von den Abgleichern definierte Bedingungen erfüllt sind. Sie können *Abgleicher* und *benutzerdefinierte Funktionen* für jeden Sensor erstellen, den Sie überwachen möchten. Ausführlichere Informationen finden Sie unter [Data Processing and User-Defined Functions](concepts-user-defined-functions.md) (Datenverarbeitung und benutzerdefinierte Funktionen). 

Suchen Sie in der Beispieldatei *provisionSample.yaml* nach einem Abschnitt, der mit dem Typ **userdefinedfunctions** beginnt. Dieser Abschnitt stellt eine benutzerdefinierte Funktion mit einem **Namen** bereit, die für die Liste der Abgleicher unter **matcherNames** ausgeführt wird. Sie können auch Ihre eigene JavaScript-Datei für die benutzerdefinierte Funktion als **Skript** bereitstellen. Beachten Sie auch den Abschnitt mit dem Namen **roleassignments**. In diesem Abschnitt wird der benutzerdefinierten Funktion die Rolle *Space Administrator* zugewiesen, die den Zugriff auf die Ereignisse von allen bereitgestellten Gebäudebereichen ermöglicht. 

1. Konfigurieren Sie die benutzerdefinierte Funktion zum Einschließen des Temperaturabgleichers, indem Sie im Knoten `matcherNames` der Datei *provisionSample.yaml* die folgende Zeile hinzufügen oder ihre Auskommentierung aufheben:

    ```yaml
            - Matcher Temperature
    ```

1. Öffnen Sie die Datei **_src\actions\userDefinedFunctions\availability.js_** im Editor. Auf diese Datei wird im **script**-Element der Datei *provisionSample.yaml* verwiesen. Die benutzerdefinierte Funktion in dieser Datei sucht nach zwei Bedingungen: keine Bewegung im Raum und Kohlendioxidwerte unter 1.000 ppm. Ändern Sie die JavaScript-Datei, um zusätzlich zu diesen Bedingungen die Temperatur zu überwachen. Fügen Sie die folgenden Codezeilen hinzu, um nach den Bedingungen keine Bewegung im Raum, Kohlendioxidwerte unter 1.000 ppm und Temperaturen unter 78 Grad Fahrenheit zu suchen.

   > [!NOTE]
   > In diesem Abschnitt wird die Datei *src\actions\userDefinedFunctions\availability.js* geändert, damit Sie sich mit einer Möglichkeit zum Schreiben von benutzerdefinierten Funktionen vertraut machen können. Sie können die Datei [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) jedoch auch direkt in Ihrem Setup verwenden. Diese Datei enthält alle Änderungen, die für dieses Tutorial erforderlich sind. Wenn Sie stattdessen diese Datei verwenden, müssen Sie darauf achten, dass Sie den richtigen Dateinamen für den **_script_**-Schlüssel in der Datei [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) verwenden.

    1. Fügen Sie am Anfang der Datei die folgenden Zeilen für die Temperatur unter dem Kommentar `// Add your sensor type here` hinzu:

        ```JavaScript
            var temperatureType = "Temperature";
            var temperatureThreshold = 78;
        ```
   
    1. Fügen Sie die folgenden Zeilen nach der Anweisung, die `var motionSensor` definiert, unter dem Kommentar `// Add your sensor variable here` hinzu:

        ```JavaScript
            var temperatureSensor = otherSensors.find(function(element) {
                return element.DataType === temperatureType;
            });
        ```
    
    1. Fügen Sie die folgende Zeile nach der Anweisung, die `var carbonDioxideValue` definiert, unter dem Kommentar `// Add your sensor latest value here` hinzu:

        ```JavaScript
            var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
        ```
    
    1. Entfernen Sie die folgenden Codezeilen unter dem Kommentar `// Modify this line to monitor your sensor value`: 

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null) {
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
                return;
            }
        ```
       
       Ersetzen Sie sie durch die folgenden Zeilen:

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
                return;
            }
        ```
    
    1. Entfernen Sie die folgenden Codezeilen unter dem Kommentar `// Modify these lines as per your sensor`:

        ```JavaScript
            var availableFresh = "Room is available and air is fresh";
            var noAvailableOrFresh = "Room is not available or air quality is poor";
        ```

       Ersetzen Sie sie durch die folgenden Zeilen:

        ```JavaScript
            var alert = "Room with fresh air and comfortable temperature is available.";
            var noAlert = "Either room is occupied, or working conditions are not right.";
        ```
    
    1. Entfernen Sie den folgenden *if-else*-Codeblock nach dem Kommentar `// Modify this code block for your sensor`:

        ```JavaScript
            // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
            if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
                log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(availableFresh));
            }
            else {
                log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
            }
        ```
      
       Ersetzen Sie ihn durch den folgenden *if-else*-Block:

        ```JavaScript
            // If sensor values are within range and room is available
            if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
                log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

                // Set up notification for this alert
                parentSpace.Notify(JSON.stringify(alert));
            }
            else {
                log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);
    
                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
            }
        ```
        
        Die geänderte benutzerdefinierte Funktion sucht nach einer Bedingung, unter der ein Raum verfügbar ist und die Kohlendioxid- und Temperaturwerte den akzeptablen Grenzwerten entsprechen. Sie generiert eine Benachrichtigung mit der Anweisung `parentSpace.Notify(JSON.stringify(alert));`, wenn diese Bedingung erfüllt ist. Die Funktion legt den Wert des überwachten Gebäudebereichs mit der entsprechenden Meldung fest, unabhängig davon, ob die Bedingung erfüllt ist.
    
    1. Speichern Sie die Datei . 
    
1. Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Ordner **_occupancy-quickstart\src_**. Führen Sie den folgenden Befehl aus, um Ihren Raumintelligenzgraphen und die benutzerdefinierte Funktion bereitzustellen. 

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Um nicht autorisierten Zugriff auf die Digital Twins-Verwaltungs-API zu verhindern, erfordert die Anwendung **_occupancy-quickstart_**, dass Sie sich mit Ihren Azure-Anmeldeinformationen anmelden. Die Anwendung speichert Ihre Anmeldeinformationen für einen kurzen Zeitraum, damit Sie sich nicht jedes Mal anmelden müssen, wenn Sie sie ausführen. Bei der erstmaligen Ausführung des Programms und beim Ablauf der gespeicherten Anmeldeinformationen werden Sie zu einer Anmeldeseite geleitet, und Sie erhalten einen sitzungsspezifischen Code, den Sie auf dieser Seite eingeben müssen. Folgen Sie den Aufforderungen auf dem Bildschirm, um sich mit Ihrem Azure-Konto anzumelden.


1. Sobald Ihr Konto authentifiziert wurde, beginnt die Anwendung, einen Beispielraumgraphen entsprechend der Konfiguration in der Datei *provisionSample.yaml* zu erstellen. Warten Sie, bis die Bereitstellung abgeschlossen ist. Dies dauert einige Minuten. Sehen Sie sich anschließend die Meldungen im Befehlsfenster an, und beobachten Sie, wie der Raumgraph erstellt wird. Beachten Sie, dass im Stammknoten oder `Venue` ein IoT-Hub erstellt wird. 

1. Kopieren Sie in der Ausgabe im Befehlsfenster den Wert der `ConnectionString` unter dem Abschnitt `Devices` in die Zwischenablage. Sie benötigen diesen Wert im nächsten Abschnitt, um die Verbindung mit dem Gerät zu simulieren.

    ![Bereitstellen des Beispiels](./media/tutorial-facilities-udf/run-provision-sample.png)

> [!TIP]
> Falls während der Bereitstellung eine Fehlermeldung mit dem Hinweis angezeigt wird, dass der E/A-Vorgang aufgrund einer Threadbeendigung oder Anwendungsanforderung abgebrochen wurde, führen Sie den Befehl noch einmal aus. Dies kann passieren, wenn wegen eines Netzwerkproblems ein Timeout des HTTP-Clients auftritt.

<a id="simulate" />

## <a name="simulate-sensor-data"></a>Simulieren von Sensordaten
In diesem Abschnitt verwenden Sie das Projekt namens *device-connectivity* im Beispiel, um Sensordaten für die Erkennung von Bewegung, Temperatur oder Kohlendioxid zu simulieren. Das Projekt generiert Zufallswerte für die Sensoren und sendet diese mithilfe der Verbindungszeichenfolge des Geräts an den IoT-Hub.

1. Navigieren Sie in einem separaten Befehlsfenster zum Digital Twins-Beispiel und anschließend zum Ordner **_device-connectivity_**.

1. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass die Abhängigkeiten für das Projekt korrekt sind:

    ```cmd/sh
    dotnet restore
    ```

1. Öffnen Sie die Datei *appSettings.json* im Editor, und bearbeiten Sie die folgenden Werte:
    1. *DeviceConnectionString*: Weisen Sie den `ConnectionString`-Wert zu, der im vorherigen Abschnitt im Ausgabefenster angezeigt wurde. Kopieren Sie unbedingt die vollständige Zeichenfolge innerhalb der Anführungszeichen. Andernfalls kann der Simulator die Verbindung mit dem IoT-Hub nicht ordnungsgemäß herstellen.

    1. *HardwareId* innerhalb des *Sensors*-Arrays: Da Sie Ereignisse von Sensoren simulieren, die in Ihrer Digital Twins-Instanz bereitgestellt wurden, müssen die Hardware-ID und die Namen der Sensoren in dieser Datei den Angaben im Knoten `sensors` in der Datei *provisionSample.yaml* entsprechen. Fügen Sie einen neuen Eintrag für den Temperatursensor hinzu. Der Knoten **Sensors** in der Datei *appSettings.json* sollte wie folgt aussehen:

        ```JSON
        "Sensors": [{
          "DataType": "Motion",
          "HardwareId": "SAMPLE_SENSOR_MOTION"
        },{
          "DataType": "CarbonDioxide",
          "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
        },{
          "DataType": "Temperature",
          "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
        }]
        ```

1. Führen Sie den folgenden Befehl aus, um die Simulation von Geräteereignissen für Temperatur, Bewegung und Kohlendioxid zu starten:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE] 
   > Da das Simulationsbeispiel nicht direkt mit Ihrer Digital Twins-Instanz kommuniziert, müssen Sie sich nicht authentifizieren.

## <a name="get-results-of-user-defined-function"></a>Abrufen der Ergebnisse einer benutzerdefinierten Funktion
Die benutzerdefinierte Funktion wird jedes Mal ausgeführt, wenn Ihre Instanz Geräte- und Sensordaten empfängt. In diesem Abschnitt fragen Sie Ihre Digital Twins-Instanz ab, um die Ergebnisse der benutzerdefinierten Funktion abzurufen. Sie sehen nahezu in Echtzeit, wann ein Raum verfügbar, die Luft frisch und die Temperatur geeignet ist. 

1. Öffnen Sie das Befehlsfenster, das Sie zum Bereitstellen des Beispiels verwendet haben, oder ein neues Befehlsfenster, und navigieren Sie erneut zum Ordner **_occupancy-quickstart\src_** des Beispiels. 

1. Führen Sie den folgenden Befehl aus, und melden Sie sich an, wenn Sie dazu aufgefordert werden:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

Das Ausgabefenster zeigt die Ausführung der benutzerdefinierten Funktion und fängt Ereignisse der Gerätesimulation ab. 

   ![Ausführen der benutzerdefinierten Funktion](./media/tutorial-facilities-udf/udf-running.png)

Je nachdem, ob die überwachte Bedingung erfüllt wird, legt die benutzerdefinierte Funktion den Wert des Gebäudebereichs wie im [obigen Abschnitt](#udf) beschrieben mit der entsprechenden Meldung fest, und der Wert wird von der Funktion `GetAvailableAndFreshSpaces` an die Konsole ausgegeben. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Klicken Sie im [Azure-Portal](http://portal.azure.com) im Menü auf der linken Seite auf **Alle Ressourcen**, wählen Sie Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie dann auf **Löschen**.
2. Sie können auch die Beispielanwendungen auf Ihrem Arbeitscomputer löschen, falls dies notwendig ist. 


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Gebäudebereiche bereitgestellt und ein Framework zum Auslösen benutzerdefinierter Benachrichtigungen erstellt haben, können Sie mit einem der folgenden Tutorials fortfahren. 

> [!div class="nextstepaction"]
> [Tutorial: Empfangen von Benachrichtigungen von den Azure Digital Twins-Gebäudebereichen mit Azure Logic Apps](tutorial-facilities-events.md)

Oder

> [!div class="nextstepaction"]
> [Tutorial: Visualisieren und Analysieren von Ereignissen in den Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights](tutorial-facilities-analyze.md)
