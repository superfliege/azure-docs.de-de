---
title: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie in Azure eine einfache per HTTP ausgelöste Funktion erstellen und veröffentlichen, indem Sie die Azure Functions-Tools für Visual Studio verwenden.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, Compute, serverlose Architektur
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/13/2018
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 6fa56a9978950d28f6e25192f2044b823050f718
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="create-your-first-function-using-visual-studio"></a>Erstellen Ihrer ersten Funktion mit Visual Studio

Mit Azure Functions können Sie Code in einer [serverlosen](https://azure.microsoft.com/overview/serverless-computing/) Umgebung ausführen, ohne vorher eine VM erstellen oder eine Webanwendung veröffentlichen zu müssen.

In diesem Artikel erfahren Sie, wie Sie die Visual Studio 2017-Tools für Azure Functions verwenden, um eine „hello world“-Funktion lokal zu erstellen und zu testen. Anschließend veröffentlichen Sie den Funktionscode in Azure. Diese Tools sind als Teil der Azure-Entwicklungsworkload in Visual Studio 2017 verfügbar.

![Azure Functions-Code in einem Visual Studio-Projekt](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

Dieses Thema enthält ein [Video](#watch-the-video) zu einigen grundlegenden Schritten.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Installieren Sie mindestens die [Version 15.5 von Visual Studio 2017](https://www.visualstudio.com/vs/) – einschließlich der Workload **Azure-Entwicklung**.

    ![Installation von Visual Studio 2017 mit der Workload „Azure-Entwicklung“](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

    Wenn Sie Visual Studio bereits installiert haben, vergewissern Sie sich, dass Sie alle ausstehenden Updates installiert haben. 

* Falls Sie die Workload für die Azure-Entwicklung mit der Version 15.4 von Visual Studio 2017 (oder mit einer älteren Version) installiert haben, müssen Sie gegebenenfalls auch [Ihre Azure Functions-Tools aktualisieren](functions-develop-vs.md#check-your-tools-version). 

## <a name="create-a-function-app-project"></a>Erstellen eines Funktionen-App-Projekts

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio erstellt ein Projekt, das eine Klasse mit den Codebausteinen für den gewählten Funktionstyp enthält. Mit dem **FunctionName**-Attribut der Methode wird der Name der Funktion festgelegt. Mit dem **HttpTrigger**-Attribut wird angegeben, dass die Funktion mit einer HTTP-Anforderung ausgelöst wird. Mit Code in den Codebausteinen wird eine HTTP-Antwort gesendet, die einen Wert aus dem Anforderungstext oder der Abfragezeichenfolge enthält. Sie können Eingabe- und Ausgabebindungen einer Funktion hinzufügen, indem Sie die geeigneten Attribute auf die Methode anwenden. Weitere Informationen finden Sie im Abschnitt [Trigger und Bindungen](functions-dotnet-class-library.md#triggers-and-bindings) in der [C#-Entwicklerreferenz zu Azure Functions](functions-dotnet-class-library.md).

![Funktionscodedatei](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

Nachdem Sie nun Ihr Funktionsprojekt und eine Funktion mit HTTP-Trigger erstellt haben, können Sie sie auf Ihrem lokalen Computer testen.

## <a name="test-the-function-locally"></a>Lokales Testen der Funktion

Mit Azure Functions Core-Tools können Sie ein Azure Functions-Projekt auf dem lokalen Entwicklungscomputer ausführen. Sie werden beim ersten Starten einer Funktion in Visual Studio zum Installieren dieser Tools aufgefordert.  

1. Drücken Sie F5, um Ihre Funktion zu testen. Akzeptieren Sie die entsprechende Aufforderung von Visual Studio zum Herunterladen und Installieren der Azure Functions Core (CLI)-Tools. Sie müssen möglicherweise auch eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können.

2. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.  

    ![Lokale Azure-Laufzeit](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. Fügen Sie die URL der HTTP-Anforderung in die Adresszeile des Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Hier ist die Antwort des Browsers auf die von der Funktion zurückgegebene lokale GET-Anforderung abgebildet: 

    ![localhost-Antwort der Funktion im Browser](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Drücken Sie UMSCHALT+F5, um das Debuggen zu beenden.

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

Falls Sie über kein Azure-Abonnement verfügen, erstellen Sie zunächst ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

Sie müssen in Ihrem Azure-Abonnement über eine Funktions-App verfügen, bevor Sie Ihr Projekt veröffentlichen können. Sie können eine Funktions-App in Ihrem Abonnement direkt in Visual Studio erstellen.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testen der Funktion in Azure

1. Kopieren Sie die Basis-URL der Funktions-App von der Seite „Veröffentlichungsprofil“. Ersetzen Sie den Teil `localhost:port` der URL, die Sie beim lokalen Testen der Funktion verwendet haben, durch die neue Basis-URL. Stellen Sie wie zuvor sicher, dass Sie die Abfragezeichenfolge `?name=<yourname>` an diese URL anhängen und die Anforderung ausführen.

    Die URL, über die Ihre per HTTP ausgelöste Funktion aufgerufen wird, sollte das folgende Format haben:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. Fügen Sie diese neue URL für die HTTP-Anforderung in die Adresszeile des Browsers ein. Hier ist die Antwort des Browsers auf die von der Funktion zurückgegebene GET-Remoteanforderung abgebildet: 

    ![Funktionsantwort im Browser](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)

## <a name="watch-the-video"></a>Video ansehen

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

## <a name="next-steps"></a>Nächste Schritte

Sie haben Visual Studio genutzt, um eine C#-Funktions-App mit einer einfachen Funktion zu erstellen und zu veröffentlichen, die per HTTP ausgelöst wird. 

+ Weitere Informationen zum Entwickeln von Funktionen wie .NET-Klassenbibliotheken finden Sie unter [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (Verwenden von .NET-Klassenbibliotheken mit Azure Functions). 
+ Informationen zum Konfigurieren Ihres Projekts für die Unterstützung anderer Arten von Triggern und Bindungen finden Sie unter [Azure Functions-Tools für Visual Studio](functions-develop-vs.md) im Abschnitt [Konfigurieren des Projekts für die lokale Entwicklung](functions-develop-vs.md#configure-the-project-for-local-development).
+ Weitere Informationen zum lokalen Testen und Debuggen mit den Azure Functions Core-Tools finden Sie unter [Lokales Codieren und Testen von Azure-Funktionen](functions-run-local.md). 
