---
title: Erstellen und Konfigurieren von Azure Kubernetes Service-Clustern in Azure mithilfe von Ansible
description: Erfahren Sie, wie Sie mithilfe von Ansible einen Azure Kubernetes Service-Cluster in Azure erstellen und verwalten können
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Aks, Container, Kubernetes
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 6d7c5f961256e0ae1831bd76353cadd761f4b8ac
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011988"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Erstellen und Konfigurieren von Azure Kubernetes Service-Clustern in Azure mithilfe von Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können Ansible verwenden, um Ihren Azure Kubernetes Service (AKS) zu verwalten. Dieser Artikel beschreibt, wie Sie mithilfe von Ansible einen Azure Kubernetes Service-Cluster erstellen und verwalten können.

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- **Konfigurieren von Ansible** - [Erstellen von Azure-Anmeldeinformationen und Konfigurieren von Ansible](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible und die Module des Azure SDK für Python** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Azure-Dienstprinzipal**: Notieren Sie [beim Erstellen des Dienstprinzipals](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal) die folgenden Werte: **App-ID**, **Anzeigename**, **Kennwort**  und **Mandant**.

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.6 erforderlich. 

## <a name="create-a-managed-aks-cluster"></a>Erstellen eines verwalteten AKS-Clusters
Das folgende Ansible-Beispielplaybook erstellt eine Ressourcengruppe und einen AKS-Cluster, der sich in der Ressourcengruppe befindet:

  ```yaml
  - name: Create Azure Kubernetes Service
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      location: eastus
      aks_name: myAKSCluster
      username: azureuser
      ssh_key: "your_ssh_key"
      client_id: "your_client_id"
      client_secret: "your_client_secret"
    tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create a managed Azure Container Services (AKS) cluster
      azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 2
            vm_size: Standard_D2_v2
        tags:
          Environment: Production
  ```

In der folgenden Aufzählung werden die vorangehenden Ansible-Playbookcodes erläutert:
- Der erste Abschnitt in **Tasks** definiert eine Ressourcengruppe namens **myResourceGroup** im Speicherort **eastus**. 
- Der zweite Abschnitt in **Tasks** definiert einen AKS-Cluster mit dem Namen **myAKSCluster** in der Ressourcengruppe **myResourceGroup**. 

Um den AKS-Cluster mit Ansible zu erstellen, speichern Sie das vorherige Beispielplaybook als `azure_create_aks.yml`, und führen Sie das Playbook mit dem folgenden Befehl aus:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

Die Ausgabe des Befehls **ansible-playbook* sieht in etwa folgendermaßen aus und zeigt, dass der AKS-Cluster erfolgreich erstellt wurde:

  ```bash
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create a Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>Skalieren der AKS-Knoten

Das Beispielplaybook aus dem vorherigen Abschnitt definiert zwei Knoten. Wenn Sie größere oder kleinere Containerworkloads in Ihrem Cluster benötigen, können Sie die Anzahl der Knoten auf einfache Weise anpassen. Im Beispielplaybook in diesem Abschnitt wird die Anzahl von Knoten von 2 auf 3 Knoten erhöht. Die Knotenanzahl wird geändert, indem der Wert **count** im Block **agent_pool_profiles** geändert wird. 

Geben Sie Ihren eigenen Wert für `ssh_key` `client_id` und `client_secret` in den Block **service_principal** ein:

```yaml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"    
        location: "{{ location }}"
        resource_group: "{{ resource_group }}" 
        dns_prefix: "{{ aks_name }}" 
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Um den Azure Kubernetes Service-Cluster mit Ansible zu skalieren, speichern Sie das vorherige Playbook als *azure_configure_aks.yml*, und führen Sie das Playbook wie folgt aus:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

Die folgende Ausgabe zeigt, dass der AKS-Cluster erfolgreich erstellt wurde:

  ```bash
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Tutorial: Skalieren einer Anwendung in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-scale)