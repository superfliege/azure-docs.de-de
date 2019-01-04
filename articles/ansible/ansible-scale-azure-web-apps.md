---
title: Skalieren von Azure App Service-Web-Apps mit Ansible
description: Hier erfahren Sie, wie Sie Ansible verwenden, um eine Web-App mit Java 8 und der Tomcat-Containerruntime in App Service unter Linux zu erstellen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, Playbook, Azure App Service, Web-App, skalieren, Java
author: tomarcher
manager: jeconnoc
ms.author: kyliel
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 740ff6d6a636377f9d58a5231692c87f935ae6d2
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601864"
---
# <a name="scale-azure-app-service-web-apps-by-using-ansible"></a>Skalieren von Azure App Service-Web-Apps mit Ansible
[Azure App Service-Web-Apps](https://docs.microsoft.com/azure/app-service/overview) (oder einfach Web-Apps) dient zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends. Sie können in Ihrer bevorzugten Sprache entwickeln: .NET, .NET Core, Java, Ruby, Node.js, PHP oder Python.

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. In diesem Artikel erfahren Sie, wie Sie Ihre App mithilfe von Ansible in Azure App Service skalieren.

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service-Web-Apps:** Falls Sie noch nicht über eine Azure App Service-Web-App verfügen, können Sie [Azure-Web-Apps mithilfe von Ansible erstellen](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app-in-app-service"></a>Zentrales Hochskalieren einer App in App Service
Zum zentralen Hochskalieren können Sie den Tarif des App Service-Plans ändern, zu dem die App gehört. In diesem Abschnitt finden Sie ein Ansible-Beispielplaybook, das den folgenden Vorgang definiert:
- Abrufen der Angaben eines vorhandenen App Service-Plans
- Aktualisieren des App Service-Plans auf S2 mit drei Workern

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App serivce plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Speichern Sie dieses Playbook unter dem Namen *webapp_scaleup.yml*.

Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook webapp_scaleup.yml
```

Nach Ausführung des Playbooks werden Sie darüber informiert, dass der App Service-Plan erfolgreich auf S2 mit drei Workern aktualisiert wurde. Die Ausgabe sieht in etwa wie folgt aus:
```Output
PLAY [localhost] **************************************************************

TASK [Gathering Facts] ********************************************************
ok: [localhost]

TASK [Get facts of existing App serivce plan] **********************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] ******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] *******************************************
changed: [localhost]

TASK [Get facts] ***************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP **********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Ansible unter Azure](https://docs.microsoft.com/azure/ansible/)