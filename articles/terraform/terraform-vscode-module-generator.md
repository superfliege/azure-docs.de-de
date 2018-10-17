---
title: Azure Terraform VS Code-Modulgenerator
description: Erfahren Sie, wie Sie mit Yeoman eine Terraform-Basisvorlage erstellen.
services: terraform
ms.service: terraform
keywords: Terraform, DevOps, virtueller Computer, Azure, Yeoman
author: v-mavick
manager: jeconnoc
ms.author: v-mavick
ms.topic: tutorial
ms.date: 09/12/2018
ms.openlocfilehash: 513b123c44cf2cd37cf81a91e0d2da9599eb1fcd
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396249"
---
# <a name="create-a-terraform-base-template-using-yeoman"></a>Erstellen einer Terraform-Basisvorlage mithilfe von Yeoman

[Terraform](https://docs.microsoft.com/azure/terraform/
) bietet eine Möglichkeit, mühelos eine Infrastruktur in Azure zu erstellen. [Yeoman](http://yeoman.io/) erleichtert die Aufgabe des Modulentwicklers beim Erstellen von Terraform-Modulen erheblich und bietet gleichzeitig ein hervorragendes Framework *bewährter Methoden*.

In diesem Artikel erfahren Sie, wie Sie mit dem Yeoman-Modulgenerator eine Terraform-Basisvorlage erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Computer mit Windows 10, Linux oder macOS 10.10+.
- **Azure-Abonnement:** Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- **Visual Studio Code**: Mit [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) untersuchen wir Dateien, die der Yeoman-Generator erstellt. Sie können einen Code-Editor Ihrer Wahl verwenden.
- **Terraform**: Sie benötigen eine Installation von [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ), um das von Yeoman erstellte Modul auszuführen.
- **Docker**: Mit [Docker](https://www.docker.com/get-started) führen wir das vom Yeoman-Generator erstellte Modul aus. (Falls gewünscht, können Sie Ruby anstelle von Docker verwenden, um das Beispielmodul auszuführen.)
- **Go-Programmiersprache**: Sie benötigen eine Installation von [Go](https://golang.org/), da die von Yeoman generierten Testfälle in Go geschrieben sind.

>[!NOTE]
>Bei den meisten in diesem Tutorial beschriebenen Verfahren spielen Befehlszeileneinträge eine Rolle. Die hier beschriebenen Schritte gelten für alle Betriebssysteme und Befehlszeilentools. In unseren Beispielen verwenden wir PowerShell. Sie können jedoch eine beliebige von mehreren Alternativen verwenden, z.B. Git Bash, Windows-Eingabeaufforderungen oder Linux- bzw. macOS-Befehlszeilenbefehle.

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

### <a name="install-nodejs"></a>Installieren von Node.js

Sie müssen [Node.js 6.0 oder höher](https://nodejs.org/en/download/) installieren, um Terraform in Cloud Shell zu verwenden.

>[!NOTE]
>Wenn Sie überprüfen möchten, ob Node.js installiert ist, öffnen Sie ein Terminalfenster, und geben Sie `node --version` ein.

### <a name="install-yeoman"></a>Installieren von Yeoman

Geben Sie an einer Eingabeaufforderung `npm install -g yo` ein.

![Installieren von Yeoman](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Installieren der Yeoman-Vorlage für das Terraform-Modul

Geben Sie an einer Befehlszeile `npm install -g generator-az-terra-module` ein.

![Installieren von generator-az-terra-module](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Um sicherzustellen, dass Yeoman installiert ist, geben Sie in einem Terminalfenster `yo --version` ein.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Erstellen eines leeren Ordners zum Speichern des von Yeoman generierten Moduls

Die Yeoman-Vorlage generiert Dateien im **aktuellen Verzeichnis**. Aus diesem Grund müssen Sie ein Verzeichnis erstellen.

>[!Note]
>Dieses leere Verzeichnis muss unter „$GOPATH/src“ erstellt werden. [Hier](https://github.com/golang/go/wiki/SettingGOPATH) finden Sie entsprechende Anweisungen.

Von einer Eingabeaufforderung aus:

1. Navigieren Sie zu dem übergeordneten Verzeichnis, dass das neue, leere Verzeichnis enthalten soll, das wir nun erstellen.
1. Geben Sie `mkdir <new-directory-name>` ein.

    >[!NOTE]
    >Ersetzen Sie <new-directory-name> durch den Namen des neuen Verzeichnisses ein. In diesem Beispiel haben wir das neue Verzeichnis `GeneratorDocSample` genannt.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. Wechseln Sie mit Eingabe von `cd <new directory's name>` in das neue Verzeichnis, und drücken Sie dann die **EINGABETASTE**.

    ![Navigieren Sie zu Ihrem neuen Verzeichnis.](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Um sicherzustellen, dass dieses Verzeichnis leer ist, geben Sie `ls` ein. Es sollten keine Dateien in der resultierenden Ausgabe dieses Befehls aufgelistet werden.

## <a name="create-a-base-module-template"></a>Erstellen einer Basismodulvorlage

Von einer Eingabeaufforderung aus:

1. Geben Sie `yo az-terra-module` ein.

1. Führen Sie die Anweisungen auf dem Bildschirm aus, um die folgenden Informationen anzugeben:

    - *Name des Terraform-Modulprojekts*

        ![Projektname](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >In diesem Beispiel haben wir `doc-sample-module` eingegeben.

    - *Möchten Sie die Docker-Imagedatei einbeziehen?*

        ![Docker-Imagedatei einbeziehen?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >Geben Sie `y` ein. Bei Auswahl von **n** unterstützt der generierte Modulcode nur die Ausführung im nativen Modus.

3. Geben Sie `ls` ein, um die resultierenden Dateien anzuzeigen, die erstellt werden.

    ![Erstellte Dateien auflisten](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Überprüfen des generierte Modulcodes

1. Starten Sie Visual Studio Code.

1. Wählen Sie in der Menüleiste **Datei > Ordner öffnen** aus, und wählen Sie den Ordner aus, den Sie erstellt haben.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Werfen wir einen Blick auf einige der Dateien, die vom Yeoman-Modulgenerator erstellt wurden.

>[!Note]
>In diesem Artikel verwenden wir die vom Yeoman-Modulgenerator erstellten Dateien „main.tf“, „variables.tf“ und „outputs.tf“. Wenn Sie allerdings Ihre eigenen Module erstellen, bearbeiten Sie diese Dateien, um die Funktionalität Ihres Terraform-Moduls anzupassen. Eine ausführliche Erläuterung dieser Dateien und ihrer Verwendung finden Sie unter [Terratest in Terraform Modules (Terratest in Terraform-Modulen)](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster).

### <a name="maintf"></a>main.tf

Definiert ein Modul namens *random-shuffle*. Die Eingabe ist eine *string_list*. Die Ausgabe ist die Anzahl der Permutationen.

### <a name="variablestf"></a>variables.tf

Definiert die vom Modul verwendeten Eingabe- und Ausgabevariablen.

### <a name="outputstf"></a>outputs.tf

Definiert, was das Modul ausgibt. Hier ist es der von **random_shuffle** zurückgegebene Wert, ein integriertes Terraform-Modul.

### <a name="rakefile"></a>Rakefile

Definiert die Schritte im Buildprozess. Diese Schritte umfassen:

- **build**: Überprüft die Formatierung der Datei „main.tf“.
- **unit**: Das generierte Modulgerüst enthält keinen Code für einen Komponententest. Wenn Sie ein Komponententestszenario angeben möchten, fügen Sie diesen Code hier hinzu.
- **e2e**: Führt einen End-to-End-Test des Moduls aus.

### <a name="test"></a>test

- Testfälle werden in Go geschrieben.
- Alle Codes im Test sind End-to-End-Tests.
- End-to-End-Tests versuchen, mit Terraform alle Elemente bereitzustellen, die unter **fixture** definiert sind, und dann die Ausgabe im **template_output.go**-Code mit den vordefinierten erwarteten Werten zu vergleichen.
- **Gopkg.lock** und **Gopkg.toml**: Definieren Sie Ihre Abhängigkeiten. 

## <a name="test-the-module-using-docker"></a>Testen des Moduls mithilfe von Docker

>[!NOTE]
>In unserem Beispiel führen wir das Modul als lokales Modul aus, sodass Azure nicht beteiligt ist.

### <a name="confirm-docker-is-installed-and-running"></a>Bestätigen, dass Docker installiert ist und ausgeführt wird

Geben Sie an einer Befehlszeile `docker version` ein.

![Docker-Version](media/terraform-vscode-module-generator/ymg-docker-version.png)

Die resultierende Ausgabe bestätigt, dass Docker installiert ist.

Um zu bestätigen, dass Docker wirklich ausgeführt wird, geben Sie `docker info` ein.

![Docker-Info](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Einrichten eines Docker-Containers

1. Geben Sie an einer Eingabeaufforderung Folgendes ein:

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

    Die Nachricht **Erfolgreich erstellt** wird angezeigt.

    ![Erfolgreich erstellt](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Geben Sie an der Eingabeaufforderung `docker image ls` ein.

    Ihr neu erstelltes Modul *terra-mod-example* ist aufgelistet.

    ![Repositoryergebnisse](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >Der Modulname *terra-mod-example* wurde in dem Befehl angegeben, den Sie oben in Schritt 1 eingegeben haben.

1. Geben Sie `docker run -it terra-mod-example /bin/sh` ein.

    Sie führen jetzt Docker aus und können die Datei durch Eingabe von `ls` auflisten.

    ![Docker-Datei auflisten](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

1. Geben Sie `bundle install` ein.

    Warten Sie, bis die Nachricht **Bundle abgeschlossen** angezeigt wird, und fahren Sie mit dem nächsten Schritt fort.

1. Geben Sie `rake build` ein.

    ![Rake build](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="perform-the-end-to-end-test"></a>Durchführen des End-to-End-Tests

1. Geben Sie `rake e2e` ein.

1. Nach einigen Augenblicken wird die Meldung **ERFOLGREICH** angezeigt.

    ![ERFOLGREICH](media/terraform-vscode-module-generator/ymg-pass.png)

1. Geben Sie `exit` ein, um den End-to-End-Test abzuschließen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Installieren und Verwenden der Azure Terraform-Erweiterung für Visual Studio Code](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
