---
title: Bereitstellen von Anwendungen für VM-Skalierungsgruppen in Azure mithilfe von Ansible
description: Erfahren Sie, wie Sie Ansible zum Konfigurieren einer VM-Skalierungsgruppe und Bereitstellen der Anwendung auf der VM-Skalierungsgruppe in Azure verwenden
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, vm, vm-skalierungsgruppen, vmss
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/11/2018
ms.openlocfilehash: 2214dd9505dff86ac26f01967a360140dee0069f
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791731"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Bereitstellen von Anwendungen für VM-Skalierungsgruppen in Azure mithilfe von Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mithilfe von Ansible Ihre Anwendungen in Azure bereitstellen. In diesem Artikel wird veranschaulicht, wie Sie eine Java-Anwendung in einer Azure-VM-Skalierungsgruppe (VMSS) bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **VM-Skalierungsgruppe**: Wenn Sie noch keine VM-Skalierungsgruppe festgelegt haben, können Sie [eine VM-Skalierungsgruppe mit Ansible erstellen](ansible-create-configure-vmss.md).
- **Git** - [Git](https://git-scm.com) wird verwendet, um ein in diesem Tutorial verwendetes Java-Beispiel herunterzuladen.
- **Java SE Development Kit (JDK)**: Mit dem [JDK](https://aka.ms/azure-jdks) wird das Java-Beispielprojekt erstellt.
- **Buildtools für Apache Maven**: Die [Buildtools für Apache Maven](https://maven.apache.org/download.cgi) werden verwendet, um das Java-Beispielprojekt zu erstellen.

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.6 erforderlich.

## <a name="get-host-information"></a>Abrufen der Hostinformationen

In diesem Abschnitt wird veranschaulicht, wie Ansible zum Abrufen der Hostinformationen für eine Gruppe von virtuellen Azure-Computern verwendet wird. Weiter unten finden Sie ein Ansible-Beispielplaybook. Der Code ruft die öffentlichen IP-Adressen und Load Balancer in der angegebenen Ressourcengruppe ab und erstellt die Hostgruppe **saclesethosts** im Bestand.

Speichern Sie das folgende Beispielplaybook als `get-hosts-tasks.yml`:

  ```yml
  - name: Get facts for all Public IPs within a resource groups
    azure_rm_publicipaddress_facts:
      resource_group: "{{ resource_group }}"
    register: output_ip_address

  - name: Get loadbalancer info
    azure_rm_loadbalancer_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ loadbalancer_name }}"
    register: output

  - name: Add all hosts
    add_host:
      groups: scalesethosts
      hostname: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}_{{ item.properties.frontendPort }}"
      ansible_host: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}"
      ansible_port: "{{ item.properties.frontendPort }}"
      ansible_ssh_user: "{{ admin_username }}"
      ansible_ssh_pass: "{{ admin_password }}"
    with_items:
      - "{{ output.ansible_facts.azure_loadbalancers[0].properties.inboundNatRules }}"
  ```

## <a name="prepare-an-application-for-deployment"></a>Vorbereiten einer Anwendung für die Bereitstellung

In diesem Abschnitt verwenden Sie Git, um ein Java-Beispielprojekt von GitHub zu klonen, und erstellen das Projekt. Speichern Sie das folgende Playbook als `app.yml`:

  ```yml
  - hosts: localhost
    vars:
      repo_url: https://github.com/spring-guides/gs-spring-boot.git
      workspace: ~/src/helloworld

    tasks:
    - name: Git Clone sample app
      git:
        repo: "{{ repo_url }}"
        dest: "{{ workspace }}"

    - name: Build sample app
      shell: mvn package chdir="{{ workspace }}/complete"
  ```

Führen Sie das Ansible-Beispielplaybook mit dem folgenden Befehl aus:

  ```bash
  ansible-playbook app.yml
  ```

Die Ausgabe des Befehls „ansible-playbook“ ähnelt der Folgenden, und Sie sehen, dass die von GitHub geklonte Beispiel-App erstellt wurde:

  ```Output
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Git Clone sample app] ***************************************************************************
  changed: [localhost]

  TASK [Build sample app] ***************************************************
  changed: [localhost]

  PLAY RECAP ***************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-vmss"></a>Bereitstellen der Anwendung für VMSS

Im folgenden Abschnitt eines Ansible-Playbooks wird die JRE (Java Runtime Environment) in der Hostgruppe **saclesethosts** installiert, und die Java-Anwendung wird in der Hostgruppe **saclesethosts** bereitgestellt:

(Ändern Sie `admin_password` in Ihr eigenes Kennwort.)

  ```yml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      scaleset_name: myVMSS
      loadbalancer_name: myVMSSlb
      admin_username: azureuser
      admin_password: "your_password"
    tasks:
    - include: get-hosts-tasks.yml

  - name: Install JRE on VMSS
    hosts: scalesethosts
    become: yes
    vars:
      workspace: ~/src/helloworld
      admin_username: azureuser

    tasks:
    - name: Install JRE
      apt:
        name: default-jre
        update_cache: yes

    - name: Copy app to Azure VM
      copy:
        src: "{{ workspace }}/complete/target/gs-spring-boot-0.1.0.jar"
        dest: "/home/{{ admin_username }}/helloworld.jar"
        force: yes
        mode: 0755

    - name: Start the application
      shell: java -jar "/home/{{ admin_username }}/helloworld.jar" >/dev/null 2>&1 &
      async: 5000
      poll: 0
  ```

Sie können das vorstehende Ansible-Beispielplaybook als `vmss-setup-deploy.yml` speichern oder [das gesamte Beispielplaybook herunterladen](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss).

Um den Verbindungstyp „SSH“ mit Kennwörtern zu verwenden, müssen Sie das Programm SSHPASS installieren.
  - Führen Sie für Ubuntu 16.04 den Befehl `apt-get install sshpass` aus.
  - Führen Sie für CentOS 7.4 den Befehl `yum install sshpass` aus.

Möglicherweise wird eine Fehlermeldung angezeigt, die der folgenden ähnelt: **Verwendung eines SSH-Kennworts anstelle eines Schlüssels ist nicht möglich, da die Hostschlüsselüberprüfung aktiviert ist, aber nicht von SSHPASS unterstützt wird. Fügen Sie den Fingerabdruck dieses Hosts Ihrer Datei „known_hosts“ hinzu, um diesen Host zu verwalten.** Wenn dieser Fehler angezeigt wird, können Sie die Hostschlüsselüberprüfung deaktivieren, indem Sie der Datei `/etc/ansible/ansible.cfg` oder der Datei `~/.ansible.cfg` die folgende Zeile hinzufügen:
  ```bash
  [defaults]
  host_key_checking = False
  ```

Führen Sie das Playbook mit dem folgenden Befehl aus:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

Die Ausgabe der Ausführung des Befehls „ansible-playbook“ gibt an, dass die Java-Beispielanwendung auf der Hostgruppe der VM-Skalierungsgruppe installiert wurde:

  ```Output
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups] **********************************************
  ok: [localhost]

  TASK [Get loadbalancer info] ****************************************************************************
  ok: [localhost]

  TASK [Add all hosts] *****************************************************************************
  changed: [localhost] ...

  PLAY [Install JRE on VMSS] *****************************************************************************

  TASK [Gathering Facts] *****************************************************************************
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM] *****************************************************************************
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application] ********************************************************************
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP ************************************************************************************************
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

Glückwunsch! Ihre Anwendung wird jetzt in Azure ausgeführt. Sie können nun zur URL des Load Balancers für Ihre VM-Skalierungsgruppe navigieren:

![Die Java-App wird in einer VM-Skalierungsgruppe in Azure ausgeführt.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Automatisches Skalieren einer VM-Skalierungsgruppe mit Ansible](https://docs.microsoft.com/azure/ansible/ansible-auto-scale-vmss)
