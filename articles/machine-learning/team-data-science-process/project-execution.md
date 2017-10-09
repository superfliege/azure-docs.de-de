---
title: "Ausführung von Data Science-Projekten – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie ein Datenanalyst ein Data Science-Projekt auf nachverfolgbare Weise mit Versionskontrolle und Zusammenarbeit ausführen kann."
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
ms.openlocfilehash: 8c318f87243d0c98b6a42bebcdffb433f9cc456e
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="execution-of-data-science-projects"></a>Ausführung von Data Science-Projekten

Dieses Dokument beschreibt, wie Datenanalysten ein Data Science-Projekt auf systematische Weise mit Versionskontrolle und Zusammenarbeit innerhalb eines Projektteams über den [Team Data Science-Prozess](overview.md) (TDSP) ausführen können. Der TDSP ist ein von Microsoft entwickeltes Framework, das eine strukturierte Abfolge von Aktivitäten für die effiziente Ausführung cloudbasierter Predictive Analytics-Lösungen bereitstellt. Unter [Rollen und Aufgaben im Team Data Science-Prozess](roles-tasks.md) finden Sie eine Übersicht über die wichtigsten Mitarbeiterrollen und zugeordneten Aufgaben für ein Data Science-Team, das eine Vereinheitlichung gemäß diesem Prozess anstrebt. 

Dieses Thema enthält Anweisungen zu folgenden Aufgaben: 

1. Durchführen einer **Sprintplanung** für Arbeitsaufgaben in einem Projekt.<br> Wenn Sie mit der Sprintplanung nicht vertraut sind, finden Sie allgemeine Informationen weiter unten und Details [hier](https://en.wikipedia.org/wiki/Sprint_(software_development) "hier"). 
2. **Hinzufügen von Arbeitselementen** zu Sprints.
3. **Verknüpfen von Arbeitsaufgaben mit Codierungsaktivitäten**, die von Git nachverfolgt werden.
4. Durchführen einer **Codeüberprüfung**. 


>[AZURE.NOTE] Wir beschreiben in den folgenden Anweisungen die erforderlichen Schritte zum Einrichten einer TDSP-Teamumgebung mithilfe von Visual Studio Team Services (VSTS). Wir geben an, wie diese Aufgaben mit VSTS erledigt werden können, da wir bei Microsoft den TDSP auf diese Weise implementieren. Die Punkte (3) und (4) in der vorherigen Liste sind Vorteile, die Sie mit VSTS immer erhalten. Wenn für Ihre Gruppe eine andere Plattform zum Hosten des Codes verwendet wird, ändern sich die vom Teamleiter auszuführenden Aufgaben in der Regel nicht. Sie müssen aber auf andere Weise ausgeführt werden. Beispielsweise lässt sich der Aufzählungspunkt **Verknüpfen einer Arbeitsaufgabe mit einer Git-Verzweigung** in Abschnitt 6 möglicherweise nicht so einfach umsetzen wie mit VSTS.

Die folgende Abbildung veranschaulicht einen typischen Workflow mit Sprintplanung, Codierung und Quellcodeverwaltung für die Implementierung eines Data Science-Projekts:

![1](./media/project-execution/project-execution-1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminologie 

Im TDSP-Sprintplanungsframework gibt es vier häufig verwendete Typen von **Arbeitselementen**: **Feature**, **User Story**, **Aufgabe** und **Fehler**. Jedes Teamprojekt verwaltet ein einzelnes Backlog für alle Arbeitselemente. Es gibt kein Backlog auf Git-Repositoryebene unter einem Teamprojekt. Hier sind die Definitionen:

- **Feature:** Ein Feature entspricht einem Projektauftrag. Andere Aufträge eines Kunden werden als unterschiedliche Features betrachtet. Ebenso werden verschiedene Phasen eines Projekts bei einem Kunden am besten als unterschiedliche Features betrachtet. Wenn Sie zum Benennen Ihrer Features ein Schema wie ***Kundenname-Auftragsname*** verwenden, können Sie den Kontext des Projekts/Auftrags dem Namen entnehmen.
- **Story:** Storys sind unterschiedliche Arbeitselemente, die zum vollständigen Abschließen eines Features (Projekts) erforderlich sind. Beispiele für Storys sind:
    - Abrufen von Daten 
    - Durchsuchen von Daten 
    - Generieren von Features
    - Erstellen von Modellen
    - Operationalisieren von Modellen 
    - Erneutes Trainieren von Modellen
- **Aufgabe:** Aufgaben sind zuweisbare Code- oder Dokumentarbeitselemente oder andere Aktivitäten, die zum Abschließen einer bestimmten Story abgeschlossen werden müssen. Beispiele für Aufgaben in der Story *Abrufen von Daten* sind:
    -  Abrufen von Anmeldeinformationen von SQL Server 
    -  Hochladen von Daten in SQL Data Warehouse 
- **Fehler:** Als Fehler werden in der Regel Korrekturen bezeichnet, die für einen vorhandenen Code oder ein Dokument erforderlich sind und zum Abschließen einer Aufgabe ausgeführt werden. Dies kann bis zu einer Story oder Aufgabe eskaliert werden, wenn der Fehler durch fehlende Phasen oder Aufgaben verursacht wird. 

>[AZURE.NOTE] Wir entlehnen die Konzepte der Features, Storys, Aufgaben und Fehler aus der Softwarecodeverwaltung (Software Code Management, SCM) für die Verwendung in Data Science. Sie können von den herkömmlichen SCM-Definitionen leicht abweichen.

##  2. <a name='SprintPlanning-2'></a>Sprintplanung 

Die Sprintplanung ist für die Projektpriorisierung sowie die Ressourcenplanung und -zuordnung nützlich. Viele Datenanalysten sind mit mehreren Projekten beschäftigt, die bis zum Abschluss mehrere Monate dauern können. Projekte schreiten häufig in verschiedenen Geschwindigkeiten voran. Auf dem VSTS-Server können Sie problemlos Arbeitselemente in Ihrem Teamprojekt erstellen, verwalten und nachverfolgen und eine Sprintplanung durchführen, um sicherzustellen, dass Ihre Projekte sich wie erwartet entwickeln. 

Unter [diesem Link](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) finden Sie eine ausführliche Anleitung zur Sprintplanung in VSTS. 


##  3. <a name='AddFeature-3'></a>Hinzufügen eines Features  

Nachdem Ihr Projektrepository in einem Teamprojekt erstellt wurde, wechseln Sie zur Seite **Übersicht** des Teams, und klicken Sie auf **Aufgaben verwalten**.

![2](./media/project-execution/project-execution-2-sprint-team-overview.png)

Um ein Feature in das Backlog einzuschließen, klicken Sie auf **Backlogs** --> **Features** --> **Neu**, geben Sie den **Titel** des Features (in der Regel den Projektnamen) ein, und klicken Sie dann auf **Hinzufügen**.

![3](./media/project-execution/project-execution-3-sprint-team-add-work.png)

Doppelklicken Sie auf das Feature, das Sie soeben erstellt haben. Geben Sie die Beschreibungen ein, weisen Sie Teammitglieder für dieses Feature zu, und legen Sie die Planungsparameter für dieses Feature fest. 

Sie können dieses Feature auch mit dem Projektrepository verknüpfen. Klicken Sie auf **Link hinzufügen** im Abschnitt **Entwicklung**. Nachdem Sie mit dem Bearbeiten des Features fertig sind, klicken Sie auf **Speichern und schließen**, um die Bearbeitung zu beenden.


##  4. <a name='AddStoryunderfeature-4'></a>Hinzufügen einer Story unter einem Feature 

Unter dem Feature können Storys hinzugefügt werden, um wichtige erforderliche Schritte zum Abschließen des Projekts (Features) zu beschreiben. Um eine neue Story hinzuzufügen, klicken Sie auf das Symbol **+** neben dem Feature in der Backlogansicht.  

![4](./media/project-execution/project-execution-4-sprint-add-story.png)

Sie können die Details der Story im Popupfenster bearbeiten, z.B. Status, Beschreibung, Kommentare, Planung und Priorität.

![5](./media/project-execution/project-execution-5-sprint-edit-story.png)

Sie können diese Story mit einem vorhandenen Repository verknüpfen, indem Sie auf **+ Link hinzufügen** unter **Entwicklung** klicken. 

![6](./media/project-execution/project-execution-6-sprint-link-existing-branch.png)


##  5. <a name='AddTaskunderstory-5'></a>Hinzufügen einer Aufgabe zu einer Story 

Aufgaben sind bestimmte ausführliche Schritte, die zum Abschließen einer Story erforderlich sind. Nachdem alle Aufgaben einer Story abgeschlossen sind, sollte auch die Story abgeschlossen werden. 

Um einer Story eine Aufgabe hinzuzufügen, klicken Sie auf das Symbol **+** neben dem Storyeintrag, wählen Sie **Aufgabe** aus, und geben Sie dann die Detailinformationen für diese Aufgabe im Popupfenster ein.

![7](./media/project-execution/project-execution-7-sprint-add-task.png)

Nachdem die Features, Storys und Aufgaben erstellt wurden, können Sie in den Ansichten **Backlog** und **Board** ihren Status nachverfolgen.

![8](./media/project-execution/project-execution-8-sprint-backlog-view.png)

![9](./media/project-execution/project-execution-9-link-to-a-new-branch.png)


##  6. <a name='Linkaworkitemwithagitbranch-6'></a>Verknüpfen eines Arbeitselements mit einer Git-Verzweigung 

VSTS bietet eine einfache Möglichkeit zum Verbinden eines Arbeitselements (Story oder Aufgabe) mit einer Git-Verzweigung. Dies ermöglicht Ihnen, Ihre Story oder Aufgabe direkt mit dem zugeordneten Code zu verknüpfen. 

Um ein Arbeitselement mit einer neuen Verzweigung zu verbinden, doppelklicken Sie auf das Arbeitselement, und klicken Sie im Popupfenster auf **Neue Verzweigung erstellen** unter **+ Link hinzufügen**.  

![10](./media/project-execution/project-execution-10-sprint-board-view.png)

Geben Sie die Informationen für diese neue Verzweigung ein, z.B. den Namen der Verzweigung, das Git-Basisrepository und die Verzweigung. Das ausgewählte Git-Repository muss das Repository für dasselbe Teamprojekt sein, dem das Arbeitselement angehört. Die Basisverzweigung kann die Hauptverzweigung oder eine andere vorhandene Verzweigung sein.

![11](./media/project-execution/project-execution-11-create-a-branch.png)

Eine bewährte Methode besteht darin, für jedes Story-Arbeitselement eine Git-Verzweigung zu erstellen. Anschließend erstellen Sie für jedes Arbeitselement eine Verzweigung anhand der Storyverzweigung. Die Verzweigungen auf diese hierarchische Weise zu organisieren, die den Beziehungen zwischen Story und Aufgabe entspricht, ist nützlich, wenn mehrere Personen an unterschiedlichen Storys im selben Projekt oder an unterschiedlichen Aufgaben in derselben Story arbeiten. Konflikte können minimiert werden, wenn jedes Teammitglied an einer anderen Verzweigung arbeitet und bei gemeinsamer Arbeit an einer Verzweigung jedes Mitglied an unterschiedlichen Codes oder anderen Artefakten arbeitet. 

Die folgende Abbildung zeigt die empfohlene Verzweigungsstrategie für TDSP. Möglicherweise benötigen Sie nicht so viele Verzweigungen, wie hier dargestellt sind, insbesondere, wenn nur eine oder zwei Personen am selben Projekt arbeiten oder nur eine Person alle Aufgaben einer Story bearbeitet. Das Trennen der Entwicklungsverzweigung von der Hauptverzweigung ist jedoch immer ratsam. Dies verringert das Risiko, dass die Releaseverzweigung durch Entwicklungsaktivitäten unterbrochen wird. Eine ausführlichere Beschreibung des Git-Verzweigungsmodells finden Sie unter [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/) (Ein erfolgreiches Git-Verzweigungsmodell).

![12](./media/project-execution/project-execution-12-git-branches.png)

Um in die Verzweigung zu wechseln, die Sie bearbeiten möchten, führen Sie den folgenden Shellbefehl aus (Windows oder Linux). 

    git checkout <branch name>

Durch das Ändern von *<branch name\>* in **master** gelangen Sie zurück zur **Hauptverzweigung**. Nach dem Wechsel in die Arbeitsverzweigung können Sie an diesem Arbeitselement arbeiten und die Code- oder Dokumentationsartefakte entwickeln, die zum Fertigstellen des Elements erforderlich sind. 

Sie können auch ein Arbeitselement mit einer vorhandenen Verzweigung verknüpfen. Klicken Sie auf der Seite **Detail** eines Arbeitselements nicht auf **Neue Verzweigung erstellen**, sondern auf **+ Link hinzufügen**. Wählen Sie dann die Verzweigung aus, mit der Sie das Arbeitselement verknüpfen möchten. 

![13](./media/project-execution/project-execution-13-link-to-an-existing-branch.png)

Sie können auch mit Git-Bash-Befehlen eine neue Verzweigung erstellen. Wenn <bas branch name\> fehlt, basiert <new branch name\> auf der Verzweigung _master_. 
    
    git checkout -b <new branch name> <base branch name>


##  7. <a name='WorkonaBranchandCommittheChanges-7'></a>Arbeiten an einer Verzweigung und Committen der Änderungen 

Angenommen, Sie nehmen eine Änderung an der Verzweigung *data\_ingestion* für das Arbeitselement vor, beispielsweise, indem Sie eine R-Datei in dem Zweig auf Ihrem lokalen Computer hinzufügen. Sofern Sie sich in Ihrer Git-Shell in der Verzweigung für dieses Arbeitselement befinden, können Sie die R-Datei, die Sie dieser Verzweigung hinzugefügt haben, mithilfe der folgenden Git-Befehle committen:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![14](./media/project-execution/project-execution-14-sprint-push-to-branch.png)

##  8. <a name='CreateapullrequestonVSTS-8'></a>Erstellen einer Pull-Anforderung auf VSTS 

Wenn Sie nach einigen Commits und Push-Vorgängen bereit sind, die aktuelle Verzweigung mit ihrer Basisverzweigung zusammenzuführen, können Sie eine **Pullanforderung** an den VSTS-Server senden. 

Wechseln Sie zur Hauptseite Ihres Teamprojekts, und klicken Sie auf **CODE**. Wählen Sie die Verzweigung, die zusammengeführt werden soll, und den Namen des Git-Repositorys, mit dem die Verzweigung zusammengeführt werden soll, aus. Klicken Sie dann auf **Pullanforderungen**, klicken Sie auf **Neue Pullanforderung**, um eine Pullanforderungsüberprüfung zu erstellen, bevor die Arbeit an der Verzweigung mit der Basisverzweigung zusammengeführt wird.

![15](./media/project-execution/project-execution-15-spring-create-pull-request.png)

Geben Sie eine Beschreibung zu dieser Pullanforderung an, fügen Sie Prüfer hinzu, und senden Sie die Anforderung.

![16](./media/project-execution/project-execution-16-spring-send-pull-request.png)

##  9. <a name='ReviewandMerge-9'></a>Überprüfen und Zusammenführen 

Wenn die Pullanforderung erstellt wird, erhalten die Prüfer eine E-Mail-Benachrichtigung, um die Pullanforderungen überprüfen zu können. Die Prüfer müssen überprüfen, ob die Änderungen funktionieren, und die Änderungen mit dem Antragsteller testen, wenn möglich. Basierend auf ihrer Bewertung können die Prüfer die Pullanforderung genehmigen oder ablehnen. 

![17](./media/project-execution/project-execution-17-add_comments.png)

![18](./media/project-execution/project-execution-18-spring-approve-pullrequest.png)

Nach der Überprüfung wird die Verzweigung durch Klicken auf die Schaltfläche **Abschließen** mit der Basisverzweigung zusammengeführt. Sie können die Verzweigung löschen, nachdem sie zusammengeführt wurde. 

![19](./media/project-execution/project-execution-19-spring-complete-pullrequest.png)

Bestätigen Sie in der linken oberen Ecke, dass die Anforderung als **ABGESCHLOSSEN** markiert ist. 

![20](./media/project-execution/project-execution-20-spring-merge-pullrequest.png)

Wenn Sie zum Repository unter **CODE** zurückkehren, erfahren Sie, dass Sie zur Hauptverzweigung weitergeleitet wurden.

![21](./media/project-execution/project-execution-21-spring-branch-deleted.png)

Sie können auch mit den folgenden Git-Befehlen Ihre Arbeitsverzweigung mit ihrer Basisverzweigung zusammenführen und anschließend die Arbeitsverzweigung löschen:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![22](./media/project-execution/project-execution-22-spring-branch-deleted-commandline.png)


##  10. <a name='DataQualityReportUtility-10'></a>Hilfsprogramm für interaktive Datenuntersuchung, Analyse und Berichterstattung (Interactive Data Exploration, Analysis and Reporting, IDEAR)

Dieses auf R-Markdown basierende Hilfsprogramm stellt ein flexibles und interaktives Tool zum Auswerten und Untersuchen von DataSets dar. Benutzer können aus dem DataSet mit minimaler Codierung schnell Berichte generieren. Benutzer können auf Schaltflächen klicken, um die im interaktiven Tool angezeigten Untersuchungsergebnisse in einen abschließenden Bericht zu exportieren. Dieser kann an Kunden geliefert oder für Entscheidungen, welche Variablen in den nachfolgenden Modellierungsschritt eingeschlossen werden sollen, herangezogen werden.

Zurzeit funktioniert das Tool nur mit Datenrahmen im Arbeitsspeicher. Zum Angeben der zu durchsuchenden Parameter des DataSets ist eine YAML-Datei erforderlich. Weitere Informationen finden Sie unter [IDEAR in TDSP Data Science-Hilfsprogrammen](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


##  11. <a name='ModelingUtility-11'></a>Hilfsprogramm für Basismodellierung und Berichterstattung

Dieses Hilfsprogramm bietet ein anpassbares, halb automatisiertes Tool zur Modellerstellung mit Hyperparametersweeping zum Vergleichen der Genauigkeit dieser Modelle. 

Das Hilfsprogramm für die Modellerstellung ist eine R-Markdowndatei, die ausgeführt werden kann und dann eine eigenständige HTML-Ausgabe mit einem Inhaltsverzeichnis für einfache Navigation durch die verschiedenen Abschnitte erzeugt. Beim Ausführen der Markdowndatei werden drei Algorithmen ausgeführt: standardisierte Regression mit dem Paket glmnet, zufällige Gesamtstruktur mit dem Paket randomForest und Verstärkung von Strukturen mit dem Paket xgboost. Jeder dieser Algorithmen erzeugt ein trainiertes Modell. Anschließend wird die Genauigkeit dieser Modelle verglichen, und Plots für die relative Wichtigkeit dieser Features werden gemeldet. Derzeit gibt es zwei Hilfsprogramme: eines für eine Aufgabe der binären Klassifikation und eines für eine Regressionsaufgabe. Der wichtigste Unterschied zwischen ihnen ist die Art, wie Steuerparameter und Genauigkeitsmetriken für diese Lernaufgaben angegeben werden. 

Mit YAML-Dateien wird Folgendes angegeben:

- die Dateneingabe (eine SQL-Quelle oder eine R-Datendatei) 
- welcher Teil der Daten für Training und welcher Teil zu Testzwecken verwendet wird
- welche Algorithmen ausgeführt werden 
- die Auswahl der Steuerparameter für die Modelloptimierung:
    - Kreuzvalidierung 
    - Bootstrapping
    - Falten der Kreuzvalidierung
- Hyperparametersätze für jeden Algorithmus 

Die Anzahl der Algorithmen, die Anzahl der Falten für die Optimierung, die Hyperparameter und die Anzahl der Hyperparametersätze für Sweeping können auch in der YAML-Datei geändert werden, um die Modelle schnell ausführen zu können. Beispielsweise können sie mit einer geringeren Anzahl von CV-Falten und einer geringeren Anzahl von Parametersätzen ausgeführt werden. Sie können auch umfassender mit einer höheren Anzahl von CV-Falten oder einer höheren Anzahl von Parametersätzen ausgeführt werden, wenn dies notwendig ist.

Weitere Informationen finden Sie unter [Automatisiertes Modellierungs- und Berichterstellungs-Hilfsprogramm in TDSP Data Science-Hilfsprogrammen](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).


##  12. <a name='PowerBI-12'></a>Nachverfolgen des Fortschritts von Projekten mit Power BI-Dashboards

Data Science-Gruppenmanager, Teamleiter und Projektleiter müssen den Fortschritt ihrer Projekte nachverfolgen: welche Arbeit von welchen Personen ausgeführt wurde und was in den Aufgabenlisten verbleibt. Bei Verwendung von VSTS können Sie zum Nachverfolgen der Aktivitäten und der zugeordneten Arbeitselemente in einem Git-Repository Power BI-Dashboards erstellen. Weitere Informationen zum Verbinden von Power BI mit Visual Studio Team Services finden Sie unter [Verbinden von Power BI mit Team Services](https://www.visualstudio.com/en-us/docs/report/powerbi/connect-vso-pbi-vs). 

Informationen zum Erstellen von Power BI-Dashboards und -Berichten zum Nachverfolgen Ihrer Git-Repositoryaktivitäten und Ihrer Arbeitselemente nach dem Verbinden der VSTS-Daten mit Power BI finden Sie unter [Erstellen von Power BI-Dashboards und -Berichten](https://www.visualstudio.com/en-us/docs/report/powerbi/report-on-vso-with-power-bi-vs). 

Hier sind zwei einfache Beispieldashboards, die wir zum Nachverfolgen von Git-Aktivitäten und Arbeitsaufgaben erstellt haben. Im ersten Beispieldashboard werden die Git-Verpflichtungsaktivitäten nach Benutzern, Datumsangaben und Repositorys aufgeführt. Sie können sie problemlos aufteilen, um diejenigen zu filtern, an denen Sie interessiert sind.

![23](./media/project-execution/project-execution-23-powerbi-git.png)

Im zweiten Beispieldashboard werden die Arbeitselemente (Storys und Aufgaben) in anderen Iterationen präsentiert. Sie sind nach Beauftragten und Prioritätsstufen gruppiert und nach Status eingefärbt.

![24](./media/project-execution/project-execution-24-powerbi-workitem.png)

 
## <a name="next-steps"></a>Nächste Schritte

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Thema [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele zum Ausführen der Schritte im Team Data Science-Prozess mit Azure Machine Learning Studio finden Sie im Lernpfad [Mit Azure ML](http://aka.ms/datascienceprocess).
