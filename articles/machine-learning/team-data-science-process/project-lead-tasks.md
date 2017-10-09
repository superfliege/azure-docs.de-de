---
title: "Aufgaben des Projektleiters eines Team Data Science-Prozesses – Azure | Microsoft-Dokumentation"
description: "Ein Überblick über die Aufgaben eines Projektleiters an einem Data Science-Teamprojekt."
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
ms.date: 09/04/2017
ms.author: bradsev;
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ed3dc8d441989239f02e12231f06d22fbef9d3dd
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="project-lead-tasks"></a>Aufgaben eines Projektleiters

Dieses Tutorial beschreibt die Aufgaben, die ein Projektleiter für sein/ihr Projektteam abschließen muss. Ziel ist es, eine auf Zusammenarbeit ausgelegte Teamumgebung einzurichten, die gemäß dem [Team Data Science-Prozess (TDSP)](overview.md) vereinheitlicht ist. TDSP ist ein von Microsoft entwickeltes Framework, das eine strukturierte Abfolge von Aktivitäten bereitstellt, die cloudbasierte, prädiktive Analyselösungen effizient ausführt. Unter [Team Data Science Process: Roles and tasks](roles-tasks.md) (Team Data Science-Prozess: Rollen und Aufgaben) finden Sie eine Übersicht über die wichtigsten Mitarbeiterrollen und zugeordneten Aufgaben für ein Data Science-Team, das eine Vereinheitlichung gemäß diesem Prozess anstrebt.

Ein **Projektleiter** verwaltet die täglichen Aktivitäten der einzelnen Datenanalysten auf einem bestimmten Data Science-Projekt. Der Workflow für die Aufgaben, die von Projektleitern zum Einrichten dieser Umgebung ausgeführt werden, sind in der folgenden Abbildung dargestellt:

![1](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Dieses Thema enthält derzeit die Aufgaben 1,2 und 6 dieses Workflows für Projektleiter.

>[AZURE.NOTE] Wir beschreiben in den folgenden Anweisungen die erforderlichen Schritte zum Einrichten einer TDSP-Teamumgebung für ein Projekt mithilfe von Visual Studio Team Services (VSTS). Wir geben an, wie diese Aufgaben mit VSTS erledigt werden können, da wir bei Microsoft TDSP auf diese Weise implementieren. Wenn für Ihre Gruppe eine andere Plattform zum Hosten des Codes verwendet wird, ändern sich die vom Teamleiter auszuführenden Aufgaben in der Regel nicht. Sie müssen jedoch auf andere Weise ausgeführt werden.


## <a name="repositories-and-directories"></a>Repositorys und Verzeichnisse

In diesem Tutorial werden abgekürzte Namen für Repositorys und Verzeichnisse verwendet. Diese Namen erleichtern es, den Vorgängen zwischen den Repositorys und Verzeichnissen zu folgen. Diese Notation (R für Git-Repositorys und D für lokale Verzeichnisse auf Ihrer DSVM) wird in den folgenden Abschnitten verwendet:

- **R3**: Das Team-Repository **ProjectTemplate** auf Git, das Ihr Teamleiter eingerichtet hat.
- **R5**: Das Projekt-Repository auf Git, das Sie für Ihr Projekt eingerichtet haben.
- **D3**: Das von R3 geklonte lokale Verzeichnis.
- **D5**: Das von R5 geklonte lokale Verzeichnis.


## <a name="0-prerequisites"></a>0. Voraussetzungen

Zum Erfüllen der Voraussetzungen müssen die dem Gruppenleiter und dem Teamleiter zugewiesenen Aufgaben ausgeführt werden. Siehe dazu [Group Manager tasks for a data science team (Aufgaben des Gruppenleiters eines Data Science-Teams)](group-manager-tasks.md) und [Team lead tasks for a data science team (Aufgaben des Teamleiters eines Data Science-Teams)](team-lead-tasks.md). 

Kurz zusammengefasst müssen die folgenden Anforderungen erfüllt sein, ehe Sie mit den Aufgaben des Teamleiters beginnen: 

- Ihr **VSTS-Gruppenserver** (oder Gruppenkonto für eine andere Codehostingplattform) wurde von Ihrem Gruppenmanager eingerichtet.
- Ihr **TeamProjectTemplate-Repository** (R3) wurde unter Ihrem Gruppenkonto von Ihrem Teamleiter auf der Plattform zum Hosten von Code, die Sie verwenden möchten, eingerichtet .
- Sie wurden von Ihrem Teamleiter zur Erstellung von Repositorys auf Ihrem Gruppenkonto für Ihr Team **autorisiert**.
- Git muss auf Ihrem Computer installiert sein. Wenn Sie eine Data Science Virtual Machine (DSVM) verwenden, ist Git bereits installiert, sodass Sie gleich loslegen können. Siehe andernfalls den Anhang [Plattformen und Tools](platforms-and-tools.md#appendix).  
- Bei Verwendung einer **Windows-DSVM** muss [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) auf Ihrem Computer installiert sein. Scrollen Sie in der Datei „README.md“ nach unten zum Abschnitt **Herunterladen und installieren**, und klicken Sie auf den *Neuesten Installer*. Dadurch gelangen Sie zur Seite mit dem neuesten Installer. Laden Sie den Installer mit der Erweiterung EXE herunter, und führen Sie ihn aus. 
- Bei Verwendung einer **Linux-DSVM** erstellen Sie auf Ihrer DSVM einen öffentlichen SSH-Schlüssel, den Sie dem VSTS-Server Ihrer Gruppe hinzufügen. Weitere Informationen zu SSH finden Sie im Anhang [Plattformen und Tools](platforms-and-tools.md#appendix) im Abschnitt **Erstellen eines öffentlichen SSH-Schlüssels**. 


## <a name="1-create-a-project-repository-r5"></a>1. Erstellen Sie ein Projekt-Repository (R5)

- Melden Sie sich mit Ihrem VSTS-Gruppenserver unter *https://\<VSTS-Servernamen\>. visualstudio.com* an. 
- Klicken Sie unter **Zuletzt geöffnete Projekte und Teams** auf **Durchsuchen**. Ein eingeblendetes Fenster listet alle Teamprojekte auf dem VSTS-Server auf. 

    ![2](./media/project-lead-tasks/project-leads-2-create-project-repo.png)

- Klicken Sie auf den Namen des Teamprojekts, in dem Sie Ihr Projektrepository erstellen möchten. Klicken Sie in diesem Beispiel auf **MyTeam**. 
- Klicken Sie dann auf **Navigieren**, um zur Startseite des Teamprojekts **MyTeam** zu gelangen:

    ![3](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)

- Klicken Sie auf **Zusammenarbeit auf Code**, um zur Git-Startseite des Teamprojekts zu gelangen.  

    ![4](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

- Klicken Sie auf den nach unten weisenden Pfeil in der oberen linken Ecke, und wählen Sie **+ Neues Repository** aus. 
    
    ![5](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)

- Geben Sie im Fenster **Ein neues Repository erstellen** einen Namen für Ihr Git-Projektrepository ein. Stellen Sie sicher, dass Sie **Git** als Typ des Repositorys auswählen. In diesem Beispiel verwenden wir den Namen *DSProject1*. 

    ![6](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

- Klicken Sie zum Erstellen Ihres Git-Projektrepositorys ***DSProject1*** auf **Erstellen**.


## <a name="2-seed-the-dsproject1-project-repository"></a>2. Seeding für das Projektrepository DSProject1 ausführen

Die Aufgabe besteht darin, Seeding für das Projektrepository (R5) **DSProject1** von Ihrem Repository für Teamprojektvorlagen (R3) auszuführen. Das Seeding-Verfahren verwendet die Verzeichnisse D3 und D5 auf Ihrem lokalen DSVM als fortgeschrittene Stagingsites. Zusammenfassend ist der Seeding-Pfad: R3 -> D3 -> D5 -> R5.

Wenn Sie Ihr Projektrepository **DSProject1** anpassen möchten, um einige bestimmte Projektbedürfnisse zu erfüllen, müssen Sie dies im vorletzten Schritt des Verfahrens tun. Hier sehen Sie eine Zusammenfassung der Schritte, durch die der Inhalt des Projektrepositorys **DSProject1** angelegt wird. Die einzelnen Schritte entsprechen den Unterabschnitten im Seeding-Verfahren:

- Klonen Sie das Vorlagenrepository für Teamprojekte zum lokalen Verzeichnis: Team R3 - geklont zu -> lokale D3.
- Klonen Sie das Repository DSProject1 in ein lokales Verzeichnis: Team R5 - geklont zu -> lokale D5.
- Kopieren Sie die Inhalte der geklonten Teamprojektvorlagen in den lokalen Klon des Repositorys DSProject1: D3 - Inhalte kopiert zu -> D5.
- (Optional) Anpassung der lokalen D5.
- Übertragen Sie den Inhalt des lokalen DSProject1 mithilfe von Push in die TeamRepositorys: D5 - Inhalt zugefügt zu -> Team R5.


### <a name="clone-your-team-project-template-repository-r3-to-a-directory-d3-on-your-local-machine"></a>Klonen Sie das Vorlagenrepository Ihres Teamprojekts (R3) in ein Verzeichnis (D3) auf dem lokalen Computer.

Erstellen Sie ein Verzeichnis auf dem lokalen Computer:

- *C:\GitRepos\MyTeamCommon* für Windows 
- *$home/GitRepos/MyTeamCommon* für Linux

Navigieren Sie in dieses Verzeichnis. Führen Sie dann den folgenden Befehl aus, um das Vorlagenrepository Ihres Teamprojekts auf Ihren lokalen Computer zu klonen. 

**Windows**
            
    git clone <the HTTPS URL of the TeamProjectTemplate repository>
    
Wenn Sie VSTS in der Regel als Codehostingplattform verwenden, lautet die *HTTPS-URL des Vorlagenrepositorys Ihres Teamprojekts*:

 ***https://\<VSTS-Servername\>.visualstudio.com/\<Ihr Teamprojektname\>/_git/\<Name der Vorlagenrepository Ihres Teamprojekts\>***. 

In diesem Beispiel sehen wir:

***https://mysamplegroup.visualstudio.com/MyTeam/_git/MyTeamProjectTemplate***. 

![7](./media/project-lead-tasks/project-leads-7-clone-team-project-template.png)
            
**Linux**

    git clone <the SSH URL of the TeamProjectTemplate repository>
        
![8](./media/project-lead-tasks/project-leads-8-clone-team-project-template-linux.png)

Wenn Sie VSTS in der Regel als Codehostingplattform verwenden, lautet die *SSH-URL des Vorlagenrepositorys des Teamprojekts*:

***ssh://\<VSTS-Servername\>@\<VSTS-Servername\>.visualstudio.com:22/\<Ihr Teamprojektname>/_git/\<Name der Vorlagenrepository Ihres Teamprojekts\>.*** 

In diesem Beispiel sehen wir:

***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/MyTeamProjectTemplate***. 

### <a name="clone-dsproject1-repository-r5-to-a-directory-d5-on-your-local-machine"></a>Klonen Sie das Repository DSProject1 (R5) in ein Verzeichnis (D5) auf dem lokalen Computer

Navigieren Sie zum Verzeichnis **GitRepos**, und führen Sie dann den folgenden Befehl aus, um das Projektrepository auf Ihren lokalen Computer zu klonen. 

**Windows**
            
    git clone <the HTTPS URL of the Project repository>

![9](./media/project-lead-tasks/project-leads-9-clone-project-repository.png)

Wenn Sie VSTS in der Regel als Codehostingplattform verwenden, lautet die _HTTPS-URL des Projektrepositorys_ ***https://\<VSTS-Servername\>.visualstudio.com/\<Ihr Teamprojektname>/_git/<Ihr Projektrepositoryname\>***. In diesem Beispiel haben wir ***https://mysamplegroup.visualstudio.com/MyTeam/_git/DSProject1***.

**Linux**

    git clone <the SSH URL of the Project repository>

![10](./media/project-lead-tasks/project-leads-10-clone-project-repository-linux.png)

Wenn Sie VSTS in der Regel als Codehostingplattform verwenden, lautet die _SSH-URL des Projektrepositorys_ _ssh ist://<VSTS-Servername\>@<VSTS-Servername\>.visualstudio.com:22/<Your Team Project Name>/\_git/<Ihr Projektrepositoryname\>. In diesem Beispiel haben wir ***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/DSProject1***.

### <a name="copy-contents-of-d3-to-d5"></a>Kopieren Sie Inhalte von D3 auf D5 

Sie müssen nun in Ihrem lokalen Computer den Inhalt von _D3_ nach _D5_ kopieren, mit Ausnahme der Git-Metadaten im .git-Verzeichnis. Die folgenden Skripts werden die Aufgabe erledigen. Stellen Sie sicher, dass Sie die richtigen und vollständigen Pfade in die Verzeichnisse eingeben. Der Quellordner ist für Ihr Team (_D3_); der Zielordner ist für Ihr Projekt bestimmt (_D5_).    

**Windows**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 -role 3
    
![11](./media/project-lead-tasks/project-leads-11-local-copy-project-lead-new.png)

Nun können Sie im Ordner _DSProject1_ alle Dateien (mit Ausnahme der .git-Dateien) sehen, die von _MyTeamProjectTemplate_ kopiert wurden.

![12](./media/project-lead-tasks/project-leads-12-teamprojectTemplate_copied_to_local.png)

**Linux**
            
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 3
        
![13](./media/project-lead-tasks/project-leads-13-local_copy_project_lead_linux_new.png)

Nun können Sie im Ordner _DSProject1_ alle Dateien (mit Ausnahme der Metadaten der .git-Dateien) sehen, die von _MyTeamProjectTemplate_ kopiert wurden.

![14](./media/project-lead-tasks/project-leads-14-teamprojectTemplate_copied_to_local_linux_new.png)


### <a name="customize-d5-if-you-need-to-optional"></a>Anpassen von D5 bei Bedarf (optional)

Wenn das Projekt einige bestimmte Verzeichnisse oder Dokumente erfordert, die Sie nicht aus Ihren Teamprojektvorlagen (im vorherigen Schritt in Ihr D5-Verzeichnis kopiert) erhalten, können Sie den Inhalt von D5 jetzt anpassen. 

### <a name="add-contents-of-dsproject1-in-d5-to-r5-on-your-group-vsts-server"></a>Fügen Sie Inhalte von DSProject1 in D5 zu R5 auf Ihrem VSTS-Gruppenserver hinzu

Jetzt müssen Sie Inhalte in **_DSProject1_** mithilfe von Push auf das _R5_-Repository im Teamprojekt Ihres VSTS-Gruppenserver übertragen. 


- Wechseln Sie zum Verzeichnis **D5**. 
- Verwenden Sie die folgenden Git-Befehle zum Hinzufügen von Inhalten in **D5** auf **R5**. Die Befehle sind für Windows- und Linux-Systeme identisch. 
    
    git status git add .
    git commit -m"push from win DSVM" git push
    
- Führen Sie ein Commit der Änderungen und Push durch. 

>[AZURE.NOTE] Wenn dies das erste Mal ist, dass Sie ein Commit in einem Git-Repository ausführen, müssen Sie globale Parameter wie *user.name* und *user.email* vor dem Ausführen des `git commit`-Befehls konfigurieren. Führen Sie die folgenden zwei Befehle aus:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Wenn Sie mit mehreren Git-Repositorys ein Commit ausführen, verwenden Sie den gleichen Namen und die gleiche E-Mail-Adresse für alle. Der gleiche Name und die gleiche E-Mail-Adresse stellen sich später als nützlich heraus, wenn Sie Power BI-Dashboards erstellen, um Ihre Git-Aktivitäten auf mehreren Repositorys nachzuverfolgen.

![15](./media/project-lead-tasks/project-leads-15-git-config-name.png)


## <a name="6-create-and-mount-azure-file-storage-as-project-resources-optional"></a>6. Erstellen Sie den Azure-Dateispeicher und stellen Sie ihn als Projektressourcen bereit (Optional)

Wenn Sie einen Azure-Dateispeicher zur Datenfreigabe von z.B. Projektrohdaten oder für Ihr Projekt erstellte Funktionen erstellen möchten, sodass alle Projektmitglieder über Zugang zu den gleichen Datasets aus mehreren DSVMs verfügen, dann befolgen Sie die Anweisungen in den Abschnitten 3 und 4 der [Team Lead tasks for a data science team (Aufgaben des Teamleiters eines Data Science-Teams)](team-lead-tasks.md). 


## <a name="next-steps"></a>Nächste Schritte

Nachfolgend finden Sie Links zu den ausführlicheren Beschreibungen der Rollen und Aufgaben, die vom Team Data Science-Prozess definiert werden:

- [Group Manager tasks for a data science team (Aufgaben des Gruppenleiters eines Data Science-Teams)](group-manager-tasks.md)
- [Team Lead tasks for a data science team (Aufgaben des Teamleiters eines Data Science-Teams)](team-lead-tasks.md)
- [Project Lead tasks for a data science team (Aufgaben des Projektleiters eines Data Science-Teams)](project-lead-tasks.md)
- [Project Individual Contributors for a data science team (Einzelne Mitwirkende zu einem Projekt eines Data Science-Teams)](project-ic-tasks.md)
