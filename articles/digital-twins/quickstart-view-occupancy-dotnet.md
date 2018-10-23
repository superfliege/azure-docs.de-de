---
title: Suchen nach verfügbaren Räumen mit Azure Digital Twins (C#) | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung führen Sie zwei .NET Core-Beispielanwendungen aus, um simulierte Bewegungs- und CO2-Telemetriedaten an einen Bereich in Azure Digital Twins zu senden. Das Ziel besteht darin, über Verwaltungs-APIs verfügbare Räume mit frischer Luft zu finden, nachdem die Verarbeitung in der Cloud durchgeführt wurde.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/02/2018
ms.author: alinast
ms.openlocfilehash: e0b47b1322a520ad8b09fd2fe2967e628b5e4e03
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322873"
---
# <a name="quickstart-find-available-rooms-using-azure-digital-twins"></a>Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins

Mit dem Azure Digital Twins-Dienst können Sie Ihre physische Umgebung digital abbilden. Anschließend können Sie durch Ereignisse in Ihrer Umgebung benachrichtigt werden und Ihre Reaktionen anpassen. 

In dieser Schnellstartanleitung wird ein [.NET-Beispielpaar](https://github.com/Azure-Samples/digital-twins-samples-csharp) verwendet, um ein imaginäres Bürogebäude zu digitalisieren und die Suche nach verfügbaren Räumen in diesem Gebäude zu veranschaulichen. Mit Digital Twins können Sie mehrere Sensoren mit Ihrer Umgebung verknüpfen. Neben der Raumverfügbarkeit können Sie dank eines simulierten CO2-Sensors auch ermitteln, ob die Luftqualität des verfügbaren Raums gut genug ist. Eine der Beispielanwendungen generiert willkürliche Sensordaten, damit Sie sich das Szenario besser vorstellen können.

## <a name="prerequisites"></a>Voraussetzungen

1. Sollten Sie kein Azure-Konto haben, erstellen Sie zunächst ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

1. Die beiden in dieser Schnellstartanleitung ausgeführten Konsolenanwendungen sind in C# geschrieben. Auf dem Entwicklungscomputer muss mindestens die [.NET Core SDK-Version 2.1.403](https://www.microsoft.com/net/download) installiert sein. Wenn das .NET Core SDK bereits installiert ist, können Sie an einer Eingabeaufforderung den Befehl `dotnet --version` ausführen, um die aktuelle Version von C# zu überprüfen.

1. Laden Sie das [C#-Beispielprojekt](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) herunter, und extrahieren Sie das ZIP-Archiv „digital-twins-samples-csharp-master.zip“. 


## <a name="create-a-digital-twins-instance"></a>Erstellen einer Digital Twins-Instanz

Erstellen Sie über das [Portal](https://portal.azure.com) mithilfe der Schritte in diesem Abschnitt eine neue Instanz von Digital Twins.

[!INCLUDE [create-digital-twins-portal](../../includes/create-digital-twins-portal.md)]

## <a name="set-permissions-for-your-app"></a>Festlegen von Berechtigungen für Ihre App

In diesem Abschnitt wird Ihre Beispielanwendung bei Azure Active Directory (AAD) registriert, damit sie auf Ihre Digital Twins-Instanz zugreifen kann. Wenn Sie bereits über eine AAD-App-Registrierung verfügen, können Sie diese für Ihr Beispiel wiederverwenden. Achten Sie dabei jedoch darauf, dass sie wie in diesem Abschnitt beschrieben konfiguriert ist. 

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="build-application"></a>Erstellen der Anwendung

Gehen Sie wie folgt vor, um die Belegungsanwendung zu erstellen:

1. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Ordner, in dem die Dateien aus dem ZIP-Archiv „digital-twins-samples-csharp-master.zip“ extrahiert wurden.
1. Führen Sie `cd occupancy-quickstart/src`aus.
1. Führen Sie `dotnet restore`aus.
1. Bearbeiten Sie *appSettings.json*, um folgende Variablen zu aktualisieren:
    - *ClientId*: Geben Sie die *Anwendungs-ID* Ihrer AAD-App-Registrierung ein, die Sie sich im vorherigen Abschnitt notiert haben.
    - *Tenant*: Geben Sie die *Verzeichnis-ID* Ihres AAD-Mandanten ein, die Sie sich ebenfalls im vorherigen Abschnitt notiert haben.
    - *BaseUrl*: Die URL der *Verwaltungs-API* Ihrer Digital Twins-Instanz (Format: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`). Ersetzen Sie die Platzhalter in dieser URL durch die Werte für Ihre Instanz aus dem vorherigen Abschnitt.

## <a name="provision-graph"></a>Bereitstellen des Graphen

In diesem Schritt wird Ihr Digital Twins-Raumgraph mit mehreren Bereichen, einem Gerät, zwei Sensoren, einer benutzerdefinierten Funktion und einer Rollenzuweisung bereitgestellt. Zur Bereitstellung des Raumgraphen wird die Datei [*provisionSample.yaml*](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) verwendet.

1. Führen Sie `dotnet run ProvisionSample`aus.
    >[!NOTE]
    >Wir verwenden das Azure CLI-Tool für die Geräteanmeldung, um den Benutzer bei Azure AD zu authentifizieren. Der Benutzer muss einen bestimmten Code eingeben, um sich über die [Microsoft-Anmeldeseite](https://microsoft.com/devicelogin) zu authentifizieren. Führen Sie nach der Codeeingabe die Schritte für die Authentifizierung aus. Wenn das Tool ausgeführt wird, wird der Benutzer jedes Mal zur Authentifizierung aufgefordert.
    
    >[!TIP]
    > Sollte bei diesem Schritt der folgende Fehler auftreten, vergewissern Sie sich, dass Ihre Variablen ordnungsgemäß kopiert wurden. 
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`


1. Der Bereitstellungsschritt kann einige Minuten dauern. In diesem Schritt wird auch eine IoT Hub-Instanz in Ihrer Digital Twins-Instanz bereitgestellt, und es wird eine Schleife durchlaufen, bis der IoT Hub-Status `Running` lautet.

    ![Bereitstellen des Beispiels][4]

1. Kopieren Sie am Ende der Ausführung die Verbindungszeichenfolge (`ConnectionString`) des Geräts, um sie im Gerätesimulatorbeispiel verwenden zu können. Kopieren Sie nur die in der folgenden Abbildung markierte Zeichenfolge:

    ![Bereitstellen des Beispiels][1]

## <a name="send-sensor-data"></a>Senden von Sensordaten

Gehen Sie zum Erstellen und Ausführen der Sensorsimulatoranwendung wie folgt vor:

1. Öffnen Sie eine neue Eingabeaufforderung, und navigieren Sie zu dem heruntergeladenen Projekt im Ordner „digital-twins-samples-csharp-master“.
1. Führen Sie `cd device-connectivity`aus.
1. Führen Sie `dotnet restore`aus.
1. Bearbeiten Sie *appsettings.json*, um *DeviceConnectionString* mit der obigen Verbindungszeichenfolge (`ConnectionString`) zu aktualisieren.
1. Führen Sie `dotnet run` aus, um mit dem Senden von Sensordaten zu beginnen. Daraufhin sollten die Daten an den Digital Twins-Dienst gesendet werden, wie in der folgenden Abbildung zu sehen:

     ![Gerätekonnektivität][2]

1. Führen Sie diesen Simulator weiter aus, um Ergebnisse parallel zur Aktion des nächsten Schritts anzeigen zu können. In diesem Fenster werden die simulierten, an Digital Twins gesendeten Sensordaten angezeigt. Im nächsten Schritt wird in Echtzeit eine Abfrage ausgeführt, um verfügbare Räume mit frischer Luft zu finden.

    >[!TIP]
    > Sollte bei diesem Schritt der folgende Fehler auftreten, vergewissern Sie sich, dass Ihre Geräteverbindungszeichenfolge (`DeviceConnectionString`) ordnungsgemäß kopiert wurde.  
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Suchen nach verfügbaren Bereichen mit frischer Luft

Das Sensorbeispiel simuliert willkürliche Datenwerte für zwei Sensoren: einen Bewegungssensor und einen CO2-Sensor. Verfügbare Bereiche mit frischer Luft sind in unserem Beispiel als Räume definiert, in denen sich niemand aufhält und deren CO2-Wert unter 1.000 ppm liegt. Ist die Bedingung nicht erfüllt, ist der Bereich nicht verfügbar oder die Luftqualität zu schlecht.

1. Öffnen Sie die Eingabeaufforderung, die Sie weiter oben zum Ausführen des Bereitstellungsschritts verwendet haben.
1. Führen Sie `dotnet run GetAvailableAndFreshSpaces`aus.
1. Sehen Sie sich diese Eingabeaufforderung und die Eingabeaufforderung für die Sensordaten nebeneinander an, wie weiter unten dargestellt. 
1. Eine Eingabeaufforderung sendet alle fünf Sekunden simulierte Bewegungs- und CO2-Daten an Digital Twins. Der andere Befehl liest den Graphen in Echtzeit, um auf der Grundlage willkürlich simulierter Daten verfügbare Räume mit frischer Luft zu ermitteln. Abhängig von den zuletzt übermittelten Sensordaten wird nahezu in Echtzeit eine der folgenden Bedingungen angezeigt:
    - Verfügbare Räume mit frischer Luft
    - Belegte Räume oder schlechte Luftqualität

     ![Abrufen verfügbarer Bereiche mit frischer Luft][3]

Öffnen Sie [Visual Studio Code](https://code.visualstudio.com/Download) mit dem Codearbeitsbereich-Projekt in „digital-twins-samples-csharp“ (siehe unten angegebener Befehl), um die Vorgänge in dieser Schnellstartanleitung und die aufgerufenen APIs nachzuvollziehen. Die Tutorials gehen ausführlich auf den Code ein und zeigen, wie Sie Konfigurationsdaten ändern und welche APIs aufgerufen werden. Weitere Informationen zu Verwaltungs-APIs finden Sie auf Ihrer Digital Twins Swagger-Seite (`https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net//management/swagger`) bzw. [hier](https://docs.westcentralus.azuresmartspaces.net/management/swagger). 

```
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den Tutorials wird ausführlich erläutert, wie Sie eine Anwendung für Gebäudetechniker erstellen, um die Produktivität von Personen zu steigern und das Gebäude effizienter zu nutzen.

Wenn Sie mit den Tutorials fortfahren möchten, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die im Rahmen dieser Schnellstartanleitung erstellt wurden:

1. Löschen Sie den Ordner, der beim Herunterladen des Beispielrepositorys erstellt wurde.
1. Klicken Sie im [Azure-Portal](http://portal.azure.com) im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre Digital Twins-Ressource aus. Klicken Sie oben im Bereich **Alle Ressourcen** auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie sich einen Überblick über ein einfaches Szenario für die Suche nach Räumen mit guten Arbeitsbedingungen verschafft. Eine ausführlichere Betrachtung dieses Szenarios finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen von Azure Digital Twins und Konfigurieren eines Raumgraphen](tutorial-facilities-setup.md)

<!-- Images -->
[1]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png
[2]: media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png
[3]: media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png
[4]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png
