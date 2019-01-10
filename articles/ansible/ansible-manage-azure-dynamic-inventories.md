---
title: Verwalten dynamischer Azure-Bestände mithilfe von Ansible
description: Hier erfahren Sie, wie Sie Ihre dynamischen Azure-Bestände mithilfe von Ansible verwalten.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, dynamischer Bestand
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: bdaf53728fb54114a41f9454fa3f6057ae042136
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54053992"
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Verwalten dynamischer Azure-Bestände mithilfe von Ansible
Mit Ansible können Bestandsinformationen aus verschiedenen Quellen (einschließlich Cloudquellen wie Azure) in einen *dynamischen Bestand* abgerufen werden. In diesem Artikel verwenden Sie [Azure Cloud Shell](./ansible-run-playbook-in-cloudshell.md), um einen dynamischen Azure-Bestand von Ansible zu konfigurieren, in dem Sie zwei virtuelle Computer erstellen, einen der beiden virtuellen Computer markieren und NGINX auf dem markierten virtuellen Computer installieren.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

- **Azure-Anmeldeinformationen** - [Erstellen von Azure-Anmeldeinformationen und Konfigurieren von Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>Erstellen der virtuellen Testcomputer

1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Öffnen Sie [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

1. Erstellen Sie für die virtuellen Computer dieses Tutorials eine Azure-Ressourcengruppe.

    > [!IMPORTANT]  
    > Für die in diesem Schritt erstellte Azure-Ressourcengruppe muss ein Name angegeben werden, der ausschließlich aus Kleinbuchstaben besteht. Andernfalls tritt bei der Erstellung des dynamischen Bestands ein Fehler auf.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Erstellen Sie mithilfe eines der folgenden Verfahren zwei virtuelle Linux-Computer in Azure:

    - **Ansible-Playbook:** Wie Sie einen virtuellen Computer auf der Grundlage eines Ansible-Playbooks erstellen, erfahren Sie im Artikel [Erstellen eines einfachen virtuellen Computers in Azure mit Ansible](/azure/virtual-machines/linux/ansible-create-vm). Wenn Sie einen der virtuellen Computer (oder beide) mit einem Playbook definieren, verwenden Sie anstelle eines Kennworts die SSH-Verbindung.

    - **Azure-Befehlszeilenschnittstelle:** Führen Sie die folgenden Befehle in Cloud Shell aus, um die beiden virtuellen Computer zu erstellen:

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-virtual-machine"></a>Markieren eines virtuellen Computers
Sie können [Ihre Azure-Ressourcen mithilfe von Tags nach benutzerdefinierten Kategorien organisieren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli). 

Geben Sie den folgenden Befehl vom Typ [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) ein, um den virtuellen Computer `ansible-inventory-test-vm1` mit dem Schlüssel `nginx` zu markieren:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Generieren eines dynamischen Bestands
Nachdem Sie Ihre virtuellen Computer definiert (und markiert) haben, können Sie den dynamischen Bestand generieren. Ansible bietet ein Python-Skript namens [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py), das API-Anforderungen an Azure Resource Manager richtet, um einen dynamischen Bestand Ihrer Azure-Ressourcen zu generieren. Die folgende Anleitung zeigt Schritt für Schritt, wie Sie unter Verwendung des Skripts `azure_rm.py` eine Verbindung mit Ihren beiden virtuellen Azure-Testcomputern herstellen:

1. Rufen Sie mithilfe des GNU-Befehls `wget` das Skript `azure_rm.py` ab:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Ändern Sie mithilfe des Befehls `chmod` die Zugriffsberechtigungen für das Skript `azure_rm.py`. Im folgenden Befehl wird der Parameter `+x` verwendet, um die Ausführung der angegebenen Datei (`azure_rm.py`) zuzulassen:

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Verwenden Sie den [ansible-Befehl](https://docs.ansible.com/ansible/2.4/ansible.html), um eine Verbindung mit Ihrer Ressourcengruppe herzustellen: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Nach der Verbindungsherstellung werden Ergebnisse angezeigt. Diese sehen in etwa wie in der folgenden Ausgabe aus:

    ```Output
    ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ansible-inventory-test-vm2 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

## <a name="enable-the-virtual-machine-tag"></a>Aktivieren des Tags für den virtuellen Computer
Nachdem Sie das gewünschte Tag festgelegt haben, müssen Sie es aktivieren. Hierzu können Sie das Tag beispielsweise über den Befehl **export** in eine Umgebungsvariable namens `AZURE_TAGS` exportieren:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Nachdem das Tag exportiert wurde, können Sie den Befehl `ansible` erneut ausführen:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

Nun wird nur noch ein einzelner virtueller Computer angezeigt. (Dabei handelt es sich um den Computer, dessen Tag dem Wert entspricht, der in die Umgebungsvariable **AZURE_TAGS** exportiert wurde.)

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Einrichten von Nginx auf dem markierten virtuellen Computer
Das Tag ermöglicht die schnelle und einfache Verwendung von Untergruppen virtueller Computer. Ein Beispiel: Angenommen, Sie möchten Nginx nur auf virtuellen Computern installieren, denen Sie das Tag `nginx` zugewiesen haben. Gehen Sie hierzu wie folgt vor:

1. Erstellen Sie für Ihr Playbook eine Datei namens `nginx.yml`:

  ```azurecli-interactive
  vi nginx.yml
  ```

1. Fügen Sie den folgenden Code in die neu erstellte Datei `nginx.yml` ein:

    ```yml
    ---
    - name: Install and start Nginx on an Azure virtual machine
    hosts: azure
    become: yes
    tasks:
    - name: install nginx
      apt: pkg=nginx state=installed
      notify:
      - start nginx

    handlers:
    - name: start nginx
      service: name=nginx state=started
    ```

1. Führen Sie das Playbook `nginx.yml` aus:

    ```azurecli-interactive
    ansible-playbook -i azure_rm.py nginx.yml
    ```

1. Nach Ausführen des Playbooks werden Ergebnisse angezeigt. Diese sehen in etwa wie in der folgenden Ausgabe aus:

    ```Output
    PLAY [Install and start Nginx on an Azure virtual machine] **********

    TASK [Gathering Facts] **********
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] **********
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] **********
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP **********
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Testen der Nginx-Installation
Dieser Abschnitt beschreibt ein Verfahren, mit dem Sie sich vergewissern können, dass Nginx auf Ihrem virtuellen Computer installiert ist.

1. Rufen Sie mithilfe des Befehls [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) die IP-Adresse des virtuellen Computers `ansible-inventory-test-vm1` ab. Der zurückgegebene Wert (die IP-Adresse des virtuellen Computers) wird dann als Parameter für den SSH-Befehl verwendet, um die Verbindung mit dem virtuellen Computer herzustellen.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. Während eine Verbindung mit dem virtuellen Computer `ansible-inventory-test-vm1` besteht, führen Sie den Befehl [nginx -v](https://nginx.org/en/docs/switches.html) aus, um zu ermitteln, ob Nginx installiert ist.

    ```azurecli-interactive
    nginx -v
    ```

1. Nach Ausführung des Befehls `nginx -v` wird die Nginx-Version (zweite Zeile) angezeigt. Das bedeutet, dass Nginx installiert ist.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)
    
    tom@ansible-inventory-test-vm1:~$
    ```

1. Drücken Sie die Tastenkombination **&lt;STRG+D**, um die Verbindung der SSH-Sitzung zu trennen.

1. Wenn Sie die obigen Schritte für den virtuellen Computer `ansible-inventory-test-vm2` ausführen, erscheint eine Informationsmeldung, die darüber informiert, wo Sie Nginx beziehen können (was impliziert, dass es noch nicht installiert ist):

    ```Output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Erstellen eines einfachen virtuellen Computers in Azure mit Ansible](/azure/virtual-machines/linux/ansible-create-vm)
