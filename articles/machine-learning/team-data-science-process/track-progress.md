---
title: Ausführung von Data Science-Projekten – Azure Machine Learning | Microsoft-Dokumentation
description: Erfahren Sie, wie ein Datenspezialist den Fortschritt eines Data Science-Projekts nachverfolgen kann.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: 32390b05d2ec258a68ed4f53135399675105a7e9
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302084"
---
# <a name="track-progress-of-data-science-projects"></a>Nachverfolgen des Fortschritts von Data Science-Projekten

Data Science-Gruppenmanager, Teamleiter und Projektleiter müssen den Fortschritt ihrer Projekte nachverfolgen: welche Arbeit von welchen Personen ausgeführt wurde und was in den Aufgabenlisten verbleibt. 

## <a name="azure-devops-dashboards"></a>Azure DevOps-Dashboards
Bei Verwendung von Azure DevOps können Sie zum Nachverfolgen der Aktivitäten und der einem bestimmten Agile-Projekt zugeordneten Arbeitselemente Dashboards erstellen. 

Weitere Informationen zum Erstellen und Anpassen von Dashboards und Widgets in Azure DevOps finden Sie in den folgenden Anweisungen:

- [Hinzufügen und Verwalten von Dashboards](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards)
- [Hinzufügen von Widgets zu einem Dashboard](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard)

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

Eine Beschreibung der Erstellung dieser Diagramme finden Sie in den Schnellstarts und Tutorials unter [Dashboards](https://docs.microsoft.com/azure/devops/report/dashboards/).
 
## <a name="next-steps"></a>Nächste Schritte

Exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Artikel [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 
