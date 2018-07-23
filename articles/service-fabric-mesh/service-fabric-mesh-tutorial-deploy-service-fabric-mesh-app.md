---
title: 'Tutorial: Bereitstellen einer Service Fabric Mesh-Anwendung in Service Fabric Mesh | Microsoft Docs'
description: Erfahren Sie, wie eine Azure Service Fabric Mesh-Anwendung veröffentlicht wird, die aus einer ASP.NET Core-Website besteht, die mit einem Back-End-Webdienst kommuniziert.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: f9dea759f6556bc521dda4efbd27176f1e06452b
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126574"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-web-application"></a>Tutorial: Bereitstellen einer Service Fabric Mesh-Anwendung

Dieses Tutorial ist der dritte Teil einer Reihe und zeigt, wie Sie eine Azure Service Fabric Mesh-Anwendung direkt über Visual Studio in Azure veröffentlichen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Veröffentlichen der App in Azure
> * Überprüfen des Status der Anwendungsbereitstellung
> * Anzeigen aller Anwendungen, die zurzeit in Ihrem Abonnement bereitgestellt sind
> * Anzeigen der Anwendungsprotokolle
> * Bereinigen der von der App verwendeten Ressourcen

In dieser Tutorialreihe lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer Service Fabric Mesh-Anwendung ](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Lokales Debuggen der App](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * Veröffentlichen der App in Azure

Sie erfahren, wie Sie eine Azure Service Fabric Mesh-App erstellen, die über ein ASP.NET-Web-Front-End und einen ASP.NET Core Web-API-Back-End-Dienst verfügt. Anschließend debuggen Sie die App auf Ihrem lokalen Entwicklungscluster und veröffentlichen die App in Azure. Wenn Sie fertig sind, verfügen Sie über eine einfache Aufgaben-App, die zeigt, wie ein Aufruf zwischen Diensten in einer Service Fabric Mesh-Webanwendung vorgenommen wird.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Stellen Sie sicher, dass Sie [Ihre Entwicklungsumgebung eingerichtet haben](service-fabric-mesh-howto-setup-developer-environment-sdk.md). Dies umfasst auch das Installieren der Service Fabric-Laufzeit, des SDK und die Installation von Docker sowie Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>Herunterladen der Aufgabenbeispielanwendung

Falls Sie die Aufgabenbeispielanwendung aus [Teil 2 dieser Tutorialreihe](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

Die Anwendung befindet sich im Verzeichnis „`src\todolistapp`“.

## <a name="publish-to-azure"></a>Veröffentlichen in Azure

Um das Service Fabric Mesh-Projekt in Azure zu veröffentlichen, klicken Sie in Visual Studio mit der rechten Maustaste auf **ServiceFabricMeshApp**, und wählen Sie dann **Veröffentlichen...** aus.

Nun wird ein Dialogfeld **Service Fabric-Anwendung veröffentlichen** angezeigt.

![Dialogfeld „Veröffentlichen“ von Service Fabric Mesh in Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Wählen Sie Ihr Azure-Konto und -Abonnement aus. Wählen Sie einen **Standort**aus. In diesem Artikel wird **USA, Osten** verwendet.

Wählen Sie unter **Ressourcengruppe** die Option **\<Neue Ressourcengruppe erstellen** aus. Ein Dialogfeld wird angezeigt, in dem Sie eine neue Ressourcengruppe erstellen. In diesem Artikel wird der Standort **USA, Osten** verwendet, und der Name der Gruppe lautet **sfmeshTutorial1RG** (wenn in Ihrem Unternehmen mehrere Personen das gleiche Abonnement verwenden, wählen Sie einen eindeutigen Gruppennamen aus).  Klicken Sie auf **Erstellen**, um die Ressourcengruppe zu erstellen, und kehren Sie dann zum Dialogfeld „Veröffentlichen“ zurück.

![Dialogfeld „Neue Ressourcengruppe“ von Service Fabric Mesh in Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

Zurück im Dialogfeld **Service Fabric-Anwendung veröffentlichen** wählen Sie unter **Azure Container Registry** die Option **\<Neue Containerregistrierung erstellen** aus. Verwenden Sie im Dialogfeld **Containerregistrierung erstellen** einen eindeutigen Namen für den **Containerregistrierungsnamen**. Geben Sie einen **Standort** an (in diesem Tutorial wird **USA, Osten** verwendet). Wählen Sie in der Dropdownliste die **Ressourcengruppe** aus, die Sie im vorherigen Schritt erstellt haben, z.B. **sfmeshTutorial1RG**. Legen Sie die **SKU** auf **Basic** fest, und klicken Sie dann auf **Erstellen**, um zum Dialogfeld „Veröffentlichen“ zurückzukehren.

![Dialogfeld „Neue Ressourcengruppe“ von Service Fabric Mesh in Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Wenn Sie einen Fehler erhalten, dass kein Ressourcenanbieter für Ihr Abonnement registriert wurde, können Sie diesen registrieren. Untersuchen Sie zunächst, ob der Ressourcenanbieter für Ihr Abonnement verfügbar ist:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Wenn der Containerregistrierungsanbieter (`Microsoft.ContainerRegistry`) verfügbar ist, registrieren Sie ihn aus PowerShell:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

Klicken Sie im Dialogfeld „Veröffentlichen“ auf die Schaltfläche **Veröffentlichen**, um die Service Fabric-Anwendung in Azure bereitzustellen.

Wenn Sie zum ersten Mal in Azure veröffentlichen, wird das Docker-Image in die Azure Container Registry (ACR) gepusht. Dies kann abhängig von der Größe des Images einige Zeit dauern. Nachfolgende Veröffentlichungen des gleichen Projekts erfolgen schneller. Sie können den Status der Bereitstellung überwachen, indem Sie den Bereich **Service Fabric-Tools** im Fenster **Ausgabe** von Visual Studio auswählen. Nach Abschluss die Bereitstellung zeigt die Ausgabe der **Service Fabric-Tools** die IP-Adresse und den Port Ihrer Anwendung in Form einer URL an.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu der URL, um die Ausführung der Website in Azure anzuzeigen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für die verbleibenden Schritte verwenden.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.35 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und zu einem Upgrade auf die neueste Version der CLI finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install].

## <a name="install-the-az-mesh-cli"></a>Installieren der Azure Mesh CLI
In der CLI-Eingabeaufforderung

1) Entfernen Sie alle vorherigen Installationen des Azure Service Fabric Mesh CLI-Moduls.

```cli
az extension remove --name mesh
```

2)  Installieren Sie das Azure Service Fabric Mesh CLI-Erweiterungsmodul. Für die Vorschau wurde die Azure Service Fabric Mesh CLI als Erweiterung der Azure CLI programmiert. In der öffentlichen Vorschauversion wird sie jedoch als Teil der Azure CLI ausgeliefert.

```cli
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

## <a name="check-application-deployment-status"></a>Überprüfen des Status der Anwendungsbereitstellung

An diesem Punkt wurde Ihre Anwendung bereitgestellt. Sie können ihren Status mithilfe des Befehls `app show` überprüfen. 

Der Anwendungsname für die Tutorial-App lautet `ServiceMeshApp`. Erfassen Sie die Details für die Anwendung mit dem folgenden Befehl:

```azurecli-interactive
az mesh app show --resource-group $rg --name ServiceMeshApp
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Anzeigen aller Anwendungen, die zurzeit in Ihrem Abonnement bereitgestellt sind

Sie können den Befehl „app list“ verwenden, um eine Liste der Anwendungen abzurufen, die Sie in Ihrem Abonnement bereitgestellt haben.

```cli
az mesh app list --output table
```

## <a name="see-the-application-logs"></a>Anzeigen der Anwendungsprotokolle

Untersuchen Sie die Protokolle für die bereitgestellte Anwendung:

```azurecli-interactive
az mesh code-package-log get --resource-group $rg --application-name ServiceMeshApp --service-name todoservice --replica-name 0 --code-package-name ServiceMeshApp
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie alle erstellten Ressourcen, wenn Sie sie nicht mehr benötigen. Da Sie eine neue Ressourcengruppe zum Hosten von ACR- und Service Fabric Mesh-Dienstressourcen erstellt haben, können Sie dieser Ressourcengruppe bedenkenlos löschen. Hierbei werden alle zugehörigen Ressourcen gelöscht.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Alternativ können Sie die Ressourcengruppe [über das Portal](../azure-resource-manager/resource-group-portal.md#delete-resource-group-or-resources) löschen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Veröffentlichen der App in Azure
> * Überprüfen des Status der Anwendungsbereitstellung
> * Anzeigen aller Anwendungen, die zurzeit in Ihrem Abonnement bereitgestellt sind
> * Anzeigen der Anwendungsprotokolle
> * Bereinigen der von der App verwendeten Ressourcen

Da Sie die Veröffentlichung einer Service Fabric Mesh-Anwendung in Azure nun abgeschlossen haben, versuchen Sie Folgendes:

* Untersuchen Sie das [Beispiel für eine Abstimmungs-App](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp), um sich mit einem weiteren Beispiel für die Kommunikation zwischen Diensten vertraut zu machen.
* Erfahren Sie mehr über [Service Fabric-Ressourcen](service-fabric-mesh-service-fabric-resources.md).
* Erfahren Sie mehr über die [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest