---
title: Erstellen, Ändern oder Löschen einer Azure-Routingtabelle mit Ansible
description: Es wird beschrieben, wie Sie eine Routingtabelle mit Ansible erstellen, ändern oder löschen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, Playbook, Netzwerke, Route, Routingtabelle
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/17/2018
ms.openlocfilehash: 29672a75408e42fb9239e5d826784b46e7280805
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332267"
---
# <a name="create-change-or-delete-an-azure-route-table-using-ansible"></a>Erstellen, Ändern oder Löschen einer Azure-Routingtabelle mit Ansible
Azure führt für Datenverkehr automatisch das Routing zwischen Azure-Subnetzen, virtuellen Netzwerken und lokalen Netzwerken durch. Wenn Sie ein Standardrouting auf Azure ändern möchten, erstellen Sie eine [Routingtabelle](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. In diesem Artikel wird gezeigt, wie Sie eine Azure-Routingtabelle erstellen, ändern oder löschen, und wie Sie die Routingtabelle einem Subnetz zuordnen. 

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Für ein Ausführen der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.7 erforderlich.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle
In diesem Abschnitt finden Sie ein Ansible-Beispielplaybook, mit dem eine Routingtabelle erstellt wird. Die Anzahl von Routingtabellen, die Sie pro Azure-Standort und -Abonnement erstellen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel zu den [Einschränkungen für Azure-Abonnements](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). 

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
```

Speichern Sie dieses Playbook als `route_table_create.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz
Jedem Subnetz können null oder eine Routingtabelle zugeordnet werden. Eine Routingtabelle kann keinem oder mehreren Subnetzen zugeordnet werden. Da Routingtabellen nicht virtuellen Netzwerken zugeordnet sind, müssen Sie jedem Subnetz, für das eine Zuordnung bestehen soll, eine Routingtabelle zuordnen. Der gesamte Datenverkehr, der das Subnetz verlässt, wird anhand von Routen, die Sie in Routingtabellen erstellt haben, [Standardrouten](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#default) und Routen weitergeleitet, die über ein lokales Netzwerk verteilt werden – sofern das virtuelle Netzwerk mit einem Gateway für das virtuelle Azure-Netzwerk verbunden ist (ExpressRoute oder VPN, falls BGP mit einem VPN-Gateway verwendet wird). Sie können eine Routingtabelle nur Subnetzen in virtuellen Netzwerken zuordnen, die an demselben Azure-Standort und unter demselben Abonnement wie die Routingtabelle vorhanden sind.

In diesem Abschnitt finden Sie ein Ansible-Beispielplaybook, in dem ein virtuelles Netzwerk und ein Subnetz erstellt werden und dann eine Routingtabelle mit dem Subnetz verknüpft wird.

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create virtual network
      azure_rm_virtualnetwork:
        name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
        - 10.1.0.0/16
        - 172.100.0.0/16
        dns_servers:
        - 127.0.0.1
        - 127.0.0.3
    - name: Create a subnet with route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
        route_table: "{ route_table_name }"
```

Speichern Sie dieses Playbook als `route_table_associate.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Aufheben der Zuordnung einer Routingtabelle zu einem Subnetz
Wenn Sie die Zuordnung einer Routingtabelle zu einem Subnetz aufheben möchten, müssen Sie die `route_table` in einem Subnetz lediglich auf `None` festlegen. Nachstehend finden Sie ein Ansible-Beispielplaybook. 

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    resource_group: myResourceGroup
  tasks:
    - name: Dissociate a route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
```

Speichern Sie dieses Playbook als `route_table_dissociate.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Erstellen einer Route
In diesem Abschnitt finden Sie ein Ansible-Beispielplaybook, mit dem eine Route in der Routingtabelle erstellt wird. In dem Beispiel ist `virtual_network_gateway` als `next_hop_type` und `10.1.0.0/16` als `address_prefix` definiert. Das Präfix kann in der Routingtabelle nicht für mehr als eine Route dupliziert werden, aber das Präfix kann in einem anderen Präfix enthalten sein. Weitere Informationen dazu, wie Azure Routen auswählt, sowie eine ausführliche Beschreibung aller Typen für den nächsten Hop finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        next_hop_type: virtual_network_gateway
        address_prefix: "10.1.0.0/16"
        route_table_name: "{{ route_table_name }}"
```
Speichern Sie dieses Playbook als `route_create.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Löschen einer Route
In diesem Abschnitt finden Sie ein Ansible-Beispielplaybook, mit dem eine Route aus einer Routingtabelle gelöscht wird.

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Remove route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        route_table_name: "{{ route_table_name }}"
        state: absent
```

Speichern Sie dieses Playbook als `route_delete.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-information-of-a-route-table"></a>Abrufen von Informationen zu einer Routingtabelle
Sie können Details zu einer Routingtabelle (route_table) über das Ansible-Modul `azure_rm_routetable_facts` anzeigen. Die Faktenmodul gibt die Informationen der Routingtabelle mit allen ihr zugeordneten Routen zurück.
Nachstehend finden Sie ein Ansible-Beispielplaybook. 

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks: 
    - name: Get route table information
      azure_rm_routetable_facts:
         resource_group: "{{ resource_group }}"
         name: "{{ route_table_name }}"
      register: query
    
    - debug:
         var: query.route_tables[0]
```

Speichern Sie dieses Playbook als `route_table_facts.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Löschen einer Routingtabelle
Wenn eine Routingtabelle Subnetzen zugeordnet ist, kann sie nicht gelöscht werden. [Heben Sie die Zuordnung einer Routingtabelle zu allen Subnetzen auf](#dissociate-a-route-table-from-a-subnet), bevor Sie versuchen, das Löschen durchzuführen.

Sie können die Routingtabelle zusammen mit allen Routen löschen. Nachstehend finden Sie ein Ansible-Beispielplaybook. 

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
```

Speichern Sie dieses Playbook als `route_table_delete.yml`. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Ansible-Playbook auszuführen:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Ansible unter Azure](https://docs.microsoft.com/azure/ansible/)
