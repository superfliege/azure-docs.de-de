---
title: Erstellen eines Kubernetes-Entwicklungsbereichs in der Cloud mit .NET Core und Visual Studio | Microsoft-Dokumentation
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
ms.openlocfilehash: 93c1c9cb27e5eb2d56583dccaffe92e9d50ecc2d
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36959273"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core-and-visual-studio"></a>Erste Schritte in Azure Dev Spaces mit .NET Core und Visual Studio

In diesem Leitfaden lernen Sie Folgendes:

- Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
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

## <a name="create-a-web-app-running-in-a-container"></a>Erstellen einer Web-App, die in einem Container ausgeführt wird

In diesem Abschnitt erstellen Sie eine ASP.NET Core-Web-App und bereiten sie für die Ausführung in einem Container in Kubernetes vor.

### <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

Erstellen Sie in Visual Studio 2017 ein neues Projekt. Derzeit muss das Projekt eine **ASP.NET Core-Webanwendung** sein. Nennen Sie das Projekt **webfrontend**.

![](media/get-started-netcore-visualstudio/NewProjectDialog1.png)

Wählen Sie die Vorlage **Webanwendung (Model-View-Controller)** aus, und legen Sie oben im Dialogfeld in den beiden Dropdownmenüs **.NET Core** und **ASP.NET Core 2.0** fest. Klicken Sie auf **OK**, um das Projekt zu erstellen.

![](media/get-started-netcore-visualstudio/NewProjectDialog2.png)


### <a name="enable-dev-spaces-for-an-aks-cluster"></a>Aktivieren von Dev Spaces für einen AKS-Cluster

Wählen Sie für das soeben erstellte Projekt wie nachfolgend gezeigt im Dropdownmenü mit den Starteinstellungen die Option **Azure Dev Spaces**.

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
> Sie können die Anwendung erst debuggen, wenn Ihr Entwicklungsbereich erstellt wurde.

### <a name="look-at-the-files-added-to-project"></a>Anzeigen der zum Projekt hinzugefügten Dateien
Während Sie auf die Erstellung des Entwicklungsbereichs warten, sehen Sie sich die Dateien an, die Ihrem Projekt hinzugefügt wurden, als Sie sich für die Verwendung eines Entwicklungsbereichs entschieden haben.

Sie sehen zuerst einen Ordner namens `charts`, der hinzugefügt wurde. In diesem Ordner wurde ein [Helm-Diagramm](https://docs.helm.sh) für Ihre Anwendung erstellt. Diese Dateien werden zum Bereitstellen der Anwendung im Entwicklungsbereich verwendet.

Sie sehen, dass die Datei `Dockerfile` hinzugefügt wurde. Diese Datei enthält Informationen, die zum Packen Ihrer Anwendung im Docker-Standardformat erforderlich sind.

Außerdem wurde die Datei `azds.yaml` hinzugefügt. Sie enthält die Konfiguration der Entwicklungszeit, die für den Entwicklungsbereich erforderlich ist.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Debuggen eines Containers in Kubernetes
Wenn der Entwicklungsbereich erstellt wurde, können Sie die Anwendung debuggen. Legen Sie einen Breakpoint im Code fest, etwa in Zeile 20 in der Datei `HomeController.cs`, in der die Variable `Message` festgelegt wird. Drücken Sie **F5**, um das Debuggen zu starten. 

Visual Studio kommuniziert zum Erstellen und Bereitstellen der Anwendung mit dem Entwicklungsbereich und öffnet dann einen Browser mit der ausgeführten Web-App. Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt, tatsächlich wird er jedoch im Entwicklungsbereich in Azure ausgeführt. Die localhost-Adresse wird verwendet, da Azure Dev Spaces einen temporären SSH-Tunnel zu dem in AKS ausgeführten Container erstellt.

Klicken Sie oben auf der Seite auf den Link **Info**, um den Breakpoint auszulösen. Sie besitzen wie bei der lokalen Ausführung des Codes Vollzugriff auf Debuginformationen, etwa Aufrufliste, lokale Variablen, Ausnahmeinformationen usw.

## <a name="iteratively-develop-code"></a>Iteratives Entwickeln von Code

Bei Azure Dev Spaces geht es nicht nur um die Ausführung von Code in Kubernetes: Mit diesem Dienst sollen Codeänderungen in einer Kubernetes-Umgebung in der Cloud schnell und iterativ sichtbar gemacht werden.

### <a name="update-a-content-file"></a>Aktualisieren einer Inhaltsdatei
1. Navigieren Sie zur Datei `./Views/Home/Index.cshtml`, und ändern Sie die HTML. Beispiel: Ändern Sie Zeile 70 (`<h2>Application uses</h2>`) ungefähr wie folgt: `<h2>Hello k8s in Azure!</h2>`
1. Speichern Sie die Datei .
1. Aktualisieren Sie die Anzeige im Browser. Daraufhin sollte auf der Webseite die aktualisierte HTML angezeigt werden.

Was ist passiert? Für Änderungen an Inhaltsdateien (z.B. HTML und CSS) ist keine erneute Kompilierung in einer .NET Core-Web-App erforderlich. Eine aktive F5-Sitzung synchronisiert daher automatisch alle geänderten Inhaltsdateien im ausgeführten Container in AKS, sodass alle Inhaltsänderungen direkt angezeigt werden.

### <a name="update-a-code-file"></a>Aktualisieren einer Codedatei
Die Aktualisierung von Codedateien ist etwas aufwendiger, da eine .NET Core-App aktualisierte Anwendungsbinärdateien neu erstellen und generieren muss.

1. Beenden Sie den Debugger in Visual Studio.
1. Öffnen Sie die Codedatei mit dem Namen `Controllers/HomeController.cs`, und bearbeiten Sie die Nachricht, die auf der Seite „Info“ angezeigt wird: `ViewData["Message"] = "Your application description page.";`.
1. Speichern Sie die Datei .
1. Drücken Sie **F5**, um das Debuggen wieder zu starten. 

Das Neuerstellen und erneute Bereitstellen eines neuen Containerimages bei jeder vorgenommenen Codeänderung kann geraume Zeit in Anspruch nehmen. Daher kompiliert Azure Dev Spaces Code im vorhandenen Container inkrementell, um den Bearbeitungs-/Debugging-Kreislauf zu beschleunigen.

Aktualisieren Sie die Web-App im Browser, und wechseln Sie zur Seite „Info“. Daraufhin sollte Ihre benutzerdefinierte Meldung auf der Benutzeroberfläche angezeigt werden.


## <a name="call-another-container"></a>Aufrufen eines anderen Containers
In diesem Abschnitt erstellen Sie einen zweiten Dienst (`mywebapi`) und lassen ihn von `webfrontend` aufrufen. Jeder Dienst wird in einem separaten Container ausgeführt. Anschließend debuggen Sie beide Container.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Herunterladen von Beispielcode für *mywebapi*
Laden Sie Beispielcode aus einem GitHub-Repository herunter, um Zeit zu sparen. Navigieren Sie zu https://github.com/Azure/dev-spaces, und klicken Sie auf **Klonen oder herunterladen**, um das GitHub-Repository herunterzuladen. Der Code für diesen Abschnitt befindet sich in `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ausführen von *mywebapi*
1. Öffnen Sie das Projekt `mywebapi` in einem *separaten Visual Studio-Fenster*.
1. Wählen Sie wie zuvor für das Projekt `webfrontend` im Dropdownmenü mit den Starteinstellungen **Azure Dev Spaces** aus. Erstellen Sie diesmal keinen neuen AKS-Cluster, sondern wählen Sie den bereits erstellten Cluster aus. Übernehmen Sie wie zuvor für „Space“ (Bereich) den Standardwert `default`, und klicken Sie auf **OK**. Sie sehen im Ausgabefenster, dass Visual Studio diesen neuen Dienst in Ihrem Entwicklungsbereich vorbereitet, um das Debuggen zu beschleunigen.
1. Drücken Sie F5, und warten Sie, bis der Dienst erstellt und bereitgestellt wurde. Der Vorgang ist abgeschlossen, wenn die Statusleiste von Visual Studio orange angezeigt wird.
1. Notieren Sie die Endpunkt-URL, die im Fenster **Ausgabe** im Bereich **Azure Dev Spaces for AKS** (Azure Dev Spaces für AKS) angezeigt wird. Sie sieht ungefähr wie folgt aus: http://localhost:\<portnumber\>. Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt, tatsächlich wird er jedoch im Entwicklungsbereich in Azure ausgeführt.
2. Wenn `mywebapi` bereit ist, öffnen Sie in Ihrem Browser die localhost-Adresse, und fügen Sie `/api/values` an die URL an, um die Standard-GET-API für `ValuesController` aufzurufen. 
3. Wenn alle Schritte erfolgreich ausgeführt wurden, sollte eine Antwort vom Dienst `mywebapi` angezeigt werden, die wie folgt aussieht:

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Senden einer Anforderung von *webfrontend* an *mywebapi*
Schreiben Sie nun Code in `webfrontend`, der eine Anforderung an `mywebapi` sendet. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `webfrontend`. *Ersetzen* Sie in der Datei `HomeController.cs` den Code für die About-Methode durch den folgenden Code:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

Im obigen Codebeispiel wird der Header `azds-route-as` aus der eingehenden Anforderung an die ausgehende Anforderung weitergeleitet. Sie sehen später, wie dadurch eine produktivere Entwicklungsumgebung in Teamszenarien ermöglicht wird.

### <a name="debug-across-multiple-services"></a>Debuggen mehrerer Dienste
1. Zu diesem Zeitpunkt sollte `mywebapi` noch mit angefügtem Debugger ausgeführt werden. Ist dies nicht der Fall, drücken Sie im Projekt `mywebapi` F5.
1. Legen Sie einen Breakpoint in der `Get(int id)`-Methode in der Datei `Controllers/ValuesController.cs` fest, die GET-Anforderungen für `api/values/{id}` verarbeitet.
1. Legen Sie im Projekt `webfrontend`, in das Sie den obigen Code eingefügt haben, direkt vor dem Senden einer GET-Anforderung an `mywebapi/api/values` einen Breakpoint fest.
1. Drücken Sie im Projekt `webfrontend` F5. Visual Studio öffnet erneut einen Browser mit dem entsprechenden localhost-Port, und die Web-App wird angezeigt.
1. Klicken Sie oben auf der Seite auf den Link **Info**, um den Breakpoint im Projekt `webfrontend` auszulösen. 
1. Drücken Sie F10, um den Vorgang fortzusetzen. Der Breakpoint im Projekt `mywebapi` wird nun ausgelöst.
1. Drücken Sie F5, um den Vorgang fortzusetzen, und Sie befinden sich wieder im Code im Projekt `webfrontend`.
1. Durch nochmaliges Drücken von F5 wird die Anforderung abgeschlossen und eine Seite im Browser zurückgegeben. In der Web-App wird auf der Seite „Info“ eine gemeinsame Nachricht der beiden Dienste angezeigt: „Hello from webfrontend and Hello from mywebapi“.

Gut gemacht! Sie besitzen nun eine Anwendung mit mehreren Containern, in der die einzelnen Container separat entwickelt und bereitgestellt werden können.

## <a name="learn-about-team-development"></a>Weitere Informationen zur Teamentwicklung

Bisher haben Sie den Code der Anwendung so ausgeführt, als wären Sie der einzige Entwickler, der an der App arbeitet. In diesem Abschnitt erfahren Sie, wie Azure Dev Spaces die Teamentwicklung optimiert:
* Ein Team von Entwicklern kann in der gleichen Umgebung arbeiten und je nach Bedarf einen gemeinsamen oder einen getrennten Entwicklungsbereich nutzen.
* Jeder Entwickler kann seinen Code isoliert und ohne Beeinträchtigung der anderen Entwickler durchlaufen.
* Testen Sie Ihren Code vollständig, bevor Sie ihn committen, ohne Modelle erstellen oder Abhängigkeiten simulieren zu müssen.

### <a name="challenges-with-developing-microservices"></a>Herausforderungen bei der Entwicklung von Microservices
Ihre Beispielanwendung ist im Moment nicht sehr komplex. Bei der realen Entwicklung stoßen Sie jedoch bald auf Herausforderungen, wenn Sie mehr Dienste hinzufügen und das Entwicklungsteam wächst.

Stellen Sie sich vor, Sie arbeiten an einem Dienst, der mit Dutzenden anderen Diensten interagiert.

- Die lokale Ausführung aller Komponenten für die Entwicklung kann daher unrealistisch werden. Ihr Entwicklungscomputer verfügt unter Umständen nicht über ausreichend Ressourcen zum Ausführen der gesamten App. Oder Ihre App verwendet vielleicht Endpunkte, die öffentlich zugänglich sein müssen (beispielsweise antwortet Ihre App auf einen Webhook von einer SaaS-App).
- Sie können versuchen, nur die Dienste auszuführen, auf die Sie angewiesen sind. Das bedeutet jedoch, dass Sie alle Abhängigkeiten kennen müssen (beispielsweise Abhängigkeiten von Abhängigkeiten). Oder es ist nicht ganz einfach, Abhängigkeiten zu erstellen und auszuführen, da Sie an diesen nicht selbst gearbeitet haben.
- Einige Entwickler greifen auf die Simulation zahlreicher Dienstabhängigkeiten zurück. Das kann manchmal hilfreich sein. Für die Verwaltung dieser Simulationen kann jedoch bald eigener Entwicklungsaufwand anfallen. Darüber hinaus führt dieser Ansatz dazu, dass Ihre Entwicklungsumgebung nicht der Produktionsumgebung entspricht, und es können sich kleine Fehler einschleichen.
- End-to-End-Tests können daher schwierig werden. Integrationstests sind nur nach einem Commit realistisch. Das bedeutet, dass Ihnen Probleme erst später im Entwicklungszyklus auffallen.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Arbeiten in einem gemeinsamen Entwicklungsbereich
Mit Azure Dev Spaces können Sie einen *gemeinsamen* Entwicklungsbereich in Azure einrichten. Jeder Entwickler kann sich auf seinen Teil der Anwendung konzentrieren und *Code vor dem Commit* iterativ in einem Entwicklungsbereich entwickeln, der bereits alle anderen Dienste und Cloudressourcen enthält, die der Entwickler für seine Szenarien benötigt. Abhängigkeiten sind immer aktuell, und Entwickler arbeiten wie in einer Produktionsumgebung.

### <a name="work-in-your-own-space"></a>Arbeiten im eigenen Bereich
Während der Entwicklung von Code für Ihren Dienst und vor dem Einchecken ist Code häufig nicht fehlerfrei. Sie strukturieren und testen den Code weiterhin iterativ und experimentieren mit Lösungen. In einem **Bereich** – einem Konzept von in Azure Dev Spaces – können Sie isoliert arbeiten, ohne Ihre Teammitglieder zu beeinträchtigen.

Stellen Sie mithilfe der folgenden Schritte sicher, dass die Dienste `webfrontend` und `mywebapi` **im Entwicklungsbereich `default`** ausgeführt werden.
1. Schließen Sie für beide Dienste alle F5-/Debugsitzungen, lassen Sie jedoch die Projekte in den jeweiligen Visual Studio-Fenstern geöffnet.
2. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `mywebapi`, und drücken Sie STRG+F5, um den Dienst ohne angefügten Debugger auszuführen.
3. Wechseln Sie zum Visual Studio-Fenster mit dem Projekt `webfrontend`, und drücken Sie STRG+F5, um den Dienst ebenfalls auszuführen.

> [!Note]
> Manchmal muss der Browser aktualisiert werden, wenn die Webseite nach dem Drücken von STRG+F5 zum ersten Mal angezeigt wird.

Jeder, der die öffentliche URL öffnet und zur Web-App navigiert, ruft den von Ihnen geschriebenen Codepfad auf, der unter Verwendung des Standardbereichs `default` beide Dienste durchläuft. Nehmen wir nun an, Sie möchten `mywebapi` weiterentwickeln: Wie können Sie vorgehen, ohne andere Entwickler zu beeinträchtigen, die den Entwicklungsbereich nutzen? Die Lösung: Sie richten Ihren eigenen Bereich ein.

### <a name="create-a-new-dev-space"></a>Erstellen eines neuen Entwicklungsbereichs
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

7. Auf der Seite mit den Projekteigenschaften sollten nun Ihr AKS-Cluster und der neue Bereich ausgewählt sein.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>Aktualisieren von Code für *mywebapi*

1. Ändern Sie im Projekt `mywebapi` in der Datei `Controllers/ValuesController.cs` den Code der `string Get(int id)`-Methode wie folgt:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Legen Sie einen Breakpoint in diesem aktualisierten Codeblock fest. (Unter Umständen wurde zuvor bereits ein Breakpoint festgelegt.)
3. Drücken Sie F5, um den Dienst `mywebapi` zu starten. Dadurch wird der Dienst in Ihrem Cluster im ausgewählten Bereich (in diesem Fall: `scott`) gestartet.

Hier sehen Sie ein Diagramm, in dem die Funktionsweise der verschiedenen Bereiche erläutert wird. Der violette Pfad zeigt eine Anforderung über den Bereich `default`. Dabei handelt es sich um den Standardpfad, der verwendet wird, wenn der URL kein Bereich vorangestellt wird. Der rosafarbene Pfad zeigt eine Anforderung über den Bereich `default/scott`.

![](media/common/Space-Routing.png)

Mit dieser integrierten Funktion von Azure Dev Spaces können Sie End-to-End-Tests für Code in einer gemeinsamen Umgebung ausführen, ohne dass die einzelnen Entwickler den kompletten Dienststapel in ihrem Bereich neu erstellen müssen. Für dieses Routing müssen Weitergabeheader in Ihrem App-Code weitergeleitet werden, wie im vorherigen Schritt in diesem Leitfaden erläutert.

### <a name="test-code-running-in-the-defaultscott-space"></a>Testen von Code, der im Bereich `default/scott` ausgeführt wird
Wenn Sie Ihre neue Version von `mywebapi` in Verbindung mit `webfrontend` testen möchten, öffnen Sie in Ihrem Browser die URL des öffentlichen Zugriffspunkts für `webfrontend` (etwa http://webfrontend.123456abcdef.eastus.aksapp.io), und navigieren Sie zur Seite „Info“. Die ursprüngliche Nachricht „Hello from webfrontend and Hello from mywebapi“ sollte angezeigt werden.

Fügen Sie nun den Teil „scott.s.“ zur URL hinzu, sodass sie ungefähr wie folgt lautet: http://scott.s.webfrontend.123456abcdef.eastus.aksapp.io. Aktualisieren Sie anschließend den Browser. Der im Projekt `mywebapi` festgelegte Breakpoint sollte erreicht werden. Klicken Sie auf F5, um den Vorgang fortzusetzen. Im Browser sollte jetzt die neue Nachricht „Hello from webfrontend and mywebapi now says something new.“ angezeigt werden. Der Grund hierfür ist, dass der Pfad zum aktualisierten Code in `mywebapi` im Bereich `default/scott` ausgeführt wird.

[!INCLUDE[](includes/well-done.md)]

[!INCLUDE[](includes/clean-up.md)]
