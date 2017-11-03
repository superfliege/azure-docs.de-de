---
title: Verwenden von Draft mit AKS und Azure Container Registry | Microsoft-Dokumentation
description: Verwenden von Draft mit AKS und Azure Container Registry
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: draft, helm, aks, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3e607a6ce5662f6ff597fafbcec8c864f25ff54c
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="use-draft-with-azure-container-service-aks"></a>Verwenden von Draft mit Azure Container Service (AKS)

Draft ist ein praktisches Open-Source-Tool zum Packen und Ausführen von Code in einem Kubernetes-Cluster. Draft wurde für den Entwicklungsiterationszyklus konzipiert (also wenn sich der Code in der Entwicklung befindet und noch nicht an die Versionskontrolle übergeben wurde). Mit Draft können Sie für Kubernetes schnell eine Anwendung erneut bereitstellen, wenn sich Änderungen am Code ergeben. Weitere Informationen zu Draft finden Sie in der [Draft-Dokumentation auf GitHub](https://github.com/Azure/draft/tree/master/docs).

In diesem Dokument erfahren Sie, wie Sie Draft mit einem Kubernetes-Cluster in AKS verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine kubectl-Verbindung mit dem Cluster hergestellt haben. Informationen hierzu finden Sie in der [AKS-Schnellstartanleitung](./kubernetes-walkthrough.md).

Außerdem benötigen Sie eine private Docker-Registrierung in Azure Container Registry (ACR). Wie Sie eine ACR-Instanz bereitstellen, erfahren Sie in der [Schnellstartanleitung für Azure Container Registry](../container-registry/container-registry-get-started-azure-cli.md).

## <a name="install-helm"></a>Installieren von Helm

Bei der Helm-Befehlszeilenschnittstelle handelt es sich um einen Client, der auf Ihrem Entwicklungssystem ausgeführt wird und es Ihnen ermöglicht, Anwendungen zu starten, zu beenden und mit Helm-Diagrammen zu verwalten.

Wenn Sie die Helm-Befehlszeilenschnittstelle auf einem Mac installieren möchten, verwenden Sie `brew`. Informationen zu weiteren Installationsoptionen finden Sie unter [Installing Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) (Installieren von Helm).

```console
brew install kubernetes-helm
```

Ausgabe:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="install-draft"></a>Installieren von Draft

Bei der Draft-Befehlszeilenschnittstelle handelt es sich um einen Client, der auf Ihrem Entwicklungssystem ausgeführt wird und die schnelle Bereitstellung von Code in einem Kubernetes-Cluster ermöglicht.

Wenn Sie die Draft-Befehlszeilenschnittstelle auf einem Mac installieren möchten, verwenden Sie `brew`. Informationen zu weiteren Installationsoptionen finden Sie im [Draft-Installationshandbuch](https://github.com/Azure/draft/blob/master/docs/install.md).

```console
brew install draft
```

Ausgabe:

```
==> Installing draft from azure/draft
==> Downloading https://azuredraft.blob.core.windows.net/draft/draft-v0.7.0-darwin-amd64.tar.gz
Already downloaded: /Users/neilpeterson/Library/Caches/Homebrew/draft-0.7.0.tar.gz
==> /usr/local/Cellar/draft/0.7.0/bin/draft init --client-only
🍺  /usr/local/Cellar/draft/0.7.0: 6 files, 61.2MB, built in 1 second
```

## <a name="configure-draft"></a>Konfigurieren von Draft

Im Zuge der Konfiguration von Draft muss eine Containerregistrierung angegeben werden. In diesem Beispiel wird Azure Container Registry verwendet.

Führen Sie den folgenden Befehl aus, um den Namen und den Anmeldeservernamen Ihrer ACR-Instanz abzurufen. Aktualisieren Sie den Befehl mit dem Namen der Ressourcengruppe, die Ihre ACR-Instanz enthält.

```console
az acr list --resource-group <resource group> --query "[].{Name:name,LoginServer:loginServer}" --output table
```

Halten Sie außerdem das Kennwort für die ACR-Instanz bereit.

Führen Sie den folgenden Befehl aus, um das ACR-Kennwort zurückzugeben. Aktualisieren Sie den Befehl mit dem Namen der ACR-Instanz.

```console
az acr credential show --name <acr name> --query "passwords[0].value" --output table
```

Initialisieren Sie Draft mithilfe des Befehls `draft init`.

```console
draft init
```

Im Rahmen dieses Vorgangs werden Sie zur Eingabe der Anmeldeinformationen für die Containerregistrierung aufgefordert. Bei Verwendung einer Azure Container Registry-Instanz ist die Registrierungs-URL der ACR-Anmeldeservername, der Benutzername ist der ACR-Instanzname, und das Kennwort ist das ACR-Kennwort.

```console
1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): <ACR Login Server>
2. Enter your username: <ACR Name>
3. Enter your password: <ACR Password>
```

Nach Abschluss des Vorgangs ist Draft im Kubernetes-Cluster konfiguriert und verwendungsbereit.

```
Draft has been installed into your Kubernetes Cluster.
Happy Sailing!
```

## <a name="run-an-application"></a>Ausführen einer Anwendung

Das Draft-Repository enthält mehrere Beispielanwendungen, die für Draft-Demos verwendet werden können. Erstellen Sie eine geklonte Kopie des Repositorys.

```console
git clone https://github.com/Azure/draft
```

Wechseln Sie in das Verzeichnis mit den Java-Beispielen.

```console
cd draft/examples/java/
```

Starten Sie den Prozess mithilfe des Befehls `draft create`. Dieser Befehl erstellt die Artefakte, die verwendet werden, um die Anwendung in einem Kubernetes-Cluster auszuführen. Zu diesen Elementen zählen eine Dockerfile-Datei, ein Helm-Diagramm und eine Datei vom Typ `draft.toml` (die Draft-Konfigurationsdatei).

```console
draft create
```

Ausgabe:

```
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Verwenden Sie den Befehl `draft up`, um die Anwendung in einem Kubernetes-Cluster auszuführen. Dieser Befehl lädt den Anwendungscode und die Konfigurationsdateien in den Kubernetes-Cluster hoch. Anschließend wird die Dockerfile-Datei ausgeführt, um ein Containerimage zu erstellen, das Image wird mithilfe von Push an die Containerregistrierung übertragen, und schließlich wird das Helm-Diagramm ausgeführt, um die Anwendung zu starten.

```console
draft up
```

Ausgabe:

```
Draft Up Started: 'open-jaguar'
open-jaguar: Building Docker Image: SUCCESS ⚓  (28.0342s)
open-jaguar: Pushing Docker Image: SUCCESS ⚓  (7.0647s)
open-jaguar: Releasing Application: SUCCESS ⚓  (4.5056s)
open-jaguar: Build ID: 01BW3VVNZYQ5NQ8V1QSDGNVD0S
```

## <a name="test-the-application"></a>Testen der Anwendung

Testen Sie die Anwendung mithilfe des Befehls `draft connect`. Dieser Befehl stellt eine Proxyverbindung mit dem Kubernetes-Pod her und ermöglicht so eine sichere lokale Verbindung. Nach Abschluss des Vorgangs kann auf die Anwendung über die angegebene URL zugegriffen werden.

Manchmal kann es ein paar Minuten dauern, bis das Containerimage heruntergeladen wurde und die Anwendung startet. Sollte beim Zugreifen auf die Anwendung ein Fehler auftreten, versuchen Sie erneut, eine Verbindung herzustellen.

```console
draft connect
```

Ausgabe:

```
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Drücken Sie nach Abschluss des Anwendungstests `Control+C`, um die Proxyverbindung zu trennen.

## <a name="expose-application"></a>Verfügbarmachen der Anwendung

Beim Testen einer Anwendung in Kubernetes empfiehlt es sich unter Umständen, die Anwendung im Internet verfügbar zu machen. Hierzu können Sie einen Kubernetes-Dienst mit einer Art [Lastenausgleich](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer) oder einen [Eingangscontroller](https://kubernetes.io/docs/concepts/services-networking/ingress/) verwenden. In diesem Dokument wird die Verwendung eines Kubernetes-Diensts beschrieben.


Als Erstes muss das Draft-Paket aktualisiert werden, um anzugeben, dass ein Dienst mit einem `LoadBalancer`-Typ erstellt werden soll. Aktualisieren Sie hierzu den Diensttyp in der Datei `values.yaml`.

```console
vi chart/java/values.yaml
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
deadly-squid-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Sobald die externe IP-Adresse nicht mehr `pending` ist, sondern eine `IP address` angezeigt wird, verwenden Sie `Control+C`, um die kubectl-Überwachung zu beenden.

```
deadly-squid-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
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
        get("/", (req, res) -> "Hello World, I'm Java - Draft Rocks!");
    }
}
```

Führen Sie den Befehl `draft up` aus, um die Anwendung erneut bereitzustellen.

```console
draft up
```

Ausgabe

```
Draft Up Started: 'deadly-squid'
deadly-squid: Building Docker Image: SUCCESS ⚓  (18.0813s)
deadly-squid: Pushing Docker Image: SUCCESS ⚓  (7.9394s)
deadly-squid: Releasing Application: SUCCESS ⚓  (6.5005s)
deadly-squid: Build ID: 01BWK8C8X922F5C0HCQ8FT12RR
```

Zeigen Sie schließlich die Anwendung an, um die Aktualisierungen zu sehen.

```console
curl 52.175.224.118
```

Ausgabe:

```
Hello World, I'm Java - Draft Rocks!
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Draft finden Sie in der Draft-Dokumentation auf GitHub.

> [!div class="nextstepaction"]
> [Draft-Dokumentation](https://github.com/Azure/draft/tree/master/docs)