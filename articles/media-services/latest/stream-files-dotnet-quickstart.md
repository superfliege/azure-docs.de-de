---
title: 'Streamen von Videodateien mit Azure Media Services: .NET | Microsoft Docs'
description: Führen Sie die in diesem Schnellstart beschriebenen Schritte aus, um ein neues Azure Media Services-Konto zu erstellen, eine Datei zu codieren und in Azure Media Player zu streamen.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/08/2018
ms.author: juliako
ms.openlocfilehash: e17a800ff61822a5040737e479c3e1855eeb8893
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701037"
---
# <a name="quickstart-stream-video-files---net"></a>Schnellstart: Streamen von Videodateien: .NET

> [!NOTE]
> Die neueste Version von Azure Media Services liegt als Vorschau vor und kann als v3 bezeichnet werden. Um mit der Verwendung von v3-APIs zu starten, sollten Sie (wie in diesem Schnellstart beschrieben) ein neues Media Services-Konto erstellen. 

Dieser Schnellstart zeigt, wie einfach es ist, mit dem Streaming von Videos für eine Vielzahl von Browsern und Geräten mit Azure Media Services zu beginnen. Das Beispiel in diesem Thema codiert Inhalte, die Sie über eine HTTPS-URL zugänglich machen. 

Am Ende des Schnellstarts sind Sie in der Lage, ein Video zu streamen.  

![Video abspielen](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Visual Studio noch nicht installiert haben, können Sie [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) abrufen.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das .NET-Streamingbeispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
 ```

Das Beispiel befindet sich im Ordner [EncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/tree/master/AMSV3Quickstarts/EncodeAndStreamFiles).

Mit dem Beispiel werden die folgenden Aktionen durchgeführt:

1. Erstellen einer Transformation (nach vorheriger Überprüfung, ob die angegebene Transformation vorhanden ist) 
2. Erstellen eines Ausgabemedienobjekts, das als Ausgabe des Codierungsauftrags verwendet wird
3. Erstellen der auf einer HTTPS-URL basierenden Auftragseingabe
4. Übermitteln des Codierungsauftrags mit der zuvor erstellten Ein- und Ausgabe
5. Überprüfen des Auftragsstatus
6. Erstellen eines Streaminglocators
7. Erstellen von Streaming-URLs

Um eine Erläuterung dazu zu erhalten, was jede Funktion im Beispiel bewirkt, untersuchen Sie den Code, und sehen Sie sich die Kommentare in [dieser Quelldatei](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) an.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Wenn Sie die App ausführen, werden URLs angezeigt, die zur Wiedergabe des Videos mit verschiedenen Protokollen verwendet werden können. 

1. Drücken Sie STRG+F5 zum Ausführen der *EncodeAndStreamFiles*-Anwendung.
2. Wählen Sie das **HLS**-Protokoll von Apple (endet auf *manifest(format=m3u8-aapl)*) aus, und kopieren Sie die Streaming-URL aus der Konsole.

![Output](./media/stream-files-tutorial-with-api/output.png)

Im [Quellcode](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) des Beispiels können Sie sehen, wie die URL erstellt wird. Um sie zu erstellen, müssen Sie den Hostnamen des Streamingendpunkts und den Streaminglocatorpfad miteinander verketten.  

## <a name="test-with-azure-media-player"></a>Testen mit Azure Media Player

Um den Stream zu testen, wird in diesem Artikel Azure Media Player verwendet. 

> [!NOTE]
> Wenn ein Player auf einer HTTPS-Website gehostet wird, stellen Sie sicher, die URL mit „https“ zu aktualisieren.

1. Öffnen Sie einen Webbrowser, und navigieren Sie zu [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Fügen Sie im Feld **URL:** einen der URL-Streamingwerte ein, die Sie bei der Ausführung der Anwendung abgerufen haben. 
3. Drücken Sie **Player aktualisieren**.

Azure Media Player kann zum Testen verwendet werden, sollte jedoch nicht in einer Produktionsumgebung zum Einsatz kommen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen in der Ressourcengruppe einschließlich der in dieser Schnellstartanleitung erstellten Media Services- und Speicherkonten nicht mehr benötigen, löschen Sie die Ressourcengruppe. Sie können das **CloudShell**-Tool verwenden.

Führen Sie in **CloudShell** den folgenden Befehl aus:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="examine-the-code"></a>Untersuchen des Codes

Um eine Erläuterung dazu zu erhalten, was jede Funktion im Beispiel bewirkt, untersuchen Sie den Code, und sehen Sie sich die Kommentare in [dieser Quelldatei](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) an.

Das Tutorial zum [Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md) bietet ein erweitertes Streamingbeispiel mit detaillierten Erläuterungen. 

## <a name="multithreading"></a>Multithreading

Die Azure Media Services v3 SDKs sind nicht threadsicher. Beim Arbeiten mit einer Multithreadanwendung sollten Sie ein neues AzureMediaServicesClient-Objekt pro Thread generieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md)
