---
title: Erstellen einer Azure Service Fabric-Containeranwendung unter Linux | Microsoft-Dokumentation
description: In diesem Schnellstart erstellen Sie Ihre erste Linux-Containeranwendung unter Azure Service Fabric.  Erstellen Sie ein Docker-Image mit Ihrer Anwendung, übertragen Sie es per Push an eine Containerregistrierung, erstellen Sie eine Service Fabric-Containeranwendung, und stellen Sie diese bereit.
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/11/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 65f048d67ef5f250691700a382e781814c57e8a8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31416369"
---
# <a name="quickstart-deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Schnellstart: Bereitstellen einer Azure Service Fabric-Containeranwendung unter Linux in Azure
Azure Service Fabric ist eine Plattform für verteilte Systeme zum Bereitstellen und Verwalten von skalierbaren und zuverlässigen Microservices und Containern. 

In diesem Schnellstart wird veranschaulicht, wie Sie Linux-Container in einem Service Fabric-Cluster bereitstellen. Nach Abschluss des Vorgangs verfügen Sie über eine Abstimmungsanwendung mit einem Python-Web-Front-End und einem Redis-Back-End, die in einem Service Fabric-Cluster ausgeführt werden. Außerdem erfahren Sie, wie Sie ein Failover für eine Anwendung ausführen und eine Anwendung in Ihrem Cluster skalieren.

![Webseite der Voting-App][quickstartpic]

In dieser Schnellstartanleitung verwenden Sie die Bash-Umgebung in Azure Cloud Shell, um Befehle über die Service Fabric-Befehlszeilenschnittstelle auszuführen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Beim erstmaligen Ausführen von Cloud Shell werden Sie zur Einrichtung Ihrer `clouddrive`-Dateifreigabe aufgefordert. Sie können die Standardwerte übernehmen oder eine bereits vorhandene Dateifreigabe anfügen. Weitere Informationen finden Sie unter [Einrichten einer `clouddrive`-Dateifreigabe](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share).

## <a name="get-the-application-package"></a>Abrufen des Anwendungspakets
Zum Bereitstellen von Containern in Service Fabric benötigen Sie eine Gruppe von Manifestdateien (die Anwendungsdefinition), in denen die einzelnen Container und die Anwendung beschrieben ist.

Klonen Sie in Cloud Shell mithilfe von „git“ eine Kopie der Anwendungsdefinition, und navigieren Sie anschließend in Ihrem Klon zum Verzeichnis `Voting`. 

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```

## <a name="create-a-service-fabric-cluster"></a>Erstellen von Service Fabric-Clustern
Für die Bereitstellung der Anwendung in Azure benötigen Sie einen Service Fabric-Cluster, in dem die Anwendung ausgeführt wird. Mit Partyclustern können Sie schnell und einfach einen Service Fabric-Cluster erstellen. Bei Partyclustern handelt es sich um zeitlich begrenzte kostenlose Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team betrieben werden. Mithilfe von Partyclustern können Sie Anwendungen bereitstellen und sich mit der Plattform vertraut machen. Der Cluster verwendet ein einzelnes selbstsigniertes Zertifikat für Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit.

Melden Sie sich an, und treten Sie einem [Linux-Cluster](http://aka.ms/tryservicefabric) bei. Klicken Sie auf den Link **PFX**, um das PFX-Zertifikat auf Ihren Computer herunterzuladen. Unter dem Link **ReadMe** finden Sie das Zertifikatkennwort sowie Anweisungen zum Konfigurieren verschiedener Umgebungen für die Verwendung des Zertifikats. Lassen Sie sowohl die**Willkommensseite** als auch die Seite mit der**Infodatei** geöffnet, da Sie einige der Anweisungen in den folgenden Schritten benötigen. 

> [!Note]
> Pro Stunde ist eine begrenzte Anzahl von Partyclustern verfügbar. Sollte beim Registrieren für einen Partycluster ein Fehler auftreten, können Sie eine Weile warten und es dann erneut versuchen. Alternativ können Sie die Schritte unter [Tutorial: Bereitstellen eines Service Fabric-Linux-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-linux-cluster.md) ausführen, um einen Cluster in Ihrem Abonnement zu erstellen. 
> 
>Beachten Sie bei der Erstellung Ihres eigenen Clusters, dass der Web-Front-End-Dienst standardmäßig für das Lauschen auf eingehenden Datenverkehr an Port 80 konfiguriert ist. Stellen Sie sicher, dass der Port in Ihrem Cluster geöffnet ist. (Wenn Sie einen Partycluster verwenden, ist dieser Port geöffnet.)
>

## <a name="configure-your-environment"></a>Konfigurieren der Umgebung
Service Fabric bietet mehrere Tools, mit denen Sie einen Cluster und die dazugehörigen Anwendungen verwalten können:

- Service Fabric Explorer: Ein browserbasiertes Tool.
- Service Fabric-Befehlszeilenschnittstelle (CLI): Basiert auf der Azure CLI 2.0.
- PowerShell-Befehle 

In dieser Schnellstartanleitung verwenden Sie die Service Fabric-Befehlszeilenschnittstelle in Cloud Shell sowie Service Fabric Explorer. In den folgenden Abschnitten erfahren Sie, wie Sie mit diesen Tools das Zertifikat installieren, das zum Herstellen einer Verbindung mit Ihrem sicheren Cluster benötigt wird.

### <a name="configure-certificate-for-the-service-fabric-cli"></a>Konfigurieren des Zertifikats für die Service Fabric-Befehlszeilenschnittstelle
Um die Befehlszeilenschnittstelle in Cloud Shell verwenden zu können, müssen Sie die PFX-Zertifikatdatei in Cloud Shell hochladen und damit anschließend eine PEM-Datei erstellen.

1. Laden Sie das Zertifikat in Ihr aktuelles Arbeitsverzeichnis in Cloud Shell hoch. Ziehen Sie dazu die PFX-Zertifikatdatei auf Ihrem Computer aus dem Downloadordner in das Cloud Shell-Fenster.  

2. Konvertieren Sie die PFX-Datei mithilfe des folgenden Befehls in eine PEM-Datei. (Für Partycluster können Sie einen spezifischen Befehl für Ihre PFX-Datei und Ihr Kennwort aus den Anweisungen auf der Seite mit der**Infodatei**kopieren.)

    ```bash
    openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
    ``` 

### <a name="configure-certificate-for-service-fabric-explorer"></a>Konfigurieren des Zertifikats für Service Fabric Explorer
Zur Verwendung von Service Fabric Explorer müssen Sie die von der Partycluster-Website heruntergeladene PFX-Zertifikatdatei in Ihren Zertifikatspeicher (Windows oder Mac) oder in den Browser selbst (Ubuntu) importieren. Sie benötigen das PFX-Kennwort für den privaten Schlüssel von der Seite mit der**Infodatei**.

Importieren Sie das Zertifikat mit der von Ihnen bevorzugten Methode in Ihr System. Beispiel: 

- Unter Windows: Doppelklicken Sie auf die PFX-Datei, und befolgen Sie die Anweisungen, um das Zertifikat in Ihrem persönlichen Zertifikatspeicher (`Certificates - Current User\Personal\Certificates`) zu installieren. Alternativ können Sie den in der**Infodatei** angegebenen PowerShell-Befehl ausführen.
- Auf einem Mac: Doppelklicken Sie auf die PFX-Datei, und befolgen Sie die Anweisungen, um das Zertifikat in Ihrer Keychain zu installieren.
- Unter Ubuntu: Der Standardbrowser in Ubuntu 16.04 ist Mozilla Firefox. Klicken Sie zum Importieren des Zertifikats rechts oben in Firefox auf die Menüschaltfläche und anschließend auf **Einstellungen**. Suchen Sie über das Suchfeld auf der Seite **Einstellungen** nach „Zertifikate“. Klicken Sie auf **Zertifikate anzeigen**, klicken Sie auf die Registerkarte **Ihre Zertifikate**, klicken Sie auf **Importieren**, und befolgen Sie die Anweisungen zum Importieren des Zertifikats.
 
   ![Installieren des Zertifikats in Firefox](./media/service-fabric-quickstart-containers-linux/install-cert-firefox.png) 

## <a name="deploy-the-service-fabric-application"></a>Bereitstellen der Service Fabric-Anwendung 
1. Stellen Sie in Cloud Shell über die Befehlszeilenschnittstelle eine Verbindung mit dem Service Fabric-Cluster in Azure her. Der Endpunkt ist der Verwaltungsendpunkt für Ihren Cluster. Die PEM-Datei haben Sie im vorherigen Abschnitt erstellt. (Für Partycluster können Sie einen spezifischen Befehl für Ihre PEM-Datei und Ihren Verwaltungsendpunkt aus den Anweisungen auf der Seite mit der**Infodatei**kopieren.)

    ```bash
    sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
    ```

2. Verwenden Sie das Installationsskript, um die Definition der Voting-Anwendung in den Cluster zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    ./install.sh
    ```

2. Navigieren Sie in einem Webbrowser zum Service Fabric Explorer-Endpunkt für Ihren Cluster. Der Endpunkt hat das folgende Format: https://\<URL Ihres Azure Service Fabric-Clusters>:19080/Explorer. Beispiel: `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. </br>(Für Party-Cluster finden Sie den entsprechenden Service Fabric Explorer-Endpunkt auf der**Willkommensseite**.) 

3. Erweitern Sie den Knoten **Anwendungen**, um zu prüfen, ob nun ein Eintrag für den Voting-Anwendungstyp und die von Ihnen erstellte Instanz vorhanden ist.

    ![Service Fabric Explorer][sfx]

3. Navigieren Sie in einem Webbrowser zur URL des Clusters (beispielsweise `http://linh1x87d1d.westus.cloudapp.azure.com:80`), um eine Verbindung mit dem ausgeführten Container herzustellen. Die Abstimmungsanwendung sollte im Browser angezeigt werden.

    ![Webseite der Voting-App][quickstartpic]


> [!NOTE]
> Service Fabric-Anwendungen können auch mit Docker Compose bereitgestellt werden. Verwenden Sie beispielsweise den folgenden Befehl, um die Anwendung mithilfe von Docker Compose im Cluster bereitzustellen und zu installieren:
>  ```bash
> sfctl compose create --deployment-name TestApp --file-path ../docker-compose.yml
> ```

## <a name="fail-over-a-container-in-a-cluster"></a>Ausführen eines Failovers für einen Container in einem Cluster
Bei einem Ausfall sorgt Service Fabric dafür, dass Ihre Containerinstanzen automatisch auf andere Knoten im Cluster verschoben werden. Sie können die Container eines Knotens auch manuell entfernen und diese geregelt auf andere Knoten im Cluster verschieben. Service Fabric bietet mehrere Skalierungsoptionen für Dienste. In den folgenden Schritten wird Service Fabric Explorer verwendet.

Führen Sie die folgenden Schritte aus, um für den Front-End-Container ein Failover auszuführen:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster. Beispiel: `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/Voting/azurevotefront**, und erweitern Sie den (durch eine GUID dargestellten) Partitionsknoten. Beachten Sie den Knotennamen in der Strukturansicht. Hier werden die Knoten angezeigt, auf denen der Container derzeit ausgeführt wird (beispielsweise `_nodetype_4`).
3. Erweitern Sie in der Strukturansicht den Knoten **Knoten**. Klicken Sie neben dem Knoten, auf dem der Container ausgeführt wird, auf die Auslassungspunkte.
4. Wählen Sie **Neu starten** aus, um den Knoten neu zu starten, und bestätigen Sie die Neustartaktion. Der Neustart bewirkt, dass für den Container ein Failover auf einen anderen Knoten im Cluster ausgeführt wird.

    ![Knotenansicht in Service Fabric Explorer][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Skalieren von Anwendungen und Diensten in einem Cluster
Service Fabric-Dienste können für einen Cluster auf einfache Weise skaliert werden, um die Last für die Dienste zu bewältigen. Sie skalieren einen Dienst, indem Sie die Anzahl von Instanzen ändern, die im Cluster ausgeführt werden.

Führen Sie die folgenden Schritte aus, um den Web-Front-End-Dienst zu skalieren:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster. Beispiel: `https://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klicken Sie in der Strukturansicht neben dem Knoten **fabric:/Voting/azurevotefront** auf die drei Auslassungspunkte und anschließend auf **Scale Service** (Dienst skalieren).

    ![Service Fabric Explorer: Starten der Dienstskalierung][containersquickstartscale]

  Sie können jetzt angeben, dass die Anzahl von Instanzen des Web-Front-End-Diensts skaliert werden soll.

3. Ändern Sie die Anzahl in **2**, und klicken Sie auf **Scale Service** (Dienst skalieren).
4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/Voting/azurevotefront**, und erweitern Sie den (durch eine GUID dargestellten) Partitionsknoten.

    ![Service Fabric Explorer: Dienstskalierung abgeschlossen][containersquickstartscaledone]

    Sie sehen jetzt, dass der Dienst über zwei Instanzen verfügt. In der Strukturansicht wird angezeigt, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben Sie die Ressourcen verdoppelt, die dem Front-End-Dienst zur Bewältigung der Benutzerauslastung zur Verfügung stehen. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Sollte ein Dienst ausfallen, stellt Service Fabric sicher, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
1. Verwenden Sie das Deinstallationsskript (uninstall.sh) aus der Vorlage, um die Anwendungsinstanz aus dem Cluster zu löschen und die Registrierung des Anwendungstyps aufzuheben. Es dauert etwas, bis die Instanz durch dieses Skript bereinigt wurde. Aus diesem Grund sollte das Installationsskript nicht unmittelbar nach diesem Skript ausgeführt werden. Sie können mithilfe von Service Fabric Explorer ermitteln, ob die Instanz entfernt und die Registrierung des Anwendungstyps aufgehoben wurde. 

    ```bash
    ./uninstall.sh
    ```

2. Nach Abschluss der Clusterverwendung können Sie das Zertifikat aus Ihrem Zertifikatspeicher entfernen. Beispiel: 
   - Unter Windows: Verwenden Sie das [MMC-Snap-In „Zertifikate“](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in). Beim Hinzufügen des Snap-Ins muss **Mein Benutzerkonto** ausgewählt sein. Navigieren Sie zu `Certificates - Current User\Personal\Certificates`, und entfernen Sie das Zertifikat.
   - Auf einem Mac: Verwenden Sie die Keychain-App.
   - Unter Ubuntu: Führen Sie die Schritte aus, die Sie zum Anzeigen von Zertifikaten verwendet haben, und entfernen Sie das Zertifikat.

3. Wenn Sie Cloud Shell nicht weiter verwenden möchten, können Sie das zugeordnete Speicherkonto löschen, um Gebühren zu vermeiden. Schließen Sie Ihre Cloud Shell-Sitzung. Klicken Sie im Azure-Portal auf das Speicherkonto, das Cloud Shell zugeordnet ist. Klicken Sie anschließend am oberen Seitenrand auf **Löschen**, und reagieren Sie auf die Eingabeaufforderungen.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie eine Linux-Containeranwendung in einem Service Fabric-Cluster in Azure bereitgestellt, ein Failover für die Anwendung ausgeführt und die Anwendung im Cluster skaliert. Weitere Informationen zur Verwendung von Linux-Containern in Service Fabric finden Sie im Tutorial für Linux-Container-Apps.

> [!div class="nextstepaction"]
> [Erstellen einer Linux-Container-App](./service-fabric-tutorial-create-container-images.md)


[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
