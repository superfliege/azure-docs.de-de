---
title: Verwenden eines Azure Marketplace-Images zum Erstellen einer Terraform-VM für Linux mit verwalteter Dienstidentität
description: Verwenden Sie ein Marketplace-Image, um eine Terraform-VM für Linux mit verwalteter Dienstidentität und Remotezustandsverwaltung zur einfachen Bereitstellung von Ressourcen in Azure zu erstellen.
keywords: Terraform, DevOps, MSI, VM, Remotezustand, Azure
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: ce8a3e8b813bf4224a1fd77d60ec30f2f8e080a7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Verwenden eines Azure Marketplace-Images zum Erstellen einer Terraform-VM für Linux mit verwalteter Dienstidentität

In diesem Artikel erfahren Sie, wie Sie mit einem [Terraform-Marketplace-Image](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) eine `Ubuntu Linux VM (16.04 LTS)` erstellen, auf der die neueste [Terraform](https://www.terraform.io/intro/index.html)-Version installiert ist und die mit der [verwalteten Dienstidentität (Managed Service Identity, MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) konfiguriert ist. Dieses Image konfiguriert außerdem ein Remote-Back-End, um die Verwaltung des [Remotezustands](https://www.terraform.io/docs/state/remote.html) mit Terraform zu ermöglichen. Mit dem Terraform-Marketplace-Image können Sie die ersten Schritte zur Verwendung von Terraform in Azure in Minutenschnelle erledigen, ohne Terraform installieren und die Authentifizierung manuell konfigurieren zu müssen. 

Für dieses Terraform-VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Gebühren für die Azure-Hardwarenutzung, die basierend auf der Größe der von Ihnen bereitgestellten VM berechnet werden. Weitere Informationen zu den Computegebühren finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine Terraform-VM für Linux erstellen können, benötigen Sie ein Azure-Abonnement. Wenn Sie noch kein Abonnement besitzen, helfen Ihnen die Informationen unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free/)weiter.  

## <a name="create-your-terraform-virtual-machine"></a>Erstellen der Terraform-VM 

Führen Sie zum Erstellen einer Instanz der Terraform-VM für Linux die folgenden Schritte aus. 

1. Navigieren Sie im Azure-Portal zum Eintrag [Ressource erstellen](https://ms.portal.azure.com/#create/hub).
2. Suchen Sie in der Suchleiste `Search the Marketplace` nach `Terraform`. Wählen Sie die Vorlage `Terraform` aus. Klicken Sie unten rechts auf der Registerkarte mit den Terraform-Details auf die Schaltfläche **Erstellen**.
![Alternativer Text](media\terraformmsi.png)
3. Die folgenden Abschnitte enthalten die Eingaben für jeden Schritt des Assistenten (**aufgelistet auf der rechten Seite**) zum Erstellen der Terraform-VM für Linux.  Im Folgenden finden Sie die erforderlichen Eingaben zum Konfigurieren der einzelnen Schritte.

## <a name="details-in-create-terraform-tab"></a>Details auf der Registerkarte „Terraform erstellen“

Die folgenden Details müssen auf der Registerkarte „Terraform erstellen“ eingegeben werden.

a. **Grundlagen**
    
* **Name**: Der Name Ihrer Terraform-VM.
* **Benutzername**: Die ID für die erste Kontoanmeldung.
* **Kennwort**: Das erste Kontokennwort (Sie können anstelle eines Kennworts einen öffentlichen SSH-Schlüssel verwenden).
* **Abonnement:**Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus, über das der Computer erstellt und abgerechnet wird. Sie müssen für dieses Abonnement über Berechtigungen zum Erstellen von Ressourcen verfügen.
* **Ressourcengruppe**: Sie können eine neue Gruppe erstellen oder eine vorhandene Gruppe verwenden.
* **Standort**: Wählen Sie das Rechenzentrum aus, das am besten geeignet ist. Normalerweise handelt es sich dabei um das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist oder das Ihrem physischen Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen.

b. **Zusätzliche Einstellungen**

* Größe: Die Größe der VM.
* VM-Datenträgertyp: Wählen Sie zwischen SSD und HDD.

c. **Zusammenfassung für Terraform**

* Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind. 

d. **Kaufen**

* Klicken Sie auf „Kaufen“, um die Bereitstellung zu starten. Ein Link zu den Bedingungen der Transaktion wird bereitgestellt. Für die VM gelten keine über die Computekosten für die Servergröße, die Sie unter „Größe“ ausgewählt haben, hinausgehenden Kosten.

Das Terraform-VM-Image führt die folgenden Schritte aus:

* Es erstellt eine VM mit vom System zugewiesener Identität basierend auf dem Ubuntu 16.04 LTS-Image.
* Es installiert die MSI-Erweiterung auf der VM, um das Ausstellen von OAuth-Token für Azure-Ressourcen zu ermöglichen.
* Es weist der verwalteten Identität RBAC-Berechtigungen zu, wodurch Besitzerrechte für die Ressourcengruppe erteilt werden.
* Es erstellt einen Vorlagenordner für Terraform (TfTemplate).
* Es konfiguriert den Terraform-Remotezustand mit dem Azure-Back-End vor.

## <a name="how-to-access-and-configure-linux-terraform-virtual-machine"></a>Zugreifen auf die Terraform-VM für Linux und Konfigurieren der VM

Nachdem die VM erstellt wurde, können Sie sich mithilfe von SSH an der VM anmelden. Verwenden Sie dazu die Kontoanmeldeinformationen, die Sie im Abschnitt „Grundlagen“ von Schritt 3 für die Textshell-Schnittstelle erstellt haben. Unter Windows können Sie ein SSH-Clienttool wie [Putty](http://www.putty.org/) herunterladen.

Nachdem Sie mit `SSH` eine Verbindung mit der VM hergestellt haben, müssen Sie der verwalteten Dienstidentität auf der VM Berechtigungen für Mitwirkende für das gesamte Abonnement erteilen. Die Berechtigung für Mitwirkende ermöglicht der verwalteten Dienstidentität auf der VM die Verwendung von Terraform zum Erstellen von Ressourcen außerhalb der VM-Ressourcengruppe. Dazu müssen Sie lediglich einmal ein Skript ausführen. Der entsprechende Befehl lautet wie folgt.

`. ~/tfEnv.sh`

Das vorherige Skript verwendet den Mechanismus zur [interaktiven Anmeldung mit Azure CLI 2.0 ](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in), um die Authentifizierung bei Azure vorzunehmen und die Berechtigung für Mitwirkende der verwalteten Dienstidentität der VM für das gesamte Abonnement zuzuweisen. 

 Für die VM wurde ein Terraform-Remotezustands-Back-End erstellt. Um dieses Backend für Ihre Terraform-Bereitstellung zu aktivieren, müssen Sie die Datei „remoteState.tf“ aus dem Verzeichnis „tfTemplate“ in das Stammverzeichnis der Terraform-Skripts kopieren.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Weitere Informationen zur Remotezustandsverwaltung finden Sie [hier](https://www.terraform.io/docs/state/remote.html). Der Speicherzugriffsschlüssel wird in dieser Datei verfügbar gemacht und muss sorgfältig in die Quellcodeverwaltung eingecheckt werden.  

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie eine Terraform-VM für Linux in Azure einrichten. Folgende zusätzliche Ressourcen können Sie nutzen, um mehr über Terraform in Azure zu erfahren. 

 [Dokumentation zu Terraform in Azure](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure Provider Documentation](http://aka.ms/terraform) (Dokumentation zum Azure-Anbieter für Terraform)  
 [Terraform Azure Provider Source](http://aka.ms/tfgit) (Referenz zum Azure-Anbieter für Terraform)  
 [Terraform Azure Modules](http://aka.ms/tfmodules) (Azure-Module für Terraform)
 

















