---
title: Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Terraform zum Erstellen von Azure-Ressourcen installieren und konfigurieren
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/23/2017
ms.author: echuvyrov
ms.openlocfilehash: d8c357474e040f2e35c51dec0c7785cf98381a37
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure
 
Terraform bietet eine einfache Möglichkeit, Cloudinfrastruktur mit der [einfachen Vorlagensprache](https://www.terraform.io/docs/configuration/syntax.html) zu definieren, eine Vorschau anzuzeigen und bereitzustellen. In diesem Artikel werden die notwendigen Schritte zum Bereitstellen von Ressourcen in Azure mithilfe von Terraform beschrieben. 

> [!TIP]
> Terraform ist Teil der [Azure Cloud Shell Bash-Funktionalität](/azure/cloud-shell/quickstart) und wird mit Anmeldeinformationen und [Azure Terraform-Modulen](https://registry.terraform.io/modules/Azure) vorkonfiguriert.

## <a name="install-terraform"></a>Installieren von Terraform

[Laden](https://www.terraform.io/downloads.html) Sie zum Installieren von Terraform das entsprechende Paket für Ihr Betriebssystem in ein separates Installationsverzeichnis herunter. Der Download enthält eine einzelne ausführbare Datei, für die Sie auch einen globalen Pfad definieren müssen. [Auf dieser Webseite](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux) finden Sie Anweisungen für das Einrichten des Pfads unter Linux und Macintosh. [Auf dieser Webseite](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) finden Sie Anweisungen für das Einrichten des Pfads unter Windows. 

Überprüfen Sie Ihre Pfadkonfiguration mit dem Befehl `terraform`. Normalerweise wird dann die Liste der verfügbaren Terraform-Optionen angezeigt:

```bash
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Einrichten des Terraform-Zugriffs auf Azure

Konfigurieren Sie einen [Azure AD-Dienstprinzipal](/cli/azure/create-an-azure-service-principal-azure-cli), um es Terraform zu ermöglichen, Ressourcen in Azure bereitzustellen. Der Dienstprinzipal gewährt Ihren Terraform-Skripts Anmeldeinformationen zur Verwendung, um in Ihrem Azure-Abonnement Ressourcen bereitstellen zu können.

Es gibt mehrere Möglichkeiten, eine Azure AD-Anwendung und einen Azure AD-Dienstprinzipal zu erstellen. Am einfachsten und schnellsten ist es, die Azure CLI 2.0 zu verwenden, die Sie [für Windows, Linux oder Mac herunterladen und installieren können](/cli/azure/install-azure-cli).

Melden Sie sich für die Verwaltung Ihres Azure-Abonnements an. Geben Sie hierzu den folgenden Befehl aus:

```azurecli-interactive
az login
```

Wenn Sie über mehrere Azure-Abonnements verfügen, werden deren Details vom Befehl `az login` zurückgegeben. Legen Sie die `SUBSCRIPTION_ID`-Umgebungsvariable so fest, dass sie den Wert des zurückgegebenen Feldes `id` für das gewünschte Abonnement enthält. 

Legen Sie das Abonnement fest, das Sie für diese Sitzung verwenden möchten.

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Fragen Sie das Konto ab, um die Werte für Abonnement- und Mandanten-ID abzurufen.

```azurecli-interactive
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Erstellen Sie anschließend separate Anmeldeinformationen für Terraform.

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Es werden „appId“, „password“, „sp_name“ sowie „tenenat“ zurückgegeben. Notieren Sie sich appId und Kennwort.

Öffnen Sie zum Testen Ihrer Anmeldeinformationen eine neue Shell, und führen Sie den folgenden Befehl mithilfe der zurückgegebenen Werte für „sp_name“, „password“ und „tenant“ aus:

```azurecli-interactive
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

## <a name="configure-terraform-environment-variables"></a>Konfigurieren der Terraform-Umgebungsvariablen

Konfigurieren Sie Terraform so, dass Mandanten-ID, Abonnement-ID, Client-ID und geheimer Clientschlüssel vom Dienstprinzipal verwendet werden, wenn Sie Azure-Ressourcen erstellen. Legen Sie die folgenden Umgebungsvariablen fest, die automatisch von den [Azure-Terraform-Modulen verwendet werden](https://registry.terraform.io/modules/Azure).

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Dieses Beispiel-Shell-Skript können Sie dazu verwenden, diese Variablen festzulegen:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

## <a name="run-a-sample-script"></a>Ausführen eines Beispielskripts

Erstellen Sie die Datei `test.tf` in einem leeren Verzeichnis, und fügen Sie das folgende Skript ein. 

```tf
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Speichern Sie die Datei, und führen Sie dann `terraform init` erneut aus. Mit diesem Befehl werden die Azure-Module heruntergeladen, die zum Erstellen einer Azure-Ressourcengruppe erforderlich sind. Die folgende Ausgabe wird angezeigt:

```
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Zeigen Sie eine Vorschau des Skripts mit `terraform plan` an, und erstellen Sie dann die Ressourcengruppe `testResouceGroup` mit `terraform apply`:

```
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben Terraform installiert und Azure-Anmeldeinformationen konfiguriert und können mit dem Bereitstellen einer Infrastruktur in Ihrem Azure-Abonnement beginnen. Sie haben die Installation dann durch Erstellen einer leeren Azure-Ressourcengruppe getestet.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Azure-Computers mit Terraform](terraform-create-complete-vm.md)

