---
title: 'Schnellstart: Erkennen von Sprache in Java unter Android mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie Sie Sprache in Java unter Android mit dem Cognitive Services Speech SDK erkennen können.
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: fmegen
ms.openlocfilehash: ad1bf15f198529487dbed92d8f8639704b0c5fc7
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325124"
---
# <a name="quickstart-recognize-speech-in-java-on-android-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in Java unter Android mit dem Speech SDK

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel enthält Informationen zur Erstellung einer Java-Anwendung für Android mithilfe des Cognitive Services Speech SDK zum Umwandeln von Sprache in Text.
Die Anwendung basiert auf dem Maven-Paket für das Microsoft Cognitive Services Speech SDK, Version 0.5.0, und Android Studio 3.1.

> [!NOTE]
> Besuchen Sie für das Speech-Geräte-SDK und das Roobo-Gerät die Seite [Speech-Geräte-SDK](speech-devices-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein PC (Windows, Linux, Mac), auf dem Android Studio ausgeführt werden kann.
* Version 3.1 von [Android Studio](https://developer.android.com/studio/).
* Ein ARM-basiertes Android-Gerät (API 23: Android 6.0 Marshmallow oder höher), [aktiviert für die Entwicklung](https://developer.android.com/studio/debug/dev-options), mit einem funktionsfähigen Mikrofon.

## <a name="create-an-android-studio-project"></a>Erstellen eines Android Studio-Projekts

Starten Sie Android Studio, und wählen Sie **Start a new Android Studio project** (Neues Android Studio-Projekt starten) aus.

![](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

Wählen Sie im dann angezeigten Assistenten zum **Erstellen eines neuen Projekts** die folgenden Optionen aus:

1. Geben Sie auf dem Bildschirm **Create Android Project** (Android-Projekt erstellen) **Quickstart** als **Anwendungsname** und **samples.speech.cognitiveservices.microsoft.com** als **Unternehmensdomäne** ein. Wählen Sie dann einen Projektspeicherort aus. Lassen Sie die Kontrollkästchen deaktiviert, und klicken Sie auf **Next** (Weiter).

   ![](media/sdk/qs-java-android-02-create-android-project.png)

1. Aktivieren Sie auf dem Bildschirm **Target Android Devices** (Android-Zielgeräte) nur die Option **Phone and Tablet** (Telefon und Tablet), wählen Sie aus der Dropdownliste darunter **API 23: Android 6.0 (Marshmallow)** aus, und klicken Sie auf **Next** (Weiter).

   ![](media/sdk/qs-java-android-03-target-android-devices.png)

1. Wählen Sie auf dem Bildschirm **Add an Activity to Mobile** (Eine Aktivität für ein mobiles Gerät hinzufügen) die Option **Empty Activity** (Leere Aktivität) aus, und klicken Sie auf **Next** (Weiter).

   ![](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. Verwenden Sie auf dem Bildschirm **Configure Activity** (Aktivität konfigurieren) **MainActivity** als Aktivitätsnamen und **activity\_main** als Layoutnamen. Aktivieren Sie die beiden Kontrollkästchen, und klicken Sie auf **Finish** (Fertig stellen).

   ![](media/sdk/qs-java-android-05-configure-activity.png)

Nach einer Weile sollte Ihr neu erstelltes Android Studio-Projekt angezeigt werden.

## <a name="configure-your-project-for-the-speech-sdk"></a>Konfigurieren des Projekts für das Speech SDK

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Die aktuelle Version des Cognitive Services Speech SDK ist `0.5.0`.

Das Speech SDK für Android ist als [AAR (Android-Bibliothek)](https://developer.android.com/studio/projects/android-library) verpackt und enthält die erforderlichen Bibliotheken sowie die erforderlichen Android-Berechtigungen für die Verwendung.
Es wird in einem Maven-Repository unter https://csspeechstorage.blob.core.windows.net/maven/ gehostet.

Wir beschreiben, wie Ihr Projekt eingerichtet werden muss, um das Speech SDK zu verwenden.

Öffnen Sie das Fenster mit der Projektstruktur unter **File** \> **Project Structure** (Datei > Projektstruktur).
Nehmen Sie im daraufhin angezeigten Fenster die folgenden Änderungen vor (klicken Sie erst auf **OK**, nachdem Sie alle Schritte abgeschlossen haben):

1. Wählen Sie **Project** (Projekt) aus, und bearbeiten Sie die Einstellungen für **Default Library Repository** (Standardbibliotheksrepository), indem Sie ein Komma und unsere Maven-Repository-URL `'https://csspeechstorage.blob.core.windows.net/maven/'` (in einfachen Anführungszeichen) anfügen:

  ![](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Wählen Sie auf demselben Bildschirm auf der linken Seite das Modul **App** und oben die Registerkarte **Dependencies** (Abhängigkeiten) aus. Klicken Sie anschließend oben rechts auf das grüne Pluszeichen, und wählen Sie **Library dependency** (Bibliotheksabhängigkeit) aus.

  ![](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Geben Sie im angezeigten Fenster den Namen und die Version unseres Speech SDK für Android ein: `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`. Klicken Sie dann auf **OK**.
   Das Speech SDK sollte nun der Liste der Abhängigkeiten hinzugefügt werden, wie unten dargestellt:

  ![](media/sdk/qs-java-android-08-dependency-added.png)

1. Wählen Sie im oberen Bereich die Registerkarte **Properties** (Eigenschaften) aus. Wählen Sie **1.8** für **Source Compability** (Quellenkompatibilität) und **Target Compatibility** (Zielkompatibilität) aus.

  ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Klicken Sie abschließend auf **OK**, um das Fenster **Project Structure** (Projektstruktur) zu schließen und alle Aktualisierungen zu übernehmen.

## <a name="create-a-minimal-ui"></a>Erstellen einer minimalen Benutzeroberfläche

Bearbeiten Sie das Layout Ihrer Hauptaktivität `activity_main.xml`.
Standardmäßig sollte sie mit einer Titelleiste mit dem Namen Ihrer Anwendung und einer TextView mit dem Text „Hello World!“ angezeigt werden.

* Klicken Sie auf die TextView. Ändern Sie deren ID-Attribut oben rechts in `hello`.

* Ziehen Sie aus der Palette oben links in Ihrem Fenster `activity_main.xml` eine Schaltfläche in den leeren Bereich über dem Text.

* Geben Sie in den Attributen der Schaltfläche auf der rechten Seite in den Wert für das `onClick`-Attribut `onSpeechButtonClicked` ein. Dies ist der Name des Schaltflächenhandlers.
  Ändern Sie dessen ID-Attribut oben rechts in `button`.

* Verwenden Sie das Zauberstabsymbol oben im Designer, wenn die Layouteinschränkungen für Sie abgeleitet werden sollen.

  ![](media/sdk/qs-java-android-10-infer-layout-constraints.png)

Der Text und die grafische Version Ihrer Benutzeroberfläche sollten jetzt etwa wie folgt aussehen:

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Bearbeiten Sie die Quelldatei `MainActivity.java`, und ersetzen Sie den Code durch Folgendes (unterhalb der Paketanweisung):

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * Die `onCreate`-Methode enthält Code, der Mikrofon- und Internetberechtigungen anfordert und die native Plattformbindung initialisiert. Das Konfigurieren der nativen Plattformbindungen ist nur einmal erforderlich, d.h., es sollte am Anfang während der Anwendungsinitialisierung erfolgen.
   
   * Die `onSpeechButtonClicked`-Methode war zuvor als Klickhandler verbunden. Das Drücken einer Schaltfläche löst die eigentliche Spracherkennung aus.

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

* Drücken Sie zum Erstellen STRG+F9, oder wählen Sie **Build** \> **Make Project** (Erstellen > Projekt erstellen) aus.

* Verbinden Sie Ihr Android-Gerät mit Ihrem Entwicklungs-PC. Stellen Sie sicher, dass Sie den [Entwicklungsmodus und USB-Debuggen aktiviert haben](https://developer.android.com/studio/debug/dev-options).

* Um die App zu starten, drücken Sie UMSCHALT+F10, oder wählen Sie **Run** \> **Run 'app'** (Ausführen > „App“ ausführen) aus.

* Wählen Sie im daraufhin angezeigten Fenster mit Bereitstellungszielen Ihr Android-Gerät aus.

  ![Starten der Anwendung im Debugger](media/sdk/qs-java-android-12-deploy.png)

* Die App sollte auf Ihrem Gerät gestartet werden.
  Wenn Sie auf die Schaltfläche drücken, werden die folgenden 15 Sekunden erkannt und in der Benutzeroberfläche angezeigt (Sie sollten die Antwort auch in Ihrem Logcat-Fenster in Android Studio sehen können):

  ![Benutzeroberfläche nach erfolgreicher Erkennung](media/sdk/qs-java-android-13-gui-on-device.png)

Mit diesem Screenshot ist der Android-Schnellstart abgeschlossen. Der vollständige Beispielcode des Projekts kann aus dem Beispielrepository heruntergeladen werden.

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/java-android`.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen der Beispiele](speech-sdk.md#get-the-samples)
