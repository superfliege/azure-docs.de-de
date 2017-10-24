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
ms.openlocfilehash: debadde78d937bcd4ec1df665aacfd1887fbcd02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Installieren der Erweiterung Durable Functions und Beispiele (Azure Functions)

Die Erweiterung [Durable Functions ](durable-functions-overview.md) für Azure Functions finden Sie im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). In diesem Artikel werden das Installieren des Pakets und eine Reihe von Beispielen für die folgenden Entwicklungsumgebungen gezeigt:

* Visual Studio 2017 (empfohlen) 
* Azure-Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio bietet derzeit die beste Lösung für das Entwickeln von Apps, die Durable Functions verwenden.  Ihre Funktionen können sowohl lokal ausgeführt als auch in Azure veröffentlicht werden. Sie können mit einem leeren Projekt oder einer Gruppe von Beispielfunktionen starten.

### <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie die [neueste Version von Visual Studio](https://www.visualstudio.com/downloads/) (mindestens Version 15.3). Schließen Sie die Azure Tools in Ihre Setupoptionen ein.

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

## <a name="azure-portal"></a>Azure-Portal

Falls gewünscht, können Sie das Azure-Portal für die Entwicklung von Durable Functions verwenden.

### <a name="create-an-orchestrator-function"></a>Erstellen einer Orchestratorfunktion

1. Erstellen Sie eine neue Funktionen-App unter [functions.azure.com](https://functions.azure.com/signin).
2. Konfigurieren Sie die Funktionen-App für die [Verwendung der 2.0-Laufzeitversion ](functions-versions.md).
3. Erstellen Sie eine neue Funktion, und wählen Sie die Vorlage **Durable Functions Orchestrator – C#**.
4. Klicken Sie unter **Erweiterungen nicht installiert** auf **Installieren**, um die Erweiterung von NuGet.org herunterzuladen.

### <a name="copy-sample-code-to-the-function-app"></a>Kopieren des Beispielcodes in die Funktionen-App

1. Laden Sie die Datei [DFSampleApp.zip](https://github.com/Azure/azure-functions-durable-extension/raw/master/docfx/files/DFSampleApp.zip) herunter.
2. Entzippen Sie die Beispieldatei in `D:\home\site\wwwroot` mithilfe von Kudu oder FTP in die Funktionen-App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen des Funktionsverkettungsbeispiels](durable-functions-sequence.md)
