---
title: Anmerkungen zu dieser Version von Azure ML Workbench (Sprint 3, Januar 2018)
description: "In diesem Dokument werden die Aktualisierungen für die Version „Sprint 3“ von Azure ML detailliert beschrieben."
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: f75fcec3b722563949b6553f17c4f3db3e223675
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="sprint-3---january-2018"></a>Sprint 3 – Januar 2018 

#### <a name="version-number-01171218263"></a>Versionsnummer: 0.1.1712.18263

>So können Sie die [Versionsnummer ermitteln](https://docs.microsoft.com/en-us/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Willkommen beim vierten Update von Azure Machine Learning Workbench. Nachfolgend werden die Aktualisierungen und Verbesserungen in diesem Sprint beschrieben. Viele dieser Aktualisierungen sind eine direkte Umsetzung des Benutzerfeedbacks. 

## <a name="notable-new-features-and-changes"></a>Wichtige neue Features und Änderungen
- Aktualisierung des Authentifizierungsstapels erzwingt Anmeldung und Kontoauswahl beim Start

## <a name="detailed-updates"></a>Detaillierte Aktualisierungen
Die nachstehende Liste zeigt die detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint.

### <a name="workbench"></a>Workbench
- Möglichkeit zum Installieren/Deinstallieren der App über „Programme hinzufügen/entfernen“
- Aktualisierung des Authentifizierungsstapels erzwingt Anmeldung und Kontoauswahl beim Start
- Verbesserte Oberfläche für einmaliges Anmelden (SSO) in Windows
- Benutzer, die mehreren Mandanten mit unterschiedlichen Anmeldeinformationen angehören, können sich jetzt bei Workbench anmelden

#### <a name="ui"></a>Benutzeroberfläche
- Allgemeine Verbesserungen und Fehlerbehebungen

### <a name="notebooks"></a>Notebooks
- Allgemeine Verbesserungen und Fehlerbehebungen

### <a name="data-preparation"></a>Vorbereitung von Daten 
- Verbesserte automatische Vorschläge beim Durchführen von Transformationen nach Beispiel
- Verbesserter Algorithmus für Musterhäufigkeitsprüfung
- Möglichkeit zum Senden von Beispieldaten und Feedback während der Durchführung von Transformationen nach Beispiel ![Screenshot mit dem Link zum Senden von Feedback bei der Transformation zur Spaltenableitung](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Verbesserungen an der Spark-Laufzeit
- Pyspark durch Scala ersetzt
- Korrektur eines Fehlers, durch den „Daten nicht anwendbar“ für den Time Series-Inspektor nicht geschlossen werden konnte 
- Korrektur eines Fehlers, bei dem bei der Datenvorbereitung für HDI keine Reaktion erfolgte

### <a name="model-management-cli-updates"></a>CLI-Aktualisierungen für Modellverwaltung 
  - Für das Bereitstellen von Ressourcen ist es nicht mehr erforderlich, der Besitzer des Abonnements zu sein. Um die Bereitstellungsumgebung einzurichten, reicht der Zugriff „Mitwirkender“ für die Ressourcengruppe aus.
  - Für kostenlose Abonnements wurde die Einrichtung der lokalen Umgebung aktiviert. 
