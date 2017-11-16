---
title: Aufgaben des Teamleiters eines Team Data Science-Prozesses - Azure | Microsoft-Dokumentation
description: "In diesem Artikel erhalten Sie einen Überblick über die Aufgaben des Teamleiters eines Data Science-Teamprojekts."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev;
ms.openlocfilehash: 58e8bfeb547fbcd7f472fbed50073b1ca3176b72
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="team-lead-tasks"></a>Aufgaben des Teamleiters

In diesem Thema werden die Aufgaben beschrieben, die ein Teamleiter für sein Data Science-Team zu erfüllen hat. Ziel ist es, eine auf Zusammenarbeit ausgelegte Teamumgebung einzurichten, die gemäß dem [Team Data Science-Prozess](overview.md) (TDSP) vereinheitlicht ist. Der TDSP ist eine flexible, iterative Data Science-Methodik zur effizienten Bereitstellung von Predictive Analytics-Lösungen und intelligenten Anwendungen. Er soll die Zusammenarbeit und die Lernprozesse im Team verbessern. Der Prozess fasst die bewährten Methoden und Strukturen von Microsoft sowie aus der gesamten Branche zusammen, die für eine erfolgreiche Implementierung von Data Science-Initiativen erforderlich sind, mit denen Unternehmen maximalen Nutzen aus ihren Analyseprogrammen ziehen können. Unter [Rollen und Aufgaben im Team Data Science-Prozess](roles-tasks.md) finden Sie eine Übersicht über die wichtigsten Mitarbeiterrollen und zugeordneten Aufgaben für ein Data Science-Team, das eine Vereinheitlichung gemäß diesem Prozess anstrebt.

Ein **Teamleiter** verwaltet ein Team in der Data Science-Einheit eines Unternehmens. Ein Team besteht aus mehreren Datenanalysten. Besteht die Data Science-Einheit nur aus einer kleinen Anzahl von Datenanalysten, können der **Gruppenleiter** und der **Teamleiter** dieselbe Person sein; er kann seine Aufgabe aber auch einem Stellvertreter übertragen. Die Aufgaben selbst ändern sich jedoch dabei nicht. Der Workflow für die Aufgaben, die von Teamleitern zum Einrichten dieser Umgebung ausgeführt werden, sind in der folgenden Abbildung dargestellt:

![1](./media/team-lead-tasks/team-leads-1-creating-teams.png)

>[AZURE.NOTE] Die Aufgaben in den Blöcken 1 und 2 der Abbildung werden benötigt, wenn Visual Studio Team Services (VSTS) als Hostingplattform für Code genutzt wird und ein separates Teamprojekt für das eigene Team realisiert werden soll. Sobald diese Aufgaben abgeschlossen wurden, können alle Repositorys des Teams unter diesem Teamprojekt erstellt werden. 

Nachdem vom Gruppenleiter verschiedene vorab erforderliche Aufgaben ausgeführt wurden, die in einem der folgenden Abschnitte aufgeführt werden, stehen die fünf grundlegenden Aufgaben (von denen einige optional sind) an, die in diesem Tutorial durchgearbeitet werden. Diese Aufgaben entsprechen den nummerierten Hauptabschnitten des vorliegenden Themas:

1. Erstellen eines **Teamprojekts** auf dem VSTS-Server der Gruppe und von zwei Team-Repositorys im Projekt:
    - **ProjectTemplate-Repository** 
    - **TeamUtilities-Repository**
2. Durchführen des Seedings für das **ProjectTemplate**-Repository des Teams aus dem **GroupProjectTemplate**-Repository, das von Ihrem Gruppenleiter eingerichtet wurde. 
3. Erstellen von Teamdaten und Analyseressourcen:
    - Hinzufügen der teamspezifischen Hilfsprogramme zum **TeamUtilities**-Repository. 
    - (Optional) Erstellen eines **Azure File Storage** zum Speichern von Datenassets, die für das gesamte Team nützlich sein können. 
4. (Optional) Bereitstellen des Azure File Storage auf dem **Data Science Virtual Machine** (DSVM) des Teamleiters und Hinzufügen von Datenassets zum DSVM.
5. Einrichten von **Sicherheitskontrollen** durch Hinzufügen von Teammitgliedern und Konfigurieren ihrer Berechtigungen.

>[AZURE.NOTE] In den folgenden Anleitungen werden die erforderlichen Schritte zum Einrichten einer TDSP-Teamumgebung per VSTS beschrieben. Wir geben an, wie diese Aufgaben mit VSTS erledigt werden können, da wir bei Microsoft TDSP auf diese Weise implementieren. Wenn für Ihre Gruppe eine andere Plattform zum Hosten des Codes verwendet wird, ändern sich die vom Teamleiter auszuführenden Aufgaben in der Regel nicht. Sie müssen aber auf andere Weise ausgeführt werden.

## <a name="repositories-and-directories"></a>Repositorys und Verzeichnisse

In diesem Thema werden abgekürzte Namen für Repositorys und Verzeichnisse verwendet. Diese Namen erleichtern es, den Vorgängen zwischen den Repositorys und Verzeichnissen zu folgen. Diese Notation (**R** für Git-Repositorys und **D** für lokale Verzeichnisse auf Ihrer DSVM) wird in den folgenden Abschnitten verwendet:

- **R1**: Das Repository **GroupProjectTemplate** auf Git, das Ihr Gruppenleiter auf Ihrem VSTS-Gruppenserver eingerichtet hat.
- **R3**: Das Team-Repository **ProjectTemplate** auf Git, das Sie eingerichtet haben.
- **R4**: Das Repository **TeamUtilities** auf Git, das Sie eingerichtet haben.
- **D1**: Das von R1 geklonte lokale Verzeichnis, das zu D3 kopiert wurde.
- **D3**: Das von R3 geklonte lokale Verzeichnis, das angepasst und zurück zu R3 kopiert wurde.
- **D4**: Das von R4 geklonte lokale Verzeichnis, das angepasst und zurück zu R4 kopiert wurde.

Die Namen für die Repositorys und Verzeichnisse in diesem Tutorial wurden unter der Annahme angegeben, dass Sie ein separates Teamprojekt für Ihr eigenes Team innerhalb einer umfassenderen Data Science-Gruppe erstellen möchten. Ihnen als Teamleiter stehen jedoch weitere Möglichkeiten offen:

- Die gesamte Gruppe kann sich entscheiden, ein einziges Teamprojekt zu erstellen. Die Projekte sämtlicher Data Science-Teams befinden sich dann unter diesem zentralen Teamprojekt. Hierzu können Sie einen Git-Administrator damit beauftragen, gemäß diesen Anweisungen ein einziges Teamprojekt zu erstellen. Dies Szenario kann beispielsweise gültig sein für:
    -  eine kleine Data Science-Gruppe, die nicht in mehrere Data Science-Teams unterteilt ist 
    -  eine größere Data Science-Gruppe mit mehreren Data Science-Teams, die jedoch die teamübergreifende Zusammenarbeit mit Aktivitäten wie Sprintplanung auf der Gruppenebene optimieren möchten. 
- Teams können festlegen, dass teamspezifische Projektvorlagen oder teamspezifische Hilfsprogramme unter dem zentralen Teamprojekt für die gesamte Gruppe verwaltet werden. In diesem Fall muss der Teamleiter Repositorys für Teamprojektvorlagen und/oder Repositorys für Teamhilfsprogramme unter ein und demselben Teamprojekt erstellen. Benennen Sie diese Repositorys *<TeamName\>ProjectTemplate* und *<TeamName\>Utilities*; beispielsweise *TeamJohnProjectTemplate* und *TeamJohnUtilities*. 

Teamleiter müssen in jedem Fall ihren Teammitgliedern mitteilen, welche Repositorys für Vorlagen und Hilfsprogramme zu nutzen sind, wenn sie die Repositorys für das Projekt und die Hilfsprogramme einrichten und klonen. Projektleiter müssen die [Aufgaben des Projektleiters eines Data Science-Teams](project-lead-tasks.md) ausführen, um Projekt-Repositorys zu erstellen, wobei es keine Rolle spielt, ob dies unter einem separaten Teamprojekt oder einem zentralen Teamprojekt erfolgt. 


## <a name="0-prerequisites"></a>0. Voraussetzungen

Zum Erfüllen der Voraussetzungen müssen die dem Gruppenleiter zugewiesenen Aufgabe ausgeführt werden. Siehe dazu [Aufgaben des Gruppenleiters eines Data Science-Teams](group-manager-tasks.md). Kurz zusammengefasst müssen die folgenden Anforderungen erfüllt sein, ehe Sie mit den Aufgaben des Teamleiters beginnen: 

- Ihr **VSTS-Gruppenserver** (oder Gruppenkonto für eine andere Codehostingplattform) wurde von Ihrem Gruppenmanager eingerichtet.
- Ihr **GroupProjectTemplate-Repository** (R1) wurde unter Ihrem Gruppenkonto von Ihrem Gruppenleiter auf der Codehostingplattform eingerichtet, die verwendet werden soll.
- Sie wurden in Ihrem Gruppenkonto **autorisiert**, Repositorys für Ihr Team zu erstellen.
- Git muss auf Ihrem Computer installiert sein. Wenn Sie eine Data Science Virtual Machine (DSVM) verwenden, ist Git bereits installiert, sodass Sie gleich loslegen können. Andernfalls helfen Ihnen die Informationen im [Anhang von Plattformen und Tools](platforms-and-tools.md#appendix) weiter.  
- Bei Verwendung einer **Windows DSVM** muss [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) auf Ihrem Computer installiert sein. Scrollen Sie in der Datei „README.md“ nach unten zum Abschnitt **Download and Install**, und klicken Sie auf den *neuesten Installer*. Dadurch gelangen Sie zur Seite mit dem neuesten Installer. Laden Sie den Installer mit der Erweiterung EXE herunter, und führen Sie ihn aus. 
- Bei Verwendung einer **Linux DSVM** erstellen Sie auf Ihrer DSVM einen öffentlichen SSH-Schlüssel, den Sie dem VSTS-Server Ihrer Gruppe hinzufügen. Weitere Informationen zu SSH finden Sie im [Anhang von Plattformen und Tools](platforms-and-tools.md#appendix) im Abschnitt **Erstellen eines öffentlichen SSH-Schlüssels**. 
    
## <a name="1-create-a-team-project-and-repositories"></a>1. Erstellen eines Teamprojekts und von Repositorys

Führen Sie diesen Schritt aus, wenn Sie VSTS als Codehostingplattform für Versionsverwaltung und Zusammenarbeit verwenden. In diesem Abschnitt erstellen Sie drei Artefakte auf dem VSTS-Server der Gruppe:

- Projekt **MyTeam** in VSTS
- Repository **MyProjectTemplate** (**R3**) in Git
- Repository **MyTeamUtilities** (**R4**) in Git

### <a name="create-the-myteam-project"></a>Erstellen des Projekts MyTeam

- Rufen Sie unter der URL `https://<VSTS Server Name\>.visualstudio.com` die Homepage des VSTS-Servers Ihrer Gruppe auf. 
- Klicken Sie auf **Neu**, um ein Teamprojekt zu erstellen. 

    ![2](./media/team-lead-tasks/team-leads-2-create-new-team.png)

- In einem Fenster „Teamprojekt erstellen“ werden Sie aufgefordert, den Projektnamen einzugeben (in diesem Beispiel **MyTeam**). Sie müssen **Agile** als **Prozessvorlage** und **Git** als **Versionskontrolle** auswählen. 

    ![3](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)

- Klicken Sie auf **Projekt erstellen**. Das Teamprojekt **MyTeam** wird in weniger als einer Minute erstellt. 

- Klicken Sie nach dem Erstellen des Teamprojekts **MyTeam** auf die Schaltfläche **Zu Projekt navigieren**. Sie werden zur Homepage Ihres Teamprojekts weitergeleitet. 

    ![4](./media/team-lead-tasks/team-leads-4-create-new-team-3.png)

- Wenn ein Popup-Fenster **Herzlichen Glückwunsch!** geöffnet wird, klicken Sie auf **Code hinzufügen** (rot umrandete Schaltfläche). Klicken Sie andernfalls auf **Code** (gelb umrandet). Dadurch gelangen Sie zur Git-Repository-Seite Ihres Teamprojekts. 

    ![5](./media/team-lead-tasks/team-leads-5-team-project-home.png)

### <a name="create-the-myprojecttemplate-repository-r3-on-git"></a>Erstellen des Repositorys „MyProjectTemplate“ (R3) in Git

- Klicken Sie auf der Git-Repository-Seite Ihres Teamprojekts auf den nach unten zeigenden Pfeil neben dem Repositorynamen **MyTeam**, und wählen Sie **Repositorys verwalten** aus.

    ![6](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)

- Klicken Sie auf der Seite **Versionskontrolle** des Teamprojekts auf **MyTeam**, und wählen Sie **Repository umbenennen** aus. 

    ![7](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)

- Geben Sie im Fenster **Repository MyTeam umbenennen** einen neuen Namen für das Repository ein. In diesem Beispiel lautet der Name *MyTeamProjectTemplate*. Sie können einen Namen wie *<Name Ihres Teams\>ProjectTemplate* eingeben. Klicken Sie auf **Umbenennen**, um fortzufahren.

    ![8](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)

### <a name="create-the-myteamutilities-repository-r4-on-git"></a>Erstellen des Repositorys „MyTeamUtilities“ (R4) in Git

- Klicken Sie zum Erstellen eines neuen Repositorys *<Name Ihres Teams\>Utilities* unter Ihrem Teamprojekt auf der Registerkarte **Versionskontrolle** der Systemsteuerung für das Teamprojekt auf **Neues Repository**.  

    ![9](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)

- Geben Sie im Fenster **Neues Repository erstellen** einen Namen für dieses Repository an. In diesem Beispiel benennen wir es *MyTeamUtilities*, was in unserer Notation **R4** entspricht. Wählen Sie einen Namen wie *<Name Ihres Teams\>Utilities* aus. Sie müssen als **Typ** **Git** auswählen. Klicken Sie zum Fortfahren auf **Erstellen**.

    ![10](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)

- Vergewissern Sie sich, dass die beiden neu erstellten Git-Repositorys unter dem Teamprojekt **MyTeam** angezeigt werden. In diesem Beispiel: 

- **MyTeamProjectTemplate** (R3) 
- **MyTeamUtilities** (R4).

    ![11](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)


## <a name="2-seed-your-team-projecttemplate-and-teamutilities-repositories"></a>2. Seeding der Repositorys „ProjectTemplate“ und „TeamUtilities“

Für das Seeding-Verfahren werden die Verzeichnisse auf Ihrem lokalen DSVM als Stagingsites für die Zwischenspeicherung verwendet. Wenn Sie die Repositorys **ProjectTemplate** und **TeamUtilities** an bestimmte Anforderungen des Teams anpassen müssen, müssen Sie dies im vorletzten Schritt des folgenden Verfahrens tun. Hier finden Sie eine Übersicht über die Schritte für das Seeding der Inhalte der Repositorys **MyTeamProjectTemplate** und **MyTeamUtilities** für ein Data Science-Team. Die einzelnen Schritte entsprechen den Unterabschnitten im Seeding-Verfahren:

- Klonen des Gruppen-Repositorys in ein lokales Verzeichnis: Team-R1 – geklont zu –> lokales D1
- Klonen der Team-Repositorys in lokale Verzeichnisse: Team-R3 und -R4 – geklont zu –> lokales D3 und D4
- Kopieren des Inhalts der Gruppenprojektvorlage in den lokalen Teamordner:  D1 – Inhalt kopiert in –> D3
- (Optional) Anpassung von lokalem D3 und D4
- Übertragen des Inhalts des lokalen Verzeichnisses in die Team-Repositorys: D3 und D4 – Inhalt hinzufügen zu –> Team-R3 und -R4


### <a name="initialize-the-team-repositories"></a>Initialisieren der Team-Repositorys

In diesem Schritt initialisieren Sie Ihr Repository für die Teamprojektvorlage aus dem Repository für die Gruppenprojektvorlage:

- Repository **MyTeamProjectTemplate** (**R3**) aus dem Repository **GroupProjectTemplate** (**R1**)


### <a name="clone-group-repositories-into-local-directories"></a>Klonen von Gruppen-Repositorys in lokale Verzeichnisse

Zum Starten dieses Verfahrens:

- Erstellen Sie Verzeichnisse auf dem lokalen Computer:
    - Für **Windows**: **C:\GitRepos\GroupCommon** und **C:\GitRepos\MyTeam**
    - Für **Linux**: **GitRepos\GroupCommon** und **GitRepos\MyTeam** in Ihrem Basisverzeichnis 
- Wechseln Sie zum Verzeichnis **GitRepos\GroupCommon**.
- Führen Sie den folgenden Befehl im Betriebssystem des lokalen Computers aus.

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/GroupCommon/_git/GroupProjectTemplate
    

![12](./media/team-lead-tasks/team-leads-12-create-two-group-repos.png)

**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/GroupCommon/_git/GroupProjectTemplate
    
    
![13](./media/team-lead-tasks/team-leads-13-clone_two_group_repos_linux.png)

Mit diesen Befehlen wird das Repository **GroupProjectTemplate** (R1) auf dem VSTS-Gruppenserver in das lokale Verzeichnis **GitRepos\GroupCommon** auf dem lokalen Computer geklont. Nach dem Klonen wird das Verzeichnis **GroupProjectTemplate** (D1) im Verzeichnis **GitRepos\GroupCommon** erstellt. Hier wird davon ausgegangen, dass Ihr Gruppenleiter das Teamprojekt **GroupCommon** erstellt hat und dass sich das Repository **GroupProjectTemplate** unter diesem Teamprojekt befindet. 


### <a name="clone-your-team-repositories-into-local-directories"></a>Klonen der Team-Repositorys in lokale Verzeichnisse

Diese Befehle klonen Ihre Repositorys **MyTeamProjectTemplate** (R3) und **MyTeamUtilities** (R4) unter dem Teamprojekt **MyTeam** auf dem VSTS-Gruppenserver in die Verzeichnisse **MyTeamProjectTemplate** (D3) und **MyTeamUtilities** (D4) in **GitRepos\MyTeam** auf Ihrem lokalen Computer. 

- Wechseln Sie zum Verzeichnis **GitRepos\MyTeam**.
- Führen Sie die folgenden Befehle im Betriebssystem des lokalen Computers aus. 

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamUtilities

![14](./media/team-lead-tasks/team-leads-14-clone_two_empty_team_repos.png)
        
**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamUtilities
    
![15](./media/team-lead-tasks/team-leads-15-clone_two_empty_team_repos_linux.png)

Nach dem Klonen werden die zwei Verzeichnisse **MyTeamProjectTemplate** (D3) und **MyTeamUtilities** (D4) im Verzeichnis **GitRepos\MyTeam** erstellt. Wir gehen hier davon aus, dass Sie Ihre Repositorys für Teamprojektvorlage und Hilfsprogramme **MyTeamProjectTemplate** bzw. **MyTeamUtilities** benannt haben. 

### <a name="copy-the-group-project-template-content-to-the-local-team-project-template-directory"></a>Kopieren des Inhalts der Gruppenprojektvorlage in das lokale Verzeichnis für die Teamprojektvorlage

Führen Sie zum Kopieren des Inhalts des lokalen Ordners **GroupProjectTemplate** (D1) in das lokale Verzeichnis **MyTeamProjectTemplate** (D3) eines der folgenden Shellskripts aus: 

####<a name="from-the-powershell-command-line-for-windows"></a>Von der PowerShell-Befehlszeile für Windows        

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 2

    
![16](./media/team-lead-tasks/team-leads-16-local_copy_team_lead_new.png)

####<a name="from-the-linux-shell-for-the-linux-dsvm"></a>Von der Linux-Shell für den **Linux-DSVM**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 2
    
![17](./media/team-lead-tasks/team-leads-17-local-copy-team-lead-linux-new.png)

Die Skripts schließen den Inhalt des Git-Verzeichnisses aus. Die Skripts fordern Sie auf, die **vollständigen Pfade** zum Quellverzeichnis D1 und zum Zielverzeichnis D3 anzugeben.
        

### <a name="customize-your-team-project-template-or-team-utilities-optional"></a>Anpassen der Teamprojektvorlage oder der Teamhilfsprogramme (optional)

Passen Sie in dieser Phase des Einrichtungsvorgangs **MyTeamProjectTemplate** (D3) und **MyTeamUtilities** (D4) an, sofern dies erforderlich ist. 

- Wenn Sie den Inhalt von D3 entsprechend den spezifischen Anforderungen Ihres Teams anpassen möchten, können Sie die Vorlagendokumente bearbeiten oder die Verzeichnisstruktur ändern.

- Wenn Ihr Team Hilfsprogramme entwickelt hat, die für das gesamte Team freigegeben werden sollen, kopieren Sie diese Hilfsprogramme, und fügen Sie sie im Verzeichnis D4 ein. 


### <a name="push-local-directory-content-to-team-repositories"></a>Übertragen des Inhalts des lokalen Verzeichnisses per Push in Team-Repositorys

Führen Sie zum Hinzufügen des Inhalts in den (optional angepassten) lokalen Verzeichnissen D3 und D4 zu den Team-Repositorys R3 und R4 die folgenden Git-Bash-Befehle von einer Windows PowerShell-Konsole oder von der Linux-Shell aus. Führen Sie Befehle aus dem Verzeichnis **GitRepos\MyTeam\MyTeamProjectTemplate** aus.

    git status
    git add .
    git commit -m"push from DSVM"
    git push
    
![18](./media/team-lead-tasks/team-leads-18-push-to-group-server-2.png)

Die Dateien im Repository „MyTeamProjectTemplate“ des VSTS-Servers Ihrer Gruppe werden beim Ausführen dieses Skripts nahezu sofort synchronisiert.

![19](./media/team-lead-tasks/team-leads-19-push-to-group-server-showed-up.png)

Führen Sie nun dieselben vier Git-Befehle aus dem Verzeichnis **GitRepos\MyTeam\MyTeamUtilities** aus. 

> [AZURE.NOTE]Wenn Sie erstmalig einen Commit in ein Git-Repository ausführen, müssen Sie die globalen Parameter *user.name* und *user.email* konfigurieren, bevor Sie den Befehl `git commit` ausführen. Führen Sie die folgenden beiden Befehle aus:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Wenn Sie einen Commit für mehrere Git-Repositorys durchführen, sollten Sie bei den einzelnen Commit-Vorgängen jeweils den gleichen Namen und die gleiche E-Mail-Adresse verwenden. Der gleiche Name und die gleiche E-Mail-Adresse stellen sich später als nützlich heraus, wenn Sie Power BI-Dashboards erstellen, um Ihre Git-Aktivitäten auf mehreren Repositorys nachzuverfolgen.

![20](./media/team-lead-tasks/team-leads-20-git-config-name.png)


## <a name="3-create-team-data-and-analytics-resources-optional"></a>3. Erstellen von Teamdaten und Analyseressourcen (optional)

Das gemeinsame Verwenden von Daten und Analyseressourcen im gesamten Team bringt Vorteile in Bezug auf Leistung und Kosten mit sich: Teammitglieder können ihre Projekte mit den gemeinsam genutzten Ressourcen ausführen, Budgeteinsparungen erzielen und effizienter zusammenarbeiten. In diesem Abschnitt finden Sie Anweisungen zum Erstellen eines Azure File Storage. Im nächsten Abschnitt erhalten Sie Anweisungen zum Bereitstellen des Azure File Storage auf Ihrem lokalen Computer. Weitere Informationen zum Freigeben weiterer Ressourcen, z.B. von virtuellen Azure Data Science-Computern oder Azure HDInsight-Spark-Clustern, finden Sie unter [Plattformen und Tools](platforms-and-tools.md). In diesem Thema erhalten Sie eine Anleitung aus Data Science-Perspektive zum Auswählen von geeigneten Ressourcen für Ihre jeweiligen Anforderungen. Zudem finden Sie hier Links zu Produktseiten und anderen relevanten und hilfreichen Tutorials, die wir veröffentlicht haben.

>[AZURE.NOTE] Um Datenübertragungen zwischen Rechenzentren zu vermeiden (dies kann ein sehr langsamer und kostspieliger Vorgang sein), müssen Sie sicherstellen, dass sich Ressourcengruppe, Speicherkonto und Azure-VM (z.B. DSVM) im selben Azure-Rechenzentrum befinden. 

Führen Sie die folgenden Skripts aus, um einen Azure File Storage für Ihr Team zu erstellen. Im Azure File Storage für Ihr Team können Sie Datenassets speichern, die für das gesamte Team nützlich sind. Die Skripts fragen Ihre Azure-Kontoinformationen und -Abonnementinformationen ab; halten Sie diese daher bereit für die Eingabe. 

### <a name="create-azure-file-storage-with-powershell-from-windows"></a>Erstellen eines Azure File Storage mit Windows PowerShell

Führen Sie dieses Skript von der PowerShell-Befehlszeile aus:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
    .\CreateFileShare.ps1

![21](./media/team-lead-tasks/team-leads-21-create-fileshare-win.png)   

Melden Sie sich bei einer entsprechenden Aufforderung bei Ihrem Microsoft Azure-Konto an:

![22](./media/team-lead-tasks/team-leads-22-file-create-s1.png)

Wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten:

![23](./media/team-lead-tasks/team-leads-23-file-create-s2.png)

Wählen Sie das zu verwendende Speicherkonto aus, oder erstellen Sie unter dem ausgewählten Abonnement ein neues Speicherkonto:

![24](./media/team-lead-tasks/team-leads-24-file-create-s3.png)

Geben Sie den Namen des zu erstellenden Azure File Storage ein. Zulässig sind nur Kleinbuchstaben, Ziffern und Bindestriche (-):

![25](./media/team-lead-tasks/team-leads-25-file-create-s4.png)

Um das Bereitstellen und Freigeben dieses Speichers nach seiner Erstellung zu ermöglichen, speichern Sie die Azure File Storage-Informationen in einer Textdatei, und notieren Sie sich den Pfad zum Speicherort. Sie benötigen diese Datei insbesondere zum Bereitstellen Ihres Azure File Storage auf den virtuellen Azure-Computern im nächsten Abschnitt. 

Es empfiehlt sich, diese Textdatei in das Repository „ProjectTemplate“ Ihres Teams einzuchecken. Wir raten, sie im Verzeichnis **Docs\DataDictionaries** zu speichern. So kann jedes Projekt im Team auf dieses Datenasset zugreifen. 

![26](./media/team-lead-tasks/team-leads-26-file-create-s5.png)


### <a name="create-azure-file-storage-with-a-linux-script"></a>Erstellen eines Azure File Storage mit einem Linux-Skript

Führen Sie dieses Skript von der Linux-Shell aus:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
    bash CreateFileShare.sh

Melden Sie sich bei Ihrem Microsoft Azure-Konto an; befolgen Sie dazu die auf diesem Bildschirm angezeigten Anweisungen:

![27](./media/team-lead-tasks/team-leads-27-file-create-linux-s1.png)

Wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten:

![28](./media/team-lead-tasks/team-leads-28-file-create-linux-s2.png)

Wählen Sie das zu verwendende Speicherkonto aus, oder erstellen Sie unter dem ausgewählten Abonnement ein neues Speicherkonto:

![29](./media/team-lead-tasks/team-leads-29-file-create-linux-s3.png)

Geben Sie den Namen des zu erstellenden Azure File Storage ein; im Namen sind nur Kleinbuchstaben, Ziffern und Bindestriche (-) zulässig:

![30](./media/team-lead-tasks/team-leads-30-file-create-linux-s4.png)

Um den Zugriff auf diesen Speicher nach seiner Erstellung zu ermöglichen, speichern Sie die Azure File Storage-Informationen in einer Textdatei, und notieren Sie sich den Pfad zum Speicherort. Sie benötigen diese Datei insbesondere zum Bereitstellen Ihres Azure File Storage auf den virtuellen Azure-Computern im nächsten Abschnitt.

Es empfiehlt sich, diese Textdatei in das Repository „ProjectTemplate“ Ihres Teams einzuchecken. Wir raten, sie im Verzeichnis **Docs\DataDictionaries** zu speichern. So kann jedes Projekt im Team auf dieses Datenasset zugreifen. 

![31](./media/team-lead-tasks/team-leads-31-file-create-linux-s5.png)


## <a name="4-mount-azure-file-storage-optional"></a>4. Bereitstellen von Azure File Storage (optional)

Nachdem der Azure File Storage erfolgreich erstellt wurde, kann er mit einem der folgenden PowerShell- bzw. Linux-Skripts auf Ihrem lokalen Computer bereitgestellt werden.

### <a name="mount-azure-file-storage-with-powershell-from-windows"></a>Bereitstellen des Azure File Storage mit Windows PowerShell

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
    .\AttachFileShare.ps1
    
Sofern Sie sich noch nicht angemeldet haben, werden Sie zunächst aufgefordert, sich anzumelden. 

Wenn gefragt werden, ob Sie über eine Datei mit Azure File Storage-Informationen verfügen, drücken Sie zum Fortfahren **Eingabe** oder **y**. Geben Sie dann den ***vollständigen Pfad und Namen** der im vorherigen Schritt erstellten Datei ein. Die Informationen zum Bereitstellen eines Azure File Storage werden direkt aus der Datei gelesen, und Sie können mit dem nächsten Schritt fortfahren.

![32](./media/team-lead-tasks/team-leads-32-attach-s1.png)

> [AZURE.NOTE] Wenn Sie über keine Datei mit Azure File Storage-Informationen verfügen, finden Sie die Schritte zum manuellen Eingeben der Informationen am Ende dieses Abschnitts.

Anschließend werden Sie aufgefordert, den Namen des Laufwerks einzugeben, das Ihrem virtuellen Computer hinzugefügt werden soll. Eine Liste der vorhandenen Laufwerknamen wird auf dem Bildschirm ausgegeben. Sie müssen einen Laufwerknamen angeben, der nicht bereits in der Liste vorhanden ist.

![33](./media/team-lead-tasks/team-leads-33-attach-s2.png)

Vergewissern Sie sich, dass ein neues Laufwerk „F“ erfolgreich auf dem Computer bereitgestellt wurde.

![34](./media/team-lead-tasks/team-leads-34-attach-s3.png)

**Manuelles Eingeben von Azure File Storage-Informationen:** Wenn Sie die Azure File Storage-Informationen nicht in einer Textdatei gespeichert haben, können Sie die Anweisungen im folgenden Bildschirm befolgen und die erforderlichen Informationen zu Abonnement, Speicherkonto und Azure File Storage einzugeben:

![35](./media/team-lead-tasks/team-leads-35-attach-s4.png)

Geben Sie den Namen Ihres Azure-Abonnements ein, wählen Sie das Speicherkonto aus, unter dem der Azure File Storage erstellt wird, und geben Sie den Azure File Storage-Namen ein:

![36](./media/team-lead-tasks/team-leads-36-attach-s5.png)

### <a name="mount-azure-file-storage-with-a-linux-script"></a>Bereitstellen eines Azure File Storage mit einem Linux-Skript

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
    bash AttachFileShare.sh

![37](./media/team-lead-tasks/team-leads-37-attach-s1-linux.png)

Sofern Sie sich noch nicht angemeldet haben, werden Sie zunächst aufgefordert, sich anzumelden. 

Wenn gefragt werden, ob Sie über eine Datei mit Azure File Storage-Informationen verfügen, drücken Sie zum Fortfahren **Eingabe** oder **y**. Geben Sie dann den ***vollständigen Pfad und Namen** der im vorherigen Schritt erstellten Datei ein. Die Informationen zum Bereitstellen eines Azure File Storage werden direkt aus der Datei gelesen, und Sie können mit dem nächsten Schritt fortfahren.

![38](./media/team-lead-tasks/team-leads-38-attach-s2-linux.png)

Anschließend werden Sie aufgefordert, den Namen des Laufwerks einzugeben, das Ihrem virtuellen Computer hinzugefügt werden soll. Eine Liste der vorhandenen Laufwerknamen wird auf dem Bildschirm ausgegeben. Sie müssen einen Laufwerknamen angeben, der nicht bereits in der Liste vorhanden ist.

![11,9](./media/team-lead-tasks/team-leads-39-attach-s3-linux.png)

Vergewissern Sie sich, dass ein neues Laufwerk „F“ erfolgreich auf dem Computer bereitgestellt wurde.

![40](./media/team-lead-tasks/team-leads-40-attach-s4-linux.png)

**Manuelles Eingeben von Azure File Storage-Informationen:** Wenn Sie die Azure File Storage-Informationen nicht in einer Textdatei gespeichert haben, können Sie die Anweisungen im folgenden Bildschirm befolgen und die erforderlichen Informationen zu Abonnement, Speicherkonto und Azure File Storage einzugeben:

- Geben Sie **n** ein.
- Wählen Sie den Index für den Namen des Abonnements aus, unter dem der Azure File Storage im vorherigen Schritt erstellt wurde:

    ![41](./media/team-lead-tasks/team-leads-41-attach-s5-linux.png)

- Wählen Sie das Speicherkonto unter Ihrem Abonnement aus, und geben Sie den Azure File Storage-Namen ein:

    ![42](./media/team-lead-tasks/team-leads-42-attach-s6-linux.png)

- Geben Sie den Namen des Laufwerks ein, das Ihrem Computer hinzugefügt werden soll. Dieser muss sich von vorhandenen Laufwerknamen unterscheiden:

    ![43](./media/team-lead-tasks/team-leads-43-attach-s7-linux.png)


## <a name="5-set-up-security-control-policy"></a>5. Einrichten der Sicherheitskontrollrichtlinie 

Klicken Sie auf der Homepage des VSTS-Servers rechts oben auf das **Zahnradsymbol** neben Ihrem Benutzernamen, und wählen Sie anschließend die Registerkarte **Sicherheit** aus. Hier können Sie Ihrem Team Mitglieder mit unterschiedlichen Berechtigungen hinzufügen.

![44](./media/team-lead-tasks/team-leads-44-add-team-members.png)

## <a name="next-steps"></a>Nächste Schritte

Es folgen Links zu den ausführlicheren Beschreibungen der Rollen und Aufgaben, die vom Team Data Science-Prozess definiert werden:

- [Group Manager tasks for a data science team (Aufgaben des Gruppenleiters eines Data Science-Teams)](group-manager-tasks.md)
- [Team Lead tasks for a data science team (Aufgaben des Teamleiters eines Data Science-Teams)](team-lead-tasks.md)
- [Project Lead tasks for a data science team (Aufgaben des Projektleiters eines Data Science-Teams)](project-lead-tasks.md)
- [Project Individual Contributors for a data science team (Einzelne Mitwirkende zu einem Projekt eines Data Science-Teams)](project-ic-tasks.md)