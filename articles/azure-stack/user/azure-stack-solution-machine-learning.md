---
title: Erstellen einer Machine Learning-Edge-Lösung mit Azure und Azure Stack | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Machine Learning-Edge-Lösung erstellen, indem Sie Python mit Azure und Azure Stack verwenden.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 30dbdff584f1bea955072e96a5e0f03cfe4c92c1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963729"
---
# <a name="tutorial-create-an-edge-machine-learning-solution-with-azure-and-azure-stack"></a>Tutorial: Erstellen einer Machine Learning-Edge-Lösung mit Azure und Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Es wird beschrieben, wie Sie eine Machine Learning-Edge-Lösung erstellen, indem Sie Azure und Azure Stack verwenden.

In diesem Tutorial erstellen Sie eine Beispielumgebung, die Folgendes ermöglicht:

> [!div class="checklist"]
> - Erstellen eines Speicherkontos und eines Containers für bereinigte Daten
> - Erstellen einer Ubuntu Data Science Virtual Machine (DSVM) im Azure-Portal
> - Bereitstellen von Azure Machine Learning Services in Azure, um Modelle zu erstellen und zu trainieren
> - Erstellen von Azure Machine Learning Services-Konten
> - Bereitstellen und Verwenden von Azure Container Registry
> - Bereitstellen eines Kubernetes-Clusters in Azure Stack
> - Erstellen eines Azure Stack-Speicherkontos und einer Speicherwarteschlange für Daten
> - Erstellen einer neuen Azure Stack-Funktion zum Verschieben der bereinigten Daten aus Azure Stack nach Azure

**Beispiele für die Verwendung dieser Lösung**

 -  Ihre Organisation nutzt einen DevOps-Ansatz oder hat dies für die nahe Zukunft geplant.
 -  Sie möchten CI/CD-Methoden für Ihre gesamte Azure Stack-Implementierung und die öffentliche Cloud implementieren.
 -  Sie möchten die CI/CD-Pipeline für die Cloud- und lokalen Umgebungen konsolidieren.
 -  Sie möchten die Möglichkeit haben, Anwendungen mit Cloud- oder lokalen Diensten zu entwickeln.
 -  Sie möchten einheitliche Entwicklerfähigkeiten für Cloud- und lokale Anwendungen nutzen.

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack ist eine Erweiterung von Azure. Mit Azure Stack holen Sie sich die Agilität und Innovation von Cloud Computing in Ihre lokale Umgebung. Sie erhalten die einzige Hybrid Cloud, mit der Sie Hybrid-Apps überall entwickeln und bereitstellen können.  
> 
> Im Whitepaper [Design Considerations for Hybrid Applications](https://aka.ms/hybrid-cloud-applications-pillars) (Überlegungen zum Entwurf für Hybridanwendungen) werden die wichtigen Aspekte in Bezug auf die Softwarequalität (Platzierung, Skalierbarkeit, Verfügbarkeit, Resilienz, Verwaltbarkeit und Sicherheit) beschrieben, die für das Entwerfen, Bereitstellen und Betreiben von Hybridanwendungen erforderlich sind. Die Überlegungen zum Entwurf dienen als Hilfe beim Optimieren des Designs von Hybridanwendungen, um für Produktionsumgebungen das Auftreten von Problemen zu minimieren.

## <a name="prerequisites"></a>Voraussetzungen

Es sind einige Komponenten und etwas Vorbereitungszeit erforderlich, um diesen Anwendungsfall zu erstellen:

 -  Ein Azure-OEM/-Hardwarepartner kann eine Azure Stack-Produktionsumgebung bereitstellen. Ein ASDK kann von allen Benutzern bereitgestellt werden.

 -  Ein Azure Stack-Bediener muss ebenfalls App Service bereitstellen, Pläne und Angebote sowie ein Mandantenabonnement erstellen und das Windows Server 2016-Image hinzufügen.

 -  Ein Hybridnetzwerk und ein App Service-Setup sind erforderlich (weitere Informationen zur [App-Integration in VNETs](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)).

 -  Der private [Build- und Release-Agent](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts) für die VSTS-Integration muss vor der Bereitstellung eingerichtet werden. (Stellen Sie sicher, dass alle verwendeten Komponenten die Anforderungen erfüllen, bevor Sie beginnen.)

Vorkenntnisse in Bezug auf Azure und Azure Stack sind erforderlich. Beginnen Sie mit diesen Themen, um sich weiter zu informieren, bevor Sie fortfahren:

 -  [Einführung in Azure](https://azure.microsoft.com/overview/what-is-azure/)

 -  [Azure Stack Key Concepts (Wichtige Begriffe für Azure Stack)](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

 -  [Azure Stack-CI/CD-Hybridlösung – Leitfaden](/azure-stack-solution-pipeline.md)

**Azure**

 -  Ein Azure-Abonnement. (Erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).)

 -  Eine neue Web-App-URL, die per [Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) in Azure erstellt wurde.

 -  Bereitstellung von [Azure Container Services (ACS) Kubernetes in Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

 -  Bereitstellung des Azure Machine Learning-Diensts (Vorschauversion) [Vierteiliges Tutorial](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/tutorial-classifying-iris-part-1)

 -  Azure Machine Learning-Experimentieren-[Konto](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/experimentation-service-template)

**Azure Stack**

 -  Ein integriertes Azure Stack-System oder eine Bereitstellung des Azure Stack Development Kit.

    - Eine Anleitung zur Installation von Azure Stack finden Sie unter [Installieren des Azure Stack Development Kit](/articles/azure-stack/asdk/asdk-install).
     - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Es kann einige Stunden dauern, bis diese Installation abgeschlossen ist.

 -  Bereitstellung von [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy)-PaaS-Diensten für Azure Stack

 -  [Pläne/Angebote](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in der Azure Stack-Umgebung

 -  Ein [Mandantenabonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) in der Azure Stack-Umgebung

 -  Ein Ubuntu Server-VM-Image (über den [Azure Stack Marketplace](https://buildazure.com/2016/05/04/azure-marketplace-ubuntu-server-16-04-lts/) erhältlich). Diese VM wird im Mandantenabonnement von Azure Stack als privater Build-Agent und auf den Kubernetes-VMs erstellt. Wenn dieses Image nicht verfügbar ist, kann der Azure Stack-Bediener assistieren, um sicherzustellen, dass es der Umgebung hinzugefügt wird. (Verwenden Sie nicht den Build 18.04 von Ubuntu, da er derzeit nicht unterstützt wird.)

 -  Eine Web-App unter dem Mandantenabonnement (Notieren Sie sich die neue Web-App-URL zur späteren Verwendung.)

 -  Bereitstellung des auf VSTS Linux basierenden virtuellen Computers für den privaten Build-Agent unter dem Mandantenabonnement

 -  Bereitstellung einer Instanz von [Azure Container Services (ACS) Kubernetes für Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

**Entwicklertools**

 -  [VSTS-Arbeitsbereich](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) (Bei der Anmeldung wird ein Projekt mit dem Namen „MyFirstProject“ erstellt.)

 -  [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio)-Installation und [VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services)-Anmeldung

 -  [Lokaler Klon](https://www.visualstudio.com/docs/git/gitquickstart) des Projekts

 -  Installation des [Linux-Subsystems für Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) (für BASH und SSH)

 -  Installation von [Docker für Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)

 -  Installation von [Azure Machine Learning Workbench (Vorschauversion)](https://aka.ms/azureml-wb-msi)

 -  Installation der [Python](https://www.python.org/ftp/python/3.7.0/python-3.7.0rc1-amd64.exe)-Umgebung

**VSTS**

 -  **VSTS-Konto** Nutzen Sie das schnelle Einrichten von Continuous Integration für Build, Test und Bereitstellung. Weitere Informationen zu VSTS-Konten finden Sie unter [Create the VSTS Account](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student?view=vsts) (Erstellen des VSTS-Kontos).

 -  **Coderepository**: Bei Verwendung von vorhandenen Coderepositorys in GitHub, BitBucket, Dropbox, OneDrive und TFS kann die VSTS-Plattform mehrere Coderepositorys nutzen, um die Entwicklungspipeline zu optimieren. Weitere Informationen zu Coderepositorys finden Sie im Tutorial [Get Started with Azure Repos and Visual Studio](https://docs.microsoft.com/vsts/git/gitquickstart?view=vsts&tabs=visual-studio) (Erste Schritte mit Azure-Repositorys und Visual Studio).

 -  **Dienstverbindung**: Stellen Sie eine Verbindung mit externen und Remotediensten her, um Aufgaben für Tests, Builds und Bereitstellungen durchzuführen. Weitere Informationen zu VSTS-Dienstverbindungen finden Sie unter [Service connections for builds and releases](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints?view=vsts) (Dienstverbindungen für Builds und Releases).

 -  **Builddefinitionen**: Definieren Sie automatisierte Buildprozesse, und stellen Sie über den Aufgabenkatalog eine Gruppe von Aufgaben zusammen. Weitere Informationen zu Builddefinitionen finden Sie unter [Erstellen einer Builddefinition](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts).

 -  **Releasedefinitionen**: Definieren Sie den Bereitstellungsprozess für eine Sammlung mit Umgebungen, in der Anwendungsartefakte bereitgestellt werden. Weitere Informationen zu Releasedefinitionen finden Sie unter [Erstellen einer Releasedefinition](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts).

 -  **Pool mit gehosteten VSTS Linux-Build-Agents**: Ermöglicht das schnelle Erstellen, Testen und Bereitstellen von Anwendungen mit einem von Microsoft verwalteten und gewarteten gehosteten Agent. Weitere Informationen zu gehosteten VSTS-Build-Agents finden Sie in der Dokumentation zu [gehosteten Agents](https://docs.microsoft.com/vsts/build-release/concepts/agents/hosted?view=vsts).

## <a name="step-1-create-a-storage-account"></a>Schritt 1: Erstellen eines Speicherkontos

Erstellen eines Speicherkontos und eines Containers für bereinigte Daten

1.  Melden Sie sich am [*Azure-Portal*](https://portal.azure.com/) an.

2.  Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Alle Dienste**. Scrollen Sie nach unten zu **Speicher**, und wählen Sie **Speicherkonten**. Wählen Sie im Fenster **Speicherkonten** die Option **Hinzufügen**.

3.  Geben Sie einen Namen für das Speicherkonto ein.

    > [!Note]  
    > Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Der Name des Speicherkontos muss innerhalb von Azure eindeutig sein. Im Azure-Portal wird angezeigt, ob der ausgewählte Speicherkontoname bereits verwendet wird.

4.  Geben Sie das Bereitstellungsmodell an, das verwendet werden soll: **Resource Manager**.

5.  Wählen Sie den Typ des Speicherkontos aus: **Allgemein V1**. Geben Sie anschließend die Leistungsstufe an: **Standard**.

6.  Wählen Sie die Replikationsoption für das Speicherkonto aus: **GRS**.

7.  Wählen Sie ein neues Speicherkontoabonnement aus.

8.  Geben Sie eine neue Ressourcengruppe an, oder wählen Sie eine vorhandene Ressourcengruppe aus.

9.  Wählen Sie den geografischen Standort für das Speicherkonto aus.

10. Wählen Sie **Erstellen**, um das Speicherkonto zu erstellen.

    ![Alt text](\media\azure-stack-solution-machine-learning\image1.png)

11.  Wählen Sie das vor Kurzem erstellte Speicherkonto aus.

12.  Wählen Sie **Blobs**.

    ![Alt text](media\azure-stack-solution-machine-learning\image2.png)

13.  Wählen Sie **+ Container** und dann **Container**.

    ![Alt text](media\azure-stack-solution-machine-learning\image3.png)

14.  Geben Sie als Name des Containers **uploadeddata** ein, und wählen Sie den Zugriffstyp **Container**.

15.  Wählen Sie **Erstellen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image4.png)

## <a name="step-2-create-a-data-science-virtual-machine"></a>Schritt 2: Erstellen einer Data Science Virtual Machine

Erstellen Sie eine Ubuntu Data Science Virtual Machine (DSVM) im Azure-Portal.

1.  Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com/) am Azure-Portal an.

2.  Wählen Sie den Link **+ NEU**, und suchen Sie nach „Virtueller Data Science-Computer für Linux Ubuntu CSP“.

    ![Alt text](media\azure-stack-solution-machine-learning\image5.png)

1.  Wählen Sie in der Liste **Virtuelle Data Science-Computer für Linux (Ubuntu)** aus, und führen Sie die Anweisungen auf dem Bildschirm aus, um die DSVM zu erstellen.

    ![Alt text](media\azure-stack-solution-machine-learning\image6.png)

> ![Wichtig]  
> **Wählen Sie** Kennwort** als *Authentifizierungstyp*.

Ordnen Sie die neue DSVM in derselben Ressourcengruppe wie das neu erstellte Speicherkonto an. Alle ML-Edgeobjekte werden in Azure in dieser Ressourcengruppe bereitgestellt.

1.  Unter „Einstellungen“ > „Optionale Features konfigurieren“:

    a.  Wählen Sie das zuvor erstellte **Speicherkonto** aus.

    b.  Erstellen Sie ein neues **virtuelles Netzwerk**, ein **Subnetz** und eine **öffentliche IP-Adresse**. Standardmäßig sollte eine Name erstellt werden, der auf dem Ressourcengruppennamen basiert.

    c.  Erstellen Sie eine neue **NSG**. Die NSG sollte automatisch erstellt werden, und die richtigen Regeln sollten bereits angewendet worden sein.

    d.  Wählen Sie unter **Diagnosespeicherkonto** das zuvor erstellte Speicherkonto aus.

    e.  Hinweis: Wenn AAD für das Azure-Abonnement aktiviert und konfiguriert wurde, kann auch die verwaltete Dienstidentität aktiviert werden.

2.  Klicken Sie auf **OK**.

### <a name="connect-to-the-dsvm"></a>Herstellen einer Verbindung mit der DSVM

Stellen Sie nach der Erstellung der DSVM über das Windows-Subsystem für Linux eine Verbindung mit der VM her.

```Bash  
    ssh <user>@<DSVM Public IP>
```

1.  Wählen Sie „JA“ ein, wenn Sie zum Bestätigen der VM-Verbindung aufgefordert werden.

2.  Geben Sie das für die DSVM erstellte Kennwort ein.

### <a name="update-the-dsvm"></a>Aktualisieren der DSVM 

```Bash  
sudo su 
apt-get update 
apt-get -y upgrade 
apt-get -y dist-upgrade 
apt-get -y autoremove
```

## <a name="step-3-deploy-azure-machine-learning-services"></a>Schritt 3: Bereitstellen von Azure Machine Learning Services

Stellen Sie Azure Machine Learning Services in Azure bereit.

Azure Machine Learning Services (Vorschauversion) sind eine integrierte End-to-End-Lösung für Data Science und Advanced Analytics. Sie stellt eine Hilfe für professionelle Datenanalysten dar, um Daten vorzubereiten, Experimente zu entwickeln und Modelle für die Cloud bereitzustellen.

In diesem Abschnitt wird Folgendes beschrieben:

> [!div class="checklist"]
> - Erstellen von Dienstkonten für Azure Machine Learning-Dienste
> - Installieren von Azure Machine Learning Workbench und Anmeldung
> - Erstellen eines Projekts in Workbench
> - Ausführen eines Skripts in diesem Projekt
> - Zugriff auf die Befehlszeilenschnittstelle (CLI)

Als Bestandteil des Microsoft Azure-Portfolios erfordern die Azure Machine Learning-Dienste ein Azure-Abonnement. Erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), um ein Azure-Abonnement zu erhalten.

Es sind geeignete Berechtigungen erforderlich, um Ressourcen erstellen zu können, z.B. Ressourcengruppen, virtuelle Computer usw.

Die Azure Machine Learning Workbench-Anwendung kann unter den folgenden Betriebssystemen installiert werden:

 -  Windows 10 oder Windows Server 2016
 -  macOS Sierra oder High Sierra

## <a name="step-4-create-azure-machine-learning-services"></a>Schritt 4: Erstellen von Azure Machine Learning-Diensten

Erstellen von Azure Machine Learning Services-Konten

Verwenden Sie das Azure-Portal für die Bereitstellung der Azure Machine Learning-Konten:

1.  Melden Sie sich mit den Anmeldeinformationen, die für das Azure-Abonnement verwendet werden sollen, am [Azure-Portal](https://portal.azure.com/) an. Erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), um ein Azure-Abonnement zu erhalten.

    ![Alt text](media\azure-stack-solution-machine-learning\image7.png)

1.  Klicken Sie in der linken oberen Ecke des Portals auf die Schaltfläche **Ressource erstellen** (+).

    ![Erstellen einer Ressource im Azure-Portal](media\azure-stack-solution-machine-learning\image8.png)

1.  Geben Sie in der Suchleiste **Machine Learning** ein. Wählen Sie das Suchergebnis mit dem Namen **Machine Learning-Experimentieren (Vorschauversion)** aus.

    ![Suche in Azure Machine Learning](media\azure-stack-solution-machine-learning\image9.png)

1.  Scrollen Sie im Bereich **Machine Learning-Experimentieren** nach unten, und wählen Sie **Erstellen**, um mit der Definition des Experimentieren-Kontos zu beginnen.

    ![Azure Machine Learning – Erstellen eines Experimentieren-Kontos](media\azure-stack-solution-machine-learning\image10.png)

1.  Konfigurieren Sie im Bereich **ML-Experimentieren** das Machine Learning-Experimentieren-Konto.

    | Einstellung | Vorgeschlagener Wert für Tutorial | BESCHREIBUNG |
    |---------------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Experimentation account name (Name des Experimentieren-Kontos) | Eindeutiger Name | Geben Sie einen eindeutigen Namen ein, mit dem das Konto identifiziert wird. Verwenden Sie einen Abteilungs- oder Projektnamen, mit dem das Experiment am besten beschrieben wird. Der Name sollte 2 bis 32 Zeichen lang sein. Es sind nur alphanumerische Zeichen und der Bindestrich (-) zulässig. |
    | Abonnement | Das Abonnement | Wählen Sie das Azure-Abonnement aus, das für das Experiment verwendet werden soll. Wählen Sie bei mehreren vorhandenen Abonnements das Abonnement aus, über das die Ressource abgerechnet wird. |
    | Ressourcengruppe | Die Ressourcengruppe | Verwenden Sie eine vorhandene Ressourcengruppe im Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe für dieses Experimentieren-Konto zu erstellen. |
    | Standort | Die Region, die den Benutzern am nächsten liegt | Wählen Sie den Standort aus, der den Benutzern und den Datenressourcen am nächsten ist. |
    | Number of seats (Anzahl von Arbeitsplätzen) | 2 | Geben Sie die Anzahl von Arbeitsplätzen ein. Erfahren Sie, wie sich [Arbeitsplätze auf die Preise auswirken](https://azure.microsoft.com/pricing/details/machine-learning/).<br><br>Für diese Schnellstartanleitung sind nur zwei Arbeitsplätze erforderlich. Arbeitsplätze können je nach Bedarf im Azure-Portal hinzugefügt oder entfernt werden. |
    | Speicherkonto | Eindeutiger Name | Wählen Sie **Neu erstellen**, und geben Sie einen Namen an, um ein [Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal) zu erstellen. Der Name muss 3 bis 24 Zeichen lang sein und darf nur alphanumerische Zeichen enthalten. Wählen Sie alternativ hierzu die Option **Vorhandene verwenden** und dann in der Liste das vorhandene Speicherkonto. Das Speicherkonto ist erforderlich. Es wird zum Speichern von Projektartefakten und Ausführungsverlaufsdaten verwendet. |
    | Workspace for Experimentation account (Arbeitsbereich für Experimentieren-Konto) | IrisGarden<br>(in Tutorials verwendeter Name) | Geben Sie einen Namen für einen Arbeitsbereich für dieses Konto an. Der Name sollte 2 bis 32 Zeichen lang sein. Es sind nur alphanumerische Zeichen und der Bindestrich (-) zulässig. Dieser Arbeitsbereich enthält die Tools, die zum Erstellen, Verwalten und Veröffentlichen von Experimenten erforderlich sind. |
    | Assign owner for the workspace (Besitzer für den Arbeitsbereich zuweisen) | Das Konto | Wählen Sie das eigene Konto als Besitzer des Arbeitsbereichs aus. |
    | Erstellen eines Kontos für die Modellverwaltung | **check** | Erstellen Sie ein Modellverwaltungskonto. Es wird verwendet, um die Modelle als Echtzeit-Webdienste bereitzustellen und zu verwalten. <br><br>Dieser Schritt ist zwar optional, aber es wird empfohlen, das Modellverwaltungskonto zur gleichen Zeit wie das Experimentieren-Konto zu erstellen. |
    | Kontoname | Eindeutiger Name | Wählen Sie einen eindeutigen Namen, mit dem das Modellverwaltungskonto identifiziert wird. Verwenden Sie den Abteilungs- oder Projektnamen, mit dem das Experiment am besten identifiziert wird. Der Name sollte 2 bis 32 Zeichen lang sein. Es sind nur alphanumerische Zeichen und der Bindestrich (-) zulässig. |
    | Model Management pricing tier (Modellverwaltungstarif) | **DEVTEST** | Wählen Sie **Es wurde kein Tarif ausgewählt**, um den Tarif für das neue Konto für die Modellverwaltung anzugeben. Um Kosten zu sparen, können Sie den Tarif DEVTEST wählen, falls er für das Abonnement verfügbar ist (eingeschränkte Verfügbarkeit). Wählen Sie andernfalls den Tarif S1. Wählen Sie „Auswählen“, um die Tarifauswahl zu speichern. |
    | An Dashboard anheften | Häkchen | Wählen Sie die Option **An Dashboard anheften**, um das Machine Learning-Experimentieren-Konto komfortabel auf der Titelseite des Dashboards im Azure-Portal nachverfolgen zu können. |

    ![Konfiguration des Machine Learning-Experimentieren-Kontos](media\azure-stack-solution-machine-learning\image11.png)

1.  Wählen Sie **Erstellen**, um mit dem Erstellungsprozess des Experimentieren-Kontos zusammen mit dem Modellverwaltungskonto zu beginnen.

    ![Konfiguration des Machine Learning-Experimentieren-Kontos](media\azure-stack-solution-machine-learning\image12.png)

    Die Erstellung eines Kontos kann einige Momente dauern. Überprüfen Sie den Status des Bereitstellungsprozesses, indem Sie in der Symbolleiste des Azure-Portals das Benachrichtigungssymbol (Glocke) wählen.

    ![Benachrichtigungen im Azure-Portal](media\azure-stack-solution-machine-learning\image13.png)

### <a name="install-and-log-in-to-workbench"></a>Installieren von und Anmelden bei Workbench 

Azure Machine Learning Workbench ist für Windows oder macOS verfügbar. Zeigen Sie die Liste der [unterstützten Plattformen](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation) an.

**Warnung:** Es kann ca. eine Stunde dauern, bis die Installation abgeschlossen ist.

1.  Laden Sie das aktuelle Workbench-Installationsprogramm herunter, und führen Sie es aus.

    > [!Important]  
    > Laden Sie den Installer vollständig auf den Datenträger herunter, und führen Sie ihn dann von dort aus. Führen Sie ihn nicht direkt über das Download-Widget des Browsers aus.<br>**Unter Windows:<br>** a. Laden Sie [AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi) herunter.<br>b. Doppelklicken Sie im Datei-Explorer auf das heruntergeladene Installationsprogramm.

1.  Befolgen Sie im Installationsprogramm die Anweisungen auf dem Bildschirm bis zum Abschluss.

    **Die Installation kann bis zu 30 Minuten dauern. **
    
    `Windows: C:\\Users\\<user>\\AppData\\Local\\AmlWorkbench`
    
    Das Installationsprogramm lädt alle erforderlichen Abhängigkeiten herunter, z.B. Python, Miniconda und andere zugehörige Bibliotheken, und richtet sie ein. Diese Installation enthält auch das plattformübergreifende Azure-Befehlszeilentool (Azure CLI).

1.  Starten Sie Workbench durch Auswählen der Schaltfläche **Workbench starten** auf der letzten Anzeige des Installationsprogramms.

    Wenn das Installationsprogramm geschlossen ist, können Sie es über die Desktopverknüpfung **Machine Learning Workbench** starten.

1.  Wählen Sie die Option **Bei Microsoft anmelden**, um die Authentifizierung für Azure Machine Learning Workbench durchzuführen. Verwenden Sie die gleichen Anmeldeinformationen, die im Azure-Portal zum Erstellen der Experimentieren- und Modellverwaltungskonten verwendet wurden.

    Nach dem Anmelden verwendet Workbench das erste Experimentieren-Konto, das in den Azure-Abonnements gefunden wird, und zeigt alle Arbeitsbereiche und Projekte an, die diesem Konto zugeordnet sind.

    > [!Tip]  
    > Wechseln Sie über das Symbol unten links im Workbench-Anwendungsfenster zu einem anderen Experimentieren-Konto.

### <a name="create-a-new-project-in-workbench"></a>Erstellen eines neuen Projekts in Workbench

1.  Öffnen Sie die Azure Machine Learning Workbench-App, und melden Sie sich an, falls dies erforderlich ist.

    - Starten Sie den Vorgang unter Windows mit der Desktopverknüpfung **Machine Learning Workbench**.

    - Unter macOS wählen Sie im Launchpad **Azure ML Workbench**.

1.  Wählen Sie im Bereich **PROJEKTE** das Pluszeichen (+) und dann **Neues Projekt**.

    ![Neuer Arbeitsbereich](media\azure-stack-solution-machine-learning\image14.png)

1.  Füllen Sie die Formularfelder aus, und wählen Sie die Schaltfläche **Erstellen**, um in der Workbench ein neues Projekt zu erstellen.

    | Feld | Vorgeschlagener Wert für Tutorial | BESCHREIBUNG |
    |-------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Projektname | myIris | Geben Sie einen eindeutigen Namen ein, mit dem das Konto identifiziert wird. Verwenden Sie einen Abteilungs- oder Projektnamen, mit dem das Experiment am besten beschrieben wird. Der Name sollte 2 bis 32 Zeichen lang sein. Es sind nur alphanumerische Zeichen und der Bindestrich (-) zulässig. |
    | Projektverzeichnis | c:\Temp\ | Geben Sie das Verzeichnis an, in dem das Projekt erstellt wird. |
    | Projektbeschreibung | Lassen Sie dieses Feld leer. | Optionales Feld für eine Beschreibung der Projekte. |
    | Visualstudio.com GIT Repository URL (Git-Repository-URL für Visualstudio.com) | Lassen Sie dieses Feld leer. | Optionales Feld. Ordnen Sie ein Projekt für die Quellcodeverwaltung und Zusammenarbeit einem Git-Repository in Visual Studio Team Services zu. [Einrichten eines Repositorys](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project). |
    | Ausgewählter Arbeitsbereich | IrisGarden (falls vorhanden) | Wählen Sie einen Arbeitsbereich aus, der im Azure-Portal für das Experimentieren-Konto erstellt wurde. <br>Bei Verwendung der Schnellstartanleitung ist der Arbeitsbereich mit dem Namen IrisGarden aufgeführt. Verwenden Sie andernfalls den Arbeitsbereich mit dem Namen des Experimentieren-Kontos oder einen bevorzugten Kontonamen. |
    | Projektvorlage | Klassifizieren von Schwertlilien | Vorlagen enthalten Skripts und Daten, die zum Erkunden des Produkts verwendet werden können. Diese Vorlage enthält die benötigten Skripts und Daten für diesen Schnellstart und andere Tutorials auf dieser Dokumentationswebsite. |

    ![Neues Projekt](media\azure-stack-solution-machine-learning\image15.png)

1.  Ein neues Projekt wird erstellt, und das Projektdashboard wird mit diesem Projekt geöffnet. Untersuchen Sie die Projektstartseite, Datenquellen, Notebooks und Quellcodedateien.

    ![Öffnen des Projekts](media\azure-stack-solution-machine-learning\image16.png)

### <a name="attach-a-dsvm-compute-target"></a>Anfügen eines DSVM-Computeziels

Nachdem die DSVM erstellt wurde, können Sie sie an das Azure ML-Projekt anfügen.

1.  Starten Sie über die Azure ML Workbench-App die Azure ML Workbench-CLI, indem Sie **Datei**->**PowerShell öffnen** wählen.

    ![Alt text](media\azure-stack-solution-machine-learning\image17.png)

1.  Verwenden Sie den folgenden Befehl, nachdem die PowerShell-Eingabeaufforderung geöffnet wurde:

    ```PowerShell  
        az login
    ```

1.  Sie erhalten folgende Aufforderung:

     ![Alt text](media\azure-stack-solution-machine-learning\image18.png)

1.  Navigieren Sie zur Website (wie in der Aufforderung angegeben), und geben Sie den bereitgestellten Code ein.

    ![Alt text](media\azure-stack-solution-machine-learning\image19.png)

1.  Wählen Sie „Weiter“, wenn Sie dazu aufgefordert werden, und wählen Sie anschließend das Azure-Konto aus, dem das Azure ML-Experimentieren-Konto zugeordnet ist.

    ![Alt text](media\azure-stack-solution-machine-learning\image20.png)

1.  Die Azure ML Workbench-CLI sendet dann die folgende Aufforderung:

    ![Alt text](media\azure-stack-solution-machine-learning\image21.png)

1.  Fügen Sie die DSVM an, wenn angezeigt wird, dass die Anmeldung für das ML-Konto und den Arbeitsbereich erfolgreich war.

    ```PowerShell  
        az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password>
    ```

    Die folgende Benachrichtigung wird angezeigt:

    ![Alt text](media\azure-stack-solution-machine-learning\image22.png)

    ```PowerShell  
        # prepare the Docker image on the DSVM 
        az ml experiment prepare -c <compute target name>
    ```

Dieser Prozess dauert eine Weile. Es wird eine erhebliche Menge an Text generiert, während verschiedene Docker-Images per Pullvorgang beschafft und registriert werden und dann der erforderliche Code und die Anwendungen darauf angewendet werden.

Experimente können jetzt auf dieser DSVM ausgeführt werden.

### <a name="create-a-data-preparation-package"></a>Erstellen eines Datenaufbereitungspakets

Als Nächstes können Sie mit der Datenaufbereitung in Azure Machine Learning Workbench beginnen. Jede Transformation, die Sie in Workbench vornehmen, wird im JSON-Format in einem lokalen Datenaufbereitungspaket (\*.DPREP-Datei) gespeichert. Dieses Datenaufbereitungspaket ist der primäre Container für die Datenaufbereitungsmaßnahmen in Workbench.

Dieses Datenaufbereitungspaket kann später an eine Laufzeit (z.B. „local-C\#/CoreCLR“, „Scala/Spark“ oder „Scala/HDI“) übergeben werden.

1.  Wählen Sie das Ordnersymbol aus, um die Dateiansicht zu öffnen, und wählen Sie anschließend **iris.csv**, um diese Datei zu öffnen.

    Die Datei enthält eine Tabelle mit fünf Spalten und 50 Zeilen. Bei vier Spalten handelt es sich um numerische Featurespalten. Die fünfte Spalte ist eine Zeichenfolgenzielspalte. Keine der Spalten besitzt einen Headernamen.

    ![Iris.csv](media\azure-stack-solution-machine-learning\image23.png)

1.  Wählen Sie unter **Datenansicht** das Pluszeichen (**+**), um eine neue Datenquelle hinzuzufügen. Die Seite **Datenquelle hinzufügen** wird geöffnet.

    ![Datenansicht in Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image24.png)

1.  Wählen Sie **Textdateien (\*.csv, \*.json, \*.txt., ...)**.

    ![Datenquelle in Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image25.png)

1.  Klicken Sie auf **Weiter**.

2.  Navigieren Sie zur Datei **iris.csv**, und wählen Sie **Fertig stellen**. Hierbei werden Standardwerte für Parameter verwendet, z.B. in Bezug auf das Trennzeichen und die Datentypen.

    > [!Important]  
    > Wählen Sie für diese Übung im aktuellen Projektverzeichnis die Datei **iris.csv** aus. Sonst kann es sein, dass die nachfolgenden Schritte fehlschlagen.

    ![Auswählen von „iris“](media\azure-stack-solution-machine-learning\image26.png)

1.  Eine neue Datei mit dem Namen `*iris-1.dsource` wird erstellt. Die Datei wird mit dem Zusatz `-1` eindeutig benannt, da das Beispielprojekt bereits über eine nicht nummerierte Datei **iris.dsource** verfügt.

    Die Datei wird geöffnet, und die Daten werden angezeigt. Diesem Dataset werden automatisch mehrere Spaltenüberschriften von **Column1** bis **Column5** hinzugefügt. Scrollen Sie nach unten, und beachten Sie, dass die letzte Zeile des Datasets leer ist. Die Zeile ist leer, weil die CSV-Datei einen zusätzlichen Zeilenumbruch enthält.

    ![Iris-Datenansicht](media\azure-stack-solution-machine-learning\image27.png)

1.  Wählen Sie die Schaltfläche **Metriken**. Histogramme werden generiert und angezeigt.

    Wechseln Sie zurück zur Datenansicht, indem Sie die Schaltfläche **Daten** wählen.

    ![Iris-Datenansicht](media\azure-stack-solution-machine-learning\image28.png)

1.  Sehen Sie sich die Histogramme an. Für jede Spalte wurde ein vollständiger Satz mit Statistiken berechnet.

    ![Iris-Datenansicht](media\azure-stack-solution-machine-learning\image29.png)

1.  Klicken Sie auf die Schaltfläche **Vorbereiten**, um mit der Erstellung eines Datenaufbereitungspakets zu beginnen. Das Dialogfeld **Vorbereiten** wird geöffnet.

    Das Beispielprojekt enthält eine Datenaufbereitungsdatei mit dem Namen **iris.dprep**, die standardmäßig ausgewählt ist.

    ![Iris-Datenansicht](media\azure-stack-solution-machine-learning\image30.png)

1.  Wählen Sie im Menü die Option **+ New Data Preparation Package** (+ Neues Datenaufbereitungspaket), um ein neues Datenaufbereitungspaket zu erstellen.

    ![Iris-Datenansicht](media\azure-stack-solution-machine-learning\image31.png)

1.  Geben Sie einen neuen Wert für den Paketnamen ein (**iris-1**), und klicken Sie anschließend auf **OK**.

    Ein neues Datenaufbereitungspaket mit dem Namen **iris-1.dprep** wird erstellt und im Editor für die Aufbereitung von Daten geöffnet.

    ![Iris-Datenansicht](media\azure-stack-solution-machine-learning\image32.png)

    Als Nächstes muss die Datenaufbereitung durchgeführt werden.

1.  Wählen Sie jeweils eine Spaltenüberschrift aus, damit der Text der Überschrift bearbeitet werden kann. Benennen Sie dann die einzelnen Spalten wie folgt um:

    Geben Sie für die fünf Spalten der Reihenfolge nach **Sepal Length** (Länge des Kelchblatts), **Sepal Width** (Breite des Kelchblatts), **Petal Length** (Länge des Kronblatts), **Petal Width** (Breite des Kronblatts) bzw. **Species** (Art) ein.

    ![Umbenennen der Spalten](media\azure-stack-solution-machine-learning\image33.png)

1.  Zählen eindeutiger Werte:

    1.  Klicken Sie auf die Spalte **Species** (Art).

    2.  Klicken Sie mit der rechten Maustaste darauf, um sie auszuwählen.

    3.  Wählen Sie im Menü die Option **Werteanzahl**.

        Daraufhin öffnet sich unterhalb der Daten der Bereich **Inspectors** (Inspektoren). Ein Histogramm mit vier Balken wird angezeigt. Die Zielspalte enthält die vier unterschiedlichen Werte **Iris-virginica**, **Iris-versicolor**, **Iris-setosa** und **(null)**.

    ![Auswählen der Wertanzahl](media\azure-stack-solution-machine-learning\image34.png)

    ![Histogramm mit Wertanzahl](media\azure-stack-solution-machine-learning\image35.png)

1.  Klicken Sie zum Herausfiltern der NULL-Werte auf den Balken „(null)“ und anschließend auf das Minuszeichen (**-**).

    Daraufhin wird die Zeile „(null)“ ausgegraut, um anzugeben, dass sie herausgefiltert wurde.

    ![Herausfiltern von NULL-Werten](media\azure-stack-solution-machine-learning\image36.png)

1.  Beachten Sie die einzelnen Datenaufbereitungsschritte im Bereich **SCHRITTE**. Während die Spalten umbenannt und die Zeilen mit NULL-Werten gefiltert werden, wird jede Aktion als Datenaufbereitungsschritt aufgezeichnet. Bearbeiten Sie die einzelnen Schritte, um die Einstellungen anzupassen und die Schritte neu anzuordnen und zu entfernen.

    ![Alt text](media\azure-stack-solution-machine-learning\image37.png)

1.  Schließen Sie den Editor für die Aufbereitung von Daten. Klicken Sie auf der Registerkarte **iris-1** mit dem Diagrammsymbol auf das Symbol **X**, um die Registerkarte zu schließen. Die Arbeit wird automatisch in der Datei **iris-1.dprep** unter der Überschrift **Data Preparations** (Datenaufbereitungen) gespeichert.

    ![Close (Schließen)](media\azure-stack-solution-machine-learning\image38.png)

### <a name="generate-python-code-to-invoke-a-data-preparation-package"></a>Generieren von Python-Code zum Aufrufen eines Datenaufbereitungspakets

Die Ausgabe eines Datenaufbereitungspakets kann direkt in Python oder in einem Jupyter Notebook untersucht werden. Die Pakete können übergreifend für mehrere Laufzeiten ausgeführt werden. Hierzu zählen etwa Python, Spark (einschließlich Docker) und HDInsight in der lokalen Umgebung.

1.  Suchen Sie auf der Registerkarte „Data Preparations“ (Datenaufbereitungen) nach der Datei **iris-1.dprep**.

2.  Klicken Sie mit der rechten Maustaste auf die Datei **Iris-1.dprep**, und wählen Sie im Kontextmenü dann die Option **Generate Data Access Code File** (Datenzugriffs-Codedatei generieren).

    ![Generieren von Code](media\azure-stack-solution-machine-learning\image39.png)

    Eine neue Datei mit dem Namen **iris-1.py** wird mit den folgenden Codezeilen geöffnet, um die Logik aufzurufen, die als Datenaufbereitungspaket erstellt wurde:

    ```Python
    # Use the Azure Machine Learning data preparation package
    from azureml.dataprep import package

    # Use the Azure Machine Learning data collector to log various metrics
    from azureml.logging import get_azureml_logger
    logger = get_azureml_logger()

    # This call will load the referenced package and return a DataFrame.
    # If run in a PySpark environment, this call returns a
    # Spark DataFrame. If not, it will return a Pandas DataFrame.
    df = package.run('iris-1.dprep', dataflow_idx=0)
    # Remove this line and add code that uses the DataFrame
    df.head(10)
    ```

    Je nach Kontext, in dem dieser Code ausgeführt wird, stellt drep eine andere Art von Datenrahmen dar:

    -  Bei der Ausführung für eine Python-Laufzeit wird ein [Pandas-Datenrahmen](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) verwendet.

    -  Bei der Ausführung in einem Spark-Kontext wird ein [Spark-Datenrahmen](https://spark.apache.org/docs/latest/sql-programming-guide.html) verwendet.

### <a name="review-irissklearnpy-and-the-configuration-files"></a>Prüfen der Datei „iris_sklearn.py“ und der Konfigurationsdateien

1.  Wählen Sie im geöffneten Projekt im Bereich ganz links die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste im Projektordner zu öffnen.

    ![Öffnen des Azure Machine Learning Workbench-Projekts](media\azure-stack-solution-machine-learning\image40.png)

1.  Wählen Sie die Python-Skriptdatei **iris_sklearn.py** aus.

    ![Auswählen eines Skripts](media\azure-stack-solution-machine-learning\image41.png)

    Der Code wird in Workbench in einer neuen Registerkarte des Text-Editors geöffnet.

    > [!Note]  
    > Der angezeigte Code entspricht unter Umständen nicht genau dem oben abgebildeten Codebeispiel, da dieses Beispielprojekt regelmäßig aktualisiert wird.

    ![Öffnen einer Datei](media\azure-stack-solution-machine-learning\image42.png)

1.  Sehen Sie sich den Code des Python-Skripts an, um sich mit dem Codierungsstil vertraut zu machen.

    Mit dem Skript **iris_sklearn.py** werden die folgenden Aufgaben durchgeführt:  

    -  Lädt das Standard-Datenaufbereitungspaket **iris.dprep** zum Erstellen eines [Pandas-Datenrahmens](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).

    -   Fügt zufällige Funktionen hinzu, um die Lösung des Problems zu erschweren. Zufälligkeit ist erforderlich, da Iris ein kleines Dataset ist, das mit nahezu 100-prozentiger Genauigkeit leicht klassifiziert werden kann.

    -  Verwendet die Machine Learning-Bibliothek „scikit-learn“ zum Erstellen eines logistischen Regressionsmodells. Diese Bibliothek ist standardmäßig in Azure Machine Learning Workbench enthalten.

    -  Serialisiert das Modell, indem die [pickle](https://docs.python.org/3/library/pickle.html)-Bibliothek in einer Datei im Ordner `outputs` verwendet wird.

    -  Lädt das serialisierte Modell und führt dann die Deserialisierung zurück in den Arbeitsspeicher durch.

    -  Verwendet das deserialisierte Modell zum Treffen einer Vorhersage für einen neuen Datensatz.

    -  Erstellt zwei Graphen, eine Konfusionsmatrix und eine Grenzwertoptimierungskurve (Receiver Operating Characteristic, ROC) für mehrere Klassen, indem die Bibliothek [matplotlib](https://matplotlib.org/) verwendet wird, und speichert sie anschließend im Ordner „outputs“. Installieren Sie diese Bibliothek in der Umgebung, falls sie nicht vorhanden ist.

    -  Nimmt die Regularisierungsrate und die Modellgenauigkeit automatisch in den Ausführungsverlauf auf. Hierbei wird immer das Objekt `run_logger` verwendet, um die Regularisierungsrate und die Modellgenauigkeit in den Protokollen aufzuzeichnen.

### <a name="run-irissklearnpy-in-the-local-environment"></a>Ausführen von „iris_sklearn.py“ in der lokalen Umgebung

1.  Starten Sie die Azure Machine Learning-Befehlszeilenschnittstelle (CLI):

    1.  Starten Sie Azure Machine Learning Workbench.

    2.  Wählen Sie im Workbench-Menü die Option **Datei**> **Eingabeaufforderung öffnen**.

    Das Fenster für die Azure Machine Learning-Befehlszeilenschnittstelle (CLI) wird in Windows im Projektordner `C:\Temp\\myIris\` gestartet. Dieses Projekt entspricht dem Projekt, das in Teil 1 des Tutorials erstellt wurde.

    > [!Important]  
    > Verwenden Sie diese CLI-Fenster, um die nächsten Schritte ausführen zu können.

1.  Installieren Sie im CLI-Fenster die Python-Bibliothek für das Zeichnen (**matplotlib**), falls sie noch nicht installiert wurde.

    Das Skript **iris_sklearn.py** verfügt über Abhängigkeiten von zwei Python-Paketen: **scikit-learn** und **matplotlib**. Das Paket **scikit-learn** wird von Azure Machine Learning Workbench als Hilfe für Benutzer installiert. Es kann aber sein, dass die Installation von **matplotlib** trotzdem erforderlich ist.

    Wenn Sie fortfahren, ohne **matplotlib** zu installieren, kann der Code in diesem Tutorial trotzdem erfolgreich ausgeführt werden. Mit dem Code können aber nicht die Konfusionsmatrixausgabe und Grenzwertoptimierungskurven (Receiver Operating Characteristic, ROC) für mehrere Klassen erstellt werden (siehe Verlaufsvisualisierungen).

    ```CLI
    pip install matplotlib
    python -m pip install --upgrade pip
    ```

    Diese Installation dauert ungefähr eine Minute.

1.  Wechseln Sie zurück zur Workbench-Anwendung.

2.  Suchen Sie nach der Registerkarte **iris_sklearn.py**.

    ![Suchen nach Registerkarte mit Skript](media\azure-stack-solution-machine-learning\image43.png)

1.  Wählen Sie in der Symbolleiste dieser Registerkarte die Option **local** als Ausführungsumgebung und „iris_sklearn.py“ als auszuführendes Skript aus. Unter Umständen sind die Optionen bereits ausgewählt.

    ![Alt text](media\azure-stack-solution-machine-learning\image44.png)

1.  Geben Sie rechts in der Symbolleiste im Feld **Argumente** den Wert „0,01“ ein.

    Dieser Wert entspricht der Regularisierungsrate des Modells für die logistische Regression.

    ![Auswählen von „local“ und des Skripts](media\azure-stack-solution-machine-learning\image45.png)

1.  Wählen Sie die Schaltfläche **Ausführen**. Ein Auftrag wird sofort geplant. Der Auftrag wird im Bereich **Aufträge** rechts im Workbench-Fenster aufgelistet.

    ![Auswählen von „local“ und des Skripts](media\azure-stack-solution-machine-learning\image46.png)

    Nach kurzer Zeit wechselt der Status des Auftrags von **Wird übermittelt** zu **Wird ausgeführt** und schließlich zu **Abgeschlossen**.

1.  Wählen Sie im Bereich **Aufträge** im Text zum Auftragsstatus die Option **Abgeschlossen**.

    ![Ausführen von „sklearn“](media\azure-stack-solution-machine-learning\image47.png)

    Im daraufhin geöffneten Popupfenster wird der Standardausgabetext (stdout) für die Ausführung angezeigt. Wählen Sie zum Schließen des stdout-Texts oben rechts im Popupfenster die Schaltfläche **Schließen** (**x**).

    ![Standardausgabe](media\azure-stack-solution-machine-learning\image48.png)

1.  Wählen Sie in demselben Auftragsstatus im Bereich **Aufträge** direkt über dem Status **Abgeschlossen** und der Startzeit den blauen Text **iris_sklearn.py \[n\] **(* n* ist die Ausführungsanzahl) aus. Das Fenster mit den **Ausführungseigenschaften** wird geöffnet und enthält für die jeweilige Ausführung die folgenden Informationen:

    -  Informationen zu den **Ausführungseigenschaften**

    -  **Ausgaben**

    -  **Metriken**

    -  **Visualisierungen**, falls vorhanden

    -  **Protokolle**

    Nach Abschluss der Ausführung werden im Popupfenster die folgenden Ergebnisse angezeigt:

    > [!Note]  
    > Da im Tutorial für den Trainingssatz zuvor eine zufällige Anordnung verwendet wurde, weichen die genauen Ergebnisse unter Umständen von den hier gezeigten Ergebnissen ab.

    ```Python  
        Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]

        Iris dataset shape: (150, 5)
        Regularization rate is 0.01
        LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
        Accuracy is 0.6792452830188679

        ==========================================
        Serialize and deserialize using the outputs folder.

        Export the model to model.pkl
        Import the model from model.pkl
        New sample: [[3.0, 3.6, 1.3, 0.25]]
        Predicted class is ['Iris-setosa']
        Plotting confusion matrix...
        Confusion matrix in text:
        [[50  0  0]
        [ 1 37 12]
        [ 0  4 46]]
        Confusion matrix plotted.
        Plotting ROC curve....

        ROC curve plotted.
        Confusion matrix and ROC curve plotted. See them in Run History details pane.

    ```

1.  Schließen Sie die Registerkarte **Run Properties** (Ausführungseigenschaften), und wechseln Sie anschließend zurück zur Registerkarte **iris_sklearn.py**.

1.  Wiederholen Sie diesen Vorgang für weitere Ausführungen.

Geben Sie im Feld **Argumente** mehrere Werte von `0.001` bis `10` ein. Wählen Sie **Ausführen**, um den Code einige weitere Male auszuführen. Der jeweils geänderte Argumentwert wird in das Modell für die logistische Regression im Code eingefügt. Dies führt jeweils zu unterschiedlichen Ergebnissen.

### <a name="review-the-run-history-in-detail"></a>Ausführliches Prüfen des Ausführungsverlaufs

In Azure Machine Learning Workbench wird jede Skriptausführung als Datensatz des Ausführungsverlaufs erfasst. Zeigen Sie den Ausführungsverlauf eines bestimmten Skripts an, indem Sie die Ansicht **Ausführungen** öffnen.

1.  Wählen Sie zum Öffnen der Liste mit den **Ausführungen** in der linken Symbolleiste die Schaltfläche **Ausführungen** (Uhrsymbol). Wählen Sie anschließend **iris_sklearn.py**, um das **Ausführungsdashboard** von „iris_sklearn.py“ anzuzeigen.

    ![Ausführen der Ansicht](media\azure-stack-solution-machine-learning\image49.png)

1.  Die Registerkarte **Run Dashboard** (Ausführungsdashboard) wird geöffnet.

    Überprüfen Sie die in mehreren Ausführungen erfassten Statistiken. Die Graphen werden oben in der Registerkarte gerendert. Jede Ausführung verfügt über eine fortlaufende Nummer, und die Ausführungsdetails sind in der Tabelle unten auf dem Bildschirm aufgeführt.

    ![Ausführungsdashboard](media\azure-stack-solution-machine-learning\image50.png)

1.  Filtern Sie die Tabelle, und wählen Sie dann einen beliebigen Graphen aus, um für jede Ausführung Status, Dauer, Genauigkeit und Regularisierungsrate anzuzeigen.

2.  Aktivieren Sie in der Tabelle **Ausführungen** die Kontrollkästchen neben mindestens zwei Ausführungen. Wählen Sie die Schaltfläche **Vergleichen**, um einen Bereich für einen ausführlichen Vergleich zu öffnen. Überprüfen Sie die Gegenüberstellung der Ausführungen.

3.  Wählen Sie oben links im Bereich **Vergleich** die Zurück-Schaltfläche **Ausführungsliste**, um wieder zurück zum **Ausführungsdashboard** zu wechseln.

    ![Wechseln zurück zur Liste „Ausführungen“](media\azure-stack-solution-machine-learning\image51.png)

1.  Wählen Sie eine einzelne Ausführung aus, um die Ausführungsdetailansicht anzuzeigen. Beachten Sie, dass die Statistiken für die ausgewählte Ausführung im Abschnitt **Run Properties** (Ausführungseigenschaften) aufgeführt sind. Die Dateien, die in den Ausgabeordner geschrieben werden, sind im Abschnitt **Ausgaben** aufgeführt. Laden Sie sie von dort herunter.

    ![Ausführungsdetails](media\azure-stack-solution-machine-learning\image52.png)

Die beiden Diagramme, d.h. die Konfusionsmatrix und die ROC-Kurve für mehrere Klassen, werden im Abschnitt **Visualisierungen** gerendert. Alle Protokolldateien werden zudem im Abschnitt **Protokolle** angezeigt.

### <a name="run-scripts-in-the-azure-machine-learning-ml-workbench-cli-window"></a>Ausführen von Skripts im Azure Machine Learning (ML) Workbench-CLI-Fenster

1.  Starten Sie die Azure Machine Learning-Befehlszeilenschnittstelle (CLI):

    1.  Starten Sie Azure Machine Learning Workbench.

    2.  Wählen Sie im Workbench-Menü die Option **Datei** > **Eingabeaufforderung öffnen**.

    Die CLI-Eingabeaufforderung beginnt im Projektordner `C:\\Temp\\myIris` unter Windows. Dies ist das Projekt, das in Teil 1 des Tutorials erstellt wurde.

    > [!Important]  
    > Verwenden Sie diese CLI-Fenster, um die nächsten Schritte ausführen zu können.

1.  Melden Sie sich im CLI-Fenster an Azure an. [Hier finden Sie weitere Informationen zu „az login“](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    Überspringen Sie diesen Schritt, wenn Sie bereits angemeldet sind.

1.  Geben Sie an der Eingabeaufforderung Folgendes ein:

    ```CLI
        az login
    ```

    Mit diesem Befehl wird ein Code zurückgegeben, den Sie im Browser unter [https://aka.ms/devicelogin](https://aka.ms/devicelogin) verwenden können.

1.  Navigieren Sie im Browser zu [https://aka.ms/devicelogin](https://aka.ms/devicelogin). Geben Sie den Code ein, den Sie in der CLI erhalten haben.

    Die Workbench-App und die CLI verwenden bei der Authentifizierung von Azure-Ressourcen unabhängige Anmeldeinformationscaches. Die Authentifizierung ist erst wieder erforderlich, wenn das zwischengespeicherte Token abgelaufen ist.

1.  Legen Sie das zu verwendende Abonnement fest, falls die Organisation über mehrere Azure-Abonnements (Unternehmensumgebung) verfügt. Suchen Sie nach dem Abonnement, legen Sie es mit der Abonnement-ID fest, und testen Sie es.

1.  Listen Sie alle zugänglichen Azure-Abonnements auf, indem Sie diesen Befehl verwenden:

    ```CLI
        az account list -o table 
    ```

    Mit dem Befehl **az account list** wird die Liste mit den für die Anmeldung verfügbaren Abonnements zurückgegeben. Identifizieren Sie bei mehr als einem Abonnement den ID-Wert für das gewünschte Abonnement.

1.  Legen Sie das Azure-Abonnement fest, das als Standardkonto verwendet werden soll:

    ```CLI
        az account set -s <the-subscription-id
    ```

    Hierbei steht <the-subscription-id> für den ID-Wert des verwendeten Abonnements. Geben Sie die Klammern nicht mit an.

1.  Überprüfen Sie die neue Abonnementeinstellung, indem Sie die Details für das aktuelle Abonnement anfordern.

    ```CLI
        az account show
    ```

1.  Übermitteln Sie das Skript **iris_sklearn.py** im CLI-Fenster als Experiment.

    Die Ausführung von „iris_sklearn.py“ erfolgt basierend auf dem lokalen Computekontext.

1.  Unter Windows:

    ```CLI
        az ml experiment submit -c local .\\iris_sklearn.py
    ```

1.  Unter MacOS:

    ```CLI
        az ml experiment submit -c local iris_sklearn.py
    ```

    Die Ausgabe sollte in etwa wie folgt aussehen: 

    ```
        RunId: myIris_1521077190506

    Executing user inputs .....
    ===========================

    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]

    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679

    ==========================================
    Serialize and deserialize using the outputs folder.

    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.

    Execution Details
    =================
    RunId: myIris_1521077190506

    ```

6.  Überprüfen Sie die Ausgabe. Die Ausgabe und die Ergebnisse sollten der Ausführung des Skripts mit Workbench entsprechen.

7.  Wechseln Sie zurück zu Workbench, und gehen Sie wie folgt vor:

    Wählen Sie im linken Bereich das Ordnersymbol aus, um die Projektdateien aufzulisten.  Öffnen Sie das Python-Skript mit dem Namen **run.py**. Dieses Skript ist nützlich, um eine Schleife durch verschiedene Regularisierungsraten durchzuführen. 

    ![Wechseln zurück zur Liste „Ausführungen“](media\azure-stack-solution-machine-learning\image53.png)

1.  Führen Sie das Experiment mehrfach mit diesen Raten aus.

    Mit diesem Skript wird ` aniris_sklearn.pyjob` mit der Regularisierungsrate `10.0` (extrem hoher Wert) gestartet. Über das Skript wird die Rate während der folgenden Ausführung halbiert (usw.), bis der Mindestwert `0.005` erreicht ist. Das Skript enthält den folgenden Code:

    ![Wechseln zurück zur Liste „Ausführungen“](media\azure-stack-solution-machine-learning\image54.png)

1.  Führen Sie das Skript **run.py** wie folgt über die Befehlszeile aus:

    ```CLI
        python run.py
    ```
Mit diesem Befehl wird `iris_sklearn.py` mehrere Male mit unterschiedlichen Regularisierungsraten gesendet.

Wenn `run.py` abgeschlossen ist, werden Graphen mit unterschiedlichen Metriken in der Workbench in der Liste mit den Ausführungsverläufen angezeigt.

### <a name="run-scripts-in-an-ubuntu-based-data-science-virtual-machine-dsvm-on-azure"></a>Ausführen von Skripts in einer Ubuntu-basierten Data Science Virtual Machine (DSVM) in Azure

Für die Ausführung des Skripts in einem Docker-Container auf einem Linux-Remotecomputer ist SSH-Zugriff (Benutzername und Kennwort) erforderlich, um den Remotecomputer auszuführen. Zudem muss auf dem Computer eine Docker-Engine installiert sein und ausgeführt werden.

1.  Bearbeiten Sie die generierte Datei „<your DSVM Name>.runconfig“ unter „aml_config“, und ändern Sie das Framework über den Standardwert „PySpark“ in „Python“:

    ```yaml  
    Framework: Python
    ```
1.  Führen Sie den gleichen Befehl wie zuvor im CLI-Fenster aus, indem Sie die Ziel-*<DSVM>* verwenden. Dieses Mal geht es um die Ausführung von „iris_sklearn.py“ in einem Docker-Remotecontainer (Ersetzen Sie <DSVM> durch den Namen der Data Science VM, und lassen Sie die Klammern weg):

    ```CLI
        az ml experiment submit -c <DSVM> iris_sklearn.py
    ```

Der Befehl wird wie in einer docker-python-Umgebung ausgeführt, aber die Ausführung erfolgt auf dem virtuellen Linux-Remotecomputer. Im CLI-Fenster werden die gleichen Ausgabeinformationen angezeigt.

### <a name="download-the-model-pickle-file"></a>Herunterladen der pickle-Modelldatei

Im vorherigen Teil des Tutorials wurde das Skript **iris_sklearn.py** lokal in Machine Learning Workbench ausgeführt. Mit dieser Aktion wurde das Modell für die logistische Regression über das beliebte Python-Paket [pickle](https://docs.python.org/3/library/pickle.html) für die Objektserialisierung serialisiert.

1.  Öffnen Sie die Machine Learning Workbench-Anwendung. Öffnen Sie anschließend das Projekt **myIris**, das in den vorherigen Teilen der Tutorialreihe erstellt wurde.

2.  Wählen Sie nach dem Öffnen des Projekts im linken Bereich die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste im Projektordner zu öffnen.

3.  Wählen Sie die Datei **iris_sklearn.py** aus. Der Python-Code wird in Workbench in einer neuen Registerkarte des Text-Editors geöffnet.

4.  Sehen Sie in der Datei **iris_sklearn.py** nach, um zu ermitteln, wo die pickle-Datei generiert wurde. Drücken Sie STRG+F, um das Dialogfeld **Suchen** zu öffnen, und suchen Sie im Python-Code nach dem Wort **pickle**.

In diesem Codeausschnitt ist dargestellt, wie die pickle-Ausgabedatei generiert wurde. Die pickle-Ausgabedatei hat auf dem Datenträger den Namen **model.pkl**.

    ```Python
        print("Export the model to model.pkl")
        f = open('./outputs/model.pkl', 'wb')
        pickle.dump(clf1, f)
        f.close()

    ```

1.  Suchen Sie in den Ausgabedateien eines vorherigen Ausführungslaufs nach der pickle-Modelldatei.

    Beim Ausführen des Skripts **iris_sklearn.py** wird die Modelldatei unter dem Namen **model.pkl** in den Ordner **outputs** geschrieben. Dieser Ordner befindet sich in der Ausführungsumgebung, die für die Ausführung des Skripts ausgewählt wurde, und nicht im lokalen Projektordner. 

    1. Wählen Sie zum Suchen nach der Datei im linken Bereich die Schaltfläche **Ausführungen** (Uhrsymbol), um die Liste **All Runs** (Alle Ausführungen) zu öffnen.  

    2. Die Registerkarte **All Runs** (Alle Ausführungen) wird geöffnet. Wählen Sie in der Tabelle mit den Ausführungen eine der letzten Ausführungen aus, für die das Ziel **local** und der Skriptname **iris_sklearn.py** lauten.  

    3. Der Bereich **Run Properties** (Ausführungseigenschaften) wird geöffnet. Oben rechts im Bereich befindet sich der Abschnitt **Ausgaben**. d\. Aktivieren Sie zum Herunterladen der pickle-Datei das Kontrollkästchen neben der Datei **model.pkl**, und wählen Sie anschließend **Herunterladen**. Speichern Sie die Datei im Stammverzeichnis des Projektordners. Die Datei wird in den nächsten Schritten benötigt.  

    ![Herunterladen der pickle-Datei](media\azure-stack-solution-machine-learning\image55.png)

### <a name="get-scoring-script-and-schema-files"></a>Abrufen des Bewertungsskripts und von Schemadateien

Zum Bereitstellen des Webdiensts zusammen mit der Modelldatei wird auch ein Bewertungsskript benötigt. Optional ist ein Schema für die Webdienst-Eingabedaten erforderlich. Mit dem Bewertungsskript wird die Datei **model.pkl** aus dem aktuellen Ordner geladen und zum Erzeugen neuer Vorhersagen verwendet.

1.  Öffnen Sie die Machine Learning Workbench-Anwendung. Öffnen Sie anschließend das Projekt **myIris**, das im vorherigen Teil der Tutorialreihe erstellt wurde.

2.  Wählen Sie nach dem Öffnen des Projekts im linken Bereich die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste im Projektordner zu öffnen.

3.  Wählen Sie die Datei **score_iris.py** aus. Das Python-Skript wird geöffnet. Diese Datei wird als Bewertungsdatei verwendet.

    ![Bewertungsdatei](media\azure-stack-solution-machine-learning\image56.png)

1.  Führen Sie das Skript aus, um die Schemadatei abzurufen. Wählen Sie in der Befehlsleiste die Umgebung **local** und das Skript **score_iris.py** und anschließend **Ausführen**.

    Mit diesem Skript wird im Abschnitt **outputs** eine JSON-Datei erstellt, mit der das für das Modell benötigte Eingabedatenschema erfasst wird.

1.  Beachten Sie den Bereich **Aufträge** rechts im Bereich **Projektdashboard**. Warten Sie, bis für den aktuellen Auftrag **score_iris.py** in Grün der Status **Abgeschlossen** angezeigt wird. Wählen Sie anschließend den Hyperlink **score_iris.py** für die letzte Auftragsausführung aus, um die Ausführungsdetails anzuzeigen.

2.  Wählen Sie im Bereich **Run Properties** (Ausführungseigenschaften) im Abschnitt **Ausgaben** die neu erstellte Datei **service_schema.json** aus. Aktivieren Sie das Kontrollkästchen neben dem Dateinamen, und wählen Sie anschließend **Herunterladen**. Speichern Sie die Datei im Stammverzeichnis des Projekts.

3.  Wechseln Sie zurück zur vorherigen Registerkarte und zum Skript **score_iris.py**. Mit der Datensammlung können Modelleingaben und Vorhersagen für den Webdienst erfasst werden. Die folgenden Schritte sind für die Datensammlung von Interesse.

4.  Sehen Sie sich den Code oben in der Datei an, mit dem die **ModelDataCollector**-Klasse importiert wird. Er enthält die Funktionalität für die Modelldatensammlung:

    ```Python  
        from azureml.datacollector import ModelDataCollector
    ```

1.  Die folgenden Codezeilen in der Funktion **init()** dienen zum Instanziieren von **ModelDataCollector**:

    ```Python  
        global inputs_dc, prediction_dc
        inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
        prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1.  Die folgenden Codezeilen in der Funktion **run(input_df)** dienen zum Sammeln der Eingabe- und Vorhersagedaten:

    ```Python  
        inputs_dc.collect(input_df)
        prediction_dc.collect(pred)
    ```

Bereiten Sie nun die Umgebung für die Operationalisierung des Modells vor.

## <a name="step-5-deploy-and-use-azure-container-registry"></a>Schritt 5: Bereitstellen und Verwenden von Azure Container Registry

Bereitstellen und Verwenden von Azure Container Registry

Erstellen Sie mit dem Befehl **az acr create** eine Azure Container Registry-Instanz. Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Die Ressourcengruppe ist identisch.

    ```CLI
        az acr create --resource-group <ResourceGroup> --name  <acrName> --sku Basic
    ```

### <a name="container-registry-login"></a>Anmeldung bei der Containerregistrierung

Verwenden Sie den Befehl **az acr login**, um sich bei der ACR-Instanz anzumelden. Geben Sie den eindeutigen Namen an, den die Containerregistrierung bei ihrer Erstellung erhalten hat.

    ```CLI
        az acr login --name <acrName>
    ```

Nach Abschluss des Vorgangs wird eine Meldung angezeigt, dass die Anmeldung erfolgreich war.

### <a name="prepare-to-operationalize-locally-for-development-and-testing-the-service"></a>Vorbereiten der lokalen Operationalisierung für die Entwicklung und das Testen des Diensts

Verwenden Sie den *lokalen Bereitstellungsmodus* für die Ausführung in Docker-Containern auf dem lokalen Computer und für die Entwicklung und das Testen.

Die Docker-Engine muss lokal ausgeführt werden, um die folgenden Schritte zum Operationalisieren des Modells auszuführen. Verwenden Sie das Flag `-h` am Ende jedes Befehls, um die entsprechende Hilfemeldung anzuzeigen.

    > [!Note]  
    > If Docker engine is not locally available, proceed by creating a cluster in Azure for deployment and keep the cluster for re-use, or delete it after the tutorial to avoid ongoing charges.

    > [!Note]  
    > Web services deployed locally do not appear in Azure Portal's list of services. They will be running in Docker on the local machine.

1.  Öffnen Sie die Befehlszeilenschnittstelle (CLI). Wählen Sie in der Anwendung Machine Learning Workbench im Menü **Datei** die Option **Eingabeaufforderung öffnen**.

    Die Eingabeaufforderung wird geöffnet und zeigt den aktuellen Speicherort des Projektordners an: **c:\\temp\\myIris**.

1.  Stellen Sie sicher, dass der Azure-Ressourcenanbieter **Microsoft.ContainerRegistry** im Abonnement registriert ist. Registrieren Sie diesen Ressourcenanbieter, bevor Sie in Schritt 3 eine Umgebung erstellen. Überprüfen Sie, ob die Registrierung bereits durchgeführt wurde, indem Sie den folgenden Befehl verwenden:

    ```CLI
        az provider list --query "\[\].{Provider:namespace, Status:registrationState}" --out table
    ```

    Zeigen Sie die folgende Ausgabe an:

    ```CLI
        Provider                                    Status 
        --------                                    --------
        Microsoft.Authorization                     Registered 
        Microsoft.ContainerRegistry                 Registered 
        microsoft.insights                          Registered 
        Microsoft.MachineLearningExperimentation    Registered 
    ```

    Verwenden Sie den folgenden Befehl, wenn **Microsoft.ContainerRegistry** nicht registriert ist:

    ```CLI
    az provider register --namespace Microsoft.ContainerRegistry
    ```

    Die Registrierung kann einige Minuten dauern. Überprüfen Sie den Registrierungsstatus, indem Sie den obigen Befehl **az provider list** oder den folgenden Befehl verwenden:

    ```CLI
    az provider show -n Microsoft.ContainerRegistry
    ```

    In der dritten Zeile der Ausgabe wird **"registrationState": "Registering"** angezeigt. Warten Sie kurz ab, und wiederholen Sie dann den Befehl **show**, bis in der Ausgabe **"registrationState": "Registered"** angezeigt wird.

1.  Erstellen Sie die Umgebung. Führen Sie diesen Schritt einmal pro Umgebung aus.

    Für den folgenden Setupbefehl ist der Zugriffstyp „Mitwirkender“ für das Abonnement erforderlich. Der Zugriff vom Typ „Mitwirkender“ auf die Ressourcengruppe für die Bereitstellung ist erforderlich. Geben Sie den Namen der Ressourcengruppe als Teil des Setupbefehls ein, indem Sie das Flag „-g“ verwenden.

    ```CLI
    az ml env setup -n <new deployment environment name> --location <e.g. eastus2> -g <existing resource group name>
    ```

    Befolgen Sie die Anweisungen auf dem Bildschirm, um ein Speicherkonto zum Speichern von Docker-Images, eine Azure Container Registry zum Auflisten von Docker-Images und ein Azure Application Insights-Konto zum Erfassen von Telemetriedaten bereitzustellen. **Wenn Sie den Switch „-c“ verwenden, wird mit dem Befehl zusätzlich ein Container Service-Cluster erstellt.**

    Der Clustername ist eine Möglichkeit zur Identifizierung der Umgebung. Der Standort sollte dem Standort des Kontos für die Modellverwaltung entsprechen, das über das Azure-Portal erstellt wurde.

    Überprüfen Sie den Status mithilfe des folgenden Befehls, um sicherzustellen, dass die Umgebung erfolgreich eingerichtet wurde:

    ```CLI
    az ml env show -n <deployment environment name> -g <existing resource group name>
    ```

    Achten Sie darauf, dass „Bereitstellungsstatus“ wie hier gezeigt den Wert „Erfolgreich“ enthält, bevor die Umgebung in Schritt 5 festgelegt wird:

    ![Bereitstellungsstatus](media\azure-stack-solution-machine-learning\image57.png)

1.  Richten Sie die Umgebung ein.

    Verwenden Sie nach Abschluss der Einrichtung den folgenden Befehl, um die Umgebungsvariablen festzulegen, die zum Operationalisieren der Umgebung erforderlich sind. Verwenden Sie den gleichen Umgebungsnamen wie zuvor in Schritt 3. Verwenden Sie den gleichen Ressourcengruppennamen, der im Befehlsfenster ausgegeben wurde, nachdem der Setupprozess abgeschlossen war.

    ```CLI
        az ml env set -n <deployment environment name> -g <existing resource group name>
    ```

1.  Geben Sie den folgenden Befehl ein, um zu überprüfen, ob die operationalisierte Umgebung für die lokale Webdienstbereitstellung richtig konfiguriert ist:

    ```CLI
    az ml env show
    ```

    Erstellen Sie nun den Echtzeit-Webdienst.

    > [!Note]  
    > Für nachfolgende Webdienstbereitstellungen können Sie das Konto und die Umgebung für die Modellverwaltung wiederverwenden. Es ist nicht erforderlich, diese Komponenten für jeden Webdienst zu erstellen. Einem Konto oder einer Umgebung können mehrere Webdienste zugeordnet sein.

### <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Erstellen eines Echtzeit-Webdiensts mit separaten Befehlen

Als Alternative zum oben gezeigten Befehl **az ml service create realtime** können diese Schritte auch separat ausgeführt werden.

Registrieren Sie zuerst das Modell. Generieren Sie anschließend das Manifest, erstellen Sie das Docker-Image, und erstellen Sie den Webdienst. Bei diesem Schritt-für-Schritt-Ansatz kann bei jedem Schritt flexibler vorgegangen werden. Außerdem können die in den vorherigen Schritten generierten Entitäten wiederverwendet werden.

1. Registrieren Sie das Modell, indem Sie den Namen der pickle-Datei angeben.

    ```CLI
    az ml model register --model model.pkl --name model.pkl
    ```

    Mit diesem Befehl wird eine Modell-ID generiert.

2.  Erstellen Sie ein Manifest.

    Verwenden Sie zum Erstellen eines Manifests den folgenden Befehl, und geben Sie die Modell-ID aus dem vorherigen Schritt an: 
    
    ```CLI
    az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
    ```

    Mit diesem Befehl wird eine Manifest-ID generiert.

3.  Erstellen Sie ein Docker-Image.

    Verwenden Sie zum Erstellen eines Docker-Images den folgenden Befehl, und geben Sie den Manifest-ID-Wert aus dem vorherigen Schritt an. Die Conda-Abhängigkeiten können auch über den Switch `-c` genutzt werden. 
    
    ```CLI
    az ml image create -n irisimage --manifest-id <manifest ID> -c aml_config\conda_dependencies.yml
    ```
    
    Mit diesem Befehl wird eine Docker-Image-ID generiert.

2.  Erstellen Sie den Dienst.

    Verwenden Sie zum Erstellen eines Diensts den folgenden Befehl, und geben Sie die Image-ID aus dem vorherigen Schritt an: 
    
    ```CLI
    az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
    ```
    
    Mit diesem Befehl wird eine Webdienst-ID generiert.
    
    Führen Sie als Nächstes den Webdienst aus.

## <a name="step-6-deploy-a-kubernetes-cluster-to-azure-stack"></a>Schritt 6: Bereitstellen eines Kubernetes-Clusters in Azure Stack

Bereitstellen eines Kubernetes-Clusters in Azure Stack

Kubernetes kann mithilfe von Azure Resource Manager-Vorlagen, die mit der Azure Container Services-Engine (ACS) generiert wurden, in Azure Stack installiert werden. [*Kubernetes*](https://kubernetes.io/) ist ein Open Source-System für die Automatisierung der Bereitstellung, Skalierung und Verwaltung von Anwendungen in Containern. Ein [*Container*](https://www.docker.com/what-container) ist ähnlich wie ein virtueller Computer in einem Image enthalten. Im Gegensatz zu einem virtuellen Computer enthält das Containerimage aber die Ressourcen, die zum Ausführen einer Anwendung benötigt werden, z.B. den Code, die Runtime zum Ausführen des Codes, bestimmte Bibliotheken und Einstellungen.

Verwenden Sie Kubernetes für folgende Zwecke:

 -  Entwickeln hochgradig skalierbarer, aktualisierbarer Anwendungen, die in wenigen Sekunden bereitgestellt werden können

 -  Vereinfachen des Entwurfs der Anwendung und Verbessern ihrer Zuverlässigkeit durch verschiedene Helm-Anwendungen. [*Helm*](https://github.com/kubernetes/helm) ist ein Open Source-Verpackungstool, das dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten.

 -  Einfaches Überwachen und Diagnostizieren der Integrität von Anwendungen mit Skalierungs- und Upgradefunktionen

> [!Note]  
> Da die Installation des Clusters ungefähr eine Stunde dauert, sollten Sie entsprechend planen.

### <a name="prerequisites-for-kubernetes-cluster-deployment"></a>Voraussetzungen für die Bereitstellung von Kubernetes-Clustern

Überprüfen Sie die Berechtigungen und die Azure Stack-Bereitschaft, um zu beginnen:

1.  Vergewissern Sie sich, dass das Element **Kubernetes-Cluster erstellen (Vorschauversion)** auf dem Azure Stack Marketplace verfügbar ist. Falls dieses Element fehlt, können Sie sich an einen Azure Stack-Bediener wenden, um es der Azure Stack-Umgebung hinzufügen zu lassen.

2.  Überprüfen Sie, ob Sie Anwendungen im Azure Active Directory-Mandanten (Azure AD) erstellen können. Für die Kubernetes-Bereitstellung sind Berechtigungen erforderlich.

    Anweisungen zum Überprüfen der Berechtigungen finden Sie unter [*Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

3.  Generieren Sie ein öffentlich-privates SSH-Schlüsselpaar für die Anmeldung beim virtuellen Linux-Computer in Azure Stack. Der öffentliche Schlüssel wird beim Erstellen des Clusters benötigt. Eine Anleitung finden Sie unter [Generieren eines Authentifizierungsschlüssels für SSH](#generate-an-authenticatio-key-for-ssh).

4.  Überprüfen Sie, ob das Abonnement im Azure Stack-Mandantenportal gültig ist und ob genügend öffentliche IP-Adressen zum Hinzufügen neuer Anwendungen verfügbar sind.

    Der Cluster kann nicht in einem Azure Stack-Abonnement vom Typ **Administrator** bereitgestellt werden. Verwenden Sie ein Abonnement vom Typ **Benutzer**.

###  <a name="generate-an-authentication-key-for-ssh"></a>Generieren eines Authentifizierungsschlüssels für SSH

Verwenden Sie in der „Windows-Subsystem für Linux“-Sitzung die folgenden Befehle, um einen Authentifizierungsschlüssel zu generieren: 

1. Geben Sie Folgendes ein: 

    ```Bash  
    ssh-keygen -t rsa
    ```
    
2. Wählen Sie `id_rsa`, wenn Sie dazu aufgefordert werden. 
3. Erstellen Sie eine Passphrase, wenn die entsprechende Aufforderung angezeigt wird. Es ist wichtig, dass Sie sich dieses Kennwort zur späteren Verwendung notieren. 
    Die Ausgabe sieht etwa wie folgt aus: 
    
    ```Bash  
    Your identification has been saved in `id_rsa`.
    Your public key has been saved in `id_rsa.pub`. 
    The key fingerprint is: SHA256:lUtUUjzaqWqGeolEPKeBmsnrhcNGM9Dn2OxYatt05SE  <user>@<machine-name>
    The key's randomart image is:  
    ```
    ![Alt text](media\azure-stack-solution-machine-learning\image58.png)

4. Fügen Sie nach dem Generieren des Schlüssels die Schlüsselinformationen ein, indem Sie die folgenden Befehle verwenden: 
    ```Bash
    cat id_rsa.pub
    ```
    Die Ausgabe sieht etwa wie folgt aus: 
    ```Bash
    ssh-rsa  AAAAB3NzaC1yc2EAAAADAQABAAABAQDHaWrAktR3BlQ356T8Qvv8O2Q/U/hsXQwS9xJbuduuc9lkJwddzgmNCyM9PooZWYtGVXyHU6SC3YH1XkwqGtKhtPb03d24hmhX1euAaqIqHHSp4WgUS5s1gNsp37L8QCSGNCnF31FWavI8bnjOjccUkMowKl8iyGN++5UyQgNuynEVUbFJjrntoDL66HUu88xDxTcVB7rqDr2QKFwYJkg4HSoHYpezJd7W8kcmv33eh0xs8nlDA7Dzu7zXpyVc54bH9XtPR6EUXaQa+UqKaNlQNiJqEs+1X/zNuK9V6NLVNiO0h3jCHI3K8o4VnZyRKHCQProasiiPLUUJ6SF/RTLN  <user>@<machine-name>
    ```
    
5. Kopieren Sie den generierten Schlüssel in das Feld „Öffentlicher SSH-Schlüssel“.

### <a name="create-a-service-principal-in-azure-ad"></a>Erstellen eines Dienstprinzipals in Azure AD

1.  Melden Sie sich am globalen [*Azure-Portal*](http://www.poartal.azure.com/) an.

2.  Melden Sie sich mit dem Azure AD-Mandanten an, der der Azure Stack-Instanz zugeordnet ist.

3.  Erstellen Sie eine Azure AD-Anwendung.

    1. Wählen Sie **Azure Active Directory** > **+ App-Registrierungen**> **Registrierung einer neuen Anwendung**.
    2. Geben Sie unter **Name** einen Namen für die Anwendung ein. 
    3. Wählen Sie **Web-App/API** aus. 
    4. Geben Sie `http://localhost` als **Anmelde-URL** ein. 
    5. Klicken Sie auf **Erstellen**.

1.  Notieren Sie den Wert der **Anwendungs-ID**. Diese ID wird beim Erstellen des Clusters benötigt und als **Client-ID des Dienstprinzipals** angegeben.

2.  Wählen Sie **Einstellungen** > **Schlüssel** aus.

    1. Geben Sie die **Beschreibung** ein. 
    2. Wählen Sie unter **Gültig bis** die Option **Läuft nie ab** aus. 
    3. Wählen Sie **Speichern**aus. Notieren Sie sich die Schlüsselzeichenfolge. Diese Schlüsselzeichenfolge wird beim Erstellen des Clusters benötigt und als **Clientgeheimnis des Dienstprinzipals** angegeben.

### <a name="give-the-service-principal-access"></a>Erteilen des Zugriffs für den Dienstprinzipal

Gewähren Sie für den Dienstprinzipal Zugriff auf das Abonnement, damit darüber Ressourcen erstellt werden können.

1.  Melden Sie sich beim [Verwaltungsportal](https://adminportal.local.azurestack.external/) an.

2.  Wählen Sie **Weitere Dienste** > **Benutzerabonnements ** > **+ Hinzufügen**.

3.  Wählen Sie das erstellte Abonnement aus.

4.  Klicken Sie auf **Zugriffssteuerung (IAM)** > **+ Hinzufügen**.

5.  Wählen Sie die Rolle **Besitzer**.

6.  Wählen Sie den für den Dienstprinzipal erstellten Anwendungsnamen aus. Geben Sie den Namen in das Suchfeld ein, falls dies erforderlich ist.

7.  Wählen Sie **Speichern**aus.

8.  Öffnen Sie das [Azure Stack-Portal](https://portal.local.azurestack.external).

9.  Klicken Sie auf **+Neu** > **Compute** > **Kubernetes-Cluster**. Klicken Sie auf **Erstellen**.

    ![Bereitstellen einer Lösungsvorlage](media\azure-stack-solution-machine-learning\image59.png)

10\. Wählen Sie in „Kubernetes-Cluster erstellen“ die Option **Grundlagen** aus.

    ![Deploy Solution Template](media\azure-stack-solution-machine-learning\image60.png)

11. Geben Sie unter **Linux VM Admin Username** (Benutzername des Linux-VM-Administrators) einen Namen ein. Hierbei handelt es sich um den Benutzernamen für die virtuellen Linux-Computer, die Teil des Kubernetes-Clusters und des DVM (Deployment Virtual Machine, virtueller Computer für die Bereitstellung) sind.

12. Geben Sie den **öffentlichen SSH-Schlüssel** für die Autorisierung bei allen Linux-Computern ein, die als Teil des Kubernetes-Clusters und des DVM erstellt werden.

13. Geben Sie das für die Region eindeutige **DNS-Präfix des Masterprofils** ein. Diese Angabe muss ein für die Region eindeutiger Name sein, z.B. `ask8s-12345`. Es wird empfohlen, den gleichen Namen wie für die Ressourcengruppe zu wählen.

    > [!Note]  
    > Verwenden Sie für jeden Cluster ein neues, eindeutiges DNS-Präfix des Masterprofils.

14. Geben Sie unter **Agent Pool Profile Count** (Agentpool-Profilanzahl) einen Wert ein. Er gibt die Anzahl von Agents im Cluster an. Sie können einen Wert von 1 bis 4 angeben.

15. Geben Sie unter **Service Principal ClientId** (Dienstprinzipal-ClientId) einen Wert ein. Dieser wird vom Kubernetes Azure-Cloudanbieter verwendet.

16. Geben Sie unter **Clientgeheimnis des Dienstprinzipals** das Clientgeheimnis für den Dienstprinzipal ein, das beim Erstellen der Dienstprinzipalanwendung generiert wurde.

17. Geben Sie die **Version des Kubernetes Azure Cloudanbieters** ein. Dabei handelt es sich um die Version des Kubernetes Azure-Anbieters. Azure Stack veröffentlicht für jede Azure Stack-Version einen benutzerdefinierten Kubernetes-Build.

18. Wählen Sie die **Abonnement-ID** aus.

19. Geben Sie den Namen einer neuen Ressourcengruppe ein, oder wählen Sie eine vorhandene Ressourcengruppe aus. Der Ressourcenname muss alphanumerisch und in Kleinbuchstaben angegeben sein.

20. Wählen Sie den **Standort** der Ressourcengruppe aus. Dies ist die Region, die für die Azure Stack-Installation ausgewählt wird.

### <a name="specify-the-azure-stack-settings"></a>Angeben der Einstellungen für Azure Stack

1.  Wählen Sie **Azure Stack Stamp Settings** (Azure Stack-Stempeleinstellungen) aus.

    ![Bereitstellen einer Lösungsvorlage](media\azure-stack-solution-machine-learning\image61.png)

2.  Geben Sie den **Azure Resource Manager-Endpunkt des Mandanten** ein. Dabei handelt es sich um den Azure Resource Manager-Endpunkt für die Verbindungsherstellung, um die Ressourcengruppe für den Kubernetes-Cluster zu erstellen. Der Endpunkt des Azure Stack-Bedieners wird für ein integriertes System benötigt. Verwenden Sie für das Azure Stack Development Kit (ASDK) Folgendes: `https://management.local.azurestack.external`.

3.  Geben Sie die **Mandanten-ID** für den Mandanten ein. Den Wert für die Mandanten-ID finden Sie unter [*Abrufen der Mandanten-ID*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

### <a name="install-kubectl-on-windows-powershell-environment"></a>Installieren von kubectl in der Windows PowerShell-Umgebung

Führen Sie in der WSL-Umgebung die folgenden Befehle aus, um kubectl in der WSL-Umgebung zu installieren.

```PowerShell  
Install-script -name install-kubectl -scope CurrentUser -force
Install-kubectl.ps1 -downloadlocation “C:\Users\<Current User>\Documents\Kube
```

### <a name="install-kubectl-on-the-windows-subsystem-for-linux-environment"></a>Installieren von kubectl im Windows-Subsystem für die Linux-Umgebung

Führen Sie in der WSL-Umgebung die folgenden Befehle aus, um kubectl in der WSL-Umgebung zu installieren.

```Bash  
    apt-get update && apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
    cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
    deb http://apt.kubernetes.io/ kubernetes-xenial main
    EOF
    apt-get update
    apt-get install -y kubectl
```

### <a name="configure-kubectl"></a>Konfigurieren von kubectl

Damit kubectl einen Kubernetes-Cluster finden und darauf zugreifen kann, wird eine *kubeconfig-Datei* benötigt. Sie wird automatisch erstellt, wenn mit „kube-up.sh“ ein Cluster erstellt oder ein Minikube-Cluster bereitgestellt wird. Weitere Informationen zur Erstellung von Clustern finden Sie in den [*Leitfäden zu den ersten Schritten*](https://kubernetes.io/docs/setup/). Informationen zum Zugriff auf einen Cluster, der von einem anderen Benutzer erstellt wurde, finden Sie in [*diesem Dokument zum Freigeben des Clusterzugriffs*](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/). Standardmäßig befindet sich die kubectl-Konfiguration unter **~.kube/config**.

### <a name="check-the-kubectl-configuration"></a>Überprüfen der kubectl-Konfiguration

Überprüfen Sie, ob kubectl richtig konfiguriert ist, indem Sie den Clusterstatus abrufen:

```Bash  
kubectl cluster-info
```

kubectl ist für den Zugriff auf den Cluster richtig konfiguriert, wenn die URL-Antwort angezeigt wird.

kubectl ist nicht richtig konfiguriert oder kann keine Verbindung mit einem Kubernetes-Cluster herstellen, wenn die folgende Meldung angezeigt wird:

```Bash  
The connection to the server <server-name:port> was refused -  did you specify the right host or port?
```

Wenn Sie beispielsweise einen Kubernetes-Cluster auf einem lokalen Laptop ausführen, ist unter Umständen ein Tool (Minikube oder ein ähnliches Tool) erforderlich, um die oben angegebenen Befehle erneut auszuführen.

Wenn in den kubectl-Clusterinformationen die URL-Antwort zurückgegeben wird, aber immer noch nicht auf den Cluster zugegriffen werden kann, sollten Sie wie folgt die Richtigkeit der Konfiguration überprüfen:

```Bash  
> kubectl cluster-info dump
```

### <a name="enable-shell-autocompletion"></a>Aktivieren der automatischen Vervollständigung für die Shell

kubectl unterstützt die automatische Vervollständigung, um die Shell-Aktivierung schnell und einfach zu machen.

Das eigentliche Vervollständigungsskript wird von kubectl generiert und ist über das Profil zugänglich.

### <a name="connect-to-the-kubernetes-cluster"></a>Herstellen einer Verbindung mit dem Kubernetes-Cluster

Zum Herstellen einer Verbindung mit dem Cluster wird der Kubernetes-Befehlszeilenclient (**kubectl**) benötigt. Eine Anleitung zum Herstellen einer Verbindung mit dem Cluster sowie zu seiner Verwaltung finden Sie in der [Azure Container Services-Dokumentation](https://docs.microsoft.com/azure/container-service/dcos-swarm/).

Ein beliebiger SSH-Client kann verwendet werden, um für den Kubernetes-Cluster eine Verbindung herzustellen. Das Windows-Subsystem für Linux (WSL) wird empfohlen. Der Computer, mit dem die Verbindung mit dem Kubernetes-Cluster hergestellt wird, befindet sich in der für den Cluster erstellten Ressourcengruppe und wird mit dem Präfix „vmd-edge-ml-stack“ gestartet.

```Bash  
ssh <user>@vmd-edge-ml-stack.<FQDN of Kubernetes Machine in  Azure Stack>
```

Führen Sie nach der Herstellung der Verbindung mit dem Kubernetes-Computer Folgendes aus, um die Kubernetes-Konfigurationsdatei zu erhalten.

```Bash  
sudo find / -name \*kubeconfig\* -type f
```

Die Ausgabe sieht in etwa wie folgt aus:

```Bash  
/var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Kopieren Sie diese Dateipfadinformationen, und führen Sie dann den folgenden Befehl aus, indem Sie den kopierten kubeconfig-Dateipfad einfügen:

```Bash  
sudo cat  /var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Die Ausgabe enthält einen großen Textblock. Dies ist der unformatierte JSON-Inhalt. Kopieren Sie diesen Ausgabetext, und fügen Sie diesen Code dann in eine Visual Studio-Datei ein. Speichern Sie sie als JSON-Datei. Das Ergebnis ist eine lokal gespeicherte Datei „kubeconfig.json“. (Speicherung im Verzeichnis „/mnt/c/users/<current user>/documents/Kube“ als „kubeconfig.json“)

### <a name="configure-the-kubernetes-cluster"></a>Konfigurieren des Kubernetes-Clusters

Nachdem die lokale JSON-Datei abgerufen wurde, können Sie in einer neuen WSL-Sitzung die folgenden Befehle verwenden, um den Cluster zu konfigurieren.

```Bash  
    cd /mnt/c/users/<current user>/documents/Kube
    kubectl proxy
    kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
    kubectl proxy
    set KUBECONFIG=”/mnt/c/users/<current user>/documents/Kube/kubeconfig.json”
    kubectl.exe config view
```

Die Kubernetes-Konfigurationseinstellungen werden definiert (siehe Ausgabe unten).

![Alt text](media\azure-stack-solution-machine-learning\image62.png)

Starten Sie den lokalen Proxydienst:

```Bash  
kubectl proxy
```

Navigieren Sie zur Kubernetes-Clusterbenutzeroberfläche mit der folgenden Adresse: `https://localhost:8001`.

![Alt text](media\azure-stack-solution-machine-learning\image63.png)

Sie verfügen nun über einen Ort zum Bereitstellen des Containers und einen Container in der Cloud, den Sie lokal anzeigen können.

![Alt text](media\azure-stack-solution-machine-learning\image64.png)

Passen Sie die Datei **iris_deployment.yaml** (in /*mnt/c/users/<current user>/documents/Kube directory*) so an, dass **webservicename** und **Image** und **Name** des Containers mit der Bereitstellung übereinstimmen, indem Sie einen Code-Editor Ihrer Wahl verwenden.

![Alt text](media\azure-stack-solution-machine-learning\image65.png)

Legen Sie den Containerport auf **5001** fest.

![Alt text](media\azure-stack-solution-machine-learning\image66.png)

Erstellen Sie anschließend das **imagePullSecret**-Element:

### <a name="create-a-secret-in-the-cluster-that-holds-the-authorization-token"></a>Erstellen eines Geheimnisses im Cluster, in dem das Autorisierungstoken enthalten ist

In einem Kubernetes-Cluster wird das Geheimnis vom Typ **docker-registry** verwendet, um die Authentifizierung für eine Containerregistrierung durchzuführen und ein privates Image per Pullvorgang zu beziehen.

Erstellen Sie dieses Geheimnis, und geben Sie ihm den Namen **azuremlcr**:

```Bash  
kubectl create secret docker-registry azuremlcr --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password= "<your-pword>" --docker-email=<your-email>
```

Suchen Sie nach Folgendem:

- **<your-registry-server>** ist der **Anmeldeserver** von Azure Container Registry.
- **<your-name>** ist der **Benutzername** von Azure Container Registry.
- **<your-pword>** ist das **Kennwort** von Azure Container Registry. Stellen Sie sicher, dass das Kennwort in Anführungszeichen gesetzt ist.
- **<your-email>** ist der Benutzer, der für den Container über Lese-/Schreibzugriff verfügt.
- Diese Informationen finden Sie in **Azure Container** **Registry** unter **Zugriffsschlüssel**.
- Die Docker-Anmeldeinformationen sind im Cluster jetzt als Geheimnis mit dem Namen **azuremlcr** festgelegt.

Speichern Sie die Datei **iris_deployment.yaml** (im Verzeichnis /*mnt/c/users/<current user>/documents/Kube*).

### <a name="create-the-kubernetes-container"></a>Erstellen des Kubernetes-Containers

```Bash  
kubectl.exe create -f /mnt/c/users/<current  user>/documents/Kube/iris_deployment.yaml
```

    ![Alt text](media\azure-stack-solution-machine-learning\image67.png)

Überprüfen Sie den Status der Bereitstellung:

```Bash  
Kubectl get deployments
```

    ![Alt text](media\azure-stack-solution-machine-learning\image68.png)

Es kann eine Weile dauern, bis die Bereitstellung abgeschlossen ist.

### <a name="configure-visual-studio-team-services-to-deploy-automatically"></a>Konfigurieren von Visual Studio Team Services für die automatische Bereitstellung

#### <a name="create-a-team-project"></a>Erstellen eines Teamprojekts

1.  Stellen Sie sicher, dass eine [Mitgliedschaft in der Administratorengruppe der Projektsammlung](https://docs.microsoft.com/vsts/organizations/security/set-project-collection-level-permissions?view=vsts) besteht. Zum Erstellen von Teamprojekten muss die Berechtigung **Neue Projekte erstellen** auf **Zulassen** festgelegt werden.

2.  Wählen Sie auf der Seite „Projekte“ die Option **Neues Projekt**.

    ![Alt text](media\azure-stack-solution-machine-learning\image69.png)

1.  Geben Sie dem Projekt den Namen **HybridMLIris**.

2.  Wählen Sie als Anfangstyp für die Quellcodeverwaltung **Git** aus.

3.  Wählen Sie einen Prozess und dann die Option **Erstellen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image70.png)

### <a name="import-some-code--create-repository"></a>Importieren von Code – Erstellen des Repositorys

Ein Git-Repository für YAML-Code ist erforderlich.

#### <a name="add-user-to-the-git-repo"></a>Hinzufügen eines Benutzers zum Git-Repository

1.  Wählen Sie im Standarddashboard des Projekts die Option „Git-Anmeldeinformationen generieren“.

    ![Alt text](media\azure-stack-solution-machine-learning\image71.png)

1.  Geben Sie bei Bedarf ein Kennwort ein, und wählen Sie „Git-Anmeldeinformationen speichern“.

    ![Alt text](media\azure-stack-solution-machine-learning\image72.png)

1.  Initialisieren Sie das Repository, indem Sie die Schaltfläche **Initialisieren** wählen. Erstellen Sie anschließend eine **README**-Infodatei.

    ![Alt text](media\azure-stack-solution-machine-learning\image73.png)

#### <a name="clone-the-git-repository-locally-and-upload-the-code"></a>Klonen Sie das Git-Repository lokal, und laden Sie den Code hoch. 

1.  Erstellen Sie auf dem Computer ein Verzeichnis unter `c:\\users\\<User>\\source\\repos\\hybridMLIris`, und klonen Sie das Repository.

    ```Bash  
    sudo mkdir /mnt/c/users/<User>/source sudo mkdir /mnt/c/users/<User>/source/repos sudo mkdir /mnt/c/users/<User>/source/repos/hybridMLIris cd /mnt/c/users/<User>/source/repos/hybridMLIris sudo git clone  https://<yourvstssite>.visualstudio.com/HybridMLIris/_git/HybridMLIris
    ```

    ![Alt text](media\azure-stack-solution-machine-learning\image74.png)

1.  Navigieren Sie zum neu geklonten Repository:

    ```Bash  
    ls
    cd ./HybridMLIris
    ```
    
    ![Alt text](media\azure-stack-solution-machine-learning\image75.png)

1.  Kopieren Sie die Datei **iris_deployment.yaml** in das Repository.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_deployment.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_deployment.yaml
    ``` 

1.  Committen der Änderung in Git

    ```Bash  
    git add . git commit -m Added Deployment YAML git push
    ```

    ![Alt text](media\azure-stack-solution-machine-learning\image76.png)

### <a name="prepare-the-private-build-and-release-agent-for-vsts-integration"></a>Vorbereiten des privaten Build- und Release-Agents für die VSTS-Integration

#### <a name="prerequisites"></a>Voraussetzungen

VSTS führt die Authentifizierung gegenüber Azure Resource Manager mit einem Dienstprinzipal durch. Damit VSTS Ressourcen in einem Azure Stack-Abonnement bereitstellen kann, ist der Status „Mitwirkender“ erforderlich.\ **Hier sind die allgemeinen Schritte angegeben, die konfiguriert werden müssen, um diese Authentifizierung zu aktivieren:**

1.  Es muss ein Dienstprinzipal erstellt oder ein bereits vorhandener Dienstprinzipal verwendet werden.

2.  Für den Dienstprinzipal müssen Authentifizierungsschlüssel erstellt werden.

3.  Das Azure Stack-Abonnement muss über die rollenbasierte Zugriffssteuerung überprüft werden, damit der SPN Teil der Rolle „Mitwirkender“ werden kann.

4.  In VSTS muss unter Verwendung der Azure Stack-Endpunkte und der SPN-Informationen eine neue Dienstdefinition erstellt werden.

#### <a name="service-principal-creation"></a>Dienstprinzipalerstellung

Gehen Sie gemäß der [Anleitung zum Erstellen eines Dienstprinzipals](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) vor, und wählen Sie als Anwendungstyp die Option „Web-App/API“ aus.

**Zugriffsschlüsselerstellung**

Ein Dienstprinzipal benötigt einen Schlüssel für die Authentifizierung. Führen Sie die Schritte in diesem Abschnitt aus, um einen Schlüssel zu generieren.

1.  Wählen Sie in Azure Active Directory unter **App-Registrierungen** die Anwendung aus.

    ![Anwendung auswählen](media\azure-stack-solution-machine-learning\image77.png)

1.  Notieren Sie sich den Wert von **„Anwendungs-ID“. Der Wert wird beim Konfigurieren des Dienstendpunkts in VSTS verwendet.**

    ![Alt text](media\azure-stack-solution-machine-learning\image78.png)

1.  Klicken Sie zum Generieren eines Authentifizierungsschlüssels auf **Einstellungen**.

    ![Klicken auf „Einstellungen“](media\azure-stack-solution-machine-learning\image79.png)

1.  Wählen Sie **Schlüssel** aus.

    ![Schlüssel auswählen](media\azure-stack-solution-machine-learning\image80.png)

1.  Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.

    ![Schlüssel speichern](media\azure-stack-solution-machine-learning\image81.png)

Nach dem Speichern des Schlüssels wird der Wert des Schlüssels angezeigt. Kopieren Sie diesen Wert, da Sie ihn später noch benötigen. Der **Schlüsselwert** und die Anwendungs-ID sind für die Anmeldung als Anwendung erforderlich. Speichern Sie den Schlüsselwert an einem Ort, von dem die Anwendung ihn abrufen kann.

![Alt text](media\azure-stack-solution-machine-learning\image82.png)

#### <a name="get-tenant-id"></a>Abrufen der Mandanten-ID

Im Rahmen der Dienstendpunkt-Konfiguration muss in VSTS die **Mandanten-ID** angegeben werden, die dem AAD-Verzeichnis entspricht, in dem der Azure Stack-Stempel bereitgestellt wurde. Führen Sie die Schritte in diesem Abschnitt aus, um die Mandanten-ID zu ermitteln.

1.  Wählen Sie **Azure Active Directory**.

    ![Azure Active Directory auswählen](media\azure-stack-solution-machine-learning\image83.png)

1.  Wählen Sie zum Abrufen der Mandanten-ID die Option **Eigenschaften** für den Azure AD-Mandanten aus.

    ![Auswählen von Azure AD-Eigenschaften](media\azure-stack-solution-machine-learning\image84.png)

1.  Kopieren Sie die **-Verzeichnis-ID**. Dieser Wert ist die Mandanten-ID.

    ![tenant ID](media\azure-stack-solution-machine-learning\image85.png)

Gewähren von Rechten für den Dienstprinzipal zum Bereitstellen von Ressourcen im Azure Stack-Abonnement

Weisen Sie die Anwendung einer Rolle zu, um auf Ressourcen im Abonnement zuzugreifen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Legen Sie den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene fest. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn beispielsweise der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1.  Navigieren Sie zur gewünschten Bereichsebene, um die Anwendung zuzuweisen. Um z.B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Abonnements** aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image86.jpeg)

1.  Wählen Sie das **Abonnement** (Ressourcengruppe oder Ressource) für die Zuweisung der Anwendung aus.

    ![Abonnement für Zuweisung auswählen](media\azure-stack-solution-machine-learning\image87.png)

1.  Wählen Sie **Access Control (IAM)** aus.

    ![„Zugriff“ auswählen](media\azure-stack-solution-machine-learning\image88.png)

1.  Wählen Sie **Hinzufügen**.

    ![„Hinzufügen“ wählen](media\azure-stack-solution-machine-learning\image89.png)

1.  Wählen Sie die Rolle für die Zuweisung der Anwendung aus. In der folgenden Abbildung ist die Rolle **Besitzer** dargestellt.

    ![Alt text](media\azure-stack-solution-machine-learning\image90.png)

1.  Azure Active Directory-Anwendungen werden standardmäßig nicht in den verfügbaren Optionen angezeigt. Sie können nach der Anwendung suchen, indem Sie im Suchfeld **den Namen angeben**. Wählen Sie den Namen dann aus, wenn er angezeigt wird.

    ![Alt text](media\azure-stack-solution-machine-learning\image91.png)

1.  Wählen Sie **Speichern** aus, um das Zuweisen der Rolle abzuschließen. Die Anwendung wird in der Liste mit den Benutzern angezeigt, die einer Rolle für diesen Bereich zugewiesen sind.

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Die rollenbasierte Zugriffssteuerung in Azure (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure und Azure Stack. Mit RBAC können den Benutzern nur die Zugriffsrechte gewährt werden, die diese zum Ausführen ihrer Aufgaben benötigen. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

**VSTS-Agentpools**

Agents werden in **Agentpools** zusammengefasst, um sie nicht alle einzeln verwalten zu müssen. Ein Agentpool definiert die Freigabegrenze für alle Agents in diesem Pool. In VSTS gelten Agentpools für das gesamte VSTS-Konto und können daher zwischen Teamprojekten gemeinsam genutzt werden. Weitere Informationen sowie ein Tutorial zum Erstellen von VSTS-Agentpools finden Sie unter [Agent pools and queues](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts) (Agent-Pools und -Warteschlangen).

**Hinzufügen eines persönlichen Zugriffstokens (PAT) für Azure Stack**

 -  Melden Sie sich am VSTS-Konto an, und wählen Sie den Namen **Kontoprofil**.

 -  Klicken Sie auf **Sicherheit verwalten**, um die Seite für die Tokenerstellung zu öffnen.

![Alt text](media\azure-stack-solution-machine-learning\image92.png)

![Alt text](media\azure-stack-solution-machine-learning\image93.jpeg)

![Alt text](media\azure-stack-solution-machine-learning\image94.jpeg)

> [!Note]  
> Rufen Sie die Tokeninformationen ab. Sie werden nach Verlassen dieses Bildschirms nicht erneut angezeigt.

1.  Kopieren Sie das **Token**.

    ![Alt text](media\azure-stack-solution-machine-learning\image95.png)

#### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Installieren des VSTS-Build-Agents auf dem von Azure Stack gehosteten Buildserver

1.  Stellen Sie eine Verbindung mit dem **Buildserver** her, der auf dem Azure Stack-Host bereitgestellt wird.

    > [!Note]  
    > Stellen Sie sicher, dass auf den unter Azure Stack gehosteten Buildserver über das öffentliche Internet zugegriffen werden kann. Wenn dies nicht der Fall ist, sollten Sie mit einem Azure Stack-Bediener zusammenarbeiten, um den Zugriff zu ermöglichen.

    ```Bash  
    ssh <user>@<buildserver.publicip>
    ```

2.  Aktualisieren Sie den Ubuntu-Buildserver auf die aktuelle Version (18.04):

    ```Bash  
    sudo su
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    apt-get autoremove
    do-release-upgrade -d
    ```

    > [!Note]  
    > Dieser Vorgang dauert eine Weile.

2.  Installieren Sie die Anwendungen, die für den Buildserver erforderlich sind. Führen Sie über die Bash-Shell des Ubuntu-basierten Buildservers die folgenden Befehle aus:

    ```Bash  
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list 
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install liblttng-ust0 libcurl3 libssl1.0.0 curl lsb-release libkrb5-3 zlib1g libicu60 aspnetcore-runtime-2.1 dotnet-sdk-2.1
    wget https://github.com/PowerShell/PowerShell/releases/download/v6.1.0-preview.3/powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo dpkg -i powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo apt-get install -f
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add –
    sudo apt-get update && sudo apt-get install azure-cli
    ```

2.  Laden Sie den Build-Agent herunter, stellen Sie ihn als Dienst unter Verwendung eines **persönlichen Zugriffstokens (Personal Access Token, PAT)** bereit, und führen Sie ihn als VM-Administratorkonto aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image96.png)

    ```Bash  
        cd \home\<user>
        sudo mkdir myagent && cd myagent
        wget https://vstsagentpackage.azureedge.net/agent/2.134.2/vsts-agent-linux-x64-2.134.2.tar.gz
        sudo tar zxvf vsts-agent-linux-x64-2.134.2.tar.gz
    ```

2.  Wechseln Sie zum Ordner mit dem extrahierten Build-Agent. Führen Sie den folgenden Code aus.

    ```Bash  
        cd ..
        sudo chmod -R 777 myagent
        cd myagent
        ./config.sh
    ```

    ![Alt text](media\azure-stack-solution-machine-learning\image97.png)

2.  Führen Sie nach Abschluss von **./config.sh** den folgenden Code aus, um den Dienst beim Serverstart zu aktivieren, und starten Sie den Dienst:

    ```Bash  
    sudo ./svc.sh install
    sudo ./svc.sh start
    ```

Der Agent ist jetzt im VSTS-Ordner sichtbar.

#### <a name="endpoint-creation-permissions"></a>Berechtigungen für die Endpunkterstellung

Benutzer können Endpunkte erstellen und so VSTO-Builds die Bereitstellung von Azure Service-Apps im Stapel ermöglichen. VSTS stellt eine Verbindung mit dem Build-Agent her, der wiederum eine Verbindung mit Azure Stack herstellt.

![Alt text](media\azure-stack-solution-machine-learning\image98.png)

1.  Klicken Sie im Menü **Einstellungen** auf **Sicherheit**.

2.  Wählen Sie links in der Liste **VSTS-Gruppen** die Option **Endpunktersteller** aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image99.png)

3.  Klicken Sie auf der Registerkarte **Mitglieder** auf **+ Hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image100.png)

1.  Geben Sie **Benutzername** ein, und wählen Sie den Benutzernamen in der Liste aus.

2.  Klicken Sie auf **Save changes** (Änderungen speichern).

    ![Alt text](media\azure-stack-solution-machine-learning\image101.png)

3.  Wählen Sie links in der Liste **VSTS-Gruppen** die Option **Endpunktadministratoren** aus.

4.  Klicken Sie auf der Registerkarte **Mitglieder** auf **+ Hinzufügen**.

5.  Geben Sie einen **Benutzernamen** ein, und wählen Sie den entsprechenden Benutzer in der Liste aus.

6.  Klicken Sie auf **Änderungen speichern**.

    ![buchatech](media\azure-stack-solution-machine-learning\image102.jpeg)

    Der Build-Agent in Azure Stack erhält Anweisungen von VSTS. Daraufhin werden Endpunktinformationen für die Kommunikation mit der Azure Stack-Instanz übermittelt.

    Die Verbindung zwischen VSTS und Azure Stack ist nun bereit.

    ![Alt text](media\azure-stack-solution-machine-learning\image103.png)

### <a name="configure-build-and-release-definitions"></a>Konfigurieren von Build- und Releasedefinitionen

Nachdem die Verbindungen hergestellt wurden, ordnen Sie den erstellten Azure-Endpunkt, AKS und Azure Container Registry manuell den Build- und Releasedefinitionen zu.

#### <a name="create-the-build-definition-for-the-yaml-code"></a>Erstellen der Builddefinition für den YAML-Code

1.  Wählen Sie unter dem Hub für Build und Release den Abschnitt „Builds“, und erstellen Sie eine neue Definition.

    ![Alt text](media\azure-stack-solution-machine-learning\image104.png)

1.  Wählen Sie „VSTS Git“ und dann das zuvor erstellte Repository aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image105.png)

1.  Wählen „Leere Pipeline“ als Vorlage.

    ![Alt text](media\azure-stack-solution-machine-learning\image106.png)

1.  Geben Sie dem Build den Namen **Copy Artifact**, und wählen Sie den Azure Stack-Buildserver für die Agent-Warteschlange aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image107.png)

1.  Wählen Sie in den Prozessen Phase 1 aus, und benennen Sie sie in **Copy Artifact** um. Fügen Sie der Phase dann eine Aufgabe hinzu (**Aufgabe hinzufügen**):

    ![Alt text](media\azure-stack-solution-machine-learning\image108.png)

1.  Wählen Sie in der Liste **Hilfsprogramm** die Option **Buildartefakte veröffentlichen** und dann **Hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image109.png)

1.  Wählen Sie **Pfad für Veröffentlichung** und dann die Datei **iris_deployment.yaml**.

    ![Alt text](media\azure-stack-solution-machine-learning\image110.png)

1.  Geben Sie dem Artefakt den Namen **iris_deployment**, und wählen Sie als Veröffentlichungsort **Visual Studio Team Services/TFS** aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image111.png)

1.  Wählen Sie **Speichern und in Warteschlange einreihen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image112.png)

1.  Überprüfen Sie den Status des Builds, indem Sie die Build-ID auswählen.

    ![Alt text](media\azure-stack-solution-machine-learning\image113.png)

Bei einer erfolgreichen Durchführung wird in etwa Folgendes ausgegeben:

![Alt text](media\azure-stack-solution-machine-learning\image114.png)

#### <a name="create-the-release-definition-for-the-yaml-code"></a>Erstellen der Releasedefinition für den YAML-Code

1.  Wählen Sie unter dem Hub für Build und Release den Abschnitt „Releases“ und dann eine neue Definition aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image115.png)

1.  Wählen Sie „Leere Pipeline“ als Vorlage.

    ![Alt text](media\azure-stack-solution-machine-learning\image106.png)

1.  Geben Sie der Umgebung den Namen „Azure Stack“.

    ![Alt text](media\azure-stack-solution-machine-learning\image116.png)

1.  Fügen Sie ein neues Artefakt hinzu, indem Sie **Artefakte** und **+ Hinzufügen** auswählen.

2.  Wählen Sie als Quelltyp „Build“ und **HybridMLIris** als Projekt aus.

3.  Wählen Sie anschließend die Builddefinition aus, die zuvor für die Quelle erstellt wurde.

4.  Wählen Sie anschließend **Hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image117.png)

1.  Wählen Sie „Azure Stack“ als Umgebung aus, und fügen Sie Azure Stack dann eine neue Aufgabe hinzu.

    ![Alt text](media\azure-stack-solution-machine-learning\image118.png)


1.  Legen Sie für die Phase „Agent“ die Agent-Warteschlange auf den gehosteten Azure Stack-Buildserver fest.

    ![Alt text](media\azure-stack-solution-machine-learning\image119.png)

1.  Fügen Sie dieser Phase eine neue Aufgabe hinzu, wählen Sie unter „Bereitstellen“ die Aufgabe „Für Kubernetes bereitstellen“ aus, und wählen Sie dann die Option „Hinzufügen“.

    ![Alt text](media\azure-stack-solution-machine-learning\image120.png)


1.  Verwenden Sie den Namen **Kubectl Apply** (Standardname), und wählen Sie den apply-Befehl aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image121.png)

    Erstellen Sie jetzt eine neue Kubernetes-Dienstverbindung.

#### <a name="create-kubernetes-service-endpoint"></a>Erstellen eines Kubernetes-Dienstendpunkts

1.  Wählen Sie unter „Kubernetes-Dienstverbindung“ die Schaltfläche **+ Neu** und dann in der Liste die Option **Kubernetes**. Sie können diesen Endpunkt verwenden, um **VSTS** und **Azure Container Service (AKS)** zu verbinden.

2.  **Verbindungsname**: Geben Sie den Verbindungsnamen an.

3.  **Server-URL**: Geben Sie die Adresse des Containerdiensts im Format „http://{API-Serveradresse}“ an.

4.  **Kubeconfig**: Führen Sie zum Abrufen des Kubeconfig-Werts die folgenden Azure-Befehle an einer Eingabeaufforderung aus, die mit Administratorrechten gestartet wurde.

    > [!Important]  
    > Verwenden Sie diese CLI-Fenster, um die nächsten Schritte ausführen zu können.

6.  Melden Sie sich im CLI-Fenster an Azure an. [Hier finden Sie weitere Informationen zu „az login“](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

7.  Geben Sie an der Eingabeaufforderung Folgendes ein:

    ```CLI
        az login
    ```

10. Mit diesem Befehl wird ein Code zurückgegeben, den Sie im Browser unter <https://aka.ms/devicelogin> verwenden können.

11. Navigieren Sie im Browser zu <https://aka.ms/devicelogin>. Geben Sie bei entsprechender Aufforderung den Code, den Sie in der CLI erhalten haben, in den Browser ein.

    ![Kubernetes-Dienstendpunkt](media\azure-stack-solution-machine-learning\image122.png)

1.  Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, um die Anmeldeinformationen für den Zugriff auf den Kubernetes-Cluster zu erhalten.

### <a name="azure-ml-workbench-cli"></a>Azure ML Workbench-CLI

az aks get-credentials resource-group <yourResourceGroup> name <yourazurecontainerservice>

![Kubernetes-Dienstendpunkt](media\azure-stack-solution-machine-learning\image123.png)

1.  Navigieren Sie zum Ordner **.kube** unter dem Basisverzeichnis (z.B. C:\\Benutzer\\<user>\\Dokumente\\Kube).

2.  Kopieren Sie den Inhalt der Datei **config**, und fügen Sie ihn in das Fenster für die Kubernetes-Verbindung ein. Wählen Sie die Schaltfläche **OK**.

    ![Kubernetes-Dienstendpunkt](media\azure-stack-solution-machine-learning\image124.png)
    

3.  Nachdem der Kubernetes-Endpunkt erstellt und ausgewählt wurde, können Sie das Kontrollkästchen „Konfigurationsdateien verwenden“ aktivieren, um eine Konfigurationsdatei hinzuzufügen. Navigieren Sie anschließend unter „Verknüpfte Artefakte“ zur Datei „iris_deployment.yaml“.

    ![Alt text](media\azure-stack-solution-machine-learning\image125.png)

    ![Alt text](media\azure-stack-solution-machine-learning\image126.png)

4.  Speichern Sie die Releasedefinition.

#### <a name="check-the-status-of-the-release-definition"></a>Überprüfen Sie den Status der Releasedefinition. 

Nach dem Speichern der Releasedefinition sollte VSTS automatisch ein Release starten.

Überprüfen Sie den Status der Bereitstellung, indem Sie an der WSL-Eingabeaufforderung einen kurzen Befehl ausführen und anschließend die Kubernetes-Benutzeroberfläche prüfen.

```Bash  
kubectl get deployments
```

Die Ausgabe sollte während des Bereitstellungsprozesses etwa wie folgt aussehen:

![Alt text](media\azure-stack-solution-machine-learning\image127.png)

```Bash  
kubectl proxy
```

Wenn die Kubernetes-Benutzeroberfläche ausgeführt wird, können Sie unter [**https://localhost:8001/**](https://localhost:8001/) zur Bereitstellung und dann zu **Workloads > Replikatgruppen** navigieren.

![Alt text](media\azure-stack-solution-machine-learning\image128.png)

### <a name="deploy-the-yaml-service"></a>Bereitstellen des YAML-Diensts

#### <a name="upload-the-irisserviceyaml-to-the-repository-and-sync-changes"></a>Hochladen der Datei „iris_service.yaml“ in das Repository und Synchronisieren der Änderungen

1.  Navigieren Sie zum neu geklonten Repository:

    ```Bash  
    cd /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/
    ```

    ![Alt text](media\azure-stack-solution-machine-learning\image75.png)

1.  Kopieren Sie die Datei **iris_service.yaml** in das Repository.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_service.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_service.yaml
    ```

1.  Committen der Änderung in Git

    ```Bash  
    git add .
    git commit -m “Added Service YAML” 
    git push
    ```

![Alt text](media\azure-stack-solution-machine-learning\image129.png)

#### <a name="update-the-build-definition-for-the-yaml-code"></a>Aktualisieren der Builddefinition für den YAML-Code

1.  Wählen Sie unter dem Hub für Build und Release den Abschnitt „Builds“ und dann die zuvor erstellte Definition aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image130.png)

2.  Wählen Sie die Schaltfläche „Bearbeiten“, um die Definition zu bearbeiten.

    ![Alt text](media\azure-stack-solution-machine-learning\image131.png)

3.  Verwenden Sie **Aufgabe hinzufügen**, um der Phase eine Aufgabe hinzuzufügen. Wählen Sie in der Liste **Hilfsprogramm** die Option **Buildartefakte veröffentlichen** und dann **Hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image108.png)

4.  Verwenden Sie den Namen **Kubectl Apply** (Standardname), und wählen Sie den apply-Befehl aus.



#### <a name="update-the-release-definition-for-the-yaml-code"></a>Aktualisieren der Releasedefinition für den YAML-Code

1.  Wählen Sie unter dem Hub für Build und Release den Abschnitt „Releases“ und dann die zuvor erstellte Releasedefinition aus. Wählen Sie anschließend den Link „Bearbeiten“.

    ![Alt text](media\azure-stack-solution-machine-learning\image132.png)

1.  Wählen Sie **Azure Stack** als Umgebung aus, und fügen Sie Azure Stack dann eine neue Aufgabe hinzu.

    ![Alt text](media\azure-stack-solution-machine-learning\image133.png)

1.  Fügen Sie dieser Phase mit **Neue Aufgabe** eine neue Aufgabe hinzu, wählen Sie unter **Bereitstellen** die Aufgabe **Für Kubernetes bereitstellen** aus, und wählen Sie dann die Option **Hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image134.png)

1.  Verwenden Sie den Namen **Kubectl Apply** (Standardname), und wählen Sie den apply-Befehl aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image109.png)

1.  Legen Sie die Kubernetes-Dienstverbindung auf die zuvor erstellte Azure Stack-Verbindung fest, und aktivieren Sie dann das Kontrollkästchen **Konfigurationsdateien verwenden**, um eine Konfigurationsdatei hinzuzufügen. Navigieren Sie unter „Verknüpfte Artefakte“ zur Datei „iris_service.yaml“.

    ![Alt text](media\azure-stack-solution-machine-learning\image135.png)


    ![Alt text](media\azure-stack-solution-machine-learning\image136.png)

1.  Speichern Sie die Releasedefinition.

#### <a name="check-the-status-of-the-release-definition"></a>Überprüfen des Status der Releasedefinition

Nach dem Speichern der Releasedefinition sollte VSTS automatisch ein Release starten.

Überprüfen Sie den Status der Bereitstellung, indem Sie an der WSL-Eingabeaufforderung einen kurzen Befehl ausführen und anschließend die Kubernetes-Benutzeroberfläche prüfen.

```Bash  
kubectl get deployments
```

Die Ausgabe sollte während des Bereitstellungsprozesses etwa wie folgt aussehen:

![Alt text](media\azure-stack-solution-machine-learning\image127.png)


```Bash  
kubectl proxy
```

Wenn die Kubernetes-Benutzeroberfläche ausgeführt wird, können Sie unter [**https://localhost:8001/**](https://localhost:8001/) zur Bereitstellung und dann zu **Workloads > Replikatgruppen** navigieren.

![Alt text](media\azure-stack-solution-machine-learning\image137.png)


### <a name="kubernetes-scoring-and-validation"></a>Kubernetes-Bewertung und -Überprüfung

Starten der Kubernetes-Benutzeroberfläche

```Bash  
kubectl proxy
```

Navigieren Sie zur Kubernetes-Benutzeroberfläche und dann zu **Bereitstellungen** -> **Iris-Deployment** -> **Neue Replikatgruppe** -> **Iris-Deployment-xxxxxxxxx** („xxxxxxxxx“ steht hier für die Bereitstellungs-ID).

![Alt text](media\azure-stack-solution-machine-learning\image138.png)

Navigieren Sie anschließend zu **Dienste**, und wählen Sie den **externen Endpunkt** des Diensts aus, um seine Funktionsfähigkeit zu überprüfen.

![Alt text](media\azure-stack-solution-machine-learning\image139.png)

Es sollte eine Überprüfungsmeldung angezeigt werden, die in etwa wie folgt aussieht:

![Alt text](media\azure-stack-solution-machine-learning\image140.png)

#### <a name="create-azure-stack-scoring-function-app-in-the-azure-stack-portal"></a>Erstellen der Funktions-App für die Azure Stack-Bewertung im Azure Stack-Portal

Eine Funktions-App ist erforderlich, um die Ausführung der einzelnen Funktionen zu hosten. Eine Funktions-App ermöglicht das Gruppieren von Funktionen als logische Einheit. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen.

1.  Wählen Sie im Azure Stack-Benutzerportal oben links die Schaltfläche **+ Neu** und dann **Web + Mobil** >**Funktions-App**.

    ![Alt text](media\azure-stack-solution-machine-learning\image141.png)

1.  Geben Sie der Funktion den Namen **data-functions**, und fügen Sie sie in derselben Ressourcengruppe mit den verbleibenden Machine Learning-Inhalten an. Lassen Sie vom Tool automatisch einen neuen App Service-Plan für den Verbrauch erstellen, und verwenden Sie das Speicherkonto, das zuvor für den App-Speicher erstellt wurde.

    ![Definieren neuer Funktions-App-Einstellungen](media\azure-stack-solution-machine-learning\image142.png)

1.  Wählen Sie **Erstellen**, um die Funktions-App bereitzustellen.

2.  Wählen Sie oben rechts im Portal das Benachrichtigungssymbol aus, und achten Sie auf die Meldung **Bereitstellung erfolgreich**.

    ![Definieren neuer Funktions-App-Einstellungen](media\azure-stack-solution-machine-learning\image143.png)

1.  Wählen Sie **Zu Ressource wechseln**, um die neue Funktions-App anzuzeigen.

    ![Alt text](media\azure-stack-solution-machine-learning\image144.png)

1.  Erstellen Sie eine neue Funktion, indem Sie **Funktionen** und dann die Schaltfläche **+ Neue Funktion** wählen.

    ![Alt text](media\azure-stack-solution-machine-learning\image145.png)

1.  Auswählen des HTTP-Triggers

    ![Alt text](media\azure-stack-solution-machine-learning\image146.png)

1.  Wählen Sie **C\#** als Sprache aus, und geben Sie der Funktion den Namen **clean-score-data**. Legen Sie die Autorisierungsstufe auf **Anonym** fest.

    ![Alt text](media\azure-stack-solution-machine-learning\image147.png)

1.  Kopieren Sie den Inhalt des Beispielcodes für „clean-score-data“, und fügen Sie ihn in die Funktion ein.

    ![Alt text](media\azure-stack-solution-machine-learning\image148.png)

#### <a name="use-postman-to-validate-functions"></a>Verwenden von Postman zum Überprüfen von Funktionen

Um sicherzustellen, dass Sie Kubernetes und Funktionen richtig eingerichtet haben, können Sie die kostenlose App Postman nutzen, um Schemas und Funktionen zu testen und zu überprüfen. Zum Starten dieses Prozesses müssen Sie zuerst einige Informationen von Ihrer Kubernetes-Instanz sammeln.

1.  Navigieren Sie zur Kubernetes-Benutzeroberfläche und dann zu **Bereitstellungen** -> **Iris-Deployment** -> **Neue Replikatgruppe** -> **Iris-Deployment-xxxxxxxxx** („xxxxxxxxx“ steht hier für die Bereitstellungs-ID).

    ![Alt text](media\azure-stack-solution-machine-learning\image138.png)

1.  Navigieren Sie dann zu **Dienste**, und kopieren Sie den **externen Endpunkt**.

    ![Alt text](media\azure-stack-solution-machine-learning\image149.png)

1.  Sie können die Postman-App bei Bedarf [hier](https://www.getpostman.com/apps) herunterladen und dann installieren.

2.  Melden Sie sich an der Postman-App an, und schließen Sie das Dialogfeld „Neue Datei“.

    ![Alt text](media\azure-stack-solution-machine-learning\image150.png)

1.  Wählen Sie in der Postman-App die Option „POST“.

    ![Alt text](media\azure-stack-solution-machine-learning\image151.png)

1.  Fügen Sie die URL des **externen Endpunkts** unter **Request URL** (Anforderungs-URL) in die Postman-App ein, und fügen Sie am Ende der URL wie unten gezeigt **\\score** hinzu.

    ![Alt text](media\azure-stack-solution-machine-learning\image152.png)

1.  Wählen Sie die Registerkarte **Body** (Text) aus, und geben Sie als Datentyp **raw** (Unformatiert) und dann **JSON** an.

    ![Alt text](media\azure-stack-solution-machine-learning\image153.png)

1.  Navigieren Sie in einem Webbrowser zu **Externer Endpunkt**. Fügen Sie der URL **/swagger.json** Folgendes hinzu. Die Swagger-Datei des Diensts wird dann zum Testen des Setups verwendet.

    ![Alt text](media\azure-stack-solution-machine-learning\image154.png)

1.  Kopieren Sie das Beispiel, das in der Datei **Swagger.JSON** aufgeführt ist.

2.  Fügen Sie in der Postman-App das Beispiel in den Post-Text ein, und wählen Sie **Send** (Senden). Es sollte ein Wert zurückgegeben werden, der dem unten angegebenen Wert ähnelt.

    ![Alt text](media\azure-stack-solution-machine-learning\image155.png)

## <a name="step-7-create-an-azure-stack-storage-account-and-storage-queue"></a>Schritt 7: Erstellen eines Azure Stack-Speicherkontos und einer Speicherwarteschlange

Erstellen eines Azure Stack-Speicherkontos und einer Speicherwarteschlange für Daten

1.  Melden Sie sich am Azure Stack-Benutzerportal an. (Jede Azure Stack-Instanz verfügt über eine eindeutige Portal-URL.)

2.  Erweitern Sie im Azure Stack-Benutzerportal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Alle Dienste**. Scrollen Sie nach unten zu **Speicher**, und wählen Sie **Speicherkonten**. Wählen Sie im Fenster **Speicherkonten** die Option **Hinzufügen**.

3.  Geben Sie einen Namen für das Speicherkonto ein.

4.  Wählen Sie die Replikationsoption für das Speicherkonto aus: **LRS**.

5.  Geben Sie eine neue Ressourcengruppe an, oder wählen Sie eine vorhandene Ressourcengruppe aus.

6.  Wählen Sie als Speicherort für das Speicherkonto die Option **Lokal**.

7.  Wählen Sie **Erstellen**, um das Speicherkonto zu erstellen.

    ![Alt text](media\azure-stack-solution-machine-learning\image156.png)

1.  Wählen Sie das vor Kurzem erstellte Speicherkonto aus.

2.  Wählen Sie **Warteschlangen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image157.png)

1.  Wählen Sie die Option **+ Warteschlange**, geben Sie der Warteschlange einen Namen, und wählen Sie dann **OK**.

    ![Alt text](media\azure-stack-solution-machine-learning\image158.png)

1.  Rufen Sie die **Verbindungszeichenfolge** für die Speicherwarteschlange ab, und kopieren Sie sie.

    ![Alt text](media\azure-stack-solution-machine-learning\image159.png)

1.  Navigieren Sie zur Azure-Funktions-App, und wählen Sie dann die Option **Anwendungseinstellungen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image160.png)

1.  Scrollen Sie in den Anwendungseinstellungen der Funktions-App nach unten zu den Anwendungseinstellungen, und wählen Sie **+ Neue Einstellung hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image161.png)

1.  Geben Sie den Namen des Speicherkontos im Feld **Name** ein, und fügen Sie am Ende „_STORAGE“ hinzu.

Die Anwendung verfügt dann über die Information, dass es sich um den Endpunkt eines Speicherkontos handelt.

1.  Fügen Sie die Verbindungszeichenfolge anschließend in das Feld **Wert** ein.

    ![Alt text](media\azure-stack-solution-machine-learning\image162.png)

1.  Scrollen Sie nach oben an den Anfang der Anwendungseinstellungen, und wählen Sie **Speichern**.

    ![Alt text](media\azure-stack-solution-machine-learning\image163.png)

### <a name="update-the-scoring-function-to-use-storage-queue"></a>Aktualisieren der Bewertungsfunktion für die Verwendung der Speicherwarteschlange

1.  Wählen Sie **Integrieren** für die Funktion aus, und deaktivieren Sie die Option **GET**.

2.  Wählen Sie **Speichern**aus.

3.  Wählen Sie anschließend in den Ausgaben die Option **+ Neue Ausgabe**.

    ![Alt text](media\azure-stack-solution-machine-learning\image164.png)

1.  Wählen Sie dann **Azure Queue Storage** und **Auswählen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image165.png)

1.  Aktualisieren Sie den **Warteschlangennamen** auf die zuvor erstellte Speicherwarteschlange, und legen Sie dann die **Speicherkontoverbindung** auf die zuvor erstellte Speicherkontoverbindung fest. Wählen Sie anschließend **Speichern**.

    ![Alt text](media\azure-stack-solution-machine-learning\image166.png)

## <a name="step-8-create-a-function-to-handle-clean-data"></a>Schritt 8: Erstellen einer Funktion zum Verarbeiten von bereinigten Daten

Erstellen Sie eine neue Azure Stack-Funktion, um die bereinigten Daten aus Azure Stack nach Azure zu verschieben.

1.  Erstellen Sie eine neue Funktion, indem Sie **Funktionen** und dann die Schaltfläche **+ Neue Funktion** wählen.

    ![Alt text](media\azure-stack-solution-machine-learning\image167.png)

1.  Wählen Sie **Zeitgebertrigger**.

    ![Alt text](media\azure-stack-solution-machine-learning\image168.png)

1.  Wählen Sie **C\#** als Sprache aus, und geben Sie der Funktion den Namen **upload-to-azure**. Legen Sie den Zeitplan auf **0 0 \*/1 \* \* \*** fest (in CRON-Notation einmal pro Stunde).

    ![Alt text](media\azure-stack-solution-machine-learning\image169.png)

### <a name="get-the-connection-string-to-the-azure-hosted-storage-account"></a>Abrufen der Verbindungszeichenfolge für das gehostete Azure-Speicherkonto

1.  Navigieren Sie zu <https://portal.azure.com>, und wählen Sie dann das zuvor erstellte **Azure-Speicherkonto** aus.

2.  Wählen Sie **Zugriffsschlüssel**, und kopieren Sie dann die **Verbindungszeichenfolge** für das Speicherkonto.

    ![Alt text](media\azure-stack-solution-machine-learning\image170.png)

### <a name="update-the-upload-to-azure-function-to-use-the-azure-hosted-storage"></a>Aktualisieren der Funktion „upload-to-azure“ für die Verwendung des gehosteten Azure-Speichers

1.  Navigieren Sie zur Azure-Funktions-App, und wählen Sie dann die Option **Anwendungseinstellungen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image171.png)

1.  Scrollen Sie in den Anwendungseinstellungen der Funktions-App nach unten zu den Anwendungseinstellungen, und wählen Sie **+ Neue Einstellung hinzufügen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image172.png)

1.  Geben Sie den Namen des Speicherkontos im Feld **Name** ein, und fügen Sie am Ende „_STORAGE“ hinzu.

Die Anwendung verfügt dann über die Information, dass es sich um den Endpunkt eines Speicherkontos handelt.

1.  Fügen Sie die Verbindungszeichenfolge des gehosteten Azure-Speicherkontos dann in das Feld **Wert** ein.

    ![Alt text](media\azure-stack-solution-machine-learning\image173.png)

1.  Scrollen Sie nach oben an den Anfang der Anwendungseinstellungen, und wählen Sie **Speichern**.

    ![Alt text](media\azure-stack-solution-machine-learning\image174.png)

1.  Navigieren Sie zurück zur Funktion **upload-to-azure**.

2.  Aktivieren Sie die Funktion **Integrieren**.

3.  Wählen Sie anschließend in den Ausgaben die Option **+ Neue Ausgabe**.

    ![Alt text](media\azure-stack-solution-machine-learning\image175.png)

1.  Wählen Sie dann **Azure Blob Storage** und **Auswählen**.

    ![Alt text](media\azure-stack-solution-machine-learning\image176.png)

1.  Aktualisieren Sie den **Pfad** zum zuvor erstellten Speichercontainer im folgenden Format: **uploadeddata/{rand-guid}.txt**. Legen Sie anschließend die **Speicherkontoverbindung** auf die zuvor erstellte Speicherkontoverbindung mit Azure fest, und wählen Sie **Speichern**.

    ![Alt text](media\azure-stack-solution-machine-learning\image177.png)

1.  Kopieren Sie den Inhalt des Beispielcodes für **upload-to-azure**, und fügen Sie ihn in die Funktion ein.

2.  Nehmen Sie je nach Bedarf Änderungen vor, um auf die Speicherkonto-Verbindungszeichenfolge zu verweisen.

3.  Speichern Sie, und führen Sie den Code aus.

    ![Alt text](media\azure-stack-solution-machine-learning\image178.png)

1.  Überprüfen Sie das gehostete Azure-Speicherkonto, um anzuzeigen, ob die Daten aus Azure für die Cloud analysiert wurden. Wenn der Vorgang erfolgreich war, wird in etwa die folgende Ausgabe angezeigt:

    ![Alt text](media\azure-stack-solution-machine-learning\image179.png)

Die Daten wurden mit in Azure Stack gehostetem Kubernetes Machine Learning in Bezug auf sensible Daten bereinigt und aus der lokalen Azure Stack-Instanz über in Azure Stack gehostete Funktions-Apps in die öffentliche Azure-Cloud hochgeladen. Die Daten können nun für Uploads in einem Edgeszenario bzw. Szenario ohne Verbindung bereitgestellt werden.

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zu Azure-Cloudmustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns).