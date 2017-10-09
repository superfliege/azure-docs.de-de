---
title: Aufgaben einzelner Mitwirkender am Team Data Science-Prozess in Azure | Microsoft-Dokumentation
description: "Überblick über die Aufgaben eines einzelnen Mitwirkenden an einem Data Science-Teamprojekt."
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
ms.openlocfilehash: bbe691174409202a8fd9602a69e764f0a8e2816b
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="individual-contributor-tasks"></a>Aufgaben einzelner Mitwirkender

In diesem Thema werden die Aufgaben beschrieben, die ein einzelner Mitwirkender für sein Data Science-Team erfüllen muss. Ziel ist es, eine auf Zusammenarbeit ausgelegte Teamumgebung einzurichten, die gemäß dem [Team Data Science Process](overview.md) (TDSP) vereinheitlicht ist. Unter [Rollen und Aufgaben im Team Data Science-Prozess](roles-tasks.md) finden Sie eine Übersicht über die wichtigsten Mitarbeiterrollen und zugeordneten Aufgaben für ein Data Science-Team, das eine Vereinheitlichung gemäß diesem Prozess anstrebt.

Die Aufgaben der einzelnen Mitwirkenden am Projekt (Data Scientists) zum Einrichten der TDSP-Umgebung für das Projekt sind nachstehend dargestellt: 

![1](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **GroupUtilities** ist das Repository, das der gesamten Gruppe nützliche Hilfsprogramme bereitstellt. 
- **TeamUtilities** ist das Repository, das speziell für Ihr Team eingerichtet wird. 

Informationen zum Ausführen eines Data Science-Projekts gemäß dem TDSP finden Sie unter [Ausführung von Data Science-Projekten](project-execution.md). 

>[AZURE.NOTE] Wir beschreiben in den folgenden Anweisungen die erforderlichen Schritte zum Einrichten einer TDSP-Teamumgebung mithilfe von Visual Studio Team Services (VSTS). Wir geben an, wie diese Aufgaben mit VSTS erledigt werden können, da wir bei Microsoft den TDSP auch damit umsetzen. Wenn für Ihre Gruppe eine andere Plattform zum Hosten des Codes verwendet wird, ändern sich die vom Teamleiter auszuführenden Aufgaben in der Regel nicht. Sie müssen jedoch auf andere Weise ausgeführt werden.


## <a name="repositories-and-directories"></a>Repositorys und Verzeichnisse

In diesem Tutorial werden abgekürzte Namen für Repositorys und Verzeichnisse verwendet. Diese Namen erleichtern es, den Vorgängen zwischen den Repositorys und Verzeichnissen zu folgen. Diese Notation (**R** für Git-Repositorys und **D** für lokale Verzeichnisse auf Ihrer DSVM) wird in den folgenden Abschnitten verwendet:

- **R2**: Das Repository „GroupUtilities“ auf Git, das Ihr Gruppenleiter auf Ihrem VSTS-Gruppenserver eingerichtet hat.
- **R4**: Das Repository „TeamUtilities“ auf Git, das Ihr Teamleiter eingerichtet hat.
- **R5**: Das Repository „Project“ auf Git, das Ihr Projektleiter eingerichtet hat.
- **D2**: Das von R2 geklonte lokale Verzeichnis.
- **D4**: Das von R4 geklonte lokale Verzeichnis.
- **D5**: Das von R5 geklonte lokale Verzeichnis.


## <a name="step-0-prerequisites"></a>Voraussetzungen

Zum Erfüllen der Voraussetzungen müssen die dem Gruppenleiter zugewiesenen Aufgabe ausgeführt werden. Siehe dazu [Aufgaben des Gruppenleiters eines Data Science-Teams](group-manager-tasks.md). Kurz zusammengefasst müssen die folgenden Anforderungen erfüllt sein, ehe Sie mit den Aufgaben des Teamleiters beginnen: 
- Ihr Gruppenleiter hat das Repository **GroupUtilities** (sofern vorhanden) eingerichtet. 
- Ihr Teamleiter hat das Repository **TeamUtilities** (sofern vorhanden) eingerichtet.
- Ihr Projektleiter hat das Projektrepository eingerichtet. 
- Sie wurden vom Projektleiter Ihrem Projektrepository mit der Berechtigung hinzugefügt, das Projektrepository zu klonen und Daten per Push zurück in das Projektrepository zu übertragen.

Die zweite Voraussetzung, das Repository **TeamUtilities**, ist je nachdem optional, ob Ihr Team über ein teamspezifisches Repository für Hilfsprogramme verfügt. Wenn beliebige der anderen drei Voraussetzungen nicht erfüllt sind, wenden Sie sich an den Teamleiter, Projektleiter oder deren Stellvertreter, um die Umgebung gemäß den Anweisungen unter [Aufgaben für den Teamleiter eines Data Science-Teams](team-lead-tasks.md) oder unter [Aufgaben für den Projektleiter eines Data Science-Teams](project-lead-tasks.md) einzurichten.

- Git muss auf Ihrem Computer installiert sein. Wenn Sie eine Data Science Virtual Machine (DSVM) verwenden, ist Git bereits installiert, sodass Sie gleich loslegen können. Siehe andernfalls den Anhang [Plattformen und Tools](platforms-and-tools.md#appendix).  
- Bei Verwendung einer **Windows DSVM** muss [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) auf Ihrem Computer installiert sein. Scrollen Sie in der Datei „README.md“ nach unten zum Abschnitt **Download and Install**, und klicken Sie auf den *neuesten Installer*. Dadurch gelangen Sie zur Seite mit dem neuesten Installer. Laden Sie den Installer mit der Erweiterung EXE herunter, und führen Sie ihn aus. 
- Bei Verwendung einer **Linux DSVM** erstellen Sie auf Ihrer DSVM einen öffentlichen SSH-Schlüssel, den Sie dem VSTS-Server Ihrer Gruppe hinzufügen. Weitere Informationen zu SSH finden Sie im Anhang [Plattformen und Tools](platforms-and-tools.md#appendix) im Abschnitt **Erstellen eines öffentlichen SSH-Schlüssels**. 
- Wenn Ihr Team- und/oder Projektleiter einen Azure-Dateispeicher eingerichtet hat, der für Ihre DSVM bereitgestellt werden soll, müssen Sie den Leiter um die Informationen zum Azure-Dateispeicher bitten. 

## <a name="step-1-3-clone-group-team-and-project-repositories-to-local-machine"></a>Schritt 1 bis 3: Klonen von Gruppen-, Team- und Projektrepositorys auf den lokalen Computer

Dieser Abschnitt enthält Anweisungen zum Abschließen der ersten drei Aufgaben der einzelnen Mitwirkenden am Projekt: 

- Klonen des Repositorys **GroupUtilities** R2 nach D2
- Klonen des Repositorys **TeamUtilities** R4 nach D4 
- Klonen des Repositorys **Project** R5 nach D5

Erstellen Sie auf dem lokalen Computer das Verzeichnis ***C:\GitRepos*** (für Windows) oder ***$home/GitRepos*** (für Linux), und wechseln Sie dann zu diesem Verzeichnis. 

Führen Sie (entsprechend Ihrem Betriebssystem) einen der folgenden Befehle zum Klonen der Repositorys **GroupUtilities**, **TeamUtilities** und **Project** in Verzeichnisse auf Ihrem lokalen Computer aus: 

**Windows**
    
    git clone <the URL of the GroupUtilities repository>
    git clone <the URL of the TeamUtilities repository>
    git clone <the URL of the Project repository>
    
![2](./media/project-ic-tasks/project-ic-2-clone-three-repo-to-ic.png)

Vergewissern Sie sich, dass die drei Ordner in Ihrem Projektverzeichnis angezeigt werden.

![3](./media/project-ic-tasks/project-ic-3-three-repo-cloned-to-ic.png)

**Linux**
    
    git clone <the SSH URL of the GroupUtilities repository>
    git clone <the SSH URL of the TeamUtilities repository>
    git clone <the SSH URL of the Project repository>

![4](./media/project-ic-tasks/project-ic-4-clone-three-repo-to_ic-linux.png)

Vergewissern Sie sich, dass die drei Ordner in Ihrem Projektverzeichnis angezeigt werden.

![5](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="step-4-5-mount-azure-file-storage-to-your-dsvm-optional"></a>Schritte 4 und 5: Bereitstellen des Azure-Dateispeichers auf Ihrer DSVM (optional)

Zum Bereitstellen von Azure-Dateispeicher für Ihre DSVM lesen Sie die Anweisungen in Abschnitt 4 von [Aufgaben des Teamleiters eines Data Science-Teams](team-lead-tasks.md).

## <a name="next-steps"></a>Nächste Schritte

Es folgen Links zu den ausführlicheren Beschreibungen der Rollen und Aufgaben, die vom Team Data Science-Prozess definiert werden:

- [Aufgaben des Gruppenleiters eines Data Science-Teams](group-manager-tasks.md)
- [Aufgaben des Teamleiters eines Data Science-Teams](team-lead-tasks.md)
- [Aufgaben des Projektleiters eines Data Science-Teams](project-lead-tasks.md)
- [Einzelne Projektwirkende eines Data Science-Teams](project-ic-tasks.md)


