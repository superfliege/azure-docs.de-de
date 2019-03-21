---
title: Automatisches Skalieren einer VM-Skalierungsgruppe in Azure mit Ansible
description: Informationen zum Verwenden von Ansible zum Skalieren einer VM-Skalierungsgruppe in Azure mit Autoskalierung
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, skalieren, autoskalierung, virtueller computer, vm-skalierungsgruppen, vmss
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/10/2018
ms.openlocfilehash: 578ad3207f62e74805be056ca11d3bd9b46513da
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792428"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-azure-using-ansible"></a>Automatisches Skalieren einer VM-Skalierungsgruppe in Azure mit Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre VM-Skalierungsgruppe in Azure wie jede andere Azure-Ressource verwalten. 

Beim Erstellen einer Skalierungsgruppe definieren Sie die Anzahl von VM-Instanzen, die Sie ausführen möchten. Wenn sich die Nachfrage nach Ihrer Anwendung ändert, können Sie die Anzahl von VM-Instanzen automatisch erhöhen oder verringern lassen. Dank der Möglichkeit zum automatischen Skalieren können Sie über den gesamten Lebenszyklus Ihrer App die Kundennachfrage decken oder auf Änderungen der Anwendungsleistung reagieren. In diesem Artikel erstellen Sie eine Einstellung für die automatische Skalierung und ordnen sie einer vorhandene VM-Skalierungsgruppe zu. In der Einstellung für die automatische Skalierung können Sie eine Regel konfigurieren, die das horizontal Herunter- und Hochskalieren ermöglicht.

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- Eine vorhandene Azure-VM-Skalierungsgruppe. - Wenn Sie keine haben, [Erstellen Sie VM-Skalierungsgruppen mit Ansible in Azure](https://docs.microsoft.com/azure/ansible/ansible-create-configure-vmss).

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.7 erforderlich. 

## <a name="auto-scale-based-on-a-schedule"></a>Automatisches Skalieren basierend auf einem Zeitplan   
Zum Aktivieren der automatischen Skalierung für eine Skalierungsgruppe legen Sie zuerst ein Profil für die automatische Skalierung fest. Dieses Profil definiert die standardmäßige, minimale und maximale Kapazität der Skalierungsgruppe. Diese Grenzwerte sind hilfreich bei der Kostenkontrolle und sorgen nicht nur dafür, dass nicht fortlaufend VM-Instanzen erstellt werden, sondern gewährleisten auch ein ausgewogenes Verhältnis zwischen angemessener Leistung und einer minimalen Anzahl von Instanzen, die beim horizontalen Herunterskalieren erhalten bleiben. 

Sie können VM-Skalierungsgruppen horizontal herunter- und hochskalieren – entweder nach einem sich wiederholenden Zeitplan oder zu einem bestimmten Datum. Dieser Abschnitt stellt ein Beispiel für ein Ansible-Playbook vor, das eine Autoskalierungseinstellung erstellt, die die Anzahl der VM-Instanzen in Ihren Skalierungsgruppen jeden Montag um 10:00 Uhr Pacific Standard Time (PST) auf drei erhöht. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks: 
    - name: Create auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         target:
           namespace: "Microsoft.Compute"
           types: "virtualMachineScaleSets"
           name: "{{ vmss_name }}"
         enabled: true
         profiles:
         - count: '3'
           min_count: '3'
           max_count: '3'
           name: Auto created scale condition
           recurrence_timezone: Pacific Standard Time
           recurrence_frequency: Week
           recurrence_days:
              - Monday
           recurrence_mins:
              - '0'
           recurrence_hours:
              - '10'
```

Speichern Sie dieses Playbook als *vmss-auto-scale.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="auto-scale-based-on-performance-data"></a>Automatische Skalierung basierend auf Leistungsdaten
Wenn sich die Nachfrage für Ihre Anwendung erhöht, erhöht sich auch die Last für die VM-Instanzen in Ihren Skalierungsgruppen. Falls es sich um eine dauerhafte Last und nicht nur um eine kurzzeitige höhere Nachfrage handelt, können Sie die Regeln für die automatische Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu erhöhen. Nachdem diese VM-Instanzen erstellt und Ihre Anwendungen bereitgestellt wurden, beginnt die Skalierungsgruppe damit, über das Lastenausgleichsmodul Datenverkehr darauf zu verteilen. Sie steuern, welche Metriken überwacht werden, z.B. CPU oder Datenträger, wie lange die Anwendungslast einen bestimmten Schwellenwert einhalten muss und wie viele VM-Instanzen der Skalierungsgruppe hinzugefügt werden sollen.

Sie können VM-Skalierungsgruppen basierend auf Leistungsmetrik-Schwellenwerten horizontal herunter- und hochskalieren – entweder nach einem sich wiederholenden Zeitplan oder zu einem bestimmten Datum. Dieser Abschnitt stellt ein Beispiel für ein Ansible-Playbook vor, das die Workloads in den letzten 10 Minuten jeden Montag um 18:00 Uhr, Pacific Standard Time (PST), überprüft und die Anzahl der VM-Instanzen in Ihren Skalierungsgruppen auf vier horizontal hochskaliert bzw. gemäß CPU-Prozentsatzmetrik auf eine Instanz horizontal herunterskaliert. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get VMSS resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create auto scaling
    azure_rm_autoscale:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      target: "{{ vmss_id }}"
      enabled: true
      profiles:
      - count: '1'
        max_count: '1'
        min_count: '1'
        name: 'This scale condition is executed when none of the other scale condition(s) match'
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
      - count: '1'
        min_count: '1'
        max_count: '4'
        name: Auto created scale condition
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
        rules:
          - cooldown: 5
            direction: Increase
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: GreaterThan
            statistic: Average
            threshold: 70
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
          - cooldown: 5
            direction: Decrease
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: LessThan
            statistic: Average
            threshold: 30
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
```

Speichern Sie dieses Playbook als *vmss-auto-scale-metrics.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-information-for-existing-autoscale-settings"></a>Abrufen von Informationen für die vorhandenen Autoskalierungseinstellungen
Die Details zur Autoskalierungseinstellung können Sie über das *azure_rm_autoscale_facts*-Modul mit dem Playbook wie folgt abrufen:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve auto scale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

## <a name="disable-the-autoscale-settings"></a>Deaktivieren der Autoskalierungseinstellungen
Sie können die Autoskalierungseinstellung deaktivieren, indem Sie `enabled: true` in `enabled: false` ändern, oder die Autoskalierungseinstellungen mit dem Playbook wie folgt löschen:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Ansible-Beispiel-Playbook für die VM-Skalierungsgruppen](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)