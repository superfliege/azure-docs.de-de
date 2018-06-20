---
title: Erstellen eines Kubernetes-Entwicklungsbereichs in der Cloud | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 06/06/2018
ms.topic: quickstart
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 16ec493708f85e9b3819943e131b9f9c3649f27e
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824637"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-net-core-and-visual-studio"></a>Schnellstart: Erstellen eines Kubernetes-Entwicklungsbereichs mit Azure Dev Spaces (.NET Core und Visual Studio)

In diesem Leitfaden lernen Sie Folgendes:

- Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
- Iteratives Entwickeln von Code in Containern mit Visual Studio
- Debuggen von Code, der in Ihrem Cluster ausgeführt wird

> [!Note]
> **Sollten Sie einmal nicht weiterkommen**, lesen Sie den Abschnitt [Problembehandlung](troubleshooting.md), oder hinterlassen Sie einen Kommentar auf dieser Seite. Ein ausführlicheres Tutorial finden Sie [hier](get-started-netcore-visualstudio.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Kubernetes-Cluster mit Kubernetes 1.9.6 in der Region „EastUS“, „WestEurope“ oder „CanadaEast“ und mit aktiviertem HTTP-Anwendungsrouting

  ![Achten Sie darauf, dass HTTP-Anwendungsrouting aktiviert ist.](media/common/Kubernetes-Create-Cluster-3.PNG)

- Visual Studio 2017 mit installierter Workload für Webentwicklung. Ist diese Version nicht installiert, können Sie sie [hier](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) herunterladen.

## <a name="set-up-azure-dev-spaces"></a>Einrichten von Azure Dev Spaces

Installieren Sie die [Visual Studio-Erweiterung für Azure Dev Spaces](https://aka.ms/get-azds-visualstudio).

## <a name="connect-to-a-cluster"></a>Herstellen der Verbindung zu einem Cluster

Als Nächstes erstellen und konfigurieren Sie ein Projekt für Azure Dev Spaces.

### <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

Erstellen Sie in Visual Studio 2017 ein neues Projekt. Derzeit muss das Projekt eine **ASP.NET Core-Webanwendung** sein. Nennen Sie das Projekt **webfrontend**.

Wählen Sie die Vorlage **Webanwendung (Model-View-Controller)** aus, und legen Sie **.NET Core** und **ASP.NET Core 2.0** als Ziel fest.

### <a name="create-a-dev-space-in-azure"></a>Erstellen eines Entwicklungsbereichs in Azure

Während das soeben erstellte Projekt geöffnet ist, wählen Sie wie nachfolgend gezeigt im Dropdownmenü mit den Starteinstellungen **Azure Dev Spaces** aus.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Vergewissern Sie sich im nächsten angezeigten Dialogfeld, dass Sie mit dem entsprechenden Konto angemeldet sind, und wählen Sie dann einen vorhandenen Cluster aus.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Übernehmen Sie für die Dropdownliste **Space** (Bereich) vorerst `default`. Aktivieren Sie das Kontrollkästchen **Publicly Accessible** (Öffentlich zugänglich), damit auf die Web-App über einen öffentlichen Endpunkt zugegriffen werden kann.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Klicken Sie auf **OK**, um den Cluster auszuwählen oder zu erstellen.

Wenn Sie einen Cluster auswählen, der nicht für die Verwendung mit Azure Dev Spaces konfiguriert wurde, wird eine Meldung mit der Frage angezeigt, ob Sie ihn konfigurieren möchten.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Wählen Sie **OK**. 

### <a name="look-at-the-files-added-to-project"></a>Anzeigen der zum Projekt hinzugefügten Dateien
Während Sie auf die Erstellung des Entwicklungsbereichs warten, sehen Sie sich die Dateien an, die Ihrem Projekt hinzugefügt wurden, als Sie sich für die Verwendung von Azure Dev Spaces entschieden haben.

- Ein Ordner namens `charts` wurde hinzugefügt. In diesem Ordner wurde ein [Helm-Diagramm](https://docs.helm.sh) für Ihre Anwendung erstellt. Diese Dateien werden zum Bereitstellen der Anwendung im Entwicklungsbereich verwendet.
- `Dockerfile` enthält Informationen, die zum Packen Ihrer Anwendung im Docker-Standardformat erforderlich sind.
- `azds.yaml` enthält für den Entwicklungsbereich erforderliche Konfigurationsinformationen, etwa Angaben dazu, ob die Anwendung über einen öffentlichen Endpunkt zugänglich sein soll.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Debuggen eines Containers in Kubernetes
Wenn der Entwicklungsbereich erstellt wurde, können Sie die Anwendung debuggen. Legen Sie einen Breakpoint im Code fest, etwa in Zeile 20 in der Datei `HomeController.cs`, in der die Variable `Message` festgelegt wird. Drücken Sie **F5**, um das Debuggen zu starten. 

Visual Studio kommuniziert zum Erstellen und Bereitstellen der Anwendung mit dem Entwicklungsbereich und öffnet dann einen Browser mit der ausgeführten Web-App. Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt, tatsächlich wird er jedoch im Entwicklungsbereich in Azure ausgeführt. Die localhost-Adresse wird verwendet, da Azure Dev Spaces einen temporären SSH-Tunnel zu dem in Azure ausgeführten Container erstellt.

Klicken Sie oben auf der Seite auf den Link **Info**, um den Breakpoint auszulösen. Sie besitzen wie bei der lokalen Ausführung des Codes Vollzugriff auf Debuginformationen, etwa Aufrufliste, lokale Variablen, Ausnahmeinformationen usw.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Arbeiten mit mehreren Containern und Teamentwicklung](get-started-netcore-visualstudio.md#call-another-container)