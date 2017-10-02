---
title: "Verwenden von Terraform zum Erstellen einer vollständigen Linux-VM in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit Terraform eine vollständige Linux-VM-Umgebung in Azure erstellen und verwalten."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2017
ms.author: echuvyrov
ms.translationtype: HT
ms.sourcegitcommit: e05028ad46ef6ec2584cd2d3f4843cf38bb54f9e
ms.openlocfilehash: a4a418a3b277d41b62aa049941a4c65e3bb82808
ms.contentlocale: de-de
ms.lasthandoff: 09/16/2017

---

# <a name="create-a-complete-linux-virtual-machine-infrastructure-in-azure-with-terraform"></a>Erstellen einer vollständigen Linux-VM-Infrastruktur in Azure mit Terraform
Mit Terraform können Sie vollständige Infrastrukturbereitstellungen in Azure definieren und erstellen. Dazu lassen sich Terraform-Vorlagen in einem für Menschen lesbaren Format erstellen, die Azure-Ressourcen konsistent und reproduzierbar erstellen und konfigurieren. In diesem Artikel wird gezeigt, wie Sie eine vollständige Linux-Umgebung und die unterstützenden Ressourcen mit Terraform erstellen. Hier erfahren Sie auch, wie Sie [Terraform installieren und konfigurieren](terraform-install-configure.md).


## <a name="create-azure-connection-and-resource-group"></a>Erstellen einer Azure-Verbindung und -Ressourcengruppe
Lassen Sie uns alle Abschnitte einer Terraform-Vorlage durchgehen. Sie können auch die Vollversion der [Terraform-Vorlage](#complete-terraform-script) sehen, die Sie kopieren und einfügen können.

Der `provider`-Abschnitt weist Terraform an, einen Azure-Anbieter zu verwenden. Um die Werte für *subscription_id*, *client_id*, *client_secret* und [tenant_id](terraform-install-configure.md) abzurufen, sehen Sie im *Handbuch zum Installieren und Konfigurieren von Terraform* nach. Wenn Sie Umgebungsvariablen für die Werte erstellen, schließen Sie diesen Abschnitt nicht ein.

```tf
provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

Im folgenden Abschnitt wird eine Ressourcengruppe namens *myResourceGroup* am Standort *eastus* erstellt:

```tf
resource "azurerm_resource_group" "myResourceGroup" {
    name     = "myResourceGroup"
    location = "East US"

    tags {
        environment = "Terraform Demo"
    }
}
```

In allen weiteren Abschnitten verweisen Sie mit *${azurerm_resource_group.myterraform.name}* auf die Ressourcengruppe.


## <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen
Im folgenden Abschnitt wird ein virtuelles Netzwerk namens *myVnet* im Adressbereich *10.0.0.0/16* erstellt:

```tf
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}
```

Im folgenden Abschnitt wird das Subnetz *mySubnet* im virtuellen Netzwerk *myVnet* erstellt:

```tf
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.2.0/24"
}
```


## <a name="create-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Für den Zugriff auf Ressourcen über das Internet erstellen Sie eine öffentliche IP-Adresse für Ihren virtuellen Computer und weisen sie zu. Im folgenden Abschnitt wird eine öffentliche IP-Adresse *myPublicIP* erstellt:

```tf
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "East US"
    resource_group_name          = "${azurerm_resource_group.myterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe
Netzwerksicherheitsgruppen steuern den ein- und ausgehenden Netzwerkdatenverkehr des virtuellen Computers. Im folgenden Abschnitt wird eine Netzwerksicherheitsgruppe *myNetworkSecurityGroup* erstellt und eine Regel zum Zulassen von SSH-Datenverkehr über den TCP-Port 22 definiert:

```tf
resource "azurerm_network_security_group" "temyterraformpublicipnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-network-interface-card"></a>Erstellen virtueller Netzwerkschnittstellenkarten
Eine virtuelle Netzwerkschnittstellenkarte (NIC) verbindet Ihren virtuellen Computer mit einem angegebenen virtuellen Netzwerk, einer öffentlichen IP-Adresse und einer Netzwerksicherheitsgruppe. Im folgenden Abschnitt eines Ansible-Playbooks wird die virtuelle Netzwerkkarte *myNIC* erstellt, die mit den von Ihnen erstellen virtuellen Netzwerkressourcen verbunden ist:

```tf
resource "azurerm_network_interface" "myterraformnic" {
    name                = "myNIC"
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-storage-account-for-diagnostics"></a>Erstellen eines Speicherkontos für Diagnosezwecke
Um die Startdiagnose für eine VM zu speichern, benötigen Sie ein Speicherkonto. Diese Startdiagnose kann Sie bei der Problembehandlung und Überwachung des Status Ihrer VM unterstützen. Das Speicherkonto, dass Sie erstellen, dient nur der Speicherung der Startdiagnosedaten. Da jedes Speicherkonto einen eindeutigen Namen haben muss, wird im nächsten Abschnitt zufälliger Text erzeugt:

```tf
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraform.name}"
    }
    
    byte_length = 8
}
```

Nun können Sie ein Speicherkonto erstellen. Im folgenden Abschnitt wird ein Speicherkonto erstellt, dessen Name auf dem im vorherigen Schritt generierten zufälligen Text basiert:

```tf
resource "azurerm_storage_account" "mystorageaccount" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"
    location            = "East US"
    account_type        = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Der letzte Schritt besteht im Erstellen eines virtuellen Computers und dem Verwenden der erstellten Ressourcen. Im folgenden Abschnitt wird eine VM namens *myVM* erstellt und die virtuelle NIC *myNIC* angefügt. Das neueste *Ubuntu 16.04 LTS*-Image wird verwendet, und ein Benutzer namens *azureuser* wird mit deaktivierter Kennwortauthentifizierung erstellt. SSH-Schlüsseldaten finden Sie im Abschnitt *ssh_keys*.

```tf
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "East US"
    resource_group_name   = "${azurerm_resource_group.myterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.myterraformnic.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Premium_LRS"
    }

    storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    os_profile {
        computer_name  = "myvm"
        admin_username = "azureuser"
    }

    os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
            path     = "/home/azureuser/.ssh/authorized_keys"
            key_data = "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
        }
    }

    boot_diagnostics {
        enabled     = "true"
        storage_uri = "${azurerm_storage_account.mystorageaccount.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="complete-terraform-script"></a>Vervollständigen des Terraform-Skripts

Um alle diese Abschnitte zusammenzuführen und mit Terraform auszuführen, erstellen Sie eine Datei namens *terraform_azure.tf*, und fügen Sie den folgenden Inhalt ein:

```tf
variable "resourcename" {
  default = "myResourceGroup"
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}

# Create a resource group if it doesn’t exist
resource "azurerm_resource_group" "myterraform" {
    name     = "myResourceGroup"
    location = "East US"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# Create subnet
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.1.0/24"
}

# Create public IPs
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "East US"
    resource_group_name          = "${azurerm_resource_group.myterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# Create network interface
resource "azurerm_network_interface" "myterraformnic" {
    name                      = "myNIC"
    location                  = "East US"
    resource_group_name       = "${azurerm_resource_group.myterraform.name}"
    network_security_group_id = "${azurerm_network_security_group.myterraformnsg.id}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# Generate random text for a unique storage account name
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraform.name}"
    }
    
    byte_length = 8
}

# Create storage account for boot diagnostics
resource "azurerm_storage_account" "mystorageaccount" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.myterraform.name}"
    location            = "East US"
    account_type        = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual machine
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "East US"
    resource_group_name   = "${azurerm_resource_group.myterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.myterraformnic.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Premium_LRS"
    }

    storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    os_profile {
        computer_name  = "myvm"
        admin_username = "azureuser"
    }

    os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
            path     = "/home/azureuser/.ssh/authorized_keys"
            key_data = "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
        }
    }

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.mystorageaccount.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="build-and-deploy-the-infrastructure"></a>Erstellen und Bereitstellen der Infrastruktur
Wenn die Terraform-Vorlage erstellt wurde, initialisieren Sie Terraform. Dadurch wird sichergestellt, dass Terraform über alle erforderlichen Komponenten zum Erstellen der Vorlage in Azure verfügt.

```bash
terraform init
```

Lassen Sie die Vorlage als Nächstes von Terraform prüfen und validieren. Bei diesem Schritt werden die erforderlichen Ressourcen mit den von Terraform gespeicherten Zustandsinformationen verglichen. Anschließend wird die geplante Ausführung ausgegeben. Ressourcen werden *nicht* in Azure erstellt.

```bash
terraform plan
```

Nach der Ausführung des vorherigen Befehls sollte etwa der folgende Bildschirm angezeigt werden:

```bash
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

azurerm_resource_group.myterraform: Refreshing state... (ID: /subscriptions/guid/resourceGroups/myResourceGroup)
azurerm_public_ip.myterraformips: Refreshing state... (ID: /subscriptions/guid...t.Network/publicIPAddresses/myPublicIP)
azurerm_virtual_network.myterraformnetwork: Refreshing state... (ID: /subscriptions/guid...crosoft.Network/virtualNetworks/myVnet)
azurerm_subnet.myterraformsubnet: Refreshing state... (ID: /subscriptions/guid...irtualNetworks/myVnet/subnets/mySubnet)
azurerm_network_interface.myterraformnic: Refreshing state... (ID: /subscriptions/guid...rosoft.Network/networkInterfaces/myNIC)
azurerm_virtual_machine.myterraformvm: Refreshing state... (ID: /subscriptions/guid...Microsoft.Compute/virtualMachines/myVM)

The Terraform execution plan has been generated and is shown below.
Resources are shown in alphabetical order for quick scanning. Green resources
will be created (or destroyed and then created if an existing resource
exists), yellow resources are being changed in-place, and red resources
will be destroyed. Cyan entries are data sources to be read.

Note: You didn’t specify an “-out” parameter to save this plan, so when
“apply” is called, Terraform can’t guarantee this is what will execute.
  + azurerm_resource_group.myterraform
      <snip>
  + azurerm_virtual_network.myterraformnetwork
      <snip>
  + azurerm_network_interface.myterraformnic
      <snip>
  + azurerm_network_security_group.myterraformnsg
      <snip>
  + azurerm_public_ip.myterraformpublicip
      <snip>
  + azurerm_subnet.myterraformsubnet
      <snip>
  + azurerm_virtual_machine.myterraformvm
      <snip>
Plan: 7 to add, 0 to change, 0 to destroy.
```

Wenn alle Eingaben richtig und Sie für die Erstellung der Infrastruktur in Azure bereit sind, wenden Sie die Vorlage in Terraform an:

```bash
terraform apply
```

Wenn Terraform den Vorgang abschließt, wurde die VM-Infrastruktur erstellt. Sie können die öffentliche IP-Adresse Ihres virtuellen Computers mit dem Befehl [az vm show](/cli/azure/vm#show) abrufen:

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
```

Anschließend können Sie SSH in gewohnter Weise auf Ihre VM anwenden:

```bash
ssh azureuser@<publicIps>
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben die grundlegende Infrastruktur in Azure mithilfe von Terraform erstellt. Komplexere Szenarios einschließlich Beispielen zur Verwendung von Lastenausgleichen und VM-Skalierungsgruppen finden Sie in den zahlreichen [Terraform-Beispielen für Azure](https://github.com/hashicorp/terraform/tree/master/examples). Eine vollständige und aktuelle Liste der unterstützten Azure-Anbieter finden Sie in der [Terraform-Dokumentation](https://www.terraform.io/docs/providers/azurerm/index.html).
