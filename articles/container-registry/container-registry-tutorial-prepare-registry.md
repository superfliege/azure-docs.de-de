---
title: "Azure Container Registry-Tutorial – Vorbereiten einer georeplizierten Azure-Containerregistrierung"
description: "Es wird beschrieben, wie Sie eine Azure-Containerregistrierung erstellen, die Georeplikation konfigurieren, ein Docker-Image vorbereiten und es für die Registrierung bereitstellen. Dieses Tutorial ist der erste Teil einer dreiteiligen Reihe."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: acr, azure-container-registry, geo-replication
keywords: Docker, Container, Registrierung, Azure
ms.service: container-registry
ms.devlang: 
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 75408dc88b23b615971a23dc6235c563229d75aa
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="prepare-a-geo-replicated-azure-container-registry"></a>Vorbereiten einer georeplizierten Azure-Containerregistrierung

Eine Azure-Containerregistrierung ist eine private Docker-Registrierung, die in Azure bereitgestellt wird und die Sie netzwerknah zu Ihren Bereitstellungen anordnen können. In diesen drei Tutorialartikeln wird beschrieben, wie Sie die Georeplikation zum Bereitstellen einer ASP.NET Core-Webanwendung, die in einem Linux-Container ausgeführt wird, auf zwei Instanzen vom Typ [Web-Apps für Container](../app-service/containers/index.yml) verwenden. Es wird veranschaulicht, wie das Image von Azure aus dem jeweils nächstgelegenen georeplizierten Repository automatisch auf den einzelnen Web-App-Instanzen bereitgestellt wird.

Dieses Tutorial ist der erste Teil einer dreiteiligen Serie:

> [!div class="checklist"]
> * Erstellen einer georeplizierten Azure-Containerregistrierung
> * Klonen des Anwendungsquellcodes von GitHub
> * Erstellen eines Docker-Containerimage aus der Anwendungsquelle
> * Übertragen des Containerimage per Push an Ihre Registrierung

In nachfolgenden Tutorials stellen Sie den Container aus Ihrer privaten Registrierung für eine Web-App bereit, die in zwei Azure-Regionen ausgeführt wird. Anschließend aktualisieren Sie den Code in der Anwendung und aktualisieren beide Web-App-Instanzen mit einem einzelnen `docker push`-Element für Ihre Registrierung.

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial müssen Sie mindestens Version 2.0.20 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Begriffe wie Container und Containerimages sowie grundlegende Docker-Befehle bekannt sind. Eine Einführung in die Grundlagen der Container finden Sie bei Bedarf unter [Get started with Docker (Erste Schritte mit Docker)]( https://docs.docker.com/get-started/).

Für dieses Tutorial ist eine Docker-Entwicklungsumgebung erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- oder [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-System konfiguriert werden kann.

Azure Cloud Shell umfasst keine Docker-Komponenten, die zum Abschließen der einzelnen Schritte dieses Tutorials erforderlich sind. Aus diesem Grund empfehlen wir Ihnen eine lokale Installation der Azure CLI und der Docker-Entwicklungsumgebung.

> [!IMPORTANT]
> Die Georeplikationsfunktion von Azure Container Registry befindet sich derzeit in der **Vorschauphase**. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Melden Sie sich auf dem [Azure-Portal](http://portal.azure.com)an.

Klicken Sie auf **Neu** > **Container** > **Azure Container Registry**.

![Erstellen einer Containerregistrierung über das Azure-Portal][tut-portal-01]

Konfigurieren Sie Ihre neue Registrierung mit den folgenden Einstellungen:

* **Registrierungsname**: Erstellen Sie einen Registrierungsnamen, der in Azure global eindeutig ist und 5 bis 50 alphanumerische Zeichen enthält.
* **Ressourcengruppe**: **Neu erstellen** > `myResourceGroup`
* **Standort**: `West US`
* **Administratorbenutzer**: `Enable` (für Web-App für Container zum Pullen von Images erforderlich)
* **SKU**: `Premium` (für die Georeplikation erforderlich)

Klicken Sie auf **Erstellen**, um die ACR-Instanz bereitzustellen.

![Erstellen einer Containerregistrierung über das Azure-Portal][tut-portal-02]

Im restlichen Teil dieses Tutorials verwenden wir `<acrName>` als Platzhalter für den von Ihnen gewählten **Registrierungsnamen** des Containers.

> [!TIP]
> Da es sich bei Azure-Containerregistrierungen normalerweise um Ressourcen mit langer Lebensdauer handelt, die übergreifend für mehrere Containerhosts verwendet werden, empfehlen wir Ihnen, Ihre Registrierung in einer eigenen Ressourcengruppe zu erstellen. Wenn Sie georeplizierte Registrierungen und Webhooks konfigurieren, werden diese zusätzlichen Ressourcen in derselben Ressourcengruppe angeordnet.
>

## <a name="configure-geo-replication"></a>Konfigurieren der Georeplikation

Nachdem Sie nun über eine Premium-Registrierung verfügen, können Sie die Georeplikation konfigurieren. Ihre Web-App, die Sie im nächsten Tutorial für die Ausführung in zwei Regionen konfigurieren, kann ihre Containerimages dann per Pullvorgang aus der nächstgelegenen Registrierung beziehen.

Navigieren Sie im Azure-Portal zu Ihrer neuen Containerregistrierung, und wählen Sie unter **DIENSTE** die Option **Replikationen**:

![Replikationen auf der Benutzeroberfläche für Containerregistrierungen im Azure-Portal][tut-portal-03]

Es wird eine Karte mit grünen Sechsecken angezeigt, die für die Azure-Regionen stehen, für die eine Georeplikation möglich ist:

 ![Karte mit Regionen im Azure-Portal][tut-map-01]

Replizieren Sie Ihre Registrierung in der Region „USA, Osten“, indem Sie das grüne Sechseck auswählen, und wählen Sie dann unter **Replikation erstellen** die Option **Erstellen**:

 ![Benutzeroberfläche zum Erstellen von Replikaten im Azure-Portal][tut-portal-04]

Wenn die Replikation abgeschlossen ist, wird im Portal für beide Regionen *Bereit* angezeigt. Verwenden Sie die Schaltfläche **Aktualisieren**, um den Status der Replikation zu aktualisieren. Es kann ca. eine Minute dauern, bis die Replikate erstellt und synchronisiert wurden.

![Benutzeroberfläche für den Replikatstatus im Azure-Portal][tut-portal-05]

## <a name="container-registry-login"></a>Anmeldung bei der Containerregistrierung

Nachdem Sie nun die Georeplikation konfiguriert haben, erstellen Sie ein Containerimage und übertragen es per Pushvorgang in Ihre Registrierung. Sie müssen sich zuerst bei der ACR-Instanz anmelden, damit Sie Images per Push übertragen können. Mit [SKUs vom Typ Basic, Standard und Premium](container-registry-skus.md) können Sie die Authentifizierung mit Ihrer Azure-Identität durchführen.

Verwenden Sie den Befehl [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#az_acr_login), um sich zu authentifizieren und die Anmeldeinformationen für Ihre Registrierung zwischenzuspeichern. Ersetzen Sie `<acrName>` durch den Namen der Registrierung, die Sie in den vorherigen Schritten erstellt haben.

```azurecli
az acr login --name <acrName>
```

Der Befehl gibt `Login Succeeded` zurück, wenn der Vorgang abgeschlossen ist.

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Das Beispiel in diesem Tutorial enthält eine kleine Webanwendung, die mit [ASP.NET Core](http://dot.net) erstellt wurde. Die App stellt eine HTML-Seite bereit, auf der die Region angezeigt wird, aus der das Image von Azure Container Registry bereitgestellt wurde.

![Tutorial-App im Browser][tut-app-01]

Verwenden Sie Git, um das Beispiel in ein lokales Verzeichnis herunterzuladen, und wechseln Sie mit `cd` in das Verzeichnis:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Aktualisieren der Dockerfile

Mit der im Beispiel enthaltenen Dockerfile wird veranschaulicht, wie der Container erstellt wird. Der Ausgangspunkt ist ein offizielles [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore)-Image. Anschließend werden die Anwendungsdateien in den Container kopiert, die Abhängigkeiten werden installiert, die Ausgabe wird mit dem offiziellen Image [aspnetcore-build](https://store.docker.com/community/images/microsoft/aspnetcore-build) kompiliert, und zuletzt wird ein optimiertes aspnetcore-Image erstellt.

Die Dockerfile befindet sich unter `./AcrHelloworld/Dockerfile` auf der geklonten Quelle.

```dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

Die Anwendung im Image *acr-helloworld* versucht, die Region zu ermitteln, aus der der Container bereitgestellt wurde. Hierfür werden Informationen zum Anmeldeserver der Registrierung vom DNS abgefragt. Sie müssen die URL des Anmeldeservers Ihrer Registrierung in der Umgebungsvariablen `DOCKER_REGISTRY` in der Dockerfile angeben.

Rufen Sie zuerst die URL des Anmeldeservers Ihrer Registrierung ab, indem Sie den Befehl `az acr show` verwenden. Ersetzen Sie `<acrName>` durch den Namen der Registrierung, die Sie in den vorherigen Schritten erstellt haben.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Ausgabe:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Aktualisieren Sie als Nächstes die Zeile `DOCKER_REGISTRY` mit der URL des Anmeldeservers Ihrer Registrierung. In diesem Beispiel aktualisieren wir die Zeile gemäß unserem Namen der Beispielregistrierung (*uniqueregistryname*):

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Erstellen des Containerimage

Nachdem Sie die Dockerfile mit der URL Ihrer Registrierung aktualisiert haben, können Sie `docker build` verwenden, um das Containerimage zu erstellen. Führen Sie den folgenden Befehl aus, um das Image zu erstellen und mit der URL Ihrer privaten Registrierung zu kennzeichnen. Ersetzen Sie `<acrName>` wieder durch den Namen Ihrer Registrierung:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Beim Erstellen des Docker-Image werden mehrere Ausgabezeilen angezeigt (hier gekürzt angegeben):

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer
...
Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Verwenden Sie den Befehl `docker images`, um das erstellte Image anzuzeigen:

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                                     TAG                 IMAGE ID            CREATED              SIZE
uniqueregistryname.azurecr.io/acr-helloworld   v1                  c9ca1763cfb1        About a minute ago   285MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Übertragen des Images zu Azure Container Registry mithilfe von Push

Verwenden Sie abschließend den Befehl `docker push`, um das Image *acr-helloworld* per Pushvorgang in Ihre Registrierung zu übertragen. Ersetzen Sie `<acrName>` durch den Namen Ihrer Registrierung.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Da Sie Ihre Registrierung für die Georeplikation konfiguriert haben, wird Ihr Image mit diesem einzelnen `docker push`-Befehl automatisch sowohl in der Region *USA, Westen* als auch in *USA, Osten* repliziert.

Ausgabe:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
9716cfe18412: Pushed
074867a942d5: Pushed
a77666945b96: Pushed
953ff32f2036: Pushed
aa2e77726d3c: Pushed
98b800c91d50: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:c515bcebf249b591b558318e2d0ec21d1320340dbf335730eb32372ff7d34255 size: 1792
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine private, georeplizierte Containerregistrierung und ein Containerimage erstellt und dieses Image anschließend per Pushvorgang in Ihre Registrierung übertragen. Mithilfe der Schritte in diesem Tutorial haben Sie folgende Aktionen durchgeführt:

> [!div class="checklist"]
> * Erstellen einer georeplizierten Azure-Containerregistrierung
> * Klonen des Anwendungsquellcodes von GitHub
> * Erstellen eines Docker-Containerimage aus der Anwendungsquelle
> * Übertragen des Containerimage per Push an Ihre Registrierung

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihren Container auf mehreren Web-App für Container-Instanzen bereitstellen, indem Sie die Georeplikation für die lokale Bereitstellung der Images verwenden.

> [!div class="nextstepaction"]
> [Bereitstellen einer Web-App über Azure Container Registry](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
