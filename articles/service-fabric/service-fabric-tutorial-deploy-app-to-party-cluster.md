---
title: Bereitstellen einer Service Fabric-App in einem Cluster in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie über Visual Studio eine Anwendung in einem Cluster bereitstellen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: 601742d82c1bd9a0e691de28ff9c4a09f12b538e
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186378"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Tutorial: Bereitstellen einer Service Fabric-Anwendung in einem Cluster in Azure

Dieses Tutorial ist der zweite Teil einer Reihe und zeigt, wie Sie eine Azure Service Fabric-Anwendung für einen neuen Cluster in Azure bereitstellen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Partyclusters
> * Bereitstellen einer Anwendung in einem Remotecluster mithilfe von Visual Studio

In dieser Tutorialreihe lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * Bereitstellen der Anwendung in einem Remotecluster
> * [Hinzufügen eines HTTPS-Endpunkts zu einem ASP.NET Core-Front-End-Dienst](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Konfigurieren von CI/CD mit Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.
* [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
* [Installieren Sie das Service Fabric SDK.](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Herunterladen der Beispielanwendung „Voting“

Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="publish-to-a-service-fabric-cluster"></a>Veröffentlichen in einem Service Fabric-Cluster

Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio in einem Cluster bereitstellen. Ein [Service Fabric-Cluster](https://docs.microsoft.com/en-gb/azure/service-fabric/service-fabric-deploy-anywhere) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden.

In diesem Tutorial stehen Ihnen zwei Optionen zur Verfügung, um die Voting-Anwendung mithilfe von Visual Studio in einem Service Fabric-Cluster bereitzustellen:

* Verwenden Sie einen Testpartycluster als Veröffentlichungsziel.
* Verwenden Sie einen bereits vorhandenen Cluster aus Ihrem Abonnement als Veröffentlichungsziel.  Sie können Service Fabric-Cluster über das [Azure-Portal](https://portal.azure.com), über [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md), mithilfe von [Azure CLI](./scripts/cli-create-cluster.md)-Skripts oder mithilfe einer [Azure Resource Manager-Vorlage](service-fabric-tutorial-create-vnet-and-windows-cluster.md) bereitstellen.

> [!NOTE]
> Viele Dienste verwenden den Reverseproxy, um miteinander zu kommunizieren. Bei Clustern, die über Visual Studio erstellt werden, sowie bei Partyclustern ist der Reverseproxy standardmäßig aktiviert.  Bei Verwendung eines bereits vorhandenen Clusters müssen Sie den [Reverseproxy im Cluster aktivieren](service-fabric-reverseproxy.md#setup-and-configuration).


### <a name="find-the-votingweb-service-endpoint-for-your-azure-subscription"></a>Suchen des VotingWeb-Dienstendpunkts für Ihr Azure-Abonnement

Wenn Sie die Voting-Anwendung in Ihrem eigenen Azure-Abonnement veröffentlichen möchten, suchen Sie nach dem Endpunkt des Front-End-Webdiensts. Bei Verwendung eines Partyclusters ist der vom Voting-Beispiel verwendete Port 8080 automatisch geöffnet und muss nicht im Lastenausgleich des Partyclusters konfiguriert werden.

Der Front-End-Webdienst lauscht an einem bestimmten Port.  Bei der Bereitstellung eines Clusters in Azure durch die Anwendung werden sowohl der Cluster als auch die Anwendung hinter einem Azure-Lastenausgleichsmodul ausgeführt.  Der Anwendungsport muss mithilfe einer Regel im Azure Load Balancer für diesen Cluster geöffnet werden, damit eingehender Datenverkehr den Webdienst erreichen kann.  Den Port (z.B. 8080) finden Sie in der Datei *VotingWeb/PackageRoot/ServiceManifest.xml* im **Endpoint**-Element:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Öffnen Sie diesen Port für Ihr Azure-Abonnement mithilfe einer Lastenausgleichsregel in Azure – entweder über ein [PowerShell-Skript](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) oder über den Lastenausgleich für diesen Cluster im [Azure-Portal](https://portal.azure.com).

### <a name="join-a-party-cluster"></a>Beitreten zu einem Partycluster

> [!NOTE]
> Falls Sie die Anwendung in Ihrem eigenen Cluster innerhalb eines Azure-Abonnements veröffentlichen möchten, fahren Sie mit „Veröffentlichen der Anwendung mit Visual Studio“ im nächsten Abschnitt fort.

Partycluster sind kostenlose, zeitlich begrenzte Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team ausgeführt werden, in denen jeder Benutzer Anwendungen bereitstellen und mehr über die Plattform erfahren kann. Der Cluster verwendet ein einzelnes selbstsigniertes Zertifikat für Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit.

Melden Sie sich an, und [treten Sie einem Windows-Cluster bei](http://aka.ms/tryservicefabric). Klicken Sie auf den Link **PFX**, um das PFX-Zertifikat auf Ihren Computer herunterzuladen. Klicken Sie auf den Link **How to connect to a secure Party cluster?** (Herstellen einer Verbindung mit einem sicheren Partycluster), und kopieren Sie das Zertifikatkennwort. Das Zertifikat, das Zertifikatkennwort und der Wert für **Verbindungsendpunkt** werden in den folgenden Schritten verwendet.

![PFX-Zertifikat und Verbindungsendpunkt](./media/service-fabric-quickstart-dotnet/party-cluster-cert.png)

> [!Note]
> Pro Stunde ist eine begrenzte Anzahl von Partyclustern verfügbar. Sollte bei der Registrierung für einen Partycluster ein Fehler auftreten, können Sie eine Weile warten und es dann erneut versuchen. Alternativ können Sie die Schritte aus dem [Tutorial zum Bereitstellen einer .NET-App](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) ausführen, um in Ihrem Azure-Abonnement einen Service Fabric-Cluster zu erstellen und die Anwendung darin bereitzustellen. Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.
>

Installieren Sie das PFX-Zertifikat auf Ihrem Windows-Computer im Zertifikatspeicher *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Notieren Sie sich den Fingerabdruck für einen späteren Schritt.

> [!Note]
> Der Web-Front-End-Dienst ist standardmäßig für das Lauschen auf eingehenden Datenverkehr an Port 8080 konfiguriert. Port 8080 ist im Partycluster geöffnet.  Wenn Sie den Anwendungsport ändern müssen, verwenden Sie einen der im Partycluster geöffneten Ports.
>

### <a name="publish-the-application-using-visual-studio"></a>Veröffentlichen der Anwendung mit Visual Studio

Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio in einem Cluster bereitstellen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Voting**, und wählen Sie **Veröffentlichen**. Das Dialogfeld „Veröffentlichen“ wird angezeigt.

2. Kopieren Sie den **Verbindungsendpunkt** von der Seite des Partyclusters oder aus Ihrem Azure-Abonnement in das Feld **Verbindungsendpunkt**. Beispiel: `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Klicken Sie auf **Erweiterte Verbindungsparameter**, und vergewissern Sie sich, dass die Werte für *FindValue* und *ServerCertThumbprint* dem Fingerabdruck des Zertifikats, das in einem früheren Schritt für einen Partycluster installiert wurde, oder dem Fingerabdruck des Zertifikats für Ihr Azure-Abonnement entsprechen.

    ![Dialogfeld „Veröffentlichen“](./media/service-fabric-quickstart-dotnet/publish-app.png)

    Jede Anwendung im Cluster muss einen eindeutigen Namen besitzen.  Bei Partyclustern handelt es sich jedoch um eine öffentliche, freigegebene Umgebung, und unter Umständen tritt in einer vorhandenen Anwendung ein Konflikt auf.  Kommt es zu einem Namenskonflikt, benennen Sie das Visual Studio-Projekt um, und stellen Sie es erneut bereit.

3. Klicken Sie auf **Veröffentlichen**.

4. Öffnen Sie einen Browser, und geben Sie die Clusteradresse gefolgt von „:8080“ (oder einem anderen Port, sofern konfiguriert) ein, um Ihre Voting-Anwendung im Cluster abzurufen (Beispiel: `http://zwin7fh14scd.westus.cloudapp.azure.com:8080`). Sie sollten jetzt sehen, dass die Anwendung im Cluster in Azure ausgeführt wird. Fügen Sie auf der Voting-Webseite Abstimmungsoptionen hinzu, löschen Sie Abstimmungsoptionen, und stimmen Sie für einzelne oder mehrere dieser Optionen ab.

    ![Anwendungs-Front-End](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Partyclusters
> * Bereitstellen einer Anwendung in einem Remotecluster mithilfe von Visual Studio

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Aktivieren von HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
