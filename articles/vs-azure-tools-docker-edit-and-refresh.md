---
title: Debuggen von Apps in einem lokalen Docker-Container | Microsoft Docs
description: Erfahren Sie, wie Sie eine in einem lokalen Docker-Container ausgeführte App ändern, den Container über das Feature zum Bearbeiten und Aktualisieren aktualisieren und Haltepunkte für das Debuggen setzen.
services: container-service
author: ghogen
manager: douge
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.topic: article
ms.workload: multiple
ms.date: 09/11/2018
ms.author: ghogen
ms.openlocfilehash: 8844992ffbed2d57ad54a5f0e4a2d0b05c1d277a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968728"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Debuggen von Apps in einem lokalen Docker-Container
## <a name="overview"></a>Übersicht
Visual Studio 2017 bietet eine zuverlässige Möglichkeit der Entwicklung in einem Docker-Container und der lokalen Überprüfung Ihrer Anwendung.
Sie müssen bei jeder Codeänderung den Container nicht neu starten.
In diesem Artikel erfahren Sie, wie Sie das Feature zum Bearbeiten und Aktualisieren nutzen, um eine ASP.NET Core-Web-App in einem lokalen Docker-Container zu starten, die erforderlichen Änderungen vorzunehmen und den Browser dann zu aktualisieren, um die Änderungen anzuzeigen.
Ferner wird gezeigt, wie Sie zum Debuggen Haltepunkte festlegen.

## <a name="prerequisites"></a>Voraussetzungen
Die folgenden Tools müssen installiert sein.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit installierter Workload für Webentwicklung.

Um Docker-Container lokal ausführen zu können, benötigen Sie einen lokalen Docker-Client.
Sie können die [Docker Toolbox](https://www.docker.com/products/docker-toolbox) verwenden, wofür Hyper-V deaktiviert werden muss. Sie können auch [Docker für Windows](https://www.docker.com/get-docker) nutzen, das Hyper-V verwendet und Windows 10 erfordert.

Wenn Sie Docker Toolbox verwenden, müssen Sie [den Docker-Client konfigurieren](vs-azure-tools-docker-setup.md).

## <a name="1-create-a-web-app"></a>1. Erstellen einer Web-App
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-edit-your-code-and-refresh"></a>2. Bearbeiten des Codes und Aktualisieren
Um Änderungen schnell zu durchlaufen, können Sie Ihre Anwendung in einem Container starten, Änderungen weiter vornehmen und wie mithilfe von IIS Express anzeigen.

1. Legen Sie die Projektmappenkonfiguration auf `Debug` fest, und drücken Sie **&lt;STRG+F5>**, um Ihr Docker-Image zu erstellen und lokal auszuführen.

    Sobald das Containerimage erstellt wurde und in einem Docker-Container ausgeführt wird, startet Visual Studio die Web-App in Ihrem Standardbrowser.
    Lesen Sie den Abschnitt [Problembehandlung](vs-azure-tools-docker-troubleshooting-docker-errors.md) , wenn Sie den Browser Microsoft Edge verwenden oder sonstige Fehlermeldungen erhalten.
2. Wechseln Sie zur Seite „Info“, auf der wir unsere Änderungen vornehmen.
3. Kehren Sie zu Visual Studio zurück, und öffnen Sie `Views\Home\About.cshtml`.
4. Fügen Sie den folgenden HTML-Inhalt am Ende der Datei hinzu, und speichern Sie die Änderungen.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Sobald der .NET-Build fertig gestellt ist, sehen Sie im Ausgabefenster dieses Zeilen. Kehren Sie zu Ihrem Browser zurück, und aktualisieren Sie die Seite „Info“.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```

6. Ihre Änderungen wurden übernommen!

## <a name="3-debug-with-breakpoints"></a>3. Debuggen mit Haltepunkten
Änderungen müssen öfters überprüft werden, wozu die Debugfunktionen von Visual Studio genutzt werden.

1. Kehren Sie zu Visual Studio zurück, und öffnen Sie `About.cshtml.cs`
2. Ersetzen Sie den Inhalt der OnGet()-Methode durch Folgendes:

   ```cs
       Message = "Your application description page from within a Container";
   ```

3. Setzen Sie links neben die Codezeile einen Haltepunkt.
4. Drücken Sie **&lt;F5>**, um mit dem Debuggen zu beginnen.
5. Navigieren Sie zur Seite „Info“, und den Haltepunkt zu erreichen.
6. Kehren Sie zu Visual Studio zurück, um den Haltepunkt anzuzeigen, und untersuchen Sie den Wert der Nachricht.

   ![][2]

## <a name="summary"></a>Zusammenfassung
Die Docker-Unterstützung in Visual Studio 2017 bietet die Produktivität einer lokalen Umgebung und realitätsnahe Entwicklung innerhalb eines Docker-Containers.

## <a name="troubleshooting"></a>Problembehandlung
[Problembehandlung bei der Visual Studio Docker-Entwicklung](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Weitere Informationen zu Docker mit Visual Studio und Azure
* [Containerentwicklung mit Visual Studio](/visualstudio/containers) – eine Containerentwicklungs-Hubseite
* [Docker-Integration für Azure Pipelines](http://aka.ms/dockertoolsforvsts) – Erstellen und Bereitstellen von Docker-Containern
* [Docker-Tools für Visual Studio Code](http://aka.ms/dockertoolsforvscode) – Sprachdienste für die Bearbeitung von Docker-Dateien, mit weiteren e2e-Szenarien in Planung
* [Informationen zu Windows-Containern](http://aka.ms/containers)– Informationen zu Windows Server und Nano Server
* [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) - [Dokumentation zum Azure Kubernetes Service](/azure/aks)

## <a name="various-docker-tools"></a>Verschiedene Docker-Tools
[Some great docker tools (Blog von Steve Lasker) (Einige großartige Docker-Tools)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Nützliche Artikel
[Introduction to Microservices from NGINX (Einführung in Microservices von NGINX)](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Präsentationen
* [Steve Lasker: VS Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Introduction to ASP.NET Core @ build 2016 - Where You At Demo](https://channel9.msdn.com/Events/Build/2016/B810) (Einführung in ASP.NET Core auf der Build 2016)
* [Developing .NET apps in containers, Channel 9 (Entwickeln von .NET-Apps in Containern)](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
