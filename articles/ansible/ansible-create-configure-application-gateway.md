---
title: Verwalten von Webdatenverkehr mit Azure Application Gateway unter Verwendung von Ansible
description: Hier erfahren Sie, wie Sie mit Ansible eine Azure Application Gateway-Instanz zum Verwalten von Webdatenverkehr erstellen und konfigurieren.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, Playbook, Application Gateway, Lastenausgleich, Webdatenverkehr
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: af7f22ae5c289a01e6876d8ce586cb32383c8d3b
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253353"
---
# <a name="manage-web-traffic-with-azure-application-gateway-by-using-ansible"></a>Verwalten von Webdatenverkehr mit Azure Application Gateway unter Verwendung von Ansible

[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können.

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. In diesem Artikel erfahren Sie, wie Sie mithilfe von Ansible ein benutzerdefiniertes Anwendungsgateway erstellen. Außerdem erfahren Sie hier, wie Sie mit diesem Gateway den Datenverkehr für zwei Webserver verwalten, die in Azure-Containerinstanzen ausgeführt werden.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen von zwei Azure-Containerinstanzen mit HTTPD-Images
> * Erstellen eines Anwendungsgateways für die Verwendung mit den Azure-Containerinstanzen im Serverpool

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.7 erforderlich. 

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

Speichern Sie dieses Playbook als *rg.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Erstellen Sie zunächst ein virtuelles Netzwerk, damit das Anwendungsgateway mit anderen Ressourcen kommunizieren kann.

Das folgende Beispiel erstellt ein virtuelles Netzwerk namens **myVNet**, ein Subnetz namens **myAGSubnet** und eine öffentliche IP-Adresse namens **myAGPublicIPAddress** mit einer Domäne namens **mydomain**.

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

Speichern Sie dieses Playbook als *vnet_create.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook vnet_create.yml
```

## <a name="create-servers"></a>Erstellen von Servern

Das folgende Beispiel zeigt, wie Sie zwei Azure-Containerinstanzen mit HTTPD-Images erstellen, die als Webserver für das Anwendungsgateway verwendet werden.  

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

Speichern Sie dieses Playbook als *aci_create.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Das folgende Beispiel erstellt ein Anwendungsgateway namens **myAppGateway** mit Konfigurationen für Back-End, Front-End und HTTP.  

* **appGatewayIP** wird im Block **gateway_ip_configurations** definiert. Für die IP-Konfiguration des Gateways ist ein Subnetzverweis erforderlich.
* **appGatewayBackendPool** wird im Block **backend_address_pools** definiert. Ein Anwendungsgateway muss über mindestens einen Back-End-Adresspool verfügen.
* **appGatewayBackendHttpSettings** wird im Block **backend_http_settings_collection** definiert. Diese Einstellung gibt an, dass für die Kommunikation der Port 80 und ein HTTP-Protokoll verwendet werden.
* **appGatewayHttpListener** wird im Bock **backend_http_settings_collection** definiert. Hierbei handelt es sich um den Standardlistener, der „appGatewayBackendPool“ zugeordnet ist.
* **appGatewayFrontendIP** wird im Block **frontend_ip_configurations** definiert. Diese Einstellung weist „myAGPublicIPAddress“ zu „appGatewayHttpListener“ zu.
* **rule1** wird im Block **request_routing_rules** definiert. Hierbei handelt es sich um die Standardroutingregel, die „appGatewayHttpListener“ zugeordnet ist.

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

Speichern Sie dieses Playbook als *appgw_create.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook appgw_create.yml
```

Die Erstellung des Anwendungsgateways kann einige Minuten dauern.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Im obigen Beispielplaybook für Netzwerkressourcen haben Sie die Domäne **mydomain** in **eastus** erstellt. Navigieren Sie in Ihrem Browser zu `http://mydomain.eastus.cloudapp.azure.com`. Wenn die folgende Seite angezeigt wird, funktioniert das Anwendungsgateway wie erwartet.

![Erfolgreicher Test eines funktionierenden Anwendungsgateways](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie sie löschen, indem Sie den folgenden Code ausführen. Darin wird eine Ressourcengruppe mit dem Namen **myResourceGroup** gelöscht.

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

Speichern Sie dieses Playbook als *rg_delete.yml*. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ansible unter Azure](https://docs.microsoft.com/azure/ansible/)
