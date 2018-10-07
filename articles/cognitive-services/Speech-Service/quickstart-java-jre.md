---
title: 'Schnellstart: Erkennen von Sprache in Java (Windows oder Linux)'
titleSuffix: Microsoft Cognitive Services
description: Informationen zur Spracherkennung in Java (Windows oder Linux)
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: 00603c467ec96e52fc2b7745263153a68d20f584
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053961"
---
# <a name="quickstart-recognize-speech-in-java-on-windows-or-linux-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in Java unter Windows oder Linux mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine Java-Konsolenanwendung mit dem [Speech SDK](speech-sdk.md). Sie transkribieren über das Mikrofon Ihres Computers Sprache in Echtzeit in Text. Die Anwendung wird mit dem Maven-Paket für das Speech SDK und der Eclipse Java-IDE (v4.8) unter 64-Bit-Windows oder Ubuntu Linux 16.04 erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

> [!NOTE]
> Weitere Informationen zum SDK für sprachaktivierte Geräte und zum Roobo-Gerät finden Sie unter [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen einen Abonnementschlüssel für den Spracherkennungsdienst, um diesen Schnellstart abzuschließen. Sie können einen solchen Schlüssel kostenlos abrufen. Details dazu erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).


## <a name="create-and-configure-project"></a>Erstellen und Konfigurieren des Projekts

Wenn Sie Ubuntu 16.04 verwenden, führen Sie vor dem Starten von Eclipse die folgenden Befehle aus, um sicherzustellen, dass die erforderlichen Pakete installiert sind.

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

1. Starten Sie Eclipse.

1. Geben Sie im Eclipse-Startprogramm im Feld **Arbeitsbereich** den Namen eines neuen Arbeitsbereichsverzeichnisses ein. Wählen Sie dann **Starten** aus.

   ![Screenshot des Eclipse-Startprogramms](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Nach kurzer Zeit wird das Hauptfenster der Eclipse-IDE angezeigt. Schließen Sie den Begrüßungsbildschirm, wenn ein solcher vorhanden ist.

1. Erstellen Sie über die Eclipse-Menüleiste ein neues Projekt, indem Sie **Datei** > **Neu** > **Projekt** auswählen.

1. Das Dialogfeld **Neues Projekt** wird angezeigt. Wählen Sie **Java-Projekt** aus, und wählen Sie dann **Weiter** aus.

   ![Screenshot des Dialogfelds „Neues Projekt“ mit hervorgehobenem Java-Projekt](media/sdk/qs-java-jre-02-select-wizard.png)

1. Der Assistent für ein neues Java-Projekt wird gestartet. Geben Sie im Feld **Projektname** den Namen **Schnellstart** ein, und wählen Sie **JavaSE-1.8** als Ausführungsumgebung aus. Wählen Sie **Fertig stellen** aus.

   ![Screenshot des Assistenten für ein neues Java-Projekt](media/sdk/qs-java-jre-03-create-java-project.png)

1. Wenn das Fenster **Zugeordnete Perspektive öffnen?** angezeigt wird, wählen Sie **Perspektive öffnen** aus.

1. Klicken Sie im **Paket-Explorer** mit der rechten Maustaste auf das Projekt **Schnellstart**. Wählen Sie **Konfigurieren** > **In Maven-Projekt konvertieren** aus dem Kontextmenü aus.

   ![Screenshot des Paket-Explorers](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Das Fenster **Neue POM-Datei erstellen** wird angezeigt. Geben Sie im Feld **Gruppen-ID** **com.microsoft.cognitiveservices.speech.samples** und im Feld **Artefakt-ID** **Schnellstart** ein. Klicken Sie dann auf **Fertig stellen**.

   ![Screenshot des Fensters „Neue POM-Datei erstellen“](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Öffnen Sie die Datei **pom.xml**, und bearbeiten Sie diese.

   * Erstellen Sie vor dem schließenden Tag `</project>` am Ende der Datei einen Repositoryabschnitt mit einem Verweis auf das Maven-Repository für das Speech SDK. Das folgende Beispiel zeigt dies:

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Fügen Sie anschließend einen Abschnitt „Abhängigkeiten“ mit der Version 1.0.0 des Speech SDK als Abhängigkeit hinzu:

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

   * Speichern Sie die Änderungen.

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Um Ihrem Java-Projekt eine neue leere Klasse hinzuzufügen, wählen Sie **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** **speechsdk.quickstart** in das Feld **Paket** und **Main** in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](media/sdk/qs-java-jre-06-create-main-java.png)

1. Ersetzen Sie den Code in `Main.java` mit dem folgenden Ausschnitt:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.
Die nächsten 15 Sekunden der Spracheingabe vom Mikrofon werden erkannt und im Konsolenfenster protokolliert.

![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-java-jre-07-console-output.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/java-jre`.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für Java](how-to-recognize-intents-from-speech-java.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
