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
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: b9b7b8af5ce3d75788fd2c4f5e0309b5ca561a8f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35378411"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Über das Cognitive Services Speech SDK

Das Cognitive Services Speech SDK (Software Development Kit) stellt Ihren Anwendungen nativen Zugriff auf die Funktionen des Speech-Diensts bereit, wodurch die Softwareentwicklung erleichtert wird. Derzeit stellt das SDK den Zugriff auf die **Spracherkennung**, die **Sprachübersetzung** und die **Absichtserkennung** bereit.

In der Tabelle werden die derzeit unterstützten Programmiersprachen und Betriebssysteme aufgeführt.

|Unterstütztes Betriebssystem|Programmiersprache|
|-|-|
|Windows|C/C++, C#|
|Linux|C/C++|
|Geräte|Java\*|

\* *Das Java SDK ist Teil des [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).*

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

|Pfad|Beschreibung|
|-|-|
|`license.md`|Lizenz|
|`third-party-notices.md`|Hinweise zu Drittanbietern|
|`include`|Headerdateien für C und C++|
|`lib/x64`|Native x64-Bibliothek für die Verknüpfung mit Ihrer Anwendung|
|`lib/x86`|Native x86-Bibliothek für die Verknüpfung mit Ihrer Anwendung|

Wenn Sie eine Anwendung erstellen möchten, kopieren oder verschieben Sie die erforderlichen Binärdateien (und Bibliotheken) in Ihre Entwicklungsumgebung, und fügen Sie diese nach Bedarf in Ihren Buildprozess ein.

## <a name="get-the-java-sdk"></a>Abrufen des Java SDK

Das Java SDK ist Teil des [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [See how to recognize speech in C# (Erkennen von Sprache in C#)](quickstart-csharp-windows.md)
