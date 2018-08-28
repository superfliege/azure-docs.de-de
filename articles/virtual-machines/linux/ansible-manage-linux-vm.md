---
title: Verwalten eines virtuellen Linux-Computers in Azure mithilfe von Ansible
description: Hier erfahren Sie, wie Sie mithilfe von Ansible einen virtuellen Linux-Computer in Azure verwalten.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 66106346b298fae22cce47081916a6c8eec8fd40
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250648"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Verwalten eines virtuellen Linux-Computers in Azure mithilfe von Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre virtuellen Azure-Computer wie jede andere Ressource verwalten. In diesem Artikel wird veranschaulicht, wie Sie mithilfe eines Ansible-Playbooks einen virtuellen Linux-Computer starten und beenden. 

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen.

- **Konfigurieren von Azure Cloud Shell** oder **Installieren und Konfigurieren von Ansible auf einem virtuellen Linux-Computer**

  **Konfigurieren von Azure Cloud Shell**

  1. **Konfigurieren von Azure Cloud Shell:** Sollten Sie noch nicht mit Azure Cloud Shell vertraut sein, finden Sie im Artikel [Schnellstart für Bash in Azure Cloud Shell](/azure/cloud-shell/quickstart) Informationen zu den ersten Schritten mit Cloud Shell sowie zur Konfiguration. 

  1. **Virtueller Linux-Computer:** Sollten Sie keinen Zugriff auf einen virtuellen Linux-Computer haben, können Sie [mit Ansible einen virtuellen Computer erstellen](ansible-create-vm.md).

  **ODER**

  **Installieren und Konfigurieren von Ansible auf einem virtuellen Linux-Computer**

  1. **Installieren von Ansible:** Installieren Sie Ansible auf einer [unterstützten Linux-Plattform](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine).

  1. **Konfigurieren von Ansible** - [Erstellen von Azure-Anmeldeinformationen und Konfigurieren von Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Aufheben der Zuordnung (Beenden) eines virtuellen Azure-Computers mithilfe von Ansible
In diesem Abschnitt wird erläutert, wie Sie mithilfe von Ansible die Zuordnung eines virtuellen Computers aufheben (beenden).

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Öffnen Sie [Cloud Shell](/azure/cloud-shell/overview).

1. Erstellen Sie für Ihr Playbook eine Datei namens `azure_vm_stop.yml`, und öffnen Sie sie wie folgt im vi-Editor:

  ```azurecli-interactive
  vi azure_vm_stop.yml
  ```

1. Drücken Sie die **I-TASTE**, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Beispielcode in den Editor ein:

    ```yaml
    - name: Stop Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
            allocated: no 
    ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Führen Sie das Ansible-Beispielplaybook aus.

  ```bash
  ansible-playbook azure_vm_stop.yml
  ```

1. Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass die Zuordnung des (beendeten) virtuellen Computers aufgehoben wurde:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>Verwenden von Ansible zum Starten eines (beendeten) virtuellen Azure-Computers, dessen Zuordnung aufgehoben wurde
In diesem Abschnitt wird erläutert, wie Sie mithilfe von Ansible einen (beendeten) virtuellen Computer starten, dessen Zuordnung aufgehoben wurde.

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Öffnen Sie [Cloud Shell](/azure/cloud-shell/overview).

1. Erstellen Sie für Ihr Playbook eine Datei namens `azure_vm_start.yml`, und öffnen Sie sie wie folgt im vi-Editor:

  ```azurecli-interactive
  vi azure_vm_start.yml
  ```

1. Drücken Sie die **I-TASTE**, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Beispielcode in den Editor ein:

    ```yaml
    - name: Start Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Start the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
    ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Führen Sie das Ansible-Beispielplaybook aus.

  ```bash
  ansible-playbook azure_vm_start.yml
  ```

1. Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass der virtuelle Computer gestartet wurde:

    Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass der virtuelle Computer gestartet wurde:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Verwalten dynamischer Azure-Bestände mithilfe von Ansible](../../ansible/ansible-manage-azure-dynamic-inventories.md)