---
title: Includedatei
description: Includedatei
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: include
manager: yuvalm
ms.openlocfilehash: 749120446619bf682d02be0f9290a6d47540c16a
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55664406"
---
### <a name="run-the-service"></a>Ausführen des Diensts

1. Drücken Sie F5 (oder geben Sie im Terminalfenster `azds up` ein), um den Dienst auszuführen. Der Dienst wird automatisch im neu ausgewählten Bereich _dev/scott_ ausgeführt. 
1. Sie können bestätigen, dass Ihr Dienst in einem eigenen Bereich ausgeführt wird, indem Sie `azds list-up` erneut ausführen. Sie werden bemerken, dass jetzt eine Instanz von *mywebapi* im Bereich _dev/scott_ ausgeführt wird (die Version in _dev_ wird noch ausgeführt, aber nicht aufgelistet).

    ```
    Name                      DevSpace  Type     Updated  Status
    mywebapi                  scott     Service  3m ago   Running
    mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
    webfrontend               dev       Service  26m ago  Running
    ```

1. Führen Sie `azds list-uris` aus, und beachten Sie die Zugriffspunkt-URL für *webfrontend*.

    ```
    Uri                                                                        Status
    -------------------------------------------------------------------------  ---------
    http://localhost:53831 => mywebapi.scott:80                                Tunneled
    http://dev.s.dev.webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
    ```

1. Ersetzen Sie das URL-Präfix „dev.s.“ in Ihrem Webbrowser durch „scott.s“. Beachten Sie, dass auch diese aktualisierte URL aufgelöst wird. Diese URL ist eindeutig für den Bereich _dev/scott_. Die spezielle URL bewirkt, dass bei Anforderungen, die an die „Scott-URL“ gesendet werden, zuerst versucht wird, diese an Dienste im Bereich _dev/scott_ weiterzuleiten. Ist dies nicht möglich, erfolgt ein Fallback auf Dienste im Bereich _dev_.

<!--
TODO: replace 2 & 3 with below once bug#753164 and PR#158827 get pushed to production.

You can confirm that your service is running in its own space by running `azds list-up` again. First, you'll notice an instance of *mywebapi* is now running in the _dev/scott_ space (the version running in _dev_ is still running but it is not listed). If you run `azds list-uris`, you will notice that the access point URL for *webfrontend* is prefixed with the text "scott.s.". This URL is unique to the _dev/scott_ space. The special URL signifies that requests sent to the "Scott URL" will try to first route to services in the _dev/scott_ space, but if that fails, they will fall back to services in the _dev_ space.

```
Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

```
Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```
-->

![](../articles/dev-spaces/media/common/space-routing.png)

Mit dieser integrierten Funktion von Azure Dev Spaces können Sie Code in einer freigegebenen Umgebung testen, ohne dass die einzelnen Entwickler den kompletten Dienststapel in ihrem Bereich neu erstellen müssen. Für dieses Routing muss Ihr App-Code Weitergabeheader weiterleiten, wie im vorherigen Schritt dieses Leitfadens erläutert.

### <a name="test-code-in-a-space"></a>Testen von Code in einem Bereich
Wenn Sie Ihre neue Version von *mywebapi* mit *webfrontend* testen möchten, öffnen Sie in Ihrem Browser die URL des öffentlichen Zugriffspunkts für ein *webfrontend*, und navigieren Sie zur Seite „Info“. Ihre neue Meldung sollte angezeigt werden.

Entfernen Sie jetzt den Teil „scott.s.“ aus der URL, und aktualisieren Sie den Browser. Jetzt sollten Sie das frühere Verhalten sehen (mit der in _dev_ ausgeführten *mywebapi*-Version).

Sobald Sie einen _dev_-Bereich haben, der immer die aktuellen Änderungen enthält, und vorausgesetzt, Ihre Anwendung ist zur Nutzung des in diesem Abschnitt des Tutorials beschriebenen bereichsbasierten Routings von Dev Spaces konzipiert, sollte es leicht nachvollziehbar sein, von welch großem Nutzen Dev Spaces beim Testen neuer Features im Kontext der größeren Anwendung sein kann. Anstatt _alle_ Dienste in Ihrem privaten Bereich bereitstellen zu müssen, können Sie einen von _dev_ abgeleiteten privaten Bereich erstellen und darin nur die Dienste bereitstellen, an denen Sie tatsächlich arbeiten. Die Routinginfrastruktur von Dev Spaces übernimmt dann den Rest: Sie nutzt alle Dienste, die Sie in Ihrem privaten Bereich findet, und verwendet standardmäßig wieder die aktuelle Version im Bereich _dev_. Und besser noch: _Mehrere_ Entwickler können zur gleichen Zeit aktiv unterschiedliche Dienste in ihrem eigenen Bereich entwickeln, ohne einander zu stören.

### <a name="well-done"></a>Gut gemacht!
Sie haben den Leitfaden zu den ersten Schritten abgeschlossen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
> * Iteratives Entwickeln von Code in Containern
> * Unabhängiges Entwickeln von zwei separaten Diensten und Verwenden der DNS-Dienstermittlung von Kubernetes, um einen anderen Dienst aufzurufen
> * Produktives Entwickeln und Testen Ihres Codes in einer Teamumgebung
> * Einrichten einer Baseline der Funktionen mithilfe von Dev Spaces, um isolierte Änderungen im Kontext einer größeren Microserviceanwendung mühelos zu testen

Nachdem Sie sich mit Azure Dev Spaces vertraut gemacht haben, können Sie [Ihren Entwicklungsbereich für Teammitglieder freigeben](../articles/dev-spaces/how-to/share-dev-spaces.md) und ihnen zeigen, wie dies die Zusammenarbeit vereinfacht.

## <a name="clean-up"></a>Bereinigen
Um eine Azure Dev Spaces-Instanz in einem Cluster vollständig zu löschen, einschließlich aller Entwicklungsbereiche und der darin ausgeführten Dienste, können Sie den Befehl `az aks remove-dev-spaces` verwenden. Beachten Sie, dass diese Aktion nicht rückgängig gemacht werden kann. Sie können dem Cluster die Unterstützung für Azure Dev Spaces wieder hinzufügen, aber dies entspricht einem kompletten Neubeginn. Ihre alten Dienste und Bereiche werden nicht wiederhergestellt.

Im folgenden Beispiel werden die Azure Dev Spaces-Controller Ihres aktiven Abonnements aufgelistet, und anschließend wird der Azure Dev Spaces-Controller gelöscht, der dem AKS-Cluster „myaks“ in der Ressourcengruppe „myaks-rg“ zugeordnet ist.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```