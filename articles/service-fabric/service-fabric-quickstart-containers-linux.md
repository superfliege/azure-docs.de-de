---
title: Erstellen einer Azure Service Fabric-Containeranwendung unter Linux | Microsoft-Dokumentation
description: "Erstellen Sie Ihre erste Linux-Containeranwendung unter Azure Service Fabric.  Erstellen Sie ein Docker-Image mit Ihrer Anwendung, übertragen Sie es per Push an eine Containerregistrierung, erstellen Sie eine Service Fabric-Containeranwendung, und stellen Sie diese bereit."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: d07d5d59632791a52bcb3a2f54bebe194cc76a54
ms.openlocfilehash: 44eaaae123490934bc62b4ea30968656900d48fc
ms.contentlocale: de-de
ms.lasthandoff: 10/04/2017

---

# <a name="deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Bereitstellen einer Azure Service Fabric-Containeranwendung unter Linux in Azure
Azure Service Fabric ist eine Plattform für verteilte Systeme zum Bereitstellen und Verwalten von skalierbaren und zuverlässigen Microservices und Containern. 

In diesem Schnellstart wird veranschaulicht, wie Sie Linux-Container in einem Service Fabric-Cluster bereitstellen. Nach Abschluss des Vorgangs verfügen Sie über eine Abstimmungsanwendung bestehend aus einem Python-Web-Front-End und einem Redis-Back-End, die in einem Service Fabric-Cluster ausgeführt werden. 

![quickstartpic][quickstartpic]

In dieser Schnellstartanleitung wird Folgendes vermittelt:
> [!div class="checklist"]
> * Bereitstellen von Linux-Containern in Service Fabric
> * Skalieren und Durchführen des Failovers für Container in Service Fabric

## <a name="prerequisite"></a>Voraussetzung
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/en-us/free/) erstellen, bevor Sie beginnen.
  
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle (CLI) lokal installieren und nutzen möchten, sollten Sie sicherstellen, dass Sie Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie „az --version“ aus, um nach der Version zu suchen. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) Informationen dazu.

## <a name="get-application-package"></a>Abrufen des Anwendungspakets
Zum Bereitstellen von Containern in Service Fabric benötigen Sie eine Gruppe von Manifestdateien (die Anwendungsdefinition), in denen die einzelnen Container und die Anwendung beschrieben ist.

Verwenden Sie „git“ in der Cloud Shell, um eine Kopie der Anwendungsdefinition zu klonen.

```azurecli-interactive
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/Voting
```

## <a name="deploy-the-containers-to-a-service-fabric-cluster-in-azure"></a>Bereitstellen der Container in einem Service Fabric-Cluster in Azure
Für die Anwendungsbereitstellung in einem Cluster in Azure können Sie einen eigenen Cluster oder einen Partycluster verwenden.

Bei Partyclustern handelt es sich um zeitlich begrenzte kostenlose Service Fabric-Cluster, die in Azure gehostet werden. Sie werden vom Service Fabric-Team verwaltet, und alle Interessenten können Anwendungen bereitstellen und sich mit der Plattform vertraut machen. [Befolgen Sie die Anweisungen](http://aka.ms/tryservicefabric), um Zugriff auf einen Partycluster zu erhalten. 

Informationen zum Erstellen Ihres eigenen Clusters finden Sie unter [Erstellen Ihres ersten Service Fabric-Clusters in Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Der Web-Front-End-Dienst ist für das Lauschen auf eingehenden Datenverkehr über Port 80 konfiguriert. Stellen Sie sicher, dass der Port in Ihrem Cluster geöffnet ist. Wenn Sie einen Partycluster verwenden, ist dieser Port geöffnet.
>

### <a name="deploy-the-application-manifests"></a>Bereitstellen der Anwendungsmanifeste 
Installieren der [Service Fabric-Befehlszeilenschnittstelle (sfctl)](service-fabric-cli.md) in Ihrer CLI-Umgebung

```azurecli-interactive
pip3 install --user sfctl 
export PATH=$PATH:~/.local/bin
```
Stellen Sie eine Verbindung mit dem Service Fabric-Cluster in Azure her, indem Sie die Azure CLI verwenden. Der Endpunkt ist der Verwaltungsendpunkt Ihres Clusters, z.B. `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.

```azurecli-interactive
sfctl cluster select --endpoint http://linh1x87d1d.westus.cloudapp.azure.com:19080
```

Verwenden Sie das bereitgestellte Installationsskript, um die Definition der Abstimmungsanwendung in den Cluster zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

```azurecli-interactive
./install.sh
```

Öffnen Sie einen Browser, und navigieren Sie zum Service Fabric Explorer unter http://\<URL meines Azure Service Fabric-Clusters>:19080/Explorer, z.B. `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. Erweitern Sie den Knoten „Anwendungen“, um zu prüfen, ob nun ein Eintrag für den Abstimmungsanwendungstyp und die von Ihnen erstellte Instanz vorhanden ist.

![Service Fabric Explorer][sfx]

Stellen Sie eine Verbindung mit dem ausgeführten Container her.  Öffnen Sie einen Webbrowser mit der URL Ihres Clusters, z.B. `http://linh1x87d1d.westus.cloudapp.azure.com:80`. Die Abstimmungsanwendung sollte im Browser angezeigt werden.

![quickstartpic][quickstartpic]

## <a name="fail-over-a-container-in-a-cluster"></a>Durchführen eines Failovers für einen Container in einem Cluster
Mit Service Fabric wird sichergestellt, dass Ihre Containerinstanzen automatisch auf andere Knoten im Cluster verschoben werden, falls es zu einem Ausfall kommt. Sie können die Container eines Knotens auch manuell entfernen und diese geregelt auf andere Knoten im Cluster verschieben. Sie haben mehrere Möglichkeiten, um Ihre Dienste zu skalieren. In diesem Beispiel verwenden wir Service Fabric Explorer.

Führen Sie die folgenden Schritte aus, um für den Front-End-Container ein Failover durchzuführen:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/Voting/azurevotefront**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt). Beachten Sie den Knotennamen in der Strukturansicht. Hier werden die Knoten angezeigt, auf denen der Container derzeit ausgeführt wird, z.B. `_nodetype_4`.
3. Erweitern Sie in der Strukturansicht den Knoten **Knoten**. Klicken Sie auf die Auslassungspunkte (drei Punkte) neben dem Knoten, auf dem der Container ausgeführt wird.
4. Wählen Sie **Neu starten**, um den Knoten neu zu starten, und bestätigen Sie die Neustartaktion. Der Neustart bewirkt, dass für den Container ein Failover auf einen anderen Knoten im Cluster durchgeführt wird.

![sfxquickstartshownodetype][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Skalieren von Anwendungen und Diensten in einem Cluster
Service Fabric-Dienste können für einen Cluster auf einfache Weise skaliert werden, um die Last für die Dienste zu bewältigen. Sie skalieren einen Dienst, indem Sie die Anzahl von Instanzen ändern, die im Cluster ausgeführt werden.

Führen Sie die folgenden Schritte aus, um den Web-Front-End-Dienst zu skalieren:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klicken Sie in der Strukturansicht auf das Auslassungszeichen (drei Punkte) neben dem Knoten **fabric:/Voting/azurevotefront**, und wählen Sie **Scale Service** (Dienst skalieren).

    ![containersquickstartscale][containersquickstartscale]

  Sie können jetzt angeben, dass die Anzahl von Instanzen des Web-Front-End-Diensts skaliert werden soll.

3. Ändern Sie die Anzahl in **2**, und klicken Sie auf **Scale Service** (Dienst skalieren).
4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/Voting/azurevotefront**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt).

    ![containersquickstartscaledone][containersquickstartscaledone]

    Sie sehen jetzt, dass der Dienst über zwei Instanzen verfügt. In der Strukturansicht wird angezeigt, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben wir die Ressourcen verdoppelt, die für unseren Front-End-Dienst zum Verarbeiten der Benutzerauslastung verfügbar sind. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Wenn ein Dienst ausfällt, wird von Service Fabric sichergestellt, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="clean-up"></a>Bereinigen
Verwenden Sie das Deinstallationsskript aus der Vorlage, um die Anwendungsinstanz aus dem Cluster zu löschen und die Registrierung des Anwendungstyps aufzuheben. Bei diesem Befehl dauert das Bereinigen der Instanz einige Zeit, und der Befehl 'install'sh' sollte nicht direkt nach diesem Skript ausgeführt werden. 

```bash
./uninstall.sh
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Bereitstellen einer Linux-Containeranwendung in Azure
> * Durchführen eines Failovers für einen Container in einem Service Fabric-Cluster
> * Skalieren eines Containers in einem Service Fabric-Cluster

* Weitere Informationen zum Ausführen von [Containern in Service Fabric](service-fabric-containers-overview.md)
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric
* Sehen Sie sich die [Codebeispiele zu Service Fabric-Containern](https://github.com/Azure-Samples/service-fabric-dotnet-containers) auf GitHub an.

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png

