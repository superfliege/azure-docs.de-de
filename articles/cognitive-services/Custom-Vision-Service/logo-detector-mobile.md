---
title: 'Tutorial: Verwenden der benutzerdefinierten Logoerkennung zum Erkennen von Azure-Diensten: Custom Vision'
titlesuffix: Azure Cognitive Services
description: In diesem Tutorial arbeiten Sie Schritt für Schritt eine Beispiel-App durch, die Azure Custom Vision im Rahmen eines Szenarios zur Logoerkennung verwendet. Erfahren Sie, wie Custom Vision mit anderen Komponenten verwendet wird, um eine End-to-End-Anwendung bereitzustellen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pafarley
ms.openlocfilehash: 520e1981544f6ad0a6f1412a47dc8ea50fdd53a5
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55771434"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Tutorial: Erkennen von Azure-Dienstlogos in Bildern von der Kamera

In diesem Tutorial arbeiten Sie Schritt für Schritt eine Beispiel-App durch, die Azure Custom Vision im Rahmen eines größeren Szenarios verwendet. Die AI Visual Provision-App (eine Xamarin.Forms-App für mobile Plattformen) analysiert Kamerabilder von Azure-Dienstlogos und stellt dann die eigentlichen Dienste im Azure-Konto des Benutzers bereit. Hier erfahren Sie, wie Custom Vision in Verbindung mit anderen Komponenten verwendet wird, um eine zweckmäßige End-to-End-Anwendung bereitzustellen. Sie können die gesamte App selbst ausführen oder einfach den Custom Vision-Teil des Setups durchführen und erkunden, wie dieser von der App genutzt wird.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> - Erstellen einer benutzerdefinierten Objekterkennung zum Erkennen von Azure-Dienstlogos
> - Verbinden Ihrer App mit der Maschinelles Sehen-API von Azure und Custom Vision
> - Erstellen eines Azure-Dienstprinzipalkontos zum Bereitstellen von Azure-Diensten über die App

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- Xamarin-Workload für Visual Studio (siehe [Installieren von Xamarin](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- iOS- oder Android-Emulator für Visual Studio
- [Azure-Befehlszeilenschnittstelle (Azure CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (optional)

## <a name="get-the-source-code"></a>Herunterladen des Quellcodes

Wenn Sie die bereitgestellte Web-App verwenden möchten, können Sie den Quellcode der App aus dem Repository für [AI Visual Provision](https://github.com/Microsoft/AIVisualProvision) auf GitHub klonen oder herunterladen. Öffnen Sie die Datei *Source/VisualProvision.sln* in Visual Studio. Sie werden später die erforderlichen Änderungen an einigen der Projektdateien vornehmen, um die App ausführen zu können.

## <a name="create-an-object-detector"></a>Erstellen einer Objekterkennung

Melden Sie sich bei der [Custom Vision-Website](https://customvision.ai/) an, und erstellen Sie ein neues Projekt. Geben Sie ein Projekt zur Objekterkennung an, und verwenden Sie die Domäne „Logo“ (dadurch kann der Dienst einen für die Logoerkennung optimierten Algorithmus verwenden). 

![Dialogfeld für ein neues Projekt auf der Custom Vision-Website im Chrome-Browser](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Als Nächstes müssen Sie den Algorithmus für die Logoerkennung trainieren, indem Sie Bilder von Azure-Dienstlogos hochladen und manuell markieren bzw. taggen. Im AIVisualProvision-Repository finden Sie eine Reihe von Trainingsbildern, die Sie hierfür verwenden können. Klicken Sie auf der Website auf der Registerkarte **Training Images** (Trainingsbilder) auf die Schaltfläche **Bilder hinzufügen**, und navigieren Sie dann zum Ordner **Documents/Images/Training_DataSet** des Repositorys. Sie müssen die Logos manuell in jedem Bild markieren. Wenn Sie dieses Projekt nur testen möchten, können Sie daher ggf. nur eine Teilmenge der Bilder hochladen. Sie müssen mindestens 15 Instanzen jedes Tags hochladen, das Sie verwenden möchten.

Nachdem Sie die Trainingsbilder hochgeladen haben, wählen Sie das erste Bild in der Anzeige aus. Daraufhin wird das Fenster zum Anwenden der Markierung angezeigt. Zeichnen Sie Rahmen, und weisen Sie Tags für die Logos in jedem Bild zu. 

![Bild von Logos, auf die auf der Custom Vision-Website Tags angewendet werden](media/azure-logo-tutorial/tag-logos.png)

Die App ist für die Verwendung mit bestimmten Tagzeichenfolgen konfiguriert. Die entsprechenden Definitionen finden Sie in der Datei *Source\VisualProvision\Services\Recognition\RecognitionService.cs*:

[!code-csharp[tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?range=18-33)]

Wenn Sie ein Bild markiert haben, navigieren Sie nach rechts, um das nächste Bild zu markieren. Schließen Sie das Fenster zum Anwenden der Markierung, wenn Sie fertig sind.

## <a name="train-the-object-detector"></a>Trainieren der Objekterkennung

Legen Sie im linken Bereich die Option **Tags** auf **Tagged** (Markiert) fest. Nun sollten alle Bilder angezeigt werden. Klicken Sie anschließend auf die grüne Schaltfläche am oberen Rand der Seite, um das Modell zu trainieren. Dadurch wird der Algorithmus dafür trainiert, die gleichen Tags in neuen Bildern zu erkennen. Zudem wird das Modell anhand einiger Ihrer vorhandenen Bilder getestet, um Genauigkeitsbewertungen zu generieren.

![Registerkarte „Training Images“ (Trainingsbilder) der Custom Vision-Website mit der Schaltfläche „Train“ (Trainieren)](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Abrufen der Vorhersage-URL

Nachdem Sie das Modell trainiert haben, können Sie es in Ihre App integrieren. Zu diesem Zweck müssen Sie die Endpunkt-URL (die Adresse des Modells, das von der App abgefragt wird) und den Vorhersageschlüssel (um der App Zugriff auf Vorhersageanforderungen zu gewähren) abrufen. Klicken Sie auf der Registerkarte **Leistung** oben auf der Seite auf die Schaltfläche **Prediction URL** (Vorhersage-URL).

![Custom Vision-Website mit einem Bildschirm für die Vorhersage-API, auf dem eine URL-Adresse und ein API-Schlüssel angezeigt werden](media/azure-logo-tutorial/cusvis-endpoint.png)

Kopieren Sie die URL der Bilddatei und den Wert `Prediction-key` in die entsprechenden Felder in der Datei *Source\VisualProvision\AppSettings.cs*:

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=22-26)]

## <a name="examine-custom-vision-usage"></a>Überprüfen der Custom Vision-Nutzung

Öffnen Sie die Datei *Source/VisualProvision/Services/Recognition/CustomVisionService.cs*, um zu überprüfen, wie Ihr Custom Vision-Schlüssel und Ihre Endpunkt-URL in der App verwendet werden. Die **PredictImageContentsAsync**-Methode akzeptiert einen Bytestream einer Bilddatei sowie ein Abbruchtoken (für die asynchrone Aufgabenverwaltung), ruft die Custom Vision-Vorhersage-API auf und gibt das Ergebnis der Vorhersage zurück. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?range=12-28)]

Dieses Ergebnis wird in Form einer **PredictionResult**-Instanz zurückgegeben, die eine Liste von **Vorhersageinstanzen** enthält. Eine **Vorhersage** enthält ein erkanntes Tag und den zugehörigen Begrenzungsrahmen für die Position im Bild.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?range=3-12)]

Wenn Sie mehr darüber erfahren möchten, wie die App diese Daten verarbeitet, beginnen Sie mit der **GetResourcesAsync**-Methode, die in der Datei *Source/VisualProvision/Services/Recognition/RecognitionService.cs* definiert ist. 

## <a name="add-computer-vision"></a>Hinzufügen von maschinellem Sehen

Der Custom Vision-Teil des Tutorials ist hiermit abgeschlossen. Wenn Sie die App ausführen möchten, müssen Sie jedoch auch den Dienst für maschinelles Sehen integrieren. Die App verwendet die Texterkennungsfunktion der Maschinelles Sehen-API, um den Logoerkennungsprozess zu ergänzen. Ein Azure-Logo kann anhand seiner Darstellung _oder_ des Texts in der Nähe des Logos erkannt werden. Anders als Custom Vision-Modelle wird das maschinelle Sehen vorab trainiert, um bestimmte Vorgänge für Bilder oder Videos auszuführen.

Sie müssen lediglich den Dienst für maschinelles Sehen abonnieren, um einen Schlüssel und eine Endpunkt-URL zu erhalten. Falls Sie Hilfe bei diesem Schritt benötigen, finden Sie unter [Abrufen von Abonnementschlüsseln](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe) weitere Informationen.

![Der Dienst für maschinelles Sehen im Azure-Portal mit ausgewähltem Schnellstartmenü (hier sind ein Link für Schlüssel und die API-Endpunkt-URL hervorgehoben)](media/azure-logo-tutorial/comvis-keys.png)

Öffnen Sie dann die Datei *Source\VisualProvision\AppSettings.cs*, und füllen Sie die Variablen `ComputerVisionEndpoint` und `ComputerVisionKey` mit den richtigen Werten auf.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=28-32)]


## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Die App erfordert ein Azure-Dienstprinzipalkonto, um Dienste in Ihrem Azure-Abonnement bereitzustellen. Mit einem Dienstprinzipal können Sie mithilfe der rollenbasierten Zugriffssteuerung bestimmte Berechtigungen an eine App delegieren. Im [Leitfaden für Dienstprinzipale ](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals) finden Sie bei Bedarf weitere Informationen.

Sie können einen Dienstprinzipal wie im Folgenden beschrieben mit Azure Cloud Shell oder mit der Azure CLI erstellen. Melden Sie sich zunächst an, und wählen Sie das Abonnement aus, das Sie verwenden möchten.

```console
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Erstellen Sie dann den Dienstprinzipal (beachten Sie, dass dieser Vorgang etwas dauern kann).

```console
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

Nach der erfolgreichen Erstellung sollte die folgende JSON-Ausgabe mit den erforderlichen Anmeldeinformationen angezeigt werden.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```
Notieren Sie sich die Werte von `clientId` und `tenantId`. Kopieren Sie die Werte in die entsprechenden Felder in der Datei *Source\VisualProvision\AppSettings.cs*.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=8-16)]

## <a name="run-the-app"></a>Ausführen der App

An diesem Punkt haben Sie der App Zugriff auf Folgendes gewährt:
* Ein trainiertes Custom Vision-Modell
* Den Dienst für maschinelles Sehen
* Ein Dienstprinzipalkonto 

Gehen Sie folgendermaßen vor, um die App auszuführen:

1. Wählen Sie in Visual Studio im Projektmappen-Explorer das Projekt „VisualProvision.Android“ oder „VisualProvision.iOS“ aus, und wählen Sie dann im Dropdownmenü auf der Hauptsymbolleiste einen entsprechenden Emulator oder ein verbundenes mobiles Gerät aus (zum Ausführen eines iOS-Emulators ist ein MacOS-Gerät erforderlich). Führen Sie anschließend die App aus.

1. Geben Sie auf dem ersten Bildschirm, der geladen wird, die Client-ID Ihres Dienstprinzipals, die Mandanten-ID und das Kennwort ein. Klicken Sie auf die Schaltfläche **Anmelden**.

    > [!NOTE]
    > Bei einigen Emulatoren wird die Schaltfläche **Anmelden** möglicherweise in diesem Schritt nicht aktiviert. Beenden Sie in diesem Fall die App, öffnen Sie die Datei _Source/VisualProvision/Pages/LoginPage.xaml_, suchen Sie nach dem `Button`-Element mit der Bezeichnung „LOGIN BUTTON“, und entfernen Sie die Zeile:
      ```xaml
      IsEnabled="{Binding IsValid}"
      ```
    
    > Führen Sie die App dann erneut aus.

    ![App-Bildschirm mit Feldern für die Anmeldeinformationen des Dienstprinzipals](media/azure-logo-tutorial/app-credentials.png)

1. Wählen Sie auf dem nächsten Bildschirm Ihr Azure-Abonnement im Dropdownmenü aus (das Dropdownmenü sollte alle Abonnements enthalten, auf die der Dienstprinzipal Zugriff hat). Klicken Sie auf die Schaltfläche **Weiter**.

    ![App-Bildschirm mit einem Dropdownfeld für das Azure-Zielabonnement](media/azure-logo-tutorial/app-az-subscription.png)

    Die App fordert Sie nun ggf. auf, den Geräten Zugriff auf die Kamera und den Fotospeicher zu gewähren. Akzeptieren Sie diese Berechtigungen.

1. Als Nächstes wird die Kamera auf Ihrem Gerät aktiviert. Nehmen Sie ein Foto von einem der Azure-Dienstlogos auf, die Sie trainiert haben. Sie sollten nun in einem Bereitstellungsdialogfeld aufgefordert werden, eine Region und eine Ressourcengruppe für die neuen Dienste auszuwählen (wie bei der Bereitstellung über das Azure-Portal). 

    ![Smartphone-Kamerabildschirm mit zwei Ausschnitten von Azure-Logos in der Ansicht](media/azure-logo-tutorial/app-camera-capture.png)

    ![App-Bildschirm mit Feldern für die Eingabe der Region und Ressourcengruppe für die Bereitstellung](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Wenn Sie alle Schritte dieses Szenarios ausgeführt und die App zum Bereitstellen von Azure-Diensten in Ihrem Konto verwendet haben, müssen Sie abschließend im [Azure-Portal](https://ms.portal.azure.com/) die Dienste stornieren, die Sie nicht nutzen möchten.

Wenn Sie vorhaben, in Zukunft ein eigenes Objekterkennungsprojekt mit Custom Vision zu erstellen, sollten Sie auch das Logoerkennungsprojekt löschen, das Sie in diesem Tutorial erstellt haben. In der kostenlosen Testversion von Custom Vision können zwei Projekte erstellt werden. Navigieren Sie auf der [Custom Vision-Website](https://customvision.ai) zu **Projekte**, und klicken Sie unter **My New Project** (Mein neues Projekt) auf das Papierkorbsymbol.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine voll funktionsfähige Xamarin.Forms-App eingerichtet und kennen gelernt, die den Custom Vision Service zum Erkennen von Logos in Bildern von der Kamera eines mobilen Geräts verwendet. Befassen Sie sich als Nächstes mit den bewährten Methoden für das Erstellen eines Custom Vision-Modells, damit Sie eine eigene leistungsfähige App mit hoher Genauigkeit erstellen können.

> [!div class="nextstepaction"]
> [Verbessern der Klassifizierung](getting-started-improving-your-classifier.md)