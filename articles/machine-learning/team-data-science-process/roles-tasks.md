---
title: "Rollen und Aufgaben im Team Data Science-Prozess – Azure | Microsoft-Dokumentation"
description: "Überblick über die Hauptkomponenten, Mitarbeiterrollen und zugeordneten Aufgaben in einem Data Science-Teamprojekt"
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
ms.openlocfilehash: b502a586cdb7351d5b22c6b0db966723b8ebb7b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="team-data-science-process-roles-and-tasks"></a>Rollen und Aufgaben im Team Data Science-Prozess

Der Team Data Science-Prozess (TDSP) ist ein von Microsoft entwickeltes Framework, das eine strukturierte Methodik zum effizienten Erstellen von Predictive Analytics-Lösungen und von intelligenten Anwendungen bereitstellt. In diesem Artikel finden Sie eine Übersicht über die wichtigsten Mitarbeiterrollen und zugeordneten Aufgaben in einem Data Science-Team, das diesen Prozess als Standard einsetzt. 

Diese Einführung enthält Links zu Tutorials, die Anweisungen zum Einrichten der TDSP-Umgebung für die gesamte Data Science-Gruppe, Data Science-Teams und Projekte bereitstellen. Es wird ausführlich beschrieben, wie Visual Studio Team Services (VSTS) in den Tutorials als Plattform zum Hosten von Code und als flexibles Planungstool zur Verwaltung von Teamaufgaben und Repositorys sowie zur Zugriffssteuerung verwendet wird. 

Mit diesen Informationen können Sie den TDSP auch auf Ihrer eigenen Codehostingplattform und Ihrem eigenen flexiblen Planungstool implementieren. 

## <a name="structures-of-data-science-groups-and-teams"></a>Aufbau von Data Science-Gruppen und -Teams
Data Science-Funktionen werden in Unternehmen häufig in der folgenden Hierarchie organisiert:

1. ***Data Science-Gruppe/n***

2. ***Data Science-Team/s innerhalb der Gruppe/n***

In einer solchen Hierarchie gibt es Gruppen- und Teamleiter. Ein Data Science-Projekt wird in der Regel von einem Data Science-Team durchgeführt, das häufig aus Projektleitern (zuständig für die Projektverwaltung und Governancetasks) sowie Datenanalysten oder Technikern (einzelne Mitwirkende/technische Mitarbeiter) besteht. Diese führen die Teile des Projekts mit Bezug zu Data Science und Data Engineering aus. Vor der Ausführung erfolgen Setup und Governance durch die Gruppen-, Team- oder Projektleiter.

## <a name="definition-of-four-tdsp-roles"></a>Definition von vier TDSP-Rollen
Basierend auf der oben genannten Annahme können nun vier verschiedene Rollen im Mitarbeiterteam definiert werden:

1. ***Gruppenleiter***: Der Gruppenleiter verwaltet die gesamte Data Science-Einheit in einem Unternehmen. Eine Data Science-Einheit kann aus mehreren Teams bestehen, von denen jedes an verschiedenen Data Science-Projekten in bestimmten Branchen arbeitet. Ein Gruppenleiter kann seine Aufgaben an einen Stellvertreter delegieren. Dadurch bleiben die der Rolle zugeordneten Aufgaben jedoch unverändert.

2. ***Teamleiter***: Der Teamleiter verwaltet ein Team in der Data Science-Einheit eines Unternehmens. Ein Team besteht aus mehreren Datenanalysten. Besteht die Data Science-Einheit nur aus einer kleinen Anzahl von Datenanalysten, können der Gruppenleiter und der Teamleiter dieselbe Person sein.

3. ***Projektleiter***: Der Projektleiter verwaltet die täglichen Aktivitäten der einzelnen Datenanalysten in einem bestimmten Data Science-Projekt.

4. ***Einzelne Projektmitwirkende***: Datenanalysten, Business Analysts, Datentechniker, Architekten usw. Ein einzelner Projektmitwirkender führt ein Data Science-Projekt aus. 


**[AZURE.NOTE]**: Je nach Aufbau eines Unternehmens kann eine einzelne Person mehrere Rollen oder können mehrere Personen eine Rolle übernehmen. Dies ist häufig in kleineren Unternehmen der Fall oder in Unternehmen mit einer kleinen Anzahl von Data Science-Mitarbeitern.

## <a name="tasks-to-be-completed-by-four-personnel"></a>Aufgaben dieser vier Mitarbeiterrollen

Die folgende Abbildung veranschaulicht die Aufgaben der obersten Ebene für die Mitarbeiterrollen in Bezug auf die Übernahme und Implementierung des Team Data Science-Prozesses von Microsoft. 

![Überblick über die Rollen und Aufgaben](./media/roles-tasks/overview-tdsp-top-level.png)

Dieses Schema und der folgende, detailliertere Überblick über die den einzelnen Rollen im TDSP zugewiesenen Aufgaben helfen Ihnen je nach Ihrer Zuständigkeit innerhalb der Organisation bei der Auswahl des geeigneten Tutorials.

>[AZURE.NOTE] In den folgenden Anleitungen erfahren Sie, wie Sie eine TDSP-Umgebung einrichten und andere Data Science-Aufgaben in Visual Studio Team Services (VSTS) ausführen. Die Erläuterungen beziehen sich auf VSTS, da bei Microsoft auf diese Weise der TDSP implementiert wird. VSTS erleichtert die Zusammenarbeit, indem die Verwaltung von Arbeitselementen zur Nachverfolgung von Aufgaben sowie ein Codehostingdienst integriert sind, der die Freigabe von Dienstprogrammen, die Organisation der Versionen und die Bereitstellung von rollenbasierter Sicherheit ermöglicht. Sie können die hier beschriebenen Aufgaben im TDSP bei Bedarf auch mit einer anderen Plattform implementieren. Je nach Plattform sind in diesem Fall jedoch möglicherweise nicht alle Funktionen von VSTS verfügbar. 
>
>Außerdem wird der [virtuelle Data Science-Computer (DSVM)](http://aka.ms/dsvm) als Analysedesktop in der Azure-Cloud verwendet, der über mehrere beliebte vorkonfigurierte Data Science-Tools verfügt und in verschiedenen Azure-Diensten und Microsoft-Software integriert ist. Sie können den DSVM oder eine beliebige Entwicklungsumgebung verwenden, um den TDSP zu implementieren. 


## <a name="group-manager-tasks"></a>Aufgaben des Gruppenleiters

Die folgenden Aufgaben werden vom Gruppenleiter (oder einem festgelegten TDSP-Systemadministrator) ausgeführt, um den TDSP zu übernehmen:

- Erstellen Sie ein **Gruppenkonto** auf einer Codehostingplattform (z.B. Github, Git, VSTS usw.).
- Erstellen Sie im Gruppenkonto ein **Projektvorlagenrepository**. Führen Sie dafür ein Seeding aus dem vom TDSP-Team von Microsoft entwickelten Projektvorlagenrepository aus. Das Vorlagenrepository für das TDSP-Projekt von Microsoft stellt eine **standardisierte Verzeichnisstruktur** mit Verzeichnissen für Daten, Code und Dokumente sowie eine Gruppe von **standardisierten Dokumentvorlagen** für einen effizienten Data Science-Prozess bereit. 
- Erstellen Sie ein **Dienstprogrammrepository**, und führen Sie dafür ein Seeding aus dem vom TDSP-Team von Microsoft entwickelten Dienstprogrammrepository aus. Das TDSP-Dienstprogrammrepository von Microsoft stellte eine Reihe nützlicher Dienstprogramme bereit, mit denen Datenanalysten effizienter arbeiten können. Es enthält u.a. Dienstprogramme für die interaktive Durchsuchung, Analyse und Berichterstellung von Daten sowie für die Basismodellierung und Grundlinienberichterstellung.
- Richten Sie die **Richtlinie für Sicherheitskontrollen** dieser beiden Repositorys in Ihrem Gruppenkonto ein.  

Ausführliche Anleitungen finden Sie unter [Aufgaben des Gruppenleiters eines Data Science-Teams](group-manager-tasks.md). 


## <a name="team-lead-tasks"></a>Aufgaben des Teamleiters

Die folgenden Aufgaben werden vom Teamleiter (oder einem festgelegten Teamprojektadministrator) ausgeführt, um den TDSP zu übernehmen:

- Wenn Sie VSTS als Codehostingplattform für die Versionsverwaltung und Zusammenarbeit ausgewählt haben, erstellen Sie auf dem VSTS-Gruppenserver ein **Teamprojekt**. Andernfalls können Sie diese Aufgabe überspringen.
- Erstellen Sie im Teamprojekt ein **Vorlagenrepository für das Teamprojekt**. Führen Sie dafür ein Seeding aus dem Vorlagenrepository für das Gruppenprojekt aus, das vom Gruppenleiter oder seinem Stellvertreter eingerichtet wurde. 
- Erstellen Sie das **Dienstprogrammrepository für das Team**, und fügen Sie dem Repository die teamspezifischen Dienstprogramme hinzu. 
- Optional: Erstellen Sie einen **[Azure-Dateispeicher](https://azure.microsoft.com/services/storage/files/)** zum Speichern von Datenassets, die für das gesamte Team nützlich sein können. Die anderen Teammitglieder können diesen freigegebenen Cloud-Dateispeicher auf ihren Analysedesktops bereitstellen.
- Optional: Stellen Sie den Azure-Dateispeicher auf dem **virtuellen Data Science-Computer** (DSVM) des Teamleiters bereit, und fügen Sie Datenassets hinzu.
- Richten Sie **Sicherheitskontrollen** ein, indem Sie Teammitglieder hinzufügen und ihre Berechtigungen konfigurieren. 

Ausführliche Anleitungen finden Sie unter [Aufgaben des Teamleiters eines Data Science-Teams](team-lead-tasks.md).  


## <a name="project-lead-tasks"></a>Aufgaben des Projektleiters

Die folgenden Aufgaben werden vom Projektleiter ausgeführt, um den TDSP zu übernehmen:

- Erstellen Sie im Teamprojekt ein **Projektrepository**, und führen Sie dafür ein Seeding aus dem Vorlagenrepository für das Teamprojekt aus. 
- Optional: Erstellen Sie einen **Azure-Dateispeicher** zum Speichern von Projektdatenassets. 
- Optional: Stellen Sie den Azure-Dateispeicher auf dem **virtuellen Data Science-Computer** (DSVM) des Projektleiters bereit, und fügen Sie Projektdatenassets hinzu.
- Richten Sie **Sicherheitskontrollen** ein, indem Sie Projektmitglieder hinzufügen und ihre Berechtigungen konfigurieren. 

Ausführliche Anleitungen finden Sie unter [Aufgaben des Projektleiters eines Data Science-Teams](project-lead-tasks.md). 

## <a name="project-individual-contributor-tasks"></a>Aufgaben einzelner Projektmitwirkender

Die folgenden Aufgaben werden von einzelnen Projektmitwirkenden (in der Regel Datenanalysten) ausgeführt, um ein Data Science-Projekt mit dem TDSP durchzuführen:

- Klonen Sie das **Projektrepository**, das vom Projektleiter eingerichtet wurde. 
- Optional: Stellen Sie den freigegebenen **Azure-Dateispeicher** des Teams und des Projekts auf dem **virtuellen Data Science-Computer** (DSVM) bereit.
- Führen Sie das Projekt aus. 

 
Ausführliche Anleitungen für die Integration in ein Projekt finden Sie unter [Einzelne Projektmitwirkende eines Data ScienceTeams](project-ic-tasks.md). 


## <a name="data-science-project-execution"></a>Ausführung des Data Science-Projekts
 
Mit diesen relevanten Anweisungen können Datenanalysten, Projektleiter und Teamleiter Arbeitselemente erstellen, um alle Aufgaben und Phasen innerhalb eines Projekts von Anfang bis Ende nachzuverfolgen. Mit Git wird die Zusammenarbeit der Datenanalysten gefördert. Außerdem wird sichergestellt, dass die während der Ausführung des Projekts entstandenen Artefakte mit Versionskontrolle ausgeführt und von allen Projektmitgliedern gemeinsam verwendet werden.

Die Anweisungen für die Ausführung des Projekts wurden auf der Annahme entwickelt, dass sich sowohl die Arbeitselemente als auch die Git-Repositorys des Projekts auf VSTS befinden. Wenn in beiden Fällen VSTS verwendet wird, können Sie die Arbeitselemente und Git-Verzweigungen Ihres Projektrepositorys miteinander verknüpfen. Auf diese Weise können Sie leicht nachverfolgen, wie weit ein Arbeitselement bereits fortgeschritten ist. 

Die folgende Abbildung zeigt den Workflow der Projektausführung mit dem TDSP:

![Typische Ausführung eines Data Science-Projekts](./media/roles-tasks/overview-project-execute.png)

Die Schritte des Workflows können in drei Aktivitäten unterteilt werden:

- Sprintplanung (Projektleiter)
- Entwicklung von Artefakten in Git-Verzweigungen, um Arbeitselemente zu bearbeiten (Datenanalysten)
- Codeüberprüfung und Zusammenführung von Verzweigungen und Hauptverzweigungen (Projektleiter oder andere Teammitglieder)

Ausführliche Anleitungen zum Workflow der Projektausführung finden Sie unter [Ausführung von Data Science-Projekten](project-execution.md).

## <a name="next-steps"></a>Nächste Schritte

Es folgen Links zu den ausführlicheren Beschreibungen der Rollen und Aufgaben, die vom Team Data Science-Prozess definiert werden:

- [Group Manager tasks for a data science team (Aufgaben des Gruppenleiters eines Data Science-Teams)](group-manager-tasks.md)
- [Team Lead tasks for a data science team (Aufgaben des Teamleiters eines Data Science-Teams)](team-lead-tasks.md)
- [Project Lead tasks for a data science team (Aufgaben des Projektleiters eines Data Science-Teams)](project-lead-tasks.md)
- [Project Individual Contributors for a data science team (Einzelne Mitwirkende zu einem Projekt eines Data Science-Teams)](project-ic-tasks.md)