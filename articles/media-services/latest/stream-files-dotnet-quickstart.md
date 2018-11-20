---
title: 'Streamen von Videodateien mit Azure Media Services: .NET | Microsoft Docs'
description: Führen Sie die in diesem Schnellstart beschriebenen Schritte aus, um ein neues Azure Media Services-Konto zu erstellen, eine Datei zu codieren und in Azure Media Player zu streamen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: fc8fc1af51332df032e864c84791791a38bc8601
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612219"
---
# <a name="quickstart-stream-video-files---net"></a>Schnellstart: Streamen von Videodateien: .NET

Dieser Schnellstart zeigt, wie einfach es ist, zu Codieren und mit dem Streaming von Videos für eine Vielzahl von Browsern und Geräten mit Azure Media Services zu beginnen. Ein Eingabeinhalt kann über HTTPS-URLs, SAS-URLs oder Pfade zu Dateien in Azure Blob Storage angegeben werden.
Das Beispiel in diesem Thema codiert Inhalte, die Sie über eine HTTPS-URL zugänglich machen. Beachten Sie, dass AMS v3 derzeit keine segmentierte Transfercodierung über HTTPS-URLs unterstützt.

Am Ende des Schnellstarts sind Sie in der Lage, ein Video zu streamen.  

![Video abspielen](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie Visual Studio noch nicht installiert haben, können Sie [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) abrufen.
- Installieren und verwenden Sie die Befehlszeilenschnittelle lokal. Dieser Artikel erfordert mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

    Derzeit funktionieren nicht alle Befehle der [Befehlszeilenschnittstelle von Media Services v3](https://aka.ms/ams-v3-cli-ref) in Azure Cloud Shell. Es wird empfohlen, die Befehlszeilenschnittstelle lokal zu verwenden.

- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das .NET-Streamingbeispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone http://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
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

Wenn Sie die Ressourcen in der Ressourcengruppe einschließlich der in dieser Schnellstartanleitung erstellten Media Services- und Speicherkonten nicht mehr benötigen, löschen Sie die Ressourcengruppe.

Führen Sie den folgenden CLI-Befehl aus:

```azurecli
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
