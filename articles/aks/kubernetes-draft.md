---
title: Verwenden von Draft mit AKS und Azure Container Registry
description: Verwenden von Draft mit AKS und Azure Container Registry
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: bb80d68d7ac37c8930626b5163597dc0e492a0d2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="use-draft-with-azure-kubernetes-service-aks"></a>Verwenden von Draft mit Azure Kubernetes Service (AKS)

Draft ist ein Open-Source-Tool für die Einbindung und Bereitstellung dieser Container in Kubernetes-Clustern. Dadurch können Sie sich auf den Entwicklungszyklus (die „innere Schleife“ der konzentrierten Entwicklung) konzentrieren. Draft wird verwendet, während der Code entwickelt wird, jedoch vor dem Commit an die Versionskontrolle. Mit Draft können Sie für Kubernetes schnell eine Anwendung erneut bereitstellen, wenn sich Änderungen am Code ergeben. Weitere Informationen zu Draft finden Sie in der [Draft-Dokumentation auf GitHub][draft-documentation].

In diesem Dokument erfahren Sie, wie Sie Draft mit einem Kubernetes-Cluster in AKS verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine kubectl-Verbindung mit dem Cluster hergestellt haben. Informationen hierzu finden Sie in der [AKS-Schnellstartanleitung][aks-quickstart].

Außerdem benötigen Sie eine private Docker-Registrierung in Azure Container Registry (ACR). Wie Sie eine ACR-Instanz bereitstellen, erfahren Sie in der [Schnellstartanleitung für Azure Container Registry][acr-quickstart].

Helm muss ebenfalls in Ihrem AKS-Cluster installiert sein. Weitere Informationen zum Installieren von Helm finden Sie unter [Verwenden von Helm mit Azure Kubernetes Service (AKS)][aks-helm].

Schließlich müssen Sie [Docker](https://www.docker.com) installieren.

## <a name="install-draft"></a>Installieren von Draft

Bei der Draft-Befehlszeilenschnittstelle handelt es sich um einen Client, der auf Ihrem Entwicklungssystem ausgeführt wird und die schnelle Bereitstellung von Code in einem Kubernetes-Cluster ermöglicht.

> [!NOTE]
> Wenn Sie Draft vor Version 0.12 installiert haben, müssen Sie Draft zuerst mit `helm delete --purge draft` aus dem Cluster löschen und dann die lokale Konfiguration durch Ausführen von `rm -rf ~/.draft` entfernen. Wenn Sie macOS verwenden, führen Sie `brew upgrade draft` aus.

Wenn Sie die Draft-Befehlszeilenschnittstelle auf einem Mac installieren möchten, verwenden Sie `brew`. Informationen zu weiteren Installationsoptionen finden Sie im [Draft-Installationshandbuch][install-draft].

```console
brew tap azure/draft
brew install draft
```

Initialisieren Sie Draft jetzt mithilfe des Befehls `draft init`.

```console
draft init
```

## <a name="configure-draft"></a>Konfigurieren von Draft

Draft erstellt Containerimages lokal und stellt sie anschließend aus der lokalen Registrierung bereit (im Fall von Minikube), oder Sie müssen die Imageregistrierung angeben, die verwendet werden soll. In diesem Beispiel wird Azure Container Registry (ACR) verwendet. Daher müssen Sie eine Vertrauensbeziehung zwischen dem AKS-Cluster und der ACR-Registrierung herstellen und Draft so konfigurieren, dass der Container per Push in ACR übertragen wird.

### <a name="create-trust-between-aks-cluster-and-acr"></a>Erstellen der Vertrauensstellung zwischen dem AKS-Cluster und ACR

Zum Einrichten der Vertrauensstellung zwischen einem AKS-Cluster und einer ACR-Registrierung ändern Sie den Azure Active Directory-Dienstprinzipal, der für AKS verwendet wird, indem Sie ihm die Rolle „Mitwirkender“ mit dem Umfang des ACR-Repositorys hinzufügen. Führen Sie hierzu die folgenden Befehle aus, und ersetzen Sie dabei _&lt;aks-rg-name&gt;_ und _&lt;aks-cluster-name&gt;_ mit der Ressourcengruppe und dem Namen Ihres AKS-Clusters sowie _&lt;acr-rg-nam&gt;_ und _&lt;acr-repo-name&gt;_ mit der Ressourcengruppe und dem Repositorynamen Ihres ACR-Repositorys, mit dem Sie die Vertrauensstellung erstellen möchten.

```console
export AKS_SP_ID=$(az aks show -g <aks-rg-name> -n <aks-cluster-name> --query "servicePrincipalProfile.clientId" -o tsv)
export ACR_RESOURCE_ID=$(az acr show -g <acr-rg-name> -n <acr-repo-name> --query "id" -o tsv)
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

(Diese Schritte und andere Authentifizierungsmechanismen für den Zugriff auf ACR finden Sie unter [Authentifizieren bei ACR](../container-registry/container-registry-auth-aks.md).)

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Konfigurieren von Draft für Pushübertragungen und Bereitstellungen aus ACR

Da jetzt eine Vertrauensbeziehung zwischen AKS und ACR besteht, aktivieren Sie mit den folgenden Schritten die Verwendung von ACR über Ihren AKS-Cluster.
1. Legen Sie den Draft-Konfigurationswert `registry` fest, indem Sie `draft config set registry <registry name>.azurecr.io` ausführen, wobei _&lt;registry name&lt;_ der Name der ACR-Registrierung ist.
2. Melden Sie sich bei der ACR-Registrierung durch Ausführen von `az acr login -n <registry name>` an.

Da Sie jetzt lokal bei ACR angemeldet sind und eine Vertrauensbeziehung mit AKS und ACR erstellt haben, sind keine Kennwörter oder Geheimnisse für Übertragungen per Push und Pull von ACR in AKS erforderlich. Die Authentifizierung erfolgt auf der Azure Resource Manager-Ebene mithilfe von Azure Active Directory.

## <a name="run-an-application"></a>Ausführen einer Anwendung

Das Draft-Repository enthält mehrere Beispielanwendungen, die für Draft-Demos verwendet werden können. Erstellen Sie eine geklonte Kopie des Repositorys.

```console
git clone https://github.com/Azure/draft
```

Wechseln Sie in das Verzeichnis mit den Java-Beispielen.

```console
cd draft/examples/example-java/
```

Starten Sie den Prozess mithilfe des Befehls `draft create`. Dieser Befehl erstellt die Artefakte, die verwendet werden, um die Anwendung in einem Kubernetes-Cluster auszuführen. Zu diesen Elementen zählen eine Dockerfile-Datei, ein Helm-Diagramm und eine Datei vom Typ `draft.toml` (die Draft-Konfigurationsdatei).

```console
draft create
```

Ausgabe:

```console
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Verwenden Sie den Befehl `draft up`, um die Anwendung in einem Kubernetes-Cluster auszuführen. Dieser Befehl erstellt die Dockerfile-Datei, um ein Containerimage zu erstellen. Das Image wird per Push an ACR übertragen, und schließlich wird das Helm-Diagramm installiert, um die Anwendung in AKS zu starten.

Bei der ersten Ausführung können Push und Pull des Containerimages eine längere Zeit dauern. Sobald die Basisebenen zwischengespeichert sind, verkürzt sich die Zeit deutlich.

```console
draft up
```

Ausgabe:

```console
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (3.0007s)
example-java: Releasing Application: SUCCESS ⚓  (0.9322s)
example-java: Build ID: 01C9NPDYQQH2CZENDMZW7ESJAM
Inspect the logs with `draft logs 01C9NPDYQQH2CZENDMZW7ESJAM`
```

## <a name="test-the-application"></a>Testen der Anwendung

Testen Sie die Anwendung mithilfe des Befehls `draft connect`. Dieser Befehl stellt eine Proxyverbindung mit dem Kubernetes-Pod her und ermöglicht so eine sichere lokale Verbindung. Nach Abschluss des Vorgangs kann auf die Anwendung über die angegebene URL zugegriffen werden.

Manchmal kann es ein paar Minuten dauern, bis das Containerimage heruntergeladen wurde und die Anwendung startet. Sollte beim Zugreifen auf die Anwendung ein Fehler auftreten, versuchen Sie erneut, eine Verbindung herzustellen.

```console
draft connect
```

Ausgabe:

```console
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Testen Sie nun Ihre Anwendung durch Navigieren zu http://localhost:46143 (im vorangegangenen Beispiel kann Ihr Port ein anderer sein). Drücken Sie nach Abschluss des Anwendungstests `Control+C`, um die Proxyverbindung zu trennen.

> [!NOTE]
> Sie können auch den Befehl `draft up --auto-connect` zum Erstellen und Bereitstellen Ihrer Anwendung und zum sofortigen Verbinden mit dem ersten ausgeführten Container verwenden, um den Iterationszyklus noch schneller zu machen.

## <a name="expose-application"></a>Verfügbarmachen der Anwendung

Beim Testen einer Anwendung in Kubernetes empfiehlt es sich unter Umständen, die Anwendung im Internet verfügbar zu machen. Hierzu können Sie einen Kubernetes-Dienst mit einer Art [Lastenausgleich][kubernetes-service-loadbalancer] oder einen [Eingangscontroller][kubernetes-ingress] verwenden. In diesem Dokument wird die Verwendung eines Kubernetes-Diensts beschrieben.


Als Erstes muss das Draft-Paket aktualisiert werden, um anzugeben, dass ein Dienst mit einem `LoadBalancer`-Typ erstellt werden soll. Aktualisieren Sie hierzu den Diensttyp in der Datei `values.yaml`.

```console
vi charts/java/values.yaml
```

Suchen Sie die Eigenschaft `service.type`, und ändern Sie den Wert von `ClusterIP` in `LoadBalancer`.

```yaml
replicaCount: 2
image:
  repository: openjdk
  tag: 8-jdk-alpine
  pullPolicy: IfNotPresent
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
  ```

Führen Sie `draft up` aus, um die Anwendung erneut auszuführen.

```console
draft up
```

Es kann ein paar Minuten dauern, bis der Dienst eine öffentliche IP-Adresse zurückgibt. Verwenden Sie zum Überwachen des Fortschritts den Befehl `kubectl get service` mit einem Überwachungselement.

```console
kubectl get service -w
```

Die *externe IP-Adresse* für den Dienst wird zunächst als `pending` angezeigt.

```
example-java-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Sobald die externe IP-Adresse nicht mehr `pending` ist, sondern eine `IP address` angezeigt wird, verwenden Sie `Control+C`, um die kubectl-Überwachung zu beenden.

```
example-java-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Um die Anwendung anzuzeigen, navigieren Sie zu der externen IP-Adresse.

```console
curl 52.175.224.118
```

Ausgabe:

```
Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Durchlaufen der Anwendung

Nachdem Draft konfiguriert wurde und die Anwendung in Kubernetes ausgeführt wird, können Sie nun den Code durchlaufen. Führen Sie zum Testen von aktualisiertem Code jeweils den Befehl `draft up` aus, um die laufende Anwendung zu aktualisieren.

Aktualisieren Sie in diesem Beispiel die Java-Anwendung „Hello World“.

```console
vi src/main/java/helloworld/Hello.java
```

Aktualisieren Sie den Text „Hello World“.

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
}
```

Führen Sie den Befehl `draft up --auto-connect` aus, um die Anwendung erneut bereitzustellen, sobald ein Pod reagieren kann.

```console
draft up --auto-connect
```

Output

```
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (4.0010s)
example-java: Releasing Application: SUCCESS ⚓  (1.1336s)
example-java: Build ID: 01C9NPMJP6YM985GHKDR2J64KC
Inspect the logs with `draft logs 01C9NPMJP6YM985GHKDR2J64KC`
Connect to java:4567 on localhost:39249
Your connection is still active.
Connect to java:4567 on localhost:39249
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567

```

Zeigen Sie schließlich die Anwendung an, um die Aktualisierungen zu sehen.

```console
curl 52.175.224.118
```

Ausgabe:

```
Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Draft finden Sie in der Draft-Dokumentation auf GitHub.

> [!div class="nextstepaction"]
> [Draft-Dokumentation][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[install-draft]: https://github.com/Azure/draft/blob/master/docs/install.md
[kubernetes-ingress]: ./ingress.md
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[aks-quickstart]: ./kubernetes-walkthrough.md
