---
title: Verwenden von Terraform mit Azure Cloud Shell
description: Verwenden Sie Terraform mit Azure Cloud Shell, um die Authentifizierung und die Vorlagenkonfiguration zu vereinfachen.
keywords: Terraform DevOps, Skalierungsgruppe, virtueller Computer, Netzwerk, Speicher, Module
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/19/2017
ms.topic: article
ms.openlocfilehash: 253e5d341f93e61d851893eb05832fbf35707cfc
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="terraform-cloud-shell-development"></a>Entwicklung mit Terraform und Cloud Shell 

Terraform kann hervorragend über eine Bash-Befehlszeile (etwa das macOS-Terminal oder Bash unter Windows oder Linux) verwendet werden. Die Ausführung Ihrer Terraform-Konfigurationen in der Bash-Umgebung von [Azure Cloud Shell](/azure/cloud-shell/overview) hat einige besondere Vorteile, die den Entwicklungszyklus beschleunigen.

Dieser Konzeptartikel behandelt Cloud Shell-Features, die Sie beim Schreiben von Terraform-Skripts für die Bereitstellung in Azure unterstützen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische Konfiguration von Anmeldeinformationen

Terraform ist nach der Installation sofort in Cloud Shell verfügbar. Terraform-Skripts werden über Azure authentifiziert, wenn Sie bei Cloud Shell angemeldet sind, und ermöglichen die Verwaltung der Infrastruktur ohne zusätzliche Konfiguration. Dank automatischer Authentifizierung entfallen das manuelle Erstellen eines Active Directory-Dienstprinzipals sowie das Konfigurieren der Variablen für den Azure-Terraform-Anbieter.


## <a name="using-modules-and-providers"></a>Verwenden von Modulen und Anbietern

Azure-Terraform-Module benötigen Anmeldeinformationen, um auf die Ressourcen in Ihrem Azure-Abonnement zugreifen und Änderungen an den Ressourcen vornehmen zu können. In Cloud Shell können Sie Ihren Skripts den folgenden Code hinzufügen, um Azure-Terraform-Module in Cloud-Shell zu verwenden:

```tf
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell übergibt die erforderlichen Werte für den `azurerm`-Anbieter über Umgebungsvariablen, wenn Sie einen der `terraform`-CLI-Befehle verwenden.

## <a name="other-cloud-shell-developer-tools"></a>Andere Cloud Shell-Entwicklungstools

Dateien und Shellzustände bleiben in Azure Storage zwischen Cloud Shell-Sitzungen erhalten. Verwenden Sie den [Azure Storage-Explorer](/azure/vs-azure-tools-storage-manage-with-storage-explorer), um Dateien auf Ihrem lokalen Computer zu kopieren und in Cloud Shell hochzuladen.

Die Azure CLI 2.0 steht in Cloud Shell zur Verfügung und eignet sich bestens zum Testen von Konfigurationen sowie zum Überprüfen der Arbeit nach Abschluss von `terraform apply` oder `terraform destroy`.


## <a name="next-steps"></a>Nächste Schritte

[Create a small VM cluster using the Module Registry](terraform-create-vm-cluster-module.md) (Erstellen eines kleinen VM-Clusters mithilfe der Modulregistrierung)
[Create a small VM cluster using custom HCL](terraform-create-vm-cluster-with-infrastructure.md) (Erstellen eines kleinen VM-Clusters mithilfe von benutzerdefinierter HCL)
