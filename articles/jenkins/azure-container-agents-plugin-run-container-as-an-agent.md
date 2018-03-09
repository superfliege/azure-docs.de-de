---
title: Erstellen eines Projekts in Azure mithilfe von Jenkins und Azure Container Instances
description: "Erfahren Sie, wie Sie mit dem Azure Container Agent-Plug-In für Jenkins ein Projekt in Azure mit Azure Container Instances erstellen."
services: multiple
documentationcenter: 
author: tomarcher
manager: rloutlaw
editor: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 02/28/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 557b21340a0ba4e5381d7505b14a172aa3478b84
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="build-a-project-in-azure-using-jenkins-and-azure-container-instances"></a>Erstellen eines Projekts in Azure mithilfe von Jenkins und Azure Container Instances

Mit Azure Container Instances sind Sie leicht einsatzbereit, ohne dass Sie virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen. Azure Container Instances bietet eine sekundengenaue Abrechnung basierend auf der benötigten Kapazität und ist damit eine interessante Option für vorübergehende Workloads wie das Erstellen von Builds.

Folgendes wird vermittelt:
> [!div class="checklist"]
> * Installieren und Konfigurieren eines Jenkins-Servers in Azure
> * Installieren und Konfigurieren des Azure Container Agents-Plug-Ins für Jenkins
> * Verwenden von Azure Container Instances zum Erstellen der [Spring PetClinic-Beispielanwendung](https://github.com/spring-projects/spring-petclinic)

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement** – Weitere Informationen zu den Azure-Kaufoptionen finden Sie unter [Azure erwerben](https://azure.microsoft.com/pricing/purchase-options/) oder [Kostenlose Testversion (1 Monat)](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI 2.0 oder Azure Cloud Shell** – Installieren Sie eines der folgenden Produkte, in das Azure-Befehle eingegeben werden:

    - [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) – Ermöglicht das Ausführen von Azure-Befehlen über ein Befehls- oder Terminalfenster.
    - [Azure Cloud Shell](/azure/cloud-shell/quickstart.md) – Browserbasierte Shelloberfläche. Cloud Shell ermöglicht den Zugriff auf eine browserbasierte Befehlszeilenumgebung, die extra auf Azure-Verwaltungsaufgaben ausgerichtet ist.

## <a name="install-a-jenkins-server-on-azure-using-the-jenkins-marketplace-image"></a>Installieren eines Jenkins-Servers in Azure mit dem Jenkins Marketplace-Image

Jenkins unterstützt ein Modell, bei dem der Jenkins-Server die Arbeit an einen oder mehrere Agents delegiert, sodass eine einzelne Jenkins-Installation eine große Anzahl von Projekten hosten oder verschiedene Umgebungen bereitstellen kann, die für Builds oder Tests benötigt werden. Die Schritte in diesem Abschnitt führen Sie durch die Installation und Konfiguration eines Jenkins-Servers in Azure.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](../../includes/jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-the-jenkins-server-running-on-azure"></a>Herstellen einer Verbindung mit dem in Azure ausgeführten Jenkins-Server

Nachdem Sie Jenkins in Azure installiert haben, müssen Sie eine Verbindung zu Jenkins herstellen. Die folgenden Schritte führen Sie durch das Einrichten einer SSH-Verbindung mit der in Azure ausgeführten Jenkins-VM. 

[!INCLUDE [jenkins-connect-to-jenkins-server-running-on-azure](../../includes/jenkins-connect-to-jenkins-server-running-on-azure.md)]

## <a name="update-jenkins-dns"></a>Aktualisieren des Jenkins-DNS

Jenkins muss seine eigene URL kennen, wenn es Links erstellt, die auf sich selbst verweisen. Die URL muss z. B. verwendet werden, wenn Jenkins E-Mails sendet, die direkte Links zu Buildergebnissen enthalten. 

In diesem Abschnitt wird das Festlegen der Jenkins-URL erläutert.

1. Navigieren Sie in Ihrem Browser unter `http://localhost:8080` zum Jenkins-Dashboard.

1. Wählen Sie **Manage Jenkins** aus.

    ![Verwalten der Jenkins-Optionen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wählen Sie **Configure System** aus.

    ![Verwalten der Jenkins-Plug-In-Option im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Geben Sie unter **Jenkins Location** die URL des Jenkins-Servers ein.

1. Wählen Sie **Speichern**aus.

## <a name="update-jenkins-to-allow-java-network-launch-protocol-jnlp"></a>Aktualisieren von Jenkins zum Zulassen von JNLP (Java Network Launch Protocol)

Der Jenkins-Agent stellt über das JNLP (Java Network Launch Protocol) eine Verbindung mit dem Jenkins-Server her. In diesem Abschnitt wird erläutert, wie ein Port für JNLP-Agents für die Kommunikation mit dem Jenkins-Server angegeben wird.

1. Wählen Sie im Jenkins-Dashboard **Manage Jenkins** aus.

    ![Verwalten der Jenkins-Optionen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wählen Sie **Configure Global Security** aus.

    ![Konfigurieren der globalen Sicherheit im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-global-security.png)

1. Wählen Sie unter **Agents** die Option **Fixed** aus, und geben Sie einen Port ein. Der Screenshot zeigt den Portwert 12345 als Beispiel an. Sie sollten einen für Ihre Umgebung geeigneten Port angeben.

    ![Aktualisieren der globalen Sicherheitseinstellungen von Jenkins zum Zulassen von JNLP](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-set-jnlp.png)

1. Wählen Sie **Speichern**aus.

1. Geben Sie über die Azure CLI 2.0 oder die Azure Cloud Shell den folgenden Befehl ein, um eine eingehende Regel für die Jenkins-Netzwerksicherheitsgruppe zu erstellen:

    ```azurecli
    az network nsg rule create  \
    --resource-group JenkinsResourceGroup \
    --nsg-name JenkinsNSG  \
    --name allow-https \
    --description "Allow access to port 12345 for HTTPS" \
    --access Allow \
    --protocol Tcp  \
    --direction Inbound \
    --priority 102 \
    --source-address-prefix "*"  \
    --source-port-range "*"  \
    --destination-address-prefix "*" \
    --destination-port-range "12345"
    ```

## <a name="create-and-add-an-azure-service-principal-to-the-jenkins-credentials"></a>Erstellen und Hinzufügen eines Azure-Dienstprinzipals zu den Jenkins-Anmeldeinformationen

Ein Azure-Dienstprinzipal ist für die Bereitstellung in Azure erforderlich. Die folgenden Schritte führen Sie durch den Prozess der Erstellung eines Dienstprinzipals (falls Sie noch keinen besitzen) und der Aktualisierung von Jenkins mit Ihrem Dienstprinzipal.

1. Wenn Sie bereits über einen Dienstprinzipal verfügen (und dessen Abonnement-ID, Mandanten, appID und Kennwort kennen), können Sie diesen Schritt überspringen. Wenn Sie einen Sicherheitsprinzipal erstellen müssen, finden Sie weitere Informationen im Artikel [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Notieren Sie sich beim Erstellen des Prinzipals die Werte für die Abonnement-ID, den Mandanten, die appID und das Kennwort.

1. Wählen Sie **Credentials** aus.

    ![Verwalten der Option für Anmeldeinformationen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials.png)

1. Wählen Sie unter **System** die Option **Credentials** aus.

    ![Verwalten der Option für Systemanmeldeinformationen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-system.png)

1. Wählen Sie **Global credentials (unrestricted)** aus.

    ![Verwalten der Option für globale Systemanmeldeinformationen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-global.png)

1. Wählen Sie **Adding some credentials** aus.

    ![Hinzufügen von Anmeldeinformationen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-adding-credentials.png)

1. Wählen Sie in der Dropdownliste **Kind** die Option **Microsoft Azure Service Principal** aus, damit auf der Seite Felder anzeigt werden, die für das Hinzufügen eines Dienstprinzipals spezifisch sind. Geben Sie dann die angeforderten Werte wie folgt ein:

    - **Scope**: Wählen Sie die Option für **Global (Jenkins, Knoten, Elemente, alle untergeordneten Elemente usw.)** aus.
    - **Subscription ID**: Geben Sie die Azure-Abonnement-ID ein, die Sie beim Ausführen von `az account set` angegeben haben.
    - **Client ID**: Verwenden Sie den `appId`-Wert, der von `az ad sp create-for-rbac` zurückgegeben wurde.
    - **Client Secret**: Verwenden Sie den `password`-Wert, der von `az ad sp create-for-rbac` zurückgegeben wurde.
    - **Tenant ID**: Verwenden Sie den `tenant`-Wert, der von `az ad sp create-for-rbac` zurückgegeben wurde.
    - **Azure Environment**: Wählen Sie `Azure` aus.
    - **ID**: Geben Sie `myTestSp` ein. Dieser Wert wird weiter unten in diesem Tutorial erneut verwendet.
    - **Description**: (Optional) Geben Sie einen Beschreibungswert für diesen Prinzipal ein.

    ![Angeben neuer Dienstprinzipaleigenschaften im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-principal-properties.png)

1. Nachdem Sie die Informationen eingegeben haben, die den Prinzipal definieren, können Sie optional **Verify Service Principal** auswählen, um sicherzustellen, dass alles ordnungsgemäß funktioniert. Wenn Ihr Dienstprinzipal ordnungsgemäß definiert ist, wird die Meldung, dass der Microsoft Azure-Dienstprinzipal erfolgreich überprüft wurde, unter dem Feld **Description** angezeigt.

1. Wenn Sie fertig sind, wählen Sie **OK** aus, um den Prinzipal zu Jenkins hinzuzufügen. Das Jenkins-Dashboard zeigt den neu hinzugefügten Prinzipal auf der Seite **Global Credentials** an.

## <a name="create-an-azure-resource-group-for-your-azure-container-instances"></a>Erstellen einer Azure-Ressourcengruppe für Ihre Azure Container Instances

Azure Container Instances müssen in einer Azure-Ressourcengruppe platziert werden. Eine Azure-Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält.

Geben Sie den folgenden Befehl über die Azure CLI 2.0 oder Azure Cloud Shell ein, um eine Ressourcengruppe namens `JenkinsAciResourceGroup` an der Position `eastus` zu erstellen:

```azurecli
az group create --name JenkinsAciResourceGroup --location eastus
```

Der Befehl `az group create` zeigt nach der Fertigstellung Ergebnisse an, die dem folgenden Beispiel ähnlich sind:

```JSON
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/JenkinsAciResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "JenkinsAciResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="install-the-azure-container-agents-plugin-for-jenkins"></a>Installieren des Azure Container Agents-Plug-Ins für Jenkins

Wenn Sie das Azure Container Agents-Plug-In bereits installiert haben, können Sie diesen Abschnitt überspringen.

Nachdem Sie die Azure-Ressourcengruppe für Ihren Jenkins-Agent erstellt haben, veranschaulichen die folgenden Schritte das Installieren des Azure Container Agents-Plug-Ins:

1. Wählen Sie im Jenkins-Dashboard **Manage Jenkins** aus.

    ![Verwalten der Jenkins-Optionen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wählen Sie **Manage Plugins** aus.

    ![Verwalten der Jenkins-Plug-In-Option im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-plugins.png)

1. Wählen Sie **Available** aus.

    ![Anzeigen der verfügbaren Jenkins-Plug-In-Option im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-view-available-plugins.png)

1. Geben Sie `Azure Container Agents` in das Textfeld **Filter** ein. (Die Liste wird während der Eingabe des Texts gefiltert.)

    ![Filtern der verfügbaren Jenkins-Plug-Ins im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-filter-available-plugins.png)

1. Aktivieren Sie das Kontrollkästchen neben dem Plug-In **Azure Container Agents**, und wählen Sie dann eine der Optionen für die Installation aus. Für die Zwecke dieser Demo wurde die Option **Install without restart** ausgewählt.

    ![Installieren des Azure Container Agents-Plug-Ins über das Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin.png)

1.  Nachdem Sie die Option zur Installation des/der gewünschten Plug-Ins ausgewählt haben, zeigt das Jenkins-Dashboard eine Seite mit dem Status der Installation an.

    ![Installationsstatus zum Installieren des Azure Container Agents-Plug-Ins über das Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin-confirmation.png)

    Wählen Sie **Go back to the top page** aus, um zur Hauptseite des Jenkins-Dashboards zurückzukehren.

## <a name="configure-the-azure-container-agents-plugin"></a>Konfigurieren des Azure Container Agents-Plug-Ins

Nachdem das Azure Container Agents-Plug-In installiert wurde, führt Sie dieser Abschnitt durch die Konfiguration des Plug-Ins im Jenkins-Dashboard.

1. Wählen Sie im Jenkins-Dashboard **Manage Jenkins** aus.

    ![Verwalten der Jenkins-Optionen im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wählen Sie **Configure System** aus.

    ![Verwalten der Jenkins-Plug-In-Option im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Suchen Sie den Abschnitt **Cloud** am unteren Seitenrand, und wählen Sie aus der Dropdownliste **Add a new cloud** die Option **Azure Container Instance** aus.

    ![Hinzufügen eines neuen Cloudanbieters über das Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-cloud-provider.png)

1. Geben Sie im Abschnitt **Azure Container Instance** die folgenden Werte an:

    - **Cloud name**: (Optional, da dieser Wert standardmäßig einen automatisch generierten Namen erhält.) Geben Sie einen Namen für diese Instanz an. 
    - **Azure Credential**: Wählen Sie den Dropdownpfeil und dann den Eintrag `myTestSp` aus, der den zuvor erstellten Azure-Dienstprinzipal identifiziert.
    - **Resource Group**: Wählen Sie den Dropdownpfeil und dann den Eintrag `JenkinsAciResourceGroup` aus, der die zuvor erstellte Azure Container Instance-Ressourcengruppe identifiziert.

    ![Definieren der Azure Container Instance-Eigenschaften](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-properties.png)

1. Wählen Sie den Dropdownpfeil **Add Container Template** und dann **Aci Container Template** aus.

1. Geben Sie im Abschnitt **Aci container Template** die folgenden Werte an:

    - **Name**: Geben Sie `ACI-container` ein.
    - **Labels**: Geben Sie `ACI-container` ein.
    - **Docker Image**: Geben Sie `cloudbees/jnlp-slave-with-java-build-tools` ein.

    ![Definieren der Azure Container Instance-Imageeigenschaften](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-image-properties.png)

1. Wählen Sie **Advanced** (Erweitert).

1. Wählen Sie den Dropdownpfeil **Retention Strategy** und dann **Container Idle Retention Strategy** aus. Durch die Auswahl dieser Option hält Jenkins den Agent aktiv, bis auf dem Agent kein neuer Auftrag ausgeführt wird und die angegebene Leerlaufzeit abgelaufen ist.

    ![Definieren der Azure Container Instance-Aufbewahrungsstrategie](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-retention-strategy.png)

1. Wählen Sie **Speichern**aus.

## <a name="create-the-spring-petclinic-application-job-in-jenkins"></a>Erstellen des Spring PetClinic-Anwendungsauftrags in Jenkins

Die folgenden Schritte führen Sie durch die Erstellung eines Jenkins-Auftrags (als Freestyle-Projekt) zur Erstellung der Spring PetClinic-Anwendung.

1. Wählen Sie im Jenkins-Dashboard **New Item** aus.

    ![Menüoption „New Item“ im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-item.png)

1. Geben Sie `myPetClinicProject` als Elementnamen ein, und wählen Sie **Freestyle project** aus.

    ![Neues Freestyle-Projekt im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project.png)

1. Klicken Sie auf **OK**.

1. Wählen Sie die Registerkarte **General** aus, und geben Sie die folgenden Werte an:

    - **Restrict where project can be run**: Wählen Sie diese Option aus.
    - **Label Expression**: Geben Sie `ACI-container` ein. Wenn Sie das Feld verlassen, wird in einer Meldung bestätigt, dass die Bezeichnung von der im vorherigen Schritt erstellten Cloudkonfiguration unterstützt wird.

    ![Allgemeine Einstellungen für ein neues Freestyle-Projekt im Jenkins-Dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project-general.png)

1. Wählen Sie die Registerkarte **Source Code Management** aus, und geben Sie die folgenden Werte an:

    - **Source Code Management**: Wählen Sie **Git** aus.
    - **Repository URL**: Geben Sie die folgende URL für das GitHub-Repository der Spring PetClinic-Beispielanwendung ein: `https://github.com/spring-projects/spring-petclinic.git`.

1. Wählen Sie die Registerkarte **Build** aus, und führen Sie die folgenden Aufgaben aus:

    - Wählen Sie den Dropdownpfeil **Add build step** und dann **Invoke top-level Maven targets** aus.

    - Geben Sie für **Goals** den Wert `package` ein.

1. Wählen Sie zum Speichern der neuen Projektdefinition die Option **Save** aus.

## <a name="build-the-spring-petclinic-application-job-in-jenkins"></a>Erstellen des Spring PetClinic-Anwendungsauftrags in Jenkins

Es ist an der Zeit, das Projekt zu erstellen. In diesem Abschnitt wird erläutert, wie Sie ein Projekt über das Jenkins-Dashboard erstellen.

1. Wählen Sie im Jenkins-Dashboard `myPetClinicProject` aus.

    ![Wählen Sie das zu erstellende Projekt aus dem Jenkins-Dashboard aus.](./media/azure-container-agents-plugin-run-container-as-an-agent/select-project-to-build.png)

1. Wählen Sie **Build now** aus. 

    ![Erstellen Sie das Projekt über das Jenkins-Dashboard.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-project.png)

1. Wenn Sie ein Build in Jenkins starten, wird der Build in eine Warteschlange eingereiht. Im Falle eines Azure Container Agents kann der Build erst ausgeführt werden, wenn der Azure Container Agent gestartet und online gestellt wurde. Bis dahin erhalten Sie eine Meldung, die den Namen des Agents und die Tatsache angibt, dass der Build noch aussteht. (Dieser Vorgang dauert etwa fünf Minuten, ist aber nur notwendig, wenn Sie den Agenten zum ersten Mal für einen Build verwenden. Nachfolgende Builds sind viel schneller, da der Agent zu diesem Zeitpunkt online ist.)

    ![Der Build wird in die Warteschlange eingereiht, bis der Agent erstellt und online gestellt wurde.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-pending.png)

    Sobald der Build beginnt, zeigt eine Statusanzeige an, dass der Build ausgeführt wird:

    ![Der Build wird ausgeführt, sobald die Statusanzeige angezeigt wird.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-running.png)

1. Wenn die Statusanzeige nicht mehr angezeigt wird, wählen Sie den Pfeil neben der Buildnummer aus. Wählen Sie im Kontextmenü **Console output** aus.

    ![Klicken Sie auf das Menüelement „Console Log“, um die Buildinformationen anzuzeigen.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-menu.png)

1. Die Konsolenprotokollinformationen, die vom Buildprozess ausgegeben werden. Wählen Sie zum Anzeigen aller Buildinformationen (einschließlich Informationen zum Build, der remote auf dem von Ihnen erstellten Azure-Agent ausgeführt wird) die Option **Full log** aus.

    ![Klicken Sie auf den Link „Full log“, um ausführlichere Buildinformationen anzuzeigen.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log.png)

    Die Option „Full log“ zeigt ausführliche Buildinformationen an:

    ![Die Option „Full log“ zeigt ausführliche Buildinformationen an.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-full.png)
    
1. Um die Bereitschaft des Builds anzuzeigen, scrollen Sie zum unteren Ende des Protokolls.

    ![Die Buildbereitschaft wird am unteren Rand des Buildprotokolls angezeigt.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-disposition.png)

## <a name="clean-up-azure-resources"></a>Bereinigen von Azure-Ressourcen

In diesem Tutorial haben Sie Ressourcen erstellt, die in zwei Azure-Ressourcengruppen enthalten sind: 
    - `JenkinsResourceGroup`: Enthält die Azure-Ressourcen für den Jenkins-Server.
    - `JenkinsAciResourceGroup`: Enthält die Azure-Ressourcen für den Jenkins-Agent.
    
Wenn Sie keine der Ressourcen in einer Azure-Ressourcengruppe mehr verwenden müssen, können Sie die Ressourcengruppe wie folgt mit dem Befehl `az group delete` löschen (indem Sie den Platzhalter &lt;Ressourcengruppe> durch den Namen der zu löschenden Ressourcengruppe ersetzen):

```azurecli
az group delete -n <resourceGroup>
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Die neuesten Artikel und Beispiele finden Sie im Hub „Jenkins in Azure“](https://docs.microsoft.com/azure/jenkins/)
