---
title: Erstellen eines virtuellen Linux-Computers in Azure mithilfe von Ansible
description: Hier erfahren Sie, wie Sie mithilfe von Ansible einen virtuellen Linux-Computer in Azure erstellen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, virtueller Computer
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/22/2018
ms.openlocfilehash: 1f7f4809e064de15bb0a18c404f0df81307b1b9a
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073984"
---
# <a name="use-ansible-to-create-a-linux-virtual-machine-in-azure"></a>Erstellen eines virtuellen Linux-Computers in Azure mithilfe von Ansible
Ansible nutzt eine deklarative Sprache und ermöglicht Ihnen dadurch das Erstellen, Konfigurieren und Bereitstellen von Azure-Ressourcen über Ansible-*Playbooks*. Die einzelnen Abschnitte dieses Artikels veranschaulichen, wie die Abschnitte eines Ansible-Playbooks aussehen können, um verschiedene Aspekte eines virtuellen Linux-Computers zu erstellen und konfigurieren. Das [vollständige Ansible-Playbook](#complete-sample-ansible-playbook) finden Sie am Ende dieses Artikels.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Ansible benötigt eine Ressourcengruppe, in der Ihre Ressourcen bereitgestellt werden. Der folgende Abschnitt eines Ansible-Beispielplaybooks erstellt eine Ressourcengruppe namens `myResourceGroup` am Standort `eastus`:

```yaml
- name: Create resource group
  azure_rm_resourcegroup:
    name: myResourceGroup
    location: eastus
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Beim Erstellen eines virtuellen Azure-Computers müssen Sie ein [virtuelles Netzwerk](/azure/virtual-network/virtual-networks-overview) erstellen oder ein vorhandenes virtuelles Netzwerk verwenden. Außerdem müssen Sie entscheiden, wie auf Ihre virtuellen Computer im virtuellen Netzwerk zugegriffen werden soll. Der folgende Abschnitt eines Ansible-Beispielplaybooks erstellt ein virtuelles Netzwerk mit dem Namen `myVnet` im Adressbereich `10.0.0.0/16`:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.0.0.0/16"
```

Alle in einem virtuellen Netzwerk bereitgestellten Azure-Ressourcen werden in einem [Subnetz](/azure/virtual-network/virtual-network-manage-subnet) innerhalb eines virtuellen Netzwerks bereitgestellt. 

Der folgende Abschnitt eines Ansible-Beispielplaybooks erstellt ein Subnetz mit dem Namen `mySubnet` im virtuellen Netzwerk `myVnet`:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```

## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
[Öffentliche IP-Adressen](/azure/virtual-network/virtual-network-ip-addresses-overview-arm) ermöglichen Internetressourcen die eingehende Kommunikation mit Azure-Ressourcen. Öffentliche IP-Adressen ermöglichen Azure-Ressourcen auch die ausgehende Kommunikation mit dem Internet und öffentlichen Azure-Diensten mit einer der Ressource zugewiesenen IP-Adresse. Die Adresse ist für die Ressource reserviert, bis Sie die Zuweisung aufheben. Wenn einer Ressource keine öffentliche IP-Adresse zugewiesen wird, kann sie trotzdem ausgehend mit dem Internet kommunizieren, Azure weist jedoch dynamisch eine verfügbare IP-Adresse zu, die nicht für die Ressource reserviert wird. 

Der folgende Abschnitt eines Ansible-Beispielplaybooks erstellt eine öffentliche IP-Adresse mit dem Namen `myPublicIP`:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```

## <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe
Mit einer [Netzwerksicherheitsgruppe](/azure/virtual-network/security-overview) können Sie Netzwerkdatenverkehr von und zu Azure-Ressourcen in einem virtuellen Azure-Netzwerk filtern. Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln, die eingehenden Netzwerkdatenverkehr an verschiedene Typen von Azure-Ressourcen oder ausgehenden Netzwerkdatenverkehr von diesen zulassen oder verweigern. 

Im folgenden Abschnitt eines Ansible-Beispielplaybooks wird die Netzwerksicherheitsgruppe `myNetworkSecurityGroup` erstellt und eine Regel zum Zulassen von SSH-Datenverkehr über den TCP-Port 22 definiert:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: Tcp
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-a-virtual-network-interface-card"></a>Erstellen einer virtuellen Netzwerkadapterkarte
Ein virtueller Netzwerkadapterkarte verbindet Ihren virtuellen Computer mit einem angegebenen virtuellen Netzwerk, einer öffentlichen IP-Adresse und einer Netzwerksicherheitsgruppe. 

Im folgenden Abschnitt eines Ansible-Beispielplaybooks wird die virtuelle Netzwerkadapterkarte `myNIC` erstellt, die mit den von Ihnen erstellen virtuellen Netzwerkressourcen verbunden ist:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Im letzten Schritt wird ein virtueller Computer erstellt, der alle Ressourcen verwendet, die Sie in den vorherigen Abschnitten dieses Artikels erstellt haben. 

Mit dem Abschnitt des Ansible-Beispielplaybooks in diesem Abschnitt wird ein virtueller Computer namens `myVM` erstellt und die virtuelle Netzwerkadapterkarte `myNIC` angefügt. Ersetzen Sie den Platzhalter „&lt;your-key-data>“ durch die Daten Ihres eigenen öffentlichen Schlüssels.

```yaml
- name: Create VM
  azure_rm_virtualmachine:
    resource_group: myResourceGroup
    name: myVM
    vm_size: Standard_DS1_v2
    admin_username: azureuser
    ssh_password_enabled: false
    ssh_public_keys:
      - path: /home/azureuser/.ssh/authorized_keys
        key_data: <your-key-data>
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.5'
      version: latest
```

## <a name="complete-sample-ansible-playbook"></a>Vollständiges Ansible-Beispielplaybook

Dieser Abschnitt enthält das gesamte Ansible-Beispielplaybook, das Sie im Rahmen dieses Artikels erstellt haben. 

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: myResourceGroup
      location: eastus
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
    register: output_ip_address
  - name: Dump public IP for VM which will be created
    debug:
      msg: "The public IP is {{ output_ip_address.state.ip_address }}."
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys:
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: <your-key-data>
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

## <a name="run-the-sample-ansible-playbook"></a>Ausführen des Ansible-Beispielplaybooks

In diesem Abschnitt wird die Ausführung des in diesem Artikel vorgestellten Ansible-Beispielplaybooks Schritt für Schritt erläutert.

1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Öffnen Sie [Cloud Shell](/azure/cloud-shell/overview).

1. Erstellen Sie für Ihr Playbook eine Datei namens `azure_create_complete_vm.yml`, und öffnen Sie sie wie folgt im vi-Editor:

  ```azurecli-interactive
  vi azure_create_complete_vm.yml
  ```

1. Drücken Sie die **I-TASTE**, um den Einfügemodus zu starten.

1. Fügen Sie das [vollständige Ansible-Beispielplaybook](#complete-sample-ansible-playbook) in den Editor ein.

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Führen Sie das Ansible-Beispielplaybook aus.

  ```bash
  ansible-playbook azure_create_complete_vm.yml
  ```

1. Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass erfolgreich ein virtueller Computer erstellt wurde:

  ```bash
  PLAY [Create Azure VM] ****************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Create resource group] *********************************************
  changed: [localhost]

  TASK [Create virtual network] *********************************************
  changed: [localhost]

  TASK [Add subnet] *********************************************************
  changed: [localhost]

  TASK [Create public IP address] *******************************************
  changed: [localhost]

  TASK [Dump public IP for VM which will be created] ********************************************************************
  ok: [localhost] => {
      "msg": "The public IP is <ip-address>."
  }

  TASK [Create Network Security Group that allows SSH] **********************
  changed: [localhost]

  TASK [Create virtual network inteface card] *******************************
  changed: [localhost]

  TASK [Create VM] **********************************************************
  changed: [localhost]

  PLAY RECAP ****************************************************************
  localhost                  : ok=8    changed=7    unreachable=0    failed=0
  ```

1. Für den Zugriff auf den virtuellen Linux-Computer wird der SSH-Befehl verwendet. Ersetzen Sie den Platzhalter „&lt;ip-address>“ durch die IP-Adresse aus dem vorherigen Schritt.

  ```bash
  ssh azureuser@<ip-address>
  ```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Verwalten eines virtuellen Linux-Computers in Azure mithilfe von Ansible](./ansible-manage-linux-vm.md)
