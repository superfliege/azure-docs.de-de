---
title: Erstellen einer Kubernetes-Entwicklungsumgebung in der Cloud mit .NET Core und Visual Studio | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 012efcbd3fa87268f3a68fdac524ce8310d10120
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34362055"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core-and-visual-studio"></a>Erste Schritte in Azure Dev Spaces mit .NET Core und Visual Studio

In diesem Leitfaden lernen Sie Folgendes:

- Erstellen einer für die Entwicklung optimierten, Kubernetes-basierten Umgebung in Azure
- Iteratives Entwickeln von Code in Containern mit Visual Studio
- Unabhängiges Entwickeln von zwei separaten Diensten und Verwenden der DNS-Dienstermittlung von Kubernetes, um einen anderen Dienst aufzurufen
- Produktives Entwickeln und Testen Ihres Codes in einer Teamumgebung

[!INCLUDE[](includes/see-troubleshooting.md)]

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="get-the-visual-studio-tools"></a>Abrufen der Visual Studio-Tools
1. Installieren Sie die aktuelle Version von [Visual Studio 2017](https://www.visualstudio.com/vs/).
1. Wählen Sie im Visual Studio-Installer unbedingt die folgende Workload aus:
    * ASP.NET und Webentwicklung
1. Installieren Sie die [Visual Studio-Erweiterung für Azure Dev Spaces](https://aka.ms/get-azds-visualstudio).

Jetzt können Sie eine ASP.NET-Web-App mit Visual Studio erstellen.

## <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

Erstellen Sie in Visual Studio 2017 ein neues Projekt. Derzeit muss das Projekt eine **ASP.NET Core-Webanwendung** sein. Nennen Sie das Projekt **webfrontend**.

![](media/get-started-netcore-visualstudio/NewProjectDialog1.png)

Wählen Sie die Vorlage **Webanwendung (Model-View-Controller)** aus, und legen Sie oben im Dialogfeld in den beiden Dropdownmenüs **.NET Core** und **ASP.NET Core 2.0** fest. Klicken Sie auf **OK**, um das Projekt zu erstellen.

![](media/get-started-netcore-visualstudio/NewProjectDialog2.png)


## <a name="create-a-dev-environment-in-azure"></a>Erstellen einer Entwicklungsumgebung in Azure

Mit Azure Dev Spaces können Sie Kubernetes-basierte Entwicklungsumgebungen erstellen, die vollständig von Azure verwaltet werden und für die Entwicklung optimiert sind. Während das soeben erstellte Projekt geöffnet ist, wählen Sie wie nachfolgend gezeigt im Dropdownmenü mit den Starteinstellungen **Azure Dev Spaces** aus.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Vergewissern Sie sich im nächsten angezeigten Dialogfeld, dass Sie mit dem entsprechenden Konto angemeldet sind, und wählen Sie dann einen vorhandenen Kubernetes-Cluster aus.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.PNG)

Übernehmen Sie für die Dropdownliste **Space** (Bereich) vorerst den Standardwert `default`. Später erfahren Sie mehr über diese Option. Aktivieren Sie das Kontrollkästchen **Publicly Accessible** (Öffentlich zugänglich), damit auf die Web-App über einen öffentlichen Endpunkt zugegriffen werden kann. Diese Einstellung ist nicht erforderlich, ist aber später in dieser exemplarischen Vorgehensweise hilfreich für die Erläuterung einiger Konzepte. Aber keine Sorge: Sie können Ihre Website in beiden Fällen mit Visual Studio debuggen.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Klicken Sie auf **OK**, um den Cluster auszuwählen oder zu erstellen.

Wenn Sie einen Cluster auswählen, der nicht für die Verwendung mit Azure Dev Spaces vorbereitet wurde, wird eine Meldung mit der Frage angezeigt, ob Sie ihn konfigurieren möchten.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Klicken Sie auf **OK**.

 Dazu wird eine Hintergrundaufgabe gestartet. Diese nimmt einige Minuten in Anspruch. Um zu überprüfen, ob der Cluster noch erstellt wird, zeigen Sie mit der Maus unten links auf der Statusleiste auf das Symbol **Hintergrundaufgaben**, wie in der folgenden Abbildung gezeigt:

![](media/get-started-netcore-visualstudio/BackgroundTasks.PNG)

> [!Note]
> Sie können die Anwendung erst debuggen, wenn die Entwicklungsumgebung erstellt wurde.

## <a name="look-at-the-files-added-to-project"></a>Anzeigen der zum Projekt hinzugefügten Dateien
Während Sie auf die Erstellung der Entwicklungsumgebung warten, sehen Sie sich die Dateien an, die Ihrem Projekt hinzugefügt wurden, als Sie sich für die Verwendung einer Entwicklungsumgebung entschieden haben.

Sie sehen zuerst einen Ordner namens `charts`, der hinzugefügt wurde. In diesem Ordner wurde ein [Helm-Diagramm](https://docs.helm.sh) für Ihre Anwendung erstellt. Diese Dateien werden zum Bereitstellen der Anwendung in der Entwicklungsumgebung verwendet.

Sie sehen, dass die Datei `Dockerfile` hinzugefügt wurde. Diese Datei enthält Informationen, die zum Packen Ihrer Anwendung im Docker-Standardformat erforderlich sind. Darüber hinaus wird die Datei `HeaderPropagation.cs` erstellt. Diese Datei wird später in dieser exemplarischen Vorgehensweise genauer erläutert. 

Außerdem wurde die Datei `azds.yaml` hinzugefügt. Sie enthält für die Entwicklungsumgebung erforderliche Konfigurationsinformationen, etwa Angaben dazu, ob die Anwendung über einen öffentlichen Endpunkt zugänglich sein soll.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Debuggen eines Containers in Kubernetes
Wenn die Entwicklungsumgebung erstellt wurde, können Sie die Anwendung debuggen. Legen Sie einen Breakpoint im Code fest, etwa in Zeile 20 in der Datei `HomeController.cs`, in der die Variable `Message` festgelegt wird. Drücken Sie **F5**, um das Debuggen zu starten. 

Visual Studio kommuniziert zum Erstellen und Bereitstellen der Anwendung mit der Entwicklungsumgebung und öffnet dann einen Browser mit der ausgeführten Web-App. Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt, tatsächlich wird er jedoch in der Entwicklungsumgebung in Azure ausgeführt. Die localhost-Adresse wird verwendet, da Azure Dev Spaces einen temporären SSH-Tunnel zu dem in Azure ausgeführten Container erstellt.

Klicken Sie oben auf der Seite auf den Link **Info**, um den Breakpoint auszulösen. Sie besitzen wie bei der lokalen Ausführung des Codes Vollzugriff auf Debuginformationen, etwa Aufrufliste, lokale Variablen, Ausnahmeinformationen usw.

## <a name="call-another-container"></a>Aufrufen eines anderen Containers
In diesem Abschnitt erstellen Sie einen zweiten Dienst (`mywebapi`) und lassen ihn von `webfrontend` aufrufen. Jeder Dienst wird in einem separaten Container ausgeführt. Anschließend debuggen Sie beide Container.

![](media/common/multi-container.png)

## <a name="download-sample-code-for-mywebapi"></a>Herunterladen von Beispielcode für *mywebapi*
Laden Sie Beispielcode aus einem GitHub-Repository herunter, um Zeit zu sparen. Navigieren Sie zu https://github.com/Azure/dev-spaces, und klicken Sie auf **Klonen oder herunterladen**, um das GitHub-Repository herunterzuladen. Der Code für diesen Abschnitt befindet sich in `samples/dotnetcore/getting-started/mywebapi`.

## <a name="run-mywebapi"></a>Ausführen von *mywebapi*
1. Öffnen Sie das Projekt `mywebapi` in einem *separaten Visual Studio-Fenster*.
1. Wählen Sie wie zuvor für das Projekt `webfrontend` im Dropdownmenü mit den Starteinstellungen **Azure Dev Spaces** aus. Wählen Sie die bereits erstellte Entwicklungsumgebung aus, statt eine neue zu erstellen. Übernehmen Sie wie zuvor für „Space“ (Bereich) den Standardwert `default`, und klicken Sie auf **OK**. Sie sehen im Ausgabefenster, dass Visual Studio diesen neuen Dienst in Ihrer Entwicklungsumgebung vorbereitet, um das Debuggen zu beschleunigen.
1. Drücken Sie F5, und warten Sie, bis der Dienst erstellt und bereitgestellt wurde. Der Vorgang ist abgeschlossen, wenn die Statusleiste von Visual Studio orange angezeigt wird.
1. Notieren Sie die Endpunkt-URL, die im Fenster **Ausgabe** im Bereich **Azure Dev Spaces for AKS** (Azure Dev Spaces für AKS) angezeigt wird. Sie sieht ungefähr wie folgt aus: http://localhost:\<portnumber\>. Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt, tatsächlich wird er jedoch in der Entwicklungsumgebung in Azure ausgeführt.
2. Wenn `mywebapi` bereit ist, öffnen Sie in Ihrem Browser die localhost-Adresse, und fügen Sie `/api/values` an die URL an, um die Standard-GET-API für `ValuesController` aufzurufen. 
3. Wenn alle Schritte erfolgreich ausgeführt wurden, sollte eine Antwort vom Dienst `mywebapi` angezeigt werden, die wie folgt aussieht:

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

## <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Senden einer Anforderung von *webfrontend* an *mywebapi*
Schreiben Sie nun Code in `webfrontend`, der eine Anforderung an `mywebapi` sendet. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `webfrontend`. *Ersetzen* Sie in der Datei `HomeController.cs` den Code für die About-Methode durch den folgenden Code:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      // Use HeaderPropagatingHttpClient instead of HttpClient so we can propagate
      // headers in the incoming request to any outgoing requests
      using (var client = new HeaderPropagatingHttpClient(this.Request))
      {
          // Call *mywebapi*, and display its response in the page
          var response = await client.GetAsync("http://mywebapi/api/values/1");
          ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
      }

      return View();
   }
   ```

Beachten Sie, wie mit der DNS-Dienstermittlung von Kubernetes auf den Dienst als `http://mywebapi` verwiesen wird. **Code wird in der Entwicklungsumgebung auf die gleiche Weise ausgeführt wie in der Produktion.**

Das obige Codebeispiel nutzt außerdem eine `HeaderPropagatingHttpClient`-Klasse. Diese Hilfsklasse befindet sich in der Datei `HeaderPropagation.cs`, die Ihrem Projekt beim Konfigurieren für die Verwendung von Azure Dev Spaces hinzugefügt wurde. `HeaderPropagatingHttpClient` wird von der bekannten Klasse `HttpClient` abgeleitet und fügt Funktionen hinzu, um bestimmte Header aus einem vorhandenen HttpRequest-ASP.NET-Objekt an ein ausgehendes HttpRequestMessage-Objekt zu übermitteln. Sie sehen später, wie dadurch eine produktivere Entwicklungsumgebung in Teamszenarien ermöglicht wird.

## <a name="debug-across-multiple-services"></a>Debuggen mehrerer Dienste
1. Zu diesem Zeitpunkt sollte `mywebapi` noch mit angefügtem Debugger ausgeführt werden. Ist dies nicht der Fall, drücken Sie im Projekt `mywebapi` F5.
1. Legen Sie einen Breakpoint in der `Get(int id)`-Methode in der Datei `ValuesController.cs` fest, die GET-Anforderungen für `api/values/{id}` verarbeitet.
1. Legen Sie im Projekt `webfrontend`, in das Sie den obigen Code eingefügt haben, direkt vor dem Senden einer GET-Anforderung an `mywebapi/api/values` einen Breakpoint fest.
1. Drücken Sie im Projekt `webfrontend` F5. Visual Studio öffnet erneut einen Browser mit dem entsprechenden localhost-Port, und die Web-App wird angezeigt.
1. Klicken Sie oben auf der Seite auf den Link **Info**, um den Breakpoint im Projekt `webfrontend` auszulösen. 
1. Drücken Sie F10, um den Vorgang fortzusetzen. Der Breakpoint im Projekt `mywebapi` wird nun ausgelöst.
1. Drücken Sie F5, um den Vorgang fortzusetzen, und Sie befinden sich wieder im Code im Projekt `webfrontend`.
1. Durch nochmaliges Drücken von F5 wird die Anforderung abgeschlossen und eine Seite im Browser zurückgegeben. In der Web-App wird auf der Seite „Info“ eine gemeinsame Nachricht der beiden Dienste angezeigt: „Hello from webfrontend and Hello from mywebapi“.

Gut gemacht! Sie besitzen nun eine Anwendung mit mehreren Containern, in der die einzelnen Container separat entwickelt und bereitgestellt werden können.

## <a name="learn-about-team-development"></a>Weitere Informationen zur Teamentwicklung

Bisher haben Sie den Code der Anwendung so ausgeführt, als wären Sie der einzige Entwickler, der an der App arbeitet. In diesem Abschnitt erfahren Sie, wie Azure Dev Spaces die Teamentwicklung optimiert:
* Ermöglichen Sie einem Team von Entwicklern, in der gleichen Umgebungsentwicklung zu arbeiten.
* Jeder Entwickler kann seinen Code isoliert und ohne Beeinträchtigung der anderen Entwickler iterieren.
* Führen Sie vor dem Committen des Codes End-to-End-Tests durch, ohne Modelle erstellen oder Abhängigkeiten simulieren zu müssen.

### <a name="challenges-with-developing-microservices"></a>Herausforderungen bei der Entwicklung von Microservices
Ihre Beispielanwendung ist im Moment nicht sehr komplex. Bei der realen Entwicklung stoßen Sie jedoch bald auf Herausforderungen, wenn Sie mehr Dienste hinzufügen und das Entwicklungsteam wächst.

Stellen Sie sich vor, Sie arbeiten an einem Dienst, der mit Dutzenden anderen Diensten interagiert.

- Die lokale Ausführung aller Komponenten für die Entwicklung kann daher unrealistisch werden. Ihr Entwicklungscomputer verfügt unter Umständen nicht über ausreichend Ressourcen zum Ausführen der gesamten App. Oder Ihre App besitzt vielleicht Endpunkte, die öffentlich zugänglich sein müssen (beispielsweise antwortet Ihre App auf einen Webhook von einer SaaS-App ).
- Sie können versuchen, nur die Dienste auszuführen, auf die Sie angewiesen sind. Das bedeutet jedoch, dass Sie alle Abhängigkeiten kennen müssen (beispielsweise Abhängigkeiten von Abhängigkeiten). Oder es ist nicht ganz einfach, Ihre Abhängigkeiten zu erstellen und auszuführen, da Sie nicht daran gearbeitet haben.
- Einige Entwickler greifen auf die Simulation zahlreicher Dienstabhängigkeiten zurück. Das kann manchmal hilfreich sein. Für die Verwaltung dieser Simulationen kann jedoch bald eigener Entwicklungsaufwand anfallen. Darüber hinaus führt dieser Ansatz dazu, dass Ihre Entwicklungsumgebung nicht der Produktionsumgebung entspricht, und es können sich kleine Fehler einschleichen.
- End-to-End-Tests können daher schwierig werden. Integrationstests sind nur nach einem Commit realistisch. Das bedeutet, dass Ihnen Probleme erst später im Entwicklungszyklus auffallen.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-development-environment"></a>Arbeiten in einer gemeinsamen Entwicklungsumgebung
Mit Azure Dev Spaces können Sie eine *gemeinsame* Entwicklungsumgebung in Azure einrichten. Jeder Entwickler kann sich auf seinen Teil der Anwendung konzentrieren und *Code vor dem Commit* iterativ in einer Umgebung entwickeln, die bereits alle anderen Dienste und Cloudressourcen enthält, die für ihre Szenarien benötigt werden. Abhängigkeiten sind immer aktuell, und Entwickler arbeiten wie in einer Produktionsumgebung.

### <a name="work-in-your-own-space"></a>Arbeiten im eigenen Bereich
Bei der Entwicklung von Code für Ihren Dienst und vor seinem Einchecken ist Code häufig nicht fehlerfrei. Sie strukturieren und testen ihn weiterhin iterativ und experimentieren mit Lösungen. **Bereiche** in Azure Dev Spaces ermöglichen isoliertes Arbeiten ohne Beeinträchtigung Ihrer Teammitglieder.

Stellen Sie mithilfe der folgenden Schritte sicher, dass die Dienste `webfrontend` und `mywebapi` in Ihrer Entwicklungsumgebung **und im Bereich `default`** ausgeführt werden.
1. Schließen Sie für beide Dienste alle F5-/Debugsitzungen, lassen Sie jedoch die Projekte in den jeweiligen Visual Studio-Fenstern geöffnet.
2. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `mywebapi`, und drücken Sie STRG+F5, um den Dienst ohne angefügten Debugger auszuführen.
3. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `webfrontend`, und drücken Sie STRG+F5, um den Dienst ebenfalls auszuführen.

> [!Note]
> Manchmal muss der Browser aktualisiert werden, wenn die Webseite nach dem Drücken von STRG+F5 zum ersten Mal angezeigt wird.

Jeder, der die öffentliche URL öffnet und zur Web-App navigiert, ruft den von Ihnen geschriebenen Codepfad auf, der unter Verwendung des Standardbereichs `default` beide Dienste durchläuft. Nehmen wir nun an, Sie möchten `mywebapi` weiterentwickeln: Wie können Sie vorgehen, ohne andere Entwickler zu stören, die die Entwicklungsumgebung nutzen? Dazu richten Sie Ihren eigenen Bereich ein.

### <a name="create-a-new-space"></a>Erstellen eines neuen Bereichs
In Visual Studio können Sie zusätzliche Bereiche erstellen, die verwendet werden, wenn Sie für Ihren Dienst F5 oder STRG+F5 drücken. Sie können einem Bereich einen beliebigen Namen mit einer beliebigen Bedeutung geben (etwa `sprint4` oder `demo`).

Gehen Sie wie folgt vor, um einen neuen Bereich zu erstellen:
1. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `mywebapi`.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.
3. Klicken Sie links auf die Registerkarte **Debuggen**, um die Azure Dev Spaces-Einstellungen anzuzeigen.
4. Hier können Sie den Cluster/Bereich ändern oder erstellen, der beim Drücken von F5 oder STRG+F5 verwendet wird. *Stellen Sie sicher, dass die zuvor erstellte Azure Dev Space-Instanz ausgewählt ist.*
5. Wählen Sie in der Dropdownliste „Space“ (Bereich) die Option **<Create New Space…>** (<Neuen Bereich erstellen...>).

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. Geben Sie im Dialogfeld **Add Space** (Bereich hinzufügen) einen Namen für den Bereich ein, und klicken Sie auf **OK**. Sie können Ihren Namen (z.B. „scott“) für den neuen Bereich verwenden, damit Ihre Kollegen erkennen können, in welchem Bereich Sie arbeiten.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Auf der Seite mit den Projekteigenschaften sollten nun Ihre Entwicklungsumgebung und der neue Bereich ausgewählt sein.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>Aktualisieren von Code für *mywebapi*

1. Ändern Sie im Projekt `mywebapi` in der Datei `ValuesController.cs` den Code der `string Get(int id)`-Methode wie folgt:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Legen Sie einen Breakpoint in diesem aktualisierten Codeblock fest. (Unter Umständen wurde zuvor bereits ein Breakpoint festgelegt.)
3. Drücken Sie F5, um den Dienst `mywebapi` zu starten. Dadurch wird der Dienst in Ihrer Entwicklungsumgebung im ausgewählten Bereich (in diesem Fall `scott`) gestartet.

Hier sehen Sie ein Diagramm, in dem die Funktionsweise der verschiedenen Bereiche erläutert wird. Der blaue Pfad zeigt eine Anforderung über den Bereich `default`. Dabei handelt es sich um den Standardpfad, der verwendet wird, wenn der URL kein Bereich vorangestellt wird. Der grüne Pfad zeigt eine Anforderung über den Bereich `scott`.

![](media/common/Space-Routing.png)

Mit dieser integrierten Funktion von Azure Dev Spaces können Sie End-to-End-Tests für Code in einer gemeinsamen Umgebung ausführen, ohne dass die einzelnen Entwickler den kompletten Dienststapel in ihrem Bereich neu erstellen müssen. Für dieses Routing müssen Weitergabeheader in Ihrem App-Code weitergeleitet werden, wie im vorherigen Schritt in diesem Leitfaden erläutert.

### <a name="test-code-running-in-the-scott-space"></a>Testen von Code, der im Bereich `scott` ausgeführt wird
Wenn Sie Ihre neue Version von `mywebapi` in Verbindung mit `webfrontend` testen möchten, öffnen Sie in Ihrem Browser die URL des öffentlichen Zugriffspunkts für `webfrontend` (etwa http://webfrontend-teamenv.123456abcdef.eastus.aksapp.io), und navigieren Sie zur Seite „Info“. Die ursprüngliche Nachricht „Hello from webfrontend and Hello from mywebapi“ sollte angezeigt werden.

Fügen Sie nun den Teil „scott.s.“ zur URL hinzu, sodass sie ungefähr wie folgt lautet: http://scott.s.webfrontend-teamenv.123456abcdef.eastus.aksapp.io. Aktualisieren Sie anschließend den Browser. Der im Projekt `mywebapi` festgelegte Breakpoint sollte erreicht werden. Klicken Sie auf F5, um den Vorgang fortzusetzen. Im Browser sollte jetzt die neue Nachricht „Hello from webfrontend and mywebapi now says something new.“ angezeigt werden. Der Grund hierfür ist, dass der Pfad zum aktualisierten Code in `mywebapi` im Bereich `scott` ausgeführt wird.

[!INCLUDE[](includes/well-done.md)]

[!INCLUDE[](includes/clean-up.md)]
