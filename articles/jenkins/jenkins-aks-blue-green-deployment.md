---
title: Bereitstellen in Azure Kubernetes Service (AKS) mithilfe von Jenkins und dem Blau/Grün-Bereitstellungsmuster
description: Hier erfahren Sie, wie die Bereitstellung in Azure Kubernetes Service (AKS) mithilfe von Jenkins und dem Blau/Grün-Bereitstellungsmuster funktioniert.
services: app-service\web
documentationcenter: ''
author: tomarcher
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 07/23/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 472622f78303593b7a4d5d5136aa47f34a1f44b1
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227823"
---
# <a name="deploy-to-azure-kubernetes-service-aks-using-jenkins-and-bluegreen-deployment-pattern"></a>Bereitstellen in Azure Kubernetes Service (AKS) mithilfe von Jenkins und dem Blau/Grün-Bereitstellungsmuster

Azure Kubernetes Service (AKS) verwaltet Ihre gehostete Kubernetes-Umgebung und ermöglicht so die schnelle und einfache Bereitstellung und Verwaltung von Containeranwendungen – ganz ohne Kenntnisse in Sachen Containerorchestrierung. Darüber hinaus übernimmt der Dienst laufende Vorgänge und Wartungsaufgaben für Sie, indem er Ressourcen nach Bedarf bereitstellt, aktualisiert und skaliert, ohne Ihre Anwendungen offline zu schalten. Weitere Informationen zu AKS finden Sie in der [AKS-Dokumentation](/azure/aks/).

Die Blau/Grün-Bereitstellung ist ein DevOps-Muster für Continuous Delivery (CD), bei dem eine vorhandene Version (blau) live bleibt, während eine neue Version (grün) bereitgestellt wird. In der Regel nutzt dieses Muster einen Lastenausgleich, um zunehmendes Datenverkehrsvolumen an die grüne Bereitstellung zu leiten. Wenn die Überwachung einen Incident entdeckt, kann der Datenverkehr an die blaue Bereitstellung umgeleitet werden, die weiterhin ausgeführt wird. Weitere Informationen zu Continuous Delivery finden Sie im Artikel [Was ist Continuous Delivery?](/azure/devops/what-is-continuous-delivery).

In diesem Tutorial erfahren Sie, wie die Bereitstellung in AKS mithilfe von Jenkins und dem Blau/Grün-Bereitstellungsmuster funktioniert. Sie führen folgende Aufgaben aus:

> [!div class="checklist"]
> * Sie erfahren alles Wissenswerte über das Blau/Grün-Bereitstellungsmuster.
> * Sie erstellen einen verwalteten Kubernetes-Cluster.
> * Sie führen ein Beispielskript aus, um einen Kubernetes-Cluster zu konfigurieren.
> * Sie konfigurieren manuell einen Kubernetes-Cluster.
> * Sie erstellen einen Jenkins-Auftrag und führen ihn aus.

## <a name="prerequisites"></a>Voraussetzungen
- [GitHub-Konto](https://github.com): Sie benötigen ein GitHub-Konto, um das Beispielrepository zu klonen.
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest): Zum Erstellen des Kubernetes-Clusters wird die Azure CLI 2.0 verwendet.
- [Chocolatey](https://chocolatey.org): Ein Paket-Manager, mit dem kubectl installiert wird.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): Eine Befehlszeilenschnittstelle, in der Befehle für Kubernetes-Cluster ausgeführt werden.
- [jq](https://stedolan.github.io/jq/download/): Ein schlanker befehlszeilenbasierter JSON-Prozessor.

## <a name="clone-the-sample-app-from-github"></a>Klonen der Beispiel-App von GitHub herunter

Im Microsoft-Repository in GitHub finden Sie eine Beispiel-App, die veranschaulicht, wie die Bereitstellung in AKS mithilfe von Jenkins und dem Blau/Grün-Bereitstellungsmuster funktioniert. In diesem Abschnitt erstellen Sie einen Fork dieses Repositorys in Ihrer GitHub-Instanz, und klonen die App in Ihrem lokalen System.

1. Navigieren Sie zum GitHub-Repository für die Beispiel-App [todo-app-java-on-azure](https://github.com/microsoft/todo-app-java-on-azure.git).

    ![Beispiel-App im Microsoft-Repository in GitHub.](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Erstellen Sie einen Fork des Repositorys, indem Sie oben rechts auf der Seite **Fork** auswählen und den Anweisungen zum Forken des Repositorys in Ihr GitHub-Konto folgen.

    ![Forken der Beispiel-App in Ihr GitHub-Konto.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Nach dem Forken des Repositorys werden Sie sehen, dass sich der Kontoname in den Namen Ihres Kontos geändert hat und ein Hinweis angibt, von wo aus der Fork erstellt wurde (Microsoft).

    ![Beispiel-App nach dem Forken in ein anderes GitHub-Konto.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Wählen Sie **Clone or download** (Klonen oder Herunterladen) aus.

    ![In GitHub können Sie ein Repository schnell klonen oder herunterladen.](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. Klicken Sie im Fenster **Clone with HTTPS** (Über HTTPS klonen) auf das Kopiersymbol.

    ![Kopieren Sie die Klon-URL in die Zwischenablage.](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Öffnen Sie ein Terminal- oder Bash-Fenster.

1. Wechseln Sie in das Verzeichnis, in dem Sie die lokale Kopie (den Klon) des Repositorys speichern möchten.

1. Klonen Sie mithilfe des Befehls `git clone` die URL, die Sie zuvor kopiert haben.

    ![Geben Sie „git clone“ und die Klon-URL ein, um einen Klon des Repositorys zu erstellen.](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Drücken Sie die EINGABETASTE, um den Klonvorgang zu starten.

    ![Der Befehl „git clone“ erstellt eine persönliche Kopie des Repositorys, die Sie zum Testen verwenden können.](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Wechseln Sie in das neu erstellte Verzeichnis, das den Klon der App-Quelle enthält.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Erstellen und Konfigurieren eines verwalteten Kubernetes-Clusters

In diesem Abschnitt führen Sie die folgenden Schritte aus:

- Sie verwenden die Azure CLI 2.0, um einen verwalteten Kubernetes-Cluster zu erstellen.
- Sie erfahren, wie Sie einen Cluster entweder manuell oder mithilfe des Setupskripts einrichten.
- Sie erstellen eine Azure Container Registry-Instanz.

> [!NOTE]   
> AKS befindet sich derzeit in der Vorschauphase. Informationen zum Aktivieren der Vorschauversion für Ihr Azure-Abonnement finden Sie unter [Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription).

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Verwenden der Azure CLI 2.0, um einen verwalteten Kubernetes-Cluster zu erstellen
Wenn Sie einen verwalteten Kubernetes-Cluster mit der [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) erstellen, stellen Sie sicher, dass Sie die Azure CLI-Version 2.0.25 oder höher verwenden.

1. Melden Sie sich beim Azure-Konto an. Nachdem Sie den Befehl `az login` eingegeben haben, erhalten Sie Erläuterungen dazu, wie Sie die Anmeldung abschließen. 
    
    ```bash
    az login
    ```

1. Wenn Sie den Befehl `az login` im vorherigen Schritt ausführen, wird eine Liste mit all Ihren Azure-Abonnements (sowie der zugehörigen Abonnement-IDs) angezeigt. In diesem Schritt legen Sie das Azure-Standardabonnement fest. Ersetzen Sie den Platzhalter &lt;ID-Ihres-Abonnements> durch die ID des gewünschten Azure-Abonnements. 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Erstellen Sie eine Ressourcengruppe. Ersetzen Sie den Platzhalter &lt;Name-Ihrer-Ressourcengruppe> durch den Namen Ihrer neuen Ressourcengruppe, und ersetzen Sie den Platzhalter &lt;Ihr-Speicherort> durch den entsprechenden Speicherort. Der Befehl `az account list-locations` listet alle Azure-Speicherorte auf. Während der AKS-Vorschau sind nicht alle Speicherorte verfügbar. Wenn Sie einen Speicherort eingeben, der zu diesem Zeitpunkt nicht gültig ist, wird eine Fehlermeldung angezeigt, in der die verfügbaren Speicherorte aufgelistet werden.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Erstellen Sie den Kubernetes-Cluster. Ersetzen Sie &lt;Name-Ihrer-Ressourcengruppe> durch den Namen der Ressourcengruppe, die im vorherigen Schritt erstellt wurde, und ersetzen Sie &lt;Name-Ihres-Kubernetes-Clusters> durch den Namen Ihres neues Clusters. (Es kann einige Minuten dauern, bis dieser Vorgang abgeschlossen ist.)

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Einrichten des Kubernetes-Clusters

Die Einrichtung einer Blau/Grün-Bereitstellung in AKS kann entweder manuell oder mit einem Setupskript erfolgen, das in dem zuvor geklonten Beispiel enthalten ist. In diesem Abschnitt werden beide Möglichkeiten erläutert.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Einrichten des Kubernetes-Clusters über das Beispielsetupskript
1. Bearbeiten Sie die Datei **deploy/aks/setup/setup.sh**, und ersetzen Sie die folgenden Platzhalter durch die entsprechenden Werte für Ihre Umgebung: 

    - **&lt;Name-Ihrer-Ressourcengruppe**
    - **&lt;Name-Ihres-Kubernetes-Clusters>**
    - **&lt;Ihr-Speicherort>**
    - **&lt;Ihr-DNS-Namenssuffix>**

    ![Das setup.sh-Skript enthält verschiedene Platzhalter, die für Ihre Umgebung angepasst werden können.](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Führen Sie das Setupskript aus.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>Manuelles Einrichten eines Kubernetes-Clusters 
1. Laden Sie die Kubernetes-Konfiguration in Ihren Profilordner herunter.

    ```bash
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Wechseln Sie in das Verzeichnis **deploy/aks/setup**. 

1. Führen Sie die folgenden **kubectl**-Befehle aus, um die Dienste für den öffentlichen Endpunkt und die beiden Testendpunkte einzurichten.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Aktualisieren Sie den DNS-Namen für den öffentlichen Endpunkt und die Testendpunkte. Beim Erstellen eines Kubernetes-Clusters wird eine [zusätzliche Ressourcengruppe](https://github.com/Azure/AKS/issues/3) mit dem Benennungsmuster **MC_&lt;Name-Ihrer-Ressourcengruppe>_&lt;yName-Ihres-Kubernetes-Clusters>_&lt;Ihr-Speicherort>** erstellt.

    Ermitteln Sie die öffentlichen IP-Adressen in der Ressourcengruppe.

    ![Öffentliche IP-Adressen in der Ressourcengruppe](./media/jenkins-aks-blue-green-deployment/publicip.png)

    Suchen Sie die externe IP-Adresse für jeden Dienst, indem Sie den folgenden Befehl ausführen:
    
    ```bash
    kubectl get service todoapp-service
    ```
    
    Aktualisieren Sie mit dem folgenden Befehl den DNS-Namen für die entsprechende IP-Adresse:

    ```bash
    az network public-ip update --dns-name aks-todoapp --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Wiederholen Sie den Aufruf für `todoapp-test-blue` und `todoapp-test-green`:

    ```bash
    az network public-ip update --dns-name todoapp-blue --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>

    az network public-ip update --dns-name todoapp-green --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Der DNS-Name muss in Ihrem Abonnement eindeutig sein. Sie können `<your-dns-name-suffix>` verwenden, um die Eindeutigkeit sicherzustellen.

### <a name="create-azure-container-registry"></a>Erstellen einer Azure Container Registry-Instanz

1. Führen Sie den Befehl `az acr create` aus, um eine Azure Container Registry-Instanz zu erstellen. Nach dem Erstellen der Azure Container Registry-Instanz verwenden Sie im nächsten Abschnitt `login server` als URL der Docker-Registrierung.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Führen Sie den Befehl `az acr credential` aus, um Ihre Azure Container Registry-Anmeldeinformationen anzuzeigen. Notieren Sie sich den Benutzername und das Kennwort für die Docker-Registrierung, da diese im nächsten Abschnitt verwendet werden.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Vorbereiten des Jenkins-Servers

In diesem Abschnitt erfahren Sie, wie Sie den Jenkins-Server für die Ausführung eines Builds vorbereiten – dies ist zu Testzwecken ausreichend. Wie jedoch bereits im Jenkins-Artikel unter [Security implication of building on master](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master) (Sicherheitsaspekte beim Erstellen auf einem Masterknoten) erläutert, ist es ratsam, einen [Azure-VM-Agent](https://plugins.jenkins.io/azure-vm-agents) oder einen [Azure-Container-Agent](https://plugins.jenkins.io/azure-container-agents) zu verwenden, um einen Agent in Azure einzurichten, auf dem Ihre Builds ausgeführt werden. 

1. Stellen Sie einen [Jenkins-Master in Azure](https://aka.ms/jenkins-on-azure) bereit.

1. Stellen Sie über SSH eine Verbindung mit dem Server her, und installieren Sie die Buildtools auf dem Server, auf dem der Build ausgeführt werden soll.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Installieren Sie Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Stellen Sie sicher, dass Benutzer `jenkins` über die Berechtigung verfügt, die `docker`-Befehle auszuführen.

1. [Installieren Sie kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Laden Sie jq herunter](https://stedolan.github.io/jq/download/).

1. Installieren Sie jq mithilfe des folgenden Befehls:

   ```bash
   sudo apt-get install jq
   ```
   
1. Installieren Sie die Plug-Ins in Jenkins, indem Sie auf dem Jenkins-Dashboard die folgenden Schritte ausführen:

    1. Wählen Sie **Manage Jenkins > Manage Plugins > Available** (Jenkins verwalten > Plug-Ins verwalten >Verfügbar) aus.
    1. Suchen Sie das Azure Container Service-Plug-In, und installieren Sie es.

1. Sie müssen Anmeldeinformationen eingeben, die zum Verwalten von Ressourcen in Azure verwendet werden. Wenn Sie noch nicht über das **Azure Credential**-Plug-In verfügen, installieren Sie es.

1. Fügen Sie die Anmeldeinformationen für Ihren Azure-Dienstprinzipal als Typ **Microsoft Azure Service Principal** hinzu.

1. Fügen Sie Ihren Benutzernamen und Ihr Kennwort für die Azure Docker-Registrierung (die Sie im Abschnitt **Erstellen einer Azure Container Registry-Instanz** erhalten haben) als Typ **Username with password** (Benutzername mit Kennwort) hinzu.

## <a name="edit-the-jenkinsfile"></a>Bearbeiten der Jenkinsfile

1. Navigieren Sie in Ihrem eigenen Repository zu `/deploy/aks/`, und öffnen Sie `Jenkinsfile`.

2. Aktualisieren Sie die Datei wie folgt:

    ```groovy
    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = '<your-resource-group-name>'
    def aks = '<your-kubernetes-cluster-name>'

    def cosmosResourceGroup = '<your-cosmodb-resource-group>'
    def cosmosDbName = '<your-cosmodb-name>'
    def dbName = '<your-dbname>'

    def dockerRegistry = '<your-acr-name>.azurecr.io'
    ```
    
    Aktualisieren Sie die ID der ACR-Anmeldeinformationen:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>Erstellen des Auftrags
1. Fügen Sie einen neuen Auftrag mit Typ **Pipeline** hinzu.

1. Wählen Sie **Pipeline > Definition > Pipeline script from SCM** (Pipeline > Definition > Pipeline-Skript von SCM) aus.

1. Geben Sie die URL des SCM-Repositorys mit &lt;Ihr-geforktes-Repository> an.

1. Geben Sie den Skriptpfad als `deploy/aks/Jenkinsfile` ein.

## <a name="run-the-job"></a>Ausführen des Auftrags

1. Überprüfen Sie, ob Sie Ihr Projekt erfolgreich in Ihrer lokalen Umgebung ausführen können. [Führen Sie das Projekt auf einem lokalen Computer aus](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it).

1. Führen Sie den Jenkins-Auftrag aus. Wenn Sie den Jenkins-Auftrag zum ersten Mal ausführen, stellt Jenkins die todo-App in der blauen Umgebung bereit, bei der es sich um die standardmäßige inaktive Umgebung handelt. 

1. Um zu überprüfen, ob der Auftrag ausgeführt wurde, navigieren Sie zu den URLs:
    - Öffentlicher Endpunkt: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Blauer Endpunkt: `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Grüner Endpunkt: `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

Der öffentliche und der blaue Testendpunkt verfügen über das gleiche Update, der grüne Endpunkt dagegen weist das Tomcat-Standardimage auf. 

Wenn Sie den Build mehr als einmal ausführen, durchläuft er die blaue und die grüne Bereitstellung. Anders gesagt: Wenn die aktuelle Umgebung blau ist, wird der Auftrag in der grünen Umgebung bereitgestellt bzw. getestet, dann wird der öffentliche Anwendungsendpunkt so aktualisiert, dass Datenverkehr an die grüne Umgebung geroutet wird, wenn der Test erfolgreich verlaufen ist.

## <a name="additional-information"></a>Zusätzliche Informationen

Weitere Informationen zur Bereitstellung ohne Ausfallzeiten finden Sie in dieser [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die in diesem Tutorial erstellten Azure-Ressourcen, wenn Sie sie nicht mehr benötigen.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Problembehandlung

Wenn bei den Jenkins-Plug-Ins Fehler auftreten, melden Sie das Problem auf der [Jenkins-JIRA-Seite](https://issues.jenkins-ci.org/) für die jeweilige Komponente.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie die Bereitstellung in Azure Kubernetes Service (AKS) mithilfe von Jenkins und dem Blau/Grün-Bereitstellungsmuster funktioniert. Weitere Informationen zum Azure-Jenkins-Anbieter finden Sie auf der Website „Jenkins in Azure“.

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/jenkins/)