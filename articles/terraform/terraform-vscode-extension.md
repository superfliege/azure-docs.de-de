---
title: Azure Terraform-Erweiterung für VS Code | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie die Terraform-Erweiterung in Visual Studio Code installiert und verwendet wird.
keywords: Terraform, DevOps, virtueller Computer, Azure
author: v-mavick
ms.author: v-mavick
ms.date: 08/14/2018
ms.topic: article
ms.prod: vs-code
ms.openlocfilehash: 0c88879faae100372055479ad4edb8c36d8f557d
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190998"
---
# <a name="azure-terraform-vs-code-extension"></a>Azure Terraform-Erweiterung für VS Code

Die Microsoft Azure Terraform-Erweiterung für Visual Studio Code (VS Code) wurde entwickelt, um die Produktivität des Entwicklers beim Erstellen und Testen mithilfe von Terraform mit Azure zu verbessern. Die Erweiterung bietet Unterstützung für Terraform-Befehle, Visualisierung von Ressourcendiagrammen und die Cloud Shell-Integration in VS Code.

## <a name="what-you-do"></a>Aufgaben

- Installieren Sie die ausführbare Open Source-Datei für HashiCorp Terraform auf Ihrem Computer.
- Installieren Sie die Terraform-Erweiterung für Azure in VS Code in Ihrer lokalen Installation von VS Code.

## <a name="what-you-learn"></a>Lerninhalt

In diesem Tutorial lernen Sie Folgendes:

- Wie Terraform die Bereitstellung von Azure-Diensten automatisieren und vereinfachen kann.
- Wie die Microsoft Terraform-Erweiterung für VS Code für Azure-Dienste installiert und verwendet werden kann.
- Wie VS Code zum Schreiben, Planen und Ausführen von Terraform-Plänen verwendet werden kann.

## <a name="what-you-need"></a>Voraussetzungen

- Ein Computer mit Windows 10, Linux oder macOS 10.10+.
- [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US).
- Ein aktives Azure-Abonnement. [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azure.microsoft.com/free/) aktivieren.
- Eine Installation des Open Source-Tools [Terraform](https://www.terraform.io/) auf Ihrem lokalen Computer.
  
## <a name="prepare-your-dev-environment"></a>Vorbereiten Ihrer Entwicklungsumgebung

### <a name="install-git"></a>Installation von Git

Für die Übungen in diesem Artikel müssen Sie [Git installieren](https://git-scm.com/).

### <a name="install-hashicorp-terraform"></a>Installieren von HashiCorp Terraform

Befolgen Sie die Anweisungen auf der HashiCorp-Webseite zum [Installieren von Terraform](https://www.terraform.io/intro/getting-started/install.html). Diese enthalten Folgendes:

- Das Herunterladen von Terraform
- Das Installieren von Terraform
- Das Überprüfen, ob Terraform ordnungsgemäß installiert wurde

>[!Tip]
>Achten Sie darauf, die Anweisungen für das Festlegen Ihrer PATH-Systemvariable zu befolgen.

### <a name="install-nodejs"></a>Installieren von Node.js

Sie müssen [Node.js 6.0 oder höher](https://nodejs.org/) installieren, um Terraform in Cloud Shell zu verwenden.

>[!NOTE]
>Wenn Sie überprüfen möchten, ob Node.js installiert ist, öffnen Sie ein Terminalfenster, und geben Sie `node -v` ein.

### <a name="install-graphviz"></a>Installieren von GraphViz

Sie müssen [GraphViz installieren](http://graphviz.org/), um die Visualisierungsfunktion von Terraform zu verwenden.

>[!NOTE]
>Wenn Sie überprüfen möchten, ob GraphViz installiert ist, öffnen Sie ein Terminalfenster, und geben Sie `dot -V` ein.

### <a name="install-the-azure-terraform-vs-code-extension"></a>Installieren der Azure Terraform-Erweiterung für VS Code

1. Starten Sie VS Code.
2. Klicken Sie auf das Symbol für **Erweiterungen**.

    ![Schaltfläche „Erweiterungen“](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

3. Verwenden Sie das Textfeld zum **Suchen nach Erweiterungen in Marketplace**, um nach der Azure Terraform-Erweiterung zu suchen:

    ![Suchen von VS Code-Erweiterungen in Marketplace](media/terraform-vscode-extension/tf-search-extensions.png)

4. Klicken Sie auf **Installieren**.

    >[!NOTE]
    >Wenn Sie für die Azure Terraform-Erweiterung auf **Installieren** klicken, installiert VS Code automatisch die Erweiterung „Azure Account“. Azure Account ist eine Abhängigkeitsdatei für die Azure Terraform-Erweiterung, die verwendet wird, um Authentifizierungen für Azure-Abonnements sowie auf Azure bezogene Codeerweiterungen durchzuführen.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Überprüfen, ob die Terraform-Erweiterung in Visual Studio Code installiert ist

1. Klicken Sie auf das Symbol für Erweiterungen.
2. Geben Sie `@installed` in das Textfeld für die Suche ein.

    ![Installierte Erweiterungen](media/terraform-vscode-extension/tf-installed-extensions.png)

Die Azure Terraform-Erweiterung wird in der Liste der installierten Erweiterungen angezeigt.

![Installierte Terraform-Erweiterungen](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

Sie können nun alle unterstützten Terraform-Befehle in Ihrer Cloud Shell-Umgebung über VS Code ausführen.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Übung 1: Grundlegende Terraform-Befehle

In dieser Übung erstellen Sie eine grundlegende Terraform-Konfigurationsdatei, die eine neue Azure-Ressourcengruppe bereitstellt, und führen diese Datei aus.

### <a name="prepare-a-test-plan-file"></a>Vorbereiten einer Testplandatei

1. Klicken Sie in VS Code auf der Menüleiste auf **Datei > Neue Datei**.
2. Navigieren Sie zu [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#), und kopieren Sie den Code im Codeblock **Beispielverwendung**.
3. Fügen Sie den kopierten Code in die neue Datei ein, die Sie in VS Code erstellt haben.

    ![Einfügen des Beispielverwendungscodes](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >Sie können den Wert **name** der Ressourcengruppe ändern, dieser muss jedoch für Ihr Azure-Abonnement eindeutig sein.

4. Klicken Sie auf der Menüleiste auf **Datei > Speichern unter**.
5. Navigieren Sie im Dialogfeld **Speichern unter** zu einem Speicherort Ihrer Wahl, und klicken Sie dann auf **Neuer Ordner**. (Ändern Sie den Namen des neuen Ordners in einen aussagekräftigeren Namen als *Neuer Ordner*.)

    >[!NOTE]
    >In diesem Beispiel wird der Ordner „TERRAFORM-TEST-PLAN“ genannt.

6. Stellen Sie sicher, dass der Ordner ausgewählt ist, und klicken Sie dann auf **Öffnen**.
7. Ändern Sie im Dialogfeld **Speichern unter** den Standardnamen der Datei in *main.tf*.

    ![Speichern unter; „main.tf“](media/terraform-vscode-extension/tf-save-as-main.png)

8. Klicken Sie auf **Speichern**.
- Klicken Sie auf der Menüleiste auf **Datei > Ordner öffnen**. Navigieren Sie zum neu erstellten Ordner, und wählen Sie diesen aus.

### <a name="run-terraform-init-command"></a>Ausführen des Terraform-Befehls *init*

1. Starten Sie Visual Studio Code.
2. Klicken Sie auf der Menüleiste von VS Code auf **Datei > Ordner öffnen...**, suchen Sie die Datei *main.tf*, und wählen Sie diese aus.

    ![main.tf-Datei](media/terraform-vscode-extension/tf-main-tf.png)

3. Klicken Sie auf der Menüleiste auf **Ansicht > Befehlspalette > Azure Terraform: Init**.
4. Kurz darauf werden Sie gefragt: *Do you want to open Cloud Shell?* (Möchten Sie Cloud Shell öffnen?) Klicken Sie auf **OK**.

    ![Möchten Sie Cloud Shell öffnen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

5. Wenn Sie Cloud Shell zum ersten Mal über einen neuen Ordner starten, werden Sie dazu aufgefordert, die Webanwendung einzurichten. Klicken Sie auf **Öffnen**.

    ![Ersten Start von Cloud Shell](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

6. Die Seite „Willkommen bei Azure Cloud Shell“ wird geöffnet. Wählen Sie „Bash“ oder „PowerShell“ aus.

    ![Willkommen bei Azure Cloud Shell](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >In diesem Beispiel wurde „Bash“ (Linux) ausgewählt.

7. Wenn Sie noch kein Azure-Speicherkonto eingerichtet haben, wird folgender Bildschirm angezeigt. Klicken Sie auf **Speicher erstellen**.

    ![Für Sie wurde kein Speicher bereitgestellt.](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell wird in der Shell gestartet, die Sie zuvor ausgewählt haben, und zeigt Informationen zu dem Cloudlaufwerk an, das gerade für Sie erstellt wurde.

    ![Cloudlaufwerk wurde erstellt](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

9. Sie können Cloud Shell nun beenden.
10. Klicken Sie auf der Menüleiste auf **Ansicht** > **Befehlspalette** > **Azure Terraform: Init**.

    ![Terraform wurde erfolgreich initialisiert.](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Visualisieren des Plans

Im bisherigen Verlauf dieses Tutorials haben Sie GraphViz installiert. Terraform kann GraphViz verwenden, um eine visuelle Darstellung einer Konfiguration oder eines Ausführungsplans zu generieren. Die Azure Terraform-Erweiterung für VS Code implementiert dieses Feature über den Befehl *visualize*.

- Klicken Sie auf der **Menüleiste** auf **Ansicht > Befehlspalette > Azure Terraform: Visualize**.

    ![Visualisieren des Plans](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Ausführen des Terraform-Befehls *plan*

Der Terraform-Befehl *plan* wird verwendet, um zu überprüfen, ob der Ausführungsplan für Änderungen wie gewünscht funktioniert.

>[!NOTE]
>Der Terraform-Befehl *plan* nimmt keine Änderungen an Ihren Azure-Ressourcen vor. Wenn Sie die Änderungen in Ihrem Plan tatsächlich anwenden möchten, verwenden Sie den Terraform-Befehl *apply*.

- Klicken Sie auf der Menüleiste auf **Ansicht** > **Befehlspalette** > **Azure Terraform: Plan**.

    ![Terraform-Befehl „plan“](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Ausführen des Terraform-Befehls *apply*

Wenn Sie mit den Ergebnissen des Terraform-Befehls *plan* zufrieden sind, können Sie den Befehl *apply* ausführen.

1. Klicken Sie auf der Menüleiste auf **Ansicht** > **Befehlspalette** > **Azure Terraform: Apply**.

    ![Terraform-Befehl „apply“](media/terraform-vscode-extension/tf-terraform-apply.png)

2. Geben Sie *yes* ein.

    ![Terraform-Befehl „apply“: Wert „yes“](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Überprüfen, ob Ihr Terraform-Plan ausgeführt wurde

So stellen Sie fest, ob Ihre neue Azure-Ressourcengruppe erfolgreich erstellt wurde:

1. Öffnen Sie das Azure-Portal.
2. Klicken Sie im linken Navigationsbereich auf **Ressourcengruppen**.

    ![Überprüfen Ihrer neuen Ressource](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

Ihre neue Ressourcengruppe sollte in der Spalte **NAME** aufgeführt werden.

>[!NOTE]
>Sie können das Fenster für das Azure-Portal geöffnet lassen, denn es wird im nächsten Schritt verwendet.

### <a name="run-terraform-destroy-command"></a>Ausführen des Terraform-Befehls *destroy*

1. Klicken Sie auf der Menüleiste auf **Ansicht** > **Befehlspalette** > **Azure Terraform: Destroy**.

    ![Terraform-Befehl „destroy“](media/terraform-vscode-extension/tf-terraform-destroy.png)

2. Geben Sie *yes* ein.

    ![Terraform-Befehl „destroy“: Wert „yes“](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Überprüfen, ob Ihre Ressourcengruppe gelöscht wurde

So bestätigen Sie, dass Terraform Ihre neue Ressourcengruppe erfolgreich gelöscht hat:

1. Klicken Sie auf der Seite *Ressourcengruppen* des Azure-Portals auf **Aktualisieren**.
2. Ihre Ressourcengruppe wird nicht mehr aufgeführt.

    ![Überprüfen, ob eine Ressourcengruppe gelöscht wurde](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Übung 2: *Terraform-Computemodul*

In dieser Übung lernen Sie, wie Sie das *Terraform-Computemodul* in die VS Code-Umgebung laden können.

### <a name="clone-the-terraform-azurerm-compute-module"></a>Klonen des Moduls „terraform-azurerm-compute“

1. Verwenden Sie [diesen Link](https://github.com/Azure/terraform-azurerm-compute), um auf das Modul „terraform-azurerm-compute“ auf GitHub zuzugreifen.
2. Klicken Sie auf **Klonen oder herunterladen**.

    ![Klonen oder herunterladen](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >In diesem Beispiel wurde der Ordner *terraform-azurerm-compute* genannt.

### <a name="open-the-folder-in-vs-code"></a>Öffnen des Ordners in VS Code

1. Starten Sie Visual Studio Code.
2. Klicken Sie auf der **Menüleiste** auf **Datei > Ordner öffnen**, navigieren Sie zu dem Ordner, den Sie im vorherigen Schritt erstellt haben, und öffnen Sie diesen.

    ![Ordner „terraform-azurerm-compute“](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Initialisieren von Terraform

Bevor Sie die Terraform-Befehle in VS Code verwenden können, müssen Sie die Plug-Ins für zwei Azure-Anbieter herunterladen: random und azurerm.

1. Geben Sie im Terminalbereich der VS Code-IDE `terraform init` ein.

    ![Terraform-Befehl „init“](media/terraform-vscode-extension/tf-terraform-init-command.png)

2. Geben Sie `az login` ein, und befolgen Sie die Anweisungen auf dem Bildschirm.

### <a name="module-test-lint"></a>Modultest: *lint*

1. Klicken Sie auf der **Menüleiste** auf **Ansicht > Befehlspalette > Azure Terraform: Execute Test**.
2. Wählen Sie aus der Liste der Testtypoptionen **lint** aus.

    ![Auswählen des Testtyps](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

3. Wenn Sie gefragt werden, ob Sie *Cloud Shell öffnen möchten*, klicken Sie auf **OK**, und befolgen Sie die Anweisungen auf dem Bildschirm.

    ![Möchten Sie Cloud Shell öffnen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Wenn Sie den **lint**- oder den **end to end**-Test durchführen, verwendet Azure einen Containerdienst, um einen Testcomputer für den eigentlichen Test bereitzustellen. Deshalb kann es mehrere Minuten dauern, bis Ihre Testergebnisse zurückgegeben werden.

Nach einigen Augenblicken wird eine Liste im Terminalbereich angezeigt, die folgendem Beispiel ähnelt:

![Testergebnisse von „lint“](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="module-test-end-to-end"></a>Modultest: *end to end*

1. Klicken Sie auf der **Menüleiste** auf **Ansicht > Befehlspalette > Azure Terraform: Execute Test**.
2. Wählen Sie aus der Liste der Testtypoptionen **end to end** aus.

    ![Auswählen des Testtyps](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

3. Wenn Sie gefragt werden, ob Sie *Cloud Shell öffnen möchten*, klicken Sie auf **OK**, und befolgen Sie die Anweisungen auf dem Bildschirm.

    ![Möchten Sie Cloud Shell öffnen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Wenn Sie den **lint**- oder den **end to end**-Test durchführen, verwendet Azure einen Containerdienst, um einen Testcomputer für den eigentlichen Test bereitzustellen. Deshalb kann es mehrere Minuten dauern, bis Ihre Testergebnisse zurückgegeben werden.

Nach einigen Augenblicken wird eine Liste im Terminalbereich angezeigt, die folgendem Beispiel ähnelt:

![Testergebnisse von „end to end“](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun einige Möglichkeiten kennengelernt, wie Terraform die Bereitstellung von Azure-Diensten über Visual Studio Code vereinfachen kann. Nun können Sie mit einer der folgenden Ressourcen fortfahren:
- Unter [Terraform Module Registry (Terraform-Modulregistrierung)](https://registry.terraform.io/) werden alle Terraform-Module aufgeführt, die für Azure und andere unterstützte Anbieter verfügbar sind.

Folgende Informationen werden für jedes dieser Module angegeben:

- Eine Beschreibung der allgemeinen Funktionen und Eigenschaften des Moduls
- Ein Verwendungsbeispiel
- Testkonfigurationen, die veranschaulichen, wie Sie das entsprechende Modul auf Ihrem lokalen Entwicklungscomputer erstellen, ausführen und testen
- Eine Dockerfile, um eine Modulentwicklungsumgebung lokal zu erstellen und auszuführen.
