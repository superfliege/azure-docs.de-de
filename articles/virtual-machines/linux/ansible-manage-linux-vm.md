---
title: Verwalten eines virtuellen Linux-Computers in Azure mithilfe von Ansible
description: Hier erfahren Sie, wie Sie mithilfe von Ansible einen virtuellen Linux-Computer in Azure verwalten.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 09/27/2018
ms.openlocfilehash: 06a0f2416ecff9e2f9a21877e26487b56452d0a0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251320"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Verwalten eines virtuellen Linux-Computers in Azure mithilfe von Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre virtuellen Azure-Computer wie jede andere Ressource verwalten. In diesem Artikel wird veranschaulicht, wie Sie mithilfe eines Ansible-Playbooks einen virtuellen Linux-Computer starten und beenden. 

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Aufheben der Zuordnung (Beenden) eines virtuellen Azure-Computers mithilfe von Ansible
In diesem Abschnitt wird erläutert, wie Sie mithilfe von Ansible die Zuordnung eines virtuellen Computers aufheben (beenden).

1.  Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1.  Öffnen Sie [Cloud Shell](/azure/cloud-shell/overview).

1.  Erstellen Sie für Ihr Playbook eine Datei namens `azure-vm-stop.yml`, und öffnen Sie sie wie folgt im vi-Editor:

    ```azurecli-interactive
    vi azure-vm-stop.yml
    ```

1.  Drücken Sie die **I-TASTE**, um den Einfügemodus zu starten.

1.  Fügen Sie den folgenden Beispielcode in den Editor ein:

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

1.  Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1.  Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1.  Führen Sie das Ansible-Beispielplaybook aus.

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1.  Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass die Zuordnung des (beendeten) virtuellen Computers aufgehoben wurde:

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

1.  Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1.  Öffnen Sie [Cloud Shell](/azure/cloud-shell/overview).

1.  Erstellen Sie für Ihr Playbook eine Datei namens `azure-vm-start.yml`, und öffnen Sie sie wie folgt im vi-Editor:

    ```azurecli-interactive
    vi azure-vm-start.yml
    ```

1.  Drücken Sie die **I-TASTE**, um den Einfügemodus zu starten.

1.  Fügen Sie den folgenden Beispielcode in den Editor ein:

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

1.  Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1.  Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1.  Führen Sie das Ansible-Beispielplaybook aus.

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1.  Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass der virtuelle Computer gestartet wurde:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Verwalten dynamischer Azure-Bestände mithilfe von Ansible](/articles/ansible/ansible-manage-azure-dynamic-inventories)