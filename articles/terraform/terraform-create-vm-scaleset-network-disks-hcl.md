---
title: Verwenden von Terraform und Erstellen einer Azure-VM-Skalierungsgruppe mit HCL
description: "Verwenden Sie Terraform, um eine Azure-VM-Skalierungsgruppe zu konfigurieren und mit einer Version zu versehen – inklusive eines virtuellen Netzwerks und verwalteten angefügten Datenträgern."
keywords: Terraform DevOps, Skalierungsgruppe, virtueller Computer, Netzwerk, Speicher, Module
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/04/2017
ms.topic: article
ms.openlocfilehash: 7a4e21d547b3d2b2399f9f68b1babd9f82a421b7
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="use-terraform-to-plan-and-create-a-networked-azure-vm-scale-set-with-managed-storage"></a>Planen und Erstellen einer vernetzten Azure-VM-Skalierungsgruppe mit verwalteten Speicher mithilfe von Terraform

In diesem Artikel verwenden Sie [Terraform](https://www.terraform.io/), um eine [Azure-VM-Skalierungsgruppe](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) mit verwalteten Datenträgern unter Verwendung der [Hashicorp Configuration Language](https://www.terraform.io/docs/configuration/syntax.html) (HCL) zu erstellen und bereitzustellen.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten Ihrer Terraform-Bereitstellung
> * Verwenden von Variablen und Ausgaben für die Terraform-Bereitstellung 
> * Erstellen und Bereitstellen der Netzwerkinfrastruktur
> * Erstellen und Bereitstellen einer VM-Skalierungsgruppe und Anfügen an das Netzwerk
> * Erstellen und Bereitstellen einer Jumpbox zum Herstellen einer SSH-Verbindung mit den virtuellen Computern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

- [Installieren Sie Terraform, und konfigurieren Sie den Zugriff auf Azure.](/azure/virtual-machines/linux/terraform-install-configure)
- Erstellen Sie ein [SSH-Schlüsselpaar](/azure/virtual-machines/linux/mac-create-ssh-keys), falls Sie noch keins besitzen.

## <a name="create-the-file-structure"></a>Erstellen der Dateistruktur

Erstellen Sie in einem leeren Verzeichnis drei neue Dateien mit folgenden Namen:

- `variables.tf`: Diese Datei enthält die Werte der in der Vorlage verwendeten Variablen.
- `output.tf`: Diese Datei beschreibt die Einstellungen, die nach der Bereitstellung angezeigt werden.
- `vmss.tf`: Diese Datei enthält den Code für die Infrastruktur der VM-Skalierungsgruppe.

## <a name="create-the-variables-and-output-definitions"></a>Erstellen der Variablen und Ausgabedefinitionen

In diesem Schritt definieren Sie Variablen zur Anpassung der durch Terraform erstellten Ressourcen.

Bearbeiten Sie die Datei `variables.tf`, kopieren Sie den folgenden Code, und speichern Sie die Änderungen.

```tf 
variable "location" {
  description = "The location where resources will be created"
  default     = "West US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources will be created"
  default     = "myResourceGroup"
}
```

Bearbeiten Sie die Datei `output.tf`, und kopieren Sie den folgenden Code, um den vollständig qualifizierten Domänennamen für die virtuellen Computer verfügbar zu machen. 

```hcl 
output "vmss_public_ip" {
    value = "${azurerm_public_ip.vmss.fqdn}"
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definieren der Netzwerkinfrastruktur in einer Vorlage

In diesem Schritt erstellen Sie in einer neuen Azure-Ressourcengruppe die folgende Netzwerkinfrastruktur: 

  - Ein VNet mit dem Adressraum 10.0.0.0/16 
  - Ein Subnetz mit dem Adressraum 10.0.2.0/24
  - Zwei öffentliche IP-Adressen: eine für den Lastenausgleich der VM-Skalierungsgruppe, eine für die Verbindungsherstellung mit der SSH-Jumpbox


Bearbeiten und kopieren Sie den folgenden Code in der Datei `vmss.tf`: 

```tf
resource "azurerm_resource_group" "vmss" {
  name     = "${var.resource_group_name}"
  location = "${var.location}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_network" "vmss" {
  name                = "vmss-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_subnet" "vmss" {
  name                 = "vmss-subnet"
  resource_group_name  = "${azurerm_resource_group.vmss.name}"
  virtual_network_name = "${azurerm_virtual_network.vmss.name}"
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "vmss" {
  name                         = "vmss-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

``` 

> [!NOTE]
> Es empfiehlt sich, die in Azure bereitgestellten Ressourcen zu kennzeichnen, um sie später leichter identifizieren zu können.

## <a name="create-the-network-infrastructure"></a>Erstellen der Netzwerkinfrastruktur

Initialisieren Sie die Terraform-Umgebung, indem Sie in dem Verzeichnis, in dem Sie die Dateien vom Typ `.tf` erstellt haben, den folgenden Befehl ausführen:

```bash
terraform init 
```

Führen Sie dann den folgenden Befehl aus, um die Infrastruktur in Azure bereitzustellen:

```bash
terraform apply
```

Vergewissern Sie sich, dass der FQDN der öffentlichen IP-Adresse Ihrer Konfiguration entspricht: ![VM-Skalierungsgruppe: Terraform-FQDN für die öffentliche IP-Adresse](./media/tf-create-vmss-step4-fqdn.png)


Die Ressourcengruppe muss über folgende Ressourcen verfügen: ![VM-Skalierungsgruppe: Terraform-Netzwerkressourcen](./media/tf-create-vmss-step4-rg.png)


## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>Bearbeiten Sie die Infrastruktur, um die VM-Skalierungsgruppe hinzuzufügen.

In diesem Schritt fügen Sie der Vorlage die folgenden Ressourcen hinzu:

- Einen Azure-Lastenausgleich und Regeln, um die Anwendung zu versorgen und an die zuvor konfigurierte öffentliche IP-Adresse anzufügen.
- Einen Azure-Back-End-Adresspool, der dem Lastenausgleich zugewiesen werden muss 
- Ein Integritätstestport, der von der Anwendung verwendet und für den Lastenausgleich konfiguriert wird 
- Eine VM-Skalierungsgruppe, die sich hinter dem Lastenausgleich befindet und in dem zuvor bereitgestellten VNet ausgeführt wird
- [Nginx](http://nginx.org/) auf den Knoten der VM-Skalierungsgruppe mit einer benutzerdefinierten Skripterweiterung

Fügen Sie am Ende der Datei `vmss.tf` den folgenden Code hinzu:

```tf
resource "azurerm_lb" "vmss" {
  name                = "vmss-lb"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  frontend_ip_configuration {
    name                 = "PublicIPAddress"
    public_ip_address_id = "${azurerm_public_ip.vmss.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_lb_backend_address_pool" "bpepool" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "BackEndAddressPool"
}

resource "azurerm_lb_probe" "vmss" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "ssh-running-probe"
  port                = "${var.application_port}"
}

resource "azurerm_lb_rule" "lbnatrule" {
    resource_group_name            = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id                = "${azurerm_lb.vmss.id}"
    name                           = "http"
    protocol                       = "Tcp"
    frontend_port                  = "${var.application_port}"
    backend_port                   = "${var.application_port}"
    backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
    frontend_ip_configuration_name = "PublicIPAddress"
    probe_id                       = "${azurerm_lb_probe.vmss.id}"
}

resource "azurerm_virtual_machine_scale_set" "vmss" {
  name                = "vmscaleset"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  upgrade_policy_mode = "Manual"

  sku {
    name     = "Standard_DS1_v2"
    tier     = "Standard"
    capacity = 2
  }

  storage_profile_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_profile_os_disk {
    name              = ""
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_profile_data_disk {
    lun          = 0
    caching        = "ReadWrite"
    create_option  = "Empty"
    disk_size_gb   = 10
  }

  os_profile {
    computer_name_prefix = "vmlab"
    admin_username       = "azureuser"
    admin_password       = "Passwword1234"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  network_profile {
    name    = "terraformnetworkprofile"
    primary = true

    ip_configuration {
      name                                   = "IPConfiguration"
      subnet_id                              = "${azurerm_subnet.vmss.id}"
      load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
    }
  }

  extension { 
    name = "vmssextension"
    publisher = "Microsoft.OSTCExtensions"
    type = "CustomScriptForLinux"
    type_handler_version = "1.2"
    settings = <<SETTINGS
    {
        "commandToExecute": "sudo apt-get -y install nginx"
    }
    SETTINGS
  }

  tags {
    environment = "codelab"
  }
}
```

Passen Sie die Bereitstellung an, indem Sie `variables.tf` den folgenden Code hinzufügen:

```tf 
variable "application_port" {
    description = "The port that you want to expose to the external load balancer"
    default     = 80
}

variable "admin_password" {
    description = "Default password for admin"
    default = "Passwwoord11223344"
}
``` 


## <a name="deploy-the-virtual-machine-scale-set-in-azure"></a>Bereitstellen der VM-Skalierungsgruppe in Azure

Führen Sie den folgenden Befehl aus, um die Bereitstellung der VM-Skalierungsgruppe zu visualisieren:

```bash
terraform plan
```

Die Ausgabe des Befehls sollte in etwa wie folgt aussehen:
![Terraform: Plan für das Hinzufügen von VM-Skalierungsgruppen](./media/tf-create-vmss-step6-plan884d3aefd9708a711bc09a66e85eb149c23a3ccff959655ec00418168b2bd481.png)

Stellen Sie anschließend die zusätzlichen Ressourcen in Azure bereit: 

```bash
terraform apply 
```

Der Inhalt der Ressourcengruppe sollte wie folgt aussehen:

![Terraform: Ressourcengruppe der VM-Skalierungsgruppe](./media/tf-create-vmss-step6-apply.png)

Öffnen Sie einen Browser, und stellen Sie eine Verbindung mit dem FQDN her, der durch den Befehl zurückgegeben wurde. 

## <a name="add-an-ssh-jumpbox-to-the-existing-network"></a>Hinzufügen einer SSH-Jumpbox zum vorhandenen Netzwerk 

In diesem Schritt konfigurieren Sie die folgenden Ressourcen:
- Eine Netzwerkschnittstelle, die mit dem gleichen Subnetz verbunden ist wie die VM-Skalierungsgruppe.
- Einen virtuellen Computer, der mit dieser Netzwerkschnittstelle verbunden ist. Auf diese Jumpbox kann remote zugegriffen werden. Nach der Verbindungsherstellung können Sie eine SSH-Verbindung mit einem beliebigen virtuellen Computer in der Skalierungsgruppe herstellen.



Fügen Sie am Ende der Datei `vmss.tf` den folgenden Code hinzu:

```hcl 
resource "azurerm_public_ip" "jumpbox" {
  name                         = "jumpbox-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}-ssh"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_network_interface" "jumpbox" {
  name                = "jumpbox-nic"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  ip_configuration {
    name                          = "IPConfiguration"
    subnet_id                     = "${azurerm_subnet.vmss.id}"
    private_ip_address_allocation = "dynamic"
    public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_machine" "jumpbox" {
  name                  = "jumpbox"
  location              = "${var.location}"
  resource_group_name   = "${azurerm_resource_group.vmss.name}"
  network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
  vm_size               = "Standard_DS1_v2"

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "jumpbox-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  os_profile {
    computer_name  = "jumpbox"
    admin_username = "azureuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  tags {
    environment = "codelab"
  }
}
```

Bearbeiten Sie `outputs.tf`, und fügen Sie den folgenden Code hinzu, um nach Abschluss der Bereitstellung den Hostnamen der Jumpbox anzuzeigen:

```
output "jumpbox_public_ip" {
    value = "${azurerm_public_ip.jumpbox.fqdn}"
}
```

## <a name="deploy-the-jumpbox"></a>Bereitstellen der Jumpbox

Stellen Sie die Jumpbox bereit.

```bash
terraform apply 
```

Nach Abschluss der Bereitstellung sieht der Inhalt der Ressourcengruppe wie folgt aus:

![Terraform: Ressourcengruppe der VM-Skalierungsgruppe](./media/tf-create-create-vmss-step8.png)

> [!NOTE]
> Die Anmeldung mit einem Kennwort ist für die bereitgestellte Jumpbox und für die bereitgestellte VM-Skalierungsgruppe deaktiviert. Melden Sie sich mit SSH an, um auf die virtuellen Computer zuzugreifen.

## <a name="clean-up-the-environment"></a>Bereinigen der Umgebung

Die folgenden Befehle löschen die in diesem Tutorial erstellten Ressourcen:

```bash
terraform destroy
```

Geben Sie `yes` ein, wenn Sie dazu aufgefordert werden, das Löschen der Ressourcen zu bestätigen. Die Bereinigung dauert einige Minuten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mithilfe von Terraform eine VM-Skalierungsgruppe in Azure bereitgestellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Initialisieren Ihrer Terraform-Bereitstellung
> * Verwenden von Variablen und Ausgaben für die Terraform-Bereitstellung 
> * Erstellen und Bereitstellen einer Netzwerkinfrastruktur
> * Erstellen und Bereitstellen einer VM-Skalierungsgruppe und Anfügen an eine vorhandene Umgebung
> * Erstellen und Bereitstellen einer Jumpbox zum Herstellen einer SSH-Verbindung mit den virtuellen Computern 
