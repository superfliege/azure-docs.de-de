---
title: Erstellen von VM-Skalierungsgruppen mit Ansible in Azure
description: Informationen zum Erstellen und Konfigurieren einer VM-Skalierungsgruppe in Azure mit Ansible
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, vm, vm-skalierungsgruppen, vmss
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/24/2018
ms.openlocfilehash: 1176987ab318a97a7db6a12e619e7b7db06ad2da
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58097888"
---
# <a name="create-virtual-machine-scale-sets-in-azure-using-ansible"></a>Erstellen von VM-Skalierungsgruppen mit Ansible in Azure
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre VM-Skalierungsgruppe in Azure wie jede andere Azure-Ressource verwalten. In diesem Artikel erfahren Sie, wie Sie Ansible verwenden können, um eine VM-Skalierungsgruppe zu erstellen und horizontal hochzuskalieren. 

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.6 erforderlich. 

## <a name="create-a-vmss"></a>Erstellen einer VM-Skalierungsgruppe
In diesem Abschnitt finden Sie ein Beispielplaybook für Ansible, das die folgenden Ressourcen definiert:
- **Die Ressourcengruppe**, in der alle Ihre Ressourcen bereitgestellt werden.
- **Das virtuelle Netzwerk** im Adressraum 10.0.0.0/16
- **Das Subnetz** innerhalb des virtuellen Netzwerks
- **Die öffentliche IP-Adresse**, über die Sie auf Ressourcen im Internet zugreifen können
- **Die Netzwerksicherheitsgruppe**, die den Flow des ein- und ausgehenden Netzwerkdatenverkehrs des virtuellen Computers kontrolliert.
- **Das Lastenausgleichsmodul**, das den Datenverkehr auf der Grundlage von Lastenausgleichsregeln auf eine Gruppe definierter VMs verteilt.
- **Die VM-Skalierungsgruppe**, die alle erstellten Ressourcen verwendet

Geben Sie Ihr Kennwort für den Wert *admin_password* ein.

  ```yml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
      vmss_lb_name: myVMSSlb
      location: eastus
      admin_username: azureuser
      admin_password: "your_password"
    tasks:
      - name: Create a resource group
        azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
      - name: Create virtual network
        azure_rm_virtualnetwork:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          address_prefixes: "10.0.0.0/16"
      - name: Add subnet
        azure_rm_subnet:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          address_prefix: "10.0.1.0/24"
          virtual_network: "{{ vmss_name }}"
      - name: Create public IP address
        azure_rm_publicipaddress:
          resource_group: "{{ resource_group }}"
          allocation_method: Static
          name: "{{ vmss_name }}"
      - name: Create Network Security Group that allows SSH
        azure_rm_securitygroup:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          rules:
            - name: SSH
              protocol: Tcp
              destination_port_range: 22
              access: Allow
              priority: 1001
              direction: Inbound

      - name: Create a load balancer
        azure_rm_loadbalancer:
          name: "{{ vmss_lb_name }}"
          location: "{{ location }}"
          resource_group: "{{ resource_group }}"
          public_ip: "{{ vmss_name }}"
          probe_protocol: Tcp
          probe_port: 8080
          probe_interval: 10
          probe_fail_count: 3
          protocol: Tcp
          load_distribution: Default
          frontend_port: 80
          backend_port: 8080
          idle_timeout: 4
          natpool_frontend_port_start: 50000
          natpool_frontend_port_end: 50040
          natpool_backend_port: 22
          natpool_protocol: Tcp

      - name: Create VMSS
        azure_rm_virtualmachine_scaleset:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          vm_size: Standard_DS1_v2
          admin_username: "{{ admin_username }}"
          admin_password: "{{ admin_password }}"
          ssh_password_enabled: true
          capacity: 2
          virtual_network_name: "{{ vmss_name }}"
          subnet_name: "{{ vmss_name }}"
          upgrade_policy: Manual
          tier: Standard
          managed_disk_type: Standard_LRS
          os_disk_caching: ReadWrite
          image:
            offer: UbuntuServer
            publisher: Canonical
            sku: 16.04-LTS
            version: latest
          load_balancer: "{{ vmss_lb_name }}"
          data_disks:
            - lun: 0
              disk_size_gb: 20
              managed_disk_type: Standard_LRS
              caching: ReadOnly
            - lun: 1
              disk_size_gb: 30
              managed_disk_type: Standard_LRS
              caching: ReadOnly
  ```

Speichern Sie das vorherige Playbook als `vmss-create.yml`-Datei, oder [laden Sie das Beispielplaybook für Ansible herunter](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml), um die Umgebung für die VM-Skalierungsgruppe mit Ansible zu erstellen.

Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

  ```bash
  ansible-playbook vmss-create.yml
  ```

Wenn das Playbook ausgeführt wurde, zeigt eine der Beispielausgabe ähnelnde Ausgabe an, dass die VM-Skalierungsgruppe erfolgreich erstellt wurde:

  ```Output
  PLAY [localhost] ***********************************************************

  TASK [Gathering Facts] *****************************************************
  ok: [localhost]

  TASK [Create a resource group] ****************************************************************************
  changed: [localhost]

  TASK [Create virtual network] ****************************************************************************
  changed: [localhost]

  TASK [Add subnet] **********************************************************
  changed: [localhost]

  TASK [Create public IP address] ****************************************************************************
  changed: [localhost]

  TASK [Create Network Security Group that allows SSH] ****************************************************************************
  changed: [localhost]

  TASK [Create a load balancer] ****************************************************************************
  changed: [localhost]

  TASK [Create VMSS] *********************************************************
  changed: [localhost]

  PLAY RECAP *****************************************************************
  localhost                  : ok=8    changed=7    unreachable=0    failed=0

  ```

## <a name="scale-out-a-vmss"></a>Horizontales Hochskalieren einer VM-Skalierungsgruppe
Die erstellte VM-Skalierungsgruppe verfügt über zwei Instanzen. Wenn Sie im Azure-Portal zu der VM-Skalierungsgruppe navigieren, wird Ihnen **Standard_DS1_v2 (2 Instanzen)** angezeigt. Sie können auch [Azure Cloud Shell](https://shell.azure.com/) verwenden, indem Sie den folgenden Befehl in Cloud Shell ausführen:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Die Ergebnisse sehen in etwa wie in der folgenden Ausgabe aus:

  ```bash
  {
    "capacity": 2,
  }
  ```

Skalieren Sie nun von zwei auf drei Instanzen. Der folgende Code für das Ansible-Playbook ruft Informationen zur VM-Skalierungsgruppe ab und skaliert deren Kapazität von zwei auf drei. 

  ```yml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
    tasks: 
      - name: Get scaleset info
        azure_rm_virtualmachine_scaleset_facts:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          format: curated
        register: output_scaleset

      - name: Dump scaleset info
        debug:
          var: output_scaleset

      - name: Modify scaleset (change the capacity to 3)
        set_fact:
          body: "{{ output_scaleset.ansible_facts.azure_vmss[0] | combine({'capacity': 3}, recursive=True) }}"

      - name: Update something in that VMSS
        azure_rm_virtualmachine_scaleset: "{{ body }}"
  ```

Wenn Sie die von Ihnen erstellte VM-Skalierungsgruppe horizontal hochskalieren möchten, speichern Sie das vorherige Playbook als `vmss-scale-out.yml`-Datei, oder [laden Sie das Beispielplaybook für Ansible herunter](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml). 

Führen Sie das Playbook mit dem folgenden Befehl aus:

  ```bash
  ansible-playbook vmss-scale-out.yml
  ```

Die Ausgabe der Ausführung des Ansible-Playbooks zeigt, dass die VM-Skalierungsgruppe erfolgreich horizontal hochskaliert wurde:

  ```Output
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get scaleset info] ***************************************************************************
  ok: [localhost]

  TASK [Dump scaleset info] ***************************************************************************
  ok: [localhost] => {
      "output_scaleset": {
          "ansible_facts": {
              "azure_vmss": [
                  {
                      ......
                  }
              ]
          },
          "changed": false,
          "failed": false
      }
  }

  TASK [Modify scaleset (set upgradePolicy to Automatic and capacity to 3)] ***************************************************************************
  ok: [localhost]

  TASK [Update something in that VMSS] ***************************************************************************
  changed: [localhost]

  PLAY RECAP ****************************************************************
  localhost                  : ok=5    changed=1    unreachable=0    failed=0
  ```

Wenn Sie im Azure-Portal zu der von Ihnen konfigurierten VM-Skalierungsgruppe navigieren, wird Ihnen **Standard_DS1_v2 (3 Instanzen)** angezeigt. Sie können die Änderung auch mithilfe von [Azure Cloud Shell](https://shell.azure.com/) überprüfen, indem Sie den folgenden Befehl ausführen:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Die Ergebnisse der Befehlsausführung in Cloud Shell zeigen, dass anschließend drei Instanzen vorhanden sind. 

  ```bash
  {
    "capacity": 3,
  }
  ```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Bereitstellen von Anwendungen für VM-Skalierungsgruppen mithilfe von Ansible](https://docs.microsoft.com/azure/ansible/ansible-deploy-app-vmss)
> 
> [Automatisches Skalieren einer VM-Skalierungsgruppe mit Ansible](https://docs.microsoft.com/azure/ansible/ansible-auto-scale-vmss)