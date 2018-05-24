---
title: Bereitstellen Ihrer App in Azure und Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie mehr zum Bereitstellen von Apps in Azure und Azure Stack mit einer hybriden CI/CD-Pipeline.
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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 2a51169fa74384778d3351d48055d05e387d0ea6
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Tutorial: Bereitstellen von Apps in Azure und Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Eine hybride Pipeline für Continuous Integration/Continuous Delivery (CI/CD) ermöglicht das Erstellen, Testen und Bereitstellen Ihrer App in mehreren Clouds.  In diesem Tutorial erstellen Sie eine Beispielumgebung, die Folgendes ermöglicht:
 
> [!div class="checklist"]
> * Initiieren eines neuen Builds auf der Grundlage von Codecommits in Ihrem VSTS-Repository (Visual Studio Team Services)
> * Automatisches Bereitstellen Ihres neu erstellten Codes in der globalen Azure-Instanz für Benutzerakzeptanztests
> * Automatisches Bereitstellen Ihres Codes in Azure Stack nach Bestehen der Tests

### <a name="about-the-hybrid-delivery-build-pipe"></a>Informationen zur Hybrid Delivery-Buildpipeline

Bei der Anwendungsbereitstellung sind Kontinuität, Sicherheit und Zuverlässigkeit für Ihre Organisation und Ihr Entwicklungsteam unverzichtbar. Mit einer hybriden CI/CD-Pipeline können Sie Ihre Pipelines für Ihre lokale Umgebung und für die öffentliche Cloud konsolidieren. Sie können den Ort wechseln, ohne die Anwendung zu wechseln.

Dies ermöglicht die Verwendung einheitlicher Entwicklungstools. Die Verwendung einheitlicher Tools für die öffentliche Azure-Cloud und Ihre lokale Azure Stack-Umgebung macht die Implementierung der CI/CD-Entwicklungspraxis deutlich einfacher. In Azure oder Azure Stack bereitgestellte Apps und Dienste sind austauschbar, und an beiden Orten kann der gleiche Code ausgeführt werden, wobei Features und Funktionen der lokalen Umgebung und der öffentlichen Cloud genutzt werden können.

Weitere Informationen:
 - [What is Continuous Integration?](https://www.visualstudio.com/learn/what-is-continuous-integration/) (Was ist Continuous Integration?)
 - [What is Continuous Delivery?](https://www.visualstudio.com/learn/what-is-continuous-delivery/) (Was ist Continuous Delivery?)


## <a name="prerequisites"></a>Voraussetzungen

Für die Erstellung einer hybriden CI/CD-Pipeline müssen einige Komponenten vorbereitet werden. Dies kann etwas dauern.
 
 - Ein Azure-OEM/-Hardwarepartner kann eine Azure Stack-Produktionsumgebung bereitstellen. Ein Azure Stack Development Kit (ASDK) kann von allen Benutzern bereitgestellt werden. 
 - Ein Azure Stack-Bediener muss auch App Service bereitstellen, Pläne und Angebote sowie ein Mandantenabonnement erstellen und das Windows Server 2016-Image hinzufügen.

Wenn Sie bereits über einige dieser Komponenten verfügen, stellen Sie vor Beginn sicher, dass diese die Anforderungen erfüllen.

In diesem Thema wird davon ausgegangen, dass Sie bereits über Kenntnisse zu Azure und Azure Stack verfügen. Wenn Sie mehr darüber erfahren möchten, bevor Sie fortfahren, schauen Sie sich diese Themen an:


In diesem Tutorial wird außerdem davon ausgegangen, dass Sie bereits grob mit Azure und Azure Stack vertraut sind. 

Weitere Informationen finden Sie bei Bedarf in den folgenden Themen:
 - [Einführung in Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack Key Concepts (Wichtige Begriffe für Azure Stack)](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

 - Erstellen Sie eine [Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) in Azure. Notieren Sie sich die neue URL der Web-App, da Sie diese später benötigen.

Azure Stack
 - Verwenden Sie ein integriertes Azure Stack-System, oder stellen Sie das hier verlinkte Azure Stack Development Kit (ASDK) bereit:
    - Eine ausführliche Anleitung zum Bereitstellen des ASDKs finden Sie im Tutorial [Bereitstellen des ASDKs mithilfe des Installationsprogramms](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).
    - Mit dem PowerShell-Skript [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) lassen sich zahlreiche Schritte nach der ASDK-Bereitstellung automatisieren.

    > [!Note]  
    > Der Installationsvorgang dauert sieben Stunden. Berücksichtigen Sie dies bei Ihrer Planung.

 - Stellen Sie PaaS-Dienste als [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) für Azure Stack bereit. 
 - Erstellen Sie [Pläne/Angebote](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in Ihrer Azure Stack-Umgebung. 
 - Erstellen Sie ein [Mandantenabonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) in Ihrer Azure Stack-Umgebung. 
 - Erstellen Sie eine Web-App in Ihrem Mandantenabonnement. Notieren Sie sich die URL der neuen Web-App zur späteren Verwendung.
 - Stellen Sie einen virtuellen VSTS-Computer bereit (ebenfalls in Ihrem Mandantenabonnement).
 - Es wird ein virtueller Windows Server 2016-Computer mit .NET 3.5 benötigt. Dieser virtuelle Computer wird in Ihrer Azure Stack-Instanz als privater Build-Agent erstellt. 

### <a name="developer-tools"></a>Entwicklertools

 - Erstellen Sie einen [VSTS-Arbeitsbereich](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). Bei der Anmeldung wird ein Projekt namens **MyFirstProject** erstellt.
 - [Installieren Sie Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio), und [melden Sie sich bei VSTS an](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
 - Stellen Sie eine Verbindung zum Projekt her, und [klonen Sie dieses lokal](https://www.visualstudio.com/docs/git/gitquickstart).
 
 > [!Note]  
 > Für die Ausführung benötigen Sie Azure Stack mit den passenden syndizierten Images (Windows Server und SQL), und App Service muss bereitgestellt werden.
 
## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Vorbereiten des privaten Build- und Release-Agents für die Visual Studio Team Services-Integration

### <a name="prerequisites"></a>Voraussetzungen

Visual Studio Team Services (VSTS) führt die Authentifizierung bei Azure Resource Manager unter Verwendung eines Dienstprinzipals durch. VSTS muss den Status „Mitwirkender“ besitzen, um Ressourcen in einem Azure Stack-Abonnement bereitstellen zu können.

Im Anschluss finden Sie die allgemeinen Konfigurationsschritte, die ausgeführt werden müssen, um diese Authentifizierung zu ermöglichen:

1. Es muss ein Dienstprinzipal erstellt oder ein bereits vorhandener Dienstprinzipal verwendet werden.
2. Für den Dienstprinzipal müssen Authentifizierungsschlüssel erstellt werden.
3. Das Azure Stack-Abonnement muss über die rollenbasierte Zugriffssteuerung überprüft werden, damit der SPN Teil der Rolle „Mitwirkender“ werden kann.
4. In VSTS muss unter Verwendung der Azure Stack-Endpunkte und der SPN-Informationen eine neue Dienstdefinition erstellt werden.

### <a name="service-principal-creation"></a>Dienstprinzipalerstellung

Gehen Sie gemäß der [Anleitung zum Erstellen eines Dienstprinzipals](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) vor, und wählen Sie als Anwendungstyp die Option „Web-App/API“ aus.

### <a name="access-key-creation"></a>Zugriffsschlüsselerstellung

Ein Dienstprinzipal benötigt einen Schlüssel für die Authentifizierung. Führen Sie die Schritte in diesem Abschnitt aus, um einen Schlüssel zu generieren.


1. Wählen Sie in Azure Active Directory unter **App-Registrierungen** Ihre Anwendung aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2.  Notieren Sie den Wert von **Anwendungs-ID**. Dieser Wert wird beim Konfigurieren des Dienstendpunkts in VSTS benötigt.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Klicken Sie zum Generieren eines Authentifizierungsschlüssels auf **Einstellungen**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Wählen Sie zum Generieren eines Authentifizierungsschlüssels die Option **Schlüssel** aus.
 
    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.
 
    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Nach dem Speichern des Schlüssels wird der Wert des Schlüssels angezeigt. Kopieren Sie diesen Wert jetzt, da Sie ihn später nicht mehr abrufen können. Sie geben den **Schlüsselwert** zusammen mit der Anwendungs-ID ein, um die Anmeldung als Anwendung durchzuführen. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-tenant-id"></a>Abrufen der Mandanten-ID

Im Rahmen der Dienstendpunkt-Konfiguration muss in VSTS die **Mandanten-ID** angegeben werden, die dem AAD-Verzeichnis entspricht, in dem Ihr Azure Stack-Stempel bereitgestellt wurde. Führen Sie die Schritte in diesem Abschnitt aus, um die Mandanten-ID zu ermitteln.

1. Wählen Sie **Azure Active Directory**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Wählen Sie zum Abrufen der Mandanten-ID die Option **Eigenschaften** für Ihren Azure AD-Mandanten aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_08.png)
 
3. Kopieren Sie die **-Verzeichnis-ID**. Dieser Wert ist Ihre Mandanten-ID.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Gewähren von Rechten für den Dienstprinzipal zum Bereitstellen von Ressourcen im Azure Stack-Abonnement 

Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Navigieren Sie zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Um z.B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Abonnements** aus. Sie können stattdessen auch eine Ressourcengruppe oder Ressource auswählen.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. Wählen Sie das **Abonnement** (Ressourcengruppe oder Ressource) aus, dem die Anwendung zugewiesen werden soll.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Wählen Sie **Access Control (IAM)** aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Wählen Sie **Hinzufügen**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten. In der folgenden Abbildung ist die Rolle **Besitzer** dargestellt.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Azure Active Directory-Anwendungen werden standardmäßig nicht in den verfügbaren Optionen angezeigt. Für die Suche nach Ihrer Anwendung müssen Sie **den Namen in das Suchfeld eingeben**. Wählen Sie sie aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Wählen Sie **Speichern** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Kontext zugewiesen sind.

### <a name="role-based-access-control"></a>Rollenbasierte Access Control

Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure ermöglicht eine präzise Zugriffsverwaltung für Azure. Mit RBAC können Sie den Benutzern nur die Zugriffsrechte gewähren, die diese zum Ausführen ihrer Aufgaben benötigen. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS-Agent-Pools

Agents werden in Agent-Pools zusammengefasst, um sie nicht alle einzeln verwalten zu müssen. Ein Agent-Pool definiert die Freigabegrenze für alle Agents in diesem Pool. In VSTS gelten Agent-Pools für das VSTS-Konto. Ein Agent-Pool kann also teamprojektübergreifend gemeinsam genutzt werden. Weitere Informationen sowie ein Tutorial zum Erstellen von VSTS-Agent-Pools finden Sie unter [Agent pools and queues](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts) (Agent-Pools und -Warteschlangen).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Hinzufügen eines persönlichen Zugriffstokens (Personal Access Token, PAT) für Azure Stack

1. Melden Sie sich bei Ihrem VSTS-Konto an, und wählen Sie den Namen Ihres Kontoprofils aus.
2. Klicken Sie auf **Sicherheit verwalten**, um die Seite für die Tokenerstellung zu öffnen.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Kopieren Sie das Token.
    
    > [!Note]  
    > Rufen Sie die Tokeninformationen ab. Sie werden nach Verlassen dieses Bildschirms nicht erneut angezeigt. 
    
    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_19.png)
    

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Installieren des VSTS-Build-Agents auf dem von Azure Stack gehosteten Buildserver

1.  Stellen Sie eine Verbindung mit dem Buildserver her, den Sie auf dem Azure Stack-Host bereitgestellt haben.

2.  Laden Sie den Build-Agent herunter, stellen Sie ihn als Dienst unter Verwendung Ihres persönlichen Zugriffstokens (Personal Access Token, PAT) bereit, und führen Sie ihn als VM-Administratorkonto aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Wechseln Sie zum Ordner mit dem extrahierten Build-Agent. Führen Sie an einer Eingabeaufforderung mit erhöhten Rechten die Datei **run.cmd** aus. 

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4.  Nach Abschluss der Ausführung von „run.cmd“ sollte der Ordner mit den extrahierten Inhalten wie folgt aussehen:

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    Der Agent wird nun im Ordner „VSTS“ angezeigt.

## <a name="endpoint-creation-permissions"></a>Berechtigungen für die Endpunkterstellung

Benutzer können Endpunkte erstellen und so VSTO-Builds die Bereitstellung von Azure Service-Apps im Stapel ermöglichen. VSTS stellt eine Verbindung mit dem Build-Agent her, der wiederum eine Verbindung mit Azure Stack herstellt. 

![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Klicken Sie im Menü **Einstellungen** auf **Sicherheit**.
2. Wählen Sie links in der Liste **VSTS-Gruppen** die Option **Endpunktersteller** aus. 

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

3. Klicken Sie auf der Registerkarte **Mitglieder** auf **+Hinzufügen**. 

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

4. Geben Sie einen Benutzernamen ein, und wählen Sie in der Liste den entsprechenden Benutzer aus.
5. Klicken Sie auf **Änderungen speichern**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

6. Wählen Sie links in der Liste **VSTS-Gruppen** die Option **Endpunktadministratoren** aus.
7. Klicken Sie auf der Registerkarte **Mitglieder** auf **+Hinzufügen**.
8. Geben Sie einen Benutzernamen ein, und wählen Sie in der Liste den entsprechenden Benutzer aus.
9. Klicken Sie auf **Änderungen speichern**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

    Der Build-Agent in Azure Stack erhält Anweisungen von VSTS. Daraufhin werden Endpunktinformationen für die Kommunikation mit der Azure Stack-Instanz übermittelt. 
    
    Die Verbindung zwischen VSTS und Azure Stack ist nun bereit.

## <a name="develop-your-application"></a>Entwickeln Ihrer Anwendung

Richten Sie die hybride CI/CD-Pipeline ein, um die Web-App in Azure und Azure Stack bereitzustellen und Änderungen automatisch an beide Clouds zu pushen.

> [!Note]  
> Für die Ausführung benötigen Sie Azure Stack mit den passenden syndizierten Images (Windows Server und SQL), und App Service muss bereitgestellt werden. Anforderungen für Azure Stack-Bediener finden Sie im Abschnitt „Voraussetzungen“ der App Service-Dokumentation.

### <a name="add-code-to-vsts-project"></a>Hinzufügen von Code zum VSTS-Projekt

1. Melden Sie sich bei Visual Studio mit einem Konto an, das über Berechtigungen zum Erstellen von Projekten in Azure Stack verfügt.

    Der hybride CI/CD-Ansatz kann für Anwendungscode und Infrastrukturcode verwendet werden. Verwenden Sie [Azure Resource Manager-Vorlagen](https://azure.microsoft.com/resources/templates/) wie etwa Web-App-Code aus VSTS für beide Clouds.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Klonen Sie das Repository**, indem Sie die Standard-Web-App erstellen und öffnen.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Erstellen einer eigenständigen Web-App-Bereitstellung für App Services in beiden Clouds

1. Bearbeiten Sie die Datei **WebApplication.csproj**: Wählen Sie **Runtimeidentifier** aus, und fügen Sie Folgendes hinzu: `win10-x64.`. Weitere Informationen finden Sie in der Dokumentation [Eigenständige Bereitstellungen (Self-contained deployments, SCD)](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd).

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Checken Sie den Code über den Team Explorer in VSTS ein.

3. Vergewissern Sie sich, dass der Anwendungscode in Visual Studio Team Services eingecheckt wurde. 

### <a name="create-the-build-definition"></a>Erstellen der Builddefinition

1. Melden Sie sich bei VSTS an, um sich zu vergewissern, dass Builddefinitionen erstellt werden können.

2. Fügen Sie den Code **-r win10-x64** hinzu. Dies ist erforderlich, um eine eigenständige Bereitstellung mit .NET Core auszulösen. 

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

3. Führen Sie den Buildvorgang aus. Der Buildvorgang für die [eigenständige Bereitstellung](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) veröffentlicht Artefakte, die in Azure und Azure Stack ausgeführt werden können.

### <a name="using-an-azure-hosted-agent"></a>Verwenden eines gehosteten Azure-Agents

Mit einem gehosteten Agent lassen sich in VSTS komfortabel Web-Apps erstellen und bereitstellen. Wartungsarbeiten und Upgrades werden automatisch von Microsoft Azure durchgeführt, was kontinuierliche und ungestörte Entwicklungs-, Test- und Bereitstellungsaktivitäten ermöglicht.

### <a name="manage-and-configure-the-continuous-deployment-cd-process"></a>Verwalten und Konfigurieren des CD-Prozesses (Continuous Deployment)

Visual Studio Team Services (VSTS) und Team Foundation Server (TFS) bieten eine äußerst flexibel konfigurier- und verwaltbare Pipeline für Releases in mehreren Umgebungen (etwa in Entwicklungs-, Staging-, Qualitätssicherungs- und Produktionsumgebungen) – einschließlich erforderlicher Genehmigungen in bestimmten Phasen.

### <a name="create-release-definition"></a>Erstellen einer Releasedefinition

![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

1. Klicken Sie auf der Registerkarte **Releases** der VSO-Seite „Build und Release“ auf **\[+]**, um ein neues Release hinzuzufügen.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\102.png)

2. Wenden Sie die Vorlage **Azure App Service-Bereitstellung** an.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\103.png)

3. Fügen Sie im Pulldownmenü **Artefakt hinzufügen** das Artefakt für die Azure Cloud-Build-App hinzu.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\104.png)

4. Klicken Sie auf der Registerkarte „Pipeline“ auf den Link **Phase,** **Aufgabe** der Umgebung, und legen Sie die Azure Cloud-Umgebungswerte fest.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\105.png)

5. Legen Sie unter **Umgebungsname** den Umgebungsnamen fest, und wählen Sie das **Azure-Abonnement** für den Azure Cloud-Endpunkt aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\106.png)

6. Legen Sie unter **Umgebungsname** den erforderlichen Namen des Azure-App-Diensts fest.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\107.png)

7. Geben Sie unter „Agent-Warteschlange“ für die gehostete Azure Cloud-Umgebung die Zeichenfolge **Hosted VS2017** ein.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\108.png)

8. Wählen Sie im Menü „Deploy Azure App Service“ (Azure App Service bereitstellen) **das gültige Paket oder den gültigen Ordner** für die Umgebung aus. Klicken Sie für den **Ordnerspeicherort** auf **OK**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\109.png)

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\110.png)

9. Speichern Sie alle Änderungen, und kehren Sie zur **Releasepipeline** zurück.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\111.png)

10. Fügen Sie ein **neues Artefakt** hinzu, und wählen Sie dabei den Build für die Azure Stack-App aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\112.png)

11. Fügen Sie mindestens eine Umgebung hinzu, und wenden Sie dabei die **Azure App Service-Bereitstellung** an.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\113.png)

12. Nennen Sie die neue Umgebung **Azure Stack**.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\114.png)

13. Suchen Sie auf der Registerkarte **Aufgabe** nach der Umgebung „Azure Stack“.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\115.png)

14. Wählen Sie das **Abonnement** für den Azure Stack-Endpunkt aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\116.png)

15. Legen Sie den Namen der Azure Stack-Web-App als **Name des App-Diensts** fest.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\117.png)

16. Wählen Sie den **Azure Stack-Agent** aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\118.png)

17. Wählen Sie im Abschnitt „Deploy Azure App Service“ (Azure App Service bereitstellen) **das gültige Paket oder den gültigen Ordner** für die Umgebung aus. Klicken Sie für den **Ordnerspeicherort** auf „OK“.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\119.png)

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\120.png)

18. Fügen Sie auf der Registerkarte „Variable“ eine Variable namens **VSTS_ARM_REST_IGNORE_SSL_ERRORS** hinzu, und legen Sie ihren Wert auf **true** und ihren Gültigkeitsbereich auf **Azure Stack** fest.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\121.png)

19. Klicken Sie bei beiden Artefakten auf das Symbol **Continuous Deployment-Trigger**, und aktivieren Sie den Trigger.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\122.png)

20. Klicken Sie für die Azure Stack-Umgebung auf das Symbol **Bedingungen vor der Bereitstellung**, und legen Sie den Trigger auf **Nach einem Release** fest.

21. Speichern Sie alle Änderungen.

> [!Note]  
> Bei der vorlagenbasierten Erstellung einer Releasedefinition wurden einige Einstellungen für die Aufgaben möglicherweise automatisch als [Umgebungsvariablen](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) definiert. Diese Einstellungen können in den Aufgabeneinstellungen nicht geändert werden. Zum Bearbeiten dieser Einstellungen müssen Sie das übergeordnete Umgebungselement auswählen.

## <a name="create-a-release"></a>Erstellen eines Release

Nachdem Sie die Releasedefinition geändert haben, können Sie die Bereitstellung starten. Zu diesem Zweck erstellen Sie ein auf der Releasedefinition basierendes Release. Ein Release kann automatisch erstellt werden – beispielsweise, wenn in der Releasedefinition der Continuous Deployment-Trigger festgelegt ist. Das bedeutet, dass nach Änderungen am Quellcode ein neuer Buildvorgang gestartet und auf dessen Grundlage ein neues Release erstellt wird. In diesem Abschnitt wird allerdings manuell ein neues Release erstellt.

1. Öffnen Sie die Dropdownliste **Release**, und wählen Sie **Release erstellen** aus.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\200.png)
 
2. Geben Sie eine Beschreibung für das Release ein, vergewissern Sie sich, dass die korrekten Artefakte ausgewählt sind, und klicken Sie anschließend auf **Erstellen**. Kurz darauf erscheint ein Banner mit dem Hinweis, dass das neue Release erstellt wurde. Klicken Sie auf den Link (Name des Release).

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\201.png)
 
3. Die Zusammenfassungsseite des Release mit den Details zum Release wird geöffnet. Im Abschnitt **Umgebungen** ändert sich der Bereitstellungsstatus für die QA-Umgebung von „IN BEARBEITUNG“ in „ERFOLGREICH“. Daraufhin erscheint ein Banner mit dem Hinweis, dass das Release auf Genehmigung wartet. Wenn eine Bereitstellung in einer Umgebung noch aussteht oder nicht erfolgreich war, wird ein blaues Informationssymbol (i) angezeigt. Zeigen Sie auf dieses Symbol, um ein Popupelement mit dem Grund anzuzeigen.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\202.png)

In anderen Ansichten (etwa in der Liste mit den Releases) wird auch ein Symbol angezeigt, das darauf hinweist, dass die Genehmigung aussteht. Wenn Sie auf das Symbol zeigen, wird ein Popupelement mit dem Umgebungsnamen und weiteren Details angezeigt. So kann ein Administrator ganz einfach erkennen, bei welchen Releases eine Genehmigung aussteht, und sich über den Gesamtstatus aller Releases informieren.

### <a name="monitor-and-track-deployments"></a>Überwachen und Nachverfolgen von Bereitstellungen

In diesem Abschnitt erfahren Sie, wie Sie über das Release, das Sie im vorherigen Abschnitt erstellt haben, Bereitstellungen (im vorliegenden Beispiel: für zwei Azure App Services-Websites) überwachen und nachverfolgen.

1. Klicken Sie auf der Zusammenfassungsseite des Release auf den Link **Protokolle**. Während des Bereitstellungsvorgangs werden auf dieser Seite das Liveprotokoll des Agents und im linken Bereich der Status der einzelnen Vorgänge des Bereitstellungsprozesses für die jeweilige Umgebung angezeigt.

    Klicken Sie für eine Genehmigung vor oder nach der Bereitstellung auf das Symbol in der Spalte **Aktion**, um Informationen zu dem Benutzer, der die Bereitstellung genehmigt (oder abgelehnt) hat, sowie die vom Benutzer angegebene Nachricht anzuzeigen.

2. Nach Abschluss der Bereitstellung wird im rechten Bereich die gesamte Protokolldatei angezeigt. Klicken Sie im linken Bereich auf einen der **Prozessschritte**, um nur den Protokolldateiinhalt für diesen Schritt anzuzeigen. So lassen sich einzelne Teile der Gesamtbereitstellung leichter nachverfolgen und debuggen. Mithilfe der Symbole und Links auf der Seite können Sie alternativ die einzelnen Protokolldateien (oder eine ZIP-Datei mit allen Protokolldateien) herunterladen.

    ![Alternativer Text](media\azure-stack-solution-hybrid-pipeline\203.png)
 
3. Öffnen Sie die Registerkarte **Zusammenfassung**, um die gesamten Releasedetails anzuzeigen. Hier werden die Details des Builds sowie die Umgebungen angezeigt, in denen er bereitgestellt wurde. Außerdem finden Sie hier den Bereitstellungsstatus und andere Informationen zum Release.

4. Klicken Sie auf die einzelnen **Umgebungslinks**, um weitere Details zu vorhandenen und noch ausstehenden Bereitstellungen für diese spezifische Umgebung anzuzeigen. Mithilfe dieser Seiten können Sie sich vergewissern, dass der gleiche Build in beiden Umgebungen bereitgestellt wurde.

5. Öffnen Sie die **bereitgestellte Produktions-App** in Ihrem Browser. Verwenden Sie für eine Azure App Services-Website beispielsweise die URL `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Cloudmustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns).
