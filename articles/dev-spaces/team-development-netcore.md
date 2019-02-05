---
title: Teamentwicklung mit Azure Dev Spaces mit .NET Core und VS Code | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: c411a227c9cb277f6bf16df5085b1e674bd37176
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463904"
---
# <a name="team-development-with-azure-dev-spaces"></a>Teamentwicklung mit Azure Dev Spaces

In diesem Tutorial erfahren Sie, wie Sie mehrere Entwicklungsbereiche (Dev Spaces) verwenden, um gleichzeitig in unterschiedlichen Entwicklungsumgebungen zu arbeiten, wobei verschiedene Arbeitsprozesse in verschiedenen Dev Spaces aber im gleichen Cluster beibehalten werden.

## <a name="call-a-service-running-in-a-separate-container"></a>Aufrufen eines in einem separaten Container ausgeführten Diensts

In diesem Abschnitt erstellen Sie einen zweiten Dienst (`mywebapi`) und lassen ihn von `webfrontend` aufrufen. Jeder Dienst wird in einem separaten Container ausgeführt. Anschließend debuggen Sie beide Container.

![Mehrere Container](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Herunterladen von Beispielcode für *mywebapi*
Laden Sie Beispielcode aus einem GitHub-Repository herunter, um Zeit zu sparen. Navigieren Sie zu https://github.com/Azure/dev-spaces, und klicken Sie auf **Klonen oder herunterladen**, um das GitHub-Repository herunterzuladen. Der Code für diesen Abschnitt befindet sich in `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ausführen von *mywebapi*
1. Öffnen Sie den Ordner `mywebapi` in einem *separaten VS Code-Fenster*.
1. Öffnen Sie die **Befehlspalette** (über das Menü **Ansicht | Befehlspalette**), verwenden Sie die automatische Vervollständigung für die Eingabe, und wählen Sie den Befehl `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces` aus. Dieser Befehl darf nicht mit dem Befehl `azds prep` verwechselt werden, der zum Konfigurieren des Projekts für die Bereitstellung dient.
1. Drücken Sie F5, und warten Sie, bis der Dienst erstellt und bereitgestellt wurde. Der Vorgang ist abgeschlossen, wenn die Debugleiste von VS Code angezeigt wird.
1. Die Endpunkt-URL sieht ungefähr wie folgt aus: http://localhost:\<portnumber\>. **Tipp: Auf der Statusleiste von VS Code wird eine klickbare URL angezeigt.** Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt. Tatsächlich wird er jedoch im Entwicklungsbereich in Azure ausgeführt. Die localhost-Adresse wird verwendet, da `mywebapi` keine öffentlichen Endpunkte definiert hat und auf den Dienst nur über die Kubernetes-Instanz zugegriffen werden kann. Um die Interaktion mit dem privaten Dienst auf Ihrem lokalen Computer zu erleichtern, erstellt Azure Dev Spaces einen temporären SSH-Tunnel zu dem in Azure ausgeführten Container.
1. Öffnen Sie in Ihrem Browser die localhost-Adresse, wenn `mywebapi` bereit ist. Fügen Sie `/api/values` an die URL an, um die Standard-GET-API für `ValuesController` aufzurufen. 
1. Wenn alle Schritte erfolgreich ausgeführt wurden, sollte eine Antwort vom Dienst `mywebapi` angezeigt werden.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Senden einer Anforderung von *webfrontend* an *mywebapi*
Schreiben Sie nun Code in `webfrontend`, der eine Anforderung an `mywebapi` sendet.
1. Wechseln Sie ins VS Code-Fenster für `webfrontend`.
1. *Ersetzen Sie* den Code für die About-Methode:

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

Im obigen Codebeispiel wird der Header `azds-route-as` aus der eingehenden Anforderung an die ausgehende Anforderung weitergeleitet. Sie erfahren später, wie dadurch Teams bei der gemeinsamen Entwicklung unterstützt werden.

### <a name="debug-across-multiple-services"></a>Debuggen mehrerer Dienste
1. Zu diesem Zeitpunkt sollte `mywebapi` noch mit angefügtem Debugger ausgeführt werden. Ist dies nicht der Fall, drücken Sie im Projekt `mywebapi` F5.
1. Legen Sie einen Breakpoint in der `Get(int id)`-Methode fest, die `api/values/{id}`-GET-Anforderungen verarbeitet.
1. Legen Sie im Projekt `webfrontend` direkt vor dem Senden einer GET-Anforderung an `mywebapi/api/values` einen Breakpoint fest.
1. Drücken Sie im Projekt `webfrontend` F5.
1. Rufen Sie die Web-App auf, und durchlaufen Sie den Code in beiden Diensten.
1. In der Web-App wird auf der Seite „Info“ eine gemeinsame Nachricht der beiden Dienste angezeigt: „Hello from webfrontend and Hello from mywebapi“.


Gut gemacht! Sie besitzen nun eine Anwendung mit mehreren Containern, in der die einzelnen Container separat entwickelt und bereitgestellt werden können.

## <a name="learn-about-team-development"></a>Weitere Informationen zur Teamentwicklung

[!INCLUDE [](../../includes/team-development-1.md)]

Sehen Sie sich die Anwendung in Aktion an. Wechseln Sie ins VS Code-Fenster für `mywebapi`, und ändern Sie die `string Get(int id)`-Methode etwa wie folgt:

```csharp
[HttpGet("{id}")]
public string Get(int id)
{
    return "mywebapi now says something new";
}
```


[!INCLUDE [](../../includes/team-development-2.md)]

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
