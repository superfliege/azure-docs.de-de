---
title: Über das Cognitive Services Speech SDK | Microsoft-Dokumentation
description: Eine Übersicht über die SDKs, die für den Speech-Dienst verfügbar sind.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: v-jerkin
ms.openlocfilehash: ed523493f456e65f7aa5d3ad33914e3e52cd7044
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113391"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Über das Cognitive Services Speech SDK

Das Cognitive Services Speech SDK (Software Development Kit) stellt Ihren Anwendungen nativen Zugriff auf die Funktionen des Speech-Diensts bereit, wodurch die Softwareentwicklung erleichtert wird. Derzeit stellt das SDK den Zugriff auf die **Spracherkennung**, die **Sprachübersetzung** und die **Absichtserkennung** bereit.

[!include[Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-windows-sdk"></a>Abrufen des Windows SDK

Die Windows-Version des Speech SDK enthält 32- und 64-Bit-Clientbibliotheken für C und C++ sowie verwaltete .NET-Bibliotheken für C#. Das SDK kann in Visual Studio mithilfe von NuGet installiert werden. Suchen Sie einfach nach `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Abrufen des Linux SDK

Stellen Sie sicher, dass Sie über den erforderlichen Compiler und über die erforderlichen Bibliotheken verfügen, indem Sie folgende Shellbefehle ausführen:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Diese Anweisungen setzen voraus, dass Sie Ubuntu 16.04 auf einem PC (x86 oder x64) ausführen. Für eine andere Ubuntu-Version oder eine andere Linux-Distribution müssen Sie diese Schritte an Ihre Umgebung anpassen.

[Laden Sie anschließend das SDK herunter](https://aka.ms/csspeech/linuxbinary), und entpacken Sie die Dateien in einem Verzeichnis Ihrer Wahl. In dieser Tabelle wird die Struktur des Ordners des SDK veranschaulicht.

|path|BESCHREIBUNG|
|-|-|
|`license.md`|Lizenz|
|`third-party-notices.md`|Hinweise zu Drittanbietern|
|`include`|Headerdateien für C und C++|
|`lib/x64`|Native x64-Bibliothek für die Verknüpfung mit Ihrer Anwendung|
|`lib/x86`|Native x86-Bibliothek für die Verknüpfung mit Ihrer Anwendung|

Wenn Sie eine Anwendung erstellen möchten, kopieren oder verschieben Sie die erforderlichen Binärdateien (und Bibliotheken) in Ihre Entwicklungsumgebung, und fügen Sie diese nach Bedarf in Ihren Buildprozess ein.

## <a name="get-the-java-sdk"></a>Abrufen des Java SDK

Das Java SDK für Android ist als [AAR (Android-Bibliothek)](https://developer.android.com/studio/projects/android-library) verpackt und enthält die erforderlichen Bibliotheken sowie die erforderlichen Android-Berechtigungen für die Verwendung.
Es wird in einem Maven-Repository unter `https://csspeechstorage.blob.core.windows.net/maven/` als Paket `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0` gehostet.
Um das Paket aus dem Android Studio-Projekt zu nutzen, nehmen Sie die folgenden Änderungen vor:

* Fügen Sie der Datei `build.gradle` auf Projektebene Folgendes im Abschnitt `repository` hinzu:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Fügen Sie der Datei `build.gradle` auf Modulebene Folgendes im Abschnitt `dependencies` hinzu:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:0.5.0'
  ```

Das Java SDK ist auch Teil des [Speech-Geräte-SDK](speech-devices-sdk.md).

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
