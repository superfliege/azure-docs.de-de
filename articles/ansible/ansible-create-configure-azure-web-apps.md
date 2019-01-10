---
title: Erstellen von Azure-Web-Apps mit Ansible
description: Hier erfahren Sie, wie Sie Ansible verwenden, um eine Web-App mit Java 8 und der Tomcat-Containerruntime in App Service unter Linux zu erstellen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, Playbook, Azure App Service, Web-App, Java
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 106fe11355d629e57f71b12d1125d188b98c2236
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051344"
---
# <a name="create-azure-app-service-web-apps-by-using-ansible"></a>Erstellen von Azure App Service-Web-Apps mit Ansible
[Azure App Service-Web-Apps](https://docs.microsoft.com/azure/app-service/overview) (oder einfach Web-Apps) dient zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends. Sie können in Ihrer bevorzugten Sprache entwickeln: .NET, .NET Core, Java, Ruby, Node.js, PHP oder Python.

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. In diesem Artikel wird veranschaulicht, wie Sie Ansible verwenden, um eine Web-App mit der Java-Runtime zu erstellen. 

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.7 erforderlich.

## <a name="create-a-simple-app-service"></a>Erstellen eines einfachen App-Diensts
In diesem Abschnitt finden Sie ein Beispielplaybook für Ansible, das die folgenden Ressourcen definiert:
- Ressourcengruppe, in der Ihr App Service-Plan und die Web-App bereitgestellt werden
- Web-App mit Java 8 und der Tomcat-Containerruntime in App Service unter Linux

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    webapp_name: myfirstWebApp
    plan_name: myAppServicePlan
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create App Service on Linux with Java Runtime
      azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        frameworks:
          - name: "java"
            version: "8"
            settings:
              java_container: tomcat
              java_container_version: 8.5
```
Speichern Sie das vorherige Playbook unter **firstwebapp.yml**.

Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook firstwebapp.yml
```

In der Ausgabe nach Ausführung des Ansible-Playbooks ist zu sehen, dass die Erstellung der Web-App erfolgreich war:

```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] *************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

PLAY RECAP *************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="create-an-app-service-by-using-traffic-manager"></a>Erstellen einer App Service-Instanz mithilfe von Traffic Manager
Mit [Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) können Sie steuern, wie Anforderungen von Webclients auf Apps in Azure App Service verteilt werden. Wenn einem Azure Traffic Manager-Profil App Service-Endpunkte hinzugefügt werden, überwacht Traffic Manager den Status Ihrer App Service-Apps. Mögliche Statuswerte sind „Ausgeführt“, „Beendet“ und „Gelöscht“. So kann Traffic Manager entscheiden, welcher der Endpunkte Datenverkehr empfangen soll.

In App Service wird eine App in einem [App Service-Plan](https://docs.microsoft.com/azure/app-service/overview-hosting-plans
) ausgeführt. Mit einem App Service-Plan wird ein Satz mit Computeressourcen für eine auszuführende Web-App definiert. Sie können Ihren App Service-Plan und die Web-App in unterschiedlichen Gruppen verwalten.

In diesem Abschnitt finden Sie ein Beispielplaybook für Ansible, das die folgenden Ressourcen definiert:
- Ressourcengruppe, in der Ihr App Service-Plan bereitgestellt wird
- App Service-Plan
- Sekundäre Ressourcengruppe, in der Ihre Web-App bereitgestellt wird
- Web-App mit Java 8 und der Tomcat-Containerruntime in App Service unter Linux
- Traffic Manager-Profil
- Traffic Manager-Endpunkt, der die erstellte Website verwendet

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group_webapp: myResourceGroupWebapp
    resource_group: myResourceGroup
    webapp_name: myLinuxWebApp
    plan_name: myAppServicePlan
    location: eastus
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group_webapp }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group_webapp }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        app_settings:
          testkey: "testvalue"
        frameworks:
          - name: java
            version: 8
            settings:
              java_container: "Tomcat"
              java_container_version: "8.5"

  - name: Get web app facts
    azure_rm_webapp_facts:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ webapp_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ traffic_manager_profile_name }}"
      location: global
      routing_method: performance
      dns_config:
        relative_name: "{{ traffic_manager_profile_name }}"
        ttl:  60
      monitor_config:
        protocol: HTTPS
        port: 80
        path: '/'

  - name: Add endpoint to traffic manager profile, using created web site
    azure_rm_trafficmanagerendpoint:
      resource_group: "{{ resource_group_webapp }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"
```
Speichern Sie das obige Playbook als **webapp.yml**, oder [laden Sie das Playbook herunter](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook webapp.yml
```

In der Ausgabe nach Ausführung des Ansible-Playbooks ist zu sehen, dass die Erstellung des App Service-Plans, der Web-App, des Traffic Manager-Profils und des Endpunkts erfolgreich war:
```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create resource group] ****************************************************************************
changed: [localhost]

TASK [Create resource group for app service plan] ****************************************************************************
changed: [localhost]

TASK [Create App Service Plan] ****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ****************************************************************************
changed: [localhost]

TASK [Get web app facts] *****************************************************************************
ok: [localhost]

TASK [Create Traffic Manager Profile] *****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] *****************************************************************************
changed: [localhost]

TASK [Get Traffic Manager Profile facts] ******************************************************************************
ok: [localhost]

PLAY RECAP ******************************************************************************
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Skalieren von Azure App Service-Web-Apps mit Ansible](https://docs.microsoft.com/azure/ansible/ansible-scale-azure-web-apps)