---
title: Terraform mit Bereitstellungsslot für Azure-Anbieter
description: 'Terraform mit Bereitstellungsslot für Azure-Anbieter: Tutorial'
keywords: Terraform, DevOps, virtueller Computer, Azure, Bereitstellungsslots
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 4/05/2018
ms.topic: article
ms.openlocfilehash: 34b16b5fb2b5b574d166693db346ebba15eaa1f9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="using-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Verwenden von Terraform zum Bereitstellen der Infrastruktur mit Azure-Bereitstellungsslots

[Azure-Bereitstellungsslots](/azure/app-service/web-sites-staged-publishing) ermöglichen den Wechsel zwischen verschiedenen Versionen Ihrer App (etwa Produktion und Staging), um die Beeinträchtigung durch fehlerhafte Bereitstellungen zu minimieren. Dieser Artikel enthält ein Beispiel für die Verwendung von Bereitstellungsslots sowie die Schritte zur Bereitstellung von zwei Apps über GitHub und Azure. Eine App wird in einem Produktionsslot und die andere in einem Stagingslot gehostet. (Die Namen „Produktion“ und „Staging“ sind willkürlich gewählt. Sie können beliebige Namen festlegen, die besser zu Ihrem Szenario passen.) Nach dem Konfigurieren Ihrer Bereitstellungsslots können Sie mithilfe von Terraform nach Bedarf zwischen den beiden Slots wechseln.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

- **GitHub-Konto:** Zum Forken und Verwenden des GitHub-Testrepositorys ist ein [GitHub](http://www.github.com)-Konto erforderlich.

## <a name="create-and-apply-the-terraform-plan"></a>Erstellen und Anwenden des Terraform-Plans

1. Navigieren Sie zum [Azure-Portal](http://portal.azure.com).

1. Öffnen Sie [Azure Cloud Shell](/azure/cloud-shell/overview), und wählen Sie als Umgebung **Bash**, sofern noch nicht geschehen.

    ![Cloud Shell-Eingabeaufforderung](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. Wechseln Sie in das Verzeichnis `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Erstellen Sie ein Verzeichnis namens `deploy`.

    ```bash
    mkdir deploy
    ```

1. Erstellen Sie ein Verzeichnis namens `swap`.

    ```bash
    mkdir swap
    ```

1. Stellen Sie mithilfe des Bash-Befehls `ls` sicher, dass beide Verzeichnisse erfolgreich erstellt wurden.

    ![Cloud Shell nach dem Erstellen von Verzeichnissen](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Wechseln Sie in das Verzeichnis `deploy`.

    ```bash
    cd deploy
    ```

1. Erstellen Sie mit dem [vi-Editor](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html) eine Datei mit dem Namen `deploy.tf`, die die [Terraform-Konfiguration](https://www.terraform.io/docs/configuration/index.html) enthält.

    ```bash
    vi deploy.tf
    ```

1. Wechseln Sie durch Drücken des Buchstabens `i` in den Einfügemodus.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```JSON
    # Configure the Azure Provider
    provider "azurerm" { }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
        app_service_name    = "${azurerm_app_service.slotDemo.name}"
    }
    ```

1. Drücken Sie die **&lt;ESC>**-Taste, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und verlassen Sie den vi-Editor, indem Sie den folgenden Befehl eingeben und die **&lt;EINGABETASTE>** drücken:

    ```bash
    :wq
    ```

1. Nach dem Erstellen der Datei können Sie ihren Inhalt überprüfen.

    ```bash
    cat deploy.tf
    ```

1. Initialisieren Sie Terraform.

    ```bash
    terraform init
    ```

1. Erstellen Sie den Terraform-Plan.

    ```bash
    terraform plan
    ```

1. Stellen Sie die in der Konfigurationsdatei `deploy.tf` definierten Ressourcen bereit. (Bestätigen Sie die Aktion, indem Sie an der Eingabeaufforderung `yes` eingeben.)

    ```bash
    terraform apply
    ```

1. Schließen Sie das Cloud Shell-Fenster.

1. Wählen Sie im Hauptmenü des Azure-Portals **Ressourcengruppen** aus.

    ![Option „Ressourcengruppen“ im Azure-Portal](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. Klicken Sie auf der Registerkarte **Ressourcengruppe** auf **slotDemoResourceGroup**.

    ![Von Terraform erstellte Ressourcengruppe](./media/terraform-slot-walkthru/resource-group.png)

Wenn Sie fertig sind, sehen Sie alle von Terraform erstellten Ressourcen.

![Von Terraform erstellte Ressourcen](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Forken des Testprojekts

Bevor Sie die Erstellung testen und zwischen den Bereitstellungsslots wechseln können, müssen Sie das Testprojekt über GitHub forken.

1. Navigieren Sie zum [Repository „awesome-terraform“ auf GitHub](https://github.com/Azure/awesome-terraform).

1. Forken Sie das **Repository „awesome-terraform“**.

    ![Forken des Repositorys „awesome-terraform“ auf GitHub](./media/terraform-slot-walkthru/fork-repo.png)

1. Befolgen Sie alle Anweisungen zum Forken Ihrer Umgebung.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Bereitstellen über GitHub in Ihren Bereitstellungsslots

Konfigurieren Sie nach dem Forken des Repositorys mit dem Testprojekt die Bereitstellungsslots mithilfe der folgenden Schritte:

1. Wählen Sie im Hauptmenü des Azure-Portals **Ressourcengruppen** aus.

1. Klicken Sie auf **slotDemoResourceGroup**.

1. Klicken Sie auf **slotAppService**.

1. Klicken Sie auf **Bereitstellungsoptionen**.

    ![Bereitstellungsoptionen für eine App Service-Ressource](./media/terraform-slot-walkthru/deployment-options.png)

1. Klicken Sie auf der Registerkarte **Bereitstellungsoption** auf **Quelle auswählen** und dann auf **GitHub**.

    ![Auswählen der Bereitstellungsquelle](./media/terraform-slot-walkthru/select-source.png)

1. Wenn Azure die Verbindung hergestellt hat und alle Optionen anzeigt, klicken Sie auf **Autorisierung**.

1. Klicken Sie auf der Registerkarte **Autorisierung** auf **Autorisieren**, und geben Sie die Anmeldeinformationen ein, die Azure für den Zugriff auf Ihr GitHub-Konto benötigt. 

1. Nach der Überprüfung Ihrer GitHub-Anmeldeinformationen durch Azure wird eine Meldung mit dem Hinweis angezeigt, dass der Autorisierungsprozess abgeschlossen ist. Klicken Sie auf **OK**, um die Registerkarte **Autorisierung** zu schließen.

1. Klicken Sie auf **Organisation auswählen**, und wählen Sie Ihre Organisation aus.

1. Klicken Sie auf **Projekt auswählen**.

1. Wählen Sie auf der Registerkarte **Projekt auswählen** das Projekt **awesome-terraform** aus.

    ![Auswählen des Projekts „awesome-terraform“](./media/terraform-slot-walkthru/choose-project.png)

1. Klicken Sie auf **Verzweigung auswählen**.

1. Klicken Sie auf der Registerkarte **Verzweigung auswählen** auf **master**.

    ![Auswählen der Masterverzweigung](./media/terraform-slot-walkthru/choose-branch-master.png)

1. Klicken Sie auf der Registerkarte **Bereitstellungsoption** auf **OK**.

Nun haben Sie den Produktionsslot bereitgestellt. Führen Sie zum Bereitstellen des Stagingslots erneut die Schritte dieses Abschnitts aus – allerdings mit folgenden Anpassungen:

- Wählen Sie in Schritt 3 die Ressource **slotAppServiceSlotOne**.

- Wählen Sie in Schritt 13 anstelle der Masterverzweigung die Arbeitsverzweigung aus.

    ![Auswählen der Arbeitsverzweigung](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Testen der App-Bereitstellungen

In den vorherigen Abschnitten haben Sie zwei Slots (**slotAppService** und **slotAppServiceSlotOne**) für die Bereitstellung über verschiedene Verzweigungen in GitHub eingerichtet. Sehen wir uns eine Vorschau der Web-Apps an, um sicherzustellen, dass sie erfolgreich bereitgestellt wurden.

Führen Sie die folgenden Schritte zweimal aus. Wählen Sie in Schritt 3 beim ersten Mal **slotAppService** und beim zweiten Mal **slotAppServiceSlotOne** aus:

1. Wählen Sie im Hauptmenü des Azure-Portals **Ressourcengruppen** aus.

1. Klicken Sie auf **slotDemoResourceGroup**.

1. Wählen Sie entweder **slotAppService** oder **slotAppServiceSlotOne** aus.

1. Klicken Sie auf der Übersichtsseite auf **URL**.

    ![Auswählen von „URL“ auf der Registerkarte „Übersicht“ zum Rendern der App](./media/terraform-slot-walkthru/resource-url.png)

> [!NOTE]
> Es kann einige Minuten dauern, bis Azure die Site über GitHub erstellt und bereitgestellt hat.
>
>

Für die Web-App **slotAppService** wird eine blaue Seite mit dem Seitentitel **Slot Demo App 1** angezeigt. Für die Web-App **slotAppServiceSlotOne** wird eine grüne Seite mit dem Seitentitel **Slot Demo App 2** angezeigt.

![Anzeigen einer Vorschau der Apps, um zu testen, ob sie richtig bereitgestellt wurden](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>Wechseln zwischen den zwei Bereitstellungsslots

Wenn Sie das Wechseln zwischen den beiden Bereitstellungsslots testen möchten, führen Sie die folgenden Schritte aus:
 
1. Wechseln Sie zur Browserregisterkarte mit **slotAppService** (die App mit der blauen Seite). 

1. Öffnen Sie das Azure-Portal auf einer separaten Registerkarte.

1. Öffnen Sie Cloud Shell.

1. Wechseln Sie in das Verzeichnis **clouddrive/swap**.

    ```bash
    cd clouddrive/swap
    ```

1. Erstellen Sie mithilfe des vi-Editors eine Datei namens `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Wechseln Sie durch Drücken des Buchstabens `i` in den Einfügemodus.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```JSON
    # Configure the Azure Provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Drücken Sie die **&lt;ESC>**-Taste, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und verlassen Sie den vi-Editor, indem Sie den folgenden Befehl eingeben und die **&lt;EINGABETASTE>** drücken:

    ```bash
    :wq
    ```

1. Initialisieren Sie Terraform.

    ```bash
    terraform init
    ```

1. Erstellen Sie den Terraform-Plan.

    ```bash
    terraform plan
    ```

1. Stellen Sie die in der Konfigurationsdatei `swap.tf` definierten Ressourcen bereit. (Bestätigen Sie die Aktion, indem Sie an der Eingabeaufforderung `yes` eingeben.)

    ```bash
    terraform apply
    ```

1. Nachdem die Slots von Terraform gewechselt wurden, gehen Sie zurück zum Browser, in dem die Web-App **slotAppService** gerendert wird, und aktualisieren Sie die Seite. 

Mit der Web-App im Stagingslot **slotAppServiceSlotOne** wurde in den Produktionsslot gewechselt, und sie wird nun grün gerendert. 

![Ausgetauschte Bereitstellungsslots](./media/terraform-slot-walkthru/slots-swapped.png)

Wenn Sie zur ursprünglichen Produktionsversion der App zurückkehren möchten, wenden Sie den Terraform-Plan erneut an, den Sie auf der Grundlage der Konfigurationsdatei `swap.tf` erstellt haben.

```bash
terraform apply
```

Nach dem Wechsel wird die ursprüngliche Konfiguration angezeigt.