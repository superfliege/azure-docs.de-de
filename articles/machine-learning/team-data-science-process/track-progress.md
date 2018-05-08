---
title: Ausführung von Data Science-Projekten – Azure Machine Learning | Microsoft-Dokumentation
description: Erfahren Sie, wie ein Datenspezialist den Fortschritt eines Data Science-Projekts nachverfolgen kann.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: ae0a32e15d86e8c2a9f8359a8caf1d205bd019f3
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="track-progress-of-data-science-projects"></a>Nachverfolgen des Fortschritts von Data Science-Projekten

Data Science-Gruppenmanager, -Teamleiter und -Projektleiter müssen den Fortschritt ihrer Teamprojekte nachverfolgen: welche Arbeit von welchen Personen ausgeführt wurde und was in den Aufgabenlisten verbleibt. 

## <a name="vsts-dashboards"></a>VSTS-Dashboards
Bei Verwendung von Visual Studio Team Services (VSTS) können Sie zum Nachverfolgen der Aktivitäten und der einem bestimmten Agile-Projekt zugeordneten Arbeitselemente Dashboards erstellen. 

Weitere Informationen zum Erstellen und Anpassen von Dashboards und Widgets in Visual Studio Team Services finden Sie in den folgenden Anweisungen:

- [Hinzufügen und Verwalten von Dashboards](https://docs.microsoft.com/vsts/report/dashboards/dashboards)
- [Hinzufügen von Widgets zu einem Dashboard](https://docs.microsoft.com/vsts/report/dashboards/add-widget-to-dashboard)

## <a name="example-dashboard"></a>Beispieldashboard

Dies ist ein einfaches Beispieldashboard, das zum Nachverfolgen der Sprintaktivitäten in einem Agile Data Science-Projekt sowie der Anzahl von Commits in den zugeordneten Repositorys erstellt wird. Im Bereich **links oben** wird Folgendes angezeigt:

- Der Countdown des aktuellen Sprints 
- Die Anzahl von Commits für jedes Repository in den letzten sieben Tagen
- Das Arbeitselement für bestimmte Benutzer 

Die verbleibenden Elemente zeigen das kumulative Flussdiagramm (CFD), den Burndown und den Burnup für ein Projekt:

- **Links unten:** CFD der Menge an Arbeit in einem bestimmten Zustand mit genehmigten Elementen in Grau, committeten in Blau und erledigten in Grün
- **Rechts oben:** Burndowndiagramm der Arbeit, die noch abgeschlossen werden muss, gegenüber der verbleibenden Zeit
- **Rechts unten:** Burnupdiagramm der abgeschlossenen Arbeit gegenüber der Gesamtmenge der Arbeit

![Dashboard](./media/track-progress/dashboard.png)

Eine Beschreibung der Erstellung dieser Diagramme finden Sie in den Schnellstarts und Tutorials unter [Dashboards](https://docs.microsoft.com/vsts/report/dashboards/).
 
## <a name="next-steps"></a>Nächste Schritte

Exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Artikel [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 
