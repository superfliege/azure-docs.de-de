---
title: Neuerstellen einer lokalen Contoso-App in Azure | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Contoso eine App in Azure mithilfe von Azure App Service, Kubernetes Services, Cosmos DB, Azure Functions und Cognitive Services neu erstellt.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 3d835a7bd93426e57c5ab204d277faca22ae0638
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "42145591"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Contoso-Migration: Neuerstellen einer lokalen App in Azure

In diesem Artikel wird erläutert, wie Contoso die App SmartHotel migriert und in Azure neu erstellt. Es migriert die Front-End-VM der App zu Azure App Service-Web-Apps. Das Back-End der App wird mithilfe von Microservices erstellt, die für Container bereitgestellt werden, die von Azure Kubernetes Service (AKS) verwaltet werden. Die Website interagiert mit Azure Functions für die Bereitstellung von Funktionalität für Fotos von Haustieren. 

Dieses Dokument stammt aus einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten und verschiedene Migrationstypen durchführen. Die Szenarios werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Migrationsartikel wird die gleiche Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung einer lokalen, zweischichtigen App SmartHotel auf einer VMware ausführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und einer verwalteten SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso eine Migration zu Azure für die App SmartHotel per Lift & Shift ausführt. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und die App-Datenbank mithilfe des [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten SQL-Instanz. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel nur mit SiteRecovery migriert. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App zu Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Contoso verwendet Site Recovery, um die App-VMs zu migrieren, und den Database Migration Service, um die App-Datenbank zu einem SQL Servercluster zu migrieren, das durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso mithilfe von Site Recovery eine Migration der Linux-App osTicket per Lift & Shift zu Azure-VMs durchführt | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs und Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery zu Azure-VMs und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
[Artikel 9: Umgestalten einer App zu einer Azure-Web-App und einer Azure SQL-Datenbank-Instanz](contoso-migration-refactor-web-app-sql.md) | Zeigt, wie Contoso die SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz migriert. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App zu einer Azure-Web-App und einer Azure Database for MySQL-Instanz](contoso-migration-refactor-linux-app-service-mysql.md) | Zeigt, wie Contoso die Linux-osTicket-App zu Azure-Web-Apps auf mehreren Websites migriert, die in GitHub für Continuous Delivery integriert sind. Es migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von TFS in VSTS](contoso-migration-tfs-vsts.md) | Zeigt, wie Contoso seine lokale Team Foundation Server-Bereitstellung (TFS) durch Migration zu Visual Studio Team Services (VSTS) in Azure migriert. | Verfügbar
[Artikel 12: Umstrukturieren einer Contoso-App zu einem Azure-Container und einer SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und in Azure umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
Artikel 13: Neuerstellen einer App in Azure | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Service, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Dieser Artikel

In diesem Artikel migriert Contoso die zweischichtige Windows-App. Die NET-App SmartHotel wird auf VMware-VMs in Azure ausgeführt. Falls Sie diese App verwenden möchten: Sie wird als Open Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was die Unternehmen mit dieser Migration erreichen möchten:

- **Steigerung des Unternehmenswachstums**: Contoso wächst. Es möchte differenzierte Benutzererfahrungen für seine Kunden auf seinen Websites bereitstellen.
- **Flexibilität**: Contoso muss schneller reagieren als die Änderungen im Marketplace geschehen, um den Erfolg in einer globalen Wirtschaft zu garantieren. 
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die Contoso-IT-Systeme bereitstellen, die mit der gleichen Geschwindigkeit wachsen können.
- **Kosten**: Contoso möchte die Lizenzierungskosten minimieren.

## <a name="migration-goals"></a>Migrationsziele

Das Cloudteam von Contoso hat folgende App-Anforderungen für diese Migration gestellt. Anhand dieser Anforderungen wird die beste Migrationsmethode bestimmt:
 - Die App wird auch in Zukunft eine wichtige Rolle in Azure spielen, wie es heute schon der Fall ist. Sie sollte reibungslos laufen und sich mühelos skalieren lassen.
 - Die App darf nicht auf IaaS-Komponenten basieren. Sie sollte für die Verwendung von PaaS- oder serverlosen Diensten ausgelegt sein.
 - Die App-Builds sollten in Clouddiensten ausgeführt werden und Container sollten sich in einer privaten unternehmensweiten Containerregistrierung in der Cloud befinden.
 - Die API-Dienst für Fotos von Haustieren sollte in der Praxis präzise und zuverlässig sein, da die von der App getroffenen Entscheidungen in den Hotels berücksichtigt werden müssen. Haustiere, denen der Zutritt erlaubt ist, dürfen in den Hotels bleiben.

## <a name="solution-design"></a>Lösungsentwurf

Nachdem die Ziele und Anforderungen formuliert wurden, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess, einschließlich der für die Migration genutzten Azure-Dienste.

### <a name="current-app"></a>Aktuelle App

- Die lokale SmartHotel-App ist auf zwei VMs aufgeteilt (WEBVM und SQLVM).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5)
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.


### <a name="proposed-architecture"></a>Vorgeschlagene Architektur

- Das Front-End der App wird als Azure App Service-Web-App in ihrer primären Region bereitgestellt.
- Eine Azure-Funktion bietet Uploads von Fotos von Haustieren, und die Website interagiert mit dieser Funktionalität.
- Die Haustierfotofunktion greift auf die Maschinelles Sehen-API von Cognitive Services und Cosmos DB zurück.
- Das Back-End der Website wird mithilfe von Microservices erstellt. Diese werden in Containern bereitgestellt, die in Azure Kubernetes Service (AKS) verwaltet werden.
- Container werden mithilfe von VSTS erstellt und mithilfe von Push an Azure Container Registry (ACR) übertragen.
- Vorerst stellt Contoso die Web-App und den Funktionscode mithilfe von Visual Studio manuell bereit.
- Microservices werden mithilfe eines PowerShell-Skripts bereitgestellt, das Kubernetes-Befehlszeilentools aufruft.

    ![Szenarioarchitektur](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Überprüfung der Lösung
Contoso wertet den vorgeschlagen Entwurf durch Erstellen einer Liste mit Vor- und Nachteilen aus.

**Aspekt** | **Details**
--- | ---
**Vorteile** | Durch den Einsatz von PaaS- und serverlosen Lösungen für die End-to-End-Bereitstellung verkürzt sich die Verwaltungszeit, die Contoso aufwenden muss, um ein Vielfaches.<br/><br/> Die Migration auf eine Microservicearchitektur bietet Contoso die Möglichkeit, seine Lösung ganz einfach im Laufe der Zeit zu erweitern.<br/><br/> Neue Funktionalität kann online geschaltet werden, ohne dass vorhandene Codebasen von Lösungen beeinträchtigt werden.<br/><br/> Die Web-App wird mit mehreren Instanzen ohne Single Point of Failure konfiguriert.<br/><br/> Die automatische Skalierung wird aktiviert, damit die App unterschiedliche hohe Datenverkehrsvolumen verarbeiten kann.<br/><br/> Mit dem Wechsel zu PaaS-Diensten kann Contoso veraltete Lösungen, die unter dem Betriebssystem Windows Server 2008 R2 ausgeführt werden, außer Betrieb nehmen.<br/><br/> Cosmos DB weist eine integrierte Fehlertoleranz auf, die keinen Konfigurationsaufwand seitens Contoso erfordert. Dies bedeutet, dass die Datenschicht kein Single Point of Failover mehr ist.
**Nachteile** | Container sind komplexer als andere Migrationsoptionen. Die Lernkurve könnte für Contoso ein Problem darstellen.  Der Grad der Komplexität steigt und bietet trotz der Kurve einen hohen Nutzen.<br/><br/> Das Betriebsteam von Contoso muss eingearbeitet werden, um Azure, Container und Microservices für die App verstehen und unterstützen zu können.<br/><br/> Contoso hat noch nicht vollständig DevOps für die gesamte Lösung implementiert. Daran muss das Unternehmen bei der Bereitstellung von Diensten für AKS, Functions und App Service denken.



### <a name="migration-process"></a>Migrationsprozess

1. Contoso stellt ACR, AKS und Cosmos DB bereit.
2. Es stellt die Infrastruktur für die Bereitstellung bereit, wie etwa die Web-Apps, Speicherkonten, Funktionen und APIs von Azure. 
3. Sobald die Infrastruktur eingerichtet ist, erstellt Contoso Microservicecontainerimages mithilfe der VSTS-Lösung, die diese mithilfe von Push an ACR überträgt.
4. Contoso stellt diese Microservices mithilfe eines PowerShell-Skripts in AKS bereit.
5. Abschließend stellt Contoso die Azure-Funktion und die Web-App bereit.

    ![Migrationsprozess](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Vereinfacht die Verwaltung, die Bereitstellung und den Betrieb von Kubernetes. Nutzt einen vollständig verwalteten Orchestrierungsdienst für Kubernetes-Container.  | AKS ist ein kostenloser Dienst.  Nur die virtuellen Computer sowie die entsprechenden verbrauchten Speicher- und Netzwerkressourcen müssen bezahlt werden. [Weitere Informationen](https://azure.microsoft.com/pricing/details/kubernetes-service/)
[Azure-Funktionen](https://azure.microsoft.com/services/functions/) | Beschleunigt die Entwicklung mit einer ereignisbasierten, serverlosen Computeumgebung. Die Skalierung erfolgt bedarfsabhängig.  | Nur verbrauchte Ressourcen müssen bezahlt werden. Der Plan wird basierend auf dem Ressourcenverbrauch und den Ausführungen pro Sekunde berechnet. [Weitere Informationen](https://azure.microsoft.com/pricing/details/functions/)
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Speichert Images für alle Arten von Containerbereitstellungen. | Die Kosten ergeben sich aus Features, Speicher und Nutzungsdauer. [Weitere Informationen](https://azure.microsoft.com/pricing/details/container-registry/)
[Azure App Service](https://azure.microsoft.com/services/app-service/containers/) | Es können schnell Web-Apps, mobile Apps und API-Apps der Unternehmensklasse auf jeder Plattform erstellt, bereitgestellt und skaliert werden. | App Service-Pläne werden sekundengenau abgerechnet. [Weitere Informationen](https://azure.microsoft.com/pricing/details/app-service/windows/)

## <a name="prerequisites"></a>Voraussetzungen

Für die Ausführung dieses Szenarios benötigen Sie (und Contoso) Folgendes:

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie müssten bereits ein Abonnement erstellt haben, als Sie die Beurteilung im ersten Artikel dieser Reihe durchgeführt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.
**Azure-Infrastruktur** | [Weitere Informationen](contoso-migration-infrastructure.md) zur Vorgehensweise von Contoso beim Einrichten einer Azure-Infrastruktur.
**Voraussetzungen für Entwickler** | Contoso benötigt die folgenden Tools auf einer Entwicklerarbeitsstation:<br/><br/> - [Visual Studio 2017 Community Edition: Version 15.5](https://www.visualstudio.com/)<br/><br/> .NET-Workload aktiviert<br/><br/> [Git-Client](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) oder Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) für die Verwendung von Windows-Containern festgelegt



## <a name="scenario-steps"></a>Szenarioschritte

Contoso geht bei der Ausführung der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Bereitstellen von AKS und ACR**: Contoso stellt mithilfe von PowerShell den verwalteten AKS-Cluster und Azure Container Registry bereit.
> * **Schritt 2: Erstellen von Docker-Containern**: Das Unternehmen richtet CI für Docker-Container mithilfe von VSTS ein und überträgt diese mithilfe von Push an ACR.
> * **Schritt 3: Bereitstellen von Back-End-Microservices**: Es stellt die restliche Infrastruktur bereit, die von Back-End-Microservices genutzt wird.
> * **Schritt 4: Bereitstellen der Front-End-Infrastruktur**: Es stellt die Front-End-Infrastruktur bereit, einschließlich Blobspeicher für Fotos von Haustieren, Cosmos DB und die Maschinelles Sehen-API.
> * **Schritt 5: Migrieren des Back-Ends**: Es stellt Microservices bereit und führt diese in AKS aus, um das Back-End zu migrieren.
> * **Schritt 6: Veröffentlichen des Front-Ends**: Es veröffentlicht die SmartHotel-App in Azure App Service und die Funktionen-App, die vom Haustierdienst aufgerufen wird.



## <a name="step-1-provision-an-aks-cluster-and-acr"></a>Schritt 1: Bereitstellen eines AKS-Cluster und von ACR

Contoso führt ein Bereitstellungsskript zum Erstellen des verwalteten Kubernetes-Clusters mit AKS und Azure Container Registry aus.

- In den Anweisungen für diesen Abschnitt wird das Repository **SmartHotel360-Azure-backend** verwendet.
- Das GitHub-Repository **SmartHotel360-Azure-backend** enthält die gesamte Software für diesen Teil der Bereitstellung.

Die Bereitstellung erfolgt wie folgt:

1. Vor Beginn stellt Contoso sicher, dass die erforderliche Software auf dem Entwicklungscomputer, den das Unternehmen verwendet, installiert ist. 
2. Es klont das Repository mithilfe von Git lokal auf dem Entwicklungscomputer.

    **git clone https://github.com/Microsoft/SmartHotel360-Azure-backend.git**

3.  Contoso öffnet den Ordner mit Visual Studio Code und verschiebt diesen in das Verzeichnis **/deploy/k8s**, das das Skript **gen-aks-env.ps1** enthält.
4. Contoso führt das Skript zum Erstellen des verwalteten Kubernetes-Clusters mit AKS und Container Registry aus.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
5.  Bei geöffneter Datei aktualisiert Contoso den $location-Parameter in **eastus2** und speichert die Datei.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

6. Es klickt auf **Ansicht** > **Integriertes Terminal**, um das in Code integrierte Terminal zu öffnen.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

7. In dem in PowerShell integrierten Terminal meldet Contoso sich bei Azure mit dem Befehl „Connect-AzureRmAccount“ an. Weitere Informationen zu den ersten Schritten mit PowerShell finden Sie [hier](https://docs.microsoft.com/powershell/azure/get-started-azureps).

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

8. Contoso authentifiziert die Azure CLI, indem es den Befehl **az login** ausführt und die Anweisungen für die Authentifizierung mithilfe eines Webbrowsers befolgt. Weitere Informationen zur Anmeldung bei der Azure CLI finden Sie [hier](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

9. Contoso führt den folgenden Befehl aus, übergibt den Ressourcengruppennamen von ContosoRG, den Namen des AKS-Clusters „smarthotel-aks-eus2“ und den Namen der neuen Registrierung.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```

    ![AKS](./media/contoso-migration-rebuild/aks6.png)

10. Azure erstellt eine andere Ressourcengruppe, die die Ressourcen für den AKS-Cluster enthält.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

11. Nachdem die Bereitstellung abgeschlossen ist, installiert Contoso das Befehlszeilentool **kubectl**. Das Tool ist bereits in Azure Cloud Shell installiert.

    **az aks install-cli**

12. Contoso überprüft die Verbindung mit dem Cluster, indem es den Befehl **kubectl get nodes** ausführt. Der Knoten weist den gleichen Namen wie die VM in der automatisch erstellten Ressourcengruppe auf.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

13. Contoso führt den folgenden Befehl zum Starten des Kubernetes-Dashboards aus: 

    **az aks browse --resource-group ContosoRG --name smarthotelakseus2**

14. Eine Browserregisterkarte wird auf dem Dashboard geöffnet. Hierbei handelt es sich um eine getunnelte Verbindung, die auf die Azure CLI zurückgreift. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-build-a-docker-container"></a>Schritt 2: Erstellen eines Docker-Containers

### <a name="create-a-vsts-and-build"></a>Erstellen eines VSTS-Projekts und eines Builds

Contoso erstellt ein VSTS-Projekt und konfiguriert einen CI-Build zum Erstellen des Containers und überträgt es dann mithilfe von Push an ACR. In den Anweisungen in diesem Abschnitt wird das Repository [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) verwendet.

1. Unter „visualstudio.com“ erstellt Contoso ein neues Konto (**contosodevops360.visualstudio.com**) und konfiguriert es für die Verwendung von Git.

2. Es erstellt ein neues Projekt (**smarthotelrefactor**) mithilfe von Git für die Versionskontrolle und Agile für den Workflow.

    ![VSTS](./media/contoso-migration-rebuild/vsts1.png) 


3. Contoso importiert das GitHub-Repository.

    ![VSTS](./media/contoso-migration-rebuild/vsts2.png)
    
4. Unter **Build und Release** erstellt Contoso mit VSTS-Git als Quelle eine neue Definition aus dem importierten Repository **smarthotel**. 

    ![VSTS](./media/contoso-migration-rebuild/vsts3.png)

6. Contoso legt fest, mit einer leeren Pipeline zu beginnen.

    ![VSTS](./media/contoso-migration-rebuild/vsts4.png)  

7. Contoso klickt auf **Gehostete Linux-Vorschau** für die Builddefinition.

    ![VSTS](./media/contoso-migration-rebuild/vsts5.png) 
 
8. Unter **Phase 1** fügt Contoso einen **Docker Compose**-Task hinzu. Dieser Task erstellt Docker Compose.

    ![VSTS](./media/contoso-migration-rebuild/vsts6.png) 

9. Contoso wiederholt den **Docker Compose**-Task und fügt einen weiteren Task hinzu. Dieser überträgt die Container mithilfe von Push an ACR.

     ![VSTS](./media/contoso-migration-rebuild/vsts7.png) 

8. Contoso wählt den ersten Task (für die Erstellung) aus und konfiguriert den Build mit dem Azure-Abonnement, der Autorisierung und ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts8.png)

9. Contoso gibt den Pfad der Datei **docker-compose.yaml** im Ordner **src** des Repositorys an. Contoso legt fest, Dienstimages zu erstellen und den aktuellen Tag einzuschließen. Wenn sich die Aktion in **Dienstimages erstellen** ändert, ändert sich der Name des VSTS-Tasks in **Dienste automatisch erstellen**.

    ![VSTS](./media/contoso-migration-rebuild/vsts9.png)

10. Contoso konfiguriert nun den zweiten Docker-Task (für die Übertragung mithilfe von Push). Das Unternehmen wählt das Abonnement und die ACR-Instanz **smarthotelacreus2** aus. 

    ![VSTS](./media/contoso-migration-rebuild/vsts10.png)

11. Es gibt erneut die Datei für die Datei „docker-compose.yaml“ ein, klickt auf **Dienstimages mithilfe von Push übertragen** und schließt das aktuelle Tag ein. Wenn sich die Aktion in **Dienstimages mithilfe von Push übertragen** ändert, ändert sich der Name des VSTS-Tasks in **Dienste automatisch mithilfe von Push übertragen**.

    ![VSTS](./media/contoso-migration-rebuild/vsts11.png)

12. Nachdem die VSTS-Tasks konfiguriert wurden, speichert Contoso die Builddefinition und beginnt mit dem Buildprozess.

    ![VSTS](./media/contoso-migration-rebuild/vsts12.png)

13. Contoso klickt auf den Buildauftrag, um den Status zu überprüfen.

    ![VSTS](./media/contoso-migration-rebuild/vsts13.png)

14. Wenn der Build fertiggestellt ist, zeigt ACR die neuen Repositorys an, die mit den von den Microservices verwendeten Containern aufgefüllt werden.

    ![VSTS](./media/contoso-migration-rebuild/vsts14.png)


## <a name="step-3-deploy-back-end-microservices"></a>Schritt 3: Bereitstellen von Back-End-Microservices

Mithilfe des erstellten AKS-Clusters und der Docker-Imagebuilds stellt Contoso nun die restliche Infrastruktur bereit, die von Back-End-Microservices genutzt wird.

- In den Anweisungen in diesem Abschnitt wird das Repository [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) verwendet.
- Im Ordner **/deploy/k8s/arm** gibt es ein einziges Skript zum Erstellen aller Elemente. 

Die Bereitstellung erfolgt wie folgt:

1. Contoso verwendet die Datei „deploy.cmd“, um die Azure-Ressourcen in der Ressourcengruppe „ContosoRG“ und in der Region „USA, Osten 2“ bereitstellen, indem es den folgenden Befehl eingibt:

    **.\deploy azuredeploy ContosoRG -c eastus2**

    ![Bereitstellen des Back-Ends](./media/contoso-migration-rebuild/backend1.png)

2. Contoso erfasst die Verbindungszeichenfolge für die einzelnen Datenbanken, die später verwendet werden.

    ![Bereitstellen des Back-Ends](./media/contoso-migration-rebuild/backend2.png)

## <a name="step-4-deploy-front-end-infrastructure"></a>Schritt 4: Bereitstellen der Front-End-Infrastruktur

Contoso muss die Infrastruktur bereitstellen, die von den Front-End-Apps verwendet wird. Es erstellt einen Blobspeichercontainer zum Speichern der Bilder der Haustiere, die Cosmos DB-Datenbank zum Speichern von Dokumenten mit den Informationen zu den Haustieren und die Maschinelles Sehen-API für die Website. 

In den Anweisungen für diesen Abschnitt wird das Repository [SmartHotel-public-web](https://github.com/Microsoft/SmartHotel360-public-web) verwendet.

### <a name="create-storage-containers"></a>Erstellen von Speichercontainern

1.  Im Azure-Portal öffnet Contoso das erstellte Speicherkonto und klickt auf **Blobs**.
2.  Es erstellt einen neuen Container (**Haustiere**), bei dem die öffentliche Zugriffsebene auf Container festgelegt ist. Benutzer laden ihre Fotos von Haustieren in diesen Container hoch.

    ![Speicherblob](./media/contoso-migration-rebuild/blob1.png)

3. Es erstellt einen zweiten neuen Container namens **Einstellungen**. Eine Datei mit allen Front-End-App-Einstellungen wird in diesen Container platziert.

    ![Speicherblob](./media/contoso-migration-rebuild/blob2.png)

4. Es speichert die Zugriffsdetails für das Speicherkonto in einer Textdatei zur späteren Verwendung.

    ![Speicherblob](./media/contoso-migration-rebuild/blob2.png)

## <a name="provision-a-cosmos-database"></a>Bereitstellen einer Cosmos DB-Datenbank

Contoso stellt eine Cosmos DB-Datenbank bereit, die für Informationen zu Haustieren verwendet wird.

1. Contoso erstellt eine **Azure Cosmos DB**-Instanz im Azure Marketplace.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos1.png)

2. Contoso gibt einen Namen (**contosomarthotel**) an, wählt die SQL-API aus und fügt sie in die Produktionsressourcengruppe „ContosoRG“ in der Hauptregion „USA, Osten 2“ ein.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos2.png)

3. Contoso fügt eine neue Sammlung zur Datenbank mit Standardkapazität und -durchsatz hinzu.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos3.png)


4. Contoso nimmt die Verbindungsinformationen für die Datenbank für die zukünftige Verwendung zur Kenntnis. 

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos4.png)


## <a name="provision-computer-vision"></a>Bereitstellen der Maschinelles Sehen-API

Contoso stellt die Maschinelles Sehen-API bereit. Die API wird von der Funktion zum Auswerten der Bilder aufgerufen, die von den Benutzern hochgeladen wurden.

1. Contoso erstellt im Azure Marketplace eine **Maschinelles Sehen**-Instanz.

     ![Maschinelles Sehen](./media/contoso-migration-rebuild/vision1.png)

2. Contoso stellt die API (**smarthotelpets**) in der Produktionsressourcengruppe „ContosoRG“ in der Hauptregion „USA, Osten 2“ bereit.

    ![Maschinelles Sehen](./media/contoso-migration-rebuild/vision2.png)

3. Contoso speichert die Verbindungseinstellungen für die API in einer Textdatei zur späteren Verwendung.

     ![Maschinelles Sehen](./media/contoso-migration-rebuild/vision3.png)

## <a name="step-5-deploy-the-back-end-services-in-azure"></a>Schritt 5: Bereitstellen von Back-End-Diensten in Azure

Nun muss Contoso erst den NGINX-Eingangscontroller bereitstellen, um eingehenden Datenverkehr für die Dienste zuzulassen, und dann die Microservices für den AKS-Cluster.

In den Anweisungen in diesem Abschnitt wird das Repository [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) verwendet.



1. Das Unternehmen verwendet Visual Studio Code, um die Datei **/deploy/k8s/config_loal.yml** zu aktualisieren. Es aktualisiert die verschiedenen Datenbankverbindungen anhand von Informationen, die Contoso zuvor notiert hat.

     ![Bereitstellen von Microservices](./media/contoso-migration-rebuild/deploy-micro.png)

    > [!NOTE]
    > Einige der Konfigurationseinstellungen (z.B. Active Directory B2C) werden nicht in diesem Artikel behandelt. Das Repository enthält weitere Informationen zu diesen Einstellungen.

2. Die Datei „deploy.ps1“ löscht den gesamten Clusterinhalt (außer den eingehenden Daten und dem Eingangscontroller) und stellt Microservices bereit. Contoso führt diese wie folgt aus:

  ```
  .\deploy.ps1 -configFile .\conf_local.yml -dockerUser smarthotelacreus2  -dockerPassword [passwordhere] -registry smarthotelacreus2.azurecr.io -imageTag latest -deployInfrastructure $false -buildImages $false -pushImages $false
  ```

3. Das Unternehmen führt den folgenden Befehl aus, um den Status der Dienste zu überprüfen:

    ```
    kubectl get services
    ```
4. Es öffnet das Kubernetes-Dashboard, um die Bereitstellung zu überprüfen:

    ```
    az aks browse --resource-group ContosoRG --name smarthotelakseus2
    ```


## <a name="step-6-publish-the-frontend"></a>Schritt 6: Veröffentlichen des Front-Ends

Im letzten Schritt veröffentlicht Contoso die SmartHotel-App in Azure App Service und die Funktionen-App, die vom Haustierdienst aufgerufen wird.

In den Anweisungen in diesem Abschnitt wird das Repository [SmartHotel-public-web](https://github.com/Microsoft/SmartHotel360-public-web) verwendet.

### <a name="set-up-web-app-to-use-contoso-resources"></a>Einrichten einer Web-App für die Nutzung von Contoso-Ressourcen

1. Contoso klont das Repository mithilfe von Git lokal auf dem Entwicklungscomputer:

    **git clone https://github.com/Microsoft/SmartHotel360-public-web.git**

2. In Visual Studio öffnet Contoso den Ordner, um alle Dateien im Repository anzuzeigen.

    ![Veröffentlichen des Front-Ends](./media/contoso-migration-rebuild/front-publish1.png)

3. Contoso nimmt die für die Standardeinstellung erforderlichen Konfigurationsänderungen vor.

    - Wenn die Web-App gestartet wird, sucht es nach der App-Einstellung **SettingsUrl**.
    - Diese Variable muss eine URL zu einer Konfigurationsdatei enthalten.
    - Standardmäßig wird die Einstellung als öffentlicher Endpunkt verwendet.

4. Contoso aktualisiert die Datei **/config-sample.json/sample.json**. Dies ist die Konfigurationsdatei für das Web, wenn der öffentliche Endpunkt verwendet wird.

    - Contoso bearbeitet die Abschnitte **urls** und **pets_config** mit den Werten für die AKS-API-Endpunkte, die Speicherkonten und die Cosmos DB-Datenbank. 
    - Die URLs sollten dem DNS-Namen der neuen Web-App entsprechen, die von Contoso erstellt wird.
    - Bei Contoso lautet dieser **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![Veröffentlichen des Front-Ends](./media/contoso-migration-rebuild/front-publish2.png)

5. Nachdem die Datei aktualisiert wurde, benennt Contoso diese in **smarthotelsettingsurl** um und lädt diese in den Speicherblob hoch, den das Unternehmen zuvor erstellt hat.

     ![Veröffentlichen des Front-Ends](./media/contoso-migration-rebuild/front-publish3.png)

6. Contoso klickt auf die Datei, um die URL abzurufen. Diese URL wird von der App verwendet, wenn das Unternehmen mit dem Abrufen der Konfigurationsdatei beginnt.

    ![Veröffentlichen des Front-Ends](./media/contoso-migration-rebuild/front-publish4.png)

7. Es aktualisiert **SettingsUrl** in den Dateien **appsettings.Production.json** in die URL der neuen Datei.

    ![Veröffentlichen des Front-Ends](./media/contoso-migration-rebuild/front-publish5.png)


### <a name="deploy-the-website-to-the-azure-app-service"></a>Bereitstellen der Website in Azure App Service

Contoso kann jetzt seine Website veröffentlichen.


1. Es aktiviert die Application Insights-Überwachung in Visual Studio 2017. Zu diesem Zweck wählt Contoso das Projekt **PublicWeb** im Projektmappen-Explorer aus und sucht nach **Application Insights hinzufügen**. Es registriert die App mit der Application Insights-Instanz, die bei der Bereitstellung der Infrastruktur erstellt wurde.

    ![Veröffentlichen der Website](./media/contoso-migration-rebuild/deploy-website1.png)

2. In Visual Studio stellt Contoso eine Verbindung zwischen dem PublicWeb-Projekt und Application Insights her und aktualisiert es, sodass angezeigt wird, dass es konfiguriert wurde.
 
    ![Veröffentlichen der Website](./media/contoso-migration-rebuild/deploy-website2.png)

3. Contoso erstellt und veröffentlicht seine Web-App in Visual Studio.

    ![Veröffentlichen der Website](./media/contoso-migration-rebuild/deploy-website3.png)

5. Contoso gibt einen App-Namen an, und fügt die App in die Produktionsressourcengruppe **ContosoRG** in der Hauptregion „USA, Osten 2“ ein.

    ![Veröffentlichen der Website](./media/contoso-migration-rebuild/deploy-website4.png)

### <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

1. Contoso erstellt und veröffentlicht die Funktion mithilfe von Visual Studio. Zu diesem Zweck klickt das Unternehmen mit der rechten Maustaste auf **PetCheckerFunction** > **Veröffentlichen**. Anschließend erstellt es eine neue App Service-Funktion.

     ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function1.png)

2. Contoso gibt den Namen **smarthotelpetchecker** an, fügt die Funktion in die Ressourcengruppe „ContosoRG“ ein und fügt einen neuen App Service-Plan hinzu.

     ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function2.png)

3. Contoso wählt das Speicherkonto aus und erstellt die Funktion.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function3.png)

4. Die Bereitstellung beginnt mit der Bereitstellung der Funktionen-App in Azure. Unter **Veröffentlichen** fügt Contoso die App-Einstellungen für den Speicher, die Cosmos DB-Datenbank und die Maschinelles Sehen-API hinzu.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function4.png)

5. Um die Funktion zuerst lokal auszuführen, aktualisiert Contoso die Einstellungen in **PetCheckerFunction/local.settings.json**.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function5.png)

6. Nachdem die Funktion bereitgestellt wurde, wird sie im Azure-Portal mit dem Status **Wird ausgeführt** angezeigt.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function6.png)


7. Contoso navigiert zur App unter [http://smarthotel360public.azurewebsites.net/Pets](http://smarthotel360public.azurewebsites.net/Pets), um zu testen, ob die Pet Checker-App wie erwartet funktioniert.
8. Es klickt auf den Avatar, um ein Bild hochzuladen.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function7.png)

9. Das erste Foto, das überprüft werden soll, ist das eines kleinen Hundes.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function8.png)

10. Die App gibt eine Meldung zur Bestätigung zurück.

    ![Bereitstellen der Funktion](./media/contoso-migration-rebuild/function9.png)




## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso die neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

- Contoso muss sicherstellen, dass seine neuen Datenbanken geschützt sind. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)
- Die App muss für die Verwendung von SSL mit Zertifikaten aktualisiert werden. Die Containerinstanz sollte erneut bereitgestellt werden, um auf 443 zu antworten.
- Zum Schutz von Geheimnissen für die Service Fabric-Apps sollte die Verwendung von Key Vault in Erwägung gezogen werden. [Weitere Informationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management)

### <a name="backups-and-disaster-recovery"></a>Sicherungen und Notfallwiederherstellung

- Contoso muss die Sicherungsanforderungen für Azure SQL-Datenbank überprüfen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)
- Die Implementierung von SQL-Failovergruppen sollte berücksichtigt werden, um ein regionales Failover für die Datenbank bereitzustellen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)
- Die geografische Replikation für die Premium-SKU von ACR kann genutzt werden. [Weitere Informationen](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Nachdem alle Ressourcen bereitgestellt wurden, sollte Contoso Azure-Tags basierend auf seiner [Infrastrukturplanung](contoso-migration-infrastructure.md#set-up-tagging) zuweisen.
- Die gesamte Lizenzierung ist in die Kosten für die PaaS-Dienste integriert, die Contoso verwendet. Dies wird über EA verrechnet.
- Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management.

## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel wurde die SmartHotel-App in Azure von Contoso neu erstellt. Das Unternehmen hat die lokale App-Front-End-VM in Azure App Service-Web-Apps neu erstellt. Das Back-End der App wurde mithilfe von Microservices erstellt, die für Container bereitgestellt werden, die von Azure Kubernetes Service (AKS) verwaltet werden. Die App-Funktionalität hat Contoso durch eine App für Fotos von Haustieren verbessert.




