---
title: "Erstellen von Containerimages für Azure Service Fabric | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Containerimages für eine Service Fabric-Anwendung mit mehreren Containern erstellen."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, Container, Microservices, Service Fabric, Azure
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: suhuruli
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e84a0a05cbe1952d8325a9aed1c1df411bfcb877
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="create-container-images-for-service-fabric"></a>Erstellen von Containerimages für Service Fabric

Dieses Tutorial ist der erste Teil einer Tutorialreihe, in der die Verwendung von Containern in einem Linux-basierten Service Fabric-Cluster veranschaulicht wird. In diesem Tutorial wird eine Anwendung mit mehreren Containern für die Verwendung mit Service Fabric vorbereitet. In den nachfolgenden Tutorials werden diese Images als Teil einer Service Fabric-Anwendung verwendet. In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub  
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Bereitstellen einer Azure Container Registry-Instanz (ACR)
> * Kennzeichnen eines Containerimages für ACR
> * Hochladen des Images in die ACR-Instanz

In dieser Tutorialreihe lernen Sie Folgendes: 

> [!div class="checklist"]
> * Erstellen von Containerimages für Service Fabric
> * [Erstellen und Ausführen einer Service Fabric-Anwendung mit Containern](service-fabric-tutorial-package-containers.md)
> * [Ausführen des Failovers und der Skalierung in Service Fabric](service-fabric-tutorial-containers-failover.md)

## <a name="prerequisites"></a>Voraussetzungen

- Einrichtung der Linux-Entwicklungsumgebung für Service Fabric. Befolgen Sie die [hier](service-fabric-get-started-linux.md) aufgeführten Anweisungen, um die Linux-Umgebung einzurichten. 
- Für dieses Tutorial müssen Sie mindestens Version 2.0.4 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 
- Außerdem ist es erforderlich, dass Sie über ein Azure-Abonnement verfügen. Weitere Informationen zu einer kostenlosen Testversion finden Sie [hier](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Die in diesem Tutorial verwendete Beispielanwendung ist eine Abstimmungs-App. Die Anwendung besteht aus einer Front-End-Webkomponente und einer Back-End-Redis-Instanz. Die Komponenten werden in Containerimages gepackt. 

Verwenden Sie Git, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen.

```bash
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

```

Geben Sie im geklonten Verzeichnis mit dem folgenden Befehl das Arbeitsverzeichnis ein. 

```bash 
cd Linux/container-tutorial/
```

Das Verzeichnis „container-tutorial“ enthält einen Ordner mit dem Namen „azure-vote“. Dieser Ordner „azure-vote“ enthält den Front-End-Quellcode und eine Dockerfile-Datei zum Erstellen des Front-Ends. Das Verzeichnis „container-tutorial“ enthält auch das Verzeichnis „redis“, das wiederum die Dockerfile-Datei zum Erstellen des Redis-Images enthält. Diese Verzeichnisse enthalten die erforderlichen Ressourcen für diese Tutorialreihe. 

## <a name="create-container-images"></a>Erstellen von Containerimages

Führen Sie im Verzeichnis „azure-vote“ den folgenden Befehl aus, um das Image für die Front-End-Webkomponente zu erstellen. Dieser Befehl verwendet die Dockerfile-Datei in diesem Verzeichnis, um das Image zu erstellen. 

```bash
docker build -t azure-vote-front .
```

Führen Sie im Verzeichnis „redis“ den folgenden Befehl aus, um das Image für das Redis-Back-End zu erstellen. Dieser Befehl verwendet die Dockerfile-Datei in diesem Verzeichnis, um das Image zu erstellen. 

```bash
docker build -t azure-vote-back .
```

Verwenden Sie anschließend den Befehl [docker-images](https://docs.docker.com/engine/reference/commandline/images/), um die erstellten Images anzuzeigen.

```bash
docker images
```

Beachten Sie, dass vier Images heruntergeladen oder erstellt wurden. Das Image *azure-vote-front* enthält die Anwendung. Es wurde von einem *Python*-Image von Docker Hub abgeleitet. Das Redis-Image wurde aus Docker Hub heruntergeladen.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Bereitstellen von Azure Container Registry

Führen Sie zunächst den Befehl [az login](/cli/azure/login) aus, um sich bei Ihrem Azure-Konto anzumelden. 

Verwenden Sie dann den Befehl [az account](/cli/azure/account#set), um Ihr Abonnement für die Erstellung der Azure Container Registry-Instanz auszuwählen. 

```bash
az account set --subscription <subscription_id>
```

Für die Bereitstellung einer Azure Container Registry-Instanz benötigen Sie zunächst eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *westus* erstellt. Wählen Sie die Ressourcengruppe in einer Region in Ihrer Nähe aus. 

```bash
az group create --name myResourceGroup --location westus
```

Erstellen Sie mit dem Befehl [az acr create](/cli/azure/acr#create) eine Azure Container Registry-Instanz. Der Name einer Containerregistrierung **muss eindeutig sein**.

```bash
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

Für den Rest dieses Tutorials verwenden wir „acrname“ als Platzhalter für den von Ihnen gewählten Namen der Containerregistrierung.

## <a name="log-in-to-your-container-registry"></a>Anmelden bei Ihrer Containerregistrierung

Melden Sie sich zunächst bei Ihrer ACR-Instanz an, bevor Sie Images in diese pushen. Verwenden Sie den Befehl [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login), um den Vorgang abzuschließen. Geben Sie den eindeutigen Namen an, den die Containerregistrierung bei ihrer Erstellung erhalten hat.

```bash
az acr login --name <acrName>
```

Nach Abschluss des Vorgangs wird eine Erfolgsmeldung zurückgegeben.

## <a name="tag-container-images"></a>Kennzeichnen von Containerimages

Jedes Containerimage muss mit dem loginServer-Namen der Registrierung gekennzeichnet werden. Dieses Tag wird beim Übertragen von Containerimages per Push in eine Imageregistrierung für das Routing verwendet.

Verwenden Sie den Befehl [docker images](https://docs.docker.com/engine/reference/commandline/images/), um eine Liste der aktuellen Images anzuzeigen.

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

Führen Sie den folgenden Befehl aus, um den loginServer-Namen abzurufen:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Kennzeichnen Sie nun das Image *azure-vote-front* mit dem „loginServer“-Namen der Containerregistrierung. Fügen Sie zudem `:v1` am Ende des Imagenamens hinzu. Dieses Tag gibt die Imageversion an.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Kennzeichnen Sie dann das Image *azure-vote-back* mit dem loginServer-Namen der Containerregistrierung. Fügen Sie zudem `:v1` am Ende des Imagenamens hinzu. Dieses Tag gibt die Imageversion an.

```bash
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
```

Führen Sie nach der Kennzeichnung den Befehl „docker images“ aus, um den Vorgang zu überprüfen.


Ausgabe:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-back                        latest              bf9a858a9269        22 minutes ago      107MB
<acrName>.azurecr.io/azure-vote-back    v1                  bf9a858a9269        22 minutes ago      107MB
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf        23 minutes ago      708MB
redis                                  latest              9813a7e8fcc0        2 days ago          107MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Übertragen von Images in die Registrierung per Push

Übertragen Sie das Image *azure-vote-front* per Push in die Registrierung. 

Verwenden Sie das folgende Beispiel, und ersetzen Sie den ACR-loginServer-Namen durch den loginServer-Namen aus Ihrer Umgebung.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Pushen Sie das Image *azure-vote-back* in die Registrierung. 

Verwenden Sie das folgende Beispiel, und ersetzen Sie den ACR-loginServer-Namen durch den loginServer-Namen aus Ihrer Umgebung.

```bash
docker push <acrLoginServer>/azure-vote-back:v1
```

Die Ausführung der Docker-Push-Befehle nimmt einige Minuten in Anspruch.

## <a name="list-images-in-registry"></a>Auflisten von Images in der Registrierung

Führen Sie den Befehl [az acr repository list](/cli/azure/acr/repository#list) aus, um eine Liste der Images zurückzugeben, die per Push in Ihre Azure Container Registry-Instanz übertragen wurden. Aktualisieren Sie den Befehl mit dem Namen der ACR-Instanz.

```bash
az acr repository list --name <acrName> --output table
```

Ausgabe:

```bash
Result
----------------
azure-vote-back
azure-vote-front
```

Nach Abschluss des Tutorials ist das Containerimage in einer privaten Azure Container Registry-Instanz gespeichert. Dieses Image wird in den nachfolgenden Tutorials aus ACR in einem Service Fabric-Cluster bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden eine Anwendung aus GitHub gepullt sowie Containerimages erstellt und in eine Registrierung gepusht. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub  
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Bereitstellen einer Azure Container Registry-Instanz (ACR)
> * Kennzeichnen eines Containerimages für ACR
> * Hochladen des Images in die ACR-Instanz

Im nächsten Tutorial erfahren Sie, wie Sie Container mithilfe von Yeoman in eine Service Fabric-Anwendung packen. 

> [!div class="nextstepaction"]
> [Packen und Bereitstellen von Containern als Service Fabric-Anwendung](service-fabric-tutorial-package-containers.md)

