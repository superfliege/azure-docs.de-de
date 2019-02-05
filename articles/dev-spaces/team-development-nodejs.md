---
title: Teamentwicklung mit Azure Dev Spaces mit VS Code | Microsoft-Dokumentation
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
ms.openlocfilehash: 8a7200723b39c40fe9596bbab0acaf752acbbaf8
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55451392"
---
# <a name="team-development-with-azure-dev-spaces"></a>Teamentwicklung mit Azure Dev Spaces

In diesem Tutorial erfahren Sie, wie Sie mehrere Entwicklungsbereiche (Dev Spaces) verwenden, um gleichzeitig in unterschiedlichen Entwicklungsumgebungen zu arbeiten, wobei verschiedene Arbeitsprozesse in verschiedenen Dev Spaces aber im gleichen Cluster beibehalten werden.

## <a name="call-a-service-running-in-a-separate-container"></a>Aufrufen eines in einem separaten Container ausgeführten Diensts

In diesem Abschnitt erstellen Sie einen zweiten Dienst (`mywebapi`) und lassen ihn von `webfrontend` aufrufen. Jeder Dienst wird in einem separaten Container ausgeführt. Anschließend debuggen Sie beide Container.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>Öffnen von Beispielcode für *mywebapi*
Der Beispielcode für `mywebapi` für diese Anleitung sollte bereits im Ordner `samples` enthalten sein. (Rufen Sie andernfalls https://github.com/Azure/dev-spaces auf, und klicken Sie auf **Klonen oder herunterladen**, um das GitHub-Repository herunterzuladen.) Der Code für diesen Abschnitt befindet sich in `samples/nodejs/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ausführen von *mywebapi*
1. Öffnen Sie den Ordner `mywebapi` in einem *separaten VS Code-Fenster*.
1. Öffnen Sie die **Befehlspalette** (über das Menü **Ansicht | Befehlspalette**), verwenden Sie die automatische Vervollständigung für die Eingabe, und wählen Sie den Befehl `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces` aus. Dieser Befehl darf nicht mit dem Befehl `azds prep` verwechselt werden, der zum Konfigurieren des Projekts für die Bereitstellung dient.
1. Drücken Sie F5, und warten Sie, bis der Dienst erstellt und bereitgestellt wurde. Der Vorgang ist abgeschlossen, wenn die Debugleiste von VS Code angezeigt wird.
1. Notieren Sie sich die Endpunkt-URL. Sie sieht ungefähr wie folgt aus: http://localhost:\<portnumber\>. **Tipp: Auf der Statusleiste von VS Code wird eine klickbare URL angezeigt.** Es sieht unter Umständen so aus, als würde der Container lokal ausgeführt, tatsächlich wird er jedoch in der Entwicklungsumgebung in Azure ausgeführt. Die localhost-Adresse wird verwendet, da `mywebapi` keine öffentlichen Endpunkte definiert hat und auf den Dienst nur über die Kubernetes-Instanz zugegriffen werden kann. Um die Interaktion mit dem privaten Dienst auf Ihrem lokalen Computer zu erleichtern, erstellt Azure Dev Spaces einen temporären SSH-Tunnel zu dem in Azure ausgeführten Container.
1. Öffnen Sie in Ihrem Browser die localhost-Adresse, wenn `mywebapi` bereit ist. Eine Antwort vom Dienst `mywebapi` („Hello from mywebapi“) sollte angezeigt werden.


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Senden einer Anforderung von *webfrontend* an *mywebapi*
Schreiben Sie nun Code in `webfrontend`, der eine Anforderung an `mywebapi` sendet.
1. Wechseln Sie ins VS Code-Fenster für `webfrontend`.
1. Fügen Sie die folgenden Codezeilen oben in `server.js` ein:
    ```javascript
    var request = require('request');
    ```

3. *Ersetzen Sie* den Code für den GET-Handler für `/api`. Bei der Verarbeitung einer Anforderung ruft diese wiederum `mywebapi` auf und gibt anschließend die Ergebnisse aus beiden Diensten zurück.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```
 4. *Entfernen Sie* die `server.close()`-Zeile am Ende von `server.js`.

Im obigen Codebeispiel wird der Header `azds-route-as` aus der eingehenden Anforderung an die ausgehende Anforderung weitergeleitet. Sie erfahren später, wie dadurch Teams bei der gemeinsamen Entwicklung unterstützt werden.

### <a name="debug-across-multiple-services"></a>Debuggen mehrerer Dienste
1. Zu diesem Zeitpunkt sollte `mywebapi` noch mit angefügtem Debugger ausgeführt werden. Ist dies nicht der Fall, drücken Sie im Projekt `mywebapi` F5.
1. Legen Sie im GET-Standardhandler für `/` einen Breakpoint fest.
1. Legen Sie im Projekt `webfrontend` direkt vor dem Senden einer GET-Anforderung an `http://mywebapi` einen Breakpoint fest.
1. Drücken Sie im Projekt `webfrontend` F5.
1. Öffnen Sie die Web-App, und durchlaufen Sie den Code in beiden Diensten. In der Web-App sollte eine gemeinsame Nachricht der beiden Dienste angezeigt werden: „Hello from webfrontend and Hello from mywebapi“.

Gut gemacht! Sie besitzen nun eine Anwendung mit mehreren Containern, in der die einzelnen Container separat entwickelt und bereitgestellt werden können.

## <a name="learn-about-team-development"></a>Weitere Informationen zur Teamentwicklung

[!INCLUDE [](../../includes/team-development-1.md)]

Sehen Sie sich das nun in Aktion an:
1. Wechseln Sie ins VS Code-Fenster für `mywebapi`, und nehmen Sie eine Codeänderung am GET-Standardhandler für `/` vor. Beispiel:

    ```javascript
    app.get('/', function (req, res) {
        res.send('mywebapi now says something new');
    });
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




