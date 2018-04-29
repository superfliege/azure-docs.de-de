---
title: Ausführen von Ansible mit Bash in Azure Cloud Shell
description: Hier erfahren Sie, wie Sie verschiedene Ansible-Aufgaben mit Bash in Azure Cloud Shell ausführen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Bash
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 02/01/2018
ms.topic: article
ms.openlocfilehash: 9fe65f4cf10119002bcb7a3855d112d850e20f1a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Ausführen von Ansible mit Bash in Azure Cloud Shell

In diesem Tutorial erfahren Sie, wie Sie verschiedene Ansible-Aufgaben von Bash aus in Cloud Shell ausführen. Diese Aufgaben umfassen das Herstellen einer Verbindung mit einem virtuellen Computer sowie das Erstellen von Ansible-Playbooks zum Erstellen und Löschen einer Azure-Ressourcengruppe.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

- **Azure-Anmeldeinformationen** - [Erstellen von Azure-Anmeldeinformationen und Konfigurieren von Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

- **Konfigurieren von Azure Cloud Shell:** Sollten Sie noch nicht mit Azure Cloud Shell vertraut sein, finden Sie im Artikel [Schnellstart für Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) Informationen zu den ersten Schritten mit Cloud Shell sowie zur Konfiguration. Starten Sie hier eine dedizierte Website für Cloud Shell:

[![Cloud Shell starten](https://shell.azure.com/images/launchcloudshell.png "Cloud Shell starten")](https://shell.azure.com)

## <a name="use-ansible-to-connect-to-a-vm"></a>Herstellen einer Verbindung mit einem virtuellen Computer mithilfe von Ansible
Ansible hat ein Python-Skript namens [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) erstellt, das API-Anforderungen an Azure Resource Manager richtet, um einen dynamischen Bestand Ihrer Azure-Ressourcen zu generieren. Die folgende Anleitung zeigt Schritt für Schritt, wie Sie unter Verwendung des Skripts `azure_rm.py` eine Verbindung mit einem virtuellen Azure-Computer herstellen:

1. Öffnen Sie Bash in Cloud Shell. Der Shell-Typ wird auf der linken Seite des Cloud Shell-Fensters angegeben.

1. Sollten Sie über keinen verwendbaren virtuellen Computer verfügen, geben Sie in Cloud Shell die folgenden Befehle ein, um einen virtuellen Computer zu Testzwecken zu erstellen:

  ```azurecli-interactive
  az group create --resource-group ansible-test-rg --location eastus
  ```

  ```azurecli-interactive
  az vm create --resource-group ansible-test-rg --name ansible-test-vm --image UbuntuLTS --generate-ssh-keys
  ```

1. Rufen Sie mithilfe des GNU-Befehls `wget` das Skript `azure_rm.py` ab:

  ```azurecli-interactive
  wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
  ```

1. Ändern Sie mithilfe des Befehls `chmod` die Zugriffsberechtigungen für das Skript `azure_rm.py`. Im folgenden Befehl wird der Parameter `+x` verwendet, um die Ausführung der angegebenen Datei (`azure_rm.py`) zuzulassen:

  ```azurecli-interactive
  chmod +x azure_rm.py
  ```

1. Verwenden Sie den [ansible-Befehl](https://docs.ansible.com/ansible/2.4/ansible.html), um eine Verbindung mit Ihrem virtuellen Computer herzustellen: 

  ```azurecli-interactive
  ansible -i azure_rm.py ansible-test-vm -m ping
  ```

  Nach der Verbindungsherstellung sollten Ergebnisse angezeigt werden. Diese sehen in etwa wie in der folgenden Ausgabe aus:

  ```Output
  The authenticity of host 'nn.nnn.nn.nn (nn.nnn.nn.nn)' can't be established.
  ECDSA key fingerprint is SHA256:&lt;some value>.
  Are you sure you want to continue connecting (yes/no)? yes
  test-ansible-vm | SUCCESS => {
      "changed": false,
      "failed": false,
      "ping": "pong"
  }
  ```

1. Falls Sie in diesem Abschnitt eine Ressourcengruppe und einen virtuellen Computer erstellt haben, führen Sie den folgenden Befehl aus:

  ```azurecli-interactive
  az group delete -n <resourceGroup>
  ```

## <a name="run-a-playbook-in-cloud-shell"></a>Ausführen eines Playbooks in Cloud Shell
Der Befehl [ansible-playbook](https://docs.ansible.com/ansible/2.4/ansible-playbook.html) dient zum Ausführen von Ansible-Playbooks, wodurch die Aufgaben auf den entsprechenden Hosts ausgeführt werden. In diesem Abschnitt erfahren Sie Schritt für Schritt, wie Sie über Cloud Shell zwei Playbooks (eins zum Erstellen einer Ressourcengruppe und eins zum Löschen der Ressourcengruppe) erstellen und ausführen. 

1. Erstellen Sie wie folgt eine Datei namens `rg.yml`:

  ```azurecli-interactive
  vi rg.yml
  ```

1. Kopieren Sie den folgenden Inhalt in das Cloud Shell-Fenster (in dem nun eine Instanz des VI-Editors gehostet wird):

  ```yml
  - name: My first Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: demoresourcegroup
        location: eastus
  ```

1. Speichern Sie die Datei, und verlassen Sie den VI-Editor, indem Sie `:wq` eingeben und die EINGABETASTE drücken.

1. Führen Sie das Playbook `rg.yml` mithilfe des Befehls `ansible-playbook` aus:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Die Ergebnisse sollten in etwa wie in der folgenden Ausgabe aussehen:

  ```Output
  PLAY [My first Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Create a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

1. Überprüfen Sie die Bereitstellung:

  ```azurecli-interactive
  az group show -n demoresourcegroup
  ```

1. Erstellen Sie nach Erstellung der Ressourcengruppe ein zweites Ansible-Playbook zum Löschen der Ressourcengruppe:

  ```azurecli-interactive
  vi rg2.yml
  ```

1. Kopieren Sie den folgenden Inhalt in das Cloud Shell-Fenster (in dem nun eine Instanz des VI-Editors gehostet wird):

  ```yml
  - name: My second Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: demoresourcegroup
        state: absent
  ```

1. Speichern Sie die Datei, und verlassen Sie den VI-Editor, indem Sie `:wq` eingeben und die EINGABETASTE drücken.

1. Führen Sie das Playbook `rg2.yml` mithilfe des Befehls `ansible-playbook` aus:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Die Ergebnisse sollten in etwa wie in der folgenden Ausgabe aussehen:

  ```Output
  The output is as following. 
  PLAY [My second Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Delete a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Erstellen eines einfachen virtuellen Computers in Azure mit Ansible](/azure/virtual-machines/linux/ansible-create-vm)
