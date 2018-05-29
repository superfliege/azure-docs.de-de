---
title: Erstellen einer Kubernetes-Entwicklungsumgebung in der Cloud | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: quickstart
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 279b7a8c20717668c0ff4be541e9168e2d8706fd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361578"
---
# <a name="quickstart-create-a-kubernetes-development-environment-with-azure-dev-spaces-net-core-and-vs-code"></a>Schnellstart: Erstellen einer Kubernetes-Entwicklungsumgebung mit Azure Dev Spaces (.NET Core und VS Code)


[!INCLUDE[](includes/learning-objectives.md)]

[!INCLUDE[](includes/see-troubleshooting.md)]

Sie können nun eine Kubernetes-basierte Entwicklungsumgebung in Azure erstellen.

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Installieren der Azure CLI
Bei Azure Dev Spaces ist der Einrichtungsaufwand für die lokalen Computer minimal. Der Großteil der Konfiguration Ihrer Entwicklungsumgebung wird in der Cloud gespeichert und kann für andere Benutzer freigegeben werden. Laden Sie zunächst die [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest) (Command-Line Interface, CLI) herunter, und führen Sie sie aus. 

> [!IMPORTANT]
> Falls die Azure-Befehlszeilenschnittstelle bereits installiert ist, vergewissern Sie sich, dass es sich dabei um mindestens Version 2.0.32 der CLI handelt.

[!INCLUDE[](includes/sign-into-azure.md)]

[!INCLUDE[](includes/use-dev-spaces.md)]

[!INCLUDE[](includes/install-vscode-extension.md)]

Während Sie auf die Erstellung des Clusters warten, können Sie mit der Codeentwicklung beginnen.

## <a name="create-an-aspnet-core-web-app"></a>Erstellen einer ASP.NET Core-Web-App
Wenn [.NET Core](https://www.microsoft.com/net) installiert ist, können Sie schnell eine ASP.NET Core-Web-App in einem Ordner namens `webfrontend` erstellen.

```cmd
   dotnet new mvc --name webfrontend
```

Oder **laden Sie Beispielcode von GitHub herunter**. Navigieren Sie dazu zu https://github.com/Azure/dev-spaces, und klicken Sie auf **Klonen oder herunterladen**, um das GitHub-Repository in Ihre lokale Umgebung herunterzuladen. Der Code für diese Anleitung befindet sich in `samples/dotnetcore/getting-started/webfrontend`.

[!INCLUDE[](includes/azds-prep.md)]

[!INCLUDE[](includes/build-run-k8s-cli.md)]

## <a name="update-a-content-file"></a>Aktualisieren einer Inhaltsdatei
Bei Azure Dev Spaces geht es nicht nur um die Ausführung von Code in Kubernetes: Mit diesem Dienst sollen Codeänderungen in einer Kubernetes-Umgebung in der Cloud schnell und iterativ sichtbar gemacht werden.

1. Navigieren Sie zur Datei `./Views/Home/Index.cshtml`, und ändern Sie die HTML. Beispiel: Ändern Sie Zeile 70 (`<h2>Application uses</h2>`) etwa ungefähr wie folgt: `<h2>Hello k8s in Azure!</h2>`
1. Speichern Sie die Datei . Kurz darauf wird im Terminalfenster eine Nachricht mit dem Hinweis angezeigt, dass eine Datei im ausgeführten Container aktualisiert wurde.
1. Aktualisieren Sie die Anzeige im Browser. Daraufhin sollte auf der Webseite die aktualisierte HTML angezeigt werden.

Was ist passiert? Für Änderungen an Inhaltsdateien (etwa HTML und CSS) ist keine erneute Kompilierung in einer .NET Core-Web-App erforderlich. Ein aktiver `azds up`-Befehl synchronisiert daher automatisch alle geänderten Inhaltsdateien im ausgeführten Container in Azure, sodass alle Inhaltsänderungen direkt angezeigt werden.

## <a name="update-a-code-file"></a>Aktualisieren einer Codedatei
Die Aktualisierung von Codedateien ist etwas aufwendiger, da eine .NET Core-App aktualisierte Anwendungsbinärdateien neu erstellen und generieren muss.

1. Drücken Sie im Terminalfenster `Ctrl+C` (zum Beenden von `azds up`).
1. Öffnen Sie die Codedatei mit dem Namen `Controllers/HomeController.cs`, und bearbeiten Sie die Nachricht, die auf der Seite „Info“ angezeigt wird: `ViewData["Message"] = "Your application description page.";`.
1. Speichern Sie die Datei .
1. Führen Sie `azds up` im Terminalfenster aus. 

Mit diesem Befehl wird das Containerimage neu erstellt und das Helm-Diagramm erneut bereitgestellt. Navigieren Sie in der Web-App zum Menü „Info“, um zu sehen, wie sich Ihre Codeänderungen in der ausgeführten Anwendung auswirken.


Es gibt jedoch eine noch *schnellere Methode* für die Codeentwicklung. Sie wird im nächsten Abschnitt erläutert. 

## <a name="debug-a-container-in-kubernetes"></a>Debuggen eines Containers in Kubernetes

[!INCLUDE[](includes/debug-intro.md)]

[!INCLUDE[](includes/init-debug-assets-vscode.md)]


### <a name="select-the-azds-debug-configuration"></a>Auswählen der AZDS-Debugkonfiguration
1. Klicken Sie zum Öffnen der Debugansicht auf der **Aktivitätsleiste** am Rand von VS Code auf das Symbol „Debuggen“.
1. Wählen Sie als aktive Debugkonfiguration **.NET Core Launch (AZDS)** (.NET Core-Start (AZDS)) aus.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Falls in der Befehlspalette keine Azure Dev Spaces-Befehle angezeigt werden, überprüfen Sie, ob die VS Code-Erweiterung für Azure Dev Spaces installiert wurde. Achten Sie darauf, dass es sich bei dem in VS Code geöffneten Arbeitsbereich um den Ordner handelt, der „azds.yaml“ enthält.


### <a name="debug-the-container-in-kubernetes"></a>Debuggen des Containers in Kubernetes
Drücken Sie **F5**, um Ihren Code in Kubernetes zu debuggen.

Wie beim Befehl `up` wird der Code mit der Entwicklungsumgebung synchronisiert, und ein Container wird erstellt und in Kubernetes bereitgestellt. Dieses Mal ist der Debugger natürlich an den Remotecontainer angefügt.

[!INCLUDE[](includes/tip-vscode-status-bar-url.md)]

Legen Sie einen Breakpoint in einer serverseitigen Codedatei fest, beispielsweise in der `Index()`-Funktion in der Quelldatei `Controllers/HomeController.cs`. Durch die Aktualisierung der Browserseite wird der Breakpoint erreicht.

Sie besitzen wie bei der lokalen Ausführung des Codes Vollzugriff auf Debuginformationen, etwa Aufrufliste, lokale Variablen, Ausnahmeinformationen usw.

### <a name="edit-code-and-refresh"></a>Bearbeiten des Codes und Aktualisieren
Nehmen Sie bei aktivem Debugger eine Codeänderung vor. Ändern Sie beispielsweise die Meldung der Seite „Info“ unter `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Speichern Sie die Datei, und klicken Sie im Bereich für **Debugaktionen** auf die Schaltfläche **Aktualisieren**. 

![](media/get-started-netcore/debug-action-refresh.png)

Das Neuerstellen und erneute Bereitstellen eines neuen Containerimages bei jeder vorgenommenen Codeänderung kann geraume Zeit in Anspruch nehmen. Daher kompiliert Azure Dev Spaces Code im vorhandenen Container inkrementell, um den Bearbeitungs-/Debugging-Kreislauf zu beschleunigen.

Aktualisieren Sie die Web-App im Browser, und wechseln Sie zur Seite „Info“. Daraufhin sollte eine benutzerdefinierte Meldung auf der Benutzeroberfläche angezeigt werden.

**Sie kennen jetzt eine Methode für die schnelle Codeiteration und das direkte Debugging in Kubernetes!**

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Arbeiten mit mehreren Containern und Teamentwicklung](get-started-netcore.md#call-a-service-running-in-a-separate-container)