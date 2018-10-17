---
title: Verwalten von Webdatenverkehr mit einem Azure Application Gateway per Ansible (Vorschau)
description: Erfahren Sie, wie Sie mit Ansible ein Azure Application Gateway zum Verwalten des Webdatenverkehrs erstellen und konfigurieren können.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, Playbook, Azure Application Gateway, Lastenausgleich, Webdatenverkehr
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 02b98cb22d897fc9599f6e44ddc57ef4211b0893
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410845"
---
# <a name="manage-web-traffic-with-azure-application-gateway-using-ansible-preview"></a>Verwalten von Webdatenverkehr mit einem Azure Application Gateway per Ansible (Vorschau)
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können. 

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. In diesem Artikel erfahren Sie, wie Sie mit Ansible ein Azure Application Gateway erstellen und den Datenverkehr der Ausführung von zwei Webservern in Azure-Containerinstanzen verwalten. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen von zwei Azure-Containerinstanzen mit httpd-Image
> * Erstellen eines Anwendungsgateways mit obigen Azure-Containerinstanzen im Back-End-Pool


## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.7 erforderlich. Sie können die Version Ansible 2.7 RC installieren, indem Sie `sudo pip install ansible[azure]==2.7.0rc2` ausführen. Ansible 2.7 wird im Oktober 2018 veröffentlicht. Danach müssen Sie die Version hier nicht mehr angeben, weil 2.7 die Standardversion ist. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.  

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen **myResourceGroup** am Standort **eastus**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Speichern Sie das obige Playbook als *rg.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen 
Sie müssen ein virtuelles Netzwerk für das Anwendungsgateway erstellen, um mit anderen Ressourcen kommunizieren zu können. 

Das folgende Beispiel erstellt ein virtuelles Netzwerk namens **myVNet**, ein Subnetz namens **myAGSubnet** und eine öffentliche IP-Adresse mit dem Namen **myAGPublicIPAddress** mit der Domäne mit dem Namen **mydomain**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    vnet_name: myVNet 
    subnet_name: myAGSubnet 
    publicip_name: myAGPublicIPAddress
    publicip_domain: mydomain
  tasks:
    - name: Create a virtual network
      azure_rm_virtualnetwork:
        name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
            - 10.1.0.0/16
            - 172.100.0.0/16
        dns_servers:
            - 127.0.0.1
            - 127.0.0.2

    - name: Create a subnet
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: 10.1.0.0/24

    - name: Create a public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}" 
        allocation_method: Dynamic
        name: "{{ publicip_name }}"
        domain_name_label: "{{ publicip_domain }}"
```

Speichern Sie das obige Playbook als *vnet_create.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook vnet_create.yml
```

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern
In diesem Beispiel erstellen Sie zwei Azure-Containerinstanzen mit httpd-Images, die als Back-End-Server für das Anwendungsgateway verwendet werden.  

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Create a container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_1_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80

    - name: Create another container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_2_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80
```

Speichern Sie das obige Playbook als *aci_create.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Jetzt erstellen wir ein Anwendungsgateway. Das folgende Beispiel erstellt ein Anwendungsgateway mit dem Namen **myAppGateway** mit Back-End-, Front-End- und http-Konfiguration.  

> [!div class="checklist"]
> * **appGatewayIP** definiert im **gateway_ip_configurations**-Block: Subnetzverweis ist für die IP-Konfiguration des Gateways erforderlich. 
> * **appGatewayBackendPool** definiert im **backend_address_pools**-Block: Ein Anwendungsgateway muss über mindestens einen Back-End-Adresspool verfügen. 
> * **appGatewayBackendHttpSettings** definiert im **backend_http_settings_collection**-Block: Gibt an, dass zur Kommunikation Port 80 und ein HTTP-Protokoll verwendet werden. 
> * **appGatewayHttpListener** definiert im **backend_http_settings_collection**-Block: der appGatewayBackendPool zugeordnete Standardlistener. 
> * **appGatewayFrontendIP** definiert im **frontend_ip_configurations**-Block: Weist myAGPublicIPAddress appGatewayHttpListener zu. 
> * **rule1** definiert im **request_routing_rules**-Block: Die Standardroutingregel, die appGatewayHttpListener zugeordnet ist. 

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    vnet_name: myVNet
    subnet_name: myAGSubnet
    location: eastus
    publicip_name: myAGPublicIPAddress
    appgw_name: myAppGateway
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Get info of Subnet
      azure_rm_resource_facts:
        api_version: '2018-08-01'
        resource_group: "{{ resource_group }}"
        provider: network
        resource_type: virtualnetworks
        resource_name: "{{ vnet_name }}"
        subresource:
          - type: subnets
            name: "{{ subnet_name }}"
      register: subnet

    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_1_name }}"
      register: aci_1_output
    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_2_name }}"
      register: aci_2_output

    - name: Create instance of Application Gateway
      azure_rm_appgateway:
        resource_group: "{{ resource_group }}"
        name: "{{ appgw_name }}"
        sku:
          name: standard_small
          tier: standard
          capacity: 2
        gateway_ip_configurations:
          - subnet:
              id: "{{ subnet.response[0].id }}"
            name: appGatewayIP
        frontend_ip_configurations:
          - public_ip_address: "{{ publicip_name }}"
            name: appGatewayFrontendIP
        frontend_ports:
          - port: 80
            name: appGatewayFrontendPort
        backend_address_pools:
          - backend_addresses:
              - ip_address: "{{ aci_1_output.response[0].properties.ipAddress.ip }}"
              - ip_address: "{{ aci_2_output.response[0].properties.ipAddress.ip }}"
            name: appGatewayBackendPool
        backend_http_settings_collection:
          - port: 80
            protocol: http
            cookie_based_affinity: enabled
            name: appGatewayBackendHttpSettings
        http_listeners:
          - frontend_ip_configuration: appGatewayFrontendIP
            frontend_port: appGatewayFrontendPort
            name: appGatewayHttpListener
        request_routing_rules:
          - rule_type: Basic
            backend_address_pool: appGatewayBackendPool
            backend_http_settings: appGatewayBackendHttpSettings
            http_listener: appGatewayHttpListener
            name: rule1
```

Speichern Sie das obige Playbook als *appgw_create.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook appgw_create.yml
```

Es kann einige Minuten dauern, bis das Anwendungsgateway erstellt wird. 

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Im obigen Beispielplaybook für Netzwerkressourcen wurde die Domäne namens **mydomain** in **eastus** erstellt. Jetzt könnten Sie zum Browser navigieren, `http://mydomain.eastus.cloudapp.azure.com` eingeben, und dann sollten Sie die folgende Seite sehen, die bestätigt, dass das Anwendungsgateway wie erwartet funktioniert.

![Zugreifen auf das Anwendungsgateway](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht benötigen, können Sie sie löschen, indem Sie das unten angegebene Beispiel ausführen. Darin wird eine Ressourcengruppe mit dem Namen **myResourceGroup** gelöscht. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Speichern Sie das obige Playbook als *rg_delete.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Ansible unter Azure](https://docs.microsoft.com/azure/ansible/)
