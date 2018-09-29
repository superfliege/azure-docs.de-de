---
title: Teamentwicklung für Azure Dev Spaces mit .NET Core und Visual Studio | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 2a04b80e728ecf0af39cb46041a005a86ea1abec
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406106"
---
# <a name="team-development-with-azure-dev-spaces"></a>Teamentwicklung mit Azure Dev Spaces

In diesem Tutorial erfahren Sie, wie Sie mehrere Entwicklungsbereiche (Dev Spaces) verwenden, um gleichzeitig in unterschiedlichen Entwicklungsumgebungen zu arbeiten, wobei verschiedene Arbeitsprozesse in verschiedenen Dev Spaces aber im gleichen Cluster beibehalten werden.

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

6. Legen Sie im Dialogfeld **Bereich hinzufügen** den übergeordneten Bereich auf **default** fest, und geben Sie einen Namen für den neuen Bereich ein. Sie können Ihren Namen (z.B. „scott“) für den neuen Bereich verwenden, damit Ihre Kollegen erkennen können, in welchem Bereich Sie arbeiten. Klicken Sie auf **OK**.

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

### <a name="well-done"></a>Gut gemacht!
Sie haben den Leitfaden zu den ersten Schritten abgeschlossen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
> * Iteratives Entwickeln von Code in Containern
> * Unabhängiges Entwickeln von zwei separaten Diensten und Verwenden der DNS-Dienstermittlung von Kubernetes, um einen anderen Dienst aufzurufen
> * Produktives Entwickeln und Testen Ihres Codes in einer Teamumgebung

Nachdem Sie sich mit Azure Dev Spaces vertraut gemacht haben, können Sie [Ihren Entwicklungsbereich für Teammitglieder freigeben](how-to/share-dev-spaces.md) und ihnen zeigen, wie dies die Zusammenarbeit vereinfacht.

## <a name="clean-up"></a>Bereinigen
Um eine Azure Dev Spaces-Instanz in einem Cluster vollständig zu löschen, einschließlich aller Entwicklungsbereiche und der darin ausgeführten Dienste, können Sie den Befehl `az aks remove-dev-spaces` verwenden. Beachten Sie, dass diese Aktion nicht rückgängig gemacht werden kann. Sie können dem Cluster die Unterstützung für Azure Dev Spaces wieder hinzufügen, aber dies entspricht einem kompletten Neubeginn. Ihre alten Dienste und Bereiche werden nicht wiederhergestellt.

Im folgenden Beispiel werden die Azure Dev Spaces-Controller Ihres aktiven Abonnements aufgelistet, und anschließend wird der Azure Dev Spaces-Controller gelöscht, der dem AKS-Cluster „myaks“ in der Ressourcengruppe „myaks-rg“ zugeordnet ist.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
