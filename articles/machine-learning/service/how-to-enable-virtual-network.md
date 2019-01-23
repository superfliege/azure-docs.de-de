---
title: Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Netzwerks
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie innerhalb eines virtuellen Azure-Netzwerks auf sichere Weise Experimente für maschinelles Lernen ausführen und Rückschlüsse ziehen. In diesem Artikel wird gezeigt, wie Sie die Computeziele zum Trainieren von Modellen erstellen und Rückschlüsse innerhalb eines virtuellen Azure-Netzwerks ziehen. Darüber hinaus finden Sie hier Informationen zu den Anforderungen für geschützte virtuelle Netzwerke (etwa die Anforderung eingehender und ausgehender Ports).
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 01/08/2019
ms.openlocfilehash: 1890f830f9b2a4b108328de056636335fd3746e3
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2019
ms.locfileid: "54248602"
---
# <a name="securely-run-experiments-and-inferencing-inside-an-azure-virtual-network"></a>Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Azure-Netzwerks

In diesem Artikel erfahren Sie, wie Sie innerhalb eines virtuellen Netzwerks Ihre Experimente ausführen und Rückschlüsse ziehen. Ein virtuelles Netzwerk fungiert als Sicherheitsgrenze und schirmt Ihre Azure-Ressourcen vom öffentlichen Internet ab. Sie können ein virtuelles Azure-Netzwerk auch in Ihr lokales Netzwerk einbinden. So können Sie auf sichere Weise Ihre Modelle trainieren und auf Ihre bereitgestellten Modelle zugreifen, um Rückschlüsse zu ziehen.

Azure Machine Learning Service ist in Sachen Computeressourcen auf andere Azure-Dienste angewiesen. Computeressourcen (Computeziele) dienen zum Trainieren und Bereitstellen von Modellen. Diese Computeziele können innerhalb eines virtuellen Netzwerks erstellt werden. So können Sie beispielsweise eine Data Science VM verwenden, um ein Modell zu trainieren, und das Modell anschließend in Azure Kubernetes Service bereitstellen. Weitere Informationen zu virtuellen Netzwerken finden Sie in der [Übersicht über virtuelle Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

## <a name="storage-account-for-your-workspace"></a>Speicherkonto für Ihren Arbeitsbereich

Wenn Sie einen Azure Machine Learning Service-Arbeitsbereich erstellen, benötigt dieser ein Azure Storage-Konto. Aktivieren Sie für dieses Speicherkonto keine Firewallregeln. Azure Machine Learning Service benötigt uneingeschränkten Zugriff auf das Speicherkonto.

Sollten Sie nicht sicher sein, ob Sie diese Einstellungen geändert haben, sehen Sie sich den Abschnitt __Ändern der Standard-Netzwerkzugriffsregel__ des Dokuments [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security) an, und führen Sie die Schritte zum _Zulassen des Zugriffs_ über __Alle Netzwerke__ aus.

## <a name="use-machine-learning-compute"></a>Verwenden von Machine Learning Compute

Wenn Sie Machine Learning Compute in einem virtuellen Netzwerk verwenden möchten, machen Sie sich anhand der folgenden Informationen mit den Netzwerkanforderungen vertraut:

- Das virtuelle Netzwerk muss sich im gleichen Abonnement und in der gleichen Region befinden wie der Azure Machine Learning Service-Arbeitsbereich.

- Das für den Machine Learning Compute-Cluster angegebene Subnetz muss über genügend nicht zugewiesene IP-Adressen für die Anzahl virtueller Computer verfügen, die für den Cluster vorgesehen sind. Falls das Subnetz nicht über genügend nicht zugewiesene IP-Adressen verfügt, wird der Cluster teilweise zugeordnet.

- Wenn Sie zum Schutz des virtuellen Netzwerks den Datenverkehr einschränken möchten, müssen Sie einige Ports für den Machine Learning Compute-Dienst geöffnet lassen. Weitere Informationen finden Sie im Abschnitt [Erforderliche Ports](#mlcports).

- Überprüfen Sie, ob Berechtigungen für die Verwaltung des virtuellen Netzwerks durch Ihre Sicherheitsrichtlinien oder -sperren für das Abonnement oder die Ressourcengruppe Ihres virtuellen Netzwerks eingeschränkt werden.

- Wenn Sie mehrere Machine Learning Compute-Cluster in einem einzelnen virtuellen Netzwerk platzieren möchten, müssen Sie möglicherweise eine Kontingenterhöhung für eine oder mehrere der Ressourcen anfordern.

    Machine Learning Compute ordnet in der Ressourcengruppe, die das virtuelle Netzwerk enthält, automatisch zusätzliche Netzwerkressourcen zu. Für jeden Machine Learning Compute-Cluster ordnet Azure Machine Learning Service folgende Ressourcen zu: 

    - Eine Netzwerksicherheitsgruppe (NSG)

    - Eine öffentliche IP-Adresse

    - Ein Lastenausgleichsmodul

    Diese Ressourcen werden durch die [Ressourcenkontingente](https://docs.microsoft.com/azure/azure-subscription-service-limits) des Abonnements beschränkt. 

### <a id="mlcports"></a>Erforderliche Ports

Machine Learning Compute verwendet derzeit den Azure Batch-Dienst, um virtuelle Computer im angegebenen virtuellen Netzwerk bereitzustellen. Das Subnetz muss die eingehende Kommunikation des Batch-Diensts zulassen. Diese Kommunikation dient zur Planung der Ausführungen auf den Machine Learning Compute-Knoten sowie zur Kommunikation mit Azure Storage und anderen Ressourcen. Batch fügt NSGs auf der Ebene der Netzwerkschnittstellen (NICs) hinzu, die an virtuelle Computer angefügt sind. Diese NSGs konfigurieren automatisch Eingangs- und Ausgangsregeln, um folgenden Datenverkehr zuzulassen:

- Eingehender TCP-Datenverkehr an den Ports 29876 und 29877 von IP-Adressen der Batch-Dienstrolle 
 
- Eingehender TCP-Datenverkehr am Port 22, um Remotezugriff zu ermöglichen.
 
- Ausgehender Datenverkehr an allen Ports zum virtuellen Netzwerk

- Ausgehender Datenverkehr an allen Ports zum Internet

Seien Sie vorsichtig, wenn Sie Eingangs-/Ausgangsregeln in von Batch konfigurierten NSGs ändern. Wenn eine NSG die Kommunikation mit den Computeknoten blockiert, legt der Machine Learning Compute-Dienst den Zustand der Computeknoten auf „Nicht verwendbar“ fest.

Sie müssen keine NSGs auf der Subnetzebene angeben, da Batch eigene NSGs konfiguriert. Wenn das angegebene Subnetz allerdings über zugewiesene NSGs und/oder eine Firewall verfügt, konfigurieren Sie die Eingangs- und Ausgangssicherheitsregeln wie weiter oben erwähnt. Die folgenden Screenshots zeigen die Regelkonfiguration im Azure-Portal:

![Screenshot: NSG-Eingangsregeln für Machine Learning Compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Screenshot: NSG-Ausgangsregeln für Machine Learning Compute](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Erstellen von Machine Learning Compute in einem virtuellen Netzwerk

Führen Sie die folgenden Schritte aus, um einen Machine Learning Compute-Cluster über das **Azure-Portal** zu erstellen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Azure Machine Learning Service-Arbeitsbereich aus.

1. Wählen Sie im Abschnitt __Anwendung__ die Option __Compute__ aus. Wählen Sie anschließend __Computeressource hinzufügen__ aus. 

    ![Hinzufügen einer Computeressource in Azure Machine Learning Service](./media/how-to-enable-virtual-network/add-compute.png)

1. Verwenden Sie die folgenden Optionen, um diese Computeressource für die Verwendung eines virtuellen Netzwerks zu konfigurieren:

    - __Netzwerkkonfiguration__: Wählen Sie __Advanced__ (Erweitert).

    - __Ressourcengruppe__: Wählen Sie die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.

    - __Virtuelles Netzwerk:__ Wählen Sie das virtuelle Netzwerk aus, das das Subnetz enthält.

    - __Subnetz__: Wählen Sie das zu verwendende Subnetz aus.

    ![Screenshot: VNET-Einstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Ein Machine Learning Compute-Cluster kann auch mithilfe des **Azure Machine Learning SDK** erstellt werden. Der folgende Code erstellt einen neuen Machine Learning Compute-Cluster im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure Virtual Network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")
    
    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name = vnet_resourcegroup_name,
                                                           vnet_name = vnet_name,
                                                           subnet_name = subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    
    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Nach Abschluss des Erstellungsprozesses können Sie Ihr Modell mit dem Cluster trainieren. Weitere Informationen finden Sie unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight"></a>Verwenden eines virtuellen Computers oder von HDInsight

Gehen Sie wie folgt vor, um einen virtuellen Computer oder einen HDInsight-Cluster in einem virtuellen Netzwerk mit Ihrem Arbeitsbereich zu verwenden:

> [!IMPORTANT]
> Azure Machine Learning Service unterstützt nur virtuelle Computer, auf denen Ubuntu ausgeführt wird.

1. Erstellen Sie einen virtuellen Computer oder einen HDInsight-Cluster (etwa über das Azure-Portal oder mithilfe der Azure-Befehlszeilenschnittstelle), und platzieren Sie ihn in einem virtuellen Azure-Netzwerk. Weitere Informationen finden Sie in den folgenden Dokumenten:
    * [Tutorial: Erstellen und Verwalten virtueller Azure-Netzwerke für virtuelle Linux-Computer mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Erweitern von Azure HDInsight per Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. Damit Azure Machine Learning Service mit dem SSH-Port des virtuellen Computers oder des Clusters kommunizieren kann, müssen Sie einen Quelleintrag für die NSG konfigurieren. Der SSH-Port ist in der Regel der Port 22. Verwenden Sie die folgenden Informationen, um Datenverkehr von dieser Quelle zuzulassen:

    * __Quelle__: Wählen Sie __Diensttag__ aus.

    * __Quelldiensttag__: Wählen Sie __AzureMachineLearning__ aus.

    * __Quellportbereiche__: Wählen __*__ aus.

    * __Ziel__: Wählen Sie __Beliebig__ aus.

    * __Zielportbereiche__: Wählen Sie __22__ aus.

    * __Protokoll:__ Wählen Sie __Beliebig__ aus.

    * __Aktion:__ Wählen Sie __Zulassen__ aus.

   ![Screenshot: Eingangsregeln für Experimente auf einem virtuellen Computer oder in HDInsight innerhalb eines virtuellen Netzwerks](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Behalten Sie die Standardausgangsregeln für die NSG bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) im Abschnitt „Standardsicherheitsregeln“.
    
1. Fügen Sie den virtuellen Computer oder den HDInsight-Cluster an Ihren Azure Machine Learning Service-Arbeitsbereich an. Weitere Informationen finden Sie unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

## <a name="use-azure-kubernetes-service-aks"></a>Verwenden von Azure Kubernetes Service (AKS)

> [!IMPORTANT]
> Überprüfen Sie die Voraussetzungen, und planen Sie die IP-Adressierung für Ihren Cluster, bevor Sie mit den folgenden Schritten fortfahren. Weitere Informationen finden Sie unter [Konfigurieren von erweiterten Netzwerken in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/configure-advanced-networking).
> 
> Behalten Sie die Standardausgangsregeln für die NSG bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) im Abschnitt „Standardsicherheitsregeln“.
>
> Azure Kubernetes Service und das virtuelle Azure-Netzwerk müssen sich in der gleichen Region befinden.

Führen Sie im __Azure-Portal__ die folgenden Schritte aus, um Azure Kubernetes Service in einem virtuellen Netzwerk Ihrem Arbeitsbereich hinzuzufügen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Azure Machine Learning Service-Arbeitsbereich aus.

1. Wählen Sie im Abschnitt __Anwendung__ die Option __Compute__ aus. Wählen Sie anschließend __Computeressource hinzufügen__ aus. 

    ![Hinzufügen einer Computeressource in Azure Machine Learning Service](./media/how-to-enable-virtual-network/add-compute.png)

1. Verwenden Sie die folgenden Optionen, um diese Computeressource für die Verwendung eines virtuellen Netzwerks zu konfigurieren:

    - __Netzwerkkonfiguration__: Wählen Sie __Advanced__ (Erweitert).

    - __Ressourcengruppe__: Wählen Sie die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.

    - __Virtuelles Netzwerk:__ Wählen Sie das virtuelle Netzwerk aus, das das Subnetz enthält.

    - __Subnetz__: Wählen Sie das Subnetz aus.

    - __Adressbereich für Kubernetes Service__: Wählen Sie den Adressbereich für Kubernetes Service aus. Dieser Adressbereich verwendet einen IP-Adressbereich in CIDR-Notation, um die für den Cluster verfügbaren IP-Adressen zu definieren. Er darf sich nicht mit dem IP-Adressbereich eines Subnetzes überschneiden. Beispiel:  10.0.0.0/16.

    - __Kubernetes-DNS-Dienst – IP-Adresse__: Wählen Sie die IP-Adresse des Kubernetes-DNS-Diensts aus. Diese IP-Adresse wird dem Kubernetes-DNS-Dienst zugewiesen. Sie muss innerhalb des Adressbereichs für Kubernetes Service liegen. Beispiel:  10.0.0.10.

    - __Adresse der Docker-Brücke__: Wählen Sie die Adresse der Docker-Brücke aus. Diese IP-Adresse wird der Docker-Brücke zugewiesen. Sie darf weder dem IP-Adressbereich eines Subnetzes noch dem Adressbereich für Kubernetes Service angehören. Beispiel:  172.17.0.1/16

   ![Azure Machine Learning Service: VNET-Einstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

    > [!TIP]
    > Wenn Sie bereits über einen AKS-Cluster in einem virtuellen Netzwerk verfügen, können Sie ihn dem Arbeitsbereich anfügen. Weitere Informationen finden Sie unter [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-to-aks.md).

Sie können auch das **Azure Machine Learning SDK** verwenden, um Azure Kubernetes Service einem virtuellen Netzwerk hinzuzufügen. Der folgende Code erstellt eine neue Azure Kubernetes Service-Instanz im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace = ws,
                                  name = "myaks",
                                  provisioning_configuration = config)
```

Nach Abschluss des Erstellungsprozesses können Sie Rückschlüsse für einen AKS-Cluster hinter einem virtuellen Netzwerk ziehen. Weitere Informationen finden Sie unter [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md)
* [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst)
* [Verwenden von SSL zum Schützen von Webdiensten mit Azure Machine Learning Service](how-to-secure-web-service.md)