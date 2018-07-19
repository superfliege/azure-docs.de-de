---
title: Azure Cognitive Services, Dokumentation zur Cognitive Services Speech SDK-API – Tutorials, API-Referenz | Microsoft-Dokumentation
description: Informationen zum Erstellen und Entwickeln von Apps mit dem Cognitive Services Speech SDK
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: fe171ba9f6f0ff36a7c23c47f145d83f7a94fb5d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069487"
---
# <a name="shipping-an-application"></a>Versenden einer Anwendung

Beachten Sie bei der Verteilung des Cognitive Services Speech SDK die [Lizenzbedingungen für das Speech SDK](license.md) sowie die [Hinweise und Informationen zu Drittanbietersoftware](third-party-notices.md). Lesen Sie außerdem die [Datenschutzerklärung von Microsoft](https://aka.ms/csspeech/privacy).

Je nach Plattform bestehen unterschiedliche Abhängigkeiten zum Ausführen Ihrer Anwendung.

## <a name="windows"></a>Windows

Das Cognitive Services Speech SDK wird unter Windows 10 und unter Windows Server 2016 getestet.

Für das Cognitive Services Speech SDK muss [Microsoft Visual C++ Redistributable für Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) auf dem System installiert sein. Installationsprogramme für die neueste Version von `Microsoft Visual C++ Redistributable for Visual Studio 2017` können Sie hier herunterladen:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Wenn in der Anwendung verwalteter Code verwendet wird, muss `.Net Framework 4.6.1` oder höher auf dem Zielcomputer installiert sein.

Für die Mikrofoneingabe müssen die Media Foundation-Bibliotheken installiert werden. Diese Bibliotheken sind Bestandteil von Windows 10 und Windows Server 2016. Das Speech SDK kann ohne diese Bibliotheken verwendet werden, wenn als Audioeingabegerät kein Mikrofon verwendet wird.

Die erforderlichen Speech SDK-Dateien können im gleichen Verzeichnis wie die Anwendung bereitgestellt werden. Auf diese Weise kann die Anwendung direkt auf die Bibliotheken zugreifen. Stellen Sie sicher, dass Sie die richtige, der Anwendung entsprechende Version (Win32/x64) auswählen.

| NAME | Funktion
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core SDK, erforderlich für die native und verwaltete Bereitstellung
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | Erforderlich für die verwaltete Bereitstellung
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Erforderlich für die verwaltete Bereitstellung

## <a name="linux"></a>Linux

Für eine native Anwendung müssen Sie die Speech SDK-Bibliothek `libMicrosoft.CognitiveServices.Speech.core.so` versenden.
Stellen Sie sicher, dass Sie die der Anwendung entsprechende Version (x86, x64) auswählen. Je nach verwendeter Linux-Version müssen Sie möglicherweise auch die folgenden Abhängigkeiten einfügen:

* Freigegebene Bibliotheken der GNU C-Bibliothek (einschließlich der POSIX Threads Programming-Bibliothek `libpthreads`)
* OpenSSL-Bibliothek (`libssl.so.1.0.0`)
* cURL-Bibliothek (`libcurl.so.4`)
* Freigegebene Bibliothek für ALSA-Anwendungen (`libasound.so.2`)

Unter Ubuntu 16.04 sollten die GNU C-Bibliotheken z.B. bereits standardmäßig installiert sein. Die letzten drei können mit folgenden Befehlen installiert werden:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
