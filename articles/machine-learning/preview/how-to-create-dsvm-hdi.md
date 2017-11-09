---
title: "Erstellen von DSVM und HDI als Computeziele für Azure Machine Learning"
description: Erstellen Sie DSVM- und HDI-Spark-Cluster als Computeziele zum Azure ML-Experimentieren.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 9ce1d32a2785bec1164d2a89dea9946fe113cb33
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="create-dsvm-and-hdi-spark-cluster-as-compute-targets"></a>Erstellen von DSVM- und HDI Spark-Clustern als Computeziele

Sie können Ihr Machine Learning-Experiment bequem durch Hinzufügen zusätzlicher Computeziele wie Ubuntu-basierte DSVM- (Data Science Virtual Machine) und Apache Spark für Azure HDInsight-Cluster vertikal oder horizontal hochskalieren. Dieser Artikel führt Sie durch die Schritte zum Erstellen dieser Computeziele in Azure. Weitere Informationen zu Azure ML-Computezielen finden Sie unter [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](experimentation-service-configuration.md).

>[!NOTE]
>Sie müssen sicherzustellen, dass Sie über die erforderlichen Berechtigungen zum Erstellen von Ressourcen verfügen, z. B. virtuelle Computer und HDI-Cluster in Azure, bevor Sie fortfahren. Je nach Ihrer Konfiguration können beide Ressourcen auch entsprechend viele Computekerne auslasten. Stellen Sie sicher, dass Ihr Abonnement über ausreichend Kapazität für die virtuelle CPU-Kerne verfügt. Sie können sich immer an den Azure-Support wenden, um die maximale Anzahl von Kernen zu erhöhen, die in Ihrem Abonnement zulässig sind.

## <a name="create-an-ubuntu-dsvm-in-azure-portal"></a>Erstellen eines Ubuntu-DSVM im Azure-Portal

Sie können einen virtuellen Data Science-Computer (Data Science Virtual Machine, DSVM) über das Azure-Portal erstellen. 

1. Melden Sie sich über „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf den Link **+NEU** und suchen Sie nach „Virtueller Data Science-Computer für Linux“.
    ![Ubuntu](media/how-to-create-dsvm-hdi/ubuntu_dsvm.png)
4. Wählen Sie in der Liste **Virtuelle Data Science-Computer für Linux (Ubuntu)** aus, und führen Sie die Anweisungen auf dem Bildschirm aus, um den DSVM zu erstellen.

>[!IMPORTANT]
>Stellen Sie sicher, dass Sie **Kennwort** als _Authentifizierungstyp_ auswählen.

![Verwenden eines Kennworts](media/how-to-create-dsvm-hdi/use_pwd.png)

## <a name="create-an-ubuntu-dsvm-using-azure-cli"></a>Erstellen eines Ubuntu-DSVM mithilfe der Azure-CLI

Sie können auch eine Azure Resource Manager-Vorlage verwenden, um einen DSVM bereitzustellen.

>[!NOTE]
>Es wird angenommen, dass alle folgenden Befehle im Stammordner eines Azure ML-Projekts ausgeführt werden.

Erstellen Sie zunächst mit Ihrem bevorzugten Text-Editor eine `mydsvm.json`-Datei im Ordner `docs`. (Wenn sich im Stammordner des Projekts kein `docs`-Ordner befindet, erstellen Sie einen.) Diese Datei wird zum Konfigurieren einiger grundlegender Parameter für die Vorlage der Azure-Ressourcenverwaltung verwendet. 

Kopieren Sie den folgenden JSON-Codeausschnitt, und fügen Sie ihn in die `mydsvm.json`-Datei ein. Füllen Sie dann die entsprechenden Werte aus:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "<admin username>"},
     "adminPassword": { "value" : "<admin password>"},
     "vmName": { "value" : "<vm name>"},
     "vmSize": { "value" : "<vm size>"}
  }
}
```

Für das Feld _vmSize_ können Sie eine beliebige unterstützte VM-Größe verwenden, die in der [Vorlage der Azure-Ressourcenverwaltung für den Ubuntu-DSVM](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/multiazuredeploywithext.json) aufgeführt ist. Es wird empfohlen, eine der nachfolgenden Größen als Computeziele für Azure ML zu verwenden. 

- Standard_DS2_v2 
- Standard_DS3_v2 
- Standard_DS4_v2 
- Standard_DS12_v2 
- Standard_DS13_v2 
- Standard_DS14_v2 
- Standard_NC6 
- Standard_NC12 
- Standard_NC24 
 
>[!TIP]
> Die mit „NC“ gekennzeichneten VM-Größen verfügen über eine GPU.

Hier erhalten Sie weitere Informationen zu diesen [Größen für virtuelle Linux-Computer in Azure](../../virtual-machines/linux/sizes.md) und ihre [Informationen zu den Preisen](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Starten Sie das CLI-Fenster über die App von Azure ML Workbench, indem Sie auf **Datei** --> **Eingabeaufforderung öffnen** oder das Menüelement **PowerShell öffnen** klicken. 

>[!NOTE]
>Sie können dies auch in einer beliebigen Befehlszeilenumgebung durchführen, in der az-cli installiert ist.

Geben Sie im Befehlszeilenfenster die folgenden Befehle ein:

```azurecli
# first make sure you have a valid Azure authentication token
$ az account get-access-token

# if you don't have a valid token, please log in to Azure first. 
# if you already do, you can skip this step.
$ az login

# list all subscriptions you have access to
$ az account list -o table

# make sure you set the subscription you want to use to create DSVM as the current subscription
$ az account set -s <subscription name or Id>

# it is always a good idea to create a resource group for the VM and associated resources to live in.
# you can use any Azure region, but it is best to create them in the region where your Azure ML Experimentation account is, e.g. eastus2, westcentralus or australiaeast.
# also, only certain Azure regions has GPU-equipped VMs available.
$ az group create -n <resource group name> -l <azure region>

# now let's create the DSVM based on the JSON configuration file you created earlier.
# note we assume the mydsvm.json config file is placed in the "docs" sub-folder.
$ az group deployment create -g <resource group name> --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters @docs/mydsvm.json

# find the FQDN (fully qualified domain name) of the VM just created. 
# you can also use IP address from the next command if FQDN is not set.
$ az vm show -g <resource group name> -n <vm name> --query "fqdns"

# find the IP address of the VM just created
$ az vm show -g <resource group name> -n <vm name> --query "publicIps"
```
## <a name="attach-a-dsvm-compute-target"></a>Anfügen eines DSVM-Computeziels
Nachdem der DSVM erstellt wurde, können Sie ihn jetzt an Ihr Azure ML-Projekt anfügen.

```azurecli
# attach the DSVM compute target
# it is a good idea to use FQDN in case the IP address changes after you deallocate the VM and restart it
$ az ml computetarget attach --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password> --type remotedocker

# prepare the Docker image on the DSVM 
$ az ml experiment prepare -c <compute target name>
```
Jetzt sollten Sie bereit sein, um Experimente auf diesem DSVM ausführen zu können.

## <a name="deallocate-a-dsvm-and-restart-it-later"></a>Aufheben der Zuordnung eines DSVMs und Neustart zu einem späteren Zeitpunkt
Wenn Sie die Computeaufgaben von Azure ML abgeschlossen haben, können Sie die DSVM-Zuordnung aufheben. Diese Aktion fährt den virtuellen Computer herunter und gibt die Computeressourcen frei, aber die virtuellen Datenträger werden beibehalten. Wenn die Zuordnung des virtuellen Computers aufgehoben ist, fallen für Sie keine Computekosten an.

So heben Sie die Zuordnung eines virtuellen Computers auf:

```azurecli
$ az vm deallocate -g <resource group name> -n <vm name>
```

Verwenden Sie den `az ml start`-Befehl, um den virtuellen Computer wieder zu aktivieren:

```azurecli
$ az vm start -g <resource group name> -n <vm name>
```

## <a name="expand-the-dsvm-os-disk"></a>Erweitern des DSVM-Betriebssystemdatenträgers
Ein virtueller Linux-Computer verfügt in Azure in der Regel über einen Betriebssystemdatenträger mit einer Größe von 30 GB. Wenn er als Computeziel für Azure ML verwendet wird, kann seine Kapazität schnell vom Docker-Modul aufgebraucht werden, das Docker-Images abruft und darüber hinaus Conda-Ebenen erstellt. Es ist daher ratsam, den Betriebssystemdatenträger zu vergrößern (z. B. 200 GB), um den Fehler zu vermeiden, dass der Datenträger voll ist, während Sie sich mitten in einer Ausführung befinden. Informationen zur einfachen Umsetzung über die Azure-CLI finden Sie unter [Erweitern virtueller Festplatten auf virtuellen Linux-Computern mit der Azure-CLI](../../virtual-machines/linux/expand-disks.md). 

## <a name="create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal"></a>Erstellen eines Apache Spark für Azure HDInsight-Clusters im Azure-Portal

Sie müssen einen Apache Spark für Azure HDInsight-Cluster im Azure-Portal erstellen, um Spark-Aufträge zum horizontalen Hochskalieren auszuführen.

1. Melden Sie sich über „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf den Link **+NEU**, und suchen Sie nach „HDInsight“.

    ![HDI-Suche](media/how-to-create-dsvm-hdi/hdi.png)
    
3. Wählen Sie in der Liste **HDInsight** aus, und klicken Sie dann auf die Schaltfläche **Erstellen**.
4. Stellen Sie auf dem Konfigurationsbildschirm **Grundlagen** unter den **Clustertyp**-Einstellungen sicher, dass Sie **Spark** als _Clustertyp_, **Linux** als _Betriebssystem_ und **Spark 2.1.0 (HDI 3.6)** als Version auswählen.

    ![Konfigurieren von HDI](media/how-to-create-dsvm-hdi/configure_hdi.png)

    >[!IMPORTANT]
    >Beachten Sie im obigen Bildschirm, dass der Cluster über das Feld _Benutzername für Clusteranmeldung_ und das Feld _SSH-Benutzername (Secure Shell)_ verfügt. Hierbei handelt es sich um zwei verschiedene Benutzeridentitäten, obwohl Sie der Einfachheit halber dasselbe Kennwort für beide Anmeldenamen angeben können. Der _Benutzername für Clusteranmeldung_ wird für die Anmeldung bei der Webbenutzeroberfläche zur Verwaltung des HDI-Cluster verwendet. Der _Benutzername für SSH-Anmeldung_ wird für die Anmeldung beim Hauptknoten des Clusters verwendet. Dies ist für Azure ML zum Verteilen von Spark-Aufträgen erforderlich.

5. Wählen Sie die erforderliche Clustergröße und Knotengröße aus, und beenden Sie den Erstellungsassistenten. Es kann bis zu 30 Minuten dauern, bis die Bereitstellung des Clusters abgeschlossen ist. 

## <a name="attach-an-hdi-spark-cluster-compute-target"></a>Anfügen eines Computeziels eines HDI Spark-Clusters

Nachdem der Spark HDI-Cluster erstellt wurde, können Sie ihn jetzt an Ihr Azure ML-Projekt anfügen.

```azurecli
# attach the HDI compute target
$ az ml computetarget attach --name <compute target name> --address <cluster name, such as myhdicluster123.azurehdinsight.net> --username <ssh username> --password <ssh password> --type cluster

# prepare the conda environment on HDI
$ az ml experiment prepare -c <compute target name>
```
Jetzt sollten Sie bereit sein, um Experimente auf diesem Spark-Cluster ausführen zu können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:
- [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](experimentation-service-configuration.md)
- [Azure Machine Learning Workbench-Experimentieren-Dienst – Konfigurationsdateien](experimentation-service-configuration-reference.md)
- [Apache Spark für Azure HDInsight-Cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/)
- [Virtueller Computer für Data Science](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)
