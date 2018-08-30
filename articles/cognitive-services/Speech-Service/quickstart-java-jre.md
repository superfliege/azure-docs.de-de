---
title: 'Schnellstart: Erkennen von Sprache in Java (Windows oder Linux)'
titleSuffix: Microsoft Cognitive Services
description: Informationen zur Spracherkennung in Java (Windows oder Linux)
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 08/16/2018
ms.author: fmegen
ms.openlocfilehash: 11aba1ae6f49d6c00fabd928ae3aefedcbea8ed8
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234262"
---
# <a name="quickstart-recognize-speech-in-java-windows-or-linux"></a>Schnellstart: Erkennen von Sprache in Java (Windows oder Linux)

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Dokument wird beschrieben, wie eine auf Java basierende Konsolenanwendung für die Java-Runtime Environment (JRE) erstellt wird, die Speech SDK verwendet.
Die Anwendung basiert auf dem Maven-Paket für das Microsoft Cognitive Services SDK.
Wir verwenden Eclipse als integrierte Entwicklungsumgebung (IDE).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* PC (Windows x64, Ubuntu 16.04 x64) zum Ausführen von Eclipse mit einem funktionsfähigen Mikrofon
* 64-Bit-JRE/JDK für Java 8
* Version 4.8 von [Eclipse](https://www.eclipse.org), 64-Bit-Version
* Führen Sie für die Installation der erforderlichen Pakete unter Ubuntu 16.04 die folgenden Befehle aus:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="create-and-configure-your-project"></a>Erstellen und Konfigurieren des Projekts

1. Starten Sie Eclipse.

1. Geben Sie im Startprogramm für Eclipse den Namen von einem neuen Verzeichnis in das **Arbeitsbereich**-Feld ein.
   Klicken Sie dann auf **Starten**.

   ![Erstellen des Eclipse-Arbeitsbereichs](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Nach einer Weile wird das Hauptfenster der Eclipse-IDE angezeigt.
   Schließen Sie dieses, wenn eine Willkommensseite angezeigt wird.

1. Klicken Sie auf **Datei** \> **Neu** \> **Projekt**.

1. Wählen Sie im angezeigten **Neues Projekt**-Assistenten **Java-Projekt** aus, und klicken Sie auf **Weiter**.

   ![Auswählen eines Assistenten](media/sdk/qs-java-jre-02-select-wizard.png)

1. Geben Sie im nächsten Fenster **quickstart** als Projektnamen ein, und wählen Sie **JavaSE-1.8** (oder höher) als Ausführungsumgebung aus.
   Klicken Sie auf **Fertig stellen**.

   ![Auswählen eines Assistenten](media/sdk/qs-java-jre-03-create-java-project.png)

1. Wenn ein Fenster mit dem Titel **Open Associated Perspective?** (Zugeordnete Perspektive öffnen) erscheint, klicken Sie auf **Open Perspective** (Perspektive öffnen).

1. Klicken Sie im **Paket-Explorer** mit der rechten Maustaste auf das **quickstart**-Projekt, und klicken Sie dann auf **Konfigurieren** \> **Konvertieren in Maven-Projekt**.

   ![Konvertieren in Maven-Projekt](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Geben Sie im eingeblendeten Fenster **com.microsoft.cognitiveservices.speech.samples** als **Gruppen-ID** und **quickstart** als **Artefakt-ID** ein. Wählen Sie **Fertig stellen** aus.

   ![Konfigurieren von Maven-POM](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Bearbeiten Sie die **pom.xml**-Datei.

  * Erstellen Sie vor dem schließenden Tag `</project>` am Ende der Datei einen Repositoryabschnitt mit einem Verweis auf das Maven-Repository für das Sprach-SDK.

    [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Fügen Sie anschließend einen Abschnitt „Abhängigkeiten“ mit der Sprach-SDK-Version 0.6.0 als Abhängigkeit hinzu:

    [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

  * Speichern Sie die Änderungen.

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Klicken Sie auf **Datei** \> **Neu** \> **Klasse**, um Ihrem Java-Projekt eine neue Klasse hinzuzufügen.

1. Geben Sie im Fenster **Neue Java-Klasse** **speechsdk.quickstart** in das **Paket**-Feld und **Main** in das **Name**-Feld ein.

   ![Erstellen einer Main-Klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Ersetzen Sie den Code in `Main.java` mit dem folgenden Ausschnitt:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Drücken Sie F11, oder klicken Sie auf **Ausführen** \> **Debuggen**.
Die nächsten 15 Sekunden der Spracheingabe vom Mikrofon werden erkannt und im Konsolenfenster protokolliert.

![Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-java-jre-07-console-output.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/java-jre`.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen der Beispiele](speech-sdk.md#get-the-samples)
