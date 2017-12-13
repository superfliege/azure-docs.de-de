---
title: "Installieren der Erweiterung Durable Functions und Beispiele – Azure"
description: "Erfahren Sie, wie Sie die Erweiterung Durable Functions für Azure Functions, für die Portalentwicklung oder Visual Studio-Entwicklung installieren."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 91b632c0c4bab2f0ac71b662cf1b73f5d37881ff
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Installieren der Erweiterung Durable Functions und Beispiele (Azure Functions)

Die Erweiterung [Durable Functions ](durable-functions-overview.md) für Azure Functions finden Sie im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). In diesem Artikel werden das Installieren des Pakets und eine Reihe von Beispielen für die folgenden Entwicklungsumgebungen gezeigt:

* Visual Studio 2017 (empfohlen) 

* Azure-Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio bietet derzeit die beste Lösung für das Entwickeln von Apps, die Durable Functions verwenden.  Ihre Funktionen können sowohl lokal ausgeführt als auch in Azure veröffentlicht werden. Sie können mit einem leeren Projekt oder einer Gruppe von Beispielfunktionen starten.

### <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie die [neueste Version von Visual Studio](https://www.visualstudio.com/downloads/) (mindestens Version 15.3). Fügen Sie die Workload **Azure-Entwicklung** in Ihre Setupoptionen ein.

### <a name="start-with-sample-functions"></a>Starten mit Beispielfunktionen

1. Laden Sie die [ZIP-Datei der Beispiel-App für Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip) herunter. Sie müssen den NuGet-Verweis nicht hinzufügen, da das Beispielprojekt ihn bereits besitzt.
2. Installieren Sie den [Azure-Speicheremulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) Version 5.2 oder höher, und führen Sie ihn aus. Alternativ können Sie die Datei *local.appsettings.json* mit echten Azure Storage-Verbindungszeichenfolgen aktualisieren.
3. Öffnen Sie das Projekt in Visual Studio 2017. 
4. Um Anweisungen zum Ausführen des Beispiels zu erhalten, beginnen Sie mit [Function chaining in Durable Functions – Hello sequence sample](durable-functions-sequence.md) (Funktionsverkettung in Durable Functions – „Hello“-Sequenzbeispiel). Das Beispiel kann lokal ausgeführt oder in Azure veröffentlicht werden.

### <a name="start-with-an-empty-project"></a>Starten mit einem leeren Projekt
 
Befolgen Sie die gleichen Anweisungen wie für den Start mit dem Beispiel, aber führen Sie die folgenden Schritte aus, statt die *ZIP*-Datei herunterzuladen:

1. Erstellen Sie ein Funktionen-App-Projekt.
2. Fügen Sie den folgenden NuGet-Paketverweis Ihrer *CSPROJ*-Datei hinzu:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code bietet eine lokale Entwicklungsumgebung, die alle wichtigen Plattformen abdeckt – Windows, macOS und Linux.  Ihre Funktionen können sowohl lokal ausgeführt als auch in Azure veröffentlicht werden. Sie können mit einem leeren Projekt oder einer Gruppe von Beispielfunktionen starten.

### <a name="prerequisites"></a>Voraussetzungen

* Installieren der [neuesten Version von Visual Studio Code](https://code.visualstudio.com/Download) 

* Befolgen Sie die Anweisungen unter „Installieren von Azure Functions Core Tools“ unter [Lokales Codieren und Testen von Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local).

    >[!IMPORTANT]
    > Wenn Sie bereits über die plattformübergreifenden Tools von Azure Functions verfügen, aktualisieren Sie diese auf die neueste verfügbare Version.

*  Installieren Sie den [Azure-Speicheremulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) Version 5.2 oder höher, und führen Sie ihn aus. Alternativ können Sie die Datei *local.appsettings.json* mit einer echten Azure Storage-Verbindung aktualisieren. 


### <a name="start-with-sample-functions"></a>Starten mit Beispielfunktionen

1. Klonen Sie das [Durable Functions-Repository](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navigieren Sie auf dem Computer zum [Ordner mit den C#-Skriptbeispielen](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Installieren Sie Azure Functions Durable Extension, indem Sie die folgenden Schritte über eine Eingabeaufforderung bzw. in einem Terminalfenster ausführen:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.1.0-beta2
    ```
4. Führen Sie Azure Storage Emulator aus, oder aktualisieren Sie die Datei *local.appsettings.json* mit echten Azure Storage-Verbindungszeichenfolgen.
3. Öffnen Sie das Projekt in Visual Studio Code. 
5. Um Anweisungen zum Ausführen des Beispiels zu erhalten, beginnen Sie mit [Function chaining in Durable Functions – Hello sequence sample](durable-functions-sequence.md) (Funktionsverkettung in Durable Functions – „Hello“-Sequenzbeispiel). Das Beispiel kann lokal ausgeführt oder in Azure veröffentlicht werden.
6. Starten Sie das Projekt, indem Sie über die Eingabeaufforderung bzw. das Terminalfenster den folgenden Befehl ausführen:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Starten mit einem leeren Projekt
 
1. Navigieren Sie über die Eingabeaufforderung bzw. das Terminalfenster zu dem Ordner, in dem sich Ihre Funktions-App befindet.
2. Installieren Sie Azure Functions Durable Extension, indem Sie die folgenden Schritte über eine Eingabeaufforderung bzw. in einem Terminalfenster ausführen:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.1.0-beta2
    ```
3. Erstellen Sie ein Funktions-App-Projekt, indem Sie den folgenden Befehl ausführen:

    ```bash
    func init
    ``` 
4. Führen Sie Azure Storage Emulator aus, oder aktualisieren Sie die Datei *local.appsettings.json* mit echten Azure Storage-Verbindungszeichenfolgen.
5. Als nächstes erstellen Sie eine neue Funktion, indem Sie den folgenden Befehl ausführen und den Schritten des Assistenten folgen:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Derzeit ist die Durable Function-Vorlage nicht verfügbar, aber Sie können mit einer der unterstützten Optionen beginnen und dann den Code ändern. Verwenden Sie als Referenz die Beispiele für [Orchestrierungsclient](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [Orchestrierungstrigger](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence) und [Aktivitätstrigger](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Öffnen Sie den Projektordner in Visual Studio Code und setzen Sie den Vorgang fort, indem Sie den Code der Vorlage ändern. 
7. Starten Sie das Projekt, indem Sie über die Eingabeaufforderung bzw. das Terminalfenster den folgenden Befehl ausführen:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Azure-Portal

Falls gewünscht, können Sie das Azure-Portal für die Entwicklung von Durable Functions verwenden.

### <a name="create-an-orchestrator-function"></a>Erstellen einer Orchestratorfunktion

1. Erstellen Sie eine neue Funktionen-App unter [functions.azure.com](https://functions.azure.com/signin).

2. Konfigurieren Sie die Funktionen-App für die [Verwendung der 2.0-Laufzeitversion ](functions-versions.md).

3. Erstellen Sie eine neue Funktion, indem Sie **Eigene benutzerdefinierte Funktion erstellen** auswählen.

4. Ändern Sie die **Sprache** in **C#**, **Szenario** in **Durable Functions**, und wählen Sie die Vorlage **Durable Functions Http Starter – C#** aus.

5. Klicken Sie unter **Erweiterungen nicht installiert** auf **Installieren**, um die Erweiterung von NuGet.org herunterzuladen. 

6. Nachdem die Installation abgeschlossen ist, fahren Sie mit der Erstellung einer Funktion für den Orchestrierungsclient fort, **„HttpStart“**, die durch Auswahl der Vorlage **Durable Functions Http Starter – C#** erstellt wird.

7. Erstellen Sie nun die Orchestrierungsfunktion **„HelloSequence“** über die Vorlage **Durable Functions Orchestrator – C#**.

8. Die letzte Funktion der Vorlage **Durable Functions Activity – C#** wird **„Hello“** genannt.

9. Wechseln Sie zur Funktion **„HttpStart“**, und kopieren Sie ihre URL.

10. Verwenden Sie Postman oder cURL, um die langlebige Funktion aufzurufen. Ersetzen Sie vor dem Testen in der URL **{functionName}** durch den Namen der Orchestratorfunktion – **HelloSequence**.  Es sind keine Daten erforderlich, verwenden Sie einfach das Verb POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Rufen Sie dann den Endpunkt **„statusQueryGetUri“** auf, um den aktuellen Status der Durable Function anzuzeigen.

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Fahren Sie mit dem Aufruf des Endpunkts **„statusQueryGetUri“** fort, bis der Status zu **„Completed“** wechselt. 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Glückwunsch! Ihre erste langlebige Funktion steht im Azure-Portal zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen des Funktionsverkettungsbeispiels](durable-functions-sequence.md)
